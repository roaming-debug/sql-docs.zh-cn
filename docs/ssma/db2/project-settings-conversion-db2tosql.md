---
description: " (转换的项目设置)  (DB2ToSQL) "
title: " (转换的项目设置)  (DB2ToSQL) |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 538c93cf-c5bb-43d5-b758-186d9fb00c19
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2f39965e87911f7fef8d50c0b0888d552a451a90
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172649"
---
# <a name="project-settings-conversion-db2tosql"></a> (转换的项目设置)  (DB2ToSQL) 
" **项目设置** " 对话框的 "转换" 页包含用于自定义 SSMA 将 DB2 语法转换为语法的方式的设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
" **项目设置** " 和 " **默认项目设置** " 对话框中提供了 "转换" 窗格：  
  
-   若要指定所有 SSMA 项目的设置，请单击 " **工具** " 菜单上的 " **默认项目设置**"，从 " **迁移目标版本** " 下拉框中选择需要查看或更改其设置的 "迁移项目类型"，然后单击左窗格底部的 " **常规** "，然后单击 " **转换**"。  
  
-   若要指定当前项目的设置，请在 " **工具** " 菜单上单击 " **项目设置**"，然后单击左窗格底部的 " **常规** "，然后单击 " **转换**"。  
  
## <a name="conversion-messages"></a>转换消息  
  
### <a name="generate-messages-about-issues-applied"></a>生成有关应用的问题的消息  
指定 SSMA 是否在转换过程中生成信息性消息，在 "输出" 窗格中显示它们，并将它们添加到转换后的代码中。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观模式：** 不  
  
**完整模式：** 不  
  
## <a name="miscellaneous-options"></a>“杂项”选项  
  
### <a name="cast-rownum-expressions-as-integers"></a>将 ROWNUM 表达式强制转换为整数  
当 SSMA 转换 ROWNUM 表达式时，它会将该表达式转换为 TOP 子句，后跟表达式。 以下示例显示了 DB2 DELETE 语句中的 ROWNUM：  
  
`DELETE FROM Table1`  
  
`WHERE ROWNUM < expression and Field1 >= 2`  
  
下面的示例显示生成的 [!INCLUDE[tsql](../../includes/tsql-md.md)] ：  
  
`DELETE TOP (expression-1)`  
  
`FROM Table1`  
  
`WHERE Field1>=2`  
  
TOP 要求 TOP 子句表达式的计算结果为一个整数。 如果整数为负数，则该语句将产生错误。  
  
-   如果选择 **"是"**，SSMA 会将表达式强制转换为整数。  
  
-   如果选择 " **否**"，则 SSMA 会将所有非整数表达式标记为转换后的代码中的错误。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/完整模式：** 不  
  
**乐观模式：** 是的  
  
### <a name="default-schema-mapping"></a>默认架构映射  
此设置指定 DB2 架构映射到 SQL Server 架构的方式。 此设置提供了两个选项：  
  
1.  **数据库架构：** 在此模式下，DB2 架构 "sch1" 默认映射到 SQL Server 数据库 "sch1" 中 SQL Server 架构的 "dbo"。  
  
2.  架构 **到架构：** 在此模式下，DB2 架构 "sch1" 默认映射到 "sch1" SQL Server "连接" 对话框中提供的默认 SQL Server 数据库中的 ""。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 架构到数据库  
  
### <a name="conversion-ways-of-merge-statement"></a>MERGE 语句的转换方法  
  
-   如果选择 " **使用 INSERT、update、delete 语句**"，SSMA 会将合并语句转换为 INSERT、UPDATE、delete 语句。  
  
-   如果选择 **使用 MERGE 语句**，SSMA 会将合并语句转换为中的 merge 语句 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
> [!WARNING]  
> 此项目设置选项仅在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012、2014中可用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 使用 MERGE 语句  
  
### <a name="convert-calls-to-subprograms-that-use-default-arguments"></a>将调用转换为使用默认参数的 subprograms  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 函数不支持省略函数调用中的参数。 另外， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 函数和过程不支持表达式作为默认参数值。  
  
