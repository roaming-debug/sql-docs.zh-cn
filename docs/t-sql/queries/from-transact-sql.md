---
title: FROM 子句，以及 JOIN、APPLY 和 PIVOT (T-SQL)
description: FROM 子句以及 JOIN、APPLY、PIVOT (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- JOIN
- FROM_TSQL
- FROM
- JOIN_TSQL
- OUTER_JOIN_TSQL
- INNER_JOIN_TSQL
- CROSS_TSQL
- CROSS_APPLY_TSQL
- APPLY_TSQL
- CROSS_JOIN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- OUTER APPLY operator
- hints [SQL Server], FROM clause
- SELECT statement [SQL Server], FROM clause
- ISO syntax
- DELETE statement [SQL Server], FROM clause
- CROSS APPLY operator
- FROM clause
- APPLY operator
- joins [SQL Server], FROM clause
- UPDATE statement [SQL Server], FROM clause
- derived tables
ms.assetid: 36b19e68-94f6-4539-aeb1-79f5312e4263
author: VanMSFT
ms.author: vanto
ms.custom: ''
ms.date: 06/01/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d1ac3e525ba2e7f3cf459a7ce0ae309d22263aa1
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172399"
---
# <a name="from-clause-plus-join-apply-pivot-transact-sql"></a>FROM 子句以及 JOIN、APPLY、PIVOT (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

在 Transact-SQL 中，FROM 子句在以下语句中可用：

- [DELETE](../statements/delete-transact-sql.md)
- [UPDATE](update-transact-sql.md)
- [SELECT](select-transact-sql.md)

SELECT 语句通常需要使用 FROM 子句。 当没有列出表列以及列出的唯一项是文本或变量或算术表达式时除外。

本文还讨论了可以在 FROM 子句中使用的以下关键字：

- [JOIN](../../relational-databases/performance/joins.md)
- APPLY
- [PIVOT](from-using-pivot-and-unpivot.md)

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
[ FROM { <table_source> } [ ,...n ] ]   
<table_source> ::=   
{  
    table_or_view_name [ FOR SYSTEM_TIME <system_time> ] [ AS ] table_alias ]   
        [ <tablesample_clause> ]   
        [ WITH ( < table_hint > [ [ , ]...n ] ) ]   
    | rowset_function [ [ AS ] table_alias ]   
        [ ( bulk_column_alias [ ,...n ] ) ]   
    | user_defined_function [ [ AS ] table_alias ]  
    | OPENXML <openxml_clause>   
    | derived_table [ [ AS ] table_alias ] [ ( column_alias [ ,...n ] ) ]   
    | <joined_table>   
    | <pivoted_table>   
    | <unpivoted_table>  
    | @variable [ [ AS ] table_alias ]  
    | @variable.function_call ( expression [ ,...n ] )   
        [ [ AS ] table_alias ] [ (column_alias [ ,...n ] ) ]     
}  
<tablesample_clause> ::=  
    TABLESAMPLE [SYSTEM] ( sample_number [ PERCENT | ROWS ] )   
        [ REPEATABLE ( repeat_seed ) ]   
  
<joined_table> ::=   
{  
    <table_source> <join_type> <table_source> ON <search_condition>   
    | <table_source> CROSS JOIN <table_source>   
    | left_table_source { CROSS | OUTER } APPLY right_table_source   
    | [ ( ] <joined_table> [ ) ]   
}  
<join_type> ::=   
    [ { INNER | { { LEFT | RIGHT | FULL } [ OUTER ] } } [ <join_hint> ] ]  
    JOIN  
  
<pivoted_table> ::=  
    table_source PIVOT <pivot_clause> [ [ AS ] table_alias ]  
  
<pivot_clause> ::=  
        ( aggregate_function ( value_column [ [ , ]...n ])   
        FOR pivot_column   
        IN ( <column_list> )   
    )   
  
<unpivoted_table> ::=  
    table_source UNPIVOT <unpivot_clause> [ [ AS ] table_alias ]  
  
<unpivot_clause> ::=  
    ( value_column FOR pivot_column IN ( <column_list> ) )   
  
<column_list> ::=  
    column_name [ ,...n ]   
  
<system_time> ::=  
{  
       AS OF <date_time>  
    |  FROM <start_date_time> TO <end_date_time>  
    |  BETWEEN <start_date_time> AND <end_date_time>  
    |  CONTAINED IN (<start_date_time> , <end_date_time>)   
    |  ALL  
}  
  
    <date_time>::=  
        <date_time_literal> | @date_time_variable  
  
    <start_date_time>::=  
        <date_time_literal> | @date_time_variable  
  
    <end_date_time>::=  
        <date_time_literal> | @date_time_variable  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
FROM { <table_source> [ ,...n ] }  
  
<table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [<tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
    | <joined_table>  
}  
  
<tablesample_clause> ::=
    TABLESAMPLE ( sample_number [ PERCENT ] ) -- Azure Synapse Analytics only  
 
<joined_table> ::=   
{  
    <table_source> <join_type> <table_source> ON search_condition   
    | <table_source> CROSS JOIN <table_source> 
    | left_table_source { CROSS | OUTER } APPLY right_table_source   
    | [ ( ] <joined_table> [ ) ]   
}  
  
