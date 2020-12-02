---
description: 升级数据层应用程序
title: 升级数据层应用程序 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.upgradedacwizard.summary.f1
- sql13.swb.upgradedacwizard.reviewplan.f1
- sql13.swb.upgradedacwizard.upgradedac.f1
- sql13.swb.upgradedacwizard.selectpackage.f1
- sql13.swb.upgradedacwizard.reviewpolicy.f1
- sql13.swb.upgradedacwizard.selectoptions.f1
- sql13.swb.upgradedacwizard.checkdrift.f1
- sql13.swb.upgradedacwizard.introduction.f1
helpviewer_keywords:
- upgrade DAC
- data-tier application [SQL Server], upgrade
- wizard [DAC], upgrade
- How to [DAC], upgrade
ms.assetid: c117df94-f02b-403f-9383-ec5b3ac3763c
author: stevestein
ms.author: sstein
ms.openlocfilehash: be606f7aa8f42b9966f6b1a2d1e3e21095b03916
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92196213"
---
# <a name="upgrade-a-data-tier-application"></a>升级数据层应用程序
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  使用“升级数据层应用程序向导”或 Windows PowerShell 脚本可以更改当前部署的数据层应用程序 (DAC) 的架构和属性，以便匹配在 DAC 的新版本中定义的架构和属性。  
  