-   如果选择 **"是"** ，并且函数调用省略了参数，则 SSMA 会将关键字 **默认值** 插入函数并调用正确的位置。 然后，它将使用警告标记调用。  
  
-   如果选择 " **否**"，SSMA 会将函数调用标记为错误。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 是的  
  
### <a name="convert-count-function-to-count_big"></a>将 COUNT 函数转换为 COUNT_BIG  
如果计数函数可能返回大于2147483647的值（即 2<sup>31</sup>-1），则应将函数转换为 COUNT_BIG。  
  
-   如果选择 **"是"**，SSMA 会将计数的所有使用转换为 COUNT_BIG。  
  
-   如果选择 " **否**"，则函数将保留为计数。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如果函数返回一个大于 2<sup>31</sup>-1 的值，则将返回错误。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/完整模式：** 是的  
  
**乐观模式：** 不  
  
### <a name="convert-forall-statement-to-while-statement"></a>将 FORALL 语句转换为 WHILE 语句  
定义 SSMA 将如何处理 PL/SQL 集合元素上的 FORALL 循环。  
  
-   如果选择 **"是"**，则 SSMA 将创建一个 WHILE 循环，其中集合元素逐个检索。  
  
-   如果选择 " **否**"，则 SSMA 将使用节点 ( ) 方法从集合中生成行集，并将其用作单个表。 这会更有效，但会使输出代码更易读。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观模式：** 不  
  
**完整模式：** 是的  
  
### <a name="convert-foreign-keys-with-set-null-referential-action-on-column-that-is-not-null"></a>将外键转换为不为 NULL 的列上的 SET NULL 引用操作  
DB2 允许创建外键约束，因为在被引用列中不允许 null 值，所以无法执行 SET NULL 操作。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不允许进行这样的外键配置。  
  
-   如果选择 **"是"**，则 SSMA 将生成 DB2 中的引用操作，但需要在将约束加载到之前进行手动更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 例如，可以选择 "无操作" 而不是 "设置为 NULL"。  
  
-   如果选择 " **否**"，则约束将被标记为错误。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 不  
  
### <a name="convert-function-calls-to-procedure-calls"></a>将函数调用转换为过程调用  
某些 DB2 函数被定义为自治事务或包含在中无效的语句 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 在这些情况下，SSMA 会创建一个过程和一个函数，该函数是过程的包装器。 转换后的函数会调用实现过程。  
  
SSMA 可以将对包装函数的调用转换为对过程的调用。 这会创建更易于阅读的代码，并可提高性能。 但是，上下文并不总是允许它;例如，不能将 SELECT 列表中的函数调用替换为过程调用。 SSMA 有几个选项可用于涵盖常见案例：  
  
-   如果选择 " **始终**"，则 SSMA 会尝试将包装函数调用转换为过程调用。 如果当前上下文不允许此转换，则生成错误消息。 这样就不会在生成的代码中留下任何函数调用。  
  
-   如果尽可能选择，SSMA 仅在函数具有输出参数 **时** 才会移动到过程调用。 如果无法移动，则会删除参数的 output 属性。 在所有其他情况下，SSMA 将保留函数调用。  
  
-   如果选择 " **从不**"，则 SSMA 会将所有函数调用作为函数调用。 有时，由于性能方面的原因，此选择可能无法接受。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 尽可能  
  
### <a name="convert-lock-table-statements"></a>Convert LOCK TABLE 语句  
SSMA 可以将许多 LOCK TABLE 语句转换为表提示。 SSMA 无法转换包含 PARTITION、SUBPARTITION、和 NOWAIT 子句的任何 LOCK TABLE 语句 @dblink ，并将此类语句与转换错误消息一起标记。  
  
-   如果选择 **"是"**，SSMA 会将支持的锁表语句转换为表提示。  
  
-   如果选择 " **否**"，则 SSMA 会将所有 LOCK TABLE 语句标记为转换错误消息。  
  
