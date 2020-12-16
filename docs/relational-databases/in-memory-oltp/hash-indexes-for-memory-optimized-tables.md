---
title: 对哈希索引进行故障排除 - 内存优化表
description: 使用此信息对 SQL Server 和 Azure SQL 数据库中内存优化表的哈希索引进行故障排除。
ms.custom: seo-dt-2019
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: e922cc3a-3d6e-453b-8d32-f4b176e98488
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4f6dc92d2d77f453f6838ebe990ed5426c665689
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460470"
---
# <a name="troubleshooting-hash-indexes-for-memory-optimized-tables"></a>内存优化表的哈希索引疑难解答
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

## <a name="prerequisite"></a>先决条件  
  
有关了解本文的重要的上下文信息，请参阅：  
  
- [内存优化表的索引](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)
- [内存优化表的哈希索引](../../relational-databases/sql-server-index-design-guide.md#hash_index) 
 
## <a name="practical-numbers"></a>可行的数字  
  
创建内存优化表的哈希索引时，需要在创建时指定桶计数。 在大多数情况下，桶计数在理想情况下应该介于索引键中非重复值数目的 1 到 2 倍之间。 

不过，即使 BUCKET_COUNT 不太低于或高于首选范围，哈希索引的性能依然很有可能是可忍受或可接受的。 至少应考虑为哈希索引指定约等于内存优化表数据增长后预计包含的行数的 BUCKET_COUNT。  
假设数据不断增长的表包含 2,000,000 行，但预计会增长到原来的 10 倍（即 20,000,000 行）。 那么，首先可以指定一个为表中行数 10 倍的桶计数。 这样，便为行数增加提供了空间。  
  
- 理想情况下，当行的数量达到初始桶计数时，你则会增加桶计数。  
- 即使行的数量增长到桶计数的 5 倍，在大多数情况下，性能表现依然良好。  
  
假设某个哈希索引包含 10,000,000 个非重复键值。  
  
- 桶计数 2,000,000 大致是你可以接受的下限。 这种性能下降的程度是可容忍的。  
  
## <a name="too-many-duplicate-values-in-the-index"></a>索引中的重复值过多？  
  
如果哈希索引值的重复率太高，哈希桶将遇到较长的链。  
  
假设你的 SupportEvent 表与前面 T-SQL 语法代码块中的表相同。 以下 T-SQL 代码演示了如何查找并显示 *所有* 值与 *唯一* 值的比率：  
  
```sql
-- Calculate ratio of:  Rows / Unique_Values.  
DECLARE @allValues float(8) = 0.0, @uniqueVals float(8) = 0.0;  
  
SELECT @allValues = Count(*) FROM SupportEvent;  
  
SELECT @uniqueVals = Count(*) FROM  
  (SELECT DISTINCT SupportEngineerName  
      FROM SupportEvent) as d;  
  
    -- If (All / Unique) >= 10.0, use a nonclustered index, not a hash.   
SELECT Cast((@allValues / @uniqueVals) as float) as [All_divby_Unique];  
go  
```
  
- 如果比率等于或大于 10.0，则表示哈希是一种较差的索引类型。 考虑改用非聚集索引，   
  
## <a name="troubleshooting-hash-index-bucket-count"></a>对哈希索引桶计数进行故障排除  
  
本节介绍如何为哈希索引的桶计数进行故障排除。  
  
### <a name="monitor-statistics-for-chains-and-empty-buckets"></a>监视链和空桶的统计信息  
  
你可以通过运行以下 T-SQL SELECT 来监视哈希索引的统计运行状况。 SELECT 语句使用名为 **sys.dm_db_xtp_hash_index_stats** 的数据管理视图 (DMV)。  
  
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
FROM  
        sys.dm_db_xtp_hash_index_stats  as h   
  JOIN sys.indexes                     as i  
          ON h.object_id = i.object_id  
          AND h.index_id  = i.index_id  
JOIN sys.memory_optimized_tables_internal_attributes ia ON h.xtp_object_id=ia.xtp_object_id
JOIN sys.tables t on h.object_id=t.object_id
WHERE ia.type=1
ORDER BY [table], [index];  
```
  
将 SELECT 结果与以下统计指导原则进行比较：  
  
- 空桶：  
  - 33% 是一个很好的目标值，但更大比例（甚至 90%）通常也是没问题的。  
  - 如果桶计数等于非重复键值的数目，则大约 33% 的桶是空的。  
  - 小于 10% 的值就太小。  
- 桶中的链：  
  - 平均链长为 1 是理想选择，以防出现没有重复索引键值的情况。 链长在 10 以内通常都是可接受的。  
  - 如果平均链长度大于 10，并且空桶百分比大于 10%，则重复的数据就会太多，哈希索引可能不是最适合的类型。  
  
### <a name="demonstration-of-chains-and-empty-buckets"></a>链和空桶的演示  
  
以下 T-SQL 代码块为你提供了一种简单的方法来测试 `SELECT * FROM sys.dm_db_xtp_hash_index_stats;`。 该代码块在 1 分钟内完成。 以下是以下代码块阶段：  
  
1. 创建包含一些哈希索引的内存优化表。  
2. 在表中填充数千行。  
    a. 取模运算符用于配置 StatusCode 列中的值重复率。  
    b. 此循环在大约 1 分钟内插入 262,144 行。  
3. 列显一条消息，要求你从 **sys.dm_db_xtp_hash_index_stats** 运行上述 SELECT。  

```sql
DROP TABLE IF EXISTS SalesOrder_Mem;  
go  
  
  
CREATE TABLE SalesOrder_Mem  
(  
  SalesOrderId   uniqueidentifier  NOT NULL  DEFAULT newid(),  
  OrderSequence  int               NOT NULL,  
  OrderDate      datetime2(3)      NOT NULL,  
  StatusCode     tinyint           NOT NULL,  
  
  PRIMARY KEY NONCLUSTERED  
      HASH (SalesOrderId)  WITH (BUCKET_COUNT = 262144),  
  
  INDEX ix_OrderSequence  
      HASH (OrderSequence) WITH (BUCKET_COUNT = 20000),  
  
  INDEX ix_StatusCode  
      HASH (StatusCode)    WITH (BUCKET_COUNT = 8),  
  
  INDEX ix_OrderDate       NONCLUSTERED (OrderDate DESC)  
)  
  WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
--------------------  
  
SET NOCOUNT ON;  
  
-- Same as PK bucket_count.  68 seconds to complete.  
DECLARE @i int = 262144;  
  
BEGIN TRANSACTION;  
  
WHILE @i > 0  
BEGIN  
  
  INSERT SalesOrder_Mem  
      (OrderSequence, OrderDate, StatusCode)  
    Values  
      (@i, GetUtcDate(), @i % 8);  -- Modulo technique.  
  
  SET @i -= 1;  
END  
COMMIT TRANSACTION;  
  
PRINT 'Next, you should query:  sys.dm_db_xtp_hash_index_stats .';  
go  
```  
  
上述 `INSERT` 循环执行以下操作：  
  
- 插入主键索引和 ix_OrderSequence 的唯一值。  
- 插入了几十万行，但只呈现了 `StatusCode` 的 8 个非重复值。 因此，索引 ix_StatusCode 中的值重复率很高。  
  
为了在桶计数不最佳时进行问题排查，请从 **sys.dm_db_xtp_hash_index_stats** 检查 SELECT 的以下输出。 对于这些结果，我们添加 `WHERE Object_Name(h.object_id) = 'SalesOrder_Mem'` 到从 D.1 部分复制的 SELECT。  
  
`SELECT` 结果显示在代码后面。为了能够更好地展示结果，我们人为地将结果拆分为两个范围较窄的结果表。  
  
- 这里是 *桶计数* 的结果。  
  
| IndexName | total_bucket_count | empty_bucket_count | EmptyBucketPercent |  
| :-------- | -----------------: | -----------------: | -----------------: |  
| ix_OrderSequence | 32768 | 13 | 0 |  
| ix_StatusCode | 8 | 4 | 50 |  
| PK_SalesOrd_B14003... | 262144 | 96525 | 36 |  
  
- 接下来是 *链长度* 的结果。  
  
| IndexName | avg_chain_length | max_chain_length |  
| :-------- | ---------------: | ---------------: |  
| ix_OrderSequence | 8 | 26 |  
| ix_StatusCode | 65536 | 65536 |  
| PK_SalesOrd_B14003... | 1 | 8 |  
  
让我们解释上述结果表中的三个哈希索引：  
  
*ix_StatusCode：*  
  
- 50% 的桶是空的，这很好。  
- 但是，平均链长度非常大，为65,536。  
  - 这表明值重复率很高。  
  - 因此，在此情况下不适合使用哈希索引。 应改用非聚集索引。  
  
*ix_OrderSequence：*  
  
- 0% 的桶是空的，这太低了。  
- 平均链长度是 8，不过，此索引中的所有值都是唯一的。  
  - 因此，应增加桶计数，以便将平均链长度减小到接近 2 或 3。  
- 由于索引键具有 262144 个唯一值，因此桶计数应至少为 262144。  
  - 如果预期将来会增长，则桶计数还应该更高。  
  
*主键索引 (PK_SalesOrd_...)：*  
  
- 36% 的桶是空的，这很好。  
- 平均链长度为 1，这也不错。 无需进行更改。  
  
### <a name="balancing-the-trade-off"></a>权衡取舍  
  
OLTP 工作负载注重每个行。 全表扫描通常不是影响 OLTP 工作负载的关键因素。 因此，必须在内存使用量与相等测试和插入操作性能之间权衡取舍。  
  
**如果内存使用量是更大的考虑因素：**  
  
- 选择近似于索引键记录数的桶计数。  
- 该 Bucket 计数不应显著小于索引键值数，因为这会影响大多数 DML 操作以及在服务器重新启动后恢复数据库所需的时间。  
  
**如果相等性测试的性能是更大的考虑因素：**  
  
- 提高桶计数，唯一索引值数目的两倍或三倍比较合适。 提高计数意味着：  
  - 查找某个特定值时检索速度更快。  
  - 内存使用量增加。  
  - 完全扫描哈希索引所需的时间增加。  
  

##  <a name="additional-reading"></a><a name="Additional_Reading"></a> 补充阅读  
 [内存优化表的哈希索引](../../relational-databases/sql-server-index-design-guide.md#hash_index)   
 [内存优化表的非聚集索引](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index)  
