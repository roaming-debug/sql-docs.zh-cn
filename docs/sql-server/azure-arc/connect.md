---
title: 连接到 Azure Arc
titleSuffix: ''
description: 将 SQL Server 的实例连接到 Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: e80892bfef7ee2c8cf22aef1b491ab5ea0c0addd
ms.sourcegitcommit: 442fbe1655d629ecef273b02fae1beb2455a762e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235561"
---
# <a name="connect-your-sql-server-to-azure-arc"></a>将 SQL Server 连接到 Azure Arc

可以通过执行以下步骤，将本地 SQL Server 实例连接到 Azure Arc。

## <a name="prerequisites"></a>必备知识

* 你的计算机上至少安装了一个 SQL Server 实例
* 对于 Windows 计算机，已安装 Azure PowerShell。 按照这些说明[安装 Azure PowerShell](/powershell/azure/install-az-ps)。
* 对于 Linux 计算机，已下载 Azure CLI 并连接到 Azure 帐户。 按照说明[安装 Azure CLI](/cli/azure/install-azure-cli-apt)。
* 已注册 Microsoft.AzureData 资源提供程序。 有关资源提供程序的详细信息，请参阅 Azure 资源提供程序和类型。
    * 在 PowerShell 中运行 `Register-AzResourceProvider -ProviderNamespace Microsoft.AzureData`
    * 在 Linux 中运行 `az provider register --namespace 'Microsoft.AzureData`



## <a name="generate-a-registration-script-for-sql-server"></a>为 SQL Server 生成注册脚本

在此步骤中，你将生成一个脚本，用于发现计算机上安装的所有 SQL Server 实例，并将其注册为“SQL Server - Azure Arc”资源。 如果宿主物理计算机或虚拟机未注册到 Azure Arc，则脚本会自动注册它。

1. 搜索“SQL Server - Azure Arc”资源类型，并通过“创建”边栏选项卡添加新资源。

![开始创建](media/join/start-creation-of-sql-server-azure-arc-resource.png)
    
2. 查看先决条件，并转到“服务器详细信息”选项卡。  

3. 选择订阅、资源组、Azure 区域和主机操作系统。 如果需要，还要指定你的网络用于连接到 Internet 的代理。

> [!IMPORTANT]
> 如果承载 SQL Server 实例的计算机已[连接到 Azure Arc](/azure/azure-arc/servers/onboard-portal)，请确保选择包含相应“计算机 - Azure Arc”资源的同一资源组。

![服务器详细信息](media/join/server-details-sql-server-azure-arc.png)

4. 转到“运行脚本”选项卡，然后下载显示的注册脚本。 门户会为你指定的宿主 OS 生成脚本。

![下载脚本](media/join/download-script-sql-server-azure-arc.png)

## <a name="connect-the-installed-sql-server-instances-to-azure-arc"></a>将已安装的 SQL Server 实例连接到 Azure Arc

在此步骤中，你将使用从 Azure 门户下载的脚本，在目标物理计算机或虚拟机上执行该脚本。 因此，计算机上安装的每个 SQL Server 实例都将注册为“SQL Server - Azure Arc”资源。 此外，如果计算机本身未安装来宾配置代理，将自动安装该代理，并将其注册为“计算机 - Azure Arc”资源。

> [!NOTE]
> 请确保用于执行该脚本的帐户满足[先决条件](overview.md#prerequisites)中所述的最低权限要求。

### <a name="windows"></a>Windows

1. 启动 powershell.exe 的管理员实例，并使用你的 Azure 凭据登录 PowerShell 模块。 按照[登录说明](/powershell/azure/install-az-ps#sign-in)进行操作。

2. 执行已下载的脚本

   ```powershell
   & '.\RegisterSqlServerArc.ps1'
   ```

   > [!NOTE]
   > 如果尚未安装适用于 powershell 的 AZ 模块，则第一次运行时可能会出现问题。 在这种情况下，请遵循脚本中的说明来安装和连接帐户，并再次运行该脚本。

### <a name="linux"></a>Linux

1. 使用 Azure CLI 通过 Azure 凭据登录。 按照[登录说明](/cli/azure/authenticate-azure-cli)进行操作

2. 对已下载的脚本授予执行权限并执行该脚本。

   ```bash
   sudo chmod +x ./RegisterSqlServerArc.sh
   ./RegisterSqlServerArc.sh
   ```

## <a name="register-sql-server-instances-on-multiple-machines"></a>在多台计算机上注册 SQL Server 实例

你可以使用为单台计算机生成的相同脚本，将多台 Windows 或 Linux 计算机上安装的多个 SQL Server 实例连接到 Azure Arc。 按照有关[将 SQL Server 实例大规模连接到 Azure Arc](connect-at-scale.md) 的说明进行操作。

## <a name="validate-the-sql-server---azure-arc-resources"></a>验证“SQL Server - Azure Arc”资源

转到 [Azure 门户](https://ms.portal.azure.com/#home)并打开新注册的“SQL Server - Azure Arc”资源以进行验证。

![验证已连接的 SQL server ](media/join/validate-sql-server-azure-arc.png)

## <a name="un-register-the-sql-server---azure-arc-resources"></a>取消注册“SQL Server - Azure Arc”资源

若要删除现有“SQL Server - Azure Arc”资源，请转到包含该资源的资源组，并将其从组中资源的列表中删除。

![取消注册 SQL Server](media/join/delete-sql-server-azure-arc.png)

## <a name="next-steps"></a>后续步骤

* [为 SQL Server 实例配置高级数据安全](configure-advanced-data-security.md)

* [为 SQL Server 实例配置按需 SQL 评估](assess.md)