下表显示了 SSMA 如何转换 DB2 锁定模式：  
  
|DB2 锁定模式|SQL Server 表提示|  
|-|-|  
|行共享|ROWLOCK、HOLDLOCK|  
|行独占|ROWLOCK、XLOCK、HOLDLOCK|  
|共享更新 = 行共享|ROWLOCK、HOLDLOCK|  
|共享|TABLOCK、HOLDLOCK|  
|专用行共享|TABLOCK、XLOCK、HOLDLOCK|  
|异|TABLOCKX、HOLDLOCK|  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 是的  
  
### <a name="convert-open-for-statements-for-ref-cursor-out-parameters"></a>为 REF CURSOR OUT 参数转换开放式语句  
在 DB2 中，可以使用开放式语句将结果集返回到类型为 REF CURSOR 的 subprogram 的 OUT 参数。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，存储过程直接返回 SELECT 语句的结果。  
  
SSMA 可以将多个打开的语句转换为 SELECT 语句。  
  
-   如果选择 **"是"**，SSMA 会将开放的语句转换为 select 语句，该语句将结果集返回到客户端。  
  
-   如果选择 " **否**"，则 SSMA 会在转换后的代码和输出窗格中生成错误消息。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 是的  
  
### <a name="convert-record-as-a-list-of-separates-variables"></a>将记录转换为分隔变量列表  
SSMA 可以将 DB2 记录转换为分隔变量，并将其转换为具有特定结构的 XML 变量。  
  
-   如果选择 **"是"**，则在可能的情况下，SSMA 会将记录转换为分隔变量的列表。  
  
-   如果选择 " **否**"，SSMA 会将记录转换为具有特定结构的 XML 变量。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 是的  
  
### <a name="convert-substr-function-calls-to-substring-function-calls"></a>将 SUBSTR 函数调用转换为子字符串函数调用  
SSMA 可以将 DB2 SUBSTR 函数调用转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **子字符串** 函数调用，具体取决于参数的数目。 如果 SSMA 无法转换 SUBSTR 函数调用，或参数的数目不受支持，则 SSMA 会将 SUBSTR 函数调用转换为自定义 SSMA 函数调用。  
  
-   如果选择 **"是"**，SSMA 会将使用三个参数的 SUBSTR 函数调用转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **子字符串**。 其他 SUBSTR 函数将被转换为调用自定义 SSMA 函数。  
  
-   如果选择 " **否**"，SSMA 会将 SUBSTR 函数调用转换为自定义 SSMA 函数调用。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观模式：** 是的  
  
**完整模式：** 不  
  
### <a name="convert-subtypes"></a>转换子类型  
SSMA 可以通过两种方式转换 PL/SQL 子类型：  
  
-   如果选择 **"是"**，则 SSMA 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从子类型创建用户定义类型，并将其用于此子类型的每个变量。  
  
-   如果选择 " **否**"，则 SSMA 将用基础类型替换子类型的所有源声明，并像平常一样转换结果。 在这种情况下，不会在中创建其他类型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 不  
  
### <a name="convert-synonyms"></a>转换同义词  
以下 DB2 对象的同义词可迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ：  
  
-   表和对象表  
  
-   视图和对象视图  
  
-   存储过程和函数  
  
-   具体化视图  
  
可以通过直接引用对象替换以下 DB2 对象的同义词：  
  
-   序列  
  
-   包  
  
-   Java 类架构对象  
  
-   用户定义的对象类型  
  
无法迁移其他同义词。 SSMA 将生成同义词的错误消息以及使用同义词的所有引用。  
  
-   如果选择 **"是"**，SSMA 会 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 根据前面的列表创建同义词和直接对象引用。  
  
-   如果选择 " **否**"，则 SSMA 将为此处列出的所有同义词创建直接对象引用。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 是的  
  
### <a name="convert-to_chardate-format"></a>转换 TO_CHAR (日期、格式)   
SSMA 可以将 DB2 TO_CHAR (日期、格式) 转换为 sysdb 数据库中的过程。  
  
