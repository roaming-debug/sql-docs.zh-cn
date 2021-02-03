---
description: CREATE PROCEDURE (Transact-SQL)
title: CREATE PROCEDURE (Transact-SQL)
ms.custom: ''
ms.date: 09/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PROC
- PROCEDURE
- CREATE PROCEDURE
- CREATE_PROC_TSQL
- PROCEDURE_TSQL
- CREATE PROC
- PROC_TSQL
- CREATE_PROCEDURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- parameters [SQL Server], stored procedures
- table-valued parameters
- SET statement, stored procedures
- stored procedures [SQL Server], creating
- wildcard parameters [SQL Server]
- maximum size of stored procedures
- WITH RECOMPILE clause
- common language runtime [SQL Server], stored procedures
- CREATE PROCEDURE statement
- local temporary procedures [SQL Server]
- WITH ENCRYPTION clause
- output parameters [SQL Server]
- nesting stored procedures
- user-defined stored procedures [SQL Server]
- system stored procedures [SQL Server], creating
- deferred name resolution, stored procedures
- referenced tables [SQL Server]
- global temporary procedures [SQL Server]
- cursor data type
- temporary stored procedures [SQL Server]
- size [SQL Server], stored procedures
- automatic stored procedure execution
- creating stored procedures
ms.assetid: afe3d86d-c9ab-44e4-b74d-4e3dbd9cc58c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c1c781ab64e5ec0a3dde6a298b320a3520667fd3
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237830"
---
# <a name="create-procedure-transact-sql"></a>CREATE PROCEDURE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]、[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中创建 [!INCLUDE[tsql](../../includes/tsql-md.md)] 或公共语言运行时 (CLR) 存储过程。 存储过程与其他编程语言中的过程类似，这是因为存储过程可以：

- 接受输入参数并以输出参数的格式向调用过程或批处理返回多个值。
- 包含用于在数据库中执行操作（包括调用其他过程）的编程语句。
- 向调用过程或批处理返回状态值，以指明成功或失败（以及失败的原因）。

使用此语句可以在当前数据库中创建永久过程，或者在 tempdb 数据库中创建临时程序。

> [!NOTE]
> 本主题讨论 .NET Framework CLR 与 SQL Server 的集成。 CLR 集成不适用于 Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)]。

转到[简单示例](#Simple)可跳过详细的语法信息，获取基本存储过程的简单示例。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
-- Transact-SQL Syntax for Stored Procedures in SQL Server and Azure SQL Database

CREATE [ OR ALTER ] { PROC | PROCEDURE }
    [schema_name.] procedure_name [ ; number ]
    [ { @parameter [ type_schema_name. ] data_type }
        [ VARYING ] [ = default ] [ OUT | OUTPUT | [READONLY]
    ] [ ,...n ]
[ WITH <procedure_option> [ ,...n ] ]
[ FOR REPLICATION ]
AS { [ BEGIN ] sql_statement [;] [ ...n ] [ END ] }
[;]

<procedure_option> ::=
    [ ENCRYPTION ]
    [ RECOMPILE ]
    [ EXECUTE AS Clause ]
```

```syntaxsql
-- Transact-SQL Syntax for CLR Stored Procedures

CREATE [ OR ALTER ] { PROC | PROCEDURE }
    [schema_name.] procedure_name [ ; number ]
    [ { @parameter [ type_schema_name. ] data_type }
        [ = default ] [ OUT | OUTPUT ] [READONLY]
    ] [ ,...n ]
[ WITH EXECUTE AS Clause ]
AS { EXTERNAL NAME assembly_name.class_name.method_name }
[;]
```

```syntaxsql
-- Transact-SQL Syntax for Natively Compiled Stored Procedures

CREATE [ OR ALTER ] { PROC | PROCEDURE } [schema_name.] procedure_name
    [ { @parameter data_type } [ NULL | NOT NULL ] [ = default ]
        [ OUT | OUTPUT ] [READONLY]
    ] [ ,... n ]
  WITH NATIVE_COMPILATION, SCHEMABINDING [ , EXECUTE AS clause ]
AS
{
  BEGIN ATOMIC WITH (set_option [ ,... n ] )
sql_statement [;] [ ... n ]
 [ END ]
}
 [;]

<set_option> ::=
    LANGUAGE = [ N ] 'language'
  | TRANSACTION ISOLATION LEVEL = { SNAPSHOT | REPEATABLE READ | SERIALIZABLE }
  | [ DATEFIRST = number ]
  | [ DATEFORMAT = format ]
  | [ DELAYED_DURABILITY = { OFF | ON } ]
```

```syntaxsql
-- Transact-SQL Syntax for Stored Procedures in Azure Synapse Analytics
-- and Parallel Data Warehouse

-- Create a stored procedure
CREATE { PROC | PROCEDURE } [ schema_name.] procedure_name
    [ { @parameterdata_type } [ OUT | OUTPUT ] ] [ ,...n ]
AS { [ BEGIN ] sql_statement [;][ ,...n ] [ END ] }
[;]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

OR ALTER

**适用对象**：Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)]、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 开始）。

如果过程已存在，则更改该过程。

schema_name：过程所属架构的名称。 过程是绑定到架构的。 如果在创建过程时未指定架构名称，则自动分配正在创建过程的用户的默认架构。

procedure_name：过程的名称。 过程名称必须遵循有关[标识符](../../relational-databases/databases/database-identifiers.md)的规则，并且在架构中必须唯一。

在命名过程时避免使用 sp_ 前缀。 此前缀由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用来指定系统过程。 如果存在同名的系统过程，则使用前缀可能导致应用程序代码中断。

可在 procedure_name 前面使用一个数字符号 (#procedure_name) 来创建局部临时程序，使用两个数字符号 (##procedure_name) 来创建全局临时程序  。 局部临时程序仅对创建了它的连接可见，并且在关闭该连接后将被删除。 全局临时程序可用于所有连接，并且在使用该过程的最后一个会话结束时将被删除。 对于 CLR 过程，不能指定临时名称。

过程或全局临时程序的完整名称（包括 ##）不能超过 128 个字符。 局部临时程序的完整名称（包括 #）不能超过 116 个字符。

**;** *number*

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

用于对同名的过程分组的可选整数。 使用一个 DROP PROCEDURE 语句可将这些分组过程一起删除。

> [!NOTE]
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]

带编号的过程不能使用 xml 或 CLR 用户定义类型，并且不能用于计划指南中。

