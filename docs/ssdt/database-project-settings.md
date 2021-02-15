---
title: 数据库项目设置
description: 了解数据库项目设置。 了解如何使用这些设置来控制数据库、调试和生成配置的各个方面。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.DebugProperties
- sql.data.tools.dacsettings.dialog
- sql.data.tools.dbsqlclrlanguagevb
- SQL.DATA.TOOLS.DBADVANCEDBUILDSETTINGSCS
- sql.data.tools.dbadvancedbuildsettingscs
- ql.data.tools.criticalerror.dialog
- sql.data.tools.dbassemblysigningchangekeypassword
- sql.data.tools.advanceddeploymentsystemdefined.dialog
- sql.data.tools.advanceddatabasesettings.dialog
- sql.data.tools.extendedpropertiesvalueeditor.dialog
- sql.data.tools.dbsqlclrlanguagecs
- ql.data.tools.dbassemblysigningpasswordneeded
- sql.data.tools.vbadvancedsettings.dialog
- sql.data.tools.dbsqlclr
- sql.data.tools.SqlCmdVariablesProperties
- sql.data.tools.dbassemblysigning
- sql.data.tools.advanceddeploymentsettings.dialog
- sql.data.tools.advancedpublishsettings.dialog
- sql.data.tools.BuildActionProperties
- sql.data.tools.serverselectionpolicy.dialog
- sql.data.tools.dbadvancedbuildsettingsvb
- sql.data.tools.dbprojectwizard.buildeventcommandline
- sql.data.tools.dbreferencepaths
- sql.data.tools.GeneralProperties
- sql.data.tools.BuildProperties
- sql.data.tools.DeployProperties
- sql.data.tools.csadvancedsettings.dialog
- sql.data.tools.dbassemblyinfo
- sql.data.tools.extendedpropertieseditor.dialog
ms.assetid: 34418730-1aaa-4948-aee2-8f1e62cda85c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 99552883fb06c01a862ddc973ff3747d11a89ee8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079548"
---
# <a name="database-project-settings"></a>数据库项目设置

使用数据库项目设置可控制数据库、调试和生成配置的各个方面。 这些设置分为以下几类。  
  
