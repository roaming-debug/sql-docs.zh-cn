---
title: 通过内存中 OLTP 加速 T-SQL 性能
description: 了解 SQL Server 和 Azure SQL 数据库的内存中 OLTP 性能功能的基础知识，其中包含面向开发人员的快速说明和核心代码示例。
ms.custom: seo-dt-2019
ms.date: 09/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 1c25a164-547d-43c4-8484-6b5ee3cbaf3a
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5287bb37b779775edb3375d545c1745c6ed63e93
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438742"
---
# <a name="survey-of-initial-areas-in-in-memory-oltp"></a>内存中 OLTP 内的初始领域调查

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  
本文适用于急于了解 Microsoft SQL Server 和 Azure SQL 数据库的内存中 OLTP 性能功能基础知识的开发人员。  
  
有关内存中 OLTP，本文提供以下内容：  
  
- 功能简介。  
- 实现功能的核心代码示例。  
  
  
SQL Server 和 SQL 数据库在内存中技术支持方面只有细微的不同。  
  
  
在现实中，一些博客主将内存中 OLTP 称为 *Hekaton*。  
  
  
<a name="benefits-of-in-memory-features-21a"></a>  
  
## <a name="benefits-of-in-memory-features"></a>内存中功能的优点  
  
SQL Server 提供的内存中功能可极大提升许多应用程序系统的性能。 本部分以直接明了的方式介绍了相关注意事项。  
  
  
### <a name="features-for-oltp-online-transactional-processing"></a>OLTP（联机事务处理）的功能  
  
  
必须同时处理大量 SQL INSERT 的系统最适合采用 OLTP 功能。  
  
- 我们的基准显示，通过采用内存中功能，速度可提升 5 倍到超过 20 倍不等。  
  
  
处理 Transact-SQL 中的繁重计算的系统很适合采用此功能。  
  
- 专用于繁重计算的存储过程可提速高达 99 倍。  
  
  
稍后你可以查看以下文章，这些文章提供从内存中 OLTP 获取性能的演示：  
  
- [演示：内存中 OLTP 的性能改善](../../relational-databases/in-memory-oltp/demonstration-performance-improvement-of-in-memory-oltp.md)通过小规模演示来演示较大的潜在性能提升。  
- [内存中 OLTP 的示例数据库](../../relational-databases/in-memory-oltp/sample-database-for-in-memory-oltp.md)提供了更大规模的演示。  
  
  
  
### <a name="features-for-operational-analytics"></a>操作分析的功能  
  
内存中分析是指 SQL SELECT，它聚合事务数据，通常通过包含一个 GROUP BY 子句来实现。 称为 *columnstore* 的索引类型是操作分析的核心。  
  
有两种主要方案：  
  
- “批操作分析” 是指在工作时间后运行或在具有事务数据副本的辅助硬件上运行的聚合进程。  
  - [Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is) 也与批操作分析相关。  
- “实时操作分析”是指工作时间内在用于事务工作负载的主硬件上运行的聚合进程。  
  
  
本文重点介绍 OLTP，而不是介绍相关分析。 有关列存储索引如何将分析引入 SQL 的信息，请参阅：  
  
