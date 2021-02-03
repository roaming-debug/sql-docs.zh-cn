---
description: 开始使用列存储进行实时运营分析
title: 开始使用列存储进行实时运营分析 | Microsoft Docs
ms.custom: ''
ms.date: 03/08/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: quickstart
ms.assetid: e1328615-6b59-4473-8a8d-4f360f73187d
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dbc0867b71cadb33b1eb9174132b2031134a72f0
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236942"
---
# <a name="get-started-with-columnstore-for-real-time-operational-analytics"></a>开始使用列存储进行实时运营分析
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 引入了实时运营分析，它可同时对相同的数据库表运行分析和 OLTP 工作负载。 除了实时运行分析以外，你还可以消除对 ETL 和数据仓库的需求。  
  
## <a name="real-time-operational-analytics-explained"></a>实时运行分析介绍  
 传统上，企业有用于运营工作负荷（即 OLTP）和分析工作负荷的独立系统。 对于此类系统，提取、转换和加载 (ETL) 作业会定期将数据从操作存储转移到分析存储。 分析数据通常存储在专用于运行分析查询的数据仓库或数据市场中。 尽管这种解决方案已成为标准，但在以下三个方面存在很大问题：  
  
-   **复杂性。** 实施 ETL 可能需要编码相当多的代码，尤其是只想要加载修改的行时。 识别哪些行已被修改是一个复杂的过程。  
  
-   **成本。** 实施 ETL 需要付出采购额外硬件和软件许可证的成本。  
  
-   **数据滞后时间。** 实施 ETL 会增大运行分析的时间延迟。 例如，如果 ETL 作业在每个工作日结束时运行，分析查询会针对至少一天的数据运行。 对于许多企业来说，这种延迟不可接受，因为企业依赖于实时分析数据。 例如，欺诈检测需要实时分析操作数据。  
  
 ![实时运行分析概述](../../relational-databases/indexes/media/real-time-operational-analytics-overview.png "实时运行分析概述")  
  
实时运行分析为这些难题提供了解决方案。   
对同一个基础表运行分析和 OLTP 工作负载时不会出现时间延迟。   对于使用实时分析的方案，成本和复杂性将大大降低，因为不需要使用 ETL，并且不需要采购和维护独立的数据仓库。  
  
> [!NOTE]  
>  实时运行分析面向包含单个数据源的方案，例如，可在其上运行操作和分析工作负载的企业资源规划 (ERP) 应用程序。 如果在运行分析工作负载之前需要集成多个源中的数据，或者你要使用预先聚合的数据（如多维数据集）实现极高的分析性能，则它不能取代独立的数据仓库。  
  
 实时分析使用行存储表中的可更新列存储索引。 列存储索引维护数据的副本，因此 OLTP 和分析工作负载可针对数据的独立副本运行。 这可以最大程度地降低对同时运行的两个工作负载的性能影响。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动维护索引更改，因此要分析的 OLTP 更改始终是最新的。 通过这种设计，对最新数据实时运行分析是切实可行的。 这适用于基于磁盘的表和内存优化表。  
  
## <a name="get-started-example"></a>入门示例  
 若要开始使用实时分析，请执行以下操作：  
  
1.  识别操作架构中包含需要用于分析的数据的表。  
  
