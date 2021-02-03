---
description: INSERT (Transact-SQL)
title: INSERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- INSERT_TSQL
- INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- inserting multiple rows
- user-defined types [SQL Server], inserting values
- DML [SQL Server], INSERT statement
- bulk load [SQL Server]
- row additions [SQL Server], INSERT statement
- inserting rows
- table value constructor [SQL Server]
- adding data
- INSERT statement [SQL Server], about INSERT statement
- INSERT statement [SQL Server]
- UDTs [SQL Server], inserting values
- adding rows
- INSERT INTO statement
- data manipulation language [SQL Server], INSERT statement
- inserting data
ms.assetid: 1054c76e-0fd5-4131-8c07-a6c5d024af50
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b5a6fd7131b63741a82c120e1e0b222405865a9d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237177"
---
# <a name="insert-transact-sql"></a>INSERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


将一行或多行添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的表或视图中。 有关示例，请参阅[示例](#InsertExamples)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  

[ WITH <common_table_expression> [ ,...n ] ]  
INSERT   
{  
        [ TOP ( expression ) [ PERCENT ] ]   
        [ INTO ]   
        { <object> | rowset_function_limited   
          [ WITH ( <Table_Hint_Limited> [ ...n ] ) ]  
        }  
    {  
        [ ( column_list ) ]   
        [ <OUTPUT Clause> ]  
        { VALUES ( { DEFAULT | NULL | expression } [ ,...n ] ) [ ,...n     ]   
        | derived_table   
        | execute_statement  
        | <dml_table_source>  
        | DEFAULT VALUES   
        }  
    }  
}  
[;]  
  
<object> ::=  
{   
    [ server_name . database_name . schema_name .   
      | database_name .[ schema_name ] .   
      | schema_name .   
    ]  
  table_or_view_name  
}  
  
<dml_table_source> ::=  
    SELECT <select_list>  
    FROM ( <dml_statement_with_output_clause> )   
      [AS] table_alias [ ( column_alias [ ,...n ] ) ]  
    [ WHERE <search_condition> ]  
        [ OPTION ( <query_hint> [ ,...n ] ) ]  
```  
  
```syntaxsql
-- External tool only syntax  

INSERT   
{  
    [BULK]  
    { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
    ( <column_definition> )  
    [ WITH (  
        [ [ , ] CHECK_CONSTRAINTS ]  
        [ [ , ] FIRE_TRIGGERS ]  
        [ [ , ] KEEP_NULLS ]  
        [ [ , ] KILOBYTES_PER_BATCH = kilobytes_per_batch ]  
        [ [ , ] ROWS_PER_BATCH = rows_per_batch ]  
        [ [ , ] ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) ]  
        [ [ , ] TABLOCK ]  
    ) ]  
}  
  
[; ] <column_definition> ::=  
 column_name <data_type>  
    [ COLLATE collation_name ]  
    [ NULL | NOT NULL ]  
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

INSERT INTO { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ ( column_name [ ,...n ] ) ]  
    {   
      VALUES ( { NULL | expression } )  
      | SELECT <select_criteria>  
    }  
    [ OPTION ( <query_option> [ ,...n ] ) ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 WITH \<common_table_expression>  
 指定在 INSERT 语句作用域内定义的临时命名结果集（也称为公用表表达式）。 结果集源自 SELECT 语句。 有关详细信息，请参阅 [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md)。  
  
 TOP (expression) [ PERCENT ]  
 指定将插入的随机行的数目或百分比。 *expression* 可以是行数或行的百分比。 有关详细信息，请参阅 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md)。  
  
 INTO  
 一个可选的关键字，可以将它用在 INSERT 和目标表之间。  
  
 server_name  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 表或视图所在的链接服务器的名称。 server_name 可以指定为[链接服务器](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)名称，或通过使用 [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) 函数。  
  
 如果将 server_name 指定为链接服务器，则需要 database_name 和 schema_name  。 如果使用 OPENDATASOURCE 指定 server_name，则 database_name 和 schema_name 可能不适用于所有数据源，并且受到访问远程对象的 OLE DB 访问接口的性能的限制  。  
  
 *database_name*  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 数据库的名称。  
  
 *schema_name*  
 表或视图所属架构的名称。  
  
 table_or view_name  
 要接收数据的表或视图的名称。  
  
 [表](../../t-sql/data-types/table-transact-sql.md)变量在其作用域内可用作 INSERT 语句中的表源。  
  
 table_or_view_name 引用的视图必须可更新，并且只在该视图的 FROM 子句中引用一个基表。 例如，多表视图中的 INSERT 必须使用只引用一个基表中的各列的 column_list。 有关可更新视图的详细信息，请参阅 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)。  
  
 rowset_function_limited  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) 或 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) 函数。 使用这些函数受到访问远程对象的 OLE DB 访问接口的性能的限制。  
  
 WITH ( \<table_hint_limited> [... *n* ] )  
 指定目标表允许的一个或多个表提示。 需要有 WITH 关键字和括号。  
  
 不允许 READPAST、NOLOCK 和 READUNCOMMITTED。 有关表提示的详细信息，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  
  
> [!IMPORTANT]  
>  在将来的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中，将删除对作为 INSERT 语句目标的表指定 HOLDLOCK、SERIALIZABLE、READCOMMITTED、REPEATABLEREAD 或 UPDLOCK 提示的功能。 这些提示不影响 INSERT 语句的性能。 请避免在新的开发工作中使用该功能，并计划修改当前使用该功能的应用程序。  
  
 对作为 INSERT 语句目标的表指定 TABLOCK 提示与指定 TABLOCKX 提示具有相同的效果。 对表采用排他锁。  
  
 (*column_list*)  
 要在其中插入数据的一列或多列的列表。 必须用括号将 column_list 括起来，并且用逗号进行分隔。  
  
 如果某列不在 column_list 中，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 必须能够基于该列的定义提供一个值；否则不能加载行。 如果列满足下面的条件，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]将自动为列提供值：  
  
-   具有 IDENTITY 属性。 使用下一个增量标识值。  
  
-   有默认值。 使用列的默认值。  
  
-   具有 timestamp 数据类型。 使用当前的时间戳值。  
  
-   可以为 Null。 使用 Null 值。  
  
-   是计算列。 使用计算值。  
  
当向标识列中插入显式值时，必须使用 column_list，并且表的 SET IDENTITY_INSERT 选项必须为 ON。  
  