-   [项目设置](#bkmk_proj_settings)  
  
-   [扩展的 Transact-SQL 验证](#bkmk_evf)  
  
-   [SQLCLR](#bkmk_sqlclr)  
  
-   [SQLCLR 和 SQLCLR 生成](#bkmk_sqlclr_sqlclrbuild)  
  
-   [生成](#bkmk_build)  
  
-   [SQLCMD 变量](#bkmk_sqlcmd_variables)  
  
-   [生成事件](#bkmk_build_events)  
  
-   [调试](#bkmk_debug)  
  
-   [引用路径](#bkmk_ref_paths)  
  
-   [代码分析](#bkmk_code_analysis)  
  
### <a name="to-configure-properties-for-your-database-project"></a>为数据库项目配置属性  
  
1.  在“解决方案资源管理器”中，右键单击要为其配置属性的数据库项目，然后选择“属性”。  
  
    或者，在“解决方案资源管理器”  中双击项目的“属性” 节点。  
  
2.  此时将显示数据库项目的属性表。  
  
3.  单击“项目设置”  选项卡。现在，您可以配置数据库项目属性的常规属性了。 请注意左窗格上各个选项卡（代表不同类别）的可用性。  
  
## <a name="project-settings"></a><a name="bkmk_proj_settings"></a>项目设置  
下表中的设置适用于此数据库项目的所有配置。  
  
|字段|默认值|说明|  
|---------|-----------------|---------------|  
|目标平台|Microsoft SQL Server 2012|指定针对此数据库项目使用的 SQL Server 版本。|  
|对常见对象启用扩展 Transact\-SQL 验证。|创建新项目时不启用。<br /><br />当你从连接到 SQL Azure 的 SQL Server 对象资源管理器创建项目、将 Azure SQL 数据库导入到项目中或将项目的“目标平台”更改为“SQL Azure”时启用。|启用此选项时，将报告项目中发现的 SQL Server 编译器验证失败的错误。 如果将目标平台更改为 SQL Azure，则会启用扩展验证。 如果您更改目标平台，则不会取消选中该选项。<br /><br />可以为其他版本的 SQL Server 启用此选项，但验证将限制为 Microsoft SQL Server 2012 部分包含数据库和 SQL Azure。 并非所有 Transact\-SQL 语法都受到所有 SQL Server 版本的支持。<br /><br />有关详细信息，请参阅本主题中稍后的[扩展的 Transact-SQL 验证](#bkmk_evf)|  
|输出类型|||  
|数据层应用程序包（.dacpac 文件）|启用并且锁定。 在生成一个数据库项目时，该数据库项目的生成输出始终生成一个 .dacpac 包。|如果正在使用具有“创建其他下级 .dacpac 文件 (v2.0)”选项的 SQL Server 数据工具 (SSDT) 版本，且希望包与 SQL Server Management Studio 或 SQL Azure 管理门户兼容，则选中该选项。 可以从 (SSDT) 直接部署 .dacpac 包，但在发布 SQL Server 数据工具时只能通过 SQL Server Management Studio 部署 2.0 版本的 .dacpac 文件。|  
|创建脚本（.sql 文件）||指定完整 .sql CREATE 脚本是否为项目中的所有对象生成，以及当项目生成时该脚本是否位于 bin\debug 文件夹中。 您可以使用 **“项目发布”** 命令或 SQL Compare 实用工具创建增量更新脚本。|  
|泛型|||  
|默认架构|dbo|指定在其中创建 SQLCLR 和 Transact\-SQL 对象的默认架构。 可以通过直接对项目指定架构来重写此设置。|  
|在文件名中包括架构名称|否|指定文件名是否包括架构作为前缀（例如，dbo.Products.table.sql）。 如果清除此复选框，则对象的文件名将采用以下形式：ObjectName.ObjectType.sql（例如 Products.table.sql）。|  
|验证标识符的大小写|是|指定在项目生成时是否验证其中的 SQL 对象中的标识符的大小写。 此选项适用于为数据库指定区分大小写的排序规则的数据库项目。|  
|数据库设置|基于数据库的标准配置设置的默认设置|您可以指定的设置示例包括针对 SQL Server 数据库的排序规则方法和数据库级别设置。|  
  
## <a name="extended-transact-sql-verification"></a><a name="bkmk_evf"></a>扩展的 Transact-SQL 验证  
  
> [!IMPORTANT]  
> 扩展 Transact-SQL 验证功能将从 SQL Server 数据工具的下一个功能版本和 Visual Studio 的下一个主要版本中删除。  
  
扩展的 Transact-SQL 验证是数据库项目系统内的一项功能，开发人员利用它可以在构建时将其数据库项目提交到 Transact-SQL 编译器服务，以针对 SQL Server 引擎的解析器和解释器验证其代码。  
  
### <a name="transact-sql-compiler-service"></a>Transact-SQL 编译器服务  
Transact-SQL 编译器服务是基于 Microsoft SQL Server 2012 数据库引擎的组件。 此服务可验证 DDL 语句的语法和语义，保真度与 Microsoft SQL Server 2012 数据库引擎相同。 从内在本质看，这意味着编译器服务不支持 Microsoft SQL Server 2012 中不推荐使用的语法或功能。 要详细了解已弃用的功能，请参见 [SQL Server 2012 中中止的数据库引擎功能](../database-engine/discontinued-database-engine-functionality-in-sql-server.md)。  
  
为了进行数据库项目验证，编译器服务会创建一个部分包含的数据库，并会针对该数据库模拟执行 DDL 语句。 有关详细信息，请参阅 [部分包含的数据库](/previous-versions/sql/sql-server-2012/ff929071(v=sql.110))。  
  
编译器服务具有两类限制。  
  
依赖数据库或实例配置的功能包括：  
  
-   3 或 4 部分对象引用  
  
-   FileTable  
  
-   更改跟踪  
  
-   Rowset 函数 - OPENROWSET、OPENQUERY、OPENDATASOURCE  
  
-   全文语义搜索  
  
目前不支持验证的功能包括：  
  
-   Service Broker  
  
-   具有用户定义的 FileGroup 的分区架构  
  
-   SQL Azure 元数据排序规则（编译器服务使用 SQL Server 2012 部分包含的数据库元数据排序规则 - Latin1_General_100_CI_AS_KS_WS_SC）  
  
### <a name="enablingdisabling-extended-verification"></a>启用/禁用扩验证  
在直接从 Azure SQL 数据库创建的数据库项目中，或在“目标平台”设置为“SQL Azure”的项目中，扩展的 Transact-SQL 验证是默认启用的。 在为 SQL Azure 数据库或以 SQL Server 2012 为目标的应用程序范围的数据库开发时，推荐使用扩展验证。 有关应用程序范围的数据库的详细信息，请参阅 [部分包含的数据库](/previous-versions/sql/sql-server-2012/ff929071(v=sql.110))。  
  
在为 SQL Server 2008/R2 开发应用程序范围的数据库以实现与 Microsoft SQL Server 2012 和 SQL Azure 兼容时，也可以使用扩展验证功能。  
  
##### <a name="to-enable-or-disable-extended-verification-at-the-project-level"></a>在项目级别启用或禁用扩展验证  
  
1.  在“解决方案资源管理器”中，右键单击项目文件，然后单击“属性”。  
  
2.  在“属性设置”中的“目标平台”下，选中或取消选中“对常见对象启用扩展 Transact-SQL 验证”。  
  
##### <a name="to-disable-extended-verification-at-the-file-level"></a>在文件级别启禁用扩展验证  
  
1.  在“解决方案资源管理器”中，右键单击 .sql 文件。  
  
    > [!NOTE]  
    > 为了在文件级别禁用扩展 Transact\-SQL 验证功能，文件的“生成操作”属性必须设置为“生成” 。  
  
2.  在“属性”中，将“扩展 T-SQL 验证”属性更改为 False。  
  
    ![文件属性](../ssdt/media/ssdt-evf.gif "文件属性")  
  
### <a name="special-considerations-for-collations"></a>排序规则的特殊注意事项  
有关部分包含的数据库中排序规则的详细信息，请参阅 [包含的数据库排序规则](/previous-versions/sql/sql-server-2012/ff929080(v=sql.110))。  
  
## <a name="sqlclr"></a><a name="bkmk_sqlclr"></a>SQLCLR  
有关“程序集”选项的信息，请参见 [“程序集信息”对话框](/visualstudio/ide/reference/assembly-information-dialog-box?queryresult=true)。  
  
有关签名的信息，请参见 [“签名”页, 项目设计器](/visualstudio/ide/reference/signing-page-project-designer?queryresult=true) 主题的“程序集签名”  一节。  
  
## <a name="sqlclr-and-sqlclr-build"></a><a name="bkmk_sqlclr_sqlclrbuild"></a>SQLCLR 和 SQLCLR 生成  
**SQLCLR** 和 **“SQLCLR 生成”** 属性页包含可供在您的项目中使用 SQL CLR 对象的许多设置。 具体来说， **SQLCLR** 属性页具有用于对 SQLCLR 程序集设置权限的权限级别设置。 它还具有“生成 DDL”设置，以便控制为已添加到项目的 SQLCLR 对象是否生成动态数据语言 (DDL)。 **“SQLCLR 生成”** 属性页包含可为配置项目中 SQLCLR 代码的编译而设置的所有编译器选项。  
  
**“SQLCLR 生成”** 属性页包含用于生成 SQL CLR 对象的高级生成设置。 已基于用来编写 SQL CLR 对象代码的语言（VB 或 C#）提供了不同选项。  
  
1.  如果对象是用 C# 编写的，则可以通过单击“SQLCLR 生成”属性页上的“高级”按钮访问这些选项。 可以在[“高级生成设置”对话框 (C#)](/visualstudio/ide/reference/advanced-build-settings-dialog-box-csharp) 中找到 C# 选项的说明。  
  
2.  如果对象是用 VB 编写的，则可以先选择 **“语言”** 下拉列表中的“VB”，然后单击 **“高级”** 按钮。 可以在[“高级编译器设置”对话框 (Visual Basic)](/visualstudio/ide/reference/advanced-compiler-settings-dialog-box-visual-basic) 中找到 VB 选项的说明  
  

## <a name="build"></a><a name="bkmk_build"></a>生成  
可以为解决方案中的每个数据库项目选择生成配置。 默认情况下有一个配置，但您可以添加自定义配置。 例如，如果您需要一个总是删除并重新创建数据库的自定义配置，您可以选择添加自定义配置。 在包含不同项目类型的解决方案中，可以创建一个自定义解决方案配置，其中包含每个项目的特定生成配置。  
  
#### <a name="to-specify-a-build-configuration-for-a-solution"></a>为解决方案指定生成配置  
  
1.  在“解决方案资源管理器” 中，单击要为其指定生成配置的解决方案节点。  
  
2.  在“生成”菜单上，单击“配置管理器” 。 此时将显示“配置管理器”对话框。  
  
    指定要对解决方案中的每个项目使用的配置设置。  
  
#### <a name="to-specify-a-build-configuration-for-a-database-project"></a>为数据库项目指定生成配置  
  
1.  在“解决方案资源管理器”中，右键单击要为其指定生成配置的数据库项目，然后选择“属性”。  
  
2.  在“生成”  选项卡上，使用“配置”  下拉列表指定要用于此项目的配置设置。  
  
下表中的设置适用于此数据库项目的生成配置。  
  
|字段|默认值|说明|  
|---------|-----------------|---------------|  
|生成输出路径|bin\Debug\|指定生成或部署数据库项目时产生生成输出的位置。 如果指定的是相对路径，则该路径必须是相对于数据库项目路径的路径。 如果路径不存在，则会创建一个。|  
|生成输出文件名|DatabaseProjectName|指定要为在生成数据库项目时所生成的输出赋予的名称|  
|将 Transact\-SQL 警告视为错误|否|指定出现 Transact\-SQL 警告是否应导致取消生成和部署过程。 如果清除此复选框，则将显示警告，但生成和部署过程将继续。 此设置特定于项目而不是用户，并且存储在 .sqlproj 文件中。|  
|禁止显示 Transact\-SQL 警告|空白|指定一个以逗号或分号分隔的警告编号列表，这些编号标识禁止显示的警告。<br /><br />即使选中了“将 Transact\-SQL 警告视为错误”复选框，禁止显示的警告也不会显示在“错误列表”窗口中，并且不会影响生成操作成功完成。|  
  
## <a name="sqlcmd-variables"></a><a name="bkmk_sqlcmd_variables"></a>SQLCMD 变量  
在 SQL Server 数据库项目中，您可以使用 SQLCMD 变量提供要用于调试或发布的动态替换。 您输入变量名称和值，并且在生成过程中，将替换这些值。 如果没有本地值，则会使用默认值。 通过在项目属性中输入这些变量，它们将在发布时自动提供并且存储于发布配置文件中。 您可以通过“加载值”按钮，将这些变量的项目值拖入发布中。  
  
请确保在项目属性中输入正确的变量，因为不会针对项目中的脚本对这些变量进行验证，并且不自动填充在脚本中使用的变量。  
  
此外，命令行发布还让您可以在命令行重写这些值或使用配置文件重写这些值。  
  
## <a name="build-events"></a><a name="bkmk_build_events"></a>生成事件  
使用这些设置可以指定要在生成操作开始之前执行的命令行以及要在生成操作完成之后执行的命令行。  
  
|字段|默认值|说明|  
|---------|-----------------|---------------|  
|预生成事件命令行|无|指定要在项目生成之前执行的命令行。 单击“编辑预生成事件”可修改该命令行。|  
|生成后事件命令行|无|指定要在项目生成之后执行的命令行。 单击“编辑生成后事件”可修改该命令行。|  
|运行生成后事件|成功生成时|指定生成后命令行应始终运行、仅在生成操作成功后运行还是仅在生成操作更新了项目输出（生成脚本）时运行。|  
  
## <a name="debug"></a><a name="bkmk_debug"></a>调试  
可以使用这些设置来控制数据库项目的调试。  
  
|字段|默认值|说明|  
|---------|-----------------|---------------|  
|启动操作|无|指定在调试您的项目时要运行的脚本或外部程序。|  
|目标连接字符串|Data Source=(localdb)\\*SolutionName*;Initial Catalog=*DatabaseProjectName*;Integrated Security=True;Pooling=False;Connect Timeout=30|指定要用作指定生成配置的目标的数据库服务器的连接信息。 默认连接字符串针对动态创建的 SQL Server LocalDB 实例和数据库。|  
|部署数据库属性|是|指定是否在部署数据库项目时部署或更新 DatabaseProperties.DatabasePropertie 设置。|  
|始终重新创建数据库|否|指定是否删除后重新创建数据库，而非执行增量升级。 例如，如果你要针对数据库的干净部署运行数据库单元测试，则可能需要选中此复选框。 如果清除此复选框，则将更新现有数据库，而不是删除并重新创建数据库。|  
|如果可能发生数据丢失则阻止增量部署|是|指定在更新可能会造成数据丢失的情况下是否应停止部署。 如果选中此复选框，则可能造成数据丢失的更改会导致部署因出现错误而停止，从而防止丢失数据。 例如，在将 `varchar(50)` 列更改为 `varchar(30)`时，部署将会停止。<br /><br />**注意：** 只有当可能发生数据丢失的表中包含数据时，才会阻止部署。 如果不会丢失任何数据，则部署将继续。|  
|DROP 目标中但不在项目中的对象|否|指定是否应将目标数据库中存在而数据库项目中不存在的对象作为部署脚本的一部分删除。 您可以排除项目中的一些文件以将它们暂时从生成脚本中移除。 但是，您可能希望在目标数据库中保留这些对象的现有版本。 如果选中“始终重新创建数据库”复选框，该复选框将没有任何作用，因为数据库会被删除。|  
|不使用 ALTER ASSEMBLY 语句更新 CLR 类型|否|指定在部署更改时，是否使用 ALTER ASSEMBLY 语句来更新公共语言运行时 (CLR) 类型，或者是否将删除并重新创建实例化 CLR 类型的对象。|  
|高级...|否|一种命令按钮，可用于指定控制部署的事件和行为的选项。|  
  
## <a name="reference-paths"></a><a name="bkmk_ref_paths"></a>引用路径  
使用此页，可以定义与跨数据库引用相关联的服务器变量和数据库变量。 此外，你可以指定这些变量的值。 有关详细信息，请参阅 [在数据库项目中使用引用](/previous-versions/visualstudio/visual-studio-2010/bb386242(v=vs.100))。  
  
## <a name="code-analysis"></a><a name="bkmk_code_analysis"></a>代码分析  
可以使用代码分析发现您的脚本中的潜在问题，例如设计、命名和性能问题。 数据库项目的规则组织成针对特定领域的预定义的规则集，并且您可以在 **“项目属性”** 属性页的 **“代码分析”** 选项卡中启用或禁用任何规则。 在同一个选项卡上，您可以指定代码分析以便在每次生成项目时自动运行，或者指定是否将警告视为错误。  
  
若要手动使用代码分析，请在“解决方案资源管理器”中右键单击你的项目，然后选择“运行代码分析”。 代码分析警告在 **“错误列表”** 窗口中列出。 可以双击某一警告以便导航到包含该问题的源代码，并且可以通过使用“显示错误帮助”上下文菜单查看警告的附加信息和可能的更正措施。 有关代码分析的详细信息，请参阅[分析数据库代码以提高代码质量](/previous-versions/visualstudio/visual-studio-2010/dd172133(v=vs.100))。  