<join_type> ::=   
    [ INNER ] [ <join hint> ] JOIN  
    | LEFT  [ OUTER ] JOIN  
    | RIGHT [ OUTER ] JOIN  
    | FULL  [ OUTER ] JOIN  
  
<join_hint> ::=   
    REDUCE  
    | REPLICATE  
    | REDISTRIBUTE  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
\<table_source>  
 指定要在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中使用的表、视图、表变量或派生表源（有无别名均可）。 虽然语句中可用的表源个数的限值根据可用内存和查询中其他表达式的复杂性而有所不同，但一个语句中最多可使用 256 个表源。 单个查询可能不支持最多有 256 个表源。  
  
> [!NOTE]  
>  如果查询中引用了许多表，查询性能会受到影响。 编译和优化时间也受到其他因素的影响。 这些因素包括：每个 \<table_source> 是否有索引和索引视图，以及 SELECT 语句中 \<select_list> 的大小。  
  
 表源在 FROM 关键字后的顺序不影响返回的结果集。 如果 FROM 子句中出现重复的名称，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会返回错误。  
  
 table_or_view_name  
 表或视图的名称。  
  
 如果表或视图存在于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的同一实例的另一个数据库中，请按照 database.schema.object_name 形式使用完全限定名称  。  
  
 如果表或视图不在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例中，请按照 linked_server.catalog.schema.object 的形式使用由四个部分组成的名称   。 有关详细信息，请参阅 [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)的数据。 如果由四个部分组成的名称的服务器部分使用的是 [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) 函数，则该名称也可用于指定远程表源。 如果指定 OPENDATASOURCE，则 database_name 和 schema_name 可能不适用于所有数据源，并且受到访问远程对象的 OLE DB 提供程序的性能的限制 。  
  
 [AS] table_alias  
 table_source 的别名，别名可带来使用上的方便，也可用于区分自联接或子查询中的表或视图。 别名往往是一个缩短了的表名，用于在联接中引用表的特定列。 如果联接中的多个表中存在相同的列名，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 要求使用表名、视图名或别名来限定列名。 如果定义了别名，则不能使用表名。  
  
 如果使用派生表、行集或表值函数或者运算符子句（如 PIVOT 或 UNPIVOT），则在子句结尾处必需的 table_alias 是所有返回列（包括分组列）的关联表名。  
  
 WITH (\<table_hint> )  
 指定查询优化器对此表和此语句使用优化或锁定策略。 有关详细信息，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  
  
rowset_function  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
指定其中一个行集函数（如 OPENROWSET），该函数返回可用于替代表引用的对象。 有关行集函数的列表的详细信息，请参阅[行集函数 (Transact-SQL)](../functions/opendatasource-transact-sql.md)。  
  
 使用 OPENROWSET 和 OPENQUERY 函数指定远程对象依赖于访问该对象的 OLE DB 访问接口的性能。  
  
 bulk_column_alias  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
 代替结果集内列名的可选别名。 只允许在使用 OPENROWSET 函数和 BULK 选项的 SELECT 语句中使用列别名。 使用 bulk_column_alias 时，为每个表列指定别名，顺序与这些列在文件中的顺序相同。  
  
> [!NOTE]  
>  此别名覆盖 XML 格式化文件的 COLUMN 元素中的 NAME 属性（如果有该属性）。  
  
 user_defined_function  
 指定表值函数。  
  
OPENXML \<openxml_clause>  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
通过 XML 文档提供行集视图。 有关详细信息，请参阅 [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)。  
  
 derived_table  
 从数据库中检索行的子查询。 derived_table 用作外部查询的输入。  
  
 derived _table 可以使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表值构造函数功能来指定多个行。 例如，`SELECT * FROM (VALUES (1, 2), (3, 4), (5, 6), (7, 8), (9, 10) ) AS MyTable(a, b);`。 有关详细信息，请参阅[表值构造函数 (Transact-SQL)](../../t-sql/queries/table-value-constructor-transact-sql.md)。  
  
 column_alias  
 代替派生表的结果集内列名的可选别名。 在选择列表中的每个列包括一个列别名，并将整个列别名列表用圆括号括起来。  
  
 table_or_view_name FOR SYSTEM_TIME \<system_time> 适用于：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]（及更高版本）和 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
指定从指定时态表及其链接的系统版本控制的历史记录表返回特定版本的数据  
  
### <a name="tablesample-clause"></a>Tablesample 子句
**适用于：** SQL Server、SQL 数据库 - 指定返回来自表的数据样本。 该样本可以是近似的。 此子句可对 SELECT 或 UPDATE 语句中的任何主表或联接表使用。 不能对视图指定 TABLESAMPLE。  
  