OUTPUT 子句  
 将插入行作为插入操作的一部分返回。 结果可返回到处理应用程序或插入到表或表变量中以供进一步处理。  
  
 引用本地分区视图、分布式分区视图或远程表的 DML 语句或包含 execute_statement 的 INSERT 语句都不支持 OUTPUT 子句。 包含 \<dml_table_source> 子句的 INSERT 语句中不支持 OUTPUT INTO 子句。 有关该子句的参数和行为的详细信息，请参阅 [OUTPUT 子句 (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md)。
  
 VALUES  
 引入要插入的数据值的一个或多个列表。 对于 column_list（如果已指定）或表中的每个列，都必须有一个数据值。 必须用圆括号将值列表括起来。  
  
 如果值列表中的各值与表中各列的顺序不相同，或者未包含表中各列的值，则必须使用 column_list 显式指定存储每个传入值的列。  
  
 您可以使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 行构造函数（又称为表值构造函数）在一个 INSERT 语句中指定多个行。 行构造函数包含一个 VALUES 子句和多个括在圆括号中且以逗号分隔的值列表。 有关详细信息，请参阅[表值构造函数 (Transact-SQL)](../../t-sql/queries/table-value-constructor-transact-sql.md)。  
  
 DEFAULT  
 强制[!INCLUDE[ssDE](../../includes/ssde-md.md)]加载为列定义的默认值。 如果某列并不存在默认值，并且该列允许 Null 值，则插入 NULL。 对于使用 timestamp 数据类型定义的列，插入下一个时间戳值。 DEFAULT 对标识列无效。  
  
 *expression*  
 一个常量、变量或表达式。 表达式不能包含 EXECUTE 语句。  
  
 当引用 Unicode 字符数据类型 nchar、nvarchar 和 ntext 时，“expression”应采用大写字母“N”作为前缀。 如果未指定“N”，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将字符串转换为与数据库或列的默认排序规则相对应的代码页。 此代码页中没有的字符都将丢失。  
  
 derived_table  
 任何有效的 SELECT 语句，它返回将加载到表中的数据行。 SELECT 语句不能包含公用表表达式 (CTE)。  
  
 *execute_statement*  
 任何有效的 EXECUTE 语句，它使用 SELECT 或 READTEXT 语句返回数据。 有关详细信息，请参阅 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)。  
  
 不能在 INSERT…EXEC 语句中指定 EXECUTE 语句的 RESULT SETS 选项。  
  
 如果 execute_statement 使用 INSERT，则每个结果集必须与表或 column_list 中的列兼容 。  
  
 可以使用 execute_statement 对同一服务器或远程服务器执行存储过程。 执行远程服务器中的过程，并将结果集返回到本地服务器并加载到本地服务器的表中。 在分布式事务中，当连接启用了多个活动结果集 (MARS) 时，无法针对环回链接服务器发出 execute_statement。  
  
 如果 execute_statement 使用 READTEXT 语句返回数据，则每个 READTEXT 语句最多可以返回 1 MB (1024 KB) 的数据。 execute_statement 还可以用于扩展过程。 execute_statement 插入由扩展过程的主线程返回的数据，但不插入主线程以外的线程的输出。  
  
 不能将表值参数指定为 INSERT EXEC 语句的目标；但是，可以将它指定为 INSERT EXEC 字符串或存储过程中的源。 有关详细信息，请参阅[使用表值参数（数据引擎）](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)。  
  
 \<dml_table_source>  
 指定插入目标表的行是 INSERT、UPDATE、DELETE 或 MERGE 语句的 OUTPUT 子句返回的行；可以通过 WHERE 子句对行进行筛选。 如果指定了 \<dml_table_source>，外部 INSERT 语句的目标必须满足以下限制： 
  
-   必须是基表而不是视图。  
  
-   不能是远程表。  
  
-   不能对其定义任何触发器。  
  
-   不能参与任何主键-外键关系。  
  
