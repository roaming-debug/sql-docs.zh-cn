---
title: 内存要求 - 内存优化表
description: 了解 SQL Server 中内存优化表的内存使用和管理方案，这些表需要为所有行和索引提供足够的内存。
ms.custom: seo-dt-2019
ms.date: 12/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 5c5cc1fc-1fdf-4562-9443-272ad9ab5ba8
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d5372743bc76fc30c3fb023e38e5ec14630b7011
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237214"
---
# <a name="estimate-memory-requirements-for-memory-optimized-tables"></a>估算内存优化表的内存需求
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

内存优化表要求存在足够的内存，以便将所有行和索引保留在内存中。 因为内存是有限的资源，所以了解和管理系统的内存使用情况非常重要。 本节中的主题介绍一些常见的内存使用和管理方案。

无论是新建内存优化表还是将现有的磁盘表迁移至 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 内存优化表，都请合理估算每个表的内存需求，以便为服务器提供充足的内存。 本节介绍如何估算使用内存优化表存放数据时所需的内存大小。  
  
如果考虑从基于磁盘的表迁移至内存优化表，在继续阅读本主题前，请先阅读主题 [确定表或存储过程是否应移植到内存中 OLTP](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md) ，了解哪类表适合迁移。 [迁移到内存中 OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md) 下的所有主题均提供了有关从基于磁盘的表迁移至内存优化表的指导。 
  
## <a name="basic-guidance-for-estimating-memory-requirements"></a>估计内存需求量的基本指南

尽管表确实需要与内存大小相适应，但从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]开始，内存优化表的大小已没有限制。  在 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中，SCHEMA_AND_DATA 表支持的数据大小为 256GB。

内存优化表的大小与数据大小加上一些行标题的开销大小之和相对应。 当将基于磁盘的表迁移到内存优化表时，内存优化表的大小将大致与原始基于磁盘的表的聚集索引或堆的大小相对应。

内存优化表的索引通常比基于磁盘的表的非聚集索引小。 非聚集索引的大小按照 `[primary key size] * [row count]`顺序排列。 哈希索引的大小为 `[bucket count] * 8 bytes`。 

当有活动的工作负荷时，则需要更多的内存以应对行版本控制和各种操作。 在实践中需要的内存量取决于工作负荷，但为安全起见，建议以内存优化表和索引预期大小的两倍开始，并观察实践中的内存需求。 行版本控制的开销始终取决于工作负荷的特征 - 特别是长时间运行的事务会增加开销。 对于大多数使用较大数据库的工作负荷（例如 > 100GB），通常会限制开销（25% 或更少）。

  
## <a name="detailed-computation-of-memory-requirements"></a>内存需求量的详细计算 
  
- [内存优化表示例](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_ExampleTable)  
  
- [表占用的内存](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_MemoryForTable)  
  
- [索引占用的内存](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_IndexMeemory)  
  
- [行版本控制占用的内存](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_MemoryForRowVersions)  
  
- [表变量占用的内存](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_TableVariables)  
  
- [表增长占用的内存](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_MemoryForGrowth)  
  
###  <a name="example-memory-optimized-table"></a><a name="bkmk_ExampleTable"></a> 内存优化表示例  

考虑以下内存优化的表架构：
  
```sql  
CREATE TABLE t_hk
(  
  col1 int NOT NULL  PRIMARY KEY NONCLUSTERED,  

  col2 int NOT NULL  INDEX t1c2_index   
      HASH WITH (bucket_count = 5000000),  

  col3 int NOT NULL  INDEX t1c3_index   
      HASH WITH (bucket_count = 5000000),  

  col4 int NOT NULL  INDEX t1c4_index   
      HASH WITH (bucket_count = 5000000),  

  col5 int NOT NULL  INDEX t1c5_index NONCLUSTERED,  

  col6 char (50) NOT NULL,  
  col7 char (50) NOT NULL,   
  col8 char (30) NOT NULL,   
  col9 char (50) NOT NULL  

)   WITH (memory_optimized = on)  ;
GO  
```  

我们将使用该架构来确定此内存优化表所需的最低内存。  
  
###  <a name="memory-for-the-table"></a><a name="bkmk_MemoryForTable"></a> 表占用的内存  

内存优化表行包含三个部分：
  
- **时间戳**   
    行标题/时间戳 = 24 个字节。  
  
- **索引指针**   
    对于表中的每个哈希索引，每行包含一个指向索引中下一行的 8 字节地址指针。  因为有 4 个索引，每行将为索引指针分配 32 个字节（每个索引需要一个 8 字节的指针）。  
  
- **数据**   
    行中数据部分的大小由各数据列类型大小的总和决定。  示例表中包含五个 4 字节整数、三个 50 字节的字符列和一个 30 字节的字符列。  因此，每行的数据部分为 4 + 4 + 4 + 4 + 4 + 50 + 50 + 30 + 50 共 200 个字节。  
  
下面是内存优化表中 5,000,000（5 百万）行的大小计算： 按以下方式估计数据行使用的总内存：  
  
#### <a name="memory-for-the-tables-rows"></a>表行占用的内存  
  
根据上述计算，内存优化表中每行的大小为 24 + 32 + 200，即 256 个字节。  总共有 5 百万行，则表将占用 5,000,000 * 256 字节，共 1,280,000,000 字节 - 大约 1.28 GB。  
  
###  <a name="memory-for-indexes"></a><a name="bkmk_IndexMeemory"></a> 索引占用的内存  

#### <a name="memory-for-each-hash-index"></a>每个哈希索引占用的内存  
  
