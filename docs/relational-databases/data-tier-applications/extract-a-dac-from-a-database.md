---
description: 从数据库中提取 DAC
title: 从数据库中提取 DAC | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2016
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.extractdacwizard.validationandsummary.f1
- sql13.swb.extractdacwizard.introduction.f1
- sql13.swb.extractdacwizard.selectdatapage.f1
- sql13.swb.extractdacwizard.setdacproperties.f1
- sql13.swb.extractdacwizard.buildandsave.f1
helpviewer_keywords:
- extract DAC
- How to [DAC], extract
- data-tier application [SQL Server], extract
- wizard [DAC], extract
ms.assetid: ae52a723-91c4-43fd-bcc7-f8de1d1f90e5
author: stevestein
ms.author: sstein
ms.openlocfilehash: 5091c3437d0e74f3c108e13efb3789997cadb2d3
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489461"
---
# <a name="extract-a-dac-from-a-database"></a>从数据库中提取 DAC
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  使用“提取数据层应用程序向导”或 Windows PowerShell 脚本可以从现有 SQL Server 数据库提取数据层应用程序 (DAC) 包。 提取过程将创建一个 DAC 包文件，其中包含数据库对象及其相关实例级别元素的定义。 例如，一个 DAC 包文件包含数据库表、存储过程、视图、用户以及映射到数据库用户的登录名。  
  
 
## <a name="before-you-begin"></a>开始之前  
 您可以从驻留在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]或者 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 4 或更高版本的实例上的数据库中提取 DAC。 如果对已从 DAC 部署的数据库运行提取进程，则仅提取数据库中对象的定义。 该进程并不引用在 **msdb** 中注册的 DAC（在 **中为** master [!INCLUDE[ssSDS](../../includes/sssds-md.md)]）。 该提取进程不注册当前数据库引擎实例中的 DAC 定义。 有关注册 DAC 的详细信息，请参阅 [Register a Database As a DAC](../../relational-databases/data-tier-applications/register-a-database-as-a-dac.md)。  
  
##  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> 限制和局限  
 只能从 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]、 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 4 (SP4) 或更高版本的数据库中提取 DAC。 如果数据库有 DAC 中不支持的对象或包含的用户，则不能提取 DAC。 有关 DAC 中支持的对象类型的详细信息，请参阅 [DAC Support For SQL Server Objects and Versions](/previous-versions/sql/sql-server-2012/ee210549(v=sql.110))。  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 提取 DAC 至少要求 ALTER ANY LOGIN 和数据库作用域 VIEW DEFINITION 权限，以及对 **sys.sql_expression_dependencies** 具有 SELECT 权限。 提取 DAC 可由 securityadmin 固定服务器角色的成员（也是从其提取 DAC 的数据库中 database_owner 固定数据库角色的成员）完成。 sysadmin 固定服务器角色的成员或名为 **sa** 的内置 SQL Server 系统管理员帐户也可以提取 DAC。  
  
##  <a name="using-the-extract-data-tier-application-wizard"></a><a name="UsingDACExtractWizard"></a> 使用“提取数据层应用程序向导”  
 **使用向导提取 DAC**  
  
1.  在 **“对象资源管理器”** 中，展开包含要从其提取 DAC 的数据库的实例的节点。  
  
2.  展开 **“数据库”** 节点。  
  
3.  右键单击要从其提取 DAC 的数据库的节点，指向“任务”，然后选择“提取数据层应用程序…”  
  
