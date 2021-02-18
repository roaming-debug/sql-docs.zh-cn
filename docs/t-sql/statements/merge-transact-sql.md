---
description: MERGE (Transact-SQL)
title: MERGE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- MERGE
- MERGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- updating data [SQL Server]
- modifying data [SQL Server], MERGE statement
- MERGE statement [SQL Server]
- adding data
- DML [SQL Server], MERGE statement
- table modifications [SQL Server], MERGE statement
- data manipulation language [SQL Server], MERGE statement
- inserting data
ms.assetid: c17996d6-56a6-482f-80d8-086a3423eecc
author: XiaoyuMSFT
ms.author: XiaoyuL
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017 ||  azure-sqldw-latest
ms.openlocfilehash: 6bb1014c22353826b6e4429726d4d28549cc274a
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489331"
---
# <a name="merge-transact-sql"></a>MERGE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb-asa.md)]

根据与源表联接的结果，对目标表运行插入、更新或删除操作。 例如，根据与另一个表的区别，在一个表中插入、更新或删除行，从而同步两个表。 

> [!NOTE]
> 对于 Azure Synapse Analytics，MERGE 目前为预览版。
  
**性能提示：** 当两个表具有匹配特性的复杂混合时，针对 MERGE 语句介绍的条件行为的效果最佳。 例如，插入不存在的行，或更新匹配的行。 仅根据另一个表的行更新一个表时，通过基本的 INSERT、UPDATE 和 DELETE 语句提升性能和可伸缩性。 例如：  
  
```sql
INSERT tbl_A (col, col2)  
SELECT col, col2
FROM tbl_B
WHERE NOT EXISTS (SELECT col FROM tbl_A A2 WHERE A2.col = tbl_B.col);  
```  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql

-- SQL Server and Azure SQL Database
[ WITH <common_table_expression> [,...n] ]  
MERGE
    [ TOP ( expression ) [ PERCENT ] ]
    [ INTO ] <target_table> [ WITH ( <merge_hint> ) ] [ [ AS ] table_alias ]  
    USING <table_source> [ [ AS ] table_alias ]
    ON <merge_search_condition>  
    [ WHEN MATCHED [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]  
    [ WHEN NOT MATCHED [ BY TARGET ] [ AND <clause_search_condition> ]  
        THEN <merge_not_matched> ]  
    [ WHEN NOT MATCHED BY SOURCE [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]  
    [ <output_clause> ]  
    [ OPTION ( <query_hint> [ ,...n ] ) ]
;  
  
<target_table> ::=  
{
    [ database_name . schema_name . | schema_name . ]  
  target_table  
}  
  
<merge_hint>::=  
{  
    { [ <table_hint_limited> [ ,...n ] ]  
    [ [ , ] INDEX ( index_val [ ,...n ] ) ] }  
}  

<merge_search_condition> ::=  
    <search_condition>  
  
<merge_matched>::=  
    { UPDATE SET <set_clause> | DELETE }  
  
<merge_not_matched>::=  
{  
    INSERT [ ( column_list ) ]
        { VALUES ( values_list )  
        | DEFAULT VALUES }  
}  
  
<clause_search_condition> ::=  
    <search_condition> 
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

```syntaxsql
-- MERGE (Preview) for Azure Synapse Analytics 
[ WITH <common_table_expression> [,...n] ]  
MERGE
    [ INTO ] <target_table> [ [ AS ] table_alias ]  
    USING <table_source> [ [ AS ] table_alias ]
    ON <merge_search_condition>  
    [ WHEN MATCHED [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]  
    [ WHEN NOT MATCHED [ BY TARGET ] [ AND <clause_search_condition> ]  
        THEN <merge_not_matched> ]  
    [ WHEN NOT MATCHED BY SOURCE [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]
    [ OPTION ( <query_hint> [ ,...n ] ) ]
;  -- The semi-colon is required, or the query will return a syntax error. 
```

## <a name="arguments"></a>参数

WITH \<common_table_expression>  
指定在 MERGE 语句作用域内定义的临时命名结果集或视图，亦称为“公用表表达式”。 结果集派生自简单查询，并由 MERGE 语句引用。 有关详细信息，请参阅 [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md)。  
  
TOP ( *expression* ) [ PERCENT ]  
指定受影响的行数或所占百分比。 *expression* 可以是行数或行百分比。 在 TOP 表达式中引用的行不是以任意顺序排列的。 有关详细信息，请参阅 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md)。  
  
在整个源表和目标表联接，且不符合插入、更新或删除操作条件的联接行遭删除后，应用 TOP 子句。 TOP 子句进一步将联接行数减少到指定值。 插入、更新或删除操作以无序方式应用于其余联接行。 也就是说，在 WHEN 子句中定义的操作中，这些行是无序分布的。 例如，指定 TOP (10) 会影响 10 行。 在这些行中，可能会更新 7 行并插入 3 行，也可能会删除 1 行、更新 5 行并插入 4 行等。  
  
由于 MERGE 语句对源表和目标表都进行完全表扫描，因此在使用 TOP 子句通过创建多个批处理来修改大型表时，I/O 性能有时会受到影响。 在这种情况下，请务必要确保所有连续批处理都以新行为目标。  
  
*database_name*  
target_table 所在数据库的名称。  
  
*schema_name*  
target_table 所属架构的名称。  
  
*target_table*  
\<table_source> 中的数据行根据 \<clause_search_condition> 进行匹配的表或视图。 *target_table* 是由 MERGE 语句的 WHEN 子句指定的任何插入、更新或删除操作的目标。  
  
如果 *target_table* 为视图，则针对它的任何操作都必须满足更新视图所需的条件。 有关详细信息，请参阅[通过视图修改数据](../../relational-databases/views/modify-data-through-a-view.md)。  
  
target_table 不得是远程表。 target_table 不得有任何针对它定义的规则。  
  
[ AS ] *table_alias*  
用于为 target_table 引用表的替代名称。  
  
USING \<table_source>  
指定根据 \<merge_search condition> 与 target_table 中的数据行进行匹配的数据源。 此匹配的结果指出了要由 MERGE 语句的 WHEN 子句采取的操作。 \<table_source> 可以是一个远程表，或者是一个能够访问远程表的派生表。
  
\<table_source> 可以是一个派生表，它使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] [表值构造函数](../../t-sql/queries/table-value-constructor-transact-sql.md)通过指定多行来构造表。  
  
 [ AS ] *table_alias*  