> [!NOTE]  
>  对升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的数据库使用 TABLESAMPLE 时，数据库的兼容级别必须设置为 110 或更高，在递归公用表表达式 (CTE) 查询中不允许 PIVOT。 有关详细信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。  
  
 SYSTEM  
 ISO 标准指定的依赖于实现的抽样方法。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，这是唯一可用的抽样方法，并且是默认应用的方法。 SYSTEM 应用基于页的抽样方法，即从表中选择一组随机页作为样本，这些页上的所有行作为样本子集返回。  
  
 sample_number  
 表示行的百分比或行数的精确或近似的常量数值表达式。 使用 PERCENT 指定时，sample_number 被隐式转换为 float 值；否则，它被转换为 bigint 。 PERCENT 是默认设置。  
  
 PERCENT  
 指定应该从表中检索表行的 sample_number 百分比。 指定 PERCENT 时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回指定的百分比的近似值。 指定 PERCENT 时，sample_number 表达式的结果必须是 0 到 100 之间的值。  
  
 ROWS  
 指定将检索的行的近似 sample_number。 指定 ROWS 时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回指定的行数的近似值。 指定 ROWS 时，sample_number 表达式的结果必须是大于零的整数值。  
  
 REPEATABLE  
 指示可以再次返回选定的样本。 使用同一个 repeat_seed 值指定时，只要对表中任何行尚未进行更改，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 就会返回相同的行集。 使用其他 repeat_seed 值指定时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 很可能将返回表中行的某些不同样本。 对表的以下操作可视为更改：插入、更新、删除、索引重新生成或碎片整理以及数据库还原或附加。  
  
 repeat_seed  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用于生成随机数的常量整数表达式。 repeat_seed 是 bigint。 如果未指定 repeat_seed，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将随机分配值。 对于特定的 repeat_seed 值，如果尚未对表应用任何更改，抽样结果始终相同。 repeat_seed 表达式的值必须是大于零的整数。  
  