-   如果选择 " **使用 TO_CHAR_DATE 函数**"，SSMA 会将 TO_CHAR (日期、格式) 转换为使用英语转换的 TO_CHAR_DATE 函数。  
  
-   如果选择 " **使用 TO_CHAR_DATE_LS 函数" (NLS 服务)**，则 SSMA 会将 TO_CHAR (DATE，格式) 转换为使用会话语言进行转换 TO_CHAR_DATE_LS 函数  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观模式：** 使用 TO_CHAR_DATE 函数  
  
**完整模式：** 使用 TO_CHAR_DATE_LS 函数 (NLS 护理)   
  
### <a name="convert-transaction-processing-statements"></a>转换事务处理语句  
SSMA 可以转换 DB2 事务处理语句：  
  
-   如果选择 **"是"**，SSMA 会将 DB2 事务处理语句转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句。  
  
-   如果选择 " **否**"，则 SSMA 会将事务处理语句标记为转换错误。  
  
> [!NOTE]  
> DB2 隐式打开事务。 若要在上模拟此行为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，你必须手动添加 BEGIN TRANSACTION 语句，你希望在何处开始事务。 或者，你可以在会话开始时对命令执行 SET IMPLICIT_TRANSACTIONS。 SSMA 在用自治事务转换子例程时，自动添加 SET IMPLICIT_TRANSACTIONS。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 是的  
  
### <a name="emulate-db2-null-behavior-in-order-by-clauses"></a>在 ORDER BY 子句中模拟 DB2 null 行为  
NULL 值在和 DB2 中的排序方式不同 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ：  
  
-   在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，NULL 值是排序列表中的最小值。 在升序列表中，将首先显示 NULL 值。  
  
-   在 DB2 中，NULL 值为排序列表中的最大值。 默认情况下，NULL 值显示在升序列表的最后。  
  
-   DB2 首先为 NULL，最后是最后一个子句，这使你可以更改 DB2 订单 NULLs 的方式。  
  
SSMA 可以通过检查是否有 NULL 值来模拟 DB2 顺序。 然后，它首先按指定顺序对 NULL 值进行排序，然后按其他值排序。  
  
-   如果选择 **"是"**，则 SSMA 将以模拟 DB2 顺序按行为的方式转换 db2 语句。  
  
-   如果选择 " **否**"，则 SSMA 将忽略 DB2 规则，并在遇到 null FIRST 和 null LAST 子句时生成错误消息。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观模式：** 不  
  
**完整模式：** 是的  
  
### <a name="emulate-row-count-exceptions-in-select"></a>在 SELECT 中模拟行计数异常  
如果带有 INTO 子句的 SELECT 语句未返回任何行，则 DB2 将引发 NO_DATA_FOUND 异常。 如果该语句返回两行或更多行，则会引发 TOO_MANY_ROWS 异常。 如果行计数与一个不同，则中的转换后的语句 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会引发任何异常。  
  
-   如果选择 **"是"**，则 SSMA 会在每个 select 语句后面添加对 sysdb 过程 db_error_exact_one_row_check 的调用。 此过程模拟 NO_DATA_FOUND 和 TOO_MANY_ROWS 异常。 这是默认设置，它允许尽可能关闭复制 DB2 行为。 如果源代码包含处理这些错误的异常处理程序，则应始终选择 **"是"** 。 请注意，如果 SELECT 语句发生在用户定义函数中，则此模块将转换为存储过程，因为执行存储过程和引发异常与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 函数上下文不兼容。  
  
-   如果选择 " **否**"，则不会生成任何异常。 当 SSMA 转换用户定义函数，并且你希望它保留在中时，这可能很有用。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 是的  
  
### <a name="generate-error-for-dbms_sqlparse"></a>为 DBMS_SQL 生成错误。分析  
  
-   如果选择 " **错误**"，则 SSMA 会在转换 DBMS_SQL 中生成错误。分析.  
  