-   **Before you begin:**  [Choosing DAC Upgrade Options](#ChoseDACUpgOptions), [Limitations and Restrictions](#LimitationsRestrictions), [Prerequisites](#Prerequisites), [Security](#Security), [Permissions](#Permissions)  
  
-   **若要升级 DAC，请使用：**  [升级数据层应用程序向导](#UsingDACUpgradeWizard)、[PowerShell](#UpgradeDACPowerShell)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
 DAC 升级是一个就地过程，此过程更改现有数据库的架构以匹配在新版本的 DAC 中定义的架构。 这一新版本的 DAC 在 DAC 包文件中提供。 有关创建 DAC 包的详细信息，请参阅 [数据层应用程序](../../relational-databases/data-tier-applications/data-tier-applications.md)。  
  
###  <a name="choosing-dac-upgrade-options"></a><a name="ChoseDACUpgOptions"></a> 选择 DAC 升级选项  
 就地升级有四种升级选项：  
  
-   **忽略数据丢失** - 如果为 True，则即使某些操作导致数据丢失，升级也将继续。 如果为 **False**，则上述操作将终止升级。 例如，如果当前数据库中的表在新的 DAC 的架构中不存在，则在指定 **True** 时该表将被删除。 默认设置为 **True**。  
  
-   **更改时阻止** - 如果为 True，则在数据库架构不同于在之前的 DAC 中定义的架构时，升级将终止。 如果为 **False**，则即使检测到更改，升级也将继续。 默认设置为 **False**。  
  
-   **失败时回滚** - 如果为 True，则升级将封装在事务中，并且在遇到错误时，将尝试回滚。 如果为 **False**，则在进行更改时所有更改都将提交，并且在出错时，您可能要还原数据库的以前的备份。 默认设置为 **False**。  
  
-   **跳过策略验证** - 如果为 True，将不评估 DAC 服务器选择策略。 如果为 **False**，将评估策略，并且在存在验证错误时升级将终止。 默认设置为 **False**。  
  
###  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> 限制和局限  
 DAC 升级只能在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]或者 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 4 (SP4) 或更高版本中执行。  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 出于谨慎起见，在开始升级前应生成完整数据库备份。 如果升级遇到了错误并且无法回滚其所有更改，可能需要还原该备份。  
  
 在开始升级前，您应该采取若干操作以便验证 DAC 包和升级操作。 有关如何执行这些检查的详细信息，请参阅 [Validate a DAC Package](../../relational-databases/data-tier-applications/validate-a-dac-package.md)。  
  
-   建议您不要使用来自未知或不可信源的 DAC 包进行升级。 此类包可能包含恶意代码，这些代码可能会执行非预期的 Transact-SQL 代码，或者通过修改架构导致错误。 在使用来自未知或不可信源的包之前，请解压缩该 DAC 并检查代码，例如存储过程或者其他用户定义的代码。  
  
-   如果在部署了上一版本的 DAC 后对当前数据库进行了更改，则某些更改可能会阻碍升级的成功完成，或者可能被升级删除。 您应该首先生成并查看在数据库中进行的任何此类更改的报告。  
  
-   出于谨慎起见，应该生成升级将执行的架构更改的列表，并且检查该列表以免有任何问题。  
  
 DAC 包中的应用程序名称必须与当前已部署的 DAC 的应用程序名称匹配。 例如，如果当前 DAC 的应用程序名称是 **GeneralLedger**，则只能通过使用其应用程序也是 **GeneralLedger** 的 DAC 包进行升级。  
  
 请确保有足够事务日志空间可用于记录所有修改。  
  
###  <a name="security"></a><a name="Security"></a> Security  
 为了提高安全性，SQL Server 身份验证登录名存储在 DAC 包中且没有密码。 在部署或升级该包时，登录名将作为含有生成的密码的已禁用登录名创建。 若要启用这些登录名，请使用具有 ALTER ANY LOGIN 权限的登录名登录，并且使用 ALTER LOGIN 来启用该登录名并且分配可以传达给用户的新密码。 对于 Windows 身份验证登录名则无需执行此操作，因为其密码不是由 SQL Server 管理的。  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 DAC 只能由 **sysadmin** 或 **serveradmin** 固定服务器角色的成员升级，或者由 **dbcreator** 固定服务器角色中具有 ALTER ANY LOGIN 权限的登录名升级。 该登录名必须是现有数据库的所有者。 名为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sa **的内置** 系统管理员帐户也可以升级 DAC。  
  
##  <a name="using-the-upgrade-data-tier-application-wizard"></a><a name="UsingDACUpgradeWizard"></a> 使用“升级数据层应用程序向导”  
 **使用向导升级 DAC**  
  
1.  在 **“对象资源管理器”** 中，展开包含要升级的 DAC 的实例的节点。  
  
2.  展开“管理”节点，然后展开“数据层应用程序”节点。  
  
3.  右键单击要升级的 DAC 的节点，然后选择“升级数据层应用程序…”  
  
4.  完成向导对话框：  
  
    1.  [“简介”页](#Introduction)  
  
    2.  [“选择包”页](#Select_dac_package)  
  
    3.  [“查看策略”页](#Review_policy)  
  
    4.  [“检测更改”页](#Detect_change)  
  
    5.  [查看升级计划](#ReviewUpgPlan)  
  
    6.  [摘要页](#Summary)  
  
    7.  [“升级 DAC”页](#Upgrade)  
  
##  <a name="introduction-page"></a><a name="Introduction"></a> “简介”页  
 此页描述数据层应用程序的升级步骤。  
  
 **不再显示此页。** - 选中该复选框可以停止在将来显示此页。  
  
 **下一步 >** - 继续到“选择包”页。  
  
 **取消** - 终止向导且不升级 DAC。  
  
##  <a name="select-package-page"></a><a name="Select_dac_package"></a> “选择包”页  
 使用此页可以指定包含数据层应用程序的新版本的 DAC 包。 该页可为两种状态。  
  
### <a name="select-the-dac-package"></a>选择 DAC 包  
 使用该页的初始状态可以选择要部署的 DAC 包。 该 DAC 包必须是有效的 DAC 包文件，并且必须具有 .dacpac 扩展名。 DAC 包中的 DAC 应用程序名称必须与当前 DAC 的应用程序名称相同。  
  
 **DAC 包** - 指定包含数据层应用程序的新版本的 DAC 包的路径和文件名。 您可以选择框右侧的 **“浏览”** 按钮以便浏览到 DAC 包的位置。  
  
 **应用程序名称** - 一个只读框，它显示创作 DAC 或者从某一数据库中提取 DAC 时分配的 DAC 应用程序名称。  
  
 “版本”- 一个只读框，它显示创作 DAC 或者从某一数据库中提取 DAC 时分配的版本。  
  
 “说明”- 一个只读框，它显示创作 DAC 或者从某一数据库中提取 DAC 时编写的版本。  
  
 **< 上一步** - 返回到“简介”页。  
  
 “下一步 >”- 显示进度栏，因为向导已确认所选文件为有效的 DAC 包。  
  
 **取消** - 终止向导且不升级 DAC。  
  
### <a name="validating-the-dac-package"></a>验证 DAC 包  
 在向导确认所选文件是有效的 DAC 包时显示一个进度栏。 如果验证该 DAC 包，则向导将继续到 **“查看策略”** 页。 如果该文件不是有效的 DAC 包，则向导会保持在 **“选择 DAC 包”** 页上。 或者选择另一个有效的 DAC 包，或者取消该向导并且生成一个新的 DAC 包。  
  
 “正在验证 DAC 的内容”- 报告验证过程的当前状态的进度栏。  
  
 “< 上一步”- 返回到“选择包”页的初始状态。  
  
 “下一步 >”- 继续到“选择包”页的最终版本。  
  
 “取消”- 终止向导且不部署 DAC。  
  
##  <a name="review-policy-page"></a><a name="Review_policy"></a> “查看策略”页  
 使用此页可查看评估 DAC 服务器选择策略的结果（如果该 DAC 具有策略）。 该 DAC 服务器选择策略是可选的，并分配给在 Microsoft Visual Studio 中创作的 DAC。 该策略使用该服务器选择策略方面指定 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例为承载该 DAC 而必须满足的条件。  
  
 **策略条件的评估结果** - 一个只读报告，显示 DAC 服务器选择策略中的条件评估是否成功。 将在单独的行上报告对每个条件进行评估的结果。  
  
 **忽略违反策略情况** - 使用此复选框可以在未能满足一个或多个策略条件的情况下继续进行升级。 只有在您确保未满足的所有条件都不会阻碍 DAC 操作成功条件的情况下，才选择此选项。  
  
 “< 上一步”- 返回到“选择包”页。  
  
 **下一步 >** - 继续到“检测更改”页。  
  
 **取消** - 终止向导且不升级 DAC。  
  
##  <a name="detect-change-page"></a><a name="Detect_change"></a> “检测更改”页  
 使用此页可以报告向导对所发生的数据库更改的检测结果，这些更改应该是使数据库架构不同于在 **msdb** 的 DAC 元数据中存储的架构定义的更改。 例如，如果 CREATE、ALTER 或 DROP 语句已用于在最初部署 DAC 后从数据库中添加、更改或删除对象。 则该页将首先显示一个进度栏，然后报告分析的结果。  
  
 **正在检测更改，这可能需要几分钟的时间** - 在向导检查数据库的当前架构和 DAC 定义中的对象之间的差异时显示一个进度栏。  
  
 **更改检测结果:** - 指示分析已完成并且在下面报告结果。  
  
 **数据库 DatabaseName 尚未更改** - 向导检测到在数据库中定义的对象和 DAC 定义中其匹配对象之间没有差异。  
  
 **数据库 DatabaseName 已更改** - 向导检测到数据库中的对象和 DAC 定义中其匹配对象之间发生了更改。  
  
 **继续而不管可能的更改丢失** - 指定你理解当前数据库中的某些对象或数据在新数据库中将不存在，并且愿意继续进行升级。 只有在您对更改报表进行了分析并且理解您必须执行以便手动传输新数据库中所需的任何对象或数据的步骤后，才应选择此按钮。 如果您不能确定，请单击 **“保存报表”** 按钮以便保存更改报表，然后单击 **“取消”**。 对报表进行分析，计划如何在升级完成后传输所需的任何对象和数据，然后重新启动该向导。  
  
 **保存报表** - 单击此按钮可以保存向导检测到的数据库中对象和其在 DAC 定义中的匹配对象之间的更改的报表。 然后，您可以查看该报表以确定是否需要在升级完成后执行一些操作，以便将报表中列出的某些或全部对象合并到新的数据库中。  
  
 “< 上一步”- 返回到“选择 DAC 包”页。  
  
 **下一步 >** - 继续到“选项”页。  
  
 “取消”- 终止向导且不部署 DAC。  
  
## <a name="options-page"></a>“选项”页  
 使用此页可以选择用于升级的失败时回滚选项。  
  
 **失败时回滚** - 选择此选项可以将升级封装在向导在出错时可尝试回滚的事务中。 有关该选项的详细信息，请参阅 [选择 DAC 升级选项](#ChoseDACUpgOptions)。  
  
 **还原默认值** - 将选项恢复为默认设置 False。  
  
 **< 上一步** - 返回到“检测更改”页。  
  
 **下一步 >** - 继续到“查看升级计划”页。  
  
 “取消”- 终止向导且不部署 DAC。  
  
##  <a name="review-the-upgrade-plan-page"></a><a name="ReviewUpgPlan"></a> “查看升级计划”页  
 使用此页可以查看升级过程将执行的操作。 仅当确信升级不会导致问题时才继续。  
  
 **将使用以下操作升级 DAC。** - 查看显示的信息以便确保将执行的操作正确。 “操作”列显示要用于执行升级的操作（如 Transact-SQL 语句）。 **“数据丢失”** 列将包含在相关操作删除数据时给出的警告。  
  
 **刷新** - 刷新操作列表。  
  
 **保存操作报告** - 将操作窗口的内容保存到某一 HTML 文件。  
  
 **继续而不管可能的更改丢失** - 指定你理解当前数据库中的某些对象或数据在新数据库中将不存在，并且愿意继续进行升级。 只有在您对更改报表进行了分析并且理解您必须执行以便手动传输新数据库中所需的任何对象或数据的步骤后，才应选择此按钮。 如果你不能确定，请单击“保存操作报告”按钮以保存更改报告，单击“保存脚本”按钮以保存 Transact-SQL 脚本，然后单击“取消”。 对报告和脚本进行分析，计划如何在升级完成后传输所需的任何对象和数据，然后重新启动该向导。  
  
 **保存脚本** - 将用于执行升级的 Transact-SQL 语句保存为文本文件。  
  
 **还原默认值** - 将选项恢复为默认设置 False。  
  
 **< 上一步** - 返回到“检测更改”页。  
  
 “下一步 >”- 继续到“摘要”页。  
  
 “取消”- 终止向导且不部署 DAC。  
  
##  <a name="summary-page"></a><a name="Summary"></a> 摘要页  
 使用此页可以最后查看在升级 DAC 时向导将执行的操作。  
  
 **将使用以下设置升级 DAC。** - 查看显示的信息以便确保将执行的操作正确。 该窗口将显示您选择要升级的 DAC 以及包含该 DAC 的新版本的 DAC 包。 该窗口还显示数据库的当前版本是否与当前的 DAC 定义相同，或者显示数据库是否已更改。  
  
 **< 上一步** - 返回到“查看升级计划”页。  
  
 **下一步 >** - 部署 DAC，并在“升级 DAC”页中显示结果。  
  
 “取消”- 终止向导且不部署 DAC。  
  
##  <a name="upgrade-dac-page"></a><a name="Upgrade"></a> “升级 DAC”页  
 此页报告升级操作是成功还是失败。  
  
 **升级 DAC** - 报告为升级 DAC 而执行的每个操作是成功还是失败。 查看信息以便确定每个操作是成功还是失败。 遇到了错误的任何操作都将在 **“结果”** 列中具有一个链接。 选择该链接可以查看针对该操作的错误报告。  
  
 **保存报表** - 选择此按钮可以将升级报表保存到某一 HTML 文件。 该文件报告每个操作的状态，并且包括任何操作生成的所有错误。 默认文件夹是您的 Windows 帐户的 Documents 文件夹中的 SQL Server Management Studio\DAC Packages 文件夹。  
  
 “完成” - 终止向导。  
  
##  <a name="using-powershell"></a><a name="UpgradeDACPowerShell"></a> 使用 PowerShell  
 **使用 PowerShell 脚本中的 IncrementalUpgrade() 方法升级 DAC**  
  
1.  创建一个 SMO Server 对象，并且将该对象设置为包含要升级的 DAC 的实例。  
  
2.  打开 **ServerConnection** 对象，并连接到同一实例。  
  
3.  使用 **System.IO.File** 加载 DAC 包文件。  
  
4.  使用 **add_DacActionStarted** 和 **add_DacActionFinished** 订阅 DAC 升级事件。  
  
5.  设置 **DacUpgradeOptions**。  
  
6.  使用 **IncrementalUpgrade** 方法升级 DAC。  
  
7.  关闭用于读取 DAC 包文件的文件流。  
  
### <a name="example-powershell"></a>示例 (PowerShell)  
 下面的示例使用 MyApplication2017.dacpac 包中的新 DAC 版本，升级 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的默认实例上名为 MyApplication 的 DAC。  
  
```  
## Set a SMO Server object to the default instance on the local computer.  
CD SQLSERVER:\SQL\localhost\DEFAULT  
$srv = get-item .  
  
## Open a Common.ServerConnection to the same instance.  
$serverconnection = New-Object Microsoft.SqlServer.Management.Common.ServerConnection($srv.ConnectionContext.SqlConnectionObject)  
$serverconnection.Connect()  
$dacstore = New-Object Microsoft.SqlServer.Management.Dac.DacStore($serverconnection)  
  
## Load the DAC package file.  
$dacpacPath = "C:\MyDACs\MyApplication2017.dacpac"  
$fileStream = [System.IO.File]::Open($dacpacPath,[System.IO.FileMode]::OpenOrCreate)  
$dacType = [Microsoft.SqlServer.Management.Dac.DacType]::Load($fileStream)  
  
## Subscribe to the DAC upgrade events.  
$dacstore.add_DacActionStarted({Write-Host `n`nStarting at $(get-date) :: $_.Description})  
$dacstore.add_DacActionFinished({Write-Host Completed at $(get-date) :: $_.Description})  
  
## Upgrade the DAC and close the package.  
$dacName  = "MyApplication"  
  
## Set the upgrade options.  
$upgradeProperties = New-Object Microsoft.SqlServer.Management.Dac.DacUpgradeOptions  
$upgradeProperties.blockonchanges = $true  
$upgradeProperties.ignoredataloss = $false  
$upgradeProperties.rollbackonfailure = $true  
$ upgradeProperties.skippolicyvalidation = $false  
  
## Upgrade the DAC  
$dacstore.IncrementalUpgrade($dacName, $dacType, $upgradeProperties)  
## Close the package file.  
$fileStream.Close()  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据层应用程序](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [SQL Server PowerShell](../../powershell/sql-server-powershell.md)  
  
