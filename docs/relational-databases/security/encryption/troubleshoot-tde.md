---
title: Azure Key Vault 中客户管理密钥的常见错误
description: 了解如何使用 Azure Key Vault 中的透明数据加密 (TDE) 和客户管理的密钥来识别和解决访问问题及常见错误。
ms.custom: seo-lt-2019
helpviewer_keywords:
- troublshooting, tde akv
- tde akv configuration, troubleshooting
- tde troubleshooting
author: jaszymas
ms.prod: sql
ms.technology: security
ms.reviewer: vanto
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: jaszymas
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: b1725b11a5cc491c4624a7196240546a649f9afa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102205"
---
# <a name="common-errors-for-transparent-data-encryption-with-customer-managed-keys-in-azure-key-vault"></a>使用 Azure Key Vault 中的客户托管密钥进行透明数据加密的常见错误

[!INCLUDE[asdb-asdbmi-asa](../../../includes/applies-to-version/asdb-asdbmi-asa.md)]

本文介绍了如何发现和解决 Azure Key Vault 密钥访问问题，这些问题导致配置为[结合使用透明数据加密 (TDE) 和 Azure Key Vault 中的客户托管密钥](/azure/sql-database/transparent-data-encryption-byok-azure-sql)的数据库变得无法访问。

## <a name="introduction"></a>介绍
如果 TDE 配置为使用 Azure Key Vault 中的客户托管密钥，必须持续访问此 TDE 保护程序，才能让数据库保持联机状态。  如果逻辑 SQL Server 在 Azure Key Vault 中失去对客户托管 TDE 保护程序的访问权限，数据库就会开始拒绝所有带有相应错误消息的连接，并将其在 Azure 门户中的状态更改为“无法访问”。

在最初的 8 小时内，如果基础 Azure Key Vault 密钥访问问题得到解决，数据库就会自动恢复并自动联机。 也就是说，对于所有间歇性和临时网络故障情况，无需执行任何用户操作，数据库就会自动联机。 在大多数情况下，需要执行用户操作来解决基础密钥保管库密钥访问问题。 

如果不再需要无法访问的数据库，可以立即删除它，以停止产生成本。 在恢复对 Azure Key Vault 密钥的访问权限，且数据库恢复联机之前，不允许对数据库执行其他任何操作。 在无法访问使用客户托管密钥加密的数据库期间，也不可能在服务器上将 TDE 选项从客户托管密钥更改为服务托管密钥。 这是必要的，可以在对 TDE 保护程序的权限遭撤销期间保护数据免遭未经授权的访问。 

如果数据库无法访问的时长超过 8 小时，便无法再自动恢复。 如果那期间之后所需的 Azure Key Vault 密钥访问已恢复，必须手动重新验证对密钥的访问，这样数据库才能恢复联机。 在这种情况下让数据库恢复联机可能需要相当长的时间（具体视数据库大小而定）。 在数据库恢复联机后，以前配置的设置（如[故障转移组](/azure/sql-database/sql-database-auto-failover-group)、PITR 历史记录和任何标记）则会丢失  。 因此，建议使用[操作组](/azure/azure-monitor/platform/action-groups)实现通知系统，以便尽快了解并解决基础密钥保管库密钥访问问题。 

## <a name="common-errors-causing-databases-to-become-inaccessible"></a>导致数据库无法访问的常见错误

使用 Key Vault 进行 TDE 时，发生的大多数问题都是由下列错误配置之一引起的：

### <a name="the-key-vault-is-unavailable-or-doesnt-exist"></a>密钥保管库不可用或不存在

- 密钥保管库遭意外删除。
- 为 Azure Key Vault 配置的防火墙禁止访问 Microsoft 服务。
- 间歇性网络错误导致密钥保管库不可用。

### <a name="no-permissions-to-access-the-key-vault-or-the-key-doesnt-exist"></a>无权访问密钥保管库或密钥不存在

