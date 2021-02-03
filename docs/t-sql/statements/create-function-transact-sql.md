---
description: CREATE FUNCTION (Transact-SQL)
title: CREATE FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FUNCTION
- CREATE FUNCTION
- CREATE_FUNCTION_TSQL
- FUNCTION_TSQL
- TVF
- MSTVF
dev_langs:
- TSQL
helpviewer_keywords:
- invoking functions
- extended stored procedures [SQL Server], functions
- table-valued parameters
- user-defined functions [SQL Server], creating
- CLR functions [SQL Server]
- CREATE FUNCTION statement
- nondeterministic functions
- user-defined data types
- functions [SQL Server], creating
- inline table-valued functions [SQL Server]
- multi-statement table-valued functions [SQL Server]
- table-valued functions [SQL Server], CREATE FUNCTION
- parameters [SQL Server], functions
- nesting user-defined functions
- deterministic functions
- scalar-valued functions
- scalar UDF
- MSTVF
- TVF
- functions [SQL Server], invoking
ms.assetid: 864b393f-225f-4895-8c8d-4db59ea60032
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bf80bb7b33dc28941791cd4fa6f09f384ee858f1
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237304"
---
# <a name="create-function-transact-sql"></a>CREATE FUNCTION (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中创建用户定义函数。 用户定义函数是接受参数、执行操作（例如复杂计算）并将操作结果以值的形式返回的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 或公共语言运行时 (CLR) 例程。 返回值可以是标量（单个）值或表。 使用此语句可以创建可通过以下方式使用的重复使用的例程：

- 在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句（如 SELECT）中

- 在调用该函数的应用程序中

- 在另一个用户定义函数的定义中

- 用于参数化视图或改进索引视图的功能

- 用于在表中定义列

- 用于为列定义 CHECK 约束

- 用于替换存储过程

- 使用内联函数作为安全策略的筛选器谓词

> [!NOTE]
>
> - 本主题讨论 .NET Framework CLR 与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的集成。 CLR 集成不适用于 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。
> - 有关 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]，请参阅 [CREATE FUNCTION (Azure Synapse Analytics)](../../t-sql/statements/create-function-sql-data-warehouse.md)。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
-- Transact-SQL Scalar Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type
 [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS return_data_type
    [ WITH <function_option> [ ,...n ] ]
    [ AS ]
    BEGIN
        function_body
        RETURN scalar_expression
    END
[ ; ]
```

```syntaxsql
-- Transact-SQL Inline Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type
    [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS TABLE
    [ WITH <function_option> [ ,...n ] ]
    [ AS ]
    RETURN [ ( ] select_stmt [ ) ]
[ ; ]
```

```syntaxsql
-- Transact-SQL Multi-Statement Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type
    [ = default ] [READONLY] }
    [ ,...n ]
  ]
)
RETURNS @return_variable TABLE <table_type_definition>
    [ WITH <function_option> [ ,...n ] ]
    [ AS ]
    BEGIN
        function_body
        RETURN
    END
[ ; ]

```

```syntaxsql
-- Transact-SQL Function Clauses
<function_option>::=
{
    [ ENCRYPTION ]
  | [ SCHEMABINDING ]
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
  | [ EXECUTE_AS_Clause ]
  | [ INLINE = { ON | OFF }]
}

<table_type_definition>:: =
( { <column_definition> <column_constraint>
  | <computed_column_definition> }
    [ <table_constraint> ] [ ,...n ]
)
<column_definition>::=
{
    { column_name data_type }
    [ [ DEFAULT constant_expression ]
      [ COLLATE collation_name ] | [ ROWGUIDCOL ]
    ]
    | [ IDENTITY [ (seed , increment ) ] ]
    [ <column_constraint> [ ...n ] ]
}

<column_constraint>::=
{
    [ NULL | NOT NULL ]
    { PRIMARY KEY | UNIQUE }
      [ CLUSTERED | NONCLUSTERED ]
      [ WITH FILLFACTOR = fillfactor
        | WITH ( < index_option > [ , ...n ] )
      [ ON { filegroup | "default" } ]
  | [ CHECK ( logical_expression ) ] [ ,...n ]
}

<computed_column_definition>::=
column_name AS computed_column_expression

<table_constraint>::=
{
    { PRIMARY KEY | UNIQUE }
      [ CLUSTERED | NONCLUSTERED ]
      ( column_name [ ASC | DESC ] [ ,...n ]
        [ WITH FILLFACTOR = fillfactor
        | WITH ( <index_option> [ , ...n ] )
  | [ CHECK ( logical_expression ) ] [ ,...n ]
}

<index_option>::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS ={ ON | OFF }
}
```

```syntaxsql
-- CLR Scalar Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type
    [ = default ] }
    [ ,...n ]
)
RETURNS { return_data_type }
    [ WITH <clr_function_option> [ ,...n ] ]
    [ AS ] EXTERNAL NAME <method_specifier>