### <a name="tablesample-clause"></a>Tablesample 子句
**适用于：** [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]

 指定返回来自表的数据样本。 该样本可以是近似的。 此子句可对 SELECT 或 UPDATE 语句中的任何主表或联接表使用。 不能对视图指定 TABLESAMPLE。 

 PERCENT  
 指定应该从表中检索表行的 sample_number 百分比。 指定 PERCENT 时，[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 返回指定的百分比的近似值。 指定 PERCENT 时，sample_number 表达式的结果必须是 0 到 100 之间的值。  


### <a name="joined-table"></a>联接的表 
联接的表是由两个或更多表的积构成的结果集。 对于多个联接，请使用圆括号来更改联接的自然顺序。  
  
### <a name="join-type"></a>联接类型
指定联接操作的类型。  
  
 INNER  
 指定返回所有匹配的行对。 放弃两个表中不匹配的行。 如果未指定任何联接类型，此设置为默认设置。  
  
 FULL [ OUTER ]  
 指定在结果集中包括左表或右表中不满足联接条件的行，并将对应于另一个表的输出列设为 NULL。 这是对通常由 INNER JOIN 返回的所有行的补充。  
  
 LEFT [ OUTER ]  
 指定在结果集中包括左表中所有不满足联接条件的行，除了由内部联接返回所有的行之外，还将另外一个表的输出列设置为 NULL。  
  
 RIGHT [OUTER]  
 指定在结果集中包括右表中所有不满足联接条件的行，除了由内部联接返回所有的行之外，还将与另外一个表对应的输出列设置为 NULL。  
  
### <a name="join-hint"></a>联接提示  
对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]，指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器为在查询的 FROM 子句中指定的每个联接使用一个联接提示或执行算法。 有关详细信息，请参阅[联接提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-join.md)。  
  
 对于 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]，这些联接提示应用于两个分布不兼容列上的 INNER 联接。 它们可以通过限制查询处理期间发生的数据移动量来提高查询性能。 允许用于 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]和[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]的联接提示如下：  
  
 REDUCE  
 减少联接右侧表中要移动的行的数量，以使两个分布不兼容表实现兼容。 REDUCE 提示也称为半联接提示。  
  
 REPLICATE  
 导致联接左侧表中的联接列中的值复制到所有节点。 右侧表将与这些列的复制版本进行联接。  
  
 REDISTRIBUTE  
 强制两个数据源分布到 JOIN 子句中指定的列上。 对于分布式表，[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 将执行 shuffle 移动。 对于复制的表，[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]将执行 trim 移动。 要了解这些移动类型，请参阅 [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] 中“了解查询计划”主题中的“DMS 查询计划操作”部分。 当查询计划使用 broadcast 移动解决分布不兼容联接时，此提示可提高性能。  
  
 JOIN  
 指示指定的联接操作应在指定的表源或视图之间执行。  
  
 ON \<search_condition>  
 指定联接所基于的条件。 虽然常常使用列运算符和比较运算符，但该条件可指定任何谓词，例如：  
  
```sql
SELECT p.ProductID, v.BusinessEntityID  
FROM Production.Product AS p   
JOIN Purchasing.ProductVendor AS v  
ON (p.ProductID = v.ProductID);  
  
```  
  
 当该条件指定列时，列不必具有相同的名称或数据类型；但是，如果数据类型不相同，则这些列要么必须相互兼容，要么是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 能够隐式转换的类型。 如果数据类型不能隐式转换，则在条件中必须使用 CONVERT 函数显式转换数据类型。  
  
 在 ON 子句中可能有仅涉及一个联接表的谓词。 这样的谓词也可能出现在查询中的 WHERE 子句中。 虽然这种谓词的放置对于 INNER 联接不会产生差别，但是在涉及 OUTER 联接时可能会导致不同的结果。 这是因为 ON 子句中的谓词在应用于联接之前先应用于表，而 WHERE 子句在语义上应用于联接结果。  
  
 有关搜索条件和谓词的详细信息，请参阅[搜索条件 (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md)。  
  
 CROSS JOIN  
 指定两个表的叉积。 返回相同的行，就好像在旧式的非 SQL-92 式联接中并没有指定 WHERE 子句。  
  
 left_table_source { CROSS | OUTER } APPLY right_table_source   
 指定针对 left_table_source 的每行，对 APPLY 运算符的 right_table_source 求值 。 当 right_table_source 包含从 left_table_source 取列值作为其参数之一的表值函数时，此功能很有用 。  
  
 必须使用 APPLY 指定 CROSS 或 OUTER。 如果指定 CROSS，针对 left_table_source 的指定行对 right_table_source 求值，且返回了空的结果集，则不生成任何行 。  
  
 如果指定 OUTER，则为 left_table_source 的每行生成一行，即使在针对该行对 right_table_source 求值且返回空的结果集时也是如此 。  
  
 有关详细信息，请参见“备注”部分。  
  
 left_table_source  
 上一个参数中定义的一个表源。 有关详细信息，请参见“备注”部分。  
  
 right_table_source  
 上一个参数中定义的一个表源。 有关详细信息，请参见“备注”部分。  
  
### <a name="pivot-clause"></a>PIVOT 子句

 table_source PIVOT \<pivot_clause>  
 指定基于 pivot_column 透视 table_source 。 table_source 是一个表或表表达式。 输出是包含 table_source 的所有列（pivot_column 和 value_column 除外）的表  。 table_source 中的列（pivot_column 和 value_column 除外）称为透视运算符的分组列  。 有关 PIVOT 和 UNPIVOT 的详细信息，请参阅[使用 PIVOT 和 UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md)。  
  
 PIVOT 对输入表执行分组列的分组操作，并为每个组返回一行。 此外，input_table 的 pivot_column 中显示的 column_list 中指定的每个值，输出中都对应一列  。  
  
 有关详细信息，请参见后面的“备注”部分。  
  
 aggregate_function  
 接受一个或多个输入的系统聚合函数或用户定义的聚合函数。 聚合函数应该对 Null 值固定不变。 对 Null 值固定不变的聚合函数在求聚合值时不考虑组中的 Null 值。  
  
 不允许使用 COUNT(*) 系统聚合函数。  
  
 value_column  
 PIVOT 运算符的值列。 与 UNPIVOT 一起使用时，value_column 不能是输入 table_source 中的现有列的名称 。  
  
 FOR pivot_column  
 PIVOT 运算符的透视列。 pivot_column 的数据类型必须可隐式或显式转换为 nvarchar()。 此列不能为 image 或 rowversion 。  
  
 使用 UNPIVOT 时，pivot_column 是从 table_source 中提取的输出列的名称 。 table_source 中不能有具有该名称的现有列。  
  
 IN (column_list )  
 在 PIVOT 子句中，列出 pivot_column 中将成为输出表的列名的值。 该列表不能指定被透视的输入 table_source 中已存在的任何列名。  
  
 在 UNPIVOT 子句中，列出 table_source 中将被提取到单个 pivot_column 中的列 。  
  
 table_alias  
 输出表的别名。 必须指定 pivot_table_alias。  
  
 UNPIVOT \<unpivot_clause>  
 指定输入表从 column_list 中的多个列缩减为名为 pivot_column 的单个列 。 有关 PIVOT 和 UNPIVOT 的详细信息，请参阅[使用 PIVOT 和 UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md)。  
  
AS OF \<date_time>  
**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本和 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
返回一个表，其中为包含过去指定时间点的实际（当前）值的每个行提供一条记录。 在内部，将在临时表及其历史记录表之间进行联合，然后筛选结果以返回在 \<date_time> 参数指定的时间点有效的行中的值。 如果 system_start_time_column_name 值小于或等于 \<date_time> 参数值，并且 system_end_time_column_name 值大于 \<date_time> 参数值，则此行的值视为有效   。   
  
从 \<start_date_time> 至 \<end_date_time>
**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]（及更高版本）和 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。
返回一个表，其中包含在指定的时间范围内保持活动状态的所有记录版本的值，不管这些版本是在 FROM 自变量的 \<start_date_time> 参数之前开始活动，还是在 TO 自变量的 \<end_date_time> 参数值之后停止活动 。 在内部，将在临时表及其历史记录表之间进行联合，然后筛选结果，以返回在指定时间范围内任意时间保持活动状态的所有行版本的值。 正好在 FROM 终结点定义的下限时间激活的行将包括在内，正好在 TO 终结点定义的上限时间激活的行将被排除。  
  
BETWEEN \<start_date_time> AND \<end_date_time>  
**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本和 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
与上面的 FROM \<start_date_time> TO \<end_date_time> 描述相同，不过，它包括 \<end_date_time> 终结点定义的上限时间激活的行。  
  
CONTAINED IN (\<start_date_time> , \<end_date_time>)  
**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本和 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
返回一个表，其中包含在 CONTAINED IN 参数的两个日期时间值定义的时间范围内打开和关闭的所有记录版本的值。 正好在下限时间激活的记录，或者在上限时间停止活动的行将包括在内。  
  
ALL  
返回具有当前表和历史记录表中所有行中的值的表。  
  
## <a name="remarks"></a>备注  
 FROM 子句支持用于联接表和派生表的 SQL-92-SQL 语法。 SQL-92 语法提供 INNER、LEFT OUTER、RIGHT OUTER、FULL OUTER 和 CROSS 联接运算符。  
  
 视图、派生表和子查询中均支持 FROM 子句内的 UNION 和 JOIN。  
  
 自联接是与自身联接的表。 基于自联接的插入和更新操作遵循 FROM 子句中的顺序。  
  
 因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会考虑来自提供列分布统计信息的链接服务器的分布及基数统计信息，所以，无需 REMOTE 联接提示来强制远程评估联接。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询处理器将考虑远程统计信息，并确定远程联接策略是否适当。 REMOTE 联接提示对不提供列分发统计信息的提供程序非常有用。  
  
## <a name="using-apply"></a>使用 APPLY  
 APPLY 运算符的左操作数和右操作数都是表表达式。 这些操作数之间的主要区别是，right_table_source 可以使用表值函数，该函数可从 left_table_source 获取一个列作为函数的参数之一 。 left_table_source 可以包括表值函数，但不能以来自 right_table_source 的列作为参数 。  
  
APPLY 运算符通过以下方式工作，以便为 FROM 子句生成表源：  
  
1.  针对 left_table_source 的每一行计算 right_table_source 以生成行集 。  
  
    right_table_source 中的值取决于 left_table_source 。 right_table_source 可以按以下方式近似表示：`TVF(left_table_source.row)`，其中，`TVF` 是表值函数。  
  
2.  通过执行 UNION ALL 操作，将计算 right_table_source 的值时为每行生成的结果集与 left_table_source 组合起来 。  
  
    APPLY 运算符的结果生成的列的列表是来自 left_table_source（与来自 right_table_source 的列的列表相组合）的一组列 。  
  
## <a name="using-pivot-and-unpivot"></a>使用 PIVOT 和 UNPIVOT  
 pivot_column 和 value_column 是 PIVOT 运算符使用的分组列 。 PIVOT 遵循以下过程获得输出结果集：  
  
1.  对分组列的 input_table 执行 GROUP BY，为每个组生成一个输出行。  
  
     输出行中的分组列获得 input_table 中该组的对应列值。  
  
2.  通过执行以下操作，为每个输出行生成列列表中的列的值：  
  
    1.  针对 pivot_column，对上一步在 GROUP BY 中生成的行另外进行分组。  
  
         对于 column_list 中的每个输出列，选择满足以下条件的子组：  
  
         `pivot_column = CONVERT(<data type of pivot_column>, 'output_column')`  
  
    2.  针对此子组上的 value_column 对 aggregate_function 求值，其结果作为相应的 output_column 的值返回  。 如果该子组为空，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将为该 output_column 生成 NULL 值。 如果聚合函数是 COUNT，且子组为空，则返回零 (0)。  

> [!NOTE]
> `UNPIVOT` 子句中的列标识符需遵循目录排序规则。 对于 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]，排序规则始终是 `SQL_Latin1_General_CP1_CI_AS`。 对于 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 部分包含的数据库，排序规则始终是 `Latin1_General_100_CI_AS_KS_WS_SC`。 如果将该列与与其他列合并，则需要 collate 子句 (`COLLATE DATABASE_DEFAULT`) 以避免冲突。   
  
 有关 PIVOT 和 UNPIVOT 及示例的详细信息，请参阅[使用 PIVOT 和 UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md)。  
  