用于为 table_source 引用表的替代名称。   
  
有关此子句的语法和参数的详细信息，请参阅 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)。  
  
ON \<merge_search_condition>  
指定联接 \<table_source> 与 target_table 以确定匹配位置所要满足的条件。
  
> [!CAUTION]  
> 请务必仅指定目标表中用于匹配目的的列。 也就是说，指定与源表中的对应列进行比较的目标表列。 请勿尝试通过在 ON 子句中筛选掉目标表中的行（如指定 `AND NOT target_table.column_x = value`）来提高查询性能。 这样做可能会返回意外和不正确的结果。  
  
WHEN MATCHED THEN \<merge_matched>  
指定根据 \<merge_matched> 子句更新或删除 *target_table 中所有与 \<table_source> ON \<merge_search_condition> 返回的行匹配、且满足其他所有搜索条件的行。  
  
MERGE 语句最多可以有两个 WHEN MATCHED 子句。 如果指定了两个子句，第一个子句必须随附 AND \<search_condition> 子句。 对于任何给定行，只有在未应用第一个 WHEN MATCHED 子句时，才会应用第二个 WHEN MATCHED 子句。 如果有两个 WHEN MATCHED 子句，一个必须指定 UPDATE 操作，另一个必须指定 DELETE 操作。 如果 \<merge_matched> 子句中指定的是 UPDATE，且根据 \<merge_search_condition> \<table_source> 中有多行与 target_table 中的一行匹配，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 便会返回错误。 MERGE 语句无法多次更新同一行，也无法更新和删除同一行。  
  
WHEN NOT MATCHED [ BY TARGET ] THEN \<merge_not_matched>  
指定针对 \<table_source> ON \<merge_search_condition> 返回且不与 target_table 中的行匹配、但满足其他搜索条件（若有）的所有行，将一行插入 target_table 中。 要插入的值是由 \<merge_not_matched> 子句指定的。 MERGE 语句只能有一个 WHEN NOT MATCHED [ BY TARGET ] 子句。

WHEN NOT MATCHED BY SOURCE THEN \<merge_matched>  
指定根据 \<merge_matched> 子句更新或删除 *target_table 中所有与 \<table_source> ON \<merge_search_condition> 返回的行不匹配而满足其他所有搜索条件的行。  
  
MERGE 语句最多可以有两个 WHEN NOT MATCHED BY SOURCE 子句。 如果指定了两个子句，第一个子句必须随附 AND \<clause_search_condition> 子句。 对于任何给定行，只有在未应用第一个 WHEN NOT MATCHED BY SOURCE 子句时，才会应用第二个 WHEN NOT MATCHED BY SOURC 子句。 如果有两个 WHEN NOT MATCHED BY SOURCE 子句，那么其中的一个必须指定 UPDATE 操作，而另一个必须指定 DELETE 操作。 在 \<clause_search_condition> 中只能引用目标表中的列。  
  
