---
description: STATS_DATE (Transact-SQL)
title: STATS_DATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STATS_DATE_TSQL
- STATS_DATE
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], last time updated
- STATS_DATE function
- query optimization statistics [SQL Server], last time updated
- last time statistics updated
- stats update date
ms.assetid: f9ec3101-1e41-489d-b519-496a0d6089fb
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ca416d3de8d295cd9f7f535ab2d3b8b65df150f6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181251"
---
# <a name="stats_date-transact-sql"></a>STATS_DATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回表或索引视图上统计信息的最新更新的日期。  
  
 有关更新统计信息的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
STATS_DATE ( object_id , stats_id )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 object_id  
 具有统计信息的表或索引视图的 ID。  
  
 *stats_id*  
 统计信息对象的 ID。  
  
## <a name="return-types"></a>返回类型  
 如果成功，则返回 datetime。 如果未创建统计信息 blob，则返回 NULL。  
  
## <a name="remarks"></a>备注  
 系统函数可以在选择列表、WHERE 子句和任何允许使用表达式的地方使用。  
 
 统计信息更新日期连同[直方图](../../relational-databases/statistics/statistics.md#histogram)和[密度矢量](../../relational-databases/statistics/statistics.md#density)一起存储在[统计信息 blob 对象](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics)中，而不是存储在元数据中。 如果未读取到任何数据，无法生成统计信息数据，则不会创建统计信息 blob，且该日期不可用。 针对谓词不返回任何行或新的空表，筛选的统计信息便是这种情况。
 
 如果统计信息对应于索引，则 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) 目录视图中的 stats_id 值与 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 目录视图中的 index_id 值相同。
  
## <a name="permissions"></a>权限  
 若要查看表或索引视图的元数据，需要 db_owner 固定数据库角色中的成员身份或权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-return-the-dates-of-the-most-recent-statistics-for-a-table"></a>A. 返回表的最近统计信息的日期  
 下面的示例返回 `Person.Address` 表上的每个统计信息对象的最新更新的日期。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_update_date  
FROM sys.stats   
WHERE object_id = OBJECT_ID('Person.Address');  
GO  
```  
  
 如果统计信息对应于索引，则 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) 目录视图中的 stats_id 值与 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 目录视图中的 index_id 值相同，以下查询返回与上一查询相同的结果。 如果统计信息不对应于索引，则它们位于 sys.stats 结果中，但是不在 sys.indexes 结果中。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name AS index_name,   
    STATS_DATE(object_id, index_id) AS statistics_update_date  
FROM sys.indexes   
WHERE object_id = OBJECT_ID('Person.Address');  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-learn-when-a-named-statistics-was-last-updated"></a>B. 了解已命名统计信息的上次更新时间  
 以下示例在 DimCustomer 表的 LastName 列上创建统计信息。 然后运行查询来显示统计信息的日期。 之后更新统计信息，并再次运行查询来显示更新后的日期。  
  
```sql
--First, create a statistics object  
USE AdventureWorksPDW2012;  
GO  
CREATE STATISTICS Customer_LastName_Stats  
ON AdventureWorksPDW2012.dbo.DimCustomer (LastName)  
WITH SAMPLE 50 PERCENT;  
GO  
  
--Return the date when Customer_LastName_Stats was last updated  
USE AdventureWorksPDW2012;  
GO  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer')  
    AND s.name = 'Customer_LastName_Stats';  
GO  
  
--Update Customer_LastName_Stats so it will have a different timestamp in the next query  
GO  
UPDATE STATISTICS dbo.dimCustomer (Customer_LastName_Stats);  
  
--Return the date when Customer_LastName_Stats was last updated.  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer')  
    AND s.name = 'Customer_LastName_Stats';  
GO    
```  
  
### <a name="c-view-the-date-of-the-last-update-for-all-statistics-on-a-table"></a>C. 查看表中所有统计信息的上次更新日期  
 此示例返回 DimCustomer 表中的每个统计信息对象最后一次更新的日期。  
  
```sql  
--Return the dates all statistics on the table were last updated.  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer');  
GO  
```  
  
 如果统计信息对应于索引，则 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) 目录视图中的 stats_id 值与 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 目录视图中的 index_id 值相同，以下查询返回与上一查询相同的结果。 如果统计信息不对应于索引，则它们位于 sys.stats 结果中，但是不在 sys.indexes 结果中。  
  
```sql  
USE AdventureWorksPDW2012;  
GO  
SELECT name AS index_name,   
    STATS_DATE(object_id, index_id) AS statistics_update_date  
FROM sys.indexes   
WHERE object_id = OBJECT_ID('dbo.DimCustomer');  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [System Functions (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [UPDATE STATISTICS (Transact-SQL)](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_autostats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)   
 [统计信息](../../relational-databases/statistics/statistics.md)    
 [sys.dm_db_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
  
  

