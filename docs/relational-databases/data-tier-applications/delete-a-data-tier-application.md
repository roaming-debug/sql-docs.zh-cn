---
description: 删除数据层应用程序
title: 删除数据层应用程序 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.deletedacwizard.deletedac.f1
- sql13.swb.deletedacwizard.summary.f1
- sql13.swb.deletedacwizard.introduction.f1
- sql13.swb.deletedacwizard.choosemethod.f1
helpviewer_keywords:
- How to [DAC], delete
- data-tier application [SQL Server], delete
- wizard [DAC], delete
- delete DAC
ms.assetid: 16fe1c18-4486-424d-81d6-d276ed97482f
author: stevestein
ms.author: sstein
ms.openlocfilehash: 2bde888ff6091ae8d05445acce1afb1469843c18
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128825"
---
# <a name="delete-a-data-tier-application"></a>删除数据层应用程序
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  您可以通过使用“删除数据层应用程序向导”或 Windows PowerShell 脚本，删除数据层应用程序。 您可以指定是保留、分离还是删除关联数据库。  
  
-   **开始之前：** [限制和局限](#LimitationsRestrictions)、[权限](#Permissions)  
  
-   若要升级 DAC，请使用：[注册数据层应用程序向导](#UsingDeleteDACWizard)、[PowerShell](#DeleteDACPowerShell)  
  
## <a name="before-you-begin"></a>开始之前  
 在删除某一数据层应用程序 (DAC) 实例时，您可以选择三个选项之一，这三个选项指定要对与该数据层应用程序相关联的数据库执行何种操作。 所有这三个选项都删除 DAC 定义元数据。 这些选项在如何处理与数据层应用程序相关联的数据库上有所不同。 向导并不删除与 DAC 或数据库相关联的任何实例级别的对象，例如登录名。  
  
|选项|数据库操作|  
|------------|----------------------|  
|删除注册|关联的数据库保持不变。|  
|分离数据库|关联的数据库被分离。 数据库引擎的实例无法引用该数据库，但数据和日志文件保持不变。|  
|删除数据库|关联的数据库被删除。 数据和日志文件被删除。|  
  
###  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> 限制和局限  
 在删除某一 DAC 后，没有自动的机制可以还原该 DAC 的定义元数据或数据库。 您可以手动重新生成 DAC 实例的方式取决于删除选项。  
  
|选项|如何重新生成 DAC 实例|  
|------------|-------------------------------------|  
|删除注册|从原来的数据库中注册一个 DAC。|  
|分离数据库|通过使用 **sp_attachdb** 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]重新附加数据库，然后从该数据库注册一个新的 DAC 实例。|  
|删除数据库|从在删除 DAC 前生成的完整备份还原数据库，然后从该数据库注册一个新的 DAC 实例。|  
  
> [!WARNING]  
>  通过从还原或重新连接的数据库注册 DAC 重新生成一个 DAC 实例时，将不会重新创建该原始 DAC 的某些部分，例如服务器选择策略。  
  
###  <a name="permissions"></a><a name="Permissions"></a> 权限  
 只能由 **sysadmin** 或 **serveradmin** 固定服务器角色的成员删除 DAC，或者由数据库所有者删除。 名为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sa **的内置** 系统管理员帐户也可以启动该向导。  
  
##  <a name="using-the-delete-data-tier-application-wizard"></a><a name="UsingDeleteDACWizard"></a> 使用“删除数据层应用程序向导”  
 **使用向导删除 DAC**  
  
1.  在 **“对象资源管理器”** 中，展开包含要删除的 DAC 的实例的节点。  
  
2.  展开 **“管理”** 节点。  
  
3.  展开 **数据层应用程序** 节点。  
  
4.  右键单击要删除的 DAC，然后选择“删除数据层应用程序…”  
  
5.  完成向导对话框：  
  
    1.  [介绍](#Introduction)  
  
    2.  [选择方法](#Choose_method)  
  
    3.  [摘要](#Summary)  
  
    4.  [删除数据层应用程序](#Delete_datatier_application)  
  
##  <a name="introduction-page"></a><a name="Introduction"></a> “简介”页  
 此页描述用于删除数据层应用程序的各个步骤。  
  
 **不再显示此页。** - 选中该复选框可以停止在将来显示此页。  
  
 “下一步 >” - 进入“选择方法”页。  
  
 **取消** - 结束向导且不删除数据层应用程序或数据库。  
  
 [使用“删除数据层应用程序向导”](#UsingDeleteDACWizard)  
  
##  <a name="choose-method-page"></a><a name="Choose_method"></a> “选择方法”页  
 使用此页可以指定用于处理与要删除的 DAC 关联的数据库的选项。  
  
 **删除注册** - 删除用于定义数据层应用程序的元数据，但保持关联的数据库不变。  
  
 **分离数据库** - 删除用于定义数据层应用程序的元数据并且分离关联的数据库。  
  
 数据库不再被 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的该实例引用，但数据和日志文件保持不变。  
  
 **删除数据库** - 删除用于定义 DAC 的元数据并且删除关联的数据库。  
  
 数据库的数据和日志文件被永久删除。  
  
 **< 上一步** - 返回到“简介”页。  
  
 “下一步 >”- 继续到“摘要”页。  
  
 **取消** - 结束向导且不删除 DAC 或数据库。  
  
 [使用“删除数据层应用程序向导”](#UsingDeleteDACWizard)  
  
##  <a name="summary-page"></a><a name="Summary"></a> 摘要页  
 使用此页可以查看在删除 DAC 实例时向导将执行的操作。  
  
 **查看选择摘要** - 查看在该框中显示的 DAC、数据库和删除方法。 如果信息正确，则选择 **“下一步”** 或者 **“完成”** 以便删除 DAC。 如果 DAC 和数据库信息不正确，则选择 **“取消”** 并且选择正确的 DAC。 如果删除方法不正确，则选择 **“上一步”** 返回到 **“选择方法”** 页并且选择其他方法。  
  
 **< 上一步** - 返回到“选择方法”页以便选择其他删除方法。  
  
 **下一步 >** - 使用你在上一页中选择的方法删除 DAC 实例，并且继续到“删除数据层应用程序”页。  
  
 **取消** - 结束向导且不删除 DAC 实例。  
  
 [使用“删除数据层应用程序向导”](#UsingDeleteDACWizard)  
  
##  <a name="delete-data-tier-application-page"></a><a name="Delete_datatier_application"></a> “删除数据层应用程序”页  
 此页报告删除操作是成功还是失败。  
  
 **删除 DAC** - 报告为删除 DAC 实例而执行的每个操作是成功还是失败。 查看信息以便确定每个操作是成功还是失败。 遇到了错误的任何操作都将在 **“结果”** 列中具有一个链接。 选择该链接可以查看针对该操作的错误报告。  
  
 **保存报表** - 选择此按钮可以将删除报表保存到某一 HTML 文件。 该文件报告每个操作的状态，并且包括任何操作生成的所有错误。 默认文件夹是您的 Windows 帐户的 Documents 文件夹中的 SQL Server Management Studio\DAC Packages 文件夹。  
  
 **完成** - 结束向导。  
  
 [使用“删除数据层应用程序向导”](#UsingDeleteDACWizard)  
  
##  <a name="using-powershell"></a><a name="DeleteDACPowerShell"></a> 使用 PowerShell  

1. 创建一个 SMO Server 对象，并且将该对象设置为包含要删除的 DAC 的实例。  
  
1. 打开 **ServerConnection** 对象，并连接到同一实例。  
  
1. 使用 `add_DacActionStarted` 和 `add_DacActionFinished` 订阅 DAC 升级事件。  
  
1. 指定要删除的 DAC。  
  
1. 根据适合的删除选项，使用以下三个示例之一：  
  
   - 若要删除 DAC 注册并保持数据库不变，请使用 `Unmanage` 方法。  
   - 若要删除 DAC 注册并分离数据库，请使用 `Uninstall` 方法并指定 `DetachDatabase`。  
   - 若要删除 DAC 注册并删除数据库，请使用 `Uninstall` 方法并指定 `DropDatabase`。
  
### <a name="delete-the-dac-and-leave-the-database"></a>删除 DAC 并保留数据库

下面的示例通过使用`Unmanage` 方法删除名为 `<myApplication>` 的 DAC，实现删除 DAC 但保持数据库不变。  
  
```powershell
## Set a SMO Server object to the default instance on the local computer.  
CD SQLSERVER:\SQL\localhost\DEFAULT  
$server = Get-Item .  
  
## Open a Common.ServerConnection to the same instance.  
$serverConnection = New-Object Microsoft.SqlServer.Management.Common.ServerConnection($server.ConnectionContext.SqlConnectionObject)  
$serverConnection.Connect()  
$dacStore = New-Object Microsoft.SqlServer.Management.Dac.DacStore($serverConnection)  
  
## Subscribe to the DAC delete events.  
$dacStore.add_DacActionStarted({Write-Host `n`nStarting at $(Get-Date) :: $_.Description})  
$dacStore.add_DacActionFinished({Write-Host Completed at $(Get-Date) :: $_.Description})  
  
## Specify the DAC to delete.  
$dacName  = "<myApplication>"  
  
## Only delete the DAC definition from msdb, the associated database remains active.  
$dacStore.Unmanage($dacName)  
```
  
### <a name="delete-the-dac-and-detach-the-database"></a>删除 DAC 并分离数据库

下面的示例通过使用 `Uninstall` 方法删除名为 `<myApplication>` 的 DAC，实现删除 DAC 且分离数据库。  
  
```powershell
## Set a SMO Server object to the default instance on the local computer.  
CD SQLSERVER:\SQL\localhost\DEFAULT  
$server = Get-Item .  
  
## Open a Common.ServerConnection to the same instance.  
$serverConnection = New-Object Microsoft.SqlServer.Management.Common.ServerConnection($server.ConnectionContext.SqlConnectionObject)  
$serverConnection.Connect()  
$dacStore = New-Object Microsoft.SqlServer.Management.Dac.DacStore($serverConnection)  
  
## Subscribe to the DAC delete events.  
$dacStore.add_DacActionStarted({Write-Host `n`nStarting at $(Get-Date) :: $_.Description})  
$dacStore.add_DacActionFinished({Write-Host Completed at $(Get-Date) :: $_.Description})  
  
## Specify the DAC to delete.  
$dacName  = "<myApplication>"  
  
## Delete the DAC definition from msdb and detach the associated database.  
$dacStore.Uninstall($dacName, [Microsoft.SqlServer.Management.Dac.DacUninstallMode]::DetachDatabase)  
```
  
### <a name="delete-the-dac-and-drop-the-database"></a>删除 DAC 并丢弃数据库

下面的示例通过使用 `Uninstall` 方法删除名为 `<myApplication>` 的 DAC，实现删除 DAC 并丢弃数据库。  
  
```powershell
## Set a SMO Server object to the default instance on the local computer.  
CD SQLSERVER:\SQL\localhost\DEFAULT  
$server = Get-Item .  
  
## Open a Common.ServerConnection to the same instance.  
$serverConnection = New-Object Microsoft.SqlServer.Management.Common.ServerConnection($server.ConnectionContext.SqlConnectionObject)  
$serverConnection.Connect()  
$dacStore = New-Object Microsoft.SqlServer.Management.Dac.DacStore($serverConnection)  
  
## Subscribe to the DAC delete events.  
$dacStore.add_DacActionStarted({Write-Host `n`nStarting at $(Get-Date) :: $_.Description})  
$dacStore.add_DacActionFinished({Write-Host Completed at $(Get-Date) :: $_.Description})  
  
## Specify the DAC to delete.  
$dacName  = "<myApplication>"  
  
## Delete the DAC definition from msdb and drop the associated database.  
$dacStore.Uninstall($dacName, [Microsoft.SqlServer.Management.Dac.DacUninstallMode]::DropDatabase)  
```
  
## <a name="see-also"></a>另请参阅  
 [数据层应用程序](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [数据层应用程序](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [部署数据层应用程序](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md)   
 [将数据库注册为 DAC](../../relational-databases/data-tier-applications/register-a-database-as-a-dac.md)   
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [数据库分离和附加 (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)  
  
  