-   不能参与合并复制或事务复制的可更新订阅。  
  
 数据库的兼容级别必须设置为 100 或更高。 有关详细信息，请参阅 [OUTPUT 子句 (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md)。  
  
 \<select_list>  
 指定要插入 OUTPUT 子句所返回的列的逗号分隔列表。 \<select_list> 中的列必须与要插入值的列兼容。 \<select_list> 无法引用聚合函数或 TEXTPTR。 
  
> [!NOTE]  
>  无论在 \<dml_statement_with_output_clause> 中对 SELECT 列表中列出的任何变量做何种更改，这些变量都将引用其原始值。  
  
 \<dml_statement_with_output_clause>  
 在 OUTPUT 子句中返回受影响行的有效 INSERT、UPDATE、DELETE 或 MERGE 语句。 语句中不能包含 WITH 子句，且不能以远程表或分区视图为目标。 如果指定了 UPDATE 或 DELETE，则所指定的 UPDATE 或 DELETE 不能是基于游标的。 源行不能作为嵌套的 DML 语句进行引用。  
  
 WHERE \<search_condition>  
 包含用于筛选 \<dml_statement_with_output_clause> 所返回行的有效 \<search_condition> 的任何 WHERE 子句。 有关详细信息，请参阅[搜索条件 (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md)。 在此上下文中使用时，\<search_condition> 不能包含子查询、执行数据访问的标量用户定义函数、聚合函数、TEXTPTR 或全文搜索谓词。 
  
 DEFAULT VALUES  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 强制新行包含为每个列定义的默认值。  
  
 BULK  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 由外部工具用来上载二进制数据流。 该选项并不旨在用于 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、SQLCMD、OSQL 之类的工具或者 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 之类的数据访问应用程序编程接口。  
  
 FIRE_TRIGGERS  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定将在二进制数据流上载操作期间执行目标表中定义的所有插入触发器。 有关详细信息，请参阅 [BULK INSERT (Transact SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)。  
  
 CHECK_CONSTRAINTS  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定在二进制数据流上载操作期间，必须检查所有对目标表或视图的约束。 有关详细信息，请参阅 [BULK INSERT (Transact SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)。  
  
 KEEPNULLS  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定在二进制数据流上载操作期间空列应该保留 null 值。 有关详细信息，请参阅[在批量导入期间保留 Null 或使用默认值 (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)。  
  
 KILOBYTES_PER_BATCH = kilobytes_per_batch  
 将每个批处理中数据的近似千字节数 (KB) 指定为 kilobytes_per_batch。 有关详细信息，请参阅 [BULK INSERT (Transact SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)。  
  
 ROWS_PER_BATCH =*rows_per_batch*  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指示二进制数据流中近似的数据行数量。 有关详细信息，请参阅 [BULK INSERT (Transact SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)。  
  
> [!NOTE]
>  如果未提供列列表，则引发一个语法错误。  

## <a name="remarks"></a>备注  
有关将数据插入 SQL 图表的详细信息，请参阅 [INSERT（SQL 图形）](../../t-sql/statements/insert-sql-graph.md)。 

## <a name="best-practices"></a>最佳实践  
 使用 @@ROWCOUNT 函数返回插入到客户端应用程序的行数。 有关详细信息，请参阅 [@@ROWCOUNT (Transact-SQL)](../../t-sql/functions/rowcount-transact-sql.md)。  
  
### <a name="best-practices-for-bulk-importing-data"></a>大容量导入数据的最佳实践  
  
#### <a name="using-insert-intoselect-to-bulk-import-data-with-minimal-logging-and-parallelism"></a>使用 INSERT INTO…SELECT 进行大容量导入数据并按最小方式记录日志和平行度 
可以使用 `INSERT INTO <target_table> SELECT <columns> FROM <source_table>` 高效地将大量行从一个表（例如临时表）传输到按最小方式记录日志的其他表中。 按最小方式记录日志可以提高语句的性能，减少在事务期间此操作填充可用事务日志空间的可能性。  
  
针对此语句的按最小方式记录日志具有以下要求：  
-   数据库的恢复模式设置为简单或大容量日志模式。  
-   目标表是空或非空堆。  
-   复制操作未使用目标表。  
-   为目标表指定了 `TABLOCK` 提示。  
  
此外，可能还可以以最小方式记录通过 MERGE 语句中的插入操作插入堆中的行。  
  
与持有较少限制性大容量更新 (BU) 锁的 `BULK INSERT` 语句不同，具有 `TABLOCK` 提示的 `INSERT INTO … SELECT` 语句持有一个针对表的排他 (X) 锁。 也就是说不能使用同时执行的多个插入操作插入行。 

但是，从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 和数据库兼容性级别 130 开始，在插入堆或聚集列存储索引 (CCI) 时，可以并行执行单个 `INSERT INTO … SELECT` 语句。 使用 `TABLOCK` 提示时，可能会出现并行插入。  

以上语句的并行度具有以下要求，这类似于针对最小日志记录的要求：  
-   目标表是空或非空堆。  
-   目标表具有聚集列存储索引 (CCI)，但没有非聚集索引。  
-   目标表不包含 IDENTITY_INSERT 设置为 OFF 的标识列。  
-   为目标表指定了 `TABLOCK` 提示。

在满足最小日志记录和并行插入要求的情况下，将实施这两项改进，这样可以确保数据加载操作的最大吞吐量。

> [!NOTE]
> 还可以使用 TABLOCK 提示为并行启用到本地临时表（由 # 前缀标识）和全局临时表（由 ## 前缀标识）的插入。
  
#### <a name="using-openrowset-and-bulk-to-bulk-import-data"></a>使用 OPENROWSET 和 BULK 大容量导入数据  
 OPENROWSET 函数可接受以下表提示，这些表提示使用 INSERT 语句提供大容量加载优化：  
  
-   `TABLOCK` 提示可以最大限度地减少插入操作的日志记录数量。 数据库的恢复模式必须设置为简单或大容量日志模式，并且目标表不能用于复制。 有关详细信息，请参阅[在批量导入中按最小方式记录日志的前提条件](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md)。  
-   `TABLOCK` 提示可以启用并行插入操作。 目标表是不包含非聚集索引的堆或聚集列存储索引 (CCI)，并且目标表不能有指定的标识列。  
-   `IGNORE_CONSTRAINTS` 提示可以暂时禁用 FOREIGN KEY 和 CHECK 约束检查。  
-   `IGNORE_TRIGGERS` 提示可以暂时禁用触发器执行。  
-   `KEEPDEFAULTS` 提示允许数据记录在某一表列缺少值时插入此列的默认值（如果有），而不是插入 NULL。  
-   `KEEPIDENTITY` 提示允许将导入数据文件中的标识值用于目标表中的标识列。  
  
这些优化类似于可与 `BULK INSERT` 命令一起使用的优化。 有关详细信息，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  
  
## <a name="data-types"></a>数据类型  
 插入行时，考虑以下数据类型行为：  
  
-   如果将值加载到 char、varchar 或 varbinary 数据类型的列中，则尾随空格（对于 char 和 varchar 为空格，对于 varbinary 为零）的填充或截断由创建表时为该列定义的 SET ANSI_PADDING 设置确定     。 有关详细信息，请参阅 [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)。  
  
     下表显示了 SET ANSI_PADDING OFF 的默认操作。  
  
    |数据类型|默认操作|  
    |---------------|-----------------------|  
    |**char**|将带有空格的值填充到已定义的列宽。|  
    |**varchar**|删除最后的非空格字符后面的尾随空格，而对于只由空格组成的字符串，一直删除到只留下一个空格。|  
    |**varbinary**|删除尾随的零。|  
  
-   如果将一个空字符串 (' ') 加载到数据类型为 varchar 或 text 的列，则默认操作是加载一个零长度的字符串 。  
  
-   将 Null 值插入到 text 或 image 列不创建有效的文本指针，也不预分配 8 KB 的文本页 。  
  
-   使用 uniqueidentifier 数据类型创建的列存储特殊格式的 16 字节二进制值。 与标识列不同，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 不为 uniqueidentifier 数据类型的列自动生成值。 在插入操作过程中，可以将 uniqueidentifier 数据类型的变量和 xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx 格式的字符串常量（包括连字符在内共 36 个字符，其中 x 表示从 0 到 9 或从 a 到 f 的十六进制数字）用于 uniqueidentifier 列 。 例如，6F9619FF-8B86-D011-B42D-00C04FC964FF 是 uniqueidentifier 变量或列的有效值。 使用 [NEWID()](../../t-sql/functions/newid-transact-sql.md) 函数获取全局唯一 ID (GUID)。  
  
### <a name="inserting-values-into-user-defined-type-columns"></a>将值插入到用户定义类型列中  
 可以通过以下方法将值插入到用户定义的类型列中：  
  
-   提供用户定义类型的值。  
  
-   提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型的值，条件是该用户定义类型支持该类型的隐式转换或显式转换。 下面的示例演示了如何基于字符串进行显式转换将值插入到用户定义的类型 `Point` 的列中。  
  
    ```sql
    INSERT INTO Cities (Location)  
    VALUES ( CONVERT(Point, '12.3:46.2') );  
    ```  
  
     由于所有用户定义的类型可以从二进制值进行隐式转换，因此还可以在不执行显式转换的情况下提供二进制值。  
  
-   调用一个用户定义函数，该函数返回用户定义类型的值。 下面的示例使用用户定义函数 `CreateNewPoint()` 创建一个用户定义类型 `Point` 的新值，并将该值插入到 `Cities` 表中。  
  
    ```sql
    INSERT INTO Cities (Location)  
    VALUES ( dbo.CreateNewPoint(x, y) );  
    ```  
  
## <a name="error-handling"></a>错误处理  
 可以通过在 TRY…CATCH 构造函数中指定 INSERT 语句，实现对该语句的错误处理。  
  
 如果 INSERT 语句违反约束或规则，或者包含与列的数据类型不兼容的值，则该语句将失败，并且返回错误消息。  
  
 如果 INSERT 是使用 SELECT 或 EXECUTE 加载多行，那么一旦加载的值中出现任何违反规则或约束的情况，就会导致终止语句，且不会加载任何行。  
  
 如果在表达式计算过程中 INSERT 语句遇到算术错误（溢出、被零除或域错误），则[!INCLUDE[ssDE](../../includes/ssde-md.md)]会处理这些错误，就好像 SET ARITHABORT 设置为 ON 一样。 停止批处理，并返回一条错误消息。 如果 SET ARITHABORT 和 SET ANSI_WARNINGS 为 OFF，并且在对表达式求值的过程中 INSERT、DELETE 或 UPDATE 语句遇到算术错误（溢出、被零除或域错误），[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将插入或更新一个 NULL 值。 如果目标列不可为空，则插入或更新操作将失败，用户将收到错误消息。  
  
## <a name="interoperability"></a>互操作性  
 当为表或视图的 INSERT 操作定义了 `INSTEAD OF` 触发器时，则执行该触发器而不是 INSERT 语句。 有关 `INSTEAD OF` 触发器的详细信息，请参阅 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 当向远程表中插入值且没有为所有列指定所有值时，用户必须标识将向其中插入指定值的列。  
  
 在将 TOP 与 INSERT 结合使用时，被引用行不按任何顺序排列，不能直接在此语句中指定 ORDER BY 子句。 如果需要使用 TOP 来插入按有意义的时间顺序排列的行，您必须同时使用 TOP 和在嵌套 select 语句中指定的 ORDER BY 子句。 请参阅本主题后面的“示例”一节。
 
使用 SELECT 和 ORDER BY 填充行的 INSERT 查询保证了标识值的计算方式，但不能保证行的插入顺序。

在并行数据仓库中，除非另外还指定了 TOP，否则 ORDER BY 子句在 VIEWS、CREATE TABLE AS SELECT、INSERT SELECT、内联函数、派生表、子查询和常见表表达式中无效。
  
## <a name="logging-behavior"></a>日志记录行为  
 INSERT 语句始终完全记入日志，只有在将 OPENROWSET 函数与 BULK 关键字一起使用或者在使用 `INSERT INTO <target_table> SELECT <columns> FROM <source_table>` 时除外。 这些操作可进行最小日志记录。 有关详细信息，请参阅本主题前面的“大容量加载数据的最佳做法”一节。  
  
## <a name="security"></a>安全性  
 在链接服务器的连接过程中，发送服务器提供登录名和密码以代表自己连接到接收服务器。 为了使该连接有效，必须使用 [sp_addlinkedsrvlogin](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md) 在链接服务器之间创建登录名映射。  
  
 使用 OPENROWSET(BULK…) 时，请务必了解 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 是如何处理模拟的。 有关详细信息，请参阅[使用 BULK INSERT 或 OPENROWSET (BULK...) 批量导入数据 (SQL Server)](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md) 中的“安全注意事项”。  
  
### <a name="permissions"></a>权限  
 需要对目标表具有 INSERT 权限。  
  
 INSERT 权限默认授予 `sysadmin` 固定服务器角色、`db_owner` 和 `db_datawriter` 固定数据库角色以及表所有者的成员。 `sysadmin`、`db_owner` 和 `db_securityadmin` 角色以及表所有者的成员可以将权限转让给其他用户。  
  
 若要使用 OPENROWSET 函数 BULK 选项执行 INSERT，你必须是 `sysadmin` 固定服务器角色成员或 `bulkadmin` 固定服务器角色成员。  
  
##  <a name="examples"></a><a name="InsertExamples"></a> 示例  
  
|类别|作为特征的语法元素|  
|--------------|------------------------------|  
|[基本语法](#BasicSyntax)|INSERT • 表值构造函数|  
|[处理列值](#ColumnValues)|IDENTITY • NEWID • 默认值 • 用户定义类型|  
|[插入来自其他表的数据](#OtherTables)|INSERT…SELECT • INSERT…EXECUTE • WITH 公用表表达式 • TOP • OFFSET FETCH|  
|[指定目标对象，而非标准表](#TargetObjects)|视图 • 表变量|  
|[向远程表中插入行](#RemoteTables)|链接服务器 • OPENQUERY 行集函数 • OPENDATASOURCE 行集函数|  
|[从表或数据文件中大容量加载数据](#BulkLoad)|INSERT…SELECT • OPENROWSET 函数|  
|[通过使用提示覆盖查询优化器的默认行为](#TableHints)|表提示|  
|[捕获 INSERT 语句的结果](#CaptureResults)|OUTPUT 子句|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> 基本语法  
 本节中的示例说明了使用最低要求的语法的 INSERT 语句的基本功能。  
  
#### <a name="a-inserting-a-single-row-of-data"></a>A. 插入单行数据  
 下面的示例在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `Production.UnitMeasure` 表中插入一行。 该表中的各列是 `UnitMeasureCode`、`Name` 和 `ModifiedDate`。 由于提供了所有列的值并按表中各列的顺序列出这些值，因此不必在列列表中指定列名  
  
```sql
INSERT INTO Production.UnitMeasure  
VALUES (N'FT', N'Feet', '20080414');  
```  
  
#### <a name="b-inserting-multiple-rows-of-data"></a>B. 插入多行数据  
 下面的示例使用[表值构造函数](../../t-sql/queries/table-value-constructor-transact-sql.md)在单个 INSERT 语句中将三行插入 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `Production.UnitMeasure` 表。 由于提供了所有列的值并按表中各列的顺序列出这些值，因此不必在列列表中指定列名。  
  
```sql
INSERT INTO Production.UnitMeasure  
VALUES (N'FT2', N'Square Feet ', '20080923'), (N'Y', N'Yards', '20080923')
    , (N'Y3', N'Cubic Yards', '20080923');  
```  
  
#### <a name="c-inserting-data-that-is-not-in-the-same-order-as-the-table-columns"></a>C. 按与表列顺序不同的顺序插入数据  
 下面的示例使用列列表显式指定插入到每个列中的值。 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `Production.UnitMeasure` 表中的列顺序为 `UnitMeasureCode`、`Name`、`ModifiedDate`；但这些列的列出顺序与 column_list 中的顺序不同。  
  
```sql
INSERT INTO Production.UnitMeasure (Name, UnitMeasureCode,  
    ModifiedDate)  
VALUES (N'Square Yards', N'Y2', GETDATE());  
```  
  
###  <a name="handling-column-values"></a><a name="ColumnValues"></a> 处理列值  
 本节中的示例说明将值插入列中的方法，这些列是使用 IDENTITY 属性或 DEFAULT 值定义的列，或者是用 uniqueidentifer 之类的数据类型定义的列，或者是用户定义类型列。  
  
#### <a name="d-inserting-data-into-a-table-with-columns-that-have-default-values"></a>D. 将数据插入其列具有默认值的表  
 下面的示例演示了如何将行插入到包含自动生成值或具有默认值的列的表中。 `Column_1` 是一个计算列，它通过将一个字符串与插入 `column_2` 的值进行串联，自动生成一个值。 `Column_2` 是用默认约束定义的。 如果没有为该列指定值，将使用默认值。 `Column_3` 是使用 rowversion 数据类型定义的，它自动生成一个唯一的、递增的二进制数字。 `Column_4` 不自动生成值。 如果没有为该列指定值，将插入 NULL。 INSERT 语句插入一些行，这些行只有部分列包含值。 在最后一个 INSERT 语句中，未指定列，只通过使用 DEFAULT VALUES 子句插入了默认值。  
  
```sql
CREATE TABLE dbo.T1   
(  
    column_1 AS 'Computed column ' + column_2,   
    column_2 varchar(30)   
        CONSTRAINT default_name DEFAULT ('my column default'),  
    column_3 rowversion,  
    column_4 varchar(40) NULL  
);  
GO  
INSERT INTO dbo.T1 (column_4)   
    VALUES ('Explicit value');  
INSERT INTO dbo.T1 (column_2, column_4)   
    VALUES ('Explicit value', 'Explicit value');  
INSERT INTO dbo.T1 (column_2)   
    VALUES ('Explicit value');  
INSERT INTO T1 DEFAULT VALUES;   
GO  
SELECT column_1, column_2, column_3, column_4  
FROM dbo.T1;  
GO  
```  
  
#### <a name="e-inserting-data-into-a-table-with-an-identity-column"></a>E. 将数据插入到含标识列的表中  
 下面的示例演示了将数据插入到标识列中的不同方法。 前两个 INSERT 语句允许为新行生成标识值。 第三个 INSERT 语句用 SET IDENTITY_INSERT 语句覆盖列的 IDENTITY 属性，并将一个显式值插入到标识列中。  
  
```sql
CREATE TABLE dbo.T1 ( column_1 int IDENTITY, column_2 VARCHAR(30));  
GO  
INSERT T1 VALUES ('Row #1');  
INSERT T1 (column_2) VALUES ('Row #2');  
GO  
SET IDENTITY_INSERT T1 ON;  
GO  
INSERT INTO T1 (column_1,column_2)   
    VALUES (-99, 'Explicit identity value');  
GO  
SELECT column_1, column_2  
FROM T1;  
GO  
```  
  
#### <a name="f-inserting-data-into-a-uniqueidentifier-column-by-using-newid"></a>F. 通过使用 NEWID() 将数据插入到 uniqueidentifier 列中  
 下面的示例使用 [NEWID](../../t-sql/functions/newid-transact-sql.md)() 函数获取 `column_2` 的 GUID。 与标识列不同，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 不为 [uniqueidentifier](../../t-sql/data-types/uniqueidentifier-transact-sql.md) 数据类型的列自动生成值（如第二个 `INSERT` 语句所示）。  
  
```sql
CREATE TABLE dbo.T1   
(  
    column_1 int IDENTITY,   
    column_2 uniqueidentifier,  
);  
GO  
INSERT INTO dbo.T1 (column_2)   
    VALUES (NEWID());  
INSERT INTO T1 DEFAULT VALUES;   
GO  
SELECT column_1, column_2  
FROM dbo.T1;  
```  
  
#### <a name="g-inserting-data-into-user-defined-type-columns"></a>G. 将数据插入到用户定义类型列中  
 下面的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句将三行插入到 `PointValue` 表的 `Points` 列中。 该列使用 [CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) (UDT)。 `Point` 数据类型由作为 UDT 属性公开的 X 和 Y 整数值组成。 必须使用 CAST 或 CONVERT 函数，才能将以逗号分隔的 X 和 Y 值转换为 `Point` 类型。 前两个语句使用 CONVERT 函数将字符串值转换为 `Point` 类型，第三个语句使用 CAST 函数。 有关详细信息，请参阅[操作 UDT 数据](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-manipulating-udt-data.md)。  
  
```sql
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '3,4'));  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '1,5'));  
INSERT INTO dbo.Points (PointValue) VALUES (CAST ('1,99' AS Point));  
```  
  
###  <a name="inserting-data-from-other-tables"></a><a name="OtherTables"></a> 插入来自其他表的数据  
 本节中的示例说明将行从一个表插入另一个表的方法。  
  
#### <a name="h-using-the-select-and-execute-options-to-insert-data-from-other-tables"></a>H. 使用 SELECT 和 EXECUTE 选项插入来自其他表的数据  
 下面的示例说明如何使用 INSERT…SELECT 或 INSERT…EXECUTE 将来自一个表的数据插入另一个表。 每种方法都基于一个多表 SELECT 语句，该语句在列列表中包含一个表达式及一个文字值。  
  
 第一个 INSERT 语句使用 SELECT 语句从 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的源表（`Employee`、`SalesPerson` 和 `Person`）中派生数据，并将结果集存储在 `EmployeeSales` 表中。 第二个 INSERT 语句使用 EXECUTE 子句调用包含 SELECT 语句的存储过程，第三个 INSERT 使用 EXECUTE 子句将 SELECT 语句作为文字字符串引用。  
  
```sql
CREATE TABLE dbo.EmployeeSales  
( DataSource   varchar(20) NOT NULL,  
  BusinessEntityID   varchar(11) NOT NULL,  
  LastName     varchar(40) NOT NULL,  
  SalesDollars money NOT NULL  
);  
GO  
CREATE PROCEDURE dbo.uspGetEmployeeSales   
AS   
    SET NOCOUNT ON;  
    SELECT 'PROCEDURE', sp.BusinessEntityID, c.LastName,   
        sp.SalesYTD   
    FROM Sales.SalesPerson AS sp    
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY sp.BusinessEntityID, c.LastName;  
GO  
--INSERT...SELECT example  
INSERT INTO dbo.EmployeeSales  
    SELECT 'SELECT', sp.BusinessEntityID, c.LastName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY sp.BusinessEntityID, c.LastName;  
GO  
--INSERT...EXECUTE procedure example  
INSERT INTO dbo.EmployeeSales   
EXECUTE dbo.uspGetEmployeeSales;  
GO  
--INSERT...EXECUTE('string') example  
INSERT INTO dbo.EmployeeSales   
EXECUTE   
('  
SELECT ''EXEC STRING'', sp.BusinessEntityID, c.LastName,   
    sp.SalesYTD   
    FROM Sales.SalesPerson AS sp   
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE ''2%''  
    ORDER BY sp.BusinessEntityID, c.LastName  
');  
GO  
--Show results.  
SELECT DataSource,BusinessEntityID,LastName,SalesDollars  
FROM dbo.EmployeeSales;  
```  
  
#### <a name="i-using-with-common-table-expression-to-define-the-data-inserted"></a>I. 使用 WITH 公共表表达式定义插入的数据  
 下面的示例在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中创建 `NewEmployee` 表。 公用表表达式 (`EmployeeTemp`) 定义要插入到 `NewEmployee` 表中的来自一个或多个表的行。 INSERT 语句引用公用表表达式中的列。  
  
```sql
CREATE TABLE HumanResources.NewEmployee  
(  
    EmployeeID int NOT NULL,  
    LastName nvarchar(50) NOT NULL,  
    FirstName nvarchar(50) NOT NULL,  
    PhoneNumber Phone NULL,  
    AddressLine1 nvarchar(60) NOT NULL,  
    City nvarchar(30) NOT NULL,  
    State nchar(3) NOT NULL,   
    PostalCode nvarchar(15) NOT NULL,  
    CurrentFlag Flag  
);  
GO  
WITH EmployeeTemp (EmpID, LastName, FirstName, Phone,   
                   Address, City, StateProvince,   
                   PostalCode, CurrentFlag)  
AS (SELECT   
       e.BusinessEntityID, c.LastName, c.FirstName, pp.PhoneNumber,  
       a.AddressLine1, a.City, sp.StateProvinceCode,   
       a.PostalCode, e.CurrentFlag  
    FROM HumanResources.Employee e  
        INNER JOIN Person.BusinessEntityAddress AS bea  
        ON e.BusinessEntityID = bea.BusinessEntityID  
        INNER JOIN Person.Address AS a  
        ON bea.AddressID = a.AddressID  
        INNER JOIN Person.PersonPhone AS pp  
        ON e.BusinessEntityID = pp.BusinessEntityID  
        INNER JOIN Person.StateProvince AS sp  
        ON a.StateProvinceID = sp.StateProvinceID  
        INNER JOIN Person.Person as c  
        ON e.BusinessEntityID = c.BusinessEntityID  
    )  
INSERT INTO HumanResources.NewEmployee   
    SELECT EmpID, LastName, FirstName, Phone,   
           Address, City, StateProvince, PostalCode, CurrentFlag  
    FROM EmployeeTemp;  
GO  
```  
  
#### <a name="j-using-top-to-limit-the-data-inserted-from-the-source-table"></a>J. 使用 TOP 限制从源表插入的数据  
 下面的示例创建 `EmployeeSales` 表，并插入 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `HumanResources.Employee` 表中的前 5 名随机雇员的姓名和本年度到目前为止的销售数据。 INSERT 语句选择 `SELECT` 语句返回的任意 5 行。 OUTPUT 子句将显示插入 `EmployeeSales` 表中的行。 请注意，SELECT 语句中的 ORDER BY 子句不用于确定前 5 名雇员。  
  
```sql
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   nvarchar(11) NOT NULL,  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  YearlySales  money NOT NULL  
 );  
GO  
INSERT TOP(5)INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, 
        inserted.LastName, inserted.YearlySales  
    SELECT sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
```  
  
 如果必须使用 TOP 来插入按有意义的时间顺序排列的行，您必须同时使用 TOP 和嵌套 select 语句中的 ORDER BY，如以下示例所示。 OUTPUT 子句将显示插入 `EmployeeSales` 表中的行。 请注意，现在基于 ORDER BY 子句的结果而非随机行插入前 5 名员工。  
  
```sql
INSERT INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, 
        inserted.LastName, inserted.YearlySales  
    SELECT TOP (5) sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
```  
  
###  <a name="specifying-target-objects-other-than-standard-tables"></a><a name="TargetObjects"></a> 指定目标对象，而非标准表  
 本节中的示例说明如何通过指定视图或表变量来插入行。  
  
#### <a name="k-inserting-data-by-specifying-a-view"></a>K. 通过指定视图来插入数据  
 下面的示例将一个视图名指定为目标对象，但将新行插入到基础基表中。 `INSERT` 语句中值的顺序必须与视图的列顺序相匹配。 有关详细信息，请参阅[通过视图修改数据](../../relational-databases/views/modify-data-through-a-view.md)。  
  
```sql
CREATE TABLE T1 ( column_1 int, column_2 varchar(30));  
GO  
CREATE VIEW V1 AS   
SELECT column_2, column_1   
FROM T1;  
GO  
INSERT INTO V1   
    VALUES ('Row 1',1);  
GO  
SELECT column_1, column_2   
FROM T1;  
GO  
SELECT column_1, column_2  
FROM V1;  
GO  
```  
  
#### <a name="l-inserting-data-into-a-table-variable"></a>L. 向表变量中插入数据  
 下面的示例将一个表变量指定为 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的目标对象。  
  
```sql
-- Create the table variable.  
DECLARE @MyTableVar table(  
    LocationID int NOT NULL,  
    CostRate smallmoney NOT NULL,  
    NewCostRate AS CostRate * 1.5,  
    ModifiedDate datetime);  
  
-- Insert values into the table variable.  
INSERT INTO @MyTableVar (LocationID, CostRate, ModifiedDate)  
    SELECT LocationID, CostRate, GETDATE() 
    FROM Production.Location  
    WHERE CostRate > 0;  
  
-- View the table variable result set.  
SELECT * FROM @MyTableVar;  
GO  
```  
  
###  <a name="inserting-rows-into-a-remote-table"></a><a name="RemoteTables"></a> 向远程表中插入行  
 本节中的示例说明如何通过使用[链接服务器](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)或[行集函数](../functions/opendatasource-transact-sql.md)引用一个远程目标表，向该表插入行。  
  
#### <a name="m-inserting-data-into-a-remote-table-by-using-a-linked-server"></a>M. 通过使用链接服务器向远程表插入数据  
 下面的示例将行插入一个远程表中。 该示例从使用 [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) 创建指向远程数据源的链接开始。 然后，将链接服务器名称 `MyLinkServer` 指定为 server.catalog.schema.object 形式的由四个部分组成的对象名称的一部分。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' 
-- or 'server_nameinstance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  
```  
  
```sql
-- Specify the remote data source in the FROM clause using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
INSERT INTO MyLinkServer.AdventureWorks2012.HumanResources.Department (Name, GroupName)  
VALUES (N'Public Relations', N'Executive General and Administration');  
GO  
```  
  
#### <a name="n-inserting-data-into-a-remote-table-by-using-the-openquery-function"></a>N. 通过使用 OPENQUERY 函数向远程表插入数据  
 下面的示例通过指定 [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) 行集函数向远程表插入一行。 在之前例子中创建的链接服务器名称用于此示例。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
```sql
INSERT OPENQUERY (MyLinkServer, 
    'SELECT Name, GroupName 
     FROM AdventureWorks2012.HumanResources.Department')  
VALUES ('Environmental Impact', 'Engineering');  
GO  
```  
  
#### <a name="o-inserting-data-into-a-remote-table-by-using-the-opendatasource-function"></a>O. 通过使用 OPENDATASOURCE 函数向远程表插入数据  
 下面的示例通过指定 [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) 行集函数向远程表插入一行。 通过使用 server_name 或 server_name\instance_name 格式，为该数据源指定一个有效的服务器名称 。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
```sql
-- Use the OPENDATASOURCE function to specify the remote data source.  
-- Specify a valid server name for Data Source using the format 
-- server_name or server_nameinstance_name.  
  
INSERT INTO OPENDATASOURCE('SQLNCLI',  
    'Data Source= <server_name>; Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department (Name, GroupName)  
    VALUES (N'Standards and Methods', 'Quality Assurance');  
GO  
```  
  
#### <a name="p-inserting-into-an-external-table-created-using-polybase"></a>P. 插入到使用 PolyBase 创建的外部表中  
 将数据从 SQL Server 导出到 Hadoop 或 Azure 存储空间。 首先，创建一个指向目标文件或目录的外部表。 然后，使用 INSERT INTO 将数据从本地 SQL Server 表导出到外部数据源。 INSERT INTO 语句将创建目标文件或目录（如果不存在），而 SELECT 语句的结果将以指定文件格式导出到指定位置。  有关详细信息，请参阅 [PolyBase 入门](../../relational-databases/polybase/polybase-guide.md)。  
  
**适用于**： [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]。  
  
```sql
-- Create an external table.   
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
        [FirstName] char(25) NOT NULL,   
        [LastName] char(25) NOT NULL,   
        [YearlyIncome] float NULL,   
        [MaritalStatus] char(1) NOT NULL  
)  
WITH (  
        LOCATION='/old_data/2009/customerdata.tbl',  
        DATA_SOURCE = HadoopHDP2,  
        FILE_FORMAT = TextFileFormat,  
        REJECT_TYPE = VALUE,  
        REJECT_VALUE = 0  
);  
  
-- Export data: Move old data to Hadoop while keeping 
-- it query-able via external table.  

INSERT INTO dbo.FastCustomer2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  
  
###  <a name="bulk-loading-data-from-tables-or-data-files"></a><a name="BulkLoad"></a> 从表或数据文件中大容量加载数据  
 本节中的示例说明通过 INSERT 语句向表中大容量加载数据的两个方法。  
  
#### <a name="q-inserting-data-into-a-heap-with-minimal-logging"></a>Q. 将数据插入堆中并按最小方式记录日志  
 下面的示例创建一个新表（一个堆），并使用最小方式记录日志将来自其他表中的数据插入到这个新表中。 此示例假定 `AdventureWorks2012` 数据库的恢复模式设置为 FULL。 若要确保使用最小方式记录，应在插入行之前将 `AdventureWorks2012` 数据库的恢复模式设置为 BULK_LOGGED，并在 INSERT INTO…SELECT 语句后重置为 FULL。 此外，为目标表 `Sales.SalesHistory` 指定了 TABLOCK 提示。 这确保语句在事务日志中占用最少的空间并且高效执行。  
  
```sql
-- Create the target heap.  
CREATE TABLE Sales.SalesHistory(  
    SalesOrderID int NOT NULL,  
    SalesOrderDetailID int NOT NULL,  
    CarrierTrackingNumber nvarchar(25) NULL,  
    OrderQty smallint NOT NULL,  
    ProductID int NOT NULL,  
    SpecialOfferID int NOT NULL,  
    UnitPrice money NOT NULL,  
    UnitPriceDiscount money NOT NULL,  
    LineTotal money NOT NULL,  
    rowguid uniqueidentifier ROWGUIDCOL  NOT NULL,  
    ModifiedDate datetime NOT NULL );  
GO  
-- Temporarily set the recovery model to BULK_LOGGED.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY BULK_LOGGED;  
GO  
-- Transfer data from Sales.SalesOrderDetail to Sales.SalesHistory  
INSERT INTO Sales.SalesHistory WITH (TABLOCK)  
    (SalesOrderID,   
     SalesOrderDetailID,  
     CarrierTrackingNumber,   
     OrderQty,   
     ProductID,   
     SpecialOfferID,   
     UnitPrice,   
     UnitPriceDiscount,  
     LineTotal,   
     rowguid,   
     ModifiedDate)  
SELECT * FROM Sales.SalesOrderDetail;  
GO  
-- Reset the recovery model.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY FULL;  
GO  
```  
  
#### <a name="r-using-the-openrowset-function-with-bulk-to-bulk-load-data-into-a-table"></a>R. 将 OPENROWSET 函数与 BULK 一起使用来将数据大容量加载到表中  
 下面的示例通过指定 OPENROWSET 函数，将来自数据文件的行插入表中。 出于性能优化目的，指定了 IGNORE_TRIGGERS 表提示。 若要查看更多示例，请参阅[使用 BULK INSERT 或 OPENROWSET (BULK...) 导入批量数据 (SQL Server)](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
```sql
INSERT INTO HumanResources.Department WITH (IGNORE_TRIGGERS) (Name, GroupName)  
SELECT b.Name, b.GroupName   
FROM OPENROWSET (  
    BULK 'C:SQLFilesDepartmentData.txt',  
    FORMATFILE = 'C:SQLFilesBulkloadFormatFile.xml',  
    ROWS_PER_BATCH = 15000)AS b ;  
```  
  
###  <a name="overriding-the-default-behavior-of-the-query-optimizer-by-using-hints"></a><a name="TableHints"></a> 通过使用提示覆盖查询优化器的默认行为  
 本节中的示例说明如何使用[表提示](../../t-sql/queries/hints-transact-sql-table.md)在处理 INSERT 语句时暂时覆盖查询优化器的默认行为。  
  
> [!CAUTION]  
>  由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器通常会为查询选择最佳执行计划，因此我们建议仅在最后迫不得已的情况下才可由资深的开发人员和数据库管理员使用提示。  
  
#### <a name="s-using-the-tablock-hint-to-specify-a-locking-method"></a>S. 使用 TABLOCK 提示指定锁定方法  
 下面的示例指定对 Production.Location 表采用排他 (X) 锁，并保持到 INSERT 语句结束。  
  
适用范围：[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]、[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。  
  
```sql
INSERT INTO Production.Location WITH (XLOCK)  
(Name, CostRate, Availability)  
VALUES ( N'Final Inventory', 15.00, 80.00);  
```  
  
###  <a name="capturing-the-results-of-the-insert-statement"></a><a name="CaptureResults"></a> 捕获 INSERT 语句的结果  
 本节中的示例说明如何使用 [OUTPUT Clause](../../t-sql/queries/output-clause-transact-sql.md) 从 INSERT 语句影响的每一行返回信息（或基于的表达式）。 这些结果可以返回到处理应用程序，以供在确认消息、存档以及其他类似的应用程序要求中使用。  
  
#### <a name="t-using-output-with-an-insert-statement"></a>T. 将 OUTPUT 用于 INSERT 语句  
 下面的示例将行插入到 `ScrapReason` 表中，并使用 `OUTPUT` 子句将语句的结果返回到 `@MyTableVar` 表变量。 由于 `ScrapReasonID` 列使用 `IDENTITY` 属性定义，因此未在 `INSERT` 语句中为该列指定值。 但应注意，[!INCLUDE[ssDE](../../includes/ssde-md.md)]为该列生成的值在 `OUTPUT` 列中的 `INSERTED.ScrapReasonID` 子句中返回。  
  
```sql
DECLARE @MyTableVar table( NewScrapReasonID smallint,  
                           Name varchar(50),  
                           ModifiedDate datetime);  
INSERT Production.ScrapReason  
    OUTPUT INSERTED.ScrapReasonID, INSERTED.Name, INSERTED.ModifiedDate  
        INTO @MyTableVar  
VALUES (N'Operator error', GETDATE());  
  
--Display the result set of the table variable.  
SELECT NewScrapReasonID, Name, ModifiedDate FROM @MyTableVar;  
--Display the result set of the table.  
SELECT ScrapReasonID, Name, ModifiedDate   
FROM Production.ScrapReason;  
```  
  
#### <a name="u-using-output-with-identity-and-computed-columns"></a>U. 将 OUTPUT 用于标识列和计算列  
 下面的示例创建 `EmployeeSales` 表，然后使用 INSERT 语句向其中插入若干行，并使用 SELECT 语句从源表中检索数据。 `EmployeeSales` 表包含标识列 (`EmployeeID`) 和计算列 (`ProjectedSales`)。 由于这些值是在插入操作期间由[!INCLUDE[ssDE](../../includes/ssde-md.md)]生成的，因此不能在 `@MyTableVar` 中定义上述两列。  
  
```sql
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   int IDENTITY (1,5)NOT NULL,  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  CurrentSales money NOT NULL,  
  ProjectedSales AS CurrentSales * 1.10   
);  
GO  
DECLARE @MyTableVar table(  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  CurrentSales money NOT NULL  
  );  
  
INSERT INTO dbo.EmployeeSales (LastName, FirstName, CurrentSales)  
  OUTPUT INSERTED.LastName,   
         INSERTED.FirstName,   
         INSERTED.CurrentSales  
  INTO @MyTableVar  
    SELECT c.LastName, c.FirstName, sp.SalesYTD  
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY c.LastName, c.FirstName;  
  
SELECT LastName, FirstName, CurrentSales  
FROM @MyTableVar;  
GO  
SELECT EmployeeID, LastName, FirstName, CurrentSales, ProjectedSales  
FROM dbo.EmployeeSales;  
```  
  
#### <a name="v-inserting-data-returned-from-an-output-clause"></a>V. 插入从 OUTPUT 子句返回的数据  
 下面的示例捕获从 MERGE 语句的 OUTPUT 子句返回的数据，并将该数据插入到另一个表中。 MERGE 语句根据在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `Quantity` 表中处理的订单每天更新 `ProductInventory` 表的 `SalesOrderDetail` 列。 它还删除库存降为 0 的产品所在的行。 本示例捕获已删除的行并将这些行插入另一个表 `ZeroInventory` 中，该表跟踪没有库存的产品。  
  
```sql
--Create ZeroInventory table.  
CREATE TABLE Production.ZeroInventory (DeletedProductID int, RemovedOnDate DateTime);  
GO  
  
INSERT INTO Production.ZeroInventory (DeletedProductID, RemovedOnDate)  
SELECT ProductID, GETDATE()  
FROM  
(   MERGE Production.ProductInventory AS pi  
    USING (SELECT ProductID, SUM(OrderQty) FROM Sales.SalesOrderDetail AS sod  
           JOIN Sales.SalesOrderHeader AS soh  
           ON sod.SalesOrderID = soh.SalesOrderID  
           AND soh.OrderDate = '20070401'  
           GROUP BY ProductID) AS src (ProductID, OrderQty)  
    ON (pi.ProductID = src.ProductID)  
    WHEN MATCHED AND pi.Quantity - src.OrderQty <= 0  
        THEN DELETE  
    WHEN MATCHED  
        THEN UPDATE SET pi.Quantity = pi.Quantity - src.OrderQty  
    OUTPUT $action, deleted.ProductID) AS Changes (Action, ProductID)  
WHERE Action = 'DELETE';  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were inserted';  
GO  
SELECT DeletedProductID, RemovedOnDate FROM Production.ZeroInventory;  
```  

#### <a name="w-inserting-data-using-the-select-option"></a>W. 使用 SELECT 选项插入数据  
 以下示例说明如何使用 INSERT 语句通过 SELECT 选项插入多行数据。 第一个 `INSERT` 语句使用 `SELECT` 语句直接从源表中检索数据，然后将结果集存储在 `EmployeeTitles` 表中。  
  
```sql
CREATE TABLE EmployeeTitles  
( EmployeeKey   INT NOT NULL,  
  LastName     varchar(40) NOT NULL,  
  Title      varchar(50) NOT NULL  
);  
INSERT INTO EmployeeTitles  
    SELECT EmployeeKey, LastName, Title   
    FROM ssawPDW.dbo.DimEmployee  
    WHERE EndDate IS NULL;  
```  
  
#### <a name="x-specifying-a-label-with-the-insert-statement"></a>X. 使用 INSERT 语句指定标签  
 以下示例说明了如何通过 INSERT 语句使用标签。  
  
```sql
-- Uses AdventureWorks  
  
INSERT INTO DimCurrency   
VALUES (500, N'C1', N'Currency1')  
OPTION ( LABEL = N'label1' );  
```  
  
#### <a name="y-using-a-label-and-a-query-hint-with-the-insert-statement"></a>Y. 通过 INSERT 语句使用标签和查询提示  
 此查询显示通过 INSERT 语句使用标签和查询联接提示的基本语法。 将查询提交到控制节点后，运行在计算节点上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在生成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询计划时应用哈希联接策略。 有关联接提示以及如何使用 OPTION 子句的详细信息，请参阅 [OPTION (SQL Server PDW)](../../t-sql/queries/option-clause-transact-sql.md)。  
  
```sql
-- Uses AdventureWorks  
  
INSERT INTO DimCustomer (CustomerKey, CustomerAlternateKey, 
    FirstName, MiddleName, LastName )   
SELECT ProspectiveBuyerKey, ProspectAlternateKey, 
    FirstName, MiddleName, LastName  
FROM ProspectiveBuyer p JOIN DimGeography g ON p.PostalCode = g.PostalCode  
WHERE g.CountryRegionCode = 'FR'  
OPTION ( LABEL = 'Add French Prospects', HASH JOIN);  
```  
  
## <a name="see-also"></a>另请参阅  
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)   
 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)   
 [IDENTITY（属性）&#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [NEWID (Transact-SQL)](../../t-sql/functions/newid-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)   
 [MERGE (Transact-SQL)](../../t-sql/statements/merge-transact-sql.md)   
 [OUTPUT 子句 (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md)   
 [使用插入的和删除的表](../../relational-databases/triggers/use-the-inserted-and-deleted-tables.md)  
  
