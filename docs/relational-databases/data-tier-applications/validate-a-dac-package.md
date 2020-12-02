---
description: 验证 DAC 包
title: 验证 DAC 包 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- data-tier application [SQL Server], validate
- data-tier application [SQL Server], compare
- validate DAC
- compare DACs
- data-tier application [SQL Server], view
- view DAC
ms.assetid: 726ffcc2-9221-424a-8477-99e3f85f03bd
author: stevestein
ms.author: sstein
ms.openlocfilehash: 717d81dd0f6f7f55af78ec4264b4f3b9fb329583
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88471370"
---
# <a name="validate-a-dac-package"></a>验证 DAC 包
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  最好在生产中部署 DAC 包之前查看其内容，并且在升级现有 DAC 之前验证升级操作。 当部署的包并非您的组织开发时更需要这样做。  
  
1.  **准备工作：**  [先决条件](#Prerequisites)  
  
2.  **若要升级 DAC，请使用：**  [查看 DAC 的内容](#ViewDACContents)、 [查看数据库更改](#ViewDBChanges)、 [查看升级操作](#ViewUpgradeActions)、 [Compare DACs](#CompareDACs)  

##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 建议您不要从未知或不可信源部署 DAC 包。 此类 DAC 可能包含恶意代码，这些代码可能会执行非预期的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码，或者通过修改架构导致错误。 使用来自未知源或不可信源的 DAC 前，请在[!INCLUDE[ssDE](../../includes/ssde-md.md)]的独立测试实例上部署它，对数据库运行 [DBCC CHECKDB (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)，然后检查数据库中的代码，例如存储过程或其他用户定义的代码。  
  
##  <a name="view-the-contents-of-a-dac"></a><a name="ViewDACContents"></a> 查看 DAC 的内容  
 有两种机制可用于查看数据层应用程序 (DAC) 包的内容。 您可以在 SQL Server 开发工具中将 DAC 包导入到某一 DAC 项目。 您可以将该包的内容解压缩到一个文件夹中。  
  
 **在 SQL Server 开发工具中查看 DAC**  
  
1.  打开“文件”菜单，选择“新建”，然后选择“项目…”  。  
  
2.  选择 **SQL Server** 项目模板，然后指定 **“名称”**、 **“位置”** 和 **“解决方案名称”**。  
  
3.  在“解决方案资源管理器”中，右键单击该项目节点，然后选择“属性…” 。  
  
4.  在“项目设置” 选项卡上的“输出类型”部分中，选中“数据层应用程序（.dacpac 文件）”复选框，然后关闭属性对话框。  
  
5.  在“解决方案资源管理器”中，右键单击该项目节点，然后选择“导入数据层应用程序…” 。  
  
6.  使用“解决方案资源管理器”可打开该 DAC 中的所有文件，例如服务器选择策略以及预部署和部署后脚本。  
  
7.  使用 **“架构视图”** 可查看架构中的所有对象，特别是查看对象（例如函数或存储过程）中的代码。  
  
 **查看文件夹中的 DAC**  
  
-   按照 [Unpack a DAC Package](../../relational-databases/data-tier-applications/unpack-a-dac-package.md)中的说明将 DAC 包解压缩到一个文件夹中。  
  
-   通过在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器中打开 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]脚本来查看其内容。  
  
-   在记事本等工具中查看文本文件的内容。  
  
##  <a name="view-database-changes"></a><a name="ViewDBChanges"></a> 查看数据库更改  
 将 DAC 的当前版本部署到生产后，可能已直接对关联的数据库进行了更改，这些更改可能与新版本 DAC 中定义的架构相冲突。 升级到新版本的 DAC 之前，检查是否已对数据库进行了此类更改。  
  
 **使用向导查看数据库更改**  
  
1.  运行“升级数据层应用程序”向导，指定当前部署的 DAC 和包含新版本 DAC 的 DAC 包。  
  
2.  在 **“检测更改”** 页上，查看已对数据库进行的更改的报告。  
  
3.  如果您不希望继续进行升级，则选择 **“取消”** 。  
  
4.  有关使用该向导的详细信息，请参阅 [升级数据层应用程序](../../relational-databases/data-tier-applications/upgrade-a-data-tier-application.md)。  
  
 **使用 PowerShell 查看数据库更改**  
  
1.  创建一个 SMO Server 对象，并且将该对象设置为包含要查看的 DAC 的实例。  
  
2.  打开 **ServerConnection** 对象，并连接到同一实例。  
  
3.  在变量中指定 DAC 名称。  
  
4.  使用 **GetDatabaseChanges()** 方法检索 **ChangeResults** 对象，并且将该对象通过管道传递给某个文本文件，以便生成新的、已删除和已更改的对象的简单报表。  
  
### <a name="view-database-changes-example-powershell"></a>查看数据库更改示例 (PowerShell)  
 **查看数据库更改示例 (PowerShell)**  
  
 下面的示例报告已在名为 MyApplicaiton 的已部署 DAC 中进行的所有数据库更改。  
  
```  
## Set a SMO Server object to the default instance on the local computer.  
CD SQLSERVER:\SQL\localhost\DEFAULT  
$srv = get-item .  
  
## Open a Common.ServerConnection to the same instance.  
$serverconnection = New-Object Microsoft.SqlServer.Management.Common.ServerConnection($srv.ConnectionContext.SqlConnectionObject)  
$serverconnection.Connect()  
$dacstore = New-Object Microsoft.SqlServer.Management.Dac.DacStore($serverconnection)  
  
## Specify the DAC instance name.  
$dacName  = "MyApplication"  
  
## Generate the change list and save to file.  
$dacChanges = $dacstore.GetDatabaseChanges($dacName) | Out-File -Filepath C:\DACScripts\MyApplicationChanges.txt  
```  
  
##  <a name="view-upgrade-actions"></a><a name="ViewUpgradeActions"></a> 查看升级操作  
 在使用新版本的 DAC 包升级从早期的 DAC 包部署的 DAC 之前，您可以生成一个报告，该报告包含在升级期间将运行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，然后查看该语句。  
  
 **使用向导报告升级操作**  
  
1.  运行“升级数据层应用程序”向导，指定当前部署的 DAC 和包含新版本 DAC 的 DAC 包。  
  
2.  对 **“摘要”** 页上，查看升级操作的报告。  
  
3.  如果您不希望继续进行升级，则选择 **“取消”** 。  
  
4.  有关使用该向导的详细信息，请参阅 [升级数据层应用程序](../../relational-databases/data-tier-applications/upgrade-a-data-tier-application.md)。  
  
 **使用 PowerShell 报告升级操作**  
  
1.  创建一个 SMO Server 对象，并将该对象设置为包含部署的 DAC 的实例。  
  
2.  打开 **ServerConnection** 对象，并连接到同一实例。  
  
3.  使用 **System.IO.File** 加载 DAC 包文件。  
  
4.  在变量中指定 DAC 名称。  
  
5.  使用 **GetIncrementalUpgradeScript()** 方法来获取升级时将运行的 Transact-SQL 语句的列表，然后将该列表通过管道传递给某个文本文件。  
  
6.  关闭用于读取 DAC 包文件的文件流。  
  
### <a name="view-upgrade-actions-example-powershell"></a>查看升级操作示例 （PowerShell)  
 **查看升级操作示例 （PowerShell)**  
  
 以下示例报告一些 Transact-SQL 语句，为了将名为 MyApplicaiton 的 DAC 升级到 MyApplication2017.dacpac 文件中定义的架构将运行这些语句。  
  
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
  
## Specify the DAC instance name.  
$dacName  = "MyApplication"  
  
## Generate the upgrade script and save to file.  
$dacstore.GetIncrementalUpgradeScript($dacName, $dacType) | Out-File -Filepath C:\DACScripts\MyApplicationUpgrade.sql  
  
## Close the filestream to the new DAC package.  
$fileStream.Close()  
```  
  
##  <a name="compare-dacs"></a><a name="CompareDACs"></a> Compare DACs  
 在升级 DAC 之前，最好检查当前 DAC 和新 DAC 之间数据库和实例级别对象中的差别。 如果您对于当前 DAC 没有该包的副本，则可以从当前数据库中提取一个包。  
  
 如果您在 SQL Server 开发工具中将两个 DAC 包都导入 DAC 项目中，则可以使用架构比较工具来分析这两个 DAC 之间的差别。  
  
 或者，将 DAC 解压缩到单独的文件夹。 然后，可以使用 WinDiff 实用工具之类的差异工具分析这些差异。  
  
## <a name="see-also"></a>另请参阅  
 [数据层应用程序](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [部署数据层应用程序](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md)   
 [升级数据层应用程序](../../relational-databases/data-tier-applications/upgrade-a-data-tier-application.md)  
  
  