## <a name="permissions"></a>权限  
 需要 DELETE、SELECT 或 UPDATE 语句的权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-a-simple-from-clause"></a>A. 使用简单 FROM 子句  
 下面的示例从 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 示例数据库中的 `TerritoryID` 表中检索 `Name` 和 `SalesTerritory` 列。  
  
```sql    
SELECT TerritoryID, Name  
FROM Sales.SalesTerritory  
ORDER BY TerritoryID ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
TerritoryID Name                            
----------- ------------------------------  
1           Northwest                       
2           Northeast                       
3           Central                         
4           Southwest                       
5           Southeast                       
6           Canada                          
7           France                          
8           Germany                         
9           Australia                       
10          United Kingdom                  
(10 row(s) affected)  
```  
  
### <a name="b-using-the-tablock-and-holdlock-optimizer-hints"></a>B. 使用 TABLOCK 和 HOLDLOCK 优化器提示  
 下面的部分事务说明了如何在 `Employee` 上放置一个显式共享表锁，以及如何读取索引。 该锁将在整个事务中被持有。  
  
```sql    
BEGIN TRAN  
SELECT COUNT(*)   
FROM HumanResources.Employee WITH (TABLOCK, HOLDLOCK) ;  
```  
  
### <a name="c-using-the-sql-92-cross-join-syntax"></a>C. 使用 SQL-92 CROSS JOIN 语法  
 下面的示例返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中 `Employee` 和 `Department` 这两个表的叉积。 包含所返回的 `BusinessEntityID` 行和所有 `Department`名称行的所有可能组合的列表。  
  
```sql    
SELECT e.BusinessEntityID, d.Name AS Department  
FROM HumanResources.Employee AS e  
CROSS JOIN HumanResources.Department AS d  
ORDER BY e.BusinessEntityID, d.Name ;  
```  
  
