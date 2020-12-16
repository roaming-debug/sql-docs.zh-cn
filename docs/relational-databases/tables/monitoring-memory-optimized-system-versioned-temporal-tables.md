---
description: 监视系统版本控制型内存优化临时表
title: 监视系统版本控制的内存优化临时表 | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 7a06785d-dbcb-44de-b95c-26b131471bee
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e593417e5b88037ad3fddbfba4a8cb99ddb50e94
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484459"
---
# <a name="monitoring-memory-optimized-system-versioned-temporal-tables"></a>监视系统版本控制型内存优化临时表


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


每个系统版本控制型内存优化表都可以使用现有视图来跟踪详细的和摘要性的内存占用情况。

详细的内存占用情况（按每个主要的系统版本控制型内部历史记录临时表进行拆分）：

```sql
--Details of memory consumption
WITH InMemoryTemporalTables
AS
   (
      SELECT SCHEMA_NAME ( T1.schema_id ) AS TemporalTableSchema
         , T1.object_id AS TemporalTableObjectId
         , IT.object_id AS InternalTableObjectId
         , OBJECT_NAME ( IT.parent_object_id ) AS TemporalTableName
         , IT.Name AS InternalHistoryStagingName
      FROM sys.internal_tables IT
      JOIN sys.tables T1 ON IT.parent_object_id = T1.object_id
      WHERE T1.is_memory_optimized = 1 AND T1.temporal_type = 2
   )
SELECT
   TemporalTableSchema
   , T.TemporalTableName
   , T.InternalHistoryStagingName,
      CASE
         WHEN C.object_id = T.TemporalTableObjectId
         THEN 'Temporal Table Consumption'
         ELSE 'Internal Table Consumption'
         END ConsumedBy
   , C.*
FROM sys.dm_db_xtp_memory_consumers C
JOIN InMemoryTemporalTables T
   ON C.object_id = T.TemporalTableObjectId OR C.object_id = T.InternalTableObjectId
   WHERE T.TemporalTableSchema = 'dbo' AND T.TemporalTableName = 'FXCurrencyPairs'
;
```

内存占用情况摘要（系统版本控制型内存优化表的总计）：

```sql
--Summary of memory consumption
WITH InMemoryTemporalTables
AS
   (
      SELECT SCHEMA_NAME ( T1.schema_id ) AS TemporalTableSchema
         , T1.object_id AS TemporalTableObjectId
         , IT.object_id AS InternalTableObjectId
         , OBJECT_NAME ( IT.parent_object_id ) AS TemporalTableName
         , IT.Name AS InternalHistoryStagingName
      FROM sys.internal_tables IT
      JOIN sys.tables T1 ON IT.parent_object_id = T1.object_id
      WHERE T1.is_memory_optimized = 1 AND T1.temporal_type = 2
   )
, DetailedConsumption
AS
(
   SELECT TemporalTableSchema
      , T.TemporalTableName
      , T.InternalHistoryStagingName
      , CASE
         WHEN C.object_id = T.TemporalTableObjectId
         THEN 'Temporal Table Consumption'
         ELSE 'Internal Table Consumption'
         END ConsumedBy
      , C.*
   FROM sys.dm_db_xtp_memory_consumers C
   JOIN InMemoryTemporalTables T
   ON C.object_id = T.TemporalTableObjectId OR C.object_id = T.InternalTableObjectId
)
SELECT TemporalTableSchema
   TemporalTableName
   , sum ( allocated_bytes ) AS allocated_bytes
   , sum ( used_bytes ) AS used_bytes
FROM DetailedConsumption
WHERE TemporalTableSchema = 'dbo' ANDTemporalTableName = 'FXCurrencyPairs'
GROUP BY TemporalTableSchema, TemporalTableName
;
```

## <a name="see-also"></a>另请参阅

- [系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [创建系统版本控制的内存优化临时表](../../relational-databases/tables/creating-a-memory-optimized-system-versioned-temporal-table.md)
- [使用带有系统版本的内存优化临时表](../../relational-databases/tables/working-with-memory-optimized-system-versioned-temporal-tables.md)
- [内存优化系统版本控制临时表的性能注意事项](../../relational-databases/tables/memory-optimized-system-versioned-temporal-tables-performance.md)
- [临时表](../../relational-databases/tables/temporal-tables.md)
- [临时表系统一致性检查](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [管理版本由系统控制的临时表中历史数据的保留期](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [临时表元数据视图和函数](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