[ ; ]
```

```syntaxsql
-- CLR Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type
    [ = default ] }
    [ ,...n ]
)
RETURNS TABLE <clr_table_type_definition>
    [ WITH <clr_function_option> [ ,...n ] ]
    [ ORDER ( <order_clause> ) ]
    [ AS ] EXTERNAL NAME <method_specifier>
[ ; ]
```

```syntaxsql
-- CLR Function Clauses
<order_clause> ::=
{
   <column_name_in_clr_table_type_definition>
   [ ASC | DESC ]
} [ ,...n]

<method_specifier>::=
    assembly_name.class_name.method_name

<clr_function_option>::=
}
    [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
  | [ EXECUTE_AS_Clause ]
}

<clr_table_type_definition>::=
( { column_name data_type } [ ,...n ] )
```

```syntaxsql
-- In-Memory OLTP: Syntax for natively compiled, scalar user-defined function
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
 ( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type
    [ NULL | NOT NULL ] [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS return_data_type
     WITH <function_option> [ ,...n ]
    [ AS ]
    BEGIN ATOMIC WITH (set_option [ ,... n ])
        function_body
        RETURN scalar_expression
    END

<function_option>::=
{
  |  NATIVE_COMPILATION
  |  SCHEMABINDING
  | [ EXECUTE_AS_Clause ]
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
}

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

OR ALTER
 适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]SP1 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

只有在函数已存在时才对其进行有条件地更改。

> [!NOTE]
> 从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU1 开始，可以使用 CLR 的可选 [OR ALTER] 语法。

schema_name 是用户定义函数所属的架构的名称。

function_name 是用户定义函数的名称。 函数名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则，并且在数据库中以及对其架构来说是唯一的。

> [!NOTE]
> 即使未指定参数，函数名称后也需要加上括号。

@parameter_name 是用户定义函数中的参数。 可声明一个或多个参数。

一个函数最多可以有 2,100 个参数。 执行函数时，如果未定义参数的默认值，则用户必须提供每个已声明参数的值。

通过将 at 符号 (@) 用作第一个字符来指定参数名称。 参数名称必须符合标识符规则。 参数是对应于函数的局部参数；其他函数中可使用相同的参数名称。 参数只能代替常量，而不能用于代替表名、列名或其他数据库对象的名称。

> [!NOTE]
> 在传递存储过程或用户定义函数中的参数时，或在声明和设置批语句中的变量时，不会遵守 ANSI_WARNINGS。 例如，如果将一个变量定义为 char(3)，然后将其值设置为大于三个字符，则数据会被截断为定义的大小，并且 `INSERT` 或 `UPDATE` 语句可以成功执行。

[ type_schema_name. parameter_data_type 是参数的数据类型及其所属的架构，后者为可选项。 对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数，允许使用除 timestamp 数据类型之外的所有数据类型（包括 CLR 用户定义类型和用户定义表类型）。 对于 CLR 函数，允许使用除 text、ntext、image、用户定义表类型和 timestamp 数据类型之外的所有数据类型（包括 CLR 用户定义类型）   。 在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数或 CLR 函数中，不能将非标量类型 **cursor** 和 **table** 指定为参数数据类型。

如果未指定 *type_schema_name*，[!INCLUDE[ssDE](../../includes/ssde-md.md)]会按以下顺序查找 *scalar_parameter_data_type*：

- 包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型名称的架构。
- 当前数据库中当前用户的默认架构。
- 当前数据库中的 **dbo** 架构。

[ =default ] 是参数的默认值。 如果定义了 default 值，则无需指定此参数的值即可执行函数。

> [!NOTE]
> 可以为除 **varchar(max)** 和 **varbinary(max)** 数据类型之外的 CLR 函数指定默认的参数值。

如果函数的参数有默认值，则调用该函数以检索默认值时，必须指定关键字 DEFAULT。 此行为与在存储过程中使用具有默认值的参数不同，在后一种情况下，不提供参数同样意味着使用默认值。 但在通过使用 EXECUTE 语句调用标量函数时，DEFAULT 关键字不是必需的。

READONLY 指示不能在函数定义中更新或修改参数。 用户定义的表类型参数 (TVP) 需要 READONLY，而其他任何参数类型都不能使用 READONLY。

return_data_type 是标量用户定义函数的返回值。 对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数，可以使用除 timestamp 数据类型之外的所有数据类型（包括 CLR 用户定义类型）。 对于 CLR 函数，允许使用除 text、ntext、image 和 timestamp 数据类型之外的所有数据类型（包括 CLR 用户定义类型）   。 在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数或 CLR 函数中，不能将非标量类型 **cursor** 和 **table** 指定为返回数据类型。

function_body 指定一系列定义函数值的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，这些语句在一起使用不会产生负面影响（例如修改表）。 function_body 仅用于标量函数和多语句表值函数 (MSTVF)。

在标量函数中，function_body 是一系列 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，这些语句一起使用可计算出标量值。

在 MSTVF 中，function_body 是一系列 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，这些语句将填充 TABLE 返回变量。

scalar_expression 指定标量函数返回的标量值。

TABLE 指定表值函数 (TVF) 的返回值为表。 只有常量和 @local_variables 可以传递到 TVF。

在内联 TVF 中，TABLE 返回值是通过单个 SELECT 语句定义的。 内联函数没有关联的返回变量。

<a name="mstvf"></a> 在 MSTVF 中，\@return_variable 是一个 TABLE 变量，用于存储和汇总应作为函数值返回的行。 只能将 \@*return_variable* 指定用于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数，而不能用于 CLR 函数。

select_stmt 定义内联表值函数 (TVF) 返回值的单个 SELECT 语句。

ORDER (\<order_clause>) 指定从表值函数中返回结果的顺序。 有关详细信息，请参阅本主题后面的“[在 CLR 表值函数中使用排序顺序](#using-sort-order-in-clr-table-valued-functions)”部分。

EXTERNAL NAME \<method_specifier> *assembly_name*.*class_name*.*method_name*
适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]SP1 及更高版本）

指定创建的函数名称应引用的程序集和方法。

- assembly_name - 必须与 `SELECT * FROM sys.assemblies;` 的 `name` 列中的值匹配。

  这是曾在 `CREATE ASSEMBLY` 语句中使用的名称。

- class_name - 必须与 `SELECT * FROM sys.assembly_modules;` 的 `assembly_name` 列中的值匹配。

  此值通常包含嵌入的句点或圆点。 在这种情况下，Transact-SQL 语法要求将该值置于一对方括号 [] 或一对双引号 "" 内。

- method_name - 必须与 `SELECT * FROM sys.assembly_modules;` 的 `method_name` 列中的值匹配。

  该方法必须是静态方法。

在典型示例中，对于 MyFood.DLL（其中所有类型都位于 MyFood 命名空间中），`EXTERNAL NAME` 值可以是：`MyFood.[MyFood.MyClass].MyStaticMethod`

> [!NOTE]
>
> - 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不能执行 CLR 代码。 可以创建、修改和删除引用公共语言运行时模块的数据库对象；不过，只有在启用 [clr enabled 选项](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md)之后，才能在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中执行这些引用。 若要启用此选项，请使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)。
> - 此选项在包含数据库中不可用。

*\<*table_type_definition*>* ( { \<column_definition> \<column_constraint>| \<computed_column_definition> } [ \<table_constraint> ] [ ,...*n* ] ) 为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数定义表数据类型。 表声明包含列定义和列约束（或表约束）。 表始终放在主文件组中。

*\< clr_table_type_definition >* ( { *column_name**data_type* } [ ,...*n* ] ) 适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]（[在某些区域以预览版提供](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)）。