2.  对于每个表，删除主要用于提高 OLTP 工作负载上现有分析速度的所有 btree 索引。 将这些索引替换为单个列存储索引。  这可以提高 OLTP 工作负载的整体性能，因为可以减少要维护的索引。  
  
    ```sql  
    --This example creates a nonclustered columnstore index on an existing OLTP table.  
    --Create the table  
    CREATE TABLE t_account (  
        accountkey int PRIMARY KEY,  
        accountdescription nvarchar (50),  
        accounttype nvarchar(50),  
        unitsold int  
    );  
  
    --Create the columnstore index with a filtered condition  
    CREATE NONCLUSTERED COLUMNSTORE INDEX account_NCCI   
    ON t_account (accountkey, accountdescription, unitsold)   
    ;  
    ```  
  
     通过集成内存中 OLTP 和内存中列存储技术来提供高性能 OLTP 和分析工作负载，可对内存中表上的列存储索引进行运营分析。 内存中表上的列存储索引必须包括所有列。  
  
    ```sql  
    -- This example creates a memory-optimized table with a columnstore index.  
    CREATE TABLE t_account (  
        accountkey int NOT NULL PRIMARY KEY NONCLUSTERED,  
        Accountdescription nvarchar (50),  
        accounttype nvarchar(50),  
        unitsold int,  
        INDEX t_account_cci CLUSTERED COLUMNSTORE  
        )  
        WITH (MEMORY_OPTIMIZED = ON );  
    GO  
  
    ```  
  
3.  这就是要执行的所有操作！  

 现在，无需对应用程序进行任何更改，就能运行实时运营分析。  分析查询将针对列存储索引运行，OLTP 操作将针对 OLTP btree 索引不断运行。 OLTP 工作负载将继续执行，但维护列存储索引会产生更多的开销。 请参阅下一部分中有关性能优化的信息。  
  
## <a name="blog-posts"></a>博客文章  
 要详细了解实时运营分析，请阅读以下博客文章。 如果你先阅读这些博客文章，则可以更容易理解性能提示部分。  
  