### <a name="d-using-the-sql-92-full-outer-join-syntax"></a>D. 使用 SQL-92 FULL OUTER JOIN 语法  
 下面的示例返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中产品名称以及 `SalesOrderDetail` 表中任何对应的销售订单。 该示例还将返回在 `Product` 表中没有列出产品的任何销售订单，以及销售订单不同于在 `Product` 表中列出的销售订单的任何产品。  
  
```sql  
-- The OUTER keyword following the FULL keyword is optional.  
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
FULL OUTER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="e-using-the-sql-92-left-outer-join-syntax"></a>E. 使用 SQL-92 LEFT OUTER JOIN 语法  
 下面的示例基于 `ProductID` 联接两个表，并保留左表中不匹配的行。 `Product` 表与每个表中的 `SalesOrderDetail` 列上的 `ProductID` 表相匹配。 所有产品，无论是否已订购，都将在结果集中显示。  
  
```sql    
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
LEFT OUTER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="f-using-the-sql-92-inner-join-syntax"></a>F. 使用 SQL-92 INNER JOIN 语法  
 下面的示例返回所有产品名称和销售订单 ID。  
  
```sql    
-- By default, SQL Server performs an INNER JOIN if only the JOIN   
-- keyword is specified.  
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
INNER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="g-using-the-sql-92-right-outer-join-syntax"></a>G. 使用 SQL-92 RIGHT OUTER JOIN 语法  
 下面的示例基于 `TerritoryID` 联接两个表，并保留右表中不匹配的行。 `SalesTerritory` 表与每个表中的 `TerritoryID` 列上的 `SalesPerson` 表相匹配。 不论是否分配了区域，所有销售人员均在结果集中显示。  
  
```sql    
SELECT st.Name AS Territory, sp.BusinessEntityID  
FROM Sales.SalesTerritory AS st   
RIGHT OUTER JOIN Sales.SalesPerson AS sp  
ON st.TerritoryID = sp.TerritoryID ;  
```  
  
### <a name="h-using-hash-and-merge-join-hints"></a>H. 使用 HASH 和 MERGE 联接提示  
 下面的示例在 `Product`、`ProductVendor` 和 `Vendor` 表之间执行三表联接，生成产品及其供应商的列表。 查询优化器使用 MERGE 联接来联接 `Product` 和 `ProductVendor`（`p` 和 `pv`）。 然后，`Product` 和 `ProductVendor` MERGE 联接（`p` 和 `pv`）的结果被 HASH 联接到 `Vendor` 表，生成（`p` 和 `pv`）和 `v`。  
  
> [!IMPORTANT]  
>  指定了联接提示后，要执行 INNER JOIN 时，INNER 关键字不再为可选，而必须显式说明。  
  
```sql    
SELECT p.Name AS ProductName, v.Name AS VendorName  
FROM Production.Product AS p   
INNER MERGE JOIN Purchasing.ProductVendor AS pv   
ON p.ProductID = pv.ProductID  
INNER HASH JOIN Purchasing.Vendor AS v  
ON pv.BusinessEntityID = v.BusinessEntityID  
ORDER BY p.Name, v.Name ;  
```  
  
### <a name="i-using-a-derived-table"></a>I. 使用派生表  
 下面的示例使用派生表（`SELECT` 子句后的 `FROM` 语句）返回所有员工的名字和姓氏及其居住的城市。  
  
```sql    
SELECT RTRIM(p.FirstName) + ' ' + LTRIM(p.LastName) AS Name, d.City  
FROM Person.Person AS p  
INNER JOIN HumanResources.Employee e ON p.BusinessEntityID = e.BusinessEntityID   
INNER JOIN  
   (SELECT bea.BusinessEntityID, a.City   
    FROM Person.Address AS a  
    INNER JOIN Person.BusinessEntityAddress AS bea  
    ON a.AddressID = bea.AddressID) AS d  