如果 \<table_source> 未返回任何行，无法访问源表中的列。 如果 \<merge_matched> 子句中指定的更新或删除操作引用了源表中的列，则会返回错误 207（列名无效）。 例如，由于无法访问源表中的 `WHEN NOT MATCHED BY SOURCE THEN UPDATE SET TargetTable.Col1 = SourceTable.Col1`，因此 `Col1` 子句可能导致该语句失败。  
  
AND \<clause_search_condition>  
指定任何有效的搜索条件。 有关详细信息，请参阅[搜索条件 (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md)。  
  
\<table_hint_limited>  
指定针对 MERGE 语句完成的每个插入、更新或删除操作，对目标表应用的一个或多个表提示。 需要有 WITH 关键字和括号。  
  
禁止使用 NOLOCK 和 READUNCOMMITTED。 有关表提示的详细信息，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  
  
对作为 INSERT 语句目标的表指定 TABLOCK 提示，与指定 TABLOCKX 提示的效果相同。 对表采用排他锁。 如果指定了 FORCESEEK，它会应用于与源表联接的目标表的隐式实例。  
  
> [!CAUTION]  
> 指定带有 WHEN NOT MATCHED [ BY TARGET ] THEN INSERT 的 READPAST 可能会导致违反 UNIQUE 约束的 INSERT 操作。  
  
INDEX ( index_val [，...n ] )  
指定目标表上一个或多个索引的名称或 ID，以执行与源表的隐式联接。 有关详细信息，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  
  
\<output_clause>  
针对 *target_table* 中不按照任何特定顺序更新、插入或删除的所有行返回一行。 **$action** 可在 output 子句中指定。 $action 是类型为 nvarchar(10) 的列，它返回每一行中 3 个值中的一个 ：“INSERT”、“UPDATE”或“DELETE”（具体视对相应行完成的操作而定）。 有关该子句的参数和行为的详细信息，请参阅 [OUTPUT 子句 (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md)。  
  
OPTION ( \<query_hint> [ ,...n ] )  
指定使用优化器提示来自定义数据库引擎处理语句的方式。 有关详细信息，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。  
  
\<merge_matched>  
指定更新或删除操作，应用于 target_table 中所有不与 \<table_source> ON \<merge_search_condition> 返回的行匹配、但满足其他所有搜索条件的行。  
  
UPDATE SET \<set_clause>  
指定目标表中要更新的列或变量名称的列表，以及用来更新它们的值。  
  
有关该子句的参数的详细信息，请参阅 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)。 不支持将变量设置为与列相同的值。  
  
DELETE  
指定删除与 *target_table* 中的行匹配的行。  
  
\<merge_not_matched>  
指定要插入到目标表中的值。  
  
(*column_list*)  
要在其中插入数据的目标表中一个或多个列的列表。 必须使用单一部分名称格式来指定这些列，否则 MERGE 语句将失败。 必须用括号将 column_list 括起来，并且用逗号进行分隔。  
  
VALUES ( *values_list*)  
返回要插入到目标表中的值的常量、变量或表达式的逗号分隔列表。 表达式不得包含 EXECUTE 语句。  
  
DEFAULT VALUES  
强制插入的行包含为每个列定义的默认值。  
  
有关此子句的详细信息，请参阅 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)。  
  