4.  完成向导对话框：  
  
    1.  [“简介”页](#Introduction)  
  
    2.  [“选择数据”页](#SelectData)  
  
    3.  [“设置属性”页](#SetProperties)  
  
    4.  [“验证和摘要”页](#ValidateSummary)  
  
    5.  [“生成包”页](#BuildPackage)  
  
###  <a name="wizard-introduction-page"></a><a name="Introduction"></a> “向导”简介页  
 此页描述用于提取数据层应用程序的各个步骤。  
  
 **不再显示此页。** - 选中该复选框可以停止在将来显示此页。  
  
 “下一步 >” - 进入“选择方法”页。  
  
 **取消** - 结束向导且不从数据层提取数据层应用程序。  
  
 [[提取向导]](#UsingDACExtractWizard)  
  
###  <a name="select-data-page"></a><a name="SelectData"></a> Select data page  
选择要包括在数据层应用程序 (DAC) 包文件中的引用数据。 在 DAC 包中包括数据是可选的。 DAC 包将已包括与数据库相关的所有受支持的数据库对象和实例对象的架构。  
  
 您可以在 DAC 包文件中包含多达 10 MB 的引用数据。 但是，对于要包含在该 DAC 中的表，它们可能不包含二进制大型对象 (BLOB) 数据类型（如 **image** 或 **varchar(max)**）。 若要提取大量数据以转移到另一个数据库，应使用 SQL Server Integration Services、大容量复制实用工具或许多其他数据迁移方法之一。  
  
 **数据库表** - 对于包含你要包括在 DAC 包中的数据的数据库表，在其旁边选中对应的复选框。 您最多可以选择十个表，这些表中具有 10,000 行或更少的行。  
  
 [[提取向导]](#UsingDACExtractWizard)  
  
###  <a name="set-properties-page"></a><a name="SetProperties"></a> Set properties page  
 使用该向导页描述数据层应用程序 (DAC)。 这些属性用于标识 DAC，并有助于与其他应用程序区分开。  
  
 **名称** - 此名称标识 DAC。 它可以不同于 DAC 包文件的名称，并且应描述您的应用程序。 例如，如果数据库用于财务应用程序，则可能要命名为“DAC Finance”。  
  
 **版本（使用 xx.xx.xx.xx，其中 x 是数字）** - 标识 DAC 版本的数值。 该 DAC 版本用于 Visual Studio 中，以便标识开发人员正在处理的 DAC 的版本。 在部署 DAC 时，该版本存储于 **msdb** 数据库中，并且以后可以在 **中的“数据层应用程序”** [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]节点下查看。  
  
 **说明：** - 可选。 说明该 DAC。 在部署 DAC 时，该描述存储于 **msdb** 数据库中，并且以后可以在 **中的“数据层应用程序”** [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]节点下查看。  
  
 **保存到 DAC 包文件(文件名中包括 .dacpac 扩展名)：** - 将 DAC 保存到某一 DAC 包文件，并且具有 .dacpac 扩展名。 单击 **“浏览”** 按钮可以指定文件的名称和位置。  
  
 **覆盖现有文件** - 如果已存在同名的 DAC 包文件，则选中此复选框可以替换该文件。  
  
###  <a name="validation-and-summary-page"></a><a name="ValidateSummary"></a> Validation and summary page  
 在此页上，该向导将验证数据层应用程序 (DAC) 是否支持所有数据库对象。 它还检查数据库对象之间的依赖关系，以便确定该组对象是否可以成功包括在 DAC 中。 之后，它将显示一个验证报表，汇总您在此向导中选择的选项。 若要更改某个选项，请单击 **“上一步”**。 若要开始提取 DAC，请单击 **“下一步”**。  
  
> **注意！** 如果 DAC 不支持一个或多个对象，则“下一步”按钮将被禁用，并且提取过程无法继续。 在这样的情况下，建议删除不支持的对象，然后再次运行此向导。  
  
 **摘要** - 你已选择的选项的摘要在“DAC 属性”下列出。 验证的结果在 **“DAC 对象”** 下列出。 有三种类型的验证结果：  
  
-   **对象成功包括在 DAC 中**：支持这些对象及其依赖关系，并且它们可以成功包括在 DAC 中。  
  
-   **对象包括在 DAC 中但具有警告**：支持这些对象，但依赖于在 DAC 中不支持的其他对象。  
  
-   **对象不包括在 DAC 中**：不支持这些对象，并且这些对象必须从数据库中删除后才能成功提取 DAC。  
  
 验证过程将检查多个级别的依赖关系。 例如，如果某一存储过程依赖于使用不支持的 CLR 数据类型的表，则该存储过程将在 **“对象包括在 DAC 中但具有警告”** 下列出。  
  
 如果 DAC 不支持一个或多个对象，则 **“下一步”** 按钮将被禁用，并且提取过程将停止。 在这样的情况下，建议删除不支持的对象，然后再次运行此向导。  
  
 **保存报告** - 使你可以保存一个基于 HTML 的文件，该文件列出摘要中“DAC 对象”节点下的所有对象。 当在 DAC 中不支持您的一些数据库对象时，此报表可能会非常有用。 使用此报表可以在尝试再次提取 DAC 前更改或删除不支持的对象。  
  
 ###  <a name="build-package-page"></a><a name="BuildPackage"></a> Build package page  
 使用该页监视向导在提取数据层应用程序 (DAC) 时的进度。  
  
 **操作** - 在“创建并保存 DAC 包文件”操作期间，向导从你的 SQL Server 数据库提取 DAC。 然后，一个 DAC 包将在内存中创建并保存到您指定的位置中。 单击 **“结果”** 列中的链接可以查看相应步骤的结果。  
  
 **保存报告** - 单击此项可将向导的进度结果保存到文件中。  
  
 “完成” - 单击此项可在完成处理后或出错时关闭向导。  
   
##  <a name="extract-a-dac-using-powershell"></a><a name="ExtractDACPowerShell"></a> 使用 PowerShell 提取 DAC  
 **使用 PowerShell 脚本中的 Extract() 方法从数据库提取 DAC**  
  
1.  创建一个 SMO Server 对象，并且将其设置为包含要从其提取 DAC 的数据库的实例。  
  
2.  添加一个指定数据库名称的变量。  
  
3.  指定 DAC 的元数据，如 DAC 名称、版本和说明。  
  
4.  为提取的 DAC 包文件指定路径和文件名。  
  
5.  使用上面指定的信息运行 Extract 方法。  
  
### <a name="example-powershell"></a>示例 (PowerShell)  
 下面的示例从名为 MyDB 的数据库中提取名为 MyApplication 的 DAC。  
  
```  
## Set a SMO Server object to the default instance on the local computer.  
CD SQLSERVER:\SQL\localhost\DEFAULT  
$srv = get-item .  
  
## Specify the database to extract to a DAC.  
$dbname = "MyDB"  
  
## Specify the DAC metadata.  
$applicationname = "MyApplication"  
$version = "1.0.0.0"  
$description = "This DAC defines the database used by my application."  
  
## Specify the location and name for the extracted DAC package.  
$dacpacPath = "C:\MyDACs\MyApplication.dacpac"  
  
## Extract the DAC.  
$extractionunit = New-Object Microsoft.SqlServer.Management.Dac.DacExtractionUnit($srv, $dbname, $applicationname, $version)  
$extractionunit.Description = $description  
$extractionunit.Extract($dacpacPath)  
```  
  
## <a name="see-also"></a>另请参阅  
 - [数据层应用程序](../../relational-databases/data-tier-applications/data-tier-applications.md) 
 - [开发管道中的 SqlPackage](../../tools/sqlpackage/sqlpackage-pipelines.md)