ON p.BusinessEntityID = d.BusinessEntityID  
ORDER BY p.LastName, p.FirstName;  
```  
  
### <a name="j-using-tablesample-to-read-data-from-a-sample-of-rows-in-a-table"></a>J. 使用 TABLESAMPLE 从表中的行样本中读取数据  
 下面的示例在 `TABLESAMPLE` 子句中使用 `FROM`，大约返回 `10` 表中所有行的 `Customer`%。  
  
```sql    
SELECT *  
FROM Sales.Customer TABLESAMPLE SYSTEM (10 PERCENT) ;  
```  
  
### <a name="k-using-apply"></a>K. 使用 APPLY  
以下示例假定数据库中存在以下表和表值函数：  

|对象名称|列名|      
|---|---|   
|Departments|DeptID、DivisionID、DeptName、DeptMgrID|      
|EmpMgr|MgrID、EmpID|     
|Employees|EmpID、EmpLastName、EmpFirstName、EmpSalary|  
|GetReports(MgrID)|EmpID、EmpLastName、EmpSalary|     
  
`GetReports` 表值函数返回直接或间接报告给指定 `MgrID` 的所有员工的列表。  
  
该示例使用 `APPLY` 返回所有部门和部门中的所有员工。 如果某个部门没有任何员工，则将不返回该部门的任何行。  
  
```sql
SELECT DeptID, DeptName, DeptMgrID, EmpID, EmpLastName, EmpSalary  
FROM Departments d    
CROSS APPLY dbo.GetReports(d.DeptMgrID) ;  
```  
  
如果您希望查询为那些没有员工的部门生成行（这将为 `EmpID`、`EmpLastName` 和 `EmpSalary` 列生成 Null 值），请改用 `OUTER APPLY`。  
  
```sql
SELECT DeptID, DeptName, DeptMgrID, EmpID, EmpLastName, EmpSalary  
FROM Departments d   
OUTER APPLY dbo.GetReports(d.DeptMgrID) ;  
```  
  
### <a name="l-using-cross-apply"></a>L. 使用 CROSS APPLY  
以下示例将检索驻留在计划缓存中的所有查询计划的快照，方法是通过查询 `sys.dm_exec_cached_plans` 动态管理视图来检索缓存中所有查询计划的计划句柄。 然后，指定 `CROSS APPLY` 运算符以将计划句柄传递给 `sys.dm_exec_query_plan`。 当前在计划缓存中的每个计划的 XML 显示计划输出位于返回的表的 `query_plan` 列中。  
  
```sql
USE master;  
GO  
SELECT dbid, object_id, query_plan   
FROM sys.dm_exec_cached_plans AS cp   
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);   
GO  
```  
  
### <a name="m-using-for-system_time"></a>M. 使用 FOR SYSTEM_TIME  
  
**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本和 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
下面的示例使用 FOR SYSTEM_TIME AS OF date_time_literal_or_variable 参数返回自 2014 年 1 月 1 日起的活跃（最新）表行。  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME AS OF '2014-01-01'  
WHERE ManagerID = 5;
```  
  
下面的示例使用 FOR SYSTEM_TIME FROM date_time_literal_or_variable TO date_time_literal_or_variable 参数返回定义期限（从 2013 年 1 月 1 日开始，到 2014 年 1 月 1 日截止，不包括上限时间）内的所有活跃行。  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME FROM '2013-01-01' TO '2014-01-01'  
WHERE ManagerID = 5;
```  
  
 下面的示例使用 FOR SYSTEM_TIME BETWEEN date_time_literal_or_variable AND date_time_literal_or_variable 参数返回定义期限（从 2013 年 1 月 1 日开始，到 2014 年 1 月 1 日截止，包括上限时间）内的所有活跃行。  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME BETWEEN '2013-01-01' AND '2014-01-01'  
WHERE ManagerID = 5;
```  
  
 下面的示例使用 FOR SYSTEM_TIME CONTAINED IN ( date_time_literal_or_variable, date_time_literal_or_variable ) 参数返回定义期限（从 2013 年 1 月 1 日开始，到 2014 年 1 月 1 日截止，包括上限时间）内的所有开放和关闭的行。  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME CONTAINED IN ( '2013-01-01', '2014-01-01' )  
WHERE ManagerID = 5;
```  
  
 下面的示例使用变量（而不是文本）为查询提供日期边界值。  
  
```sql
DECLARE @AsOfFrom datetime2 = dateadd(month,-12, sysutcdatetime());
DECLARE @AsOfTo datetime2 = dateadd(month,-6, sysutcdatetime());
  
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME FROM @AsOfFrom TO @AsOfTo  
WHERE ManagerID = 5;
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="n-using-the-inner-join-syntax"></a>N. 使用 INNER JOIN 语法  
 下面的示例返回 `FactInternetSales` 和 `DimProduct` 表（两个表中的联接键 `ProductKey` 相互匹配）中的 `SalesOrderNumber`、`ProductKey` 和 `EnglishProductName` 列。 `SalesOrderNumber` 和 `EnglishProductName` 列分别仅存在于一个表中，因此无需指定具有这些列的表别名（如下所示）；包含这些别名可提高可读性。 别名前的 AS 一词不是必需的，但为提高可读性和符合 ANSI 标准，建议使用该词。  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