- [开始使用列存储进行实时运行分析](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
- [列存储索引指南](../../relational-databases/indexes/columnstore-indexes-overview.md)  
  
  
> [!NOTE]
> 有关内存中功能的一个两分钟视频可在 [Azure SQL 数据库 - 内存中技术](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-SQL-Database-In-Memory-Technologies)中找到。 该视频的发布日期为 2015 年 12 月。  


### <a name="columnstore"></a>列存储

一系列精彩的博客文章从多个视角完美地介绍了列存储索引。 大部分文章都在深入介绍列存储支持的实时运营分析的概念。  这些文章由 Microsoft 项目经理 Sunil Agarwal 于 2016年 3 月创作。

#### <a name="real-time-operational-analytics"></a>实时运行分析

1. [Real-Time Operational Analytics Using In-Memory Technology](https://blogs.technet.microsoft.com/dataplatforminsider/2015/12/09/real-time-operational-analytics-using-in-memory-technology/)
2. [Real-Time Operational Analytics - Overview nonclustered columnstore index (NCCI)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-using-nonclustered-columnstore-index)
3. [实时运行分析：SQL Server 2016 中使用非聚集列存储索引 (NCCI) 的简单示例](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-simple-example-using-nonclustered-clustered-columnstore-index-ncci)
4. [实时运行分析：SQL Server 2016 中的 DML 操作和非聚集列存储索引 (NCCI)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-dml-operations-and-nonclustered-columnstore-index-ncci-in-sql-server-2016)
5. [实时运行分析：已筛选的非聚集列存储索引 (NCCI)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci)
6. [实时运行分析：非聚集列存储索引 (NCCI) 的压缩延迟选项](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci)
7. [实时运行分析：NCCI 及性能的压缩延迟选项](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-with-ncci-and-the-performance)
8. [实时运行分析：内存优化表和列存储索引](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-memory-optimized-table-and-columnstore-index)

#### <a name="defragment-a-columnstore-index"></a>对列存储索引进行碎片整理

1. [使用 REORGANIZE 命令的列存储索引碎片整理](/archive/blogs/sqlserverstorageengine/columnstore-index-defragmentation-using-reorganize-command)
2. [REORGANIZE 的列存储索引合并策略](/archive/blogs/sqlserverstorageengine/columnstore-index-merge-policy-for-reorganize)

#### <a name="bulk-importation-of-data"></a>批量导入数据

1. [聚集列存储：大容量加载](/archive/blogs/sqlserverstorageengine/clustered-column-store-index-bulk-loading-the-data)
2. [聚集列存储索引：数据加载优化 - 最小日志记录](/archive/blogs/sqlserverstorageengine/clustered-columnstore-index-data-load-optimizations-minimal-logging)
3. [聚集列存储索引：数据加载优化 - 并行批量导入](/archive/blogs/sqlserverstorageengine/clustered-columnstore-index-parallel-bulk-import)





<a name="features-on-in-memory-oltp-13b"></a>  
  
## <a name="features-of-in-memory-oltp"></a>内存中 OLTP 的功能  
  
让我们来了解一下内存中 OLTP 的主要功能。  
  
  
#### <a name="memory-optimized-tables"></a>内存优化表  
  
CREATE TABLE 语句上的 T-SQL 关键字 MEMORY_OPTIMIZED 是表格被创建的方式，目的是令表格存在于活动内存而非磁盘上。  
  
  
[内存优化表](./sample-database-for-in-memory-oltp.md) 在活动内存中有自身的一种表示形式，在磁盘上有辅助副本。  
  
- 磁盘副本用于在关闭然后重新启动服务器或数据库后进行常规恢复。 此内存加磁盘的双重性对你和你的代码处于完全隐藏的状态。  
  
  
#### <a name="natively-compiled-modules"></a>本机编译模块  
  
CREATE PROCEDURE 语句上的 T-SQL 关键字 NATIVE_COMPILATION 是如何创建本地编译的存储过程。 每次数据库联机循环中首次使用本机进程时，会将 T-SQL 语句编译为机器代码。 T-SQL 指令不再接受对每个指令的慢速解释。  
  
- 我们看到本机编译结果所需的持续时间是解释持续时间的 1/100。  
  
  
本机模块只能引用内存优化表，而不能引用基于磁盘的表。  
  
有三种类型的本机编译模块：  
  
- [本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)。  
- 本机编译的用户定义函数 (UDF)，它们是标量。  
- 本机编译的触发器。  
  
  
#### <a name="availability-in-azure-sql-database"></a>Azure SQL 数据库中的可用性  
  
内存中 OLTP 和列存储在 Azure SQL 数据库中可用。 有关详细信息，请参阅 [Optimize Performance using In-Memory Technologies in SQL Database](/azure/sql-database/sql-database-in-memory)（在 SQL 数据库中使用内存中技术优化性能）。
  
  
<a name="ensure-compatibility-level-gteq-130-99c"></a>  
  
## <a name="1-ensure-compatibility-level--130"></a>1.确保兼容级别 >= 130  
  
  
本部分一开始有编号的几节说明可用于实现内存中 OLTP 功能的 Transact-SQL 语法。  
  
  
首先，应将数据库的兼容级别至少设置为 130。 下面是 T-SQL 代码，用于查看当前数据库所设置的当前兼容级别。  
  
```sql
SELECT d.compatibility_level
    FROM sys.databases as d
    WHERE d.name = Db_Name();
```
  
  
  
下面是用于更新级别的 T-SQL 代码（如有必要）。  
  
  
  
```sql
ALTER DATABASE CURRENT
    SET COMPATIBILITY_LEVEL = 130;
```
  
  
  
<a name="elevate-to-snapshot-26n"></a>  
  
## <a name="2-elevate-to-snapshot"></a>2.提升为快照  
  
  
当事务同时涉及基于磁盘的表和内存优化表时，我们将该事务称为 *跨容器事务*。 在此类事务中，事务的内存优化部分必须以名为“快照”的事务隔离级别运行。  
  
若要可靠地对跨容器事务中的内存优化表强制执行此级别，请通过执行以下 T-SQL 来[变更数据库设置](../../t-sql/statements/alter-database-transact-sql-set-options.md)。  
  
  
  
```sql
ALTER DATABASE CURRENT
    SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```
  
  
  
<a name="create-an-optimized-filegroup-24r"></a>  
  
## <a name="3-create-an-optimized-filegroup"></a>3.创建优化文件组  
  
  
在 Microsoft SQL Server 中，你必须先创建声明 CONTAINS MEMORY_OPTIMIZED_DATA 的文件组，然后才能创建内存优化表。 将该文件组分配给你的数据库。 有关详细信息，请参阅：  
  
- [内存优化的文件组](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md)  
  
  
在 Azure SQL 数据库中，无需且不能创建此类文件组。  

下面的示例 T-SQL 脚本可为内存中 OLTP 启用数据库，并配置所有推荐设置。 它适用于 SQL Server 和 Azure SQL 数据库：[enable-in-memory-oltp.sql](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/enable-in-memory-oltp.sql)。

请注意，具有 MEMORY_OPTIMIZED_DATA 文件组的数据库并非支持所有的 SQL Server 功能。 有关限制的详细信息，请参阅：[内存中 OLTP 不支持的 SQL Server 功能](unsupported-sql-server-features-for-in-memory-oltp.md)
  
<a name="create-a-memory-optimized-table-26y"></a>  
  
## <a name="4-create-a-memory-optimized-table"></a>4.创建内存优化表  
  
重要的 Transact-SQL 关键字为关键字 MEMORY_OPTIMIZED。  
  
  
  
```sql
CREATE TABLE dbo.SalesOrder
    (
        SalesOrderId   integer   not null   IDENTITY
            PRIMARY KEY NONCLUSTERED,
        CustomerId   integer    not null,
        OrderDate    datetime   not null
    )
        WITH
            (MEMORY_OPTIMIZED = ON,
            DURABILITY = SCHEMA_AND_DATA);
```
  
  
  
针对内存优化表的 Transact-SQL INSERT 和 SELECT 语句与针对常规表的这些语句相同。  
  
#### <a name="alter-table-for-memory-optimized-tables"></a>针对内存优化表的 ALTER TABLE  
  
ALTER TABLE...ADD/DROP 可以在内存优化表中添加或删除列或索引。  
  
- 不能对内存优化表运行 CREATE INDEX 和 DROP INDEX，但可使用 ALTER TABLE...ADD/DROP INDEX。  
- 有关详细信息，请参阅 [变更内存优化表](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)。  
  
  
#### <a name="plan-your-memory-optimized-tables-and-indexes"></a>规划内存优化表和索引  
  
  
- [内存优化表的索引](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)  
- [内存中 OLTP 不支持的 Transact-SQL 构造](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)  
  
  
  
<a name="create-a-natively-compiled-stored-procedure-native-proc-29u"></a>  
  
## <a name="5-create-a-natively-compiled-stored-procedure-native-proc"></a>5.创建本机编译的存储过程（本机过程）  
  
  
重要的关键字为 NATIVE_COMPILATION。  
  
  
  
```sql
CREATE PROCEDURE ncspRetrieveLatestSalesOrderIdForCustomerId  
        @_CustomerId   INT  
        WITH  
            NATIVE_COMPILATION,  
            SCHEMABINDING  
    AS  
    BEGIN ATOMIC  
        WITH  
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'us_english')  
      
        DECLARE @SalesOrderId int, @OrderDate datetime;
      
        SELECT TOP 1  
                @SalesOrderId = s.SalesOrderId,  
                @OrderDate    = s.OrderDate  
            FROM dbo.SalesOrder AS s  
            WHERE s.CustomerId = @_CustomerId  
            ORDER BY s.OrderDate DESC;  
      
        RETURN @SalesOrderId;  
    END;  
```
  
  
  
关键字 SCHEMABINDING 表示，除非先删除本机过程，否则不能删除本机过程中引用的表。 有关详细信息，请参阅[创建本机编译的存储过程](../../relational-databases/in-memory-oltp/creating-natively-compiled-stored-procedures.md)。  
  
请注意，无需创建本机编译存储过程即可访问内存优化表。 也可从传统的存储过程和即席批处理引用内存优化表。
  
<a name="execute-the-native-proc-31e"></a>  
  
## <a name="6-execute-the-native-proc"></a>6.执行本机过程  
  
  
使用两行数据填充表。  
  
  
  
```sql
INSERT into dbo.SalesOrder  
        ( CustomerId, OrderDate )  
    VALUES  
        ( 42, '2013-01-13 03:35:59' ),
        ( 42, '2015-01-15 15:35:59' );
```
  
  
  
然后，对本机编译的存储过程调用 EXECUTE。  
  
  
  
```sql
DECLARE @LatestSalesOrderId int, @mesg nvarchar(128);
      
EXECUTE @LatestSalesOrderId =  
    ncspRetrieveLatestSalesOrderIdForCustomerId 42;
      
SET @mesg = CONCAT(@LatestSalesOrderId,  
    ' = Latest SalesOrderId, for CustomerId = ', 42);
PRINT @mesg;  
```
      
以下实际的 PRINT 输出：  
```output
-- 2 = Latest SalesOrderId, for CustomerId = 42  
```
  
  
  
  
<a name="guide-to-the-documentation-and-next-steps-32w"></a>  
  
### <a name="guide-to-the-documentation-and-next-steps"></a>文档指南和后续步骤  
  
  
前面的简单示例为你学习内存中 OLTP 的更高级功能打下基础。 以下各节是可能需要知道的特殊注意事项，以及在哪里可以查看每个注意事项的相关详细信息的指南。  
  
  
  
<a name="how-do-in-memory-oltp-features-work-so-much-faster-33v"></a>  
  
## <a name="how-in-memory-oltp-features-work-so-much-faster"></a>内存中 OLTP 功能如何执行得这样快  
  
  
以下各小节简要介绍内存中 OLTP 功能如何在内部工作以提高性能。  
  
  
<a name="how-do-memory-optimized-tables-perform-faster-34q"></a>  
  
### <a name="how-memory-optimized-tables-perform-faster"></a>内存优化表如何执行速度更快  
  
  
**双重特性：** 内存优化表具有双重特性：在活动内存中是一种表示形式，在硬盘上是另一种表示形式。 每个事务将提交到该表的这两种表示形式。 事务针对更快的活动内存表示形式运行。 内存优化表从活动内存比磁盘速度更快受益。 此外，活动内存的更大灵活性使得实现针对速度进行优化的更高级表结构切实可行。 高级结构也是无页的，因此它可以避免闩锁和自旋锁的开销和争用。  
  
  
**无锁：** 内存优化表依赖于乐观方法来实现数据完整性与并发性和高吞吐量这两个对立的目标。 在事务处理期间，该表不在任何版本的已更新数据行上放置锁。 在某些大容量系统中，这可以大大减少争用。  
  
  
**行版本：** 内存优化表不使用锁，而是在表本身中（不是在 tempdb 中）添加新版本的已更新行。 原始行将一直保留，直到提交事务之后。 在事务处理期间，其他进程可以读取行的原始版本。  
  
- 为基于磁盘的表创建某一行的多个版本时，行版本将暂时存储在 tempdb 中。  
  
  
**更少日志记录：** 已更新行的之前和之后版本将保存在内存优化表中。 行对提供了很多通常写入到日志文件的信息。 这使得系统将更少信息更少次数地写入到日志。 还确保事务完整性。  
  
  
<a name="how-do-native-procs-perform-faster-35x"></a>  
  
### <a name="how-native-procs-perform-faster"></a>本机过程如何执行速度更快  
  
将常规解释型存储过程转换为本机编译的存储过程可大大减少要在运行时执行的指令数。  
  
  
<a name="trade-offs-of-in-memory-features-36j"></a>  
  
## <a name="trade-offs-of-in-memory-features"></a>内存中功能的权衡  
  
  
正如在计算机科学中所常见的，内存中功能提供的性能提升是一种折中方案。 相比功能的额外成本，更好的功能带来的好处要更有价值得多。 可在此处找到关于权衡的全面指南：

- [在 SQL Server 中计划内存中 OLTP 功能的应用](../../relational-databases/in-memory-oltp/plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)

本部分的剩余部分列出了一些主要规划和权衡注意事项。
  
<a name="trade-offs-of-memory-optimized-tables-37d"></a>  
  
### <a name="trade-offs-of-memory-optimized-tables"></a>内存优化表的权衡  
  
  
**估计内存：** 必须估计内存优化表要使用的活动内存量。 计算机系统必须具有足够的内存容量，才能托管内存优化表。 有关详细信息，请参阅：  
  
- [内存使用情况的监视和故障排除](../../relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage.md)  
- [估算内存优化表的内存需求](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md)  
- [内存优化表中的表和行大小](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md)  
  
  
**对大型表进行分区：** 满足大量活动内存需求的一种方法是将大型表划分为几部分，内存中的部分存储热度高的最近数据行，磁盘上的其他部分存储冷旧数据行（如已完全发货和已完成的销售订单） 。 此分区是一个需要设计和实现的手动过程。 请参阅：  
  
- [应用程序级分区](../../relational-databases/in-memory-oltp/application-level-partitioning.md)  
- [用于对内存优化表进行分区的应用程序模式](../../relational-databases/in-memory-oltp/application-pattern-for-partitioning-memory-optimized-tables.md)  
  
<a name="trade-offs-of-native-procs-38p"></a>  
  
### <a name="trade-offs-of-native-procs"></a>本机过程的权衡  
  
- 本机编译的存储过程不能访问基于磁盘的表。 本机过程只能访问内存优化表。  
- 如果本机过程在服务器或数据库最近一次重新联机后第一次运行，则本机过程必须重新编译一次。 这会导致在本机过程开始运行前出现延迟。  
  
<a name="advanced-considerations-for-memory-optimized-tables-39n"></a>  
  
## <a name="advanced-considerations-for-memory-optimized-tables"></a>内存优化表的高级注意事项  
  
[内存优化表的索引](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md) 在某些方面与传统的磁盘上表中的索引有所不同。 仅内存优化表可使用哈希索引。
    
- [内存优化表的哈希索引](../../relational-databases/sql-server-index-design-guide.md#hash_index)
- [用于内存优化表的非聚集索引](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index) 
  
你必须计划以确保有足够的活动内存可用于计划的内存优化表及其索引。 请参阅：  
  
- [创建和管理用于内存优化对象的存储](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
可使用 DURABILITY = SCHEMA_ONLY 声明内存优化表：  
  
- 此语法指示系统，在数据库已脱机时，丢弃内存优化表中的所有数据。 仅保留表定义。  
- 当数据库重新联机时，将内存优化表重新加载到活动内存中，数据为空。  
- 当涉及到成千上万个行时，SCHEMA_ONLY 表可作为 tempdb 中 [#temporary 表](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md) 的上级替代项。  
  
也可以将表变量声明为内存优化变量。 请参阅：  
  
- [通过使用内存优化获得更快的临时表和表变量](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md)  
  
<a name="advanced-considerations-for-natively-compiled-modules-40k"></a>  
  
## <a name="advanced-considerations-for-natively-compiled-modules"></a>本机编译模块的高级注意事项  
  
通过 Transact-SQL 提供的本机编译模块的类型包括：  
  
- 本机编译的存储过程（本机过程）。  
- 本机编译的 [标量用户定义函数](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)。  
- 本机编译的触发器（本机触发器）。  
  - 只允许对内存优化表使用本机编译的触发器。  
- 本机编译的 [表值函数](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)。  
  - [Improving temp table and table variable performance using memory optimization](/archive/blogs/sqlserverstorageengine/improving-temp-table-and-table-variable-performance-using-memory-optimization)  
  
本机编译的用户定义函数 (UDF) 比解释型 UDF 运行速度更快。 下面是一些关于 UDF 的注意事项：  
  
- 当 T-SQL SELECT 使用 UDF 时，始终会在返回每行后调用 UDF。  
  - UDF 从不以内联方式运行，而是始终被调用。  
  - 编译的区别与所有 UDF 固有的重复调用的开销相比，并没有那么重要。  
  - 尽管如此，UDF 调用的开销在实用级别上通常是可接受的。  
  
有关本机 UDF 性能的测试数据和说明，请参阅：  
  
  - [Soften the RBAR impact with Native Compiled UDFs in SQL Server 2016](/archive/blogs/sqlcat/soften-the-rbar-impact-with-native-compiled-udfs-in-sql-server-2016)  
  - [本机编译的用户定义函数](https://sqlinthewild.co.za/index.php/2016/01/12/natively-compiled-user-defined-functions/)博客文章，作者 Gail Shaw，发布日期 2016 年 1 月。  
  
<a name="documentation-guide-for-memory-optimized-tables-41z"></a>  
  
## <a name="documentation-guide-for-memory-optimized-tables"></a>内存优化表的文档指南  
  
请参阅讨论内存优化表的特殊注意事项的其他文章：  
  
- [迁移到内存中 OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  - [确定表或存储过程是否应移植到内存中 OLTP](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md)  
  - 通过 SQL Server Management Studio 中的事务性能分析报告，可帮助你评估内存中 OLTP 是否将改进数据库应用程序的性能。  
  - 使用 [内存优化顾问](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md) 可帮助你将基于磁盘的数据库表迁移到内存中 OLTP。   
- [内存优化表的备份、还原和恢复](/previous-versions/sql/sql-server-2016/dn624160(v=sql.130))  
  - 内存优化表使用的存储可以比其在内存中的大小大得多，会影响数据库备份的大小。  
- [具有内存优化表的事务](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)  
  - 对于内存优化表上的事务，在 T-SQL 中包括有关重试逻辑的信息。  
- [对内存中 OLTP 的 Transact-SQL 支持](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)  
  - 内存优化表和本机过程支持和不支持的 T-SQL 和数据类型。  
- [将具有内存优化表的数据库绑定至资源池](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md)，它讨论可选的高级注意事项。  

<a name="documentation-guide-for-native-procs-42b"></a>  
  
## <a name="documentation-guide-for-native-procs"></a>本机过程的文档指南  

以下文章及其在目录 (TOC) 中的子文章详细介绍了本机编译的存储过程。

- [本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)
  
<a name="related-links-43f"></a>  
  
## <a name="related-links"></a>相关链接  
  
- 基础知识：[内存中 OLTP（内存中优化）](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
    
以下文章介绍了某些代码，演示了通过使用内存中 OLTP 可实现的性能提升：  
  
- [演示：内存中 OLTP 的性能改善](../../relational-databases/in-memory-oltp/demonstration-performance-improvement-of-in-memory-oltp.md)通过小规模演示来演示较大的潜在性能提升。  
- [内存中 OLTP 的示例数据库](../../relational-databases/in-memory-oltp/sample-database-for-in-memory-oltp.md)提供了更大规模的演示。