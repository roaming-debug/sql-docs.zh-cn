---
description: 使用带有系统版本的内存优化临时表
title: 使用系统版本控制的内存优化临时表 | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 691d4f80-6754-43f5-8b43-d4facf08f6fc
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: acfdadf0216bd243f61339982615c4bf83796dfa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177980"
---
# <a name="working-with-memory-optimized-system-versioned-temporal-tables"></a>使用带有系统版本的内存优化临时表


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


本主题介绍了使用带有系统版本的内存优化临时表与使用带有系统版本的基于磁盘的临时表有何不同。

> [!NOTE]
> 内存优化临时表仅适用于 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ，而不适用于 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。

## <a name="discovering-metadata"></a>发现元数据

若要发现带有系统版本的内存优化临时表的元数据，你需要合并 [sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) 和 [sys.internal_tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md) 中的信息。 带有系统版本的临时表会显示为内部内存中历史记录表的 parent_object_id。

此示例展示了如何查询和联接这些表。

```sql
SELECT SCHEMA_NAME (T1.schema_id) as TemporalTableSchema
    , OBJECT_NAME(IT.parent_object_id) as TemporalTableName
    , T1.object_id as TemporalTableObjectId
    , IT.Name as InternalHistoryStagingName
    , SCHEMA_NAME (T2.schema_id) as HistoryTableSchema
    , OBJECT_NAME (T1.history_table_id) as HistoryTableName
FROM sys.internal_tables IT
JOIN sys.tables T1
    ON IT.parent_object_id = T1.object_id
JOIN sys.tables T2
    ON T1.history_table_id = T2.object_id
WHERE T1.is_memory_optimized = 1 AND T1.temporal_type = 2

```

## <a name="modifying-data"></a>修改数据

可以通过本机编译的存储过程来修改带有系统版本的内存优化临时表，这样你就能将非内存优化临时表转换成带有系统版本的内存优化临时表，并保留现有的本机存储过程。

此示例展示了如何在本机编译模块中修改以前创建的表。

```sql
CREATE PROCEDURE dbo.UpdateFXCurrencyPair
   (
      @ProviderID int
      , @CurrencyID1 int
      , @CurrencyID2 int
      , @BidRate decimal(8,4)
      , @AskRate decimal(8,4)
   )
WITH NATIVE_COMPILATION, SCHEMABINDING
   , EXECUTE AS OWNER
AS
   BEGIN ATOMIC WITH
   (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'English')
      UPDATE dbo.FXCurrencyPairs SET AskRate = @AskRate, BidRate = @BidRate
     WHERE ProviderID = @ProviderID AND CurrencyID1 = @CurrencyID1 AND CurrencyID2 = @CurrencyID2
END
GO ;

```

## <a name="see-also"></a>另请参阅

- [系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [创建系统版本控制的内存优化临时表](../../relational-databases/tables/creating-a-memory-optimized-system-versioned-temporal-table.md)
- [监视系统版本控制型内存优化临时表](../../relational-databases/tables/monitoring-memory-optimized-system-versioned-temporal-tables.md)
- [经过内存优化和系统版本控制的时态表的性能注意事项](../../relational-databases/tables/
- [临时表](../../relational-databases/tables/temporal-tables.md)
- [临时表系统一致性检查](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [管理版本由系统控制的临时表中历史数据的保留期](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [临时表元数据视图和函数](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