@ parameter：在过程中声明的参数。 通过将 at 符号 (@) 用作第一个字符来指定参数名称。 参数名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。 每个过程的参数仅用于该过程本身；其他过程中可以使用相同的参数名称。

可声明一个或多个参数；最大值是 2,100。 除非定义了参数的默认值或者将参数设置为等于另一个参数，否则用户必须在调用过程时为每个声明的参数提供值。 如果过程包含[表值参数](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)，并且该参数在调用中缺失，则传入空表。 参数只能代替常量表达式，而不能用于代替表名、列名或其他数据库对象的名称。 有关详细信息，请参阅 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)。

如果指定了 FOR REPLICATION，则无法声明参数。

[ _type\_schema\_name_ **.** ] data_type：参数的数据类型以及该数据类型所属的架构。

针对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 过程的准则：

- 所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 数据类型都可以用作参数。
- 您可以使用用户定义的表类型创建表值参数。 表值参数只能是 INPUT 参数，并且这些参数必须带有 READONLY 关键字。 有关详细信息，请参阅[使用表值参数（数据引擎）](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)
- 游标数据类型只能是 OUTPUT 参数，并且必须带有 VARYING 关键字。

针对 CLR 过程的准则：

- 在托管代码中具有等效值的所有本机 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型都可以用作参数。 有关 CLR 类型与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型之间关系的详细信息，请参阅[映射 CLR 参数数据](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md)。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型及其语法的详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。

- 表值或游标数据类型不能用作参数。
- 如果参数的数据类型为 CLR 用户定义类型，则必须对此类型有 EXECUTE 权限。

VARYING 指定作为输出参数支持的结果集。 该参数由过程动态构造，其内容可能发生改变。 仅适用于游标参数。 该选项对于 CLR 过程无效。

default：参数的默认值。 如果为参数定义了默认值，则无需指定此参数的值即可执行过程。 默认值必须是常量或 NULL。 该常量值可以采用通配符的形式，这使其可以在将该参数传递到过程时使用 LIKE 关键字。

只有 CLR 过程的默认值记录在 sys.parameters.default 列中。 对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 过程参数，该列将为 NULL。

OUT | OUTPUT 指示参数是输出参数。 使用 OUTPUT 参数将值返回给过程的调用方。 除非是 CLR 过程，否则 text、ntext 和 image 参数不能用作 OUTPUT 参数  。 OUTPUT 参数可以为游标占位符，CLR 过程除外。 不能将表值数据类型指定为过程的 OUTPUT 参数。

READONLY 指示不能在过程的主体中更新或修改参数。 如果参数类型为表值类型，则必须指定 READONLY。

RECOMPILE 指示[!INCLUDE[ssDE](../../includes/ssde-md.md)]不缓存此过程的查询计划，这会强制在每次执行此过程时都对该过程进行编译。 有关强制重新编译的原因的详细信息，请参阅[重新编译存储过程](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md)。 在指定了 FOR REPLICATION 或者用于 CLR 过程时不能使用此选项。

若要指示[!INCLUDE[ssDE](../../includes/ssde-md.md)]放弃过程内单个查询的查询计划，请在该查询的定义中使用 RECOMPILE 查询提示。 有关详细信息，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。

ENCRYPTION

**适用对象**：SQL Server（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将 CREATE PROCEDURE 语句的原始文本转换为模糊格式。 模糊代码的输出在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的任何目录视图中都不能直接显示。 对系统表或数据库文件没有访问权限的用户不能检索模糊文本。 但是，可以通过 [DAC 端口](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)访问系统表的特权用户或直接访问数据文件的特权用户可以使用此文本。 此外，能够向服务器进程附加调试器的用户可在运行时从内存中检索已解密的过程。 有关如何访问系统元数据的详细信息，请参阅[元数据可见性配置](../../relational-databases/security/metadata-visibility-configuration.md)。

该选项对于 CLR 过程无效。

使用此选项创建的过程不能作为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 复制的一部分发布。

EXECUTE AS 子句指定在其中执行过程的安全上下文。

对于本机编译存储过程（从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始和在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中），EXECUTE AS 子句没有任何限制。 在 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中，对于本机编译的存储过程，支持 SELF、OWNER 和“user_name”子句。

有关详细信息，请参阅 [EXECUTE AS 子句 (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md)。

FOR REPLICATION

**适用对象**：SQL Server（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定为复制创建该过程。 因此，它不能在订阅服务器上执行。 使用 FOR REPLICATION 选项创建的过程可用作过程筛选器，且仅在复制过程中执行。 如果指定了 FOR REPLICATION，则无法声明参数。 对于 CLR 过程，不能指定 FOR REPLICATION。 对于使用 FOR REPLICATION 创建的过程，忽略 RECOMPILE 选项。

`FOR REPLICATION` 过程在 sys.objects 和 sys.procedures 中包含 RF 对象类型  。

{ [ BEGIN ] sql_statement [;] [ ...n ] [ END ] }： 组成过程主体的一个或多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 您可以使用可选的 BEGIN 和 END 关键字将这些语句括起来。 有关信息，请参阅后面的“最佳实践”、“一般备注”以及“限制和局限”部分。

EXTERNAL NAME _assembly\_name_ **.** _class\_name_ **.** _method\_name_

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。

指定 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 程序集的方法，以便 CLR 过程引用。 class_name 必须是有效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符，并且必须作为类存在于程序集中。 如果类包含一个使用句点 (.) 分隔命名空间各部分的限定命名空间的名称，则必须使用方括号 ([]) 或引号 ("") 将类名称分隔开  。 指定的方法必须为该类的静态方法。

默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不能执行 CLR 代码。 可以创建、修改和删除引用公共语言运行时模块的数据库对象；不过，只有在启用 [clr enabled 选项](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md)之后，才能在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中执行这些引用。 若要启用该选项，请使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)。

> [!NOTE]
> 包含数据库中不支持 CLR 过程。

ATOMIC WITH

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指示执行原子存储过程。 更改提交或所有更改通过引发异常回滚。 ATOMIC WITH 块对于本机编译存储过程是必需的。

如果过程（通过 RETURN 语句显式或者通过完成执行隐式）返回，则提交过程所执行的工作。 如果过程引发，则过程所执行的工作将回滚。

默认情况下，XACT_ABORT 在原子块内为 ON，并且不能更改。 XACT_ABORT 指定当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句出现运行时错误时，[!INCLUDE[tsql](../../includes/tsql-md.md)] 是否自动回滚当前事务。

 以下 SET 选项在 ATOMIC 块中始终为 ON；该选项不能更改。