\<search_condition>  
指定用于指定 \<merge_search_condition> 或 \<clause_search_condition> 的搜索条件。 有关此子句的参数的详细信息，请参阅[搜索条件 (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md)。  

\<graph search pattern>  
指定图匹配模式。 有关此子句的参数的详细信息，请参阅 [MATCH &#40;Transact-SQL&#41;](../../t-sql/queries/match-sql-graph.md)
  
## <a name="remarks"></a>备注
>[!NOTE]
> 在 Azure Synapse Analytics 中，MERGE 命令（预览）与 SQL Server 和 Azure SQL 数据库相比具有以下差异。  
> - MERGE 更新操作实现为删除和插入对。 MERGE 更新的受影响的行计数包括删除的行和插入的行。 

> - 在预览期间，包含 IDENTITY 列的表不支持 MERGE…WHEN NOT MATCHED INSERT。  

> - 此表描述了对具有不同分发类型的表的支持：

>|Azure Synapse Analytics 中的 MERGE CLAUSE|支持的 TARGE 分发表| 支持的 SOURCE 分发表|评论|  
>|-----------------|---------------|-----------------|-----------|  
>|**WHEN MATCHED**| 所有分发类型 |所有分发类型||  
>|**NOT MATCHED BY TARGET**|HASH |所有分发类型|使用 UPDATE/DELETE FROM…JOIN 来同步两个表。 |
>|**NOT MATCHED BY SOURCE**|所有分发类型|所有分发类型|||  

必须指定三个 MATCHED 子句中的至少一个子句，但可以按任何顺序指定。 无法在同一个 MATCHED 子句中多次更新一个变量。  
  
MERGE 语句对目标表指定的任何插入、更新或删除操作受限于，在此语句中定义的任何约束，包括任何级联引用完整性约束。 如果 IGNORE_DUP_KEY 对目标表中的任何唯一索引都设置为 ON，MERGE 便会忽略此设置。  
  
MERGE 语句需要一个分号 (;) 作为语句终止符。 如果运行没有终止符的 MERGE 语句，将引发错误 10713。  
  
如果在 MERGE 之后使用，[@@ROWCOUNT (Transact-SQL)](../../t-sql/functions/rowcount-transact-sql.md) 会返回为客户端插入、更新和删除的行的总数。  
  
在数据库兼容级别设置为 100 或更高时，MERGE 为完全保留的关键字。 MERGE 语句可用于设置为 90 和 100 的数据库兼容性级别；不过，当数据库兼容性级别设置为 90 时，关键字不是完全保留。  
  
使用已排入队列的更新复制时，请勿使用 MERGE 语句。 MERGE 和已排入队列的更新触发器不兼容。 使用 insert 或 update 语句替换 **MERGE** 语句。  

## <a name="trigger-implementation"></a>触发器的实现

对于在 MERGE 语句中指定的每个插入、更新或删除操作，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 都会触发对目标表定义的任何对应 AFTER 触发器，但不保证哪个操作最先或最后触发触发器。 为相同操作定义的触发器会遵循您指定的顺序进行触发。 有关设置触发器激发顺序的详细信息，请参阅[指定第一个和最后一个触发器](../../relational-databases/triggers/specify-first-and-last-triggers.md)。  
  
如果目标表已针对 MERGE 语句完成的插入、更新或删除操作启用了对自己定义的 INSTEAD OF 触发器，它必须已针对 MERGE 语句中指定的所有操作启用了 INSTEAD OF 触发器。  
  
如果对 target_table 定义了任何 INSTEAD OF UPDATE 或 INSTEAD OF DELETE 触发器，则不会运行更新或删除操作。 而是会触发触发器，并相应地填充 inserted 和 deleted 表。  
  
如果对 target_table 定义了任何 INSTEAD OF INSERT 触发器，则不会执行插入操作。 而是相应地填充表。  
  
## <a name="permissions"></a>权限

需要对源表的 SELECT 权限和对目标表的 INSERT、UPDATE 或 DELETE 权限。 有关详细信息，请参阅 [SELECT](../../t-sql/queries/select-transact-sql.md)、[INSERT](../../t-sql/statements/insert-transact-sql.md)、[UPDATE](../../t-sql/queries/update-transact-sql.md) 和 [DELETE](../../t-sql/statements/delete-transact-sql.md) 文章中的“权限”部分。  
  
## <a name="optimizing-merge-statement-performance"></a>优化 MERGE 语句性能

通过使用 MERGE 语句，可以使用单个语句替换各个 DML 语句。 由于操作是在单个语句中执行的，因此可以提高查询性能，从而最大限度地减少处理源表和目标表中数据的次数。 然而，性能的提升取决于是否进行了正确的索引和联接以及是否遵守了其他注意事项。

### <a name="index-best-practices"></a>有关索引的最佳做法

若要提高 MERGE 语句的性能，我们建议您遵循以下索引准则：

- 对源表的联接列创建唯一的涵盖索引。
- 对目标表的联接列创建唯一的聚集索引。

这些索引确保联接键唯一并且表中的数据经过排序。 因为查询优化器不需要执行额外验证处理即可定位和更新重复的行，也不需要执行其他排序操作，所以查询性能得到了提高。

### <a name="join-best-practices"></a>有关 JOIN 的最佳做法

若要提高 MERGE 语句的性能并确保获得正确的结果，我们建议您遵循以下联接准则：

- 只在 ON <merge_search_condition> 子句中指定搜索条件，确定用于匹配源表与目标表中数据的条件。 也就是说，仅指定与源表中的对应列进行比较的目标表列。 
- 不要包括与其他值（如常量）的比较。

若要从源表或目标表中筛选出行，请使用以下方法之一。

- 在适当的 WHEN 子句中指定用于行筛选的搜索条件。 例如，WHEN NOT MATCHED AND S.EmployeeName LIKE 'S%' THEN INSERT....
- 对返回筛选行的源表或目标表定义视图，并且将该视图作为源表或目标表进行引用。 如果该视图是针对目标表定义的，则针对该视图的任何操作都必须满足更新视图所需的条件。 若要详细了解如何使用视图来更新数据，请参阅“通过视图修改数据”。
- 使用 `WITH <common table expression>` 子句筛选掉源表或目标表中的行。 此方法类似于在 ON 子句中指定附加搜索条件，并可能产生不正确的结果。 建议您避免使用此方法，或者在采用它前进行全面测试。

MERGE 语句中联接操作的优化方式与 SELECT 语句中联接操作的优化方式相同。 也就是说，当 SQL Server 处理联接时，查询优化器从多种可行方法中选择最高效的方法来处理联接。 如果源表和目标表的大小相似，且前面介绍的索引准则已应用于源表和目标表，那么合并联接运算符是最高效的查询计划。 这是由于对两个表都只扫描一次，并且无需对数据进行排序。 如果源表小于目标表，最好使用嵌套循环运算符。

通过在 MERGE 语句中指定 `OPTION (<query_hint>)` 子句，可以强制使用某种特定联接。 建议您不要将哈希联接用作 MERGE 语句的查询提示，因为该联接类型不使用索引。

### <a name="parameterization-best-practices"></a>有关参数化的最佳做法

如果在没有参数的情况下执行 SELECT、INSERT、UPDATE 或 DELETE 语句，SQL Server 查询优化器可能会选择在内部参数化语句。 也就是说，使用参数替换查询中包含的任何文字值。 例如，可以在内部将语句 INSERT dbo.MyTable (Col1, Col2) VALUES (1, 10) 实现为 INSERT dbo.MyTable (Col1, Col2) VALUES (@p1, @p2)。 此过程称为“简单参数化”，它增强了关系引擎将新 SQL 语句与先前编译的现有执行计划进行匹配的能力。 由于减少了查询编译和重新编译的频率，因此可提高查询性能。 查询优化器不会对 MERGE 语句应用简单参数化过程。 由于在每次执行 MERGE 语句时都需要编译一个新计划，因此包含文字值的 MERGE 语句的性能表现可能低于各个 INSERT、UPDATE 或 DELETE 语句。

若要提高查询性能，我们建议您遵循以下参数化准则：

- 参数化 MERGE 语句的 `ON <merge_search_condition>` 子句和 `WHEN` 子句中的所有文本值。 例如，可以将 MERGE 语句合并到存储过程中，并用适当的输入参数替换文字值。
- 如果无法参数化语句，请创建 `TEMPLATE` 类型的计划指南，并在计划指南中指定 `PARAMETERIZATION FORCED` 查询提示。
- 如果频繁对数据库执行 MERGE 语句，请考虑将数据库的 PARAMETERIZATION 选项设置为 FORCED。 设置此选项时请谨慎从事。 `PARAMETERIZATION` 选项是数据库级别设置，它影响如何对数据库处理所有查询。

### <a name="top-clause-best-practices"></a>有关 TOP 子句的最佳做法

在 MERGE 语句中，TOP 子句指定在对源表和目标表进行联接之后（或在删除不符合执行插入、更新或删除操作条件的行之后）受影响的行的数量或百分比。 TOP 子句将联接行的数量进一步减少为指定值，并且以一种无序方式对其余联接行应用插入、更新或删除操作。 也就是说，在 WHEN 子句中定义的操作中，这些行是无序分布的。 例如，如果指定 TOP (10)，将会影响 10 行；在这些行中，可能会更新 7 行而插入 3 行，或者可能删除 1 行、更新 5 行并且插入 4 行，等等。

使用 TOP 子句对大型表分批执行数据操作语言 (DML) 操作是一种常见的做法。 如果出于此目的而在 MERGE 语句中使用 TOP 子句，请务必了解以下影响。

- I/O 性能可能会受到影响。

  MERGE 语句对源表和目标表都进行完全表扫描。 使操作分批执行可减少每批执行的写入操作的数量；但在每个批处理中都将对源表和目标表执行完全表扫描。 产生的读取活动可能会影响查询的性能。

- 可能产生不正确的结果。

  请务必确保所有连续批处理都以新行为目标，否则可能会发生意外行为，如在目标表中错误地插入重复行。 如果源表包含的某行未包括在目标批处理中，但却包含在总目标表中，便会发生此情况。

- 确保获得正确的结果：

  - 使用 ON 子句确定哪些源行影响现有目标行以及哪些是全新的。
  - 在 WHEN MATCHED 子句中使用附加条件来确定目标行是否已由先前的批处理进行了更新。

因为只有在应用这些子句之后才会应用 TOP 子句，所以每次执行会插入一个确实不匹配的行，或者更新一个现有行。

### <a name="bulk-load-best-practices"></a>有关大容量加载的最佳做法

MERGE 语句可以将 `OPENROWSET(BULK…)` 子句指定为表源，高效地将源数据文件中的数据大容量加载到目标表中。 通过这种方式，可以在单个批中处理整个文件。

若要改进大容量合并过程的性能，我们建议您遵循以下准则：

- 对目标表中的联接列创建聚集索引。
- 在 `OPENROWSET(BULK…)` 子句中使用 ORDER 和 UNIQUE 提示，以指定如何对源数据文件进行排序。

  默认情况下，大容量操作假定数据文件未排序。 因此，请务必根据目标表的聚集索引对源数据进行排序并使用 ORDER 提示指示该顺序，以便查询优化器可以生成更有效的查询计划。 在运行时将对提示进行验证；如果数据流不符合指定的提示，则会引发错误。

上述准则确保联接键唯一并且源文件中数据的排序顺序与目标表相符。 因为不需要执行其他排序操作和不必要的数据复制，所以提高了查询的性能。

### <a name="measuring-and-diagnosing-merge-performance"></a>测量和诊断 MERGE 性能

以下功能可帮助您测量和诊断 MERGE 语句的性能。

- 在 [sys.dm_exec_query_optimizer_info](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-optimizer-info-transact-sql.md) 动态管理视图中使用 merge stmt 计数器，以返回用于 MERGE 语句的查询优化数。
- 在 [sys.dm_exec_plan_attributes](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md) 动态管理视图中使用 merge_action_type 属性，以返回用作 MERGE 语句结果的触发器执行计划的类型。
- 使用 SQL 跟踪收集 MERGE 语句的故障排除数据，其方式与用于其他数据操作语言 (DML) 语句的方式相同。 有关详细信息，请参阅 [SQL Trace](../../relational-databases/sql-trace/sql-trace.md)。

## <a name="examples"></a>示例  

### <a name="a-using-merge-to-do-insert-and-update-operations-on-a-table-in-a-single-statement"></a>A. 使用 MERGE 在一个语句中对表执行 INSERT 和 UPDATE 操作

常见方案是，更新表中的一个或多个列（若有匹配行）。 或者，在没有匹配行的情况下，将数据作为新行插入。 处理两种方案之一的一般方法是，将参数传递给包含相应 UPDATE 和 INSERT 语句的存储过程。 借助 MERGE 语句，可以在一个语句中同时执行这两项任务。 下面的示例显示了 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中一个同时包含 INSERT 语句和 UPDATE 语句的存储过程。 随后，过程被修改为，使用一个 MERGE 语句运行等效的操作。  
  
```sql  
CREATE PROCEDURE dbo.InsertUnitMeasure  
    @UnitMeasureCode nchar(3),  
    @Name nvarchar(25)  
AS
BEGIN  
    SET NOCOUNT ON;  
-- Update the row if it exists.
    UPDATE Production.UnitMeasure  
SET Name = @Name  
WHERE UnitMeasureCode = @UnitMeasureCode  
-- Insert the row if the UPDATE statement failed.  
IF (@@ROWCOUNT = 0 )  
BEGIN  
    INSERT INTO Production.UnitMeasure (UnitMeasureCode, Name)  
    VALUES (@UnitMeasureCode, @Name)  
END  
END;  
GO  
-- Test the procedure and return the results.  
EXEC InsertUnitMeasure @UnitMeasureCode = 'ABC', @Name = 'Test Value';  
SELECT UnitMeasureCode, Name FROM Production.UnitMeasure  
WHERE UnitMeasureCode = 'ABC';  
GO  
  
-- Rewrite the procedure to perform the same operations using the
-- MERGE statement.  
-- Create a temporary table to hold the updated or inserted values
-- from the OUTPUT clause.  
CREATE TABLE #MyTempTable  
    (ExistingCode nchar(3),  
     ExistingName nvarchar(50),  
     ExistingDate datetime,  
     ActionTaken nvarchar(10),  
     NewCode nchar(3),  
     NewName nvarchar(50),  
     NewDate datetime  
    );  
GO  
ALTER PROCEDURE dbo.InsertUnitMeasure  
    @UnitMeasureCode nchar(3),  
    @Name nvarchar(25)  
AS
BEGIN  
    SET NOCOUNT ON;  
  
    MERGE Production.UnitMeasure AS target  
    USING (SELECT @UnitMeasureCode, @Name) AS source (UnitMeasureCode, Name)  
    ON (target.UnitMeasureCode = source.UnitMeasureCode)  
    WHEN MATCHED THEN
        UPDATE SET Name = source.Name  
    WHEN NOT MATCHED THEN  
        INSERT (UnitMeasureCode, Name)  
        VALUES (source.UnitMeasureCode, source.Name)  
    OUTPUT deleted.*, $action, inserted.* INTO #MyTempTable;  
END;  
GO  
-- Test the procedure and return the results.  
EXEC InsertUnitMeasure @UnitMeasureCode = 'ABC', @Name = 'New Test Value';  
EXEC InsertUnitMeasure @UnitMeasureCode = 'XYZ', @Name = 'Test Value';  
EXEC InsertUnitMeasure @UnitMeasureCode = 'ABC', @Name = 'Another Test Value';  
  
SELECT * FROM #MyTempTable;  
-- Cleanup
DELETE FROM Production.UnitMeasure WHERE UnitMeasureCode IN ('ABC','XYZ');  
DROP TABLE #MyTempTable;  
GO  
```  
  
### <a name="b-using-merge-to-do-update-and-delete-operations-on-a-table-in-a-single-statement"></a>B. 使用 MERGE 在一个语句中对表执行 UPDATE 和 DELETE 操作

下面的示例使用 MERGE 根据 `SalesOrderDetail` 表中已处理的订单，每天更新一次 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 示例数据库中的 `ProductInventory` 表。 通过减去每天对 `Quantity` 表中的每种产品所下的订单数，更新 `ProductInventory` 表的 `SalesOrderDetail` 列。 如果某种产品的订单数导致该产品的库存量下降到 0 或更少，则从 `ProductInventory` 表中删除该产品对应的行。  
  
```sql  
CREATE PROCEDURE Production.usp_UpdateInventory  
    @OrderDate datetime  
AS  
MERGE Production.ProductInventory AS target  
USING (SELECT ProductID, SUM(OrderQty) FROM Sales.SalesOrderDetail AS sod  
    JOIN Sales.SalesOrderHeader AS soh  
    ON sod.SalesOrderID = soh.SalesOrderID  
    AND soh.OrderDate = @OrderDate  
    GROUP BY ProductID) AS source (ProductID, OrderQty)  
ON (target.ProductID = source.ProductID)  
WHEN MATCHED AND target.Quantity - source.OrderQty <= 0  
    THEN DELETE  
WHEN MATCHED
    THEN UPDATE SET target.Quantity = target.Quantity - source.OrderQty,
                    target.ModifiedDate = GETDATE()  
OUTPUT $action, Inserted.ProductID, Inserted.Quantity,
    Inserted.ModifiedDate, Deleted.ProductID,  
    Deleted.Quantity, Deleted.ModifiedDate;  
GO  
  
EXECUTE Production.usp_UpdateInventory '20030501'  
```  
  
### <a name="c-using-merge-to-do-update-and-insert-operations-on-a-target-table-by-using-a-derived-source-table"></a>C. 借助派生的源表，使用 MERGE 对目标表执行 UPDATE 和 INSERT 操作

下面的示例使用 MERGE 以更新或插入行的方式来修改 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 `SalesReason` 表。 当源表中的 `NewName` 值与目标表 (`Name`) 的 `SalesReason` 列中的值匹配时，就会更新此目标表中的 `ReasonType` 列。 如果 `NewName` 的值不匹配，就会将源行插入目标表中。 此源表是一个派生表，它使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表值构造函数指定源表的多个行。 有关在派生表中使用表值构造函数的详细信息，请参阅[表值构造函数 (Transact-SQL)](../../t-sql/queries/table-value-constructor-transact-sql.md)。 下面的示例还展示了如何在表变量中存储 OUTPUT 子句的结果。 然后，通过运行返回已插入行数和已更新行数的简单选择操作，汇总 MERGE 语句的结果。  
  
```sql  
-- Create a temporary table variable to hold the output actions.  
DECLARE @SummaryOfChanges TABLE(Change VARCHAR(20));  
  
MERGE INTO Sales.SalesReason AS Target  
USING (VALUES ('Recommendation','Other'), ('Review', 'Marketing'),
              ('Internet', 'Promotion'))  
       AS Source (NewName, NewReasonType)  
ON Target.Name = Source.NewName  
WHEN MATCHED THEN  
UPDATE SET ReasonType = Source.NewReasonType  
WHEN NOT MATCHED BY TARGET THEN  
INSERT (Name, ReasonType) VALUES (NewName, NewReasonType)  
OUTPUT $action INTO @SummaryOfChanges;  
  
-- Query the results of the table variable.  
SELECT Change, COUNT(*) AS CountPerChange  
FROM @SummaryOfChanges  
GROUP BY Change;  
```  
  
### <a name="d-inserting-the-results-of-the-merge-statement-into-another-table"></a>D. 将 MERGE 语句的执行结果插入到另一个表中

下面的示例捕获从 MERGE 语句的 OUTPUT 子句返回的数据，并将该数据插入到另一个表中。 MERGE 语句根据在 `Quantity` 表中处理的订单更新 `ProductInventory` 数据库中 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 表的 `SalesOrderDetail` 列。 下面的示例捕获已更新行，并将它们插入用于跟踪库存变化的另一个表中。  
  
```sql  
CREATE TABLE Production.UpdatedInventory  
    (ProductID INT NOT NULL, LocationID int, NewQty int, PreviousQty int,  
     CONSTRAINT PK_Inventory PRIMARY KEY CLUSTERED (ProductID, LocationID));  
GO  
INSERT INTO Production.UpdatedInventory  
SELECT ProductID, LocationID, NewQty, PreviousQty
FROM  
(    MERGE Production.ProductInventory AS pi  
     USING (SELECT ProductID, SUM(OrderQty)
            FROM Sales.SalesOrderDetail AS sod  
            JOIN Sales.SalesOrderHeader AS soh  
            ON sod.SalesOrderID = soh.SalesOrderID  
            AND soh.OrderDate BETWEEN '20030701' AND '20030731'  
            GROUP BY ProductID) AS src (ProductID, OrderQty)  
     ON pi.ProductID = src.ProductID  
    WHEN MATCHED AND pi.Quantity - src.OrderQty >= 0
        THEN UPDATE SET pi.Quantity = pi.Quantity - src.OrderQty  
    WHEN MATCHED AND pi.Quantity - src.OrderQty <= 0
        THEN DELETE  
    OUTPUT $action, Inserted.ProductID, Inserted.LocationID,
        Inserted.Quantity AS NewQty, Deleted.Quantity AS PreviousQty)  
 AS Changes (Action, ProductID, LocationID, NewQty, PreviousQty)
 WHERE Action = 'UPDATE';  
GO  
```  

### <a name="e-using-merge-to-do-insert-or-update-on-a-target-edge-table-in-a-graph-database"></a>E. 使用 MERGE 对图形数据库中的目标边缘表执行 INSERT 或 UPDATE 操作

在此示例中，创建节点表 `Person` 和 `City` 以及边缘表 `livesIn`。 如果 `Person` 和 `City` 之间尚不存在 `livesIn` 边缘，则对边缘使用 MERGE 语句，并插入新行。 如果已有边缘，只需更新 `livesIn` 边缘上的 StreetAddress 属性。

```sql
-- CREATE node and edge tables
CREATE TABLE Person
    (
        ID INTEGER PRIMARY KEY,
        PersonName VARCHAR(100)
    )
AS NODE
GO

CREATE TABLE City
    (
        ID INTEGER PRIMARY KEY,
        CityName VARCHAR(100),
        StateName VARCHAR(100)
    )
AS NODE
GO

CREATE TABLE livesIn
    (
        StreetAddress VARCHAR(100)
    )
AS EDGE
GO

-- INSERT some test data into node and edge tables
INSERT INTO Person VALUES (1, 'Ron'), (2, 'David'), (3, 'Nancy')
GO

INSERT INTO City VALUES (1, 'Redmond', 'Washington'), (2, 'Seattle', 'Washington')
GO

INSERT livesIn SELECT P.$node_id, C.$node_id, c
FROM Person P, City C, (values (1,1, '123 Avenue'), (2,2,'Main Street')) v(a,b,c)
WHERE P.id = a AND C.id = b
GO

-- Use MERGE to update/insert edge data
CREATE OR ALTER PROCEDURE mergeEdge
    @PersonId integer,
    @CityId integer,
    @StreetAddress varchar(100)
AS
BEGIN
    MERGE livesIn
        USING ((SELECT @PersonId, @CityId, @StreetAddress) AS T (PersonId, CityId, StreetAddress)
                JOIN Person ON T.PersonId = Person.ID
                JOIN City ON T.CityId = City.ID)
        ON MATCH (Person-(livesIn)->City)
    WHEN MATCHED THEN
        UPDATE SET StreetAddress = @StreetAddress
    WHEN NOT MATCHED THEN
        INSERT ($from_id, $to_id, StreetAddress)
        VALUES (Person.$node_id, City.$node_id, @StreetAddress) ;
END
GO

-- Following will insert a new edge in the livesIn edge table
EXEC mergeEdge 3, 2, '4444th Avenue'
GO

-- Following will update the StreetAddress on the edge that connects Ron to Redmond
EXEC mergeEdge 1, 1, '321 Avenue'
GO

-- Verify that all the address were added/updated correctly
SELECT PersonName, CityName, StreetAddress
FROM Person , City , livesIn
WHERE MATCH(Person-(livesIn)->city)
GO
```
  
## <a name="see-also"></a>另请参阅

- [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)
- [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)
- [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)
- [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)
- [OUTPUT 子句 (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md)
- [在 Integration Services 包中执行 MERGE](../../integration-services/control-flow/merge-in-integration-services-packages.md)
- [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)
- [表值构造函数 (Transact-SQL)](../../t-sql/queries/table-value-constructor-transact-sql.md)  