-   [实时运营分析的企业用例](https://blogs.technet.microsoft.com/dataplatforminsider/2015/12/09/real-time-operational-analytics-using-in-memory-technology/)  
  
-   [使用非聚集列存储索引进行实时运营分析](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-using-nonclustered-columnstore-index)  
  
-   [使用非聚集列存储索引的简单示例](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-simple-example-using-nonclustered-clustered-columnstore-index-ncci)  
  
-   [SQL Server 如何维护事务工作负载中的非聚集列存储索引](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-dml-operations-and-nonclustered-columnstore-index-ncci-in-sql-server-2016)  
  
-   [使用筛选索引最大程度地降低对非聚集列存储索引维护的影响](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci)  
  
-   [使用压缩延迟最大程度地减少对非聚集列存储索引维护的影响](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci)  
  
-   [使用压缩延迟 - 性能数据最大程度地减少对非聚集列存储索引维护的影响](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-with-ncci-and-the-performance)  
  
-   [对内存优化表进行实时运营分析](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-memory-optimized-table-and-columnstore-index)  
  
-   [列存储索引和行组的合并策略](/archive/blogs/sqlserverstorageengine/columnstore-index-merge-policy-for-reorganize)  
  
## <a name="performance-tip-1-use-filtered-indexes-to-improve-query-performance"></a>性能提示 1：使用筛选索引来提升查询性能  
 运行实时运营分析可能会影响 OLTP 工作负载的性能。 这种影响应该很小。 下面的示例展示了如何使用筛选的索引来最大限度地降低非聚集列存储索引对事务工作负荷的影响，同时仍能提供实时分析。  
  
 为了尽量减少维护操作工作负载上非聚集列存储索引的开销，你可以使用筛选条件，以便只对 *暖* 数据或缓慢变化的数据创建非聚集列存储索引。 例如，在订单管理应用程序中，可以针对已发货的订单创建非聚集列存储索引。 订单在发货后，就很少会发生变化，因此被视为暖数据。 使用筛选索引时，非聚集列存储索引中的数据只需少量的更新，因此可降低对事务工作负载的影响。  
  
 分析查询将根据需要以透明方式访问暖数据和热数据，以提供实时分析。 如果操作工作负载的重要部分处理热数据，这些操作不需要额外维护列存储索引。 最佳做法是对筛选索引定义中使用的列使用行存储聚集索引。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用聚集索引来快速扫描不符合筛选条件的行。 如果不使用此聚集索引，必须对行存储表进行全表扫描，才能找到这些行，这可能会对分析查询的性能产生显著的负面影响。 如果不使用聚集索引，你可以创建一个互补筛选的非聚集 btree 索引来标识这些行，但我们不建议这样做，因为通过非聚集 btree 索引访问大量的行会造成很大的开销。  
  
> [!NOTE]  
>  只有基于磁盘的表才支持筛选的非聚集列存储索引。 内存优化表不支持此类索引  
  
### <a name="example-a-access-hot-data-from-btree-index-warm-data-from-columnstore-index"></a>示例 A：从 B 树索引访问热数据，从列存储索引访问温数据  
 此示例使用筛选条件 (accountkey > 0) 来确定要将哪些行放在列存储索引中。 目的是设计筛选条件和后续查询，以便从 btree 索引访问经常变化的“热”数据，从列存储索引访问更稳定的“暖”数据。  
  
 ![暖数据和热数据的组合索引](../../relational-databases/indexes/media/de-columnstore-warmhotdata.png "暖数据和热数据的组合索引")  
  
> [!NOTE]  
>  查询优化器会考虑但不是总是选择将列存储索引用于查询计划。 当查询优化器选择筛选的列存储索引时，将以透明方式合并来自列存储索引的行以及不符合筛选条件的行，以便能够进行实时分析。 这不同于常规的非聚集筛选索引，后者只能在将自身限制为索引中存在的行的查询中使用。  
  
```sql  
--Use a filtered condition to separate hot data in a rowstore table  
-- from "warm" data in a columnstore index.  
  
-- create the table  
CREATE TABLE  orders (  
         AccountKey         int not null,  
         Customername       nvarchar (50),  
        OrderNumber         bigint,  
        PurchasePrice       decimal (9,2),  
        OrderStatus         smallint not null,  
        OrderStatusDesc     nvarchar (50))  
  
-- OrderStatusDesc  
-- 0 => 'Order Started'  
-- 1 => 'Order Closed'  
-- 2 => 'Order Paid'  
-- 3 => 'Order Fullfillment Wait'  
-- 4 => 'Order Shipped'  
-- 5 => 'Order Received'  
  
CREATE CLUSTERED INDEX  orders_ci ON orders(OrderStatus)  
  
--Create the columnstore index with a filtered condition  
CREATE NONCLUSTERED COLUMNSTORE INDEX orders_ncci ON orders  (accountkey, customername, purchaseprice, orderstatus)  
where orderstatus = 5  
;  
  
-- The following query returns the total purchase done by customers for items > $100 .00  
-- This query will pick  rows both from NCCI and from 'hot' rows that are not part of NCCI  
SELECT top 5 customername, sum (PurchasePrice)  
FROM orders  
WHERE purchaseprice > 100.0   
Group By customername  
```  
  
 分析查询将配合以下查询计划执行。 你可以看到，不符合筛选条件的行是通过聚集 btree 索引访问的。  
  
 ![查询计划](../../relational-databases/indexes/media/query-plan-columnstore.png "查询计划")  
  
 有关 [筛选的聚集列存储索引](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci)的详细信息，请参阅博客  
  
## <a name="performance-tip-2-offload-analytics-to-always-on-readable-secondary"></a>性能提示 2：将分析负载转移到 Always On 可读次要副本  
 尽管你可使用已筛选的列存储索引来尽量减少列存储索引维护，但分析查询可能仍需要大量计算资源（CPU、I/O、内存），这会影响操作工作负载的性能。 对于大多数任务关键型工作负载，我们建议使用 AlwaysOn 配置。 在这种配置，你可以通过将运行中的分析负载转移到可读辅助副本来消除影响。  
  
## <a name="performance-tip-3-reducing-index-fragmentation-by-keeping-hot-data-in-delta-rowgroups"></a>性能提示 3：通过在增量行组中保存热数据来减少索引碎片  
 如果工作负荷更新/删除了已压缩的行，包含列存储索引的表可能会出现大量碎片（即已删除的行）。 有碎片的列存储索引会导致内存/存储利用效率下降。 除了资源的低效利用以外，还会对分析查询性能造成负面影响，因为需要额外的 I/O，并且需要从结果集中筛选出已删除的行。  
  
 在使用 `REORGANIZE` 命令运行索引碎片整理，或者在整个表或受影响的分区上重新生成列存储索引之前，已删除的行实际上并未删除。 索引 `REORGANIZE` 和 `REBUILD` 操作开销很高，会占用本应提供给工作负载的资源。 此外，如果过早压缩行，可能会由于更新而需要重新压缩多次，从而导致压缩开销的浪费。  
可使用 `COMPRESSION_DELAY` 选项来尽量减少索引碎片。  
  
```sql  
-- Create a sample table  
CREATE TABLE t_colstor (  
               accountkey                      int not null,  
               accountdescription              nvarchar (50) not null,  
               accounttype                     nvarchar(50),  
               accountCodeAlternatekey         int)  
  
-- Creating nonclustered columnstore index with COMPRESSION_DELAY. The columnstore index will keep the rows in closed delta rowgroup for 100 minutes   
-- after it has been marked closed  
CREATE NONCLUSTERED COLUMNSTORE index t_colstor_cci on t_colstor (accountkey, accountdescription, accounttype)   
                       WITH (DATA_COMPRESSION= COLUMNSTORE, COMPRESSION_DELAY = 100);  
  
;  
```  
  
 要详细了解[压缩延迟](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci)，请参阅相关博客。  
  
 下面是建议的最佳做法：  
  
-   **插入/查询工作负载：** 如果工作负载主要是插入和查询数据，则建议将 COMPRESSION_DELAY 的默认值设置为 0。 在单个增量行组中插入 100 万行后，新插入的行将被压缩。  
    此类工作负荷的一些示例包括：(a) 传统 DW 工作负荷；(b) 在需要分析 Web 应用程序中的单击模式时执行的单击流分析。  
  
-   **OLTP 工作负载：** 如果工作负荷需要执行大量 DML（即大量混合的更新、删除和插入操作），可以通过检查 DMV sys. dm_db_column_store_row_group_physical_stats 来查看列存储索引碎片。 如果你看到在最近压缩的行组中，10% 以上的行标记为已删除，则可以使用 COMPRESSION_DELAY 选项来增加时间延迟，达到该延迟后，行可供压缩。 例如，对于工作负荷，如果新插入的数据保持“热”状态（即多次更新）60 分钟，应将 COMPRESSION_DELAY 指定为 60。  
  
 大多数客户应该不需要采取任何措施。 COMPRESSION_DELAY 选项的默认值应可满足需要。  
对于高级用户，我们建议运行以下查询并收集过去 7 天已删除的行的百分比。  
  
```sql  
SELECT row_group_id,cast(deleted_rows as float)/cast(total_rows as float)*100 as [% fragmented], created_time  
FROM sys. dm_db_column_store_row_group_physical_stats  
WHERE object_id = object_id('FactOnlineSales2')   
             AND  state_desc='COMPRESSED'   
             AND deleted_rows>0   
             AND created_time > GETDATE() - 7  
ORDER BY created_time DESC;  
```  
  
 如果压缩行组中已删除的行数超过 20%，则平整变化率小于 5% 的旧行组（称为冷行组）会将 COMPRESSION_DELAY 设置为 (youngest_rowgroup_created_time - current_time)。 请注意，这种方法最适合用于稳定且性质相对统一的工作负载。  
  
## <a name="see-also"></a>另请参阅  
 [列存储索引指南](../../relational-databases/indexes/columnstore-indexes-overview.md)   
 [列存储索引数据加载](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Columnstore Indexes Query Performance](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [针对数据仓库的列存储索引](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)