- CONCAT_NULL_YIELDS_NULL
- QUOTED_IDENTIFIER, ARITHABORT
- NOCOUNT
- ANSI_NULLS
- ANSI_WARNINGS

SET 选项不能在 ATOMIC 块内更改。 用户会话中的 SET 选项不在本机编译存储过程的范围内使用。 这些选项在编译时是固定的。

BEGIN、ROLLBACK 和 COMMIT 操作无法在原子块内使用。

每个本机编译存储过程的外层作用域都有一个 ATOMIC 块。 这些块不能嵌套。 有关 ATOMIC 块的详细信息，请参阅[本机编译的存储过程](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md)。

NULL | NOT NULL 确定参数中是否允许使用 null 值。 NULL 是默认值。

NATIVE_COMPILATION

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指示过程已本机编译。 NATIVE_COMPILATION、SCHEMABINDING 和 EXECUTE AS 可以按任意顺序指定。 有关详细信息，请参阅[本机编译的存储过程](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md)。

SCHEMABINDING

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

确保过程引用的表不能删除或修改。 SCHEMABINDING 在本机编译存储过程中是必需的。 （有关详细信息，请参阅[本机编译的存储过程](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md)。）SCHEMABINDING 限制与其对用户定义的函数的限制是相同的。 有关详细信息，请参阅 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md) 中的 SCHEMABINDING 部分。

LANGUAGE = [N] 'language'

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

等效于 [SET LANGUAGE (Transact-SQL)](../../t-sql/statements/set-language-transact-sql.md) 会话选项。 LANGUAGE = [N] 'language' 是必须的。

TRANSACTION ISOLATION LEVEL

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

对于本机编译存储过程是必需的。 指定存储过程的事务隔离级别。 选项如下：

有关这些选项的详细信息，请参阅 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。

REPEATABLE READ 指定语句不能读取其他事务已修改但尚未提交的数据。 如果另一个事务修改由当前事务读取的数据，当前事务将失败。

SERIALIZABLE 指定以下内容：

- 语句不能读取已由其他事务修改但尚未提交的数据。
- 如果其他事务修改由当前事务读取的数据，当前事务将失败。
- 如果另一个事务插入的新行具有的键值在当前事务中任何语句读取的键范围内，当前事务将失败。

SNAPSHOT 指定事务中任何语句读取的数据都是事务开始时便存在的数据的事务上一致的版本。

DATEFIRST = *number*

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

将一周的第一天指定为 1 到 7 中的一个数字。 DATEFIRST 是可选的。 如果未指定，该设置从指定语言进行推断。

有关详细信息，请参阅 [SET DATEFIRST (Transact-SQL)](../../t-sql/statements/set-datefirst-transact-sql.md).

DATEFORMAT = format

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定用于解释 date、smalldatetime、datetime、datetime2 和 datetimeoffset 字符串的月、日和年日期部分的顺序。 DATEFORMAT 是可选的。 如果未指定，该设置从指定语言进行推断。

有关详细信息，请参阅 [SET DATEFORMAT (Transact-SQL)](../../t-sql/statements/set-dateformat-transact-sql.md).

DELAYED_DURABILITY = { OFF | ON }

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事务提交可以是完全持久、默认或延迟的持久。

有关详细信息，请参阅[控制事务持续性](../../relational-databases/logs/control-transaction-durability.md)。

## <a name="simple-examples"></a><a name="Simple"></a> 简单示例

为帮助快速入门，这里提供以下两个简单示例：`SELECT DB_NAME() AS ThisDB;` 返回当前数据库的名称。
可将该语句包装在存储过程中，例如：

```sql
CREATE PROC What_DB_is_this
AS
SELECT DB_NAME() AS ThisDB;
```

使用后列语句调用存储过程：`EXEC What_DB_is_this;`

稍微复杂一点的情况是提供一个输入参数以使过程更灵活。 例如：

```sql
CREATE PROC What_DB_is_that @ID INT
AS
SELECT DB_NAME(@ID) AS ThatDB;
```

调用过程时，请提供数据库 ID 号。 例如，`EXEC What_DB_is_that 2;` 返回 `tempdb`。

