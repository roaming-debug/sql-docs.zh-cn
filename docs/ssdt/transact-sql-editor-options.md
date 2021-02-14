---
title: Transact-SQL 编辑器选项
description: 熟悉 Transact-SQL 编辑器选项。 了解查询执行属性和查询结果属性，并查看如何调整值。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.SqlExecutionAdvancedSettingsOption
- sql.data.tools.SqlExecutionAnsiSettingsDlg
- sql.data.tools.SqlResultToTextSettingsDlg
- sql.data.tools.SqlExecutionGeneralSettingsDlg
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_RESULTS.GENERAL
- sql.data.tools.unittesting.tsqleditor
- sql.data.tools.SqlResultsToGridSettingsDlg
- sql.data.tools.queryresults.executequeryfailure
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.GENERAL
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.EDITOR_TAB_AND_STATUS_BAR
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.GENERAL
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_RESULTS.RESULTS_TO_TEXT
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_EXECUTION.ANSI
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_EXECUTION.GENERAL
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_RESULTS.RESULTS_TO_GRID
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.ONLINE_EDITING
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_EXECUTION.ADVANCED
ms.assetid: fa9a250f-7feb-433e-91bd-a09779d74c8b
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 28bb933ec92a6f96045a1f316c2757fbf3a9d564
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082430"
---
# <a name="transact-sql-editor-options"></a>Transact-SQL 编辑器选项

本主题包含有关 Transact-SQL 编辑器的一些选项的信息。 要设置这些选项，请通过“工具\选项”菜单导航到“选项”对话框。  
  
