---
title: 将 SQL Server 实例大规模连接到 Azure Arc
titleSuffix: ''
description: 本文介绍如何使用服务主体将 SQL Server 实例连接为启用了 Azure Arc 的 SQL Server（预览版）。
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: b78c76504cb9ffb76b62c1867630760fe52cf22c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063182"
---
# <a name="connect-sql-server-instances-to-azure-arc-at-scale"></a>将 SQL Server 实例大规模连接到 Azure Arc

你可以使用[为单台计算机生成的相同脚本](connect.md)，将多台 Windows 或 Linux 计算机上安装的多个 SQL Server 实例连接到 Azure Arc。 该脚本会将每台计算机及其上安装的 SQL Server 实例连接并注册到 Azure Arc。为获得最佳体验，建议使用 Azure Active Directory [服务主体](/azure/active-directory/develop/app-objects-and-service-principals)。 服务主体是一种特殊的受限管理标识，它仅被授予了最低权限，可将计算机连接到 Azure，以及为启用了 Azure Arc 的服务器和启用了 Azure Arc 的 SQL Server 创建 Azure 资源。 这比使用较高特权的帐户（例如租户管理员）更安全，并且可以遵循我们的访问控制安全性最佳做法。  

安装和配置 Connected Machine 代理的安装方法要求你在计算机上拥有管理员权限。 在 Linux 上，需使用 root 帐户；在 Windows 上，需要以“本地管理员组”的成员身份使用这些方法。