每个哈希索引是一个由 8 字节地址指针组成的哈希数组。  该数组的大小最好通过索引中唯一索引键的数目确定，例如：使用唯一 Col2 键的个数作为 t1c2_index 的数组大小。 哈希数组太大，浪费内存。  哈希数组太小，又会降低性能（许多索引键经哈希操作后映射至同一索引，导致冲突过多）。  
  
哈希索引可实现高速查找，如：  
  
```sql  
SELECT * FROM t_hk  
   WHERE Col2 = 3;
```  
  
非聚集索引在执行范围查找时更快，如：  
  
```sql  
SELECT * FROM t_hk  
   WHERE Col2 >= 3;
```  
  
如果是迁移磁盘表，可使用下面的方法确定索引 t1c2_index 的唯一键的个数。  
  
```sql
SELECT COUNT(DISTINCT [Col2])  
  FROM t_hk;
```  
  
如果是创建新表，则需要估算数组大小或在部署前通过测试收集数据。  
  
关于 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 内存优化表中哈希索引的工作原理，请参阅 [哈希索引](/previous-versions/sql/sql-server-2016/dn133190(v=sql.130))。  
  
#### <a name="setting-the-hash-index-array-size"></a>设置哈希索引数组的大小  
  
哈希数组大小由 `(bucket_count= value)` 设置，其中 `value` 为大于零的整数值。 如果 `value` 不为 2 的幂，实际的 bucket_count 将舍入为下一个最近的 2 的幂。  在示例表中，(bucket_count = 5000000)，由于 5,000,000 并非 2 的幂，实际桶计数舍入为 8,388,608 (2^23)。  在计算此哈希数组所需的内存时，必须使用该值，而非 5,000,000。  
  
因此，在本示例中，每个哈希数组所需的内存为：  
  
8,388,608 * 8 = 2^23 \* 8 = 2^23 \* 2^3 = 2^26 = 67,108,864，约 64 MB。  
  
有三个哈希索引，所以哈希索引占用的内存为 3 * 64MB = 192MB。  
  
#### <a name="memory-for-nonclustered-indexes"></a>非聚集索引占用的内存  
  
非聚集索引采用 B 树实现，其内部节点包含索引键和指向后续节点的指针。  叶节点包含索引键和指向内存中表行的指针。  
  
与哈希索引不同的是，非聚集索引的 Bucket 大小不固定。 非聚集索引随数据动态扩展/收缩。  
  
可按如下方式计算非聚集索引占用的内存：  
  
- **为非叶节点分配的内存**   
    对于典型配置，分配给非叶节点的内存只占索引所占用的整个内存的很小的百分比。 其占用的内存少到可以安全地忽略。  
  
- **叶节点占用的内存**   
    叶节点对于表中的每个唯一键都具有 1 行，并且指向具有该唯一键的数据行。  如果多行具有相同的键（即，存在不唯一的非聚集索引），则索引页节点中只有一行指向其中某一行，其他行则相互链接在一起。  因此，可通过下面的公式估算所需的总内存：
  - 非聚集索引所需内存 = （指针大小 + Sum (键列数据类型大小))* 唯一键行数  
  
 非聚集索引的最佳应用是范围查找，下面的查询即为例证：  
  
```sql  
SELECT * FROM t_hk  
   WHERE c2 > 5;  
```  
  
###  <a name="memory-for-row-versioning"></a><a name="bkmk_MemoryForRowVersions"></a> 行版本控制占用的内存

为避免锁定，内存 OLTP 在更新或删除行时采用乐观并发策略。 也就是说，对某行进行更新时，将额外创建一个该行的版本。 此外，删除是逻辑性的 — 现有行会标记为已删除，但并未立即删除。 系统将保持旧的行版本（包括已删除行）可用，直至可能会用到该版本的所有事务执行完毕为止。 
  
在任意时刻，内存中都可能存在一定数量的额外行等待垃圾回收周期释放其内存，因此，必须具有足够的内存来容纳这些额外的行。  
  
可以通过计算每秒的最大行更新数和最大删除数，然后将其乘以事务所用的最长秒数（最少为 1）来估算额外的行数。  
  
再用该值乘以行大小即可获得行版本控制所需占用的字节数。  
  
`rowVersions = durationOfLongestTransctoinInSeconds * peakNumberOfRowUpdatesOrDeletesPerSecond`  
  
可通过下面的公式估算陈旧行的内存需求：陈旧行数目 内存优化表的行大小（参见上文的 [表占用的内存](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_MemoryForTable) ）。  
  
`memoryForRowVersions = rowVersions * rowSize`  
  
###  <a name="memory-for-table-variables"></a><a name="bkmk_TableVariables"></a> 表变量占用的内存
  
只有在表变量超出范围时，才能释放表变量占用的内存。 来自表变量的行（包括作为更新的一部分删除的行）不会进行垃圾回收。 在表变量退出作用域之前不会释放内存。  
  
与过程作用域相反，用于许多事务的在大型 SQL 批处理中定义的表变量可能会占用大量内存。 因为不对它们进行垃圾回收，所以，表变量中已删除的行可能会使用大量内存并且降低性能，这是因为读取操作需要扫描已删除的行。  
  
###  <a name="memory-for-growth"></a><a name="bkmk_MemoryForGrowth"></a> 表增长占用的内存

上述计算结果为表当前所需的内存。 除该内存大小外，还需对表的增长作出预测并提供充足的内存来满足增长所需。  例如，如预测表会有 10% 的增长，则需将上述结果乘以 1.1 来算出表所需的总内存。  
  
## <a name="see-also"></a>另请参阅

[迁移到内存中 OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)