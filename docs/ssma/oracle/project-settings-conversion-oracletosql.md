---
title: " (转换的项目设置)  (OracleToSQL) |Microsoft Docs"
description: 了解如何使用 "项目设置" 对话框的 "转换" 页自定义 SSMA 将 Oracle 语法转换为 SQL Server 语法的方式。
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: a98a5e07-eb5e-47b9-a6f2-e2cb3a18309c
ms.author: alexiva
ms.openlocfilehash: 828f206c7fc6b8d653e0b141b8041f10093cdf4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068002"
---
# <a name="project-settings-conversion-oracletosql"></a>项目设置（转换）(OracleToSQL)

"**项目设置**" 对话框的 "**转换**" 页包含用于自定义 SSMA 将 Oracle 语法转换为语法的方式的设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

"**项目设置**" 和 "**默认项目设置**" 对话框中提供了 "**转换**" 窗格：

- 若要指定所有 SSMA 项目的设置，请单击 " **工具** " 菜单上的 " **默认项目设置**"，从 " **迁移目标版本** " 下拉框中选择需要查看或更改其设置的 "迁移项目类型"，然后单击左窗格底部的 " **常规** "，然后单击 " **转换**"。

- 若要指定当前项目的设置，请在 " **工具** " 菜单上单击 " **项目设置**"，然后单击左窗格底部的 " **常规** "，然后单击 " **转换**"。

## <a name="built-in-functions-and-supplied-packages"></a>内置函数和提供的包