-   如果选择 " **警告**"，SSMA 将在转换 DBMS_SQL 生成警告。分析.  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 条  
  
### <a name="generate-rowid-column"></a>生成 ROWID 列  
当 SSMA 在中创建表时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，可以创建 ROWID 列。 迁移数据时，每行都将获取 newid ( # A1 函数生成的新的唯一标识符值。  
  
-   如果选择 **"是**"，则将在所有表上创建 ROWID 列并在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 插入值时生成 guid。 如果计划使用 SSMA 测试人员，请始终选择 **"是"** 。  
  
-   如果选择 " **否**"，则不会将 ROWID 列添加到表中。  
  
-   对于包含触发器的表，为包含触发器的表添加 **rowid 列**。  
  
> [!WARNING]  
> SQL Server 2005 的情况下为默认设置，SQL Server 2008，SQL Server 2012 和2014为 **包含触发器的表添加 ROWID 列**。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观模式：** 为包含触发器的表添加 ROWID 列  
  
**完整模式：** 是的  
  
### <a name="generate-unique-index-on-rowid-column"></a>针对 ROWID 列生成唯一索引  
指定 SSMA 是否在 ROWID 生成的列上生成唯一索引列。 如果将此选项设置为 "是"，则将生成唯一索引，并且如果将其设置为 "否"，则不会对 ROWID 列生成唯一索引。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 是的  
  
### <a name="local-modules-conversion"></a>本地模块转换  
定义在独立存储过程或函数) 转换中声明的 DB2 嵌套 subprogram (的类型。  
  
-   如果选择 " **内联**"，则嵌套的 subprogram 调用将替换为其正文。  
  
-   如果选择 " **存储过程**"，嵌套的 subprogram 将转换为 SQL Server 存储过程，并且在此过程调用中将替换其调用。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 内  
  
### <a name="use-isnull-in-string-concatenation"></a>在字符串串联中使用 ISNULL  
DB2 并 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回不同的结果。 DB2 将空值视为空字符集。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回 NULL。  
  
-   如果选择 **"是"**，SSMA 将替换 DB2 串联字符 (| |与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 串联字符 (+) ) 。 SSMA 还会检查连接两侧的表达式是否为 NULL 值。  
  
-   如果选择 " **否**"，则 SSMA 将替换串联字符，但不会检查是否存在 NULL 值。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
### <a name="use-isnull-in-replace-function-calls"></a>在 REPLACE 函数调用中使用 ISNULL  
在 REPLACE 函数调用中使用 ISNULL 语句来模拟 DB2 行为。 此设置具有以下选项：  
  
-   YES  
  
-   是  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观模式：** 不  
  
**完整模式：** 是的  
  
### <a name="use-isnull-in-concat-function-calls"></a>在 CONCAT 函数调用中使用 ISNULL  
在 CONCAT 函数调用中使用 ISNULL 语句来模拟 DB2 行为。 此设置具有以下选项：  
  
-   YES  
  
-   是  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观模式：** 不  
  
**完整模式：** 是的  
  
### <a name="use-native-convert-function-when-possible"></a>尽可能使用本机转换函数  
  
-   如果选择 **"是"**，则在可能的情况下，SSMA 会将 TO_CHAR (日期、格式) 转换为本机转换函数。  
  
-   如果你选择 " **否**"，则 SSMA 会将 TO_CHAR (日期、格式) 转换为 TO_CHAR_DATE 或 TO_CHAR_DATE_LS， (它是通过 "转换 TO_CHAR (日期，格式) " 选项) 来定义的。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观模式：** 是的  
  
**完整模式：** 不  
  
### <a name="use-selectfor-xml-when-converting-selectinto-for-record-variable"></a>使用 SELECT .。。如果在转换 SELECT .。。INTO for record 变量  
指定在选择到记录变量时是否生成 XML 结果集。  
  
-   如果选择 **"是"**，则 select 语句返回 XML。  
  
-   如果选择 " **否**"，则 select 语句返回一个结果集。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 不  
  
## <a name="returning-clause-conversion"></a>返回子句转换  
  
### <a name="convert-returning-clause-in-delete-statement-to-output"></a>将 DELETE 语句中的返回子句转换为输出  
DB2 提供返回子句作为立即获取已删除值的方法。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过 OUTPUT 子句提供该功能。  
  
-   如果选择 **"是"**，SSMA 会将 DELETE 语句中的返回子句转换为 OUTPUT 子句。 由于表中的触发器可以更改值，因此返回的值可能不同于 DB2 中的值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
-   如果选择 " **否**"，则 SSMA 将在 DELETE 语句之前生成 select 语句以检索返回值。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 是的  
  
### <a name="convert-returning-clause-in-insert-statement-to-output"></a>将 INSERT 语句中的返回子句转换为输出  
DB2 提供返回子句作为立即获取插入值的方法。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过 OUTPUT 子句提供该功能。  
  
-   如果选择 **"是"**，SSMA 会将 INSERT 语句中的返回子句转换为输出。 由于表中的触发器可以更改值，因此返回的值可能不同于 DB2 中的值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
-   如果选择 " **否**"，SSMA 将通过从引用表中插入和选择值来模拟 DB2 功能。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 是的  
  
### <a name="convert-returning-clause-in-update-statement-to-output"></a>将 UPDATE 语句中的返回子句转换为输出  
DB2 提供返回子句作为立即获取更新值的方法。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过 OUTPUT 子句提供该功能。  
  
-   如果选择 **"是"**，SSMA 会将 UPDATE 语句中的返回子句转换为 OUTPUT 子句。 由于表中的触发器可以更改值，因此返回的值可能不同于 DB2 中的值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
-   如果选择 " **否**"，SSMA 将在 UPDATE 语句之后生成 select 语句，以检索返回值。  
  
在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：  
  
**默认/乐观/完整模式：** 是的  
  
## <a name="sequence-conversion"></a>序列转换  
  
### <a name="convert-sequence-generator"></a>转换序列生成器  
在 DB2 中，可以使用序列生成唯一标识符。  
  
SSMA 可以将序列转换为以下项。  
  
-   仅当转换为 SQL Server 2012 和 SQL Server 2014) 时，才可以使用 SQL Server 序列生成器 (此选项。  
  
-   使用 SSMA 序列生成器。  
  
-   使用列标识。  
  
转换为 SQL Server 2012 或 SQL Server 2014 时的默认选项是使用 SQL Server 序列生成器。 但是，SQL Server 2012 和 SQL Server 2014 不支持获取当前序列值 (如 DB2 序列 currval 方法) 的值）。 有关迁移 DB2 sequence currval 方法的指导，请参阅 SSMA 团队博客网站。  
  
SSMA 还提供了一个用于将 DB2 序列转换为 SSMA 序列模拟器的选项。 当你转换到2012之前的 SQL Server 时，这是默认选项  
  
最后，还可以将分配给表中的列的序列转换为 SQL Server 标识值。 必须在 "DB2 **表** " 选项卡上指定序列与标识列之间的映射  
  
### <a name="convert-currval-outside-triggers"></a>在触发器外转换 CURRVAL  
仅当转换序列生成器设置为 **使用列标识** 时可见。 由于 DB2 序列是与表分离的对象，因此许多使用序列的表都使用触发器来生成和插入新的序列值。 SSMA 注释掉这些语句，或在注释掉会生成错误时将其标记为错误。  
  
-   如果选择 **"是"**，SSMA 将在转换后的序列 CURRVAL 上将所有引用标记为外部触发器，并发出警告。  
  
-   如果选择 " **否**"，则 SSMA 会将转换后的序列 CURRVAL 上对外部触发器的所有引用标记为错误。  
  
## <a name="see-also"></a>另请参阅  
[用户界面参考 &#40;DB2ToSQL&#41;](../../ssma/db2/user-interface-reference-db2tosql.md)  
  