若要查看更多示例，请参阅本主题末尾的[示例](#Examples)。

## <a name="best-practices"></a>最佳实践

尽管并未列出所有最佳做法，这些建议还是可以提高过程性能。

- 使用 SET NOCOUNT ON 语句作为过程主体中的第一个语句。 也就是说，将其放置于紧接着 AS 关键字之后。 这会禁止显示在执行任何 SELECT、INSERT、UPDATE、MERGE 和 DELETE 语句后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发送回客户端的消息。 这可确保最大限度地减少生成的输出内容，让人一目了然。 不过，对于当前的硬件，没有明显的性能优势。 有关信息，请参阅 [SET NOCOUNT (Transact-SQL)](../../t-sql/statements/set-nocount-transact-sql.md)。
- 当在过程中创建或引用数据库对象时使用架构名称。 如果不需要搜索多个架构，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 解析对象名称时所需的处理时间会更少。 它还可以防止在未指定架构的情况下，创建对象期间分配用户默认架构时导致的权限和访问问题。
- 避免函数包装在 WHERE 和 JOIN 子句中指定的列。 这样做会使列具有不确定性并且禁止查询处理器使用索引。
- 避免在返回许多行数据的 SELECT 语句中使用标量函数。 因为标量函数必须应用于每一行，所以最终导致的行为将类似于基于行的处理并且会降低性能。
- 请避免使用 `SELECT *`。 而是应指定所需的列名。 这样做可以避免停止过程执行的[!INCLUDE[ssDE](../../includes/ssde-md.md)]错误。 例如，返回 12 列的表中的数据然后将数据插入 12 列临时表中的 `SELECT *` 语句将成功（在二表中任一表中列的数量或顺序发生更改之前）。
- 避免处理或返回过多的数据。 尽可能在过程代码中缩小结果的范围，这样，该过程执行的任何后续操作都将使用可能的最小数据集完成。 仅将基本数据发送到客户端应用程序。 它比跨网络发送多余的数据并且强制客户端应用程序处理不必要的大结果集更高效。
- 通过使用 BEGIN/COMMIT TRANSACTION 使用显式事务，使事务尽可能短。 更长的事务意味着更长的记录锁定和更高的死锁风险。
- 使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] TRY…CATCH 功能进行过程内的错误处理。 TRY…CATCH 可以封装整个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句块。 这不仅产生更少的性能开销，还通过显著减少的编程，使错误报告更精确。
- 在过程主体中对 CREATE TABLE 或 ALTER TABLE [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句引用的所有表列使用 DEFAULT 关键字。 这会禁止将 NULL 传递到不允许 Null 值的列。
- 对于临时表中的每一列使用 NULL 或 NOT NULL。 如果在 CREATE TABLE 或 ALTER TABLE 语句中未进行指定，则 ANSI_DFLT_ON 和 ANSI_DFLT_OFF 选项将控制[!INCLUDE[ssDE](../../includes/ssde-md.md)]为列指派 NULL 或 NOT NULL 属性的方式。 如果某个连接执行的过程对这些选项的设置与创建该过程的连接的设置不同，则为第二个连接创建的表列可能会有不同的为 Null 性，并且表现出不同的行为。 如果为每个列显式声明了 NULL 或 NOT NULL，那么将对所有执行该过程的连接使用相同的为 Null 性创建临时表。
- 使用将转换 Null 的修改语句并且包括从查询中删除含 Null 值的行的逻辑。 请注意，在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中，NULL 不是空或者“无意义”值。 它是针对未知值的占位符并且可能导致意外的行为，特别是在查询结果集或使用 AGGREGATE 函数时。
- 使用 UNION ALL 运算符来代替 UNION 或 OR 运算符，除非存在针对非重复值的特定需要。 UNION ALL 运算符要求更少的处理开销，因为重复值不从结果集中筛选出来。

## <a name="general-remarks"></a>一般备注

一个过程没有预定义的最大大小。

在过程中指定的变量可以是用户定义变量或系统变量，如 @@SPID。

第一次执行某个过程时，将编译该过程以确定检索数据的最优访问计划。 如果已经生成的计划仍保留在[!INCLUDE[ssDE](../../includes/ssde-md.md)]计划缓存中，则该过程随后执行的操作可能重新使用该计划。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动时可以自动执行一个或多个过程。 这些过程必须由系统管理员在 master 数据库中创建，并以 sysadmin 固定服务器角色作为后台进程执行 。 这些过程不能有任何输入或输出参数。 有关详细信息，请参阅[执行存储过程](../../relational-databases/stored-procedures/execute-a-stored-procedure.md)。

当一个过程通过引用 CLR 例程、类型或聚合来调用另一个过程或执行托管代码时，过程将被嵌套。 过程和托管代码引用的嵌套最高可达 32 级。 每当调用的过程或托管代码引用开始执行，嵌套级别就增加一级；执行完成后，嵌套级别就减少一级。 从托管代码内部调用的方法不根据嵌套级别限制进行计数。 但是，当一个 CLR 存储过程通过 SQL Server 托管访问接口执行数据访问操作时，在从托管代码到 SQL 的转换中将添加一级嵌套。

试图超过最高级的嵌套将导致整个调用链失败。 可以使用 @@NESTLEVEL 函数返回当前存储过程执行的嵌套级别。

## <a name="interoperability"></a>互操作性

在创建或修改 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 过程时，[!INCLUDE[tsql](../../includes/tsql-md.md)]将保存 SET QUOTED_IDENTIFIER 和 SET ANSI_NULLS 的设置。 执行过程时，将使用这些原始设置。 因此，所有客户端会话的 SET QUOTED_IDENTIFIER 和 SET ANSI_NULLS 设置在执行过程时都将被忽略。

在创建或更改过程时不保存其他 SET 选项（例如 SET ARITHABORT、SET ANSI_WARNINGS 或 SET ANSI_PADDINGS）。 如果过程的逻辑取决于特定的设置，则应在过程开头添加一条 SET 语句，以确保设置正确。 从过程中执行 SET 语句时，该设置只在过程完成之前有效。 之后，设置将还原为调用过程时的值。 这样一来，单个客户端就可以设置所需的选项，而不会影响过程的逻辑。

可以在过程中指定除了 SET SHOWPLAN_TEXT 和 SET SHOWPLAN_ALL 以外的任何 SET 语句。 这些语句在批处理中必须唯一。 选择的 SET 选项在过程执行过程中有效，之后恢复为原来的设置。

> [!NOTE]
> 在过程和用户定义函数中传递参数，或者在批处理语句中声明和设置变量时，不执行 SET ANSI_WARNINGS。 例如，如果将一个变量定义为 char(3)，然后将其值设置为大于三个字符，则数据将被截断为定义的大小，INSERT 或 UPDATE 语句可以成功执行。

## <a name="limitations-and-restrictions"></a>限制和局限

在单个批处理中，CREATE PROCEDURE 语句不能与其他 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句组合使用。

以下语句不能用于存储过程主体中的任何地方。

| CREATE | SET | USE |
|--------|-----|-----|
| CREATE AGGREGATE | SET SHOWPLAN_TEXT | USE database_name|
| CREATE DEFAULT | SET SHOWPLAN_XML
| CREATE RULE | SET PARSEONLY |
| CREATE SCHEMA | SET SHOWPLAN_ALL |
| CREATE 或 ALTER TRIGGER |
| CREATE 或 ALTER FUNCTION |
| CREATE 或 ALTER PROCEDURE |
| CREATE 或 ALTER VIEW |

 过程可以引用尚不存在的表。 在创建时，只进行语法检查。 直到第一次执行该过程时才对其进行编译。 只有在编译过程中才解析过程中引用的所有对象。 因此，如果语法正确的过程引用了不存在的表，则仍可以成功创建；但如果被引用的表不存在，则过程将在执行时将失败。

 不能将某一函数名称指定为参数默认值或者在执行过程时传递给参数的值。 但是，您可以将函数作为变量传递，如以下示例中所示：

```sql
-- Passing the function value as a variable.
DECLARE @CheckDate DATETIME = GETDATE();
EXEC dbo.uspGetWhereUsedProductID 819, @CheckDate;
GO
```

如果该过程对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的远程实例进行更改，将无法回滚这些更改。 远程过程不参与事务。

为了使[!INCLUDE[ssDE](../../includes/ssde-md.md)]在 .NET Framework 中被重载时引用正确的方法，EXTERNAL NAME 子句中指定的方法必须具有下列特征：

- 声明为静态方法。
- 接收的参数个数与过程的参数个数相同。
- 使用的参数类型与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 过程的相应参数的数据类型兼容。 有关将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型与 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据类型匹配的信息，请参阅[映射 CLR 参数数据](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md)。

## <a name="metadata"></a>元数据

下表列出了可用于返回有关存储过程的信息的目录视图和动态管理视图。

|查看|说明|
|----------|-----------------|
|[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)|返回 [!INCLUDE[tsql](../../includes/tsql-md.md)] 过程的定义。 使用 ENCRYPTION 选项创建的过程的文本不能使用 sys.sql_modules 目录视图查看。|
|[sys.assembly_modules](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)|返回有关 CLR 过程的信息。|
|[sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)|返回有关在过程中定义的参数的信息。|
|[sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)|返回过程引用的对象。|

若要估计编译后的过程大小，请使用下列性能监视器计数器。

|性能监视器对象名|性能监视器计数器名称|
|-------------------------------------|--------------------------------------|
|SQLServer：Plan Cache 对象|缓存命中率|
||Cache Pages|
||Cache Object Counts*|

*各种类别的缓存对象均可以使用这些计数器，包括即席 [!INCLUDE[tsql](../../includes/tsql-md.md)]、准备好的 [!INCLUDE[tsql](../../includes/tsql-md.md)]、过程、触发器等。 有关详细信息，请参阅 [SQL Server 计划缓存对象](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md)。

## <a name="security"></a>安全性

### <a name="permissions"></a>权限

要求数据库中的 CREATE PROCEDURE 权限以及对要在其中创建过程的架构的 ALTER 权限，或者要求 db_ddladmin 固定数据库角色中的成员身份  。

对于 CLR 存储过程，需要 EXTERNAL NAME 子句中引用的程序集的所有权，或者该程序集的 REFERENCES 权限。

## <a name="create-procedure-and-memory-optimized-tables"></a><a name="mot"></a> CREATE PROCEDURE 和内存优化表

可以通过传统和本机编译存储过程访问内存优化表。 大多数情况下，本机过程是更高效的方式。 有关详细信息，请参阅[本机编译的存储过程](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md)。

下面的示例演示如何创建访问内存优化表 `dbo.Departments` 的本机编译存储过程：

```sql
CREATE PROCEDURE dbo.usp_add_kitchen @dept_id INT, @kitchen_count INT NOT NULL
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION
AS
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')

UPDATE dbo.Departments
SET kitchen_count = ISNULL(kitchen_count, 0) + @kitchen_count
WHERE ID = @dept_id
END;
GO
```

 未使用 NATIVE_COMPILATION 创建的过程不能更改为本机编译存储过程。

 有关本机编译的存储过程中的可编程性、支持的查询外围应用和运算符的论述，请参阅[本机编译的 T-SQL 模块支持的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)。

## <a name="examples"></a><a name="Examples"></a> 示例

|类别|作为特征的语法元素|
|--------------|------------------------------|
|[基本语法](#BasicSyntax)|CREATE PROCEDURE|
|[传递参数](#Parameters)|@parameter <br> &nbsp;&nbsp;• = default <br> &nbsp;&nbsp; • OUTPUT <br> &nbsp;&nbsp; • 表值参数类型 <br> &nbsp;&nbsp; • CURSOR VARYING|
|[使用存储过程修改数据](#Modify)|UPDATE|
|[错误处理](#Error)|TRY...CATCH|
|[对过程定义进行模糊处理](#Encrypt)|WITH ENCRYPTION|
|[强制过程重新编译](#Recompile)|WITH RECOMPILE|
|[设置安全性上下文](#Security)|EXECUTE AS|

### <a name="basic-syntax"></a><a name="BasicSyntax"></a> 基本语法

此节中的示例说明了使用最低要求的语法的 CREATE PROCEDURE 语句的基本功能。

#### <a name="a-creating-a-simple-transact-sql-procedure"></a>A. 创建简单 Transact-SQL 过程

以下示例将创建一个存储过程，该存储过程将从 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的一个视图中返回所有雇员（提供姓和名）、职务以及部门名称。 此过程不使用任何参数。 该示例然后说明执行此过程的三个方法。

```sql
CREATE PROCEDURE HumanResources.uspGetAllEmployees
AS
    SET NOCOUNT ON;
    SELECT LastName, FirstName, JobTitle, Department
    FROM HumanResources.vEmployeeDepartment;
GO

SELECT * FROM HumanResources.vEmployeeDepartment;
```

可以通过以下方式执行 `uspGetEmployees` 过程：

```sql
EXECUTE HumanResources.uspGetAllEmployees;
GO
-- Or
EXEC HumanResources.uspGetAllEmployees;
GO
-- Or, if this procedure is the first statement within a batch:
HumanResources.uspGetAllEmployees;
```

#### <a name="b-returning-more-than-one-result-set"></a>B. 返回多个结果集

以下过程返回两个结果集。

```sql
CREATE PROCEDURE dbo.uspMultipleResults
AS
SELECT TOP(10) BusinessEntityID, Lastname, FirstName FROM Person.Person;
SELECT TOP(10) CustomerID, AccountNumber FROM Sales.Customer;
GO
```

#### <a name="c-creating-a-clr-stored-procedure"></a>C. 创建 CLR 存储过程

以下示例将创建 `GetPhotoFromDB` 过程，此过程引用 `HandlingLOBUsingCLR` 程序集中的 `LargeObjectBinary` 类的 `GetPhotoFromDB` 方法。 在创建过程之前，已在本地数据库中注册 `HandlingLOBUsingCLR` 程序集。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]、（如果使用从 assembly_bits 创建的程序集）。

```sql
CREATE ASSEMBLY HandlingLOBUsingCLR
FROM '\\MachineName\HandlingLOBUsingCLR\bin\Debug\HandlingLOBUsingCLR.dll';
GO
CREATE PROCEDURE dbo.GetPhotoFromDB
(
    @ProductPhotoID INT
    , @CurrentDirectory NVARCHAR(1024)
    , @FileName NVARCHAR(1024)
)
AS EXTERNAL NAME HandlingLOBUsingCLR.LargeObjectBinary.GetPhotoFromDB;
GO
```

### <a name="passing-parameters"></a><a name="Parameters"></a> 传递参数

此节中的示例说明如何使用输入参数和输出参数将值传递给存储过程以及从存储过程传递值。

#### <a name="d-creating-a-procedure-with-input-parameters"></a>D. 创建带有输入参数的过程

以下示例将创建一个存储过程，此过程通过传递特定雇员的名和姓的值来返回该雇员的信息。 此过程仅接受与传递的参数精确匹配的值。

```sql
IF OBJECT_ID ( 'HumanResources.uspGetEmployees', 'P' ) IS NOT NULL
    DROP PROCEDURE HumanResources.uspGetEmployees;
GO
CREATE PROCEDURE HumanResources.uspGetEmployees
    @LastName NVARCHAR(50),
    @FirstName NVARCHAR(50)
AS

    SET NOCOUNT ON;
    SELECT FirstName, LastName, JobTitle, Department
    FROM HumanResources.vEmployeeDepartment
    WHERE FirstName = @FirstName AND LastName = @LastName;
GO

```

可以通过以下方式执行 `uspGetEmployees` 过程：

```sql
EXECUTE HumanResources.uspGetEmployees N'Ackerman', N'Pilar';
-- Or
EXEC HumanResources.uspGetEmployees @LastName = N'Ackerman', @FirstName = N'Pilar';
GO
-- Or
EXECUTE HumanResources.uspGetEmployees @FirstName = N'Pilar', @LastName = N'Ackerman';
GO
-- Or, if this procedure is the first statement within a batch:
HumanResources.uspGetEmployees N'Ackerman', N'Pilar';

```

#### <a name="e-using-a-procedure-with-wildcard-parameters"></a>E. 使用带有通配符参数的过程

以下示例将创建一个存储过程，此过程通过传递雇员的名和姓的完整值或部分值来返回雇员的信息。 此过程模式匹配传递的参数，或者如果未提供，使用预设的默认值（以字母 `D` 开头的姓氏）。

```sql
IF OBJECT_ID ( 'HumanResources.uspGetEmployees2', 'P' ) IS NOT NULL
    DROP PROCEDURE HumanResources.uspGetEmployees2;
GO
CREATE PROCEDURE HumanResources.uspGetEmployees2
    @LastName NVARCHAR(50) = N'D%',
    @FirstName NVARCHAR(50) = N'%'
AS
    SET NOCOUNT ON;
    SELECT FirstName, LastName, JobTitle, Department
    FROM HumanResources.vEmployeeDepartment
    WHERE FirstName LIKE @FirstName AND LastName LIKE @LastName;
```

`uspGetEmployees2` 过程可以以多种组合方式执行。 此处只列出了几个可能的组合。

```sql
EXECUTE HumanResources.uspGetEmployees2;
-- Or
EXECUTE HumanResources.uspGetEmployees2 N'Wi%';
-- Or
EXECUTE HumanResources.uspGetEmployees2 @FirstName = N'%';
-- Or
EXECUTE HumanResources.uspGetEmployees2 N'[CK]ars[OE]n';
-- Or
EXECUTE HumanResources.uspGetEmployees2 N'Hesse', N'Stefen';
-- Or
EXECUTE HumanResources.uspGetEmployees2 N'H%', N'S%';
```

#### <a name="f-using-output-parameters"></a>F. 使用 OUTPUT 参数

以下示例将创建 `uspGetList` 过程。 此过程将返回价格不超过指定数值的产品的列表。 此示例显示如何使用多个 `SELECT` 语句和多个 `OUTPUT` 参数。 OUTPUT 参数允许外部过程、批处理或多条 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在过程执行期间访问设置的某个值。

```sql
IF OBJECT_ID ( 'Production.uspGetList', 'P' ) IS NOT NULL
    DROP PROCEDURE Production.uspGetList;
GO  
CREATE PROCEDURE Production.uspGetList @Product VARCHAR(40)
    , @MaxPrice MONEY
    , @ComparePrice MONEY OUTPUT
    , @ListPrice MONEY OUT
AS  
    SET NOCOUNT ON;
    SELECT p.[Name] AS Product, p.ListPrice AS 'List Price'
    FROM Production.Product AS p
    JOIN Production.ProductSubcategory AS s
      ON p.ProductSubcategoryID = s.ProductSubcategoryID
    WHERE s.[Name] LIKE @Product AND p.ListPrice < @MaxPrice;
-- Populate the output variable @ListPprice.
SET @ListPrice = (SELECT MAX(p.ListPrice)
    FROM Production.Product AS p
    JOIN Production.ProductSubcategory AS s
      ON p.ProductSubcategoryID = s.ProductSubcategoryID
    WHERE s.[Name] LIKE @Product AND p.ListPrice < @MaxPrice);
-- Populate the output variable @compareprice.
SET @ComparePrice = @MaxPrice;
GO
```

执行 `uspGetList`，返回价格低于 `$700` 的 [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] 产品（自行车）的列表。 `OUTPUT` 参数 `@Cost` 和 `@ComparePrices` 用于控制流语言，用于在“消息”窗口中返回消息。

> [!NOTE]
> OUTPUT 变量必须在创建过程时或使用变量时定义。 参数名称和变量名称不一定要匹配，但是，除非使用 `@ListPrice` = variable，否则数据类型和参数定位必须匹配。

```sql
DECLARE @ComparePrice MONEY, @Cost MONEY;
EXECUTE Production.uspGetList '%Bikes%', 700,
    @ComparePrice OUT,
    @Cost OUTPUT
IF @Cost <= @ComparePrice
BEGIN
    PRINT 'These products can be purchased for less than
    $'+RTRIM(CAST(@ComparePrice AS VARCHAR(20)))+'.'
END
ELSE
    PRINT 'The prices for all products in this category exceed
    $'+ RTRIM(CAST(@ComparePrice AS VARCHAR(20)))+'.';
```

下面是部分结果集：

```
Product                     List Price
--------------------------  ----------
Road-750 Black, 58          539.99
Mountain-500 Silver, 40     564.99
Mountain-500 Silver, 42     564.99
...
Road-750 Black, 48          539.99
Road-750 Black, 52          539.99

(14 row(s) affected)

These items can be purchased for less than $700.00.
```

#### <a name="g-using-a-table-valued-parameter"></a>G. 使用表值参数

以下示例使用表值参数类型将多个行插入表中。 该示例将创建参数类型，声明表变量来引用它，填充参数列表，然后将值传递给存储过程。 存储过程使用这些值将多个行插入表中。

```sql
/* Create a table type. */
CREATE TYPE LocationTableType AS TABLE
( LocationName VARCHAR(50)
, CostRate INT );
GO

/* Create a procedure to receive data for the table-valued parameter. */
CREATE PROCEDURE usp_InsertProductionLocation
    @TVP LocationTableType READONLY
    AS
    SET NOCOUNT ON
    INSERT INTO [AdventureWorks2012].[Production].[Location]
       ([Name]
       , [CostRate]
       , [Availability]
       , [ModifiedDate])
    SELECT *, 0, GETDATE()
    FROM @TVP;
GO

/* Declare a variable that references the type. */
DECLARE @LocationTVP
AS LocationTableType;

/* Add data to the table variable. */
INSERT INTO @LocationTVP (LocationName, CostRate)
    SELECT [Name], 0.00
    FROM
    [AdventureWorks2012].[Person].[StateProvince];

/* Pass the table variable data to a stored procedure. */
EXEC usp_InsertProductionLocation @LocationTVP;
GO
```

##### <a name="h-using-an-output-cursor-parameter"></a>H. 使用 OUTPUT 游标参数

以下示例使用 OUTPUT 游标参数将过程的局部游标传递回执行调用的批处理、过程或触发器。

首先，创建在 `Currency` 表上声明并打开一个游标的过程：

```sql
CREATE PROCEDURE dbo.uspCurrencyCursor
    @CurrencyCursor CURSOR VARYING OUTPUT
AS
    SET NOCOUNT ON;
    SET @CurrencyCursor = CURSOR
    FORWARD_ONLY STATIC FOR
      SELECT CurrencyCode, Name
      FROM Sales.Currency;
    OPEN @CurrencyCursor;
GO
```

接下来，运行以下批处理：声明一个局部游标变量，执行上述过程以将游标赋值给局部变量，然后从该游标提取行。

```sql
DECLARE @MyCursor CURSOR;
EXEC dbo.uspCurrencyCursor @CurrencyCursor = @MyCursor OUTPUT;
WHILE (@@FETCH_STATUS = 0)
BEGIN;
    FETCH NEXT FROM @MyCursor;
END;
CLOSE @MyCursor;
DEALLOCATE @MyCursor;
GO
```

### <a name="modifying-data-by-using-a-stored-procedure"></a><a name="Modify"></a> 使用存储过程修改数据

此节中的示例说明如何通过在过程定义中包含数据操作语言 (DML) 语句，在表或视图中插入或修改数据。

#### <a name="i-using-update-in-a-stored-procedure"></a>I. 在存储过程中使用 UPDATE

下面的示例在一个存储过程中使用了 UPDATE 语句。 该过程采用一个输入参数 `@NewHours` 和一个输出参数 `@RowCount`。 在 UPDATE 语句中使用 `@NewHours` 参数值来更新表 `HumanResources.Employee` 中的列 `VacationHours`。 `@RowCount` 输出参数用于将影响的行数返回给一个局部变量。 在 SET 子句中使用 CASE 表达式，以便按条件确定为 `VacationHours` 设置的值。 在按每小时向员工付薪时 (`SalariedFlag` = 0)，`VacationHours` 设置为当前小时数加上 `@NewHours` 中指定的值；否则，`VacationHours` 设置为在 `@NewHours` 中指定的值。

```sql
CREATE PROCEDURE HumanResources.Update_VacationHours
@NewHours SMALLINT, @Rowcount INT OUTPUT
AS
SET NOCOUNT ON;
UPDATE HumanResources.Employee
SET VacationHours =
    ( CASE
        WHEN SalariedFlag = 0 THEN VacationHours + @NewHours
        ELSE @NewHours
        END
    )
WHERE CurrentFlag = 1;
SET @Rowcount = @@rowcount;

GO
DECLARE @Rowcount INT
EXEC HumanResources.Update_VacationHours 40, @Rowcount OUTPUT
PRINT @Rowcount;
```

### <a name="error-handling"></a><a name="Error"></a> 错误处理

此节中的示例介绍一些方法，这些方法用于处理在执行存储过程时可能出现的错误。

#### <a name="j-using-trycatch"></a>J. 使用 TRY...CATCH

以下示例使用 TRY…CATCH 构造返回在执行存储过程期间捕获的错误信息。

```sql
CREATE PROCEDURE Production.uspDeleteWorkOrder ( @WorkOrderID INT )
AS
SET NOCOUNT ON;
BEGIN TRY
  BEGIN TRANSACTION
  -- Delete rows from the child table, WorkOrderRouting, for the specified work order.
    DELETE FROM Production.WorkOrderRouting
    WHERE WorkOrderID = @WorkOrderID;
  -- Delete the rows from the parent table, WorkOrder, for the specified work order.
    DELETE FROM Production.WorkOrder
    WHERE WorkOrderID = @WorkOrderID;
  COMMIT
END TRY

BEGIN CATCH
  -- Determine if an error occurred.
  IF @@TRANCOUNT > 0
    ROLLBACK

  -- Return the error information.
  DECLARE @ErrorMessage NVARCHAR(4000), @ErrorSeverity INT;
  SELECT @ErrorMessage = ERROR_MESSAGE(),@ErrorSeverity = ERROR_SEVERITY();
  RAISERROR(@ErrorMessage, @ErrorSeverity, 1);
END CATCH;

GO
EXEC Production.uspDeleteWorkOrder 13;

/* Intentionally generate an error by reversing the order in which rows
   are deleted from the parent and child tables. This change does not
   cause an error when the procedure definition is altered, but produces
   an error when the procedure is executed.
*/
ALTER PROCEDURE Production.uspDeleteWorkOrder ( @WorkOrderID INT )
AS

BEGIN TRY
  BEGIN TRANSACTION
  -- Delete the rows from the parent table, WorkOrder, for the specified work order.
    DELETE FROM Production.WorkOrder
    WHERE WorkOrderID = @WorkOrderID;

  -- Delete rows from the child table, WorkOrderRouting, for the specified work order.
    DELETE FROM Production.WorkOrderRouting
    WHERE WorkOrderID = @WorkOrderID;
  COMMIT TRANSACTION
END TRY

BEGIN CATCH
  -- Determine if an error occurred.
  IF @@TRANCOUNT > 0
    ROLLBACK TRANSACTION

  -- Return the error information.
  DECLARE @ErrorMessage NVARCHAR(4000), @ErrorSeverity INT;
  SELECT @ErrorMessage = ERROR_MESSAGE(),@ErrorSeverity = ERROR_SEVERITY();
  RAISERROR(@ErrorMessage, @ErrorSeverity, 1);
END CATCH;
GO
-- Execute the altered procedure.
EXEC Production.uspDeleteWorkOrder 15;

DROP PROCEDURE Production.uspDeleteWorkOrder;
```

### <a name="obfuscating-the-procedure-definition"></a><a name="Encrypt"></a> 对过程定义进行模糊处理

此节中的示例说明如何对存储过程定义进行模糊处理。

#### <a name="k-using-the-with-encryption-option"></a>K. 使用 WITH ENCRYPTION 选项

以下示例将创建 `HumanResources.uspEncryptThis` 过程。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、SQL 数据库。

```sql
CREATE PROCEDURE HumanResources.uspEncryptThis
WITH ENCRYPTION
AS
    SET NOCOUNT ON;
    SELECT BusinessEntityID, JobTitle, NationalIDNumber,
        VacationHours, SickLeaveHours
    FROM HumanResources.Employee;
GO
```

查询系统目录或使用元数据函数时，`WITH ENCRYPTION` 选项对过程定义进行模糊处理，如下面的示例所示。

运行 `sp_helptext`：

```sql
EXEC sp_helptext 'HumanResources.uspEncryptThis';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

`The text for object 'HumanResources.uspEncryptThis' is encrypted.`

直接查询 `sys.sql_modules` 目录视图：

```sql
SELECT definition FROM sys.sql_modules
WHERE object_id = OBJECT_ID('HumanResources.uspEncryptThis');
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
definition
--------------------------------
NULL
```

### <a name="forcing-the-procedure-to-recompile"></a><a name="Recompile"></a>强制过程重新编译

此节中的示例使用 WITH RECOMPILE 子句强制过程在每次执行时进行重新翻译。

#### <a name="l-using-the-with-recompile-option"></a>L. 使用 WITH RECOMPILE 选项

如果提供给过程的参数不标准，以及如果新的执行计划不应被缓存或存储在内存中时，`WITH RECOMPILE` 子句很有用。

```sql
IF OBJECT_ID ( 'dbo.uspProductByVendor', 'P' ) IS NOT NULL
    DROP PROCEDURE dbo.uspProductByVendor;
GO
CREATE PROCEDURE dbo.uspProductByVendor @Name VARCHAR(30) = '%'
WITH RECOMPILE
AS
    SET NOCOUNT ON;
    SELECT v.Name AS 'Vendor name', p.Name AS 'Product name'
    FROM Purchasing.Vendor AS v
    JOIN Purchasing.ProductVendor AS pv
      ON v.BusinessEntityID = pv.BusinessEntityID
    JOIN Production.Product AS p
      ON pv.ProductID = p.ProductID
    WHERE v.Name LIKE @Name;
```

### <a name="setting-the-security-context"></a><a name="Security"></a> 设置安全性上下文

此节中的示例使用 EXECUTE AS 子句设置执行存储过程的安全上下文。

#### <a name="m-using-the-execute-as-clause"></a>M. 使用 EXECUTE AS 子句

下面的示例演示使用 [EXECUTE AS](../../t-sql/statements/execute-as-clause-transact-sql.md) 子句指定可以在其中执行过程的安全性上下文。 在该示例中，`CALLER` 选项指定该过程可在调用过程的用户的上下文中执行。

```sql
CREATE PROCEDURE Purchasing.uspVendorAllInfo
WITH EXECUTE AS CALLER
AS
    SET NOCOUNT ON;
    SELECT v.Name AS Vendor, p.Name AS 'Product name',
      v.CreditRating AS 'Rating',
      v.ActiveFlag AS Availability
    FROM Purchasing.Vendor v
    INNER JOIN Purchasing.ProductVendor pv
      ON v.BusinessEntityID = pv.BusinessEntityID
    INNER JOIN Production.Product p
      ON pv.ProductID = p.ProductID
    ORDER BY v.Name ASC;
GO
```

#### <a name="n-creating-custom-permission-sets"></a>N. 创建自定义权限集

以下示例使用 EXECUTE AS 为数据库操作创建自定义权限。 某些操作（如 TRUNCATE TABLE）没有可授予的权限。 通过将 TRUNCATE TABLE 语句合并到存储过程中并指定该过程作为一个有权修改表的用户执行，您可以将截断表的权限扩展至授予其对过程的 EXECUTE 权限的用户。

```sql
CREATE PROCEDURE dbo.TruncateMyTable
WITH EXECUTE AS SELF
AS TRUNCATE TABLE MyDB..MyTable;
```

## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="o-create-a-stored-procedure-that-runs-a-select-statement"></a>O. 创建运行 SELECT 语句的存储过程

此示例演示用于创建和运行过程的基本语法。 运行批处理时，CREATE PROCEDURE 必须是第一个语句。 例如，要在 [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)] 中创建以下存储过程，首先要设置数据库上下文，然后运行 CREATE PROCEDURE 语句。

```sql
-- Uses AdventureWorksDW database

--Run CREATE PROCEDURE as the first statement in a batch.
CREATE PROCEDURE Get10TopResellers
AS
BEGIN
    SELECT TOP (10) r.ResellerName, r.AnnualSales
    FROM DimReseller AS r
    ORDER BY AnnualSales DESC, ResellerName ASC;
END
;
GO

--Show 10 Top Resellers
EXEC Get10TopResellers;
```

## <a name="see-also"></a>另请参阅

- [ALTER PROCEDURE (Transact-SQL)](../../t-sql/statements/alter-procedure-transact-sql.md)
- [控制流语言 (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)
- [游标](../../relational-databases/cursors.md)
- [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)
- [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [DROP PROCEDURE (Transact-SQL)](../../t-sql/statements/drop-procedure-transact-sql.md)
- [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)
- [EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-transact-sql.md)
- [存储过程（数据库引擎）](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)
- [sp_procoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md)
- [sp_recompile (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md)
- [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)
- [sys.parameters (Transact-SQL)](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)
- [sys.procedures (Transact-SQL)](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)
- [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)
- [sys.assembly_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)
- [sys.numbered_procedures (Transact-SQL)](../../relational-databases/system-catalog-views/sys-numbered-procedures-transact-sql.md)
- [sys.numbered_procedure_parameters (Transact-SQL)](../../relational-databases/system-catalog-views/sys-numbered-procedure-parameters-transact-sql.md)
- [OBJECT_DEFINITION (Transact-SQL)](../../t-sql/functions/object-definition-transact-sql.md)
- [创建存储过程](../../relational-databases/stored-procedures/create-a-stored-procedure.md)
- [使用表值参数（数据库引擎）](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)
- [sys.dm_sql_referenced_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)
- [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)  