[查询执行](#QueryExecution)  
  
[查询结果](#QueryResults)  
  
## <a name="query-execution"></a><a name="QueryExecution"></a>查询执行  
  
|属性|描述|  
|------------|---------------|  
|**SET ROWCOUNT**|默认值为 0，指示 SQL Server 在收到所有结果之前将一直等待结果。 如果希望 SQL Server 在获取指定数目的行后暂停查询，请提供一个大于 0 的值。 若要关闭此选项（以便返回所有的行），请将 SET ROWCOUNT 指定为 0。|  
|**SET TEXTSIZE**|默认值为 2,147,483,647 个字节，表示 SQL Server 将针对 text、ntext、nvarchar(max) 和 varchar(max) 数据字段提供最高上限的数据。 它将不影响 XML 数据类型。 提供较小的数值，可以在存在大量值时限制结果数量。 超出指定数量的列将被截断。|  
|**执行超时值**|指示在取消查询之前等待的秒数。 值 0 指示无限期的等待或无超时。|  
|默认情况下，在 SQLCMD 模式下打开新查询|选中此复选框可在 SQLCMD 模式下打开新查询。 只有从“工具”菜单打开该对话框时，此复选框才可见。<br /><br />选择此选项时，请记住下列限制：<br /><br />-   数据库引擎查询编辑器中的 IntelliSense 处于关闭状态。<br />-   由于查询编辑器不能从命令行运行，因此不能传入命令行参数（如变量）。<br />-   由于查询编辑器无法响应操作系统提示，因此一定要记住不要运行交互式语句。|  
|**SET NOCOUNT**|阻止在结果中返回消息，该消息指示 Transact-SQL 语句影响的行数。 有关更多信息，请参见 [SET NOCOUNT](../t-sql/statements/set-nocount-transact-sql.md)。|  
|**SET NOEXEC**|为 ON 时，告知 Microsoft® SQL Server™ 编译每批 Transact-SQL 语句但是不执行它们。 为 OFF 时，告知 Microsoft® SQL Server™ 在编译后执行所有批。有关详细信息，请参阅 [SET NOEXEC](../t-sql/statements/set-noexec-transact-sql.md)。|  
|**SET PARSEONLY**|检查每个 Transact-SQL 语句的语法并返回任何错误消息，但不编译或执行语句。 有关更多信息，请参见 [SET PARSEONLY](../t-sql/statements/set-parseonly-transact-sql.md)。|  
|**SET CONCAT_NULL_YIELDS_NULL**|控制是将串联结果视为 null 值还是空字符串值。有关详细信息，请参阅 [SET CONCAT_NULL_YIELDS_NULL](../t-sql/statements/set-concat-null-yields-null-transact-sql.md)。|  
|**SET ARITHABORT**|在查询执行过程中发生溢出或被零除错误时终止查询。 有关详细信息，请参阅 [SET ARITHABORT](https://msdn.microsoft.com/library/aa259212(v=SQL.80).aspx)。|  
|**SET SHOWPLAN_TEXT**|使 Microsoft® SQL Server™ 不执行 Transact-SQL 语句， 而是由 SQL Server 返回有关如何执行语句的详细信息。 有关详细信息，请参阅 [SET SHOWPLAN_TEXT](../t-sql/statements/set-showplan-text-transact-sql.md)。|  
|**SET STATISTICS TIME**|显示分析、编译和执行各语句所需的毫秒数。|  
|**SET STATISTICS IO**|使 Microsoft® SQL Server™ 显示有关由 Transact-SQL 语句生成的磁盘活动量的信息。|  
|**SET TRANSACTION ISOLATION LEVEL**|控制一个连接所发出的所有 Microsoft® SQL Server™ **SELECT** 语句的默认事务锁定行为。 有关更多信息，请参见  [SET TRANSACTION ISOLATION LEVEL](../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。|  
|**SET LOCK_TIMEOUT**|指定语句等待锁释放的毫秒数。 有关详细信息，请参阅 [SET LOCK_TIMEOUT](../t-sql/statements/set-lock-timeout-transact-sql.md)|  
|**SET QUERY_GOVERNOR_COST_LIMIT**|覆盖当前为当前连接所配置的值。 有关详细信息，请参阅 [SET QUERY_GOVERNOR_COST_LIMIT](../t-sql/statements/set-query-governor-cost-limit-transact-sql.md)。|  
|**SET ANSI_DEFAULTS**|控制一组用来共同指定某些 SQL-92 标准行为的 Microsoft® SQL Server™ 设置。 有关详细信息，请参阅 [SET ANSI_DEFAULTS](../t-sql/statements/set-ansi-defaults-transact-sql.md)。|  
|**SET QUOTED_IDENTIFIER**|使 Microsoft® SQL Server™ 遵从关于引号分隔标识符和文字字符串的 SQL-92 规则。 由双引号分隔的标识符可以是 Transact-SQL 保留关键字，也可以包含 Transact-SQL 标识符语法规则通常不允许的字符。有关更多信息，请参阅 [SET QUOTED_IDENTIFIER](../t-sql/statements/set-quoted-identifier-transact-sql.md)。|  
|**SET ANSI_NULL_DFLT_ON**|数据库的 ANSI null default 选项为 false 时，更改会话行为以覆盖新列的默认为 null 性。 有关详细信息，请参阅 [SET ANSI_NULL_DFLT_ON](../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)。|  
|**SET IMPLICIT_TRANSACTIONS**|为 **ON** 时，将连接设置为隐式事务模式。 为 **OFF** 时，则使连接恢复为自动提交事务模式。 有关详细信息，请参阅 [SET IMPLICIT_TRANSACTIONS](../t-sql/statements/set-implicit-transactions-transact-sql.md)。|  
|**SET CURSOR_CLOSE_ON_COMMIT**|控制在提交事务时是否关闭游标。 有关详细信息，请参阅 [SET CURSOR_CLOSE_ON_COMMIT](../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)。|  
|**SET ANSI_PADDING**|对列存储值长度小于列的定义大小的值以及在 **char**、 **varchar**、 **binary** 和 **varbinary** 数据中含有尾随空格的列存储值的方式进行控制。 有关详细信息，请参阅 [SET ANSI_PADDING](../t-sql/statements/set-ansi-padding-transact-sql.md)。|  
|**SET ANSI_WARNINGS**|为多个错误条件指定 SQL-92 标准行为。有关详细信息，请参阅 [SET ANSI_WARNINGS](../t-sql/statements/set-ansi-warnings-transact-sql.md)。|  
|**SET ANSI_NULLS**|指定在与 null 值一起使用等于 (=) 和不等于 (<>) 比较运算符时采用符合 SQL-92 标准的行为。有关详细信息，请参阅 [SET ANSI_NULLS](../t-sql/statements/set-ansi-nulls-transact-sql.md)。|  
  
## <a name="query-results"></a><a name="QueryResults"></a>查询结果  
  
|属性|描述|  
|------------|---------------|  
|在结果集中包括查询|将查询文本作为结果集的一部分返回。|  
|复制或保存结果时包括列标题|将结果复制到剪贴板或保存到文件时，包括列标题。 如果希望保存或复制的结果数据只有数据而没有列标题，请清除此复选框。|  
|执行后放弃结果|当屏幕显示接收到查询结果之后，通过放弃查询结果来释放内存。|  
|在单独选项卡中显示结果|在新文档窗口中显示结果集，而不是在查询文档窗口的底部显示。|  
|执行查询后切换到“结果”选项卡|自动将屏幕焦点设置到结果集。|  
|检索的最多字符数|非 XML 数据：<br /><br />输入一个介于 1 到 65535 之间的数字以指定每个单元中显示的最大字符数。 **注意：** 指定大量字符可能会导致结果集中显示的数据截断。 每个单元中显示的最大字符数取决于字号。 在返回较大的结果集时，如果此框中的值太大，可能会导致 SQL Server Management Studio 运行时内存不足，从而影响系统性能。<br /><br />XML 数据：<br /><br />选择 1 MB、2 MB 或 5 MB。 选择“无限制”将检索所有字符。|  
|输出格式|默认情况下，将在通过用空格分隔结果而得到的列中显示输出。 您还可以使用逗号、制表符或空格来分隔列。 选中 **“自定义分隔符”** 复选框，可以在 **“自定义分隔符”** 框中指定其他分隔字符。|  
|自定义分隔符|自行指定用于分隔列的字符。 只有在 **“输出格式”** 框中选中 **“自定义分隔符”** 复选框时，才可使用此选项。|  
|在结果集中包括列标题|如果不希望每列都带有列标题，请清除此复选框：|  
|接收到结果时滚动|选中此复选框将使得结果集的显示侧重于结尾处最近返回的记录。 清除此复选框，则使其侧重于接收到的前几行。|  
|右对齐数值|选中此复选框可以将数值与列的右侧对齐。 此选项可以更方便地查看具有固定小数位数的数值。|  
|在执行查询后放弃结果|当屏幕显示接收到查询结果之后，通过放弃查询结果来释放内存。|  
|在单独选项卡中显示结果|选中此复选框可在新文档窗口中显示结果集，而不是在查询文档窗口的底部显示。|  
|执行查询后切换到“结果”选项卡|单击此项可将屏幕焦点自动设置到结果集。|  
|每列中显示的最大字符数|此值默认为 256。 增大此值可显示更大的结果集，而不会将其截断。|  
|重置为默认值|将此页上的所有值重置为原始默认值。|  