|术语|定义|
|-|-|
|**将 COUNT 函数转换为 COUNT_BIG**|如果 `COUNT` 函数可能返回大于2147483647的值（即 2<sup>31</sup>-1），则应将函数转换为 `COUNT_BIG` 。<br /><br />如果选择 **"是"**，SSMA 会将的所有使用转换 `COUNT` 为 `COUNT_BIG` 。<br /><br />如果选择 " **否**"，则函数将保持为 `COUNT` 。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如果函数返回一个大于 2<sup>31</sup>-1 的值，则将返回错误。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/完整模式：** 是的<br />**乐观模式：** 不|
|**将 SUBSTR 函数调用转换为子字符串函数调用**|SSMA 可以将 Oracle `SUBSTR` 函数调用转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `substring` 函数调用，具体取决于参数的数目。 如果 SSMA 无法转换 `SUBSTR` 函数调用，或参数的数目不受支持，则 SSMA 会将 `SUBSTR` 函数调用转换为自定义 SSMA 函数调用。<br /><br />如果选择 **"是"**，SSMA 会将 `SUBSTR` 使用三个参数的函数调用转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `substring` 。 `SUBSTR`将转换其他函数以调用自定义 SSMA 函数。<br /><br />如果选择 " **否**"，SSMA 会将 `SUBSTR` 函数调用转换为自定义 SSMA 函数调用。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观模式：** 是的<br />**完整模式：** 不|
|**转换 TO_CHAR (日期、格式) 函数调用**|SSMA 可以从架构将 Oracle 转换为 `TO_CHAR(date, format)` 过程 `ssma_oracle` 。<br /><br />如果选择 " **使用 TO_CHAR_DATE 函数**"，则 SSMA `TO_CHAR(date, format)` 将 `TO_CHAR_DATE` 使用英语转换为转换函数。<br /><br />如果选择 " **使用 TO_CHAR_DATE_LS 函数" (NLS 服务)**，则 SSMA `TO_CHAR(date, format)` 会 `TO_CHAR_DATE_LS` 使用会话语言转换成函数以进行转换<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观模式：** 使用 TO_CHAR_DATE 函数<br />**完整模式：** 使用 TO_CHAR_DATE_LS 函数 (NLS 护理) |
|**为 DBMS_SQL 生成错误。分析**|如果选择 " **错误**"，则 SSMA 会在转换时生成错误 `DBMS_SQL.PARSE` 。<br /><br />如果选择 " **警告**"，则 SSMA 会在转换时生成警告 `DBMS_SQL.PARSE` 。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br />**默认/乐观/完整模式：** 条|
|**在 CONCAT 函数调用中使用 ISNULL**|`ISNULL` 语句用于 `CONCAT` 模拟 Oracle 行为的函数调用中。 此设置具有以下选项：<br /><br />YES<br /><br />是<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观模式：** 不<br />**完整模式：** 是的|
|**在 REPLACE 函数调用中使用 ISNULL**|`ISNULL` 语句用于 `REPLACE` 模拟 Oracle 行为的函数调用中。 此设置具有以下选项：<br /><br />YES<br /><br />是<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观模式：** 不<br />**完整模式：** 是的|
|**尽可能使用本机转换函数**|如果选择 **"是"**，则在可能的情况下，SSMA 会将转换 `TO_CHAR(date, format)` 为本机转换函数。<br /><br />如果选择 " **否**"，则 SSMA `TO_CHAR(date, format)` `TO_CHAR_DATE` `TO_CHAR_DATE_LS` 会通过 **将 TO_CHAR (日期、格式)** 选项) 来定义 (。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观模式：** 是的<br />**完整模式：** 不|

## <a name="conversion-messages"></a>转换消息

|术语|定义|
|-|-|
|**生成有关问题的消息**|指定 SSMA 是否在转换过程中生成信息性消息，在 "输出" 窗格中显示它们，并将它们添加到转换后的代码中。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观模式：** 不<br />**完整模式：** 不|

## <a name="miscellaneous-options"></a>“杂项”选项

|术语|定义|
|-|-|
|**将 ROWNUM 表达式强制转换为整数**|当 SSMA 转换 `ROWNUM` 表达式时，它将表达式转换为 `TOP` 子句，后跟表达式。 下面的示例显示了 `ROWNUM` Oracle `DELETE` 语句：<br /><br />`DELETE FROM Table1`<br />`WHERE ROWNUM < expression and Field1 >= 2`<br /><br />下面的示例显示生成的 [!INCLUDE[tsql](../../includes/tsql-md.md)] ：<br /><br />`DELETE TOP (expression-1)`<br />`FROM Table1`<br />`WHERE Field1>=2`<br /><br />`TOP`要求 `TOP` 子句表达式的计算结果为一个整数。 如果整数为负数，则该语句将产生错误。<br /><br />如果选择 **"是"**，SSMA 会将表达式强制转换为整数。<br /><br />如果选择 " **否**"，则 SSMA 会将所有非整数表达式标记为转换后的代码中的错误。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/完整模式：** 不<br />**乐观模式：** 是的|  
|**默认架构映射**|此设置指定 Oracle 架构映射到 SQL Server 架构的方式。 此设置提供了两个选项：<br /><br />**数据库架构：** 在此模式下，Oracle 架构 `sch1` 默认情况下会映射到 `dbo` SQL Server 数据库中的 SQL Server 架构 `sch1` 。<br /><br />架构 **到架构：** 在此模式下，Oracle 架构 `sch1` 默认情况下会映射到 `sch1` "连接" 对话框中提供的默认 SQL Server 数据库 SQL Server 架构。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 架构到数据库|
|**在 ORDER BY 子句中模拟 Oracle null 行为**|`NULL` 值在和 Oracle 中的排序方式不同 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ：<br /><br />在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ， `NULL` 值是排序列表中的最小值。 在升序列表中， `NULL` 将首先显示值。<br /><br />在 Oracle 中， `NULL` 值是排序列表中的最大值。 默认情况下， `NULL` 值在升序列表中显示为最后。<br /><br />Oracle 具有 `NULLS FIRST` 和 `NULLS LAST` 子句，使你可以更改 Oracle 订单的方式 `NULL` 。<br /><br />SSMA 可以 `ORDER BY` 通过检查值来模拟 Oracle 行为 `NULL` 。 然后，它首先按 `NULL` 指定顺序对值进行排序，然后按其他值排序。<br /><br />如果选择 **"是"**，SSMA 将以模拟 oracle 行为的方式转换 oracle 语句 `ORDER BY` 。<br /><br />如果选择 " **否**"，则 SSMA 将忽略 Oracle 规则并在遇到和子句时生成错误消息 `NULLS FIRST` `NULLS LAST` 。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观模式：** 不<br />**完整模式：** 是的|
|**在 SELECT 中模拟行计数异常**|如果 `SELECT` 带有 INTO 子句的语句未返回任何行，则 Oracle 会引发 `NO_DATA_FOUND` 异常。 如果该语句返回两行或更多行，则 `TOO_MANY_ROWS` 会引发异常。 如果行计数与一个不同，则中的转换后的语句 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会引发任何异常。<br /><br />如果选择 **"是"**，则 SSMA 会 `db_error_exact_one_row_check` 在每个语句后添加对特殊过程的调用 `SELECT` 。 此过程模拟 `NO_DATA_FOUND` 和 `TOO_MANY_ROWS` 异常。 这是默认设置，它允许尽可能接近地重现 Oracle 行为。 如果源代码包含处理这些错误的异常处理程序，则应始终选择 **"是"** 。 请注意，如果 `SELECT` 语句出现在用户定义函数中，则此模块将转换为存储过程，因为执行存储过程和引发异常与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 函数上下文不兼容。<br /><br />如果选择 " **否**"，则不会生成任何异常。 当 SSMA 转换用户定义函数，并且你希望它保留在中时，这可能很有用。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|
|**启用 Fix Advisor**|启用后，SSMA 将尝试从你在目标 T-sql 代码中所做的修改中学习，并在其他位置（可以应用类似模式）中建议可能的代码修复。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|
|**生成常量表达式列别名**|如果列表中的 expression `SELECT` 缺少别名，则 SSMA 会生成常量别名 (如 `expr1` 、等 `expr2` ) 或将表达式本身用作别名。 由于表达式可能会变得非常长并且列名的长度有限，因此，对此类别名使用常量基名称比较安全。 即使这是一个更安全的选项，有时也是不可能的，因为可能存在对生成的数据集的外部依赖关系。 在这些情况下，你可能想要根据其值表达式命名列，这类似于 Oracle 的行为。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观模式：** 是的<br />**完整模式：** 不|
|**省略扩展属性**|启用后，SSMA 不会将扩展属性添加到它在目标数据库中创建的对象。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 不|
|**转换错误代码**|启用后，如果找到映射，则目标 SQL Server 端的错误号将转换为 Oracle 错误代码。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/完整模式：** 是的<br />**乐观模式：** 不|
|**对类型引用使用完全类型规范**|启用后，SSMA 将遵循 (的完整类型规范，包括常规参数和返回值的缩放和精度) 。 Oracle 不允许对例程参数使用数据类型参数，但在某些情况下，可以隐式派生这些参数，例如 `%TYPE` 在 `%ROWTYPE` 使用和属性时。 在这种情况下，SSMA 可使用完全类型规范 (包括在将其转换为 SQL Server 时的精度和小数位数) 。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观模式：** 是的<br />**完整模式：** 不|
|**在字符串串联中使用 ISNULL**|Oracle 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回不同 `NULL` 的结果。 Oracle 将 `NULL` 值视为空字符集。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回 `NULL`。<br /><br />如果选择 **"是"**，则 SSMA 会将 Oracle 串联字符替换 ( # A2) 与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 串联字符 (+) 。 SSMA 还会检查值的串联两侧的表达式 `NULL` 。<br /><br />如果选择 " **否**"，SSMA 将替换串联字符，但不会检查 `NULL` 值。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|

## <a name="objects-conversion"></a>对象转换

|术语|定义|
|-|-|
|**将外键转换为不为 NULL 的列上的 SET NULL 引用操作**|Oracle 允许创建外键约束，其中无法 `SET NULL` 执行操作，因为被引用列中不允许有 Null 值。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不允许进行这样的外键配置。<br /><br />如果选择 **"是"**，则 SSMA 将生成 Oracle 中的引用操作，但需要在将约束加载到之前进行手动更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 例如，您可以选择 `NO ACTION` 而不是 `SET NULL` 。<br /><br />如果选择 " **否**"，则约束将被标记为错误。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 不|
|**转换子类型**|SSMA 可以通过两种方式转换 PL/SQL 子类型：<br /><br />如果选择 **"是"**，则 SSMA 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从子类型创建用户定义类型，并将其用于此子类型的每个变量。<br /><br />如果选择 " **否**"，则 SSMA 将用基础类型替换子类型的所有源声明，并像平常一样转换结果。 在这种情况下，不会在中创建其他类型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 不|
|**转换同义词**|以下 Oracle 对象的同义词可迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ：<br /><br />表和对象表<br /><br />视图和对象视图<br /><br />存储过程和函数<br /><br />具体化视图<br /><br />**以下** 项的同义词可以通过对对象的直接引用来替换 Oracle 对象：<br /><br />序列<br /><br />包<br /><br />Java 类架构对象<br /><br />用户定义的对象类型<br /><br />无法迁移其他同义词。 SSMA 将生成同义词的错误消息以及使用同义词的所有引用。<br /><br />如果选择 **"是"**，SSMA 会 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 根据前面的列表创建同义词和直接对象引用。<br /><br />如果选择 " **否**"，则 SSMA 将为此处列出的所有同义词创建直接对象引用。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|
|**本地模块转换**|定义在独立存储过程或函数) 转换中声明的 Oracle 嵌套 subprogram (的类型。<br /><br />如果选择 " **内联**"，则嵌套的 subprogram 调用将替换为其正文。<br /><br />如果选择 " **存储过程**"，嵌套的 subprogram 将转换为 SQL Server 存储过程，并且在此过程调用中将替换其调用。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 内|

## <a name="records-conversion"></a>记录转换

|术语|定义|
|-|-|
|**将记录转换为分隔变量列表**|SSMA 可以将 Oracle 记录转换为分隔变量，并将其转换为具有特定结构的 XML 变量。<br /><br />如果选择 **"是"**，则在可能的情况下，SSMA 会将记录转换为分隔变量的列表。<br /><br />如果选择 " **否**"，SSMA 会将记录转换为具有特定结构的 XML 变量。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|
|**使用 SELECT .。。如果在转换 SELECT .。。INTO for record 变量**|指定在选择到记录变量时是否生成 XML 结果集。<br /><br />如果选择 **"是"**，则 select 语句返回 XML。<br /><br />如果选择 " **否**"，则 select 语句返回一个结果集。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 不|

## <a name="returning-clause-conversion"></a>返回子句转换

|术语|定义|
|-|-|
|**将 DELETE 语句中的返回子句转换为输出**|Oracle 提供了一个 `RETURNING` 子句作为立即获取已删除值的方法。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过子句提供该功能 `OUTPUT` 。<br /><br />如果选择 **"是"**，SSMA 会将 `RETURNING` 语句中的子句转换 `DELETE` 为 `OUTPUT` 子句。 由于表中的触发器可以更改值，因此返回的值可能与在 Oracle 中的值不同 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br />如果选择 " **否**"，则 SSMA 将 `SELECT` 在语句前面生成语句 `DELETE` 以检索返回值。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|
|**将 INSERT 语句中的返回子句转换为输出**|Oracle 提供了一个 `RETURNING` 子句作为立即获取插入值的方法。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过子句提供该功能 `OUTPUT` 。<br /><br />如果选择 **"是"**，SSMA 会将 `RETURNING` 语句中的子句转换 `INSERT` 为 `OUTPUT` 。 由于表中的触发器可以更改值，因此返回的值可能与在 Oracle 中的值不同 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br />如果选择 " **否**"，SSMA 将通过从引用表中插入和选择值来模拟 Oracle 功能。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|
|**将 UPDATE 语句中的返回子句转换为输出**|Oracle 提供了一个 `RETURNING` 子句作为立即获取更新值的方法。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过子句提供该功能 `OUTPUT` 。<br /><br />如果选择 **"是"**，SSMA 会将 `RETURNING` 语句中的子句转换 `UPDATE` 为 `OUTPUT` 子句。 由于表中的触发器可以更改值，因此返回的值可能与在 Oracle 中的值不同 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br />如果选择 " **否**"，SSMA 将在语句后面生成 select 语句 `UPDATE` 以检索返回值。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|

## <a name="rowid-generation"></a>ROWID 生成

|术语|定义|
|-|-|
|**生成 ROWID 列**|当 SSMA 在中创建表时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，可以创建 ROWID 列。 迁移数据时，每行都会获得 `UNIQUEIDENTIFIER` 函数生成的新值 `newid()` 。<br /><br />如果选择 **"是**"，则将 `ROWID` 在所有表上创建该列，并在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 插入值时生成 guid。 如果计划使用 SSMA 测试人员，请始终选择 **"是"** 。<br /><br />如果选择 " **否**"，则不会将 ROWID 列添加到表中。<br /><br />为包含触发器的表添加包含 **触发器的表的 ROWID 列** `ROWID` 。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观模式：** 为包含触发器的表添加 ROWID 列<br /><br />**完整模式：** 是的|
|**针对 ROWID 列生成唯一索引**|指定 SSMA 是否在生成的列上生成唯一索引列 `ROWID` 。 如果将此选项设置为 "是"，则将生成唯一索引，并且如果将其设置为 "否"，则不会对列生成唯一索引 `ROWID` 。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|

## <a name="sequence-and-identity-conversion"></a>序列和标识转换

|术语|定义|
|-|-|
|**将标识转换为**|Oracle 为标识列提供多个配置选项。 SQL Server 中的标识功能不支持其中一些选项。<br /><br />保留这些选项的方法是将标识转换为序列。<br /><br />如果选择 " **序列**"，Oracle 标识列将不再转换为 SQL 标识列。 相反，将创建一个序列并使用它来生成列的默认值。<br /><br />如果选择 " **标识**"，Oracle 标识列将转换为 SQL identity 列。 不会转换不受支持的选项。 <br /><br />如果选择 " **最佳匹配**"，SSMA 会根据 Oracle 标识列的配置，决定 (标识或序列) 的最佳合适转换方法。|
|**转换序列生成器**|在 Oracle 中，您可以使用序列生成唯一标识符。<br /><br />SSMA 可以将序列转换为以下项。<br /><br />使用 SQL Server 序列生成器。<br /><br />使用 SSMA 序列生成器。<br /><br />使用列标识。<br /><br />默认选项是使用 SQL Server 序列生成器。 但 SQL Server 不支持获取当前序列值 (例如，) Oracle sequence `CURRVAL` 方法。 有关迁移 Oracle sequence 方法的指导，请参阅 SSMA 团队博客网站 `CURRVAL` 。<br /><br />SSMA 还提供了一个用于将 Oracle 序列转换为 SSMA 序列模拟器的选项。 当你转换到2012之前的 SQL Server 时，这是默认选项<br /><br />最后，还可以将分配给表中的列的序列转换为 SQL Server 标识值。 您必须在 "Oracle **表** " 选项卡上指定序列与标识列之间的映射|
|**在触发器外转换 CURRVAL**|仅当 **转换序列生成器** 设置为 **使用列标识** 时可见。 由于 Oracle 序列是与表分离的对象，因此许多使用序列的表都使用触发器来生成和插入新的序列值。 SSMA 注释掉这些语句，或在注释掉会生成错误时将其标记为错误。<br /><br />如果选择 **"是"**，则 SSMA 会在转换后的序列上将所有引用标记为外部触发器， `CURRVAL` 并发出警告。<br /><br />如果选择 " **否**"，则 SSMA 会在转换后的序列上将对外部触发器的所有引用标记为 `CURRVAL` 错误。|

## <a name="statements-conversion"></a>语句转换

|术语|定义|
|-|-|
|**MERGE 语句的转换**|如果选择 " **使用 INSERT、UPDATE、DELETE 语句**"，SSMA 会将 `MERGE` 语句转换为 `INSERT` ， `UPDATE` ， `DELETE` 语句。<br /><br />如果选择 **使用 MERGE 语句**，SSMA 会将 `MERGE` 语句转换为 `MERGE` 中的语句 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 使用 MERGE 语句|
|**将调用转换为使用默认参数的 subprograms**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 函数不支持省略函数调用中的参数。 另外， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 函数和过程不支持表达式作为默认参数值。<br /><br />如果选择 **"是"** ，并且函数调用省略了参数，则 SSMA 会将关键字 **默认值** 插入函数并调用正确的位置。 然后，它将使用警告标记调用。<br /><br />如果选择 " **否**"，SSMA 会将函数调用标记为错误。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|
|**将 FORALL 语句转换为 WHILE 语句**|定义 SSMA 将如何处理 `FORALL` PL/SQL 集合元素上的循环。<br /><br />如果选择 **"是"**，则 SSMA 将创建一个 `WHILE` 循环，其中集合元素逐个检索。<br /><br />如果选择 " **否**"，则 SSMA 将使用方法从集合中生成一个行集 `nodes()` ，并将其用作单个表。 这会更有效，但会使输出代码更易读。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观模式：** 不<br />**完整模式：** 是的|
|**将函数调用转换为过程调用**|某些 Oracle 函数定义为自治事务或包含在中无效的语句 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 在这些情况下，SSMA 会创建一个过程和一个函数，该函数是过程的包装器。 转换后的函数会调用实现过程。<br /><br />SSMA 可以将对包装函数的调用转换为对过程的调用。 这会创建更易于阅读的代码，并可提高性能。 但是，上下文并不总是允许它;例如，不能将列表中的函数调用替换为 `SELECT` 过程调用。 SSMA 有几个选项可用于涵盖常见案例：<br /><br />如果选择 " **始终**"，则 SSMA 会尝试将包装函数调用转换为过程调用。 如果当前上下文不允许此转换，则生成错误消息。 这样就不会在生成的代码中留下任何函数调用。<br /><br />如果尽可能选择，SSMA 仅在函数具有输出参数 **时** 才会移动到过程调用。 如果无法移动，则会删除参数的 output 属性。 在所有其他情况下，SSMA 将保留函数调用。<br /><br />如果选择 " **从不**"，则 SSMA 会将所有函数调用作为函数调用。 有时，由于性能方面的原因，此选择可能无法接受。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 尽可能|
|**Convert LOCK TABLE 语句**|SSMA 可以将许多 `LOCK TABLE` 语句转换为表提示。 SSMA 无法转换任何 `LOCK TABLE` 包含 `PARTITION` 、 `SUBPARTITION` 、 `@dblink` 和子句的语句 `NOWAIT` ，并将此类语句与转换错误消息一起标记。<br /><br />如果选择 **"是"**，SSMA 会将支持的 `LOCK TABLE` 语句转换为表提示。<br /><br />如果选择 " **否**"，则 SSMA 将 `LOCK TABLE` 用转换错误消息标记所有语句。<br /><br />下表显示了 SSMA 如何转换 Oracle 锁定模式：<br /><br />**Oracle 锁定模式**<br /><br />`ROW SHARE`<br />`ROW EXCLUSIVE`<br />`SHARE UPDATE = ROW SHARE`<br />`SHARE`<br />`SHARE`<br />`EXCLUSIVE`<br /><br />**SQL Server 表提示**<br /><br />`ROWLOCK, HOLDLOCK`<br />`ROWLOCK, XLOCK, HOLDLOCK`<br />`ROWLOCK, HOLDLOCK`<br />`TABLOCK, HOLDLOCK`<br />`TABLOCK, XLOCK, HOLDLOCK`<br />`TABLOCKX, HOLDLOCK`<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|
|**为 REF CURSOR OUT 参数转换开放式语句**|在 Oracle 中， `OPEN .. FOR` 可以使用语句将结果集返回到类型为 subprogram 的 `OUT` 参数 `REF CURSOR` 。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，存储过程直接返回语句的结果 `SELECT` 。<br /><br />SSMA 可以将许多 `OPEN .. FOR` 语句转换为 `SELECT` 语句。<br /><br />如果选择 **"是"**，SSMA 会将 `OPEN .. FOR` 语句转换为 `SELECT` 语句，该语句将结果集返回到客户端。<br /><br />如果选择 " **否**"，则 SSMA 会在转换后的代码和输出窗格中生成错误消息。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|
|**转换事务处理语句**|SSMA 可以转换 Oracle 事务处理语句：<br /><br />如果选择 **"是"**，SSMA 会将 Oracle 事务处理语句转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句。<br /><br />如果选择 " **否**"，则 SSMA 会将事务处理语句标记为转换错误。<br /><br />**注意：** Oracle 隐式打开事务。 若要在上模拟此行为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，必须手动添加语句，以便在 `BEGIN TRANSACTION` 何处开始事务。 或者，你可以 `SET IMPLICIT_TRANSACTIONS ON` 在会话开始时执行命令。 将 `SET IMPLICIT_TRANSACTIONS ON` 子例程与自治事务转换时，SSMA 会自动添加。<br /><br />在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：<br /><br />**默认/乐观/完整模式：** 是的|

## <a name="see-also"></a>另请参阅

[用户界面参考 (OracleToSQL)](../../ssma/oracle/user-interface-reference-oracletosql.md)
