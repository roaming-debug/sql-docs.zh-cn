---
title: SSMS 查询编辑器
description: SQL Server Management Studio (SSMS) 查询编辑器
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.query.advanced.f1
- sql13.swb.query.ansi.f1
- sql13.swb.query.general.f1
- sql13.swb.query.grid.f1
- sql13.swb.sqleditors.multiserverresultssettings
- sql13.swb.tsqlquery.f1
- sql13.swb.tsqlresults.f1
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], editor
- SQL Server Management Studio [SQL Server], Database Engine Query Editor
- SQL Server Management Studio [SQL Server], templates
- SQL Server Management Studio [SQL Server], query editor
- Query Editor [SQL Server Management Studio]
- Query Editor [Database Engine]
- Query Editor [Database Engine], Features
- Query Editor [Database Engine], Toolbar
- Query Editor [SQL Server Management Studio], full screen mode
- Query Editor [Database Engine], templates
- Query Editor [SQL Server Management Studio], about Query Editor
- Transact-SQL Editor See Query Editor [Database Engine]
- Database Engine Query Editor See Query Editor [Database Engine]
- Code Editor [SQL Server Management Studio], about Query Editor
- editors [SQL Server Management Studio], Database Engine Query Editor
- full screen mode [SQL Server Management Studio]
- writing scripts
- modifying scripts
- writing queries
- scripts [SQL Server], SQL Server Management Studio
- queries [SQL Server], SQL Server Management Studio
ms.assetid: 05cfae9b-96d5-4a35-a098-0bc3a548bcfc
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019, contperfq1
ms.date: 08/28/2020
ms.openlocfilehash: 7450a77549d05dab5a024b39be6d2b4aef6c09de
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364827"
---
# <a name="sql-server-management-studio-ssms-query-editor"></a>SQL Server Management Studio (SSMS) 查询编辑器

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

本文介绍 SQL Server Management Studio (SSMS) 查询编辑器的特性和功能。

