---
description: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)
title: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2020
ms.prod: sql
ms.prod_service: synapse-analytics
ms.reviewer: jrasnick
ms.technology: data-warehouse
ms.topic: reference
f1_keywords:
- CREATE VIEW
- VIEW_TSQL
- VIEW
- CREATE_VIEW_TSQL
- SCHEMABINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table creation [SQL Server], CREATE VIEW
- views [SQL Server], creating
- CREATE VIEW statement
- updatable partitioned views
- tables [SQL Server], virtual
- updatable views
- modifying partitioned views
- virtual tables [SQL Server]
- number of columns per view
- partitioned views [SQL Server], creating
- WITH ENCRYPTION clause
- WITH CHECK OPTION clause
- partitioned views [SQL Server], modifying
- partitioned views [SQL Server], replication
- distributed partitioned views [SQL Server]
- views [SQL Server], indexed views
- maximum number of columns per view
ms.assetid: aecc2f73-2ab5-4db9-b1e6-2f9e3c601fb9
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: a484f4d57281ad869df4f86a4d6cebfb7602c8e8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104734175"
---
# <a name="create-materialized-view-as-select-transact-sql"></a>CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)  

[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

本文说明如何在 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中使用 CREATE MATERIALIZED VIEW AS SELECT T-SQL 语句开发解决方案。 本文还会提供代码示例。

具体化视图会保留从视图定义查询返回的数据，并在基础表中的数据更改时自动更新。   它提高了复杂查询（通常是使用联接和聚合的查询）的性能，同时提供了简单的维护操作。   由于具体化视图具有执行计划自动匹配功能，因此无需在查询中引用它，优化器即会考虑将此视图作为替换项。  通过使用这一功能，数据工程师可以将具体化视图作为改进查询响应时间的机制来实现，而不必再更改查询。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE MATERIALIZED VIEW [ schema_name. ] materialized_view_name
    WITH (  
      <distribution_option>
    )
    AS <select_statement>
[;]

<distribution_option> ::=
    {  
        DISTRIBUTION = HASH ( distribution_column_name )  
      | DISTRIBUTION = ROUND_ROBIN  
    }

<select_statement> ::=
    SELECT select_criteria

```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>参数

schema_name     
 视图所属架构的名称。  
  
materialized_view_name   
视图名称。 视图名称必须符合有关标识符的规则。 可以选择是否指定视图所有者名称。  

分布选项     
仅支持 HASH 和 ROUND_ROBIN 分步。

select_statement   
具体化视图定义中的 SELECT 列表需要至少满足以下两个条件之一：
- SELECT 列表包含聚合函数。
- 具体化视图定义使用了 GROUP BY，并且 SELECT 列表包括 GROUP BY 中的所有列。  在 GROUP BY 子句中最多可以使用 32 列。

具体化视图定义的 SELECT 列表必须包含聚合函数。  支持的聚合包括 MAX、MIN、AVG、COUNT、COUNT_BIG、SUM、VAR、STDEV。

具体化视图定义的 SELECT 列表使用 MIN/MAX 聚合时，以下要求适用：
 
- 必须使用 FOR_APPEND。  例如：
  ```sql 
  CREATE MATERIALIZED VIEW mv_test2  
  WITH (distribution = hash(i_category_id), FOR_APPEND)  
  AS
  SELECT MAX(i.i_rec_start_date) as max_i_rec_start_date, MIN(i.i_rec_end_date) as min_i_rec_end_date, i.i_item_sk, i.i_item_id, i.i_category_id
  FROM syntheticworkload.item i  
  GROUP BY i.i_item_sk, i.i_item_id, i.i_category_id
  ```

- 引用的基表出现 UPDATE 或 DELETE 时，将禁用具体化视图。  此限制不适用于 INSERT。  若要重新启用具体化视图，请运行 ALTER MATERIALIZED VIEW 和 REBUILD。
  
## <a name="remarks"></a>注解

Azure 数据仓库中的具体化视图与 SQL Server 中的索引视图相似。除了具体化视图支持聚合函数外，它与索引视图适用的限制几乎相同（请参阅[创建索引视图](../../relational-databases/views/create-indexed-views.md)了解详细信息）。   

>[!Note]
>尽管 CREATE MATERIALIZED VIEW 不支持 COUNT、DISTINCT、COUNT(DISTINCT expression) 或 COUNT_BIG (DISTINCT expression)，但具有这些函数的 SELECT 查询仍可以从具体化视图中获益，提升性能，因为 Synapse SQL 优化器可以在用户查询中自动重写这些聚合，以匹配现有的具体化视图。  有关详细信息，请查看本文的示例部分。 

CREATE MATERIALIZED VIEW AS SELECT 不支持 APPROX_COUNT_DISTINCT。

具体化视图仅支持 CLUSTERED COLUMNSTORE INDEX。 

具体化视图无法引用其他视图。  

也无法在具有动态数据掩码 (DDM) 的表上创建具体化视图，即使 DDM 列不属于该具体化视图也是如此。  如果表列是活动具体化视图或禁用的具体化视图的一部分，则 DDM 无法添加到此列。  

无法在启用了行级安全性的表上创建具体化视图。

可以在已分区表上创建具体化视图。  具体化视图基表支持分区 SPLIT/MERGE，不支持分区 SWITCH。  
 
具体化视图引用的表不支持 ALTER TABLE SWITCH。 请先禁用或删除具体化视图，然后再使用 ALTER TABLE SWITCH。 在以下应用场景中，需要向具体化视图添加新列，才能创建具体化视图：

|场景|要添加到具体化视图的新列|评论|  
|-----------------|---------------|-----------------|
|具体化视图定义的 SELECT 列表缺少 COUNT_BIG()| COUNT_BIG (*) |通过具体化视图创建自动添加。  不需要任何用户操作。|
|由用户在具体化视图定义的 SELECT 列表中指定 SUM(a)，其中“a”是可为空的表达式 |COUNT_BIG (a) |用户需要手动将表达式“a”添加到具体化视图定义中。|
|由用户在具体化视图定义的 SELECT 列表中指定 AVG(a)，其中“a”是表达式。|SUM(a), COUNT_BIG(a)|通过具体化视图创建自动添加。  不需要任何用户操作。|
|由用户在具体化视图定义的 SELECT 列表中指定 STDEV(a)，其中“a”是表达式。|SUM(a), COUNT_BIG(a), SUM(square(a))|通过具体化视图创建自动添加。  不需要任何用户操作。 |
| | | |

创建后，SQL Server Management Studio 中的 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 实例的视图文件夹将显示具体化实体。

用户可以运行 [SP_SPACEUSED](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) 和 [DBCC PDW_SHOWSPACEUSED](../database-console-commands/dbcc-pdw-showspaceused-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) 来确定具体化视图占用的空间。  

可以通过 DROP VIEW 删除具体化视图。  可以使用 ALTER MATERIALIZED VIEW 禁用或重新生成具体化视图。   

具体化视图是一种自动查询优化机制。  用户不需要直接查询具体化视图。  提交用户查询后，引擎将检查用户对查询对象的权限，如果用户没有访问查询中的表或普通视图的权限，引擎将使查询在未执行的情况下失败。  如果用户的权限已经过验证，优化器将自动使用匹配的具体化视图来执行查询，以提高性能。  无论通过查询基表还是查询具体化视图来执行查询，用户都将获得相同的返回数据。  

SQL Server Management Studio 中的 EXPLAIN 计划和图形估计执行计划可以显示查询优化器是否考虑将具体化视图用于查询执行。 SQL Server Management Studio 中的图形估计执行计划可以显示查询优化器是否考虑将具体化视图用于查询执行。

若要了解 SQL 语句是否可以从新的具体化视图受益，请运行 `EXPLAIN` 命令和 `WITH_RECOMMENDATIONS`。  有关详细信息，请参阅 [EXPLAIN (Transact-SQL)](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)。

## <a name="ownership"></a>所有权
- 如果基表和要创建的具体化视图的所有者不相同，则无法创建具体化视图。
- 具体化视图及其基表可以位于不同的架构中。 创建具体化视图后，视图的架构所有者将自动成为具体化视图的所有者，并且此视图所有权无法更改。     

## <a name="permissions"></a>权限
除了满足对象所有权要求外，用户还需要具有以下权限才能创建具体化视图： 
1) 数据库中的 CREATE VIEW 权限
2) 对具体化视图的基表的 SELECT 权限
3) 对包含基表的架构的 REFERENCES 权限
4) 对包含具体化视图的架构的 ALTER 权限 


## <a name="example"></a>示例
A. 此示例显示 Synapse SQL 优化器如何自动使用具体化视图来执行查询，以获得更好的性能，即使该查询使用的是 CREATE MATERIALIZED VIEW 不支持的函数（例如 COUNT(DISTINCT expression)）也是如此。 过去需要几秒钟才能完成的查询现在不到一秒就能完成，而且无需对用户查询进行任何更改。   

``` sql 

-- Create a table with ~536 million rows
create table t(a int not null, b int not null, c int not null) with (distribution=hash(a), clustered columnstore index);

insert into t values(1,1,1);

declare @p int =1;
while (@P < 30)
    begin
    insert into t select a+1,b+2,c+3 from t;  
    select @p +=1;
end

-- A SELECT query with COUNT_BIG (DISTINCT expression) took multiple seconds to complete and it reads data directly from the base table a. 
select a, count_big(distinct b) from t group by a;

-- Create two materialized views, not using COUNT_BIG(DISTINCT expression).
create materialized view V1 with(distribution=hash(a)) as select a, b from dbo.t group by a, b;

-- Clear all cache.

DBCC DROPCLEANBUFFERS;
DBCC freeproccache;

-- Check the estimated execution plan in SQL Server Management Studio.  It shows the SELECT query is first step (GET operator) is to read data from the materialized view V1, not from base table a.
select a, count_big(distinct b) from t group by a;

-- Now execute this SELECT query.  This time it took sub-second to complete because Synapse SQL engine automatically matches the query with materialized view V1 and uses it for faster query execution.  There was no change in the user query.

DECLARE @timerstart datetime2, @timerend datetime2;
SET @timerstart = sysdatetime();

select a, count_big(distinct b) from t group by a;

SET @timerend = sysdatetime()
select DATEDIFF(ms,@timerstart,@timerend);

```

B. 在此示例中，User2 在 User1 拥有的表上创建具体化视图。  具体化视图由 User1 拥有。
```sql
/****************************************************************
Setup:
SchemaX owner = DBO
SchemaX.T1 owner = User1
SchemaX.T2 owner = User1
SchemaY owner = User1
*****************************************************************/
CREATE USER User1 WITHOUT LOGIN ;
CREATE USER User2 WITHOUT LOGIN ;
CREATE SCHEMA SchemaX;  
CREATE SCHEMA SchemaY AUTHORIZATION User1;
GO
CREATE TABLE [SchemaX].[T1] (   [vendorID] [varchar](255) Not NULL, [totalAmount] [float] Not NULL, [puYear] [int] NULL );
CREATE TABLE [SchemaX].[T2] (   [vendorID] [varchar](255) Not NULL, [totalAmount] [float] Not NULL, [puYear] [int] NULL);
GO
ALTER AUTHORIZATION ON OBJECT::SchemaX.[T1] TO User1;
ALTER AUTHORIZATION ON OBJECT::SchemaX.[T2] TO User1;

/*****************************************************************************
For user2 to create a MV in SchemaY on SchemaX.T1 and SchemaX.T2, user2 needs:
1. CREATE VIEW permission in the database
2. REFERENCES permission on the schema1
3. SELECT permission on base table T1, T2  
4. ALTER permission on SchemaY
******************************************************************************/
GRANT CREATE VIEW to User2;
GRANT REFERENCES ON SCHEMA::SchemaX to User2;  
GRANT SELECT ON OBJECT::SchemaX.T1 to User2; 
GRANT SELECT ON OBJECT::SchemaX.T2 to User2;
GRANT ALTER ON SCHEMA::SchemaY to User2; 
GO
EXECUTE AS USER = 'User2';  
GO
CREATE materialized VIEW [SchemaY].MV_by_User2 with(distribution=round_robin) 
as 
        select A.vendorID, sum(A.totalamount) as S, Count_Big(*) as T 
        from [SchemaX].[T1] A
        inner join [SchemaX].[T2] B on A.vendorID = B.vendorID group by A.vendorID ;
GO
revert;
GO
```

## <a name="see-also"></a>另请参阅

[利用具体化视图进行性能优化](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](./alter-materialized-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)      
[DROP VIEW](./drop-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)  
[EXPLAIN &#40;Transact-SQL&#41;](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD &#40;Transact-SQL&#41;](../database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Azure [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中支持的系统视图](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[Azure [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中支持的 T-SQL 语句](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)