- 密钥遭意外删除、禁用或密钥已到期。
- 逻辑 SQL Server 实例 AppId 遭意外删除。
- 逻辑 SQL Server 实例移到其他订阅中。 如果逻辑服务器移到其他订阅中，必须新建 AppId。
- 向 AppId 授予的密钥权限不足（即不包括“获取、包装和取消包装”权限）。
- 逻辑 SQL Server 实例 AppId 的权限遭撤销。

## <a name="identify-and-resolve-common-errors"></a>发现和解决常见错误

此部分列出了最常见错误的疑难解答步骤。

### <a name="missing-server-identity"></a>缺少服务器标识

**错误消息**

401 AzureKeyVaultNoServerIdentity - 在服务器上配置的服务器标识不正确。  请联系支持人员。

**检测**

使用下面的 cmdlet 或命令，以确保已向逻辑 SQL Server 实例分配标识：

- Azure PowerShell：[Get-AzureRMSqlServer](/powershell/module/AzureRM.Sql/Get-AzureRmSqlServer) 

- Azure CLI：[az-sql-server-show](/cli/azure/sql/server#az-sql-server-show)

**缓解**

使用下面的 cmdlet 或命令，以配置逻辑 SQL Server 实例的 Azure AD 标识 (AppId)：

- Azure PowerShell：[Set-AzureRmSqlServer](/powershell/module/azurerm.sql/set-azurermsqlserver)（含 `-AssignIdentity` 选项）。

- Azure CLI：[az sql server update](/cli/azure/sql/server#az-sql-server-update)（含 `--assign_identity` 选项）。

在 Azure 门户中，依次转到密钥保管库和“访问策略”  。 完成以下步骤： 

 1. 使用“新增”  按钮，为上一步中创建的服务器添加 AppId。 
 1. 分配以下密钥权限：获取、包装、解包 

若要了解详细信息，请参阅[将 Azure AD 标识分配给服务器](/azure/sql-database/transparent-data-encryption-byok-azure-sql-configure#assign-an-azure-ad-identity-to-your-server)。

> [!IMPORTANT]
> 如果在初始配置结合使用 TDE 和 Key Vault 后，逻辑 SQL Server 实例移到新租户中，请重复执行 Azure AD 标识配置步骤，以新建 AppId。 然后，将 AppId 添加到密钥保管库，并向它分配正确的密钥权限。 
>

### <a name="missing-key-vault"></a>缺少密钥保管库

**错误消息**

503 AzureKeyVaultConnectionFailed - 无法在服务器上完成操作，因为尝试连接到 Azure Key Vault 失败。 

**检测**

若要标识密钥 URI 和密钥保管库，请执行以下操作：

1. 使用下面的 cmdlet 或命令，以获取具体逻辑 SQL Server 实例的密钥 URI：

    - Azure PowerShell：[Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey)

    - Azure CLI：[az-sql-server-tde-key-show](/cli/azure/sql/server/tde-key#az-sql-server-tde-key-show) 

1. 使用密钥 URI 来标识密钥保管库：

    - Azure PowerShell：可检查 $MyServerKeyVaultKey 变量的属性，以获取密钥保管库的详细信息。

    - Azure CLI：检查返回的服务器加密保护程序，以获取密钥保管库的详细信息。

**缓解**

确认密钥保管库是否可用：

- 确保密钥保管库可用，且逻辑 SQL Server 实例拥有访问权限。
- 如果密钥保管库位于防火墙后，请务必选中允许 Microsoft 服务访问密钥保管库的复选框。
- 如果密钥保管库已遭意外删除，必须从头开始完成配置。


### <a name="missing-key"></a>缺少密钥

**错误消息**

404 ServerKeyNotFound - 在当前订阅上找不到请求的服务器密钥。  

409 ServerKeyDoesNotExists - 服务器密钥不存在。 

**检测**

若要标识密钥 URI 和密钥保管库，请执行以下操作：

- 使用[缺少密钥保管库](#missing-key-vault)中的 cmdlet 或命令，以标识添加到逻辑 SQL Server 实例的密钥 URI。 运行这些命令会返回密钥列表。

**缓解**

确认密钥保管库中是否有 TDE 保护程序：

1. 标识密钥保管库，然后在 Azure 门户中转到密钥保管库。
1. 确保密钥 URI 标识的密钥存在。

### <a name="missing-permissions"></a>缺少权限

**错误消息**

401 AzureKeyVaultMissingPermissions - 服务器缺少对 Azure Key Vault 的必需权限。 

**检测**

若要标识密钥 URI 和密钥保管库，请执行以下操作： 

- 使用[缺少密钥保管库](#missing-key-vault)中的 cmdlet 或命令，以标识逻辑 SQL Server 实例使用的密钥保管库。

**缓解**

确认逻辑 SQL Server 实例是否有权访问密钥保管库，以及是否拥有正确的密钥访问权限：

- 在 Azure 门户中，依次转到密钥保管库和“访问策略”  。 查找逻辑 SQL Server 实例 AppId。  
- 如果 AppId 存在，请确保 AppID 拥有以下密钥权限：获取、包装和解包。
- 如果 AppID 不存在，请使用“新增”  按钮进行添加。 

## <a name="getting-tde-status-from-the-activity-log"></a>从活动日志中获取 TDE 状态

为了监视由于 Azure Key Vault 密钥访问问题而导致的数据库状态，以下事件将根据 Azure 资源管理器 URL 和 Subscription+Resourcegroup+ServerName+DatabaseName 记录到[活动日志](/azure/service-health/alerts-activity-log-service-notifications)中，以获取资源 ID： 

**当服务失去对 Azure Key Vault 密钥的访问权限时发生的事件**

事件名称：MakeDatabaseInaccessible 

状态：Started 

说明:数据库已失去对 Azure Key Vault 密钥的访问权限，现在无法访问：<error message>   

 

**当自愈的 8 小时等待时间开始时发生的事件** 

事件名称：MakeDatabaseInaccessible 

状态：正在进行 

说明:数据库正在等待用户在 8 小时内重新建立 Azure Key Vault 密钥访问。   

 

**当数据库自动恢复联机时发生的事件**

事件名称：MakeDatabaseAccessible 

状态：已成功 

说明:已重新建立对 Azure Key Vault 密钥的数据库访问权限，数据库现在处于联机状态。 

 

**当在 8 小时内未解决问题且必须手动验证 Azure Key Vault 密钥访问时发生的事件** 

事件名称：MakeDatabaseInaccessible 

状态：已成功 

说明:数据库无法访问，要求用户修复 Azure Key Vault 错误，并使用重新验证密钥重新建立对 Azure Key Vault 密钥的访问权限。 

 

**当在手动重新验证密钥后数据库联机时发生的事件**

事件名称：MakeDatabaseAccessible 

状态：已成功 

说明:已重新建立对 Azure Key Vault 密钥的数据库访问权限，数据库现在处于联机状态。 

 

**当已成功重新验证 Azure Key Vault 密钥访问且数据库恢复联机时发生的事件**

事件名称：MakeDatabaseAccessible 

状态：Started 

说明:已开始恢复对 Azure Key Vault 密钥的数据库访问权限。 

 

**当重新验证 Azure Key Vault 密钥访问失败时发生的事件**

事件名称：MakeDatabaseAccessible 

状态：失败 

说明:无法恢复对 Azure Key Vault 密钥的数据库访问权限。 


## <a name="next-steps"></a>后续步骤

- 了解 [Azure 资源运行状况](/azure/service-health/resource-health-overview)。
- 将[操作组](/azure/azure-monitor/platform/action-groups)设置为，根据偏好设置（例如，电子邮件/短信/推送/语音、逻辑应用、Webhook、ITSM 或自动化 Runbook）接收通知和警报。