定义 CLR 函数的表数据类型。 表声明仅包含列名称和数据类型。 表始终放在主文件组中。

NULL|NOT NULL 仅本机编译的标量用户定义函数支持该参数。 有关详细信息，请参阅[内存中 OLTP 的标量用户定义函数](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)。

NATIVE_COMPILATION 指示用户定义函数是否已本机编译。 对于本机编译的标量用户定义函数，此参数是必需的。

BEGIN ATOMIC WITH 仅本机编译的标量用户定义函数支持该参数，且该参数是必需的。 有关详细信息，请参阅 [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md)。

SCHEMABINDING 对于本机编译的标量用户定义函数，SCHEMABINDING 参数是必需的。

EXECUTE 对于本机编译的标量用户定义函数，EXECUTE AS 是必需的。

**\<function_option>::= and \<clr_function_option>::=**

指定函数将具有以下一个或多个选项：

ENCRYPTION 适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]SP1 及更高版本）

指示[!INCLUDE[ssDE](../../includes/ssde-md.md)]会将 CREATE FUNCTION 语句的原始文本转换为模糊格式。 模糊代码的输出在任何目录视图中都不能直接显示。 对系统表或数据库文件没有访问权限的用户不能检索模糊文本。 但是，可以通过 [DAC 端口](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)访问系统表的特权用户或直接访问数据库文件的特权用户可以使用此文本。 此外，能够向服务器进程附加调试器的用户可在运行时从内存中检索原始过程。 有关如何访问系统元数据的详细信息，请参阅[元数据可见性配置](../../relational-databases/security/metadata-visibility-configuration.md)。

使用此选项可防止将函数作为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 复制的一部分发布。 不能为 CLR 函数指定此选项。

SCHEMABINDING 指定将函数绑定到其引用的数据库对象。 如果指定了 SCHEMABINDING，则不能按照将影响函数定义的方式修改基对象。 必须首先修改或删除函数定义本身，才能删除将要修改的对象的依赖关系。