INNER JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 由于内部联接无需 `INNER` 关键词，此相同的查询可写为：  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
JOIN DimProduct AS dp  
ON dp.ProductKey = fis.ProductKey;  
```  
  
 `WHERE` 子句还可能与此查询一起使用来限制结果。 此示例将结果限制为高于“SO5000”的 `SalesOrderNumber` 值：  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey  
WHERE fis.SalesOrderNumber > 'SO50000'  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="o-using-the-left-outer-join-and-right-outer-join-syntax"></a>O. 使用 LEFT OUTER JOIN 和 RIGHT OUTER JOIN 语法  
 下面的示例联接 `ProductKey` 列上的 `FactInternetSales` 和 `DimProduct` 表。 左外部联接语法保留左 (`FactInternetSales`) 表中的不匹配行。 由于 `FactInternetSales` 表不包含与 `DimProduct` 表匹配的任何 `ProductKey` 值，此查询将返回与上述的第一个内联示例相同的行。  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
LEFT OUTER JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 编写此查询时也可以不包含 `OUTER` 关键词。  
  
 在右外部联接中，右表中的不匹配行将会保留。 下面的示例将返回与上述的左外部联接示例相同的行。  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM DimProduct AS dp 
RIGHT OUTER JOIN FactInternetSales AS fis  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 以下查询使用 `DimSalesTerritory` 表作为左外部联接中的左表。 它从 `FactInternetSales` 表中检索 `SalesOrderNumber` 值。 如果没有针对特定 `SalesTerritoryKey` 的顺序，该查询将针对该行中的 `SalesOrderNumber` 返回 NULL。 此查询将按照 `SalesOrderNumber` 列进行排序，以便此列中的任何 NULL 都会显示在结果顶部。  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
LEFT OUTER JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
 可使用右外部联接改写此查询以检索相同结果：  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM FactInternetSales AS fis 
RIGHT OUTER JOIN DimSalesTerritory AS dst  
    ON fis.SalesTerritoryKey = dst.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="p-using-the-full-outer-join-syntax"></a>P. 使用 FULL OUTER JOIN 语法  
 下面的示例演示完全外部联接，它将返回两个联接表中的所有行，但会针对其他表中的不匹配值返回 NULL。  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
FULL OUTER JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
 编写此查询时也可以不包含 `OUTER` 关键词。  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
FULL JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="q-using-the-cross-join-syntax"></a>Q. 使用 CROSS JOIN 语法  
 下面的示例返回 `FactInternetSales` 和 `DimSalesTerritory` 表的叉积。 包含所返回的 `SalesOrderNumber` 和 `SalesTerritoryKey` 的所有可能组合的列表。 请注意，交叉联接查询中缺少 `ON` 子句。  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
CROSS JOIN FactInternetSales AS fis  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="r-using-a-derived-table"></a>R. 使用派生表  
 下面的示例使用派生表（`FROM` 子句后的 `SELECT` 语句）返回 `DimCustomer` 表（具有 1970 年 1 月 1 日以后的 `BirthDate` 值和姓氏“Smith”）中所有客户的 `CustomerKey` 和 `LastName` 列。  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey, LastName  
FROM  
   (SELECT * FROM DimCustomer  
    WHERE BirthDate > '01/01/1970') AS DimCustomerDerivedTable  
WHERE LastName = 'Smith'  
ORDER BY LastName;  
```  
  
### <a name="s-reduce-join-hint-example"></a>S. REDUCE 联接提示示例  
 下面的示例使用 `REDUCE` 联接提示更改对查询内的派生表的处理。 此查询中使用 `REDUCE` 联接提示时，`fis.ProductKey` 将被投影、复制和进行区别处理，然后在对 `ProductKey` 上的 `DimProduct` 执行 shuffle 时联接到 `DimProduct`。 生成的派生表将分布在 `fis.ProductKey` 上。  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN SELECT SalesOrderNumber  
FROM  
   (SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
    FROM DimProduct AS dp   
      INNER REDUCE JOIN FactInternetSales AS fis   
          ON dp.ProductKey = fis.ProductKey  
   ) AS dTable  
ORDER BY SalesOrderNumber;  
```  
  
### <a name="t-replicate-join-hint-example"></a>T. REPLICATE 联接提示示例  
 本示例演示与上一示例相同的查询，但使用的是 `REPLICATE` 联接提示，而非 `REDUCE` 联接提示。 使用 `REPLICATE` 提示会导致 `FactInternetSales` 表中的 `ProductKey`（联接）列复制到所有节点。 `DimProduct` 表将与这些值的复制版本进行联接。  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN SELECT SalesOrderNumber  
FROM  
   (SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
    FROM DimProduct AS dp   
      INNER REPLICATE JOIN FactInternetSales AS fis  
          ON dp.ProductKey = fis.ProductKey  
   ) AS dTable  
ORDER BY SalesOrderNumber;  
```  
  
### <a name="u-using-the-redistribute-hint-to-guarantee-a-shuffle-move-for-a-distribution-incompatible-join"></a>U. 使用 REDISTRIBUTE 提示以保证对分布不兼容联接使用 Shuffle 移动  
 下面的查询针对分布不兼容联接使用 REDISTRIBUTE 查询提示。 这能确保查询优化器在查询计划中使用 Shuffle 移动。 这还可确保查询计划不会使用可将分布表移动到复制表的 Broadcast 移动。  
  
 在下一示例中，REDISTRIBUTE 提示强制对 FactInternetSales 表执行 Shuffle 移动，因为 ProductKey 是 DimProduct 的分布列，但不是 FactInternetSales 的分布列。  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN  
SELECT dp.ProductKey, fis.SalesOrderNumber, fis.TotalProductCost  
FROM DimProduct AS dp 
INNER REDISTRIBUTE JOIN FactInternetSales AS fis  
    ON dp.ProductKey = fis.ProductKey;  
```  

### <a name="v-using-tablesample-to-read-data-from-a-sample-of-rows-in-a-table"></a>V. 使用 TABLESAMPLE 从表中的行样本中读取数据  
 下面的示例在 `TABLESAMPLE` 子句中使用 `FROM`，大约返回 `10` 表中所有行的 `Customer`%。  
  
```sql    
SELECT *  
FROM Sales.Customer TABLESAMPLE SYSTEM (10 PERCENT) ;
```
  
## <a name="see-also"></a>另请参阅  
 [CONTAINSTABLE (Transact-SQL)](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [OPENQUERY (Transact-SQL)](../../t-sql/functions/openquery-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)
