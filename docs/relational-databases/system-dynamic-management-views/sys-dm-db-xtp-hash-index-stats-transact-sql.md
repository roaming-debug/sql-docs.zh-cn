---
description: sys.dm_db_xtp_hash_index_stats (Transact-SQL)
title: sys.dm_db_xtp_hash_index_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_xtp_hash_index_stats
- sys.dm_db_xtp_hash_index_stats_TSQL
- dm_db_xtp_hash_index_stats
- dm_db_xtp_hash_index_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_hash_index_stats (dynamic management view)
ms.assetid: 45969884-cd61-48e8-aee5-c725c78e3e4c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f0fe801529ce7706c71ed23a8e9b077ba8aa5747
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99108755"
---
# <a name="sysdm_db_xtp_hash_index_stats-transact-sql"></a>sys.dm_db_xtp_hash_index_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  这些统计信息可用来了解和优化存储桶计数。 它还可用于检测索引键具有许多重复项的情况。  
  
 平均链长度较大指示将许多行哈希处理至同一个存储桶。 之所以出现这种情况，原因可能是：  
  
-   如果空存储桶的数目较少，或平均链长度和最大链长度接近，则存储桶总数可能会过少。 这将导致许多不同的索引键哈希处理至同一个存储桶中。  
  
-   如果空存储桶的数目较高，或最大链长度比平均链长度高，则可能许多行具有重复的索引键值，或者键值存在偏斜。 索引键相同的所有行都将哈希处理至同一个存储桶，因而，该存储桶有较长的链长度。  
  
链长度较长可能会显著影响针对各单独行的所有 DML 操作的性能，包括 SELECT 和 INSERT。 链长度较短以及空存储桶计数较高指示 bucket_count 过高。 这将降低索引扫描的性能。  
  
> [!WARNING]
> **sys.dm_db_xtp_hash_index_stats** 扫描整个表。 如果数据库中有大型表， **sys.dm_db_xtp_hash_index_stats** 可能需要较长时间才能运行。  
  
有关详细信息，请参阅 [Memory-Optimized 表的哈希索引](../../relational-databases/sql-server-index-design-guide.md#hash_index)。  
  
|列名称|类型|描述|  
|-----------------|----------|-----------------|  
|object_id|**int**|父表的对象 ID。|  
|xtp_object_id|**bigint**|内存优化表的 ID。|  
|index_id|**int**|索引 ID。|  
|total_bucket_count|**bigint**|索引中哈希存储桶的总数。|  
|empty_bucket_count|**bigint**|索引中空哈希存储桶的数量。|  
|avg_chain_length|**bigint**|索引中所有哈希存储桶的平均行链长度。|  
|max_chain_length|**bigint**|哈希存储桶中的最大行链长度。|  
|xtp_object_id|**bigint**|与内存优化表对应的内存中 OLTP 对象 ID。|  
  
## <a name="permissions"></a>权限  
 要求对服务器具有 VIEW DATABASE STATE 权限。  

## <a name="examples"></a>示例  
  
### <a name="a-troubleshooting-hash-index-bucket-count"></a>A. 对哈希索引桶计数进行故障排除

以下查询可用于排查现有表的哈希索引桶计数。 查询返回有关用户表中所有哈希索引的空 bucket 和链长百分比的统计信息。

```sql
  SELECT  
    QUOTENAME(SCHEMA_NAME(t.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(h.object_id)) as [table],   
    i.name                   as [index],   
    h.total_bucket_count,  
    h.empty_bucket_count,  
    FLOOR((  
      CAST(h.empty_bucket_count as float) /  
        h.total_bucket_count) * 100)  
                             as [empty_bucket_percent],  
    h.avg_chain_length,   
    h.max_chain_length  
  FROM sys.dm_db_xtp_hash_index_stats as h   
  INNER JOIN sys.indexes as i  
            ON h.object_id = i.object_id  
           AND h.index_id  = i.index_id  
    INNER JOIN sys.memory_optimized_tables_internal_attributes ia ON h.xtp_object_id=ia.xtp_object_id
    INNER JOIN sys.tables t on h.object_id=t.object_id
  WHERE ia.type=1
  ORDER BY [table], [index];  
``` 

有关如何解释此查询的结果的详细信息，请参阅 [排查 Memory-Optimized 表的哈希索引问题](../../relational-databases/in-memory-oltp/hash-indexes-for-memory-optimized-tables.md) 。  

### <a name="b-hash-index-statistics-for-internal-tables"></a>B. 内部表的哈希索引统计信息

某些功能使用利用哈希索引的内部表，例如内存优化表上的列存储索引。 下面的查询返回链接到用户表的内部表的哈希索引的统计信息。

```sql
  SELECT  
    QUOTENAME(SCHEMA_NAME(t.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(h.object_id)) as [user_table],
    ia.type_desc as [internal_table_type],
    i.name                   as [index],   
    h.total_bucket_count,  
    h.empty_bucket_count,  
    h.avg_chain_length,   
    h.max_chain_length  
  FROM sys.dm_db_xtp_hash_index_stats as h   
  INNER JOIN sys.indexes as i  
            ON h.object_id = i.object_id  
           AND h.index_id  = i.index_id  
    INNER JOIN sys.memory_optimized_tables_internal_attributes ia ON h.xtp_object_id=ia.xtp_object_id
    INNER JOIN sys.tables t on h.object_id=t.object_id
  WHERE ia.type!=1
  ORDER BY [user_table], [internal_table_type], [index]; 
```

请注意，不能更改内部表的索引 BUCKET_COUNT，因此应仅将此查询的输出视为 "信息性"。 因此不需要执行任何操作。  

此查询不应返回任何行，除非您使用的功能利用了内部表的哈希索引。 以下内存优化表包含列存储索引。 创建此表后，将看到内部表的哈希索引。

```sql
  CREATE TABLE dbo.table_columnstore
  (
    c1 INT NOT NULL PRIMARY KEY NONCLUSTERED,
    INDEX ix_columnstore CLUSTERED COLUMNSTORE
  ) WITH (MEMORY_OPTIMIZED=ON)
```

## <a name="see-also"></a>另请参阅  
 [内存优化表动态管理视图 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