> [!Note]
> 如果要了解如何使用 Transact-SQL (T-SQL) F1 帮助，请查看 [Transact-SQL F1 帮助](#transact-sql-f1-help)部分。
>
> 如果要了解可以通过编辑器执行的任务，请访问[编辑器任务](#editor-tasks)部分。

SSMS 中的编辑器共享典型体系结构。 文本编辑器可实现基本功能，而且可用作文本文件的基本编辑器。 其他编辑器（或查询编辑器）可通过加入语言服务（用于定义 SQL Server 支持的其中一种语言的语法），对此基本功能进行扩展。 查询编辑器还可以对编辑器功能（如 IntelliSense 和调试）实现不同级别的支持。 查询编辑器包括用于生成包含 T-SQL 和 XQuery 语句的脚本的数据库引擎查询编辑器，用于 MDX 语言的 MDX 编辑器，用于 DMX 语言的 DMX 编辑器和用于 XML for Analysis 语言的 XML/A 编辑器。
可以使用查询编辑器创建和运行包含 Transact-SQL 语句的脚本。

![新建查询](media/database-engine-query-editor-sql-server-management-studio/new-query.png)

## <a name="sql-editor-toolbar"></a>SQL 编辑器工具栏

查询编辑器打开时，SQL 编辑器工具栏上显示以下按钮。

通过依次选择 **“视图”** 菜单、 **“工具栏”** 和 **“SQL 编辑器”** ，还可添加 SQL 编辑器工具栏。 如果在没有打开任何查询编辑器窗口时添加 SQL 编辑器工具栏，则所有按钮都不可用。

![编辑器工具栏](media/database-engine-query-editor-sql-server-management-studio/editor-toolbar.png)

### <a name="connect-using-the-editor-toolbar"></a>使用编辑器工具栏连接

打开[“连接到服务器”](connect-to-server-database-engine.md)  对话框。 此对话框用于建立与服务器的连接。

还可以使用[上下文菜单](#connection-using-the-context-menu)连接到数据库。

### <a name="change-connection-using-the-editor-toolbar"></a>使用编辑器工具栏更改连接

打开“连接到服务器”  对话框。 使用对话框建立[与另一个服务器的连接](f1-help-for-server-connections-sql-server-management-studio.md)。

还可以使用[上下文菜单](#connection-using-the-context-menu)更改连接。

### <a name="available-databases-using-the-editor-toolbar"></a>使用编辑器工具栏的可用数据库

将连接更改到同一服务器上的其他数据库。

### <a name="execute-using-the-editor-toolbar"></a>使用编辑器工具栏执行

执行所选的代码，如果没有选择任何代码，则执行所有查询编辑器代码。

还可以通过选中 F5 或从[上下文菜单](#execute-using-the-context-menu)“执行”  查询。

### <a name="cancel-executing-query-using-the-editor-toolbar"></a>使用编辑器工具栏取消执行查询

向服务器发送取消请求。 有些查询不能立即取消，而必须等待适当的取消条件。 取消事务时，在回滚事务期间可能发生延迟。

还可以通过选中 Alt + Break 来取消正在执行的查询。

### <a name="parse-using-the-editor-toolbar"></a>使用编辑器工具栏进行分析

检查所选代码的语法。 如果没有选择任何代码，则检查“查询编辑器”窗口中全部代码的语法。

还可以通过选中 Ctrl + F5 在查询编辑器中检查代码。

### <a name="display-estimated-execution-plan-using-the-editor-toolbar"></a>使用编辑器工具栏显示估计的执行计划

从查询处理器中请求查询执行计划而不执行查询，并在“执行计划”窗口中显示该计划。 此计划使用索引统计信息来估计查询执行的各个部分预期返回的行数。 实际使用的查询计划可能与估计的执行计划不同。 如果返回的行数与估计值有差距，并且查询处理器更改了执行计划以提高其效率，就会发生这种情况。

还可以通过选中 Ctrl + L 或从[上下文菜单](#display-estimated-execution-plan-using-the-context-menu)显示估计的执行计划。

### <a name="query-options-using-the-editor-toolbar"></a>使用编辑器工具栏的“查询选项”

打开“查询选项”  对话框。 此对话框用于配置查询执行和查询结果的默认选项。

还可以从[上下文菜单](#query-options-using-the-context-menu)中选择“查询选项”  。

### <a name="intellisense-enabled-using-the-editor-toolbar"></a>使用编辑器工具栏启用 IntelliSense

指定 [IntelliSense](../scripting/configure-intellisense-sql-server-management-studio.md) 功能在数据库引擎查询编辑器中是否可用。 默认情况下设置此选项。

还可以通过选中 Ctrl + B 然后按 Ctrl I，或从[上下文菜单](#intellisense-enabled-using-the-context-menu)中选择“IntelliSense 已启用”  。

### <a name="include-actual-execution-plan-using-the-editor-toolbar"></a>使用编辑器工具栏包括实际执行计划

执行查询，返回查询结果和使用查询的执行计划。 这些查询在“执行计划”窗口中显示为图形查询计划。

还可以通过选中 Ctrl + M 或从[上下文菜单](#include-actual-execution-plan-using-the-context-menu)选择“包括实际执行计划”  。

### <a name="include-live-query-statistics-using-the-editor-toolbar"></a>使用编辑器工具栏包括实时查询统计信息

作为控制流，能够实时了解从一个查询计划操作员到另一个操作员的查询执行过程。

还可以从[上下文菜单](#include-live-query-statistics-using-the-context-menu)选择“包括实时查询统计信息”  。

### <a name="include-client-statistics-using-the-editor-toolbar"></a>使用编辑器工具栏包括客户端统计信息

提供一个“客户端统计信息”  窗口，其中包含有关查询、网络数据包以及查询占用时间的统计信息。

还可以通过选中 Shift + Alt + S 或从[上下文菜单](#include-client-statistics-using-the-context-menu)选择“包括实时查询统计信息”  。

### <a name="results-to-text-using-the-editor-toolbar"></a>使用编辑器工具栏将结果显示为文本

在“结果”  窗口中以文本格式返回查询结果。

还可以通过选中 Ctrl + T 或从[上下文菜单](#results-using-the-context-menu)返回将结果显示为文本。

### <a name="results-to-grid-using-the-editor-toolbar"></a>使用编辑器工具栏将结果显示为网格

在“结果”  窗口中以一个或多个网格的形式返回查询结果。 默认情况下该选项处于启用状态。

还可以通过选中 Ctrl + D 或从[上下文菜单](#results-using-the-context-menu)返回将结果显示为文本。

### <a name="results-to-file-using-the-editor-toolbar"></a>使用编辑器工具栏将结果显示为文件

在执行查询时，“保存结果”  对话框将会打开。 在 **“保存于”** 中，选择要将文件保存到的文件夹。 在“文件名”中键入文件的名称，然后选择“保存”将查询结果保存为具有 .rpt 扩展名的“报表”文件。 对于高级选项，请选择“保存”按钮上的向下箭头，再选择“通过编码保存”。

还可以通过选中 Ctrl + Shift + F 或从[上下文菜单](#results-using-the-context-menu)返回将结果显示为文本。

### <a name="comment-out-the-selected-lines-using-the-editor-toolbar"></a>使用编辑器工具栏注释掉所选行

在当前行的开头处添加一个注释运算符 (--)，以对该行进行注释。

还可以通过选中 Ctrl + K 然后选择 Ctrl + C 注释掉一行。

### <a name="uncomment-the-selected-lines-using-the-editor-toolbar"></a>使用编辑器工具栏取消注释所选行

删除当前行开头处的任何注释运算符 (--)，以使该行成为一个活动的源语句。

还可以通过选择 Ctrl + K 然后选择 Ctrl + U 来取消对某行的注释。

### <a name="decrease-indent-using-the-editor-toolbar"></a>使用编辑器工具栏减少缩进

删除行开头处的空格，从而使该行文本向左移动。

### <a name="increase-line-indent-using-the-editor-toolbar"></a>使用编辑器工具栏增加行缩进

在行开头处插入空格，从而使该行文本向右移动。

### <a name="specify-values-for-template-parameters-using-the-editor-toolbar"></a>使用编辑器工具栏指定模板参数的值

打开一个对话框，在此对话框中可以指定存储过程或函数中参数的值。

## <a name="context-menu"></a>上下文菜单

可以通过在查询编辑器的任意位置右键单击来访问上下文菜单  。 上下文菜单中的选项类似于 SQL 编辑器工具栏。 在上下文菜单中，可以看到诸如“连接”和“执行”的相同选项，但也可以看到列出的其他选项，例如“插入代码片段”和“外侧代码”。

![选项](media/database-engine-query-editor-sql-server-management-studio/context-menu.png)

### <a name="insert-snippet-using-the-context-menu"></a>使用上下文菜单插入代码片段

[T-SQL 代码片段](../scripting/add-transact-sql-snippets.md)是一个模板，可将其作为在查询编辑器中编写新 Transact-SQL 语句的起点。

### <a name="surround-with-using-the-context-menu"></a>使用上下文菜单的外侧代码

外侧代码片段是一个模板，可将其作为在 BEGIN、IF 或 WHILE 块中插入一组 Transact-SQL 语句的起点。

### <a name="connection-using-the-context-menu"></a>使用上下文菜单的“连接”

![连接](media/database-engine-query-editor-sql-server-management-studio/context-menu-connections.png)

与 SSMS 中的工具栏选项相比，上下文菜单中的“连接”  选项更多。

- **连接** - 打开“连接到服务器”对话框。 此对话框用于建立与服务器的连接。

- **断开连接** - 断开当前查询编辑器与服务器的连接。

- **断开所有查询** - 断开所有查询连接。

- **更改连接** - 打开“连接到服务器”对话框。 此对话框用于建立与另一个服务器的连接。

### <a name="open-server-in-object-explorer-using-the-context-menu"></a>使用上下文菜单在对象资源管理器中打开服务器

对象资源管理器提供一个层次结构用户界面，用于查看和管理每个 SQL Server 实例中的对象。 “对象资源管理器详细信息”窗格显示一个实例对象的表格视图以及用于搜索特定对象的功能。 对象资源管理器的功能根据服务器的类型稍有不同，但一般都包括用于数据库的开发功能和用于所有服务器类型的管理功能。

### <a name="execute-using-the-context-menu"></a>使用上下文菜单执行

执行所选的代码，如果没有选择任何代码，则执行查询编辑器中的全部代码。

### <a name="display-estimated-execution-plan-using-the-context-menu"></a>使用上下文菜单显示估计的执行计划

从查询处理器中请求查询执行计划而不实际执行查询，并在“执行计划”  窗口中显示该计划。 此计划使用索引统计信息来估计查询执行的各个部分预期返回的行数。 实际使用的查询计划可能与估计的执行计划不同。 如果返回的行数与估计值有差距，并且查询处理器更改了执行计划以提高其效率，就会发生这种情况

### <a name="intellisense-enabled-using-the-context-menu"></a>使用上下文菜单启用 IntelliSense

指定 IntelliSense 功能在数据库引擎查询编辑器中是否可用。 默认情况下设置此选项。

### <a name="trace-query-in-sql-server-profiler-using-the-context-menu"></a>使用上下文菜单在 SQL Server Profiler 中进行跟踪查询

SQL Server Profiler 是一个界面，用于创建和管理跟踪并分析和重播跟踪结果。 这些事件保存在一个跟踪文件中，稍后试图诊断问题时，可以对该文件进行分析或用它来重播一系列特定的步骤。

### <a name="analyze-query-in-database-engine-tuning-advisor-using-the-context-menu"></a>使用上下文菜单在数据库引擎优化顾问中分析查询

Microsoft 数据库引擎优化顾问 (DTA) 分析数据库并对优化查询性能提出建议。 借助数据库引擎优化顾问，你不必精通数据库结构或深谙 SQL Server，即可选择和创建索引、索引视图和分区的最佳集合。 使用 DTA，您可以执行以下任务。

### <a name="design-query-in-editor-using-the-context-menu"></a>使用上下文菜单在编辑器中设计查询

当您打开视图的定义、显示查询或视图的结果或者创建或打开查询时，查询和视图设计器将会打开。

### <a name="include-actual-execution-plan-using-the-context-menu"></a>使用上下文菜单包括实际执行计划

执行查询，返回查询结果和使用查询的执行计划。 这些查询在“执行计划”窗口中显示为图形查询计划。

### <a name="include-live-query-statistics-using-the-context-menu"></a>使用上下文菜单包括实时查询统计信息

作为控制流，能够实时了解从一个查询计划操作员到另一个操作员的查询执行过程。

### <a name="include-client-statistics-using-the-context-menu"></a>使用上下文菜单包括客户端统计信息

提供一个“客户端统计信息”  窗口，其中包含有关查询、网络数据包以及查询占用时间的统计信息。

### <a name="results-using-the-context-menu"></a>使用上下文菜单的“结果”

![“结果”选项](media/database-engine-query-editor-sql-server-management-studio/context-menu-results.png)

可以从上下文菜单中选择所需的任一“结果”  选项。

- **结果显示为文本** - 在“结果”  窗口中将查询结果返回为文本。

- **结果显示为网格** - 在“结果”  窗口中将查询结果返回为一个或多个网格。

- **结果显示为文件** - 在执行查询时，  “保存结果”对话框将会打开。 在 **“保存于”** 中，选择要将文件保存到的文件夹。 在“文件名”中键入文件的名称，然后选择“保存”将查询结果保存为具有 .rpt 扩展名的“报表”文件。 对于高级选项，请选择“保存”按钮上的向下箭头，再选择“通过编码保存”。

### <a name="properties-window-using-the-context-menu"></a>使用上下文菜单的“属性”窗口

[“属性”窗口](../menu-help/properties-window-f1-help-management-studio.md)说明 SQL Server Management Studio 中项（如连接或 Showplan 运算符）的状态，以及有关数据库对象（如表、视图和设计器等）的信息。

可以使用“属性”窗口查看当前连接的属性。 许多属性在“属性”窗口中是只读的，但可以在 Management Studio 的其他地方更改。 例如，查询的数据库属性在“属性”窗口中是只读的，但可在工具栏上更改。

### <a name="query-options-using-the-context-menu"></a>使用上下文菜单的“查询选项”

打开“查询选项”  对话框。 此对话框用于配置查询执行和查询结果的默认选项。

## <a name="transact-sql-f1-help"></a>Transact-SQL F1 帮助

查询编辑器支持在你选中 F1 时将你链接到特定 Transact-SQL 语句的参考主题。 为此，突出显示 Transact-SQL 语句的名称，然后选择 F1。 接着，帮助搜索引擎将搜索具有与突出显示的字符串匹配的 F1 帮助属性的主题。

如果帮助搜索引擎找不到具有与突出显示的字符串完全匹配的 F1 帮助关键字的主题，则无法显示该主题。 在这种情况下，有两种方法可以找到所需的帮助：

- 将您突出显示的编辑器字符串复制并粘贴到 SQL Server 联机丛书的搜索选项卡中，并执行搜索。

- 仅突出显示 Transact-SQL 语句中可能与应用于主题的 F 帮助关键字匹配的部分，然后再次选择 F1。 搜索引擎要求突出显示的字符串与分配给主题的 F1 帮助关键字之间完全匹配。 如果突出显示的字符串包含对于您的环境是唯一的元素（如列或参数名称），则搜索引擎不会获得匹配项。 要突出显示的字符串的示例包括：

  - Transact-SQL 语句的名称，如 SELECT、CREATE DATABASE 或 BEGIN TRANSACTION。

  - 内置函数的名称，如 SERVERPROPERTY 或 @@VERSION。

  - 系统存储过程表或视图的名称，如 sys.data_spaces 或 sp_tableoption。

## <a name="editor-tasks"></a>编辑器任务

| 任务说明 | 主题 |
|------------------|-------|
| 介绍可以在 SSMS 中打开编辑器的各种方法。| [打开编辑器](../scripting/open-an-editor-sql-server-management-studio.md) |
| 配置各种编辑器的选项，如行编号和 IntelliSense 选项。 | [配置编辑器](../scripting/configure-editors-sql-server-management-studio.md) |
| 如何管理视图模式，如自动换行功能、拆分窗口或选项卡。| [管理编辑器和视图模式](../scripting/manage-the-editor-and-view-mode.md) |
| 设置格式设置选项，如隐藏文本或缩进。 | [管理代码格式](../scripting/manage-code-formatting.md) |
| 通过如“渐进式搜索”或“转至”功能在编辑器窗口中导航文本内容。 | [代码和文本定位](../scripting/navigate-code-and-text.md) |
| 设置各类语法的颜色编码选项，以便更容易读取复杂语句。 | [查询编辑器中的颜色编码](../scripting/color-coding-in-query-editors.md) |
| 将文本从脚本的一个位置中拖出，然后放入一个新位置。| [拖放文本](../scripting/drag-and-drop-text.md) |
| 如何设置书签，以便更容易地查找重要代码片段。 | [管理书签](../scripting/manage-bookmarks.md) |
| 如何打印窗口或网格中的脚本或结果。| [打印代码和结果](../scripting/print-code-and-results.md) |
| 查看和使用 MDX 查询编辑器的基本功能。 | [创建 Analysis Services 脚本](/analysis-services/instances/create-analysis-services-scripts-in-management-studio) |
| 查看和使用 DMX 查询编辑器的基本功能。 | [创建 DMX 查询](/analysis-services/data-mining/create-a-dmx-query-in-sql-server-management-studio) |
| 查看和使用 XML/A 编辑器的基本功能。 | [XML 编辑器](../scripting/xml-editor-sql-server-management-studio.md) |
| 如何使用数据库引擎查询编辑器中的 sqlcmd 功能。| [编辑 SQLCMD 脚本](../scripting/edit-sqlcmd-scripts-with-query-editor.md) |
| 如何使用数据库引擎查询编辑器中的代码段。 代码段是常用语句或语句块的模板，可以自定义或扩展以包含特定站点代码段。| [T-SQL 代码片段](../scripting/add-transact-sql-snippets.md) |
| 如何使用 Transact\-SQL 调试器逐句运行代码，并查看诸如变量和参数中的值之类的调试信息。| [T-SQL 调试程序](../scripting/transact-sql-debugger.md) |

## <a name="see-also"></a>另请参阅

- [自定义菜单和快捷键](../customize-menus-and-shortcut-keys.md)
- [SQL Server Management Studio 键盘快捷键](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)