只有发生下列操作之一时，才会删除函数与其引用对象的绑定：

- 删除函数。
- 在未指定 `SCHEMABINDING` 选项的情况下，使用 `ALTER` 语句修改函数。

只有满足以下条件时，函数才能绑定到架构：

- 函数是一个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数。
- 该函数引用的用户定义函数和视图也绑定到架构。
- 该函数引用的对象是用由两部分组成的名称引用的。
- 该函数及其引用的对象属于同一数据库。
- 执行 `CREATE FUNCTION` 语句的用户对该函数引用的数据库对象具有 `REFERENCES` 权限。

RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT 指定标量函数的 OnNULLCall 特性 。 如果未指定，则默认为 CALLED ON NULL INPUT。 这意味着即使传递的参数为 NULL，也将执行函数体。

如果在 CLR 函数中指定了 RETURNS NULL ON NULL INPUT，它指示当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 接收到的任何一个参数为 NULL 时，它可以返回 NULL，而无需实际调用函数体。 如果 \<method_specifier> 中指定的 CLR 函数的方法已具有指示 RETURNS NULL ON NULL INPUT 的自定义属性，但 CREATE FUNCTION 语句指示 CALLED ON NULL INPUT，则优先采用 CREATE FUNCTION 语句指示的属性。 不能为 CLR 表值函数指定 OnNULLCall 属性。

EXECUTE AS 子句指定用于执行用户定义函数的安全上下文。 所以，您可以控制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用哪一个用户帐户来验证针对该函数引用的任何数据库对象的权限。

> [!NOTE]
> 不能为内联表值函数指定 `EXECUTE AS`。