在开始之前，请务必查看[先决条件](overview.md#prerequisites)并确保已创建具有所需权限的自定义角色。

## <a name="connecting-multiple-sql-server-instances-on-windows-using-azure-powershell"></a>使用 Azure PowerShell 在 Windows 上连接多个 SQL Server 实例

必须在每台计算机上安装 [Azure PowerShell](/powershell/azure/install-az-ps)。

1. 使用 PowerShell [`New-AzADServicePrincipal`](/powershell/module/az.resources/new-azadserviceprincipal) cmdlet 创建服务主体。 请确保将输出存储在变量中。 否则，稍后将无法检索所需的密码。

    ```azurepowershell-interactive
    $sp = New-AzADServicePrincipal -DisplayName "Arc-for-servers" -Role <your custom role>
    $sp
    ```

    ```output
    Secret                : System.Security.SecureString
    ServicePrincipalNames : {ad9bcd79-be9c-45ab-abd8-80ca1654a7d1, https://Arc-for-servers}
    ApplicationId         : ad9bcd79-be9c-45ab-abd8-80ca1654a7d1
    ObjectType            : ServicePrincipal
    DisplayName           : Hybrid-RP
    Id                    : 5be92c87-01c4-42f5-bade-c1c10af87758
    Type                  :
    ```

   > [!NOTE]
   > 创建服务主体时，你的帐户必须是要用于加入的订阅中的“所有者”或“用户访问管理员”。 如果你没有足够的权限创建角色分配，则可能会创建服务主体，但它将无法加入计算机。 [所需的权限](overview.md#required-permissions)中提供了有关如何创建自定义角色的说明。

2. 检索存储在 `$sp` 变量中的密码：

   ```azurepowershell-interactive
   $credential = New-Object pscredential -ArgumentList "temp", $sp.Secret
   $credential.GetNetworkCredential().password
   ```
3. 检索服务主体租户 ID 的值：
 
   ```azurepowershell-interactive
   $tenantId= (Get-AzContext).Tenant.Id
   ```
4. 使用适当的安全做法，复制并保存密码、应用程序 ID 和租户 ID 值。 如果忘记或丢失了服务主体密码，可以使用 [`New-AzADSpCredential`](/powershell/module/azurerm.resources/new-azurermadspcredential) cmdlet 重置它。

   > [!NOTE]
   > 请注意，Azure Arc for servers 目前不支持使用证书进行登录，因此服务主体必须具有用于进行身份验证的机密。

5. 按照[将 SQL Server 连接到 Azure Arc](connect.md) 中的说明，从门户下载 PowerShell 脚本。

6. 在 PowerShell ISE 的管理员实例中打开该脚本，并使用前述服务主体预配过程中生成的值替换以下环境变量。 这些变量最初为空。

   ```azurepowershell-interactive
   $servicePrincipalAppId="{serviceprincipalAppID}"
   $servicePrincipalSecret="{serviceprincipalPassword}"
   $servicePrincipalTenantId="{serviceprincipalTenantId}"
   ```

7. 在每台目标计算机上执行脚本

## <a name="connecting-multiple-sql-server-instances-on-linux-using-azure-cli"></a>使用 Azure CLI 在 Linux 上连接多个 SQL Server 实例

必须在每台目标计算机上[安装 Azure CLI](/cli/azure/install-azure-cli)。 如果提供了服务主体凭据，并且尚无其他用户登录，注册脚本将通过这些凭据自动登录到 Azure。 使用以下步骤在多台 Linux 计算机上连接 SQL Server 实例。

1. 使用[“az ad sp create-for-rbac”](/cli/azure/ad/sp#az_ad_sp_create_for_rbac)命令创建服务主体。

   ```azurecli-interactive
   az ad sp create-for-rbac --name <your service principal name> --role <your custom role name>
   ```

   ```output
   { "appId": "d2ff754a-e10a-4eb6-9cdc-ce6e7a4687db",
     "displayName": "Arc-for-servers",
     "name": "http://Arc-for-servers",
     "password": {SomeRandomlyGeneratedPassword}",
     "tenant": "2530e75f-673b-4841-8270-47ca6a34ef4f"
   }
   ```

   > [!NOTE]
   > 创建服务主体时，你的帐户必须是要用于加入的订阅中的“所有者”或“用户访问管理员”。 如果你没有足够的权限创建角色分配，则可能会创建服务主体，但它将无法加入计算机。 [所需的权限](overview.md#required-permissions)中提供了有关如何创建自定义角色的说明。

2. 按照[将 SQL Server 连接到 Azure Arc](connect.md) 中的说明，从门户下载 Linux shell 脚本。

3. 使用“az ad sp create-rbac”命令返回的值，替换脚本中的以下变量。 这些变量最初为空。

   ```bash
   servicePrincipalAppId="{serviceprincipalAppID}"
   servicePrincipalSecret="{serviceprincipalPassword}"
   servicePrincipalTenant="{serviceprincipalTenant}"
   ```

3. 在每台目标计算机上执行脚本
 
   ```bash
   sudo chmod +x ./RegisterSqlServerArc.sh
   ./RegisterSqlServerArc.sh
   ```

## <a name="validate-successful-onboarding"></a>验证成功加入

将 SQL Server 实例注册到启用了 Azure Arc 的 SQL Server（预览版）后，请转到 [Azure 门户](https://aka.ms/azureportal)，并查看新创建的 Azure Arc 资源。 你将看到每台已连接的计算机都对应一个新的“计算机 - Azure Arc”，每个已注册的 SQL Server 实例都对应一项新的“SQL Server - Azure Arc”资源。  

![成功加入](./media/join-at-scale/successful-onboard.png)

## <a name="next-steps"></a>后续步骤

- 了解如何使用 [Azure Policy](/azure/governance/policy/overview) 管理计算机，例如，进行 VM [来宾配置](/azure/governance/policy/concepts/guest-configuration)，验证计算机是否向预期的 Log Analytics 工作区报告，使用[用于 VM 的 Azure Monitor](/azure/azure-monitor/insights/vminsights-enable-policy) 启用监视等。

- 详细了解 [Log Analytics 代理](/azure/azure-monitor/platform/log-analytics-agent)。 若要主动监视计算机上运行的 OS 和工作负荷、使用自动化 Runbook 或更新管理等解决方案对其进行管理，或使用其他 Azure 服务（例如 [Azure 安全中心](/azure/security-center/security-center-intro)），需要安装适用于 Windows 和 Linux 的 Log Analytics 代理。

- 了解如何[使用按需 SQL 评估配置 SQL Server 实例以定期检查环境运行状况](assess.md)

- 了解如何[为 SQL Server 实例配置高级数据安全](configure-advanced-data-security.md)