有关详细信息，请参阅 [EXECUTE AS 子句 (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md)。

INLINE = { ON | OFF } 适用于：SQL Server 2019 和更高版本。

指定是否应内联此标量 UDF。 此子句仅适用于标量用户定义函数。 `INLINE` 子句不是强制性的。 如果未指定 `INLINE` 子句，则会基于 UDF 是否可内联而自动设定为 ON/OFF。 如果指定了 `INLINE=ON` 但发现 UDF 不可内联，则会引发错误。 有关详细信息，请参阅[标量 UDF 内联](../../relational-databases/user-defined-functions/scalar-udf-inlining.md)。

**\< column_definition >::=**

定义表数据类型。 表声明包含列定义和约束。 对于 CLR 函数，只能指定 column_name 和 data_type 。

column_name 是表中列的名称。 列名称必须遵循标识符的规则，且在表中必须唯一。 column_name 可以包含 1 到 128 个字符。

data_type 指定列数据类型。 对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数，可以使用除 timestamp 之外的所有数据类型（包括 CLR 用户定义类型）。 对于 CLR 函数，可以使用除 text、ntext、image、char、varchar、varchar(max) 和 timestamp 之外的所有数据类型（包括 CLR 用户定义类型）。在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数或 CLR 函数中，不能将非标量类型 cursor 指定为列数据类型       。

DEFAULT constant_expression 如果在插入过程中未显式提供值，则指定为列提供的值。 constant_expression 可以是常量、NULL 或系统函数值。 DEFAULT 定义可以应用于除具有 IDENTITY 属性的列之外的任何列。 不能为 CLR 表值函数指定 DEFAULT。

COLLATE collation_name 指定列的排序规则。 如果未指定，则为该列分配数据库的默认排序规则。 排序规则名称既可以是 Windows 排序规则名称，也可以是 SQL 排序规则名称。 如需获取排序规则列表和详细信息，请参阅 [Windows 排序规则名称 (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md) 和 [SQL Server 排序规则名称 (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)。

COLLATE 子句只能用来更改数据类型为 char、varchar、nchar 和 nvarchar 的列的排序规则   。

> [!NOTE]
> 不能为 CLR 表值函数指定 `COLLATE`。

ROWGUIDCOL 指示新列是行的全局唯一标识符列。 对于每个表，只能将其中的一个 uniqueidentifier 列指定为 ROWGUIDCOL 列。 ROWGUIDCOL 属性只能分配给 uniqueidentifier 列。

ROWGUIDCOL 属性并不强制列中所存储值的唯一性。 该属性也不会为插入表的新行自动生成值。 若要为每列生成唯一值，请在 INSERT 语句中使用 NEWID 函数。 可以指定默认值；但是，不能将 NEWID 指定为默认值。

IDENTITY 指示新列是标识列。 在为表添加新行时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将为该列提供唯一的增量值。 标识列通常与 PRIMARY KEY 约束一起使用，作为表的唯一行标识符。 可以将 IDENTITY 属性分配到 tinyint、smallint、int、decimal(p,0) 或 numeric(p,0) 列     。 每个表只能创建一个标识列。 不能对标识列使用绑定默认值和 DEFAULT 约束。 必须同时指定 seed 和 increment，或者都不指定 。 如果二者都未指定，则取默认值 (1,1)。

不能为 CLR 表值函数指定 IDENTITY。

seed 是要分配给表中第一行的整数值。

increment 是要加到表中后续行的 seed 值上的整数值 。

 **\< column_constraint >::= and \< table_constraint>::=**

为指定列或表定义约束。 对于 CLR 函数，允许的唯一约束类型为 NULL。 不允许命名约束。

NULL | NOT NULL 确定列中是否允许使用空值。 严格来讲，NULL 不是约束，但可以像指定 NOT NULL 那样指定它。 不能为 CLR 表值函数指定 NOT NULL。

PRIMARY KEY 是一个约束，该约束通过唯一索引来强制指定列的实体完整性。 在表值用户定义函数中，只能对每个表中的一列创建 PRIMARY KEY 约束。 不能为 CLR 表值函数指定 PRIMARY KEY。

UNIQUE 一个约束，该约束通过唯一索引为一个或多个指定列提供实体完整性。 一个表可以有多个 UNIQUE 约束。 不能为 CLR 表值函数指定 UNIQUE。

CLUSTERED | NONCLUSTERED 指示为 PRIMARY KEY 或 UNIQUE 约束创建聚集索引还是非聚集索引。 PRIMARY KEY 约束使用 CLUSTERED，而 UNIQUE 约束使用 NONCLUSTERED。

只能为一个约束指定 CLUSTERED。 如果为 UNIQUE 约束指定了 CLUSTERED，并且指定了 PRIMARY KEY 约束，则 PRIMARY KEY 使用 NONCLUSTERED。

不能为 CLR 表值函数指定 CLUSTERED 和 NONCLUSTERED。

CHECK 一个约束，该约束通过限制可输入一列或多列中的可能值来强制实现域完整性。 不能为 CLR 表值函数指定 CHECK 约束。

logical_expression 返回 TRUE 或 FALSE 的逻辑表达式。

**\<computed_column_definition>::=**

指定计算列。 有关计算列的详细信息，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)。

column_name 是计算列的名称。

computed_column_expression 是定义计算列的值的表达式。

**\<index_option>::=**

为 PRIMARY KEY 或 UNIQUE 索引指定索引选项。 有关索引选项的详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。

PAD_INDEX = { ON | OFF } 指定索引填充。 默认为 OFF。

FILLFACTOR = fillfactor 指定一个百分比，指示在创建或更改索引的过程中，应将每个索引页面的叶级填充到什么程度[!INCLUDE[ssDE](../../includes/ssde-md.md)]。 fillfactor 必须是 1 到 100 之间的整数。 默认值为 0。

IGNORE_DUP_KEY = { ON | OFF } 指定在插入操作尝试向唯一索引中插入重复键值时的错误响应。 IGNORE_DUP_KEY 选项仅适用于创建或重新生成索引后发生的插入操作。 默认为 OFF。

STATISTICS_NORECOMPUTE = { ON | OFF } 指定是否重新计算分布统计信息。 默认为 OFF。

ALLOW_ROW_LOCKS = { ON | OFF } 指定是否允许行锁。 默认值为 ON。

ALLOW_PAGE_LOCKS = { ON | OFF } 指定是否允许使用页锁。 默认值为 ON。

## <a name="best-practices"></a>最佳实践

如果用户定义函数不是使用 `SCHEMABINDING` 子句创建的，则对基础对象进行的任何更改可能会影响函数定义并在调用函数时可能导致意外结果。 我们建议您实现以下方法之一，以便确保函数不会由于对于其基础对象的更改而过期：

- 创建函数时指定 `WITH SCHEMABINDING` 子句。 这确保除非也修改了函数，否则无法修改在函数定义中引用的对象。
- 在修改在函数定义中指定的任何对象后执行 [sp_refreshsqlmodule](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md) 存储过程。

> [!IMPORTANT]
> 有关内联表值函数（内联 TVF）和多语句表值函数 (MSTVF) 的详细信息和性能注意事项，请参阅[创建用户定义函数（数据库引擎）](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)。

## <a name="data-types"></a>数据类型

如果在 CLR 函数中指定了参数，则这些参数应为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 类型，即以前为 *scalar_parameter_data_type* 定义的类型。 有关将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型与 CLR 集成数据类型或 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 公共语言运行时数据类型进行比较的信息，请参阅[映射 CLR 参数数据](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md)。

为了使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在类中重载时引用正确方法，\<method_specifier> 中指示的方法必须具有下列特征：

- 接收 [ ,...n ] 中指定的参数数量。
- 通过值而不是引用来接收所有参数。
- 使用与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 函数中指定的类型兼容的参数类型。

如果 CLR 函数的返回数据类型指定表类型 (RETURNS TABLE)，则 \<method_specifier> 中方法的返回数据类型应为 **IEnumerator** 或 **IEnumerable** 类型，且假定由函数创建者来实现接口。 与 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数不同，CLR 函数不能在 \<table_type_definition> 中包含 PRIMARY KEY、UNIQUE 或 CHECK 约束。 \<table_type_definition> 中指定的列数据类型必须与 \<method_specifier> 中的方法在执行时返回的结果集中的对应列的类型相匹配。 创建函数时不执行上述类型检查。

有关如何对 CLR 函数编程的详细信息，请参阅 [CLR 用户定义函数](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md)。

## <a name="general-remarks"></a>一般备注

可在使用标量表达式的位置调用标量函数。 这包括计算列和 CHECK 约束定义。 也可以使用 [EXECUTE](../../t-sql/language-elements/execute-transact-sql.md) 语句执行标量函数。 必须使用至少由两部分组成名称的函数来调用标量函数 (<schema><function>)。 有关多部分名称的详细信息，请参阅 [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。 在允许表表达式的情况下，可在 `SELECT`、`INSERT`、`UPDATE` 或 `DELETE` 语句的 `FROM` 子句中调用表值函数。 有关详细信息，请参阅[执行用户定义函数](../../relational-databases/user-defined-functions/execute-user-defined-functions.md)。

## <a name="interoperability"></a>互操作性

下列语句在函数内有效：

- 赋值语句。
- `TRY...CATCH` 语句以外的控制流语句。
- 定义局部数据变量和局部游标的 `DECLARE` 语句。
- `SELECT` 语句，其中的选择列表包含为局部变量分配值的表达式。
- 游标操作，该操作引用在函数中声明、打开、关闭和释放的局部游标。 只允许使用以 `INTO` 子句向局部变量赋值的 `FETCH` 语句；不允许使用将数据返回到客户端的 `FETCH` 语句。
- 修改本地表变量的 `INSERT`、`UPDATE` 和 `DELETE` 语句。
- 调用扩展存储过程的 `EXECUTE` 语句。

有关详细信息，请参阅[创建用户定义函数（数据库引擎）](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)。

### <a name="computed-column-interoperability"></a>计算列互操作性

函数具有下列属性。 这些属性的值确定了函数是否可用于持久化计算列或索引计算列。

|properties|说明|说明|
|--------------|-----------------|-----------|
|**IsDeterministic**|函数是确定性函数还是不确定性函数。|确定性函数中允许本地数据访问。 例如，如果每次使用一组特定输入值和相同数据库状态调用函数时，函数都返回相同结果，则该函数将被标记为确定性函数。|
|**IsPrecise**|函数是精确函数还是不精确函数。|不精确函数包含浮点运算之类的运算。|
|**IsSystemVerified**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可验证函数的精度和确定性属性。||
|**SystemDataAccess**|函数可以访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地实例中的系统数据（系统目录或虚拟系统表）。||
|**UserDataAccess**|函数可以访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地实例中的用户数据。|包含用户定义表和临时表，但不包含表变量。|

[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数的精度和确定性属性由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动确定。 CLR 函数的数据访问权限和确定性属性可由用户指定。 有关详细信息，请参阅 [CLR 集成自定义属性概述](../../relational-databases/clr-integration/database-objects/clr-integration-custom-attributes-for-clr-routines.md)。

若要显示这些属性的当前值，请使用 [OBJECTPROPERTYEX](../../t-sql/functions/objectpropertyex-transact-sql.md)。

> [!IMPORTANT]
> 必须使用确定性的 `SCHEMABINDING` 创建函数。

如果用户定义函数具有下列属性值，则可以在索引中使用调用用户定义函数的计算列：

- **IsDeterministic** = true
- **IsSystemVerified** = true（计算列是持久性计算列时除外）
- **UserDataAccess** = false
- **SystemDataAccess** = false

有关详细信息，请参阅 [计算列上的索引](../../relational-databases/indexes/indexes-on-computed-columns.md)。

### <a name="calling-extended-stored-procedures-from-functions"></a>从函数中调用扩展存储过程

如果在函数中调用扩展存储过程，则该过程不能向客户端返回结果集。 向客户端返回结果集的任何 ODS API 都将返回 FAIL。 扩展存储过程可以连接回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例；不过，该过程不应尝试与调用扩展存储过程的函数同时联接到同一事务。

与通过批处理或存储过程进行调用相似，扩展存储过程在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 Windows 安全帐户的上下文中执行。 存储过程的所有者在授予用户 EXECUTE 权限时应考虑这一点。

## <a name="limitations-and-restrictions"></a>限制和局限

用户定义函数不能用于执行修改数据库状态的操作。

用户定义函数不能包含将表作为其目标的 `OUTPUT INTO` 子句。

下列 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 语句不能包含在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 用户定义函数的定义中：

- `BEGIN DIALOG CONVERSATION`
- `END CONVERSATION`
- `GET CONVERSATION GROUP`
- `MOVE CONVERSATION`
- `RECEIVE`
- `SEND`

用户定义函数可以嵌套；也就是说，用户定义函数可相互调用。 被调用函数开始执行时，嵌套级别将增加；被调用函数执行结束后，嵌套级别将减少。 用户定义函数的嵌套级别最多可达 32 级。 如果超出最大嵌套级别数，整个调用函数链将失败。 从 [!INCLUDE[tsql](../../includes/tsql-md.md)] 用户定义函数对托管代码的任何引用都将根据 32 级嵌套限制计入一个级别。 从托管代码内部调用的方法不根据此限制进行计数。

### <a name="using-sort-order-in-clr-table-valued-functions"></a>在 CLR 表值函数中使用排序顺序

在 CLR 表值函数中使用 `ORDER` 子句时请遵循以下准则：

- 必须确保始终按指定的顺序对结果进行排序。 如果没有按指定的顺序对结果进行排序，则在执行查询时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将生成一条错误消息。
- 如果指定了 `ORDER` 子句，则必须根据列（显式或隐式）的排序规则对表值函数的输出进行排序。 例如，如果列排序规则为中文（在表值函数的 DDL 中指定或从数据库排序规则中获取），则必须根据中文排序规则对返回的结果进行排序。
- 如果指定了 `ORDER` 子句，则在返回结果时始终由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对其验证，而不管查询处理器是否会使用该子句执行进一步的优化。 只有知道 `ORDER` 子句对查询处理器有用时才使用它。
- 在以下情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询处理器将自动使用 `ORDER` 子句：

  - “插入”查询，其中 `ORDER` 子句与索引兼容。
  - 与 `ORDER` 子句兼容的 `ORDER BY` 子句。
  - 聚合，其中 `GROUP BY` 与 `ORDER` 子句兼容。
  - `DISTINCT` 聚合，其中不同的列与 `ORDER` 子句兼容。

除非在查询中还指定了 `ORDER BY`，否则 `ORDER` 子句不保证在执行 SELECT 查询时得到有序结果。 有关如何查询表值函数排序顺序中所包含的列的信息，请参阅 [sys.function_order_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-function-order-columns-transact-sql.md)。

## <a name="metadata"></a>元数据

下表列出可用于返回与用户定义函数有关的元数据的系统目录视图。

|系统视图|说明|
|-----------------|-----------------|
|[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)|请参阅下面“示例”部分中的“示例 E”。|
|[sys.assembly_modules](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)|显示 CLR 用户定义函数的有关信息。|
|[sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)|显示用户定义函数中定义的参数的有关信息。|
|[sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)|显示函数所引用的基础对象。|

## <a name="permissions"></a>权限

需要在数据库中具有 `CREATE FUNCTION` 权限，并对创建函数时所在的架构具有 `ALTER` 权限。 如果函数指定用户定义类型，则需要对该类型具有 `EXECUTE` 权限。

## <a name="examples"></a>示例

> [!NOTE]
> 有关 UDF 的更多示例和性能注意事项，请参阅[创建用户定义函数（数据库引擎）](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)。

### <a name="a-using-a-scalar-valued-user-defined-function-that-calculates-the-iso-week"></a>A. 使用计算 ISO 周的标量值用户定义函数

下面的示例将创建用户定义函数 `ISOweek`。 此函数使用日期参数来计算 ISO 周数。 要使此函数能正确计算，必须在调用该函数前调用 `SET DATEFIRST 1`。

该示例还演示了如何使用 [EXECUTE AS](../../t-sql/statements/execute-as-clause-transact-sql.md) 子句指定可执行存储过程的安全上下文。 在该示例中，`CALLER` 选项指定该过程将在调用该过程的用户的上下文中执行。 还可以指定 `SELF`、`OWNER` 和 user_name 等其他选项。

下面是函数调用。 请注意，`DATEFIRST` 设置为 `1`。

```sql
CREATE FUNCTION dbo.ISOweek (@DATE datetime)
RETURNS int
WITH EXECUTE AS CALLER
AS
BEGIN
    DECLARE @ISOweek int;
    SET @ISOweek= DATEPART(wk,@DATE)+1
        -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104');
--Special cases: Jan 1-3 may belong to the previous year
    IF (@ISOweek=0)
        SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1
            AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1;
--Special case: Dec 29-31 may belong to the next year
    IF ((DATEPART(mm,@DATE)=12) AND
        ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28))
    SET @ISOweek=1;
    RETURN(@ISOweek);
END;
GO
SET DATEFIRST 1;
SELECT dbo.ISOweek(CONVERT(DATETIME,'12/26/2004',101)) AS 'ISO Week';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
ISO Week
----------------
52
```

### <a name="b-creating-an-inline-table-valued-function"></a>B. 创建内联表值函数

下面的示例在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中返回内联表值函数。 对于销售给商店的每个产品，该函数返回三列，分别为 `ProductID`、`Name` 以及各个商店年初至今总数的累计 `YTD Total`。

```sql
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)
RETURNS TABLE
AS
RETURN
(
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'
    FROM Production.Product AS P
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID
    WHERE C.StoreID = @storeid
    GROUP BY P.ProductID, P.Name
);
GO
```

若要调用该函数，请运行此查询。

```sql
SELECT * FROM Sales.ufn_SalesByStore (602);
```

### <a name="c-creating-a-multi-statement-table-valued-function"></a>C. 创建多语句表值函数

下面的示例在 AdventureWorks2012 数据库中创建表值函数 `fn_FindReports(InEmpID)`。 如果提供一个有效雇员 ID，该函数将返回一个表，该表对应于直接或间接向该雇员报告的所有雇员。 该函数使用递归公用表表达式 (CTE) 来生成雇员的层次结构列表。 有关递归 CTE 的详细信息，请参阅 [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md)。

```sql
CREATE FUNCTION dbo.ufn_FindReports (@InEmpID INTEGER)
RETURNS @retFindReports TABLE
(
    EmployeeID int primary key NOT NULL,
    FirstName nvarchar(255) NOT NULL,
    LastName nvarchar(255) NOT NULL,
    JobTitle nvarchar(50) NOT NULL,
    RecursionLevel int NOT NULL
)
--Returns a result set that lists all the employees who report to the
--specific employee directly or indirectly.*/
AS
BEGIN
WITH EMP_cte(EmployeeID, OrganizationNode, FirstName, LastName, JobTitle, RecursionLevel) -- CTE name and columns
    AS (
        -- Get the initial list of Employees for Manager n
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, 0
        FROM HumanResources.Employee e
INNER JOIN Person.Person p
ON p.BusinessEntityID = e.BusinessEntityID
        WHERE e.BusinessEntityID = @InEmpID
        UNION ALL
        -- Join recursive member to anchor
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, RecursionLevel + 1
        FROM HumanResources.Employee e
            INNER JOIN EMP_cte
            ON e.OrganizationNode.GetAncestor(1) = EMP_cte.OrganizationNode
INNER JOIN Person.Person p
ON p.BusinessEntityID = e.BusinessEntityID
        )
-- copy the required columns to the result of the function
    INSERT @retFindReports
    SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel
    FROM EMP_cte
    RETURN
END;
GO
-- Example invocation
SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel
FROM dbo.ufn_FindReports(1);

GO
```

### <a name="d-creating-a-clr-function"></a>D. 创建 CLR 函数

此示例会创建 CLR 函数 `len_s`。 在创建该函数之前，程序集 `SurrogateStringFunction.dll` 已在本地数据库中注册。

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]SP1 及更高版本）

```sql
DECLARE @SamplesPath nvarchar(1024);
-- You may have to modify the value of this variable if you have
-- installed the sample in a location other than the default location.
SELECT @SamplesPath = REPLACE
    (  physical_name
     , 'Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\master.mdf'
     , 'Microsoft SQL Server\130\Samples\Engine\Programmability\CLR\'
    )
  FROM master.sys.database_files
  WHERE name = 'master';

CREATE ASSEMBLY [SurrogateStringFunction]
FROM @SamplesPath + 'StringManipulate\CS\StringManipulate\bin\debug\SurrogateStringFunction.dll'
WITH PERMISSION_SET = EXTERNAL_ACCESS;
GO

CREATE FUNCTION [dbo].[len_s] (@str nvarchar(4000))
RETURNS bigint
AS EXTERNAL NAME [SurrogateStringFunction].[Microsoft.Samples.SqlServer.SurrogateStringFunction].[LenS];
GO
```

有关如何创建 CLR 表值函数的示例，请参阅 [CLR 表值函数](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-table-valued-functions.md)。

### <a name="e-displaying-the-definition-of-user-defined-functions"></a>E. 显示用户定义函数的定义

```sql
SELECT definition, type
FROM sys.sql_modules AS m
JOIN sys.objects AS o ON m.object_id = o.object_id
    AND type IN ('FN', 'IF', 'TF');
GO
```

不能使用 sys.sql_modules 查看使用 `ENCRYPTION` 选项创建的函数定义；不过，可显示有关加密函数的其他信息。

## <a name="see-also"></a>另请参阅

- [创建用户定义函数（数据库引擎）](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)
- [ALTER FUNCTION (Transact-SQL)](../../t-sql/statements/alter-function-transact-sql.md)
- [DROP FUNCTION (Transact-SQL)](../../t-sql/statements/drop-function-transact-sql.md)
- [OBJECTPROPERTYEX (Transact-SQL)](../../t-sql/functions/objectpropertyex-transact-sql.md)
- [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)
- [sys.assembly_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)
- [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)
- [CLR 用户定义函数](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
- [CREATE SECURITY POLICY (Transact-SQL)](../../t-sql/statements/create-security-policy-transact-sql.md)