---
description: 列存储索引 - 设计指南
title: 列存储索引 - 设计指南 | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: fc3e22c2-3165-4ac9-87e3-bf27219c820f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 72ab05dfce314119c30e08428fdcaa2b94ba25ed
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171769"
---
# <a name="columnstore-indexes---design-guidance"></a>列存储索引 - 设计指南
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

有关设计列存储索引的概要建议。 做出少量明智的决策，就能帮助实现较高的数据压缩率和查询性能，列存储索引的目标就在于此。 

## <a name="prerequisites"></a>先决条件

本文假设读者熟悉列存储的体系结构和术语。 有关详细信息，请参阅[列存储索引 - 概述](../../relational-databases/indexes/columnstore-indexes-overview.md)和[列存储索引体系结构](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)。

### <a name="know-your-data-requirements"></a>了解数据要求
在设计列存储索引之前，应该尽可能充分地了解数据要求。 例如，仔细考虑如下所述问题的答案：

- 我的表有多大？
- 查询主要执行的分析是否要扫描大范围的值？  列存储索引的设计目的是合理处理大范围扫描，而不是查找特定的值。
- 工作负荷是否要执行大量的更新和删除操作？ 当数据稳定时，列存储索引可以正常工作。 查询应该更新和删除少于 10% 的行。
- 是否拥有数据仓库的事实表和维度表？
- 是否需要针对事务工作负荷执行分析？ 如果是，请参阅有关实时运营分析的列存储设计指南。

你可能不需要列存储索引。 包含堆或聚集索引的行存储表最适合用于查找数据、搜索特定值的查询，或者针对较小范围的值执行查询。 可对事务工作负载使用行存储索引，因为这些工作负载往往需要进行表查找而不是大范围表扫描。  

## <a name="choose-the-best-columnstore-index-for-your-needs"></a>根据需要选择最佳的列存储索引

列存储索引是聚集或非聚集索引。  聚集列存储索引可以有一个或多个非聚集 B 树索引。 试用列存储索引的过程非常简单。 如果将某个表创建为列存储索引，可以轻松将该表转换回行存储表，只需删除列存储索引即可。 

下面是选项和建议的摘要。 

| 列存储选项 | 使用时机的建议 | 压缩 |
| :----------------- | :------------------- | :---------- |
| 聚集列存储索引 | 用于：<br></br>1) 采用星型或雪花型架构的传统数据仓库工作负荷<br></br>2) 在执行极少量更新和删除操作的条件下插入大量数据的物联网 (IOT) 工作负荷。 | 10 倍平均值 |
| 基于聚集列存储索引的非聚集 B 树索引 | 用于：<br></br>    1.针对聚集列存储索引实施主键和外键约束。<br></br>    2.加速搜索特定值或小范围值的查询。<br></br>    3.加速特定行的更新和删除。| 10 倍平均值加上 NCI 的某些附加存储。|
| 以基于磁盘的堆或 B 树索引为基础的非聚集列存储索引 | 用于： <br></br>1) 包含某些分析查询的 OLTP 工作负荷。 可以删除针对分析创建的 B 树索引，然后将其替换为一个非聚集列存储索引。<br></br>2) 许多执行提取、转换和加载 (ETL) 操作，以便将数据移到单独数据仓库的传统 OLTP 工作负荷。 可以通过在某些 OLTP 表中创建非聚集列存储索引来消除 ETL 和单独的数据仓库。 | NCCI 是一个附加的索引，所需的存储平均要多出 10%。|
| 基于内存中表的列存储索引 | 与针对以基于磁盘的表为基础的非聚集列存储索引提供的建议相同，不过，基础表是一个内存中表。 | 列存储索引是一个附加索引。|

## <a name="use-a-clustered-columnstore-index-for-large-data-warehouse-tables"></a>为大型数据仓库表使用聚集列存储索引
聚集列存储索引不仅仅是一个索引，而且是主表存储。 它可以实现较高的数据压缩率，大幅提高大型数据仓库事实表和维度表的查询性能。 聚集列存储索引最适合用于分析查询而不是事务查询，因为分析查询往往针对大范围值执行操作，而不是查找特定的值。 

对于以下情况，请考虑使用聚集列存储索引：

- 每个分区至少包含 100 万行。 列存储索引包含每个分区中的行组。 如果表太小，无法填充每个分区中的行组，则你无法获得列存储压缩和查询性能的优势。
- 查询主要针对值范围执行分析。 例如，若要查找某个列的平均值，查询需要扫描所有列值。 然后，将值求和以聚合这些值，以此得出平均值。
- 大多数插入操作是针对大量数据执行的，造成的更新和删除量极少。 许多工作负荷（例如物联网 (IOT)）在执行极少量更新和删除操作的条件下插入大量数据。 使用聚集列存储索引可让这些工作负荷受益于压缩与查询性能的提升。

对于以下情况，请不要使用聚集列存储索引：

* 表需要 varchar(max)、nvarchar(max) 或 varbinary(max) 数据类型。 或者，将列存储索引设计为不包含这些列。
* 表数据不是持久性的。 需要快速存储和删除数据时，请考虑使用堆或临时表。
* 表的每个分区包含的行数不超过 100 万。 
* 在针对表执行的操作中，10% 以上是更新和删除。 大量的更新和删除会导致碎片。 碎片会影响压缩率和查询性能，然后，你需要运行一个称为“重新整理”的操作，强制将数据移入列存储并删除碎片。 有关详细信息，请参阅[最小化列存储索引中的索引碎片](/archive/blogs/sqlserverstorageengine/columnstore-index-defragmentation-using-reorganize-command)。

有关详细信息，请参阅[列存储索引 - 数据仓库](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)。

## <a name="add-b-tree-nonclustered-indexes-for-efficient-table-seeks"></a>添加 B 树非聚集索引以提高表查找的效率

从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始，可以创建非聚集 B 树索引作为聚集列存储索引中的辅助索引。 当列存储索引发生更改时，非聚集 B 树索引会更新。 这是一个可以带来优势的强大功能。 

使用辅助 B 树索引可以有效搜索特定的行，而无需全面扫描所有行。  其他选项也可供使用。 例如，可以通过在 B 树索引中使用唯一约束来实施主键或外键约束。 由于非唯一值无法插入 B 树索引中，因此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法将值插入列存储。 

对于以下情况，请考虑使用基于列存储索引的 B 树索引：
* 运行搜索特定值或小范围值的查询。
* 实施主键约束或外键约束等约束。
* 有效执行更新和删除操作。 使用 B 树索引可以快速找到需要更新和删除的特定行，而无需扫描整个表或表分区。
* 可以获得额外的存储用于存储 B 树索引。

## <a name="use-a-nonclustered-columnstore-index-for-real-time-analytics"></a>使用非聚集列存储索引进行实时分析

从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始，可以在基于磁盘的行存储表或内存中 OLTP 表中使用非聚集列存储索引。 这样，便可以针对事务表实时运行分析。 尽管事务是在基础表中发生的，但你可以针对列存储索引运行分析。 由于一个表可以管理两种索引，因此，行存储索引和列存储索引都会实时更改。

由于列存储索引实现的数据压缩率比行存储索引高出 10 倍，因此只需少量的额外存储。 例如，如果压缩的行存储表占用 20 GB，列存储索引可能只需要额外的 2 GB 空间。 所需的额外空间还取决于非聚集列存储索引中的列数。 

 对于以下情况，请考虑使用非聚集列存储索引：

* 针对事务行存储表实时运行分析。 可将设计用于分析的现有 B 树索引替换为非聚集列存储索引。 
  
*   不再需要单独的数据仓库。 在传统上，公司会在行存储表中运行事务，然后将数据载入单独的数据仓库以运行分析。 对于许多工作负载，可以通过在事务表中创建非聚集列存储索引，来消除加载过程和单独的数据仓库。

  [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 提供多种策略来保持这种方案的高性能。 试用该方案的过程非常简单，因为无需更改 OLTP 应用程序即可启用非聚集列存储索引。 

若要添加更多的处理资源，可以针对可读的辅助副本运行分析。 使用可读的辅助副本可将事务工作负荷与分析工作负荷的处理分隔开来。 

有关详细信息，请参阅[开始使用列存储索引进行实时运营分析](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)

有关选择最佳列存储索引的详细信息，请参阅 Sunil Agarwal 的博客 [Which columnstore index is right for my workload?](/archive/blogs/sql_server_team/columnstore-index-which-columnstore-index-is-right-for-my-workload)（哪种列存储索引适合我的工作负荷？）。

## <a name="use-table-partitions-for-data-management-and-query-performance"></a>使用表分区来管理数据和提高查询性能
列存储索引支持分区，这是管理和存档数据的一种良好方式。 分区还可将操作局限于一个或多个分区，从而提高查询性能。

### <a name="use-partitions-to-make-the-data-easier-to-manage"></a>使用分区来简化数据管理
对于大型表，管理数据范围的唯一可行办法就是使用分区。 行存储表的分区优势同样适用于列存储索引。 

例如，行存储表和列存储表都可使用分区来实现以下目的：

- 控制增量备份的大小。 可以将分区备份到单独的文件组，然后将它们标记为只读。 这样，以后的备份将跳过只读的文件组。 
- 通过将旧分区转移到更经济的存储来节省存储成本。 例如，可以使用分区切换将分区转移到更经济的存储位置。
- 通过限制针对分区的操作来有效执行操作。 例如，可将有碎片的分区专门用于索引维护。

此外，对于列存储索引，可以使用分区实现以下目的：

* 将存储成本进一步节省 30%。 可以使用 COLUMNSTORE_ARCHIVE 压缩选项来压缩旧分区。 数据查询性能将会下降，但如果查询分区的频率不高，这种下降是可接受的。

### <a name="use-partitions-to-improve-query-performance"></a>使用分区提高查询性能

使用分区可将查询限制为仅扫描特定的分区，从而限制扫描的行数。 例如，如果索引已按年份分区，而查询要分析上一年的数据，则它只需扫描一个分区中的数据。 

### <a name="use-fewer-partitions-for-a-columnstore-index"></a>对列存储索引使用更少的分区

除非数据相当大，否则，可以为列存储索引使用比行存储索引所用更少的分区以实现最佳性能。 如果每个分区包含的行数不超过 100 万，大多数行可能会转到增量存储，在其中，这些行不会获得列存储压缩的性能优势。 例如，如果将 100 万行载入包含 10 个分区的表，每个分区包含 10 万行，则所有行将转到增量行组。 

示例：
* 将 100 万行载入一个分区或非分区表。 获取一个包含 100 万行的压缩行组。 这非常适合用于提高数据压缩率和查询性能。
* 将 100 万行均匀载入 10 个分区。 每个分区包含 10 万行，低于列存储压缩的最小阈值。 因此，列存储索引可以包含 10 个增量行组，每个行组包含 10 万行。 可通过某些方法强制将增量行组载入列存储。 不过，如果这些是列存储索引中的唯一行，压缩行组就会太小，无法实现最佳压缩和查询性能。

有关分区的详细信息，请参阅 Sunil Agarwal 的博客文章 [Should I partition my columnstore index?](/archive/blogs/sqlserverstorageengine/columnstore-index-should-i-partition-my-columnstore-index)（我是否应该将列存储索引分区？）。

## <a name="choose-the-appropriate-data-compression-method"></a>选择适当的数据压缩方法
列存储索引为数据压缩提供两个选项：列存储压缩和存档压缩。 可在创建索引时选择压缩选项，以后可以使用 [ALTER INDEX...REBUILD](../../t-sql/statements/alter-index-transact-sql.md) 来更改选项。

### <a name="use-columnstore-compression-for-best-query-performance"></a>使用列存储压缩实现最佳查询性能
列存储压缩实现的压缩率通常比行存储索引要高出 10 倍。 它是列存储索引的标准压缩方法，可提高查询性能。 

### <a name="use-archive-compression-for-best-data-compression"></a>使用存档压缩实现最佳数据压缩
如果查询性能不是那么重要，可以使用存档压缩来最大程度地提高压缩率。 它的数据压缩率比列存储压缩更高，但同时会造成一个弊端。 它在压缩和解压缩数据时花费的时间更长，因此不是很适合用于提高查询性能。 

## <a name="use-optimizations-when-you-convert-a-rowstore-table-to-a-columnstore-index"></a>将行存储表转换为列存储索引时使用优化

如果数据已在行存储表中，你可以使用 [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md) 将该表转换为聚集列存储索引。 转换表后，可以使用多种优化方法来提高查询性能，如下文所述。

### <a name="use-maxdop-to-improve-rowgroup-quality"></a>使用 MAXDOP 提高行组质量
可以配置最大数目的处理器，用于将堆或聚集 B 树索引转换为列存储索引。 若要配置处理器，请使用最大并行度选项 (MAXDOP)。 

若要处理大量的数据，MAXDOP 1 的速度可能太慢。  将 MAXDOP 提高到 4 即可正常工作。 如果这样做导致某些行组不包含最佳行数，可以运行 [ALTER INDEX REORGANIZE](../../t-sql/statements/alter-index-transact-sql.md)，在后台将这些行合并在一起。

### <a name="keep-the-sorted-order-of-a-b-tree-index"></a>保留 B 树索引的排序顺序
由于 B 树索引已按排序顺序存储行，将行压缩到列存储索引中时保留这种顺序可以提高查询性能。

列存储索引不会将数据排序，但会使用元数据来跟踪每个行组中每个列段的最小值和最大值。  扫描一系列值时，它可以快速计算何时要跳过行组。 将数据排序后，可以跳过其他行组。 

若要在转换期间保留排序顺序，请执行以下操作：
* 结合 DROP_EXISTING 子句使用 [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md)。 这也会保留索引的名称。 如果已有脚本使用行存储索引的名称，你不需要更新这些脚本。 

    此示例将名为 `MyFactTable` 的表中的聚集行存储索引转换到聚集列存储索引。 索引名称 `ClusteredIndex_d473567f7ea04d7aafcac5364c241e09` 保持不变。

    ```sql
    CREATE CLUSTERED COLUMNSTORE INDEX ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
    ON MyFactTable  
    WITH (DROP_EXISTING = ON);  
    ```

## <a name="related-tasks"></a>Related Tasks  
下面是用于创建和维护列存储索引的任务。 
  
|任务|参考主题|说明|  
|----------|----------------------|-----------|  
|将表创建为列存储。|[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)|从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]开始，你可以将表创建为聚集列存储索引。 不需要先创建行存储表，然后将其转换为列存储。|  
|创建具有列存储索引的内存表。|[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)|从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]开始，你可以创建具有列存储索引的内存优化表。 也可以在创建表后使用 ALTER TABLE ADD INDEX 语法添加列存储索引。|  
|将行存储表转换为列存储。|[CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md)|将现有堆集或二进制树转换为列存储。 示例演示了如何在执行此转换时处理现有的索引以及索引的名称。|  
|将列存储表转换为行存储。|[创建群集索引 &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md#d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index) 或[将列存储表转换回行存储堆](../../t-sql/statements/create-columnstore-index-transact-sql.md#e-convert-a-columnstore-table-back-to-a-rowstore-heap) |通常不需要这样转换，但有时需要。 示例演示如何将列存储转换为堆或聚集索引。|   
|在行存储表中创建列存储索引。|[CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md)|一个行存储表可以有一个列存储索引。  从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]开始，列存储索引可以包含筛选条件。 示例演示了基本语法。|  
|为操作分析创建高性能索引。|[开始使用列存储进行实时运行分析](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)|介绍如何创建互补性列存储索引和 B 树索引，以便 OLTP 查询使用 B 树索引，分析查询使用列存储索引。|  
|为数据仓库创建高性能列存储索引。|[列存储索引 - 数据仓库](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)|介绍如何使用列存储表上的 B 树索引来创建高性能数据仓库查询。|  
|使用 B 树索引对列存储索引强制实施主键约束。|[列存储索引 - 数据仓库](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)|演示如何合并 B 树和列存储索引，以便对列存储索引强制实施主键约束。|  
|删除列存储索引|[DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)|删除列存储索引使用 B 树索引所用的标准 DROP INDEX 语法。 删除聚集列存储索引会将列存储表转换为堆。|  
|从列存储索引中删除行|[DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)|使用 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md) 删除行。<br /><br /> **列存储** 行： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将它标记为已逻辑删除但是未回收行的物理存储空间，直到重新生成索引。<br /><br /> **增量存储** 行： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以逻辑和物理方式删除该行。|  
|更新列存储索引中的行|[UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)|使用 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md) 更新行。<br /><br /> **列存储** 行：  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将它标记为已逻辑删除，然后将更新的行插入增量存储中。<br /><br /> **增量存储** 行： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在增量存储中更新它。|  
|强制增量存储中的所有行进入列存储。|[ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md) ...REBUILD<br /><br /> [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)|结合 REBUILD 选项的 ALTER INDEX 会强制所有行进入列存储。|  
|对列存储索引进行碎片整理|[ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)|ALTER INDEX ...REORGANIZE 可在线对列存储索引进行碎片整理。|  
|合并具有列存储索引的表。|[MERGE (Transact-SQL)](../../t-sql/statements/merge-transact-sql.md)|

## <a name="next-steps"></a>后续步骤
若要为以下服务创建空的列存储索引：

* 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)。
* [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]，请参阅 [CREATE TABLE (Azure Synapse Analytics)](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)。

有关如何将现有行存储堆或 B 树索引转换为聚集列存储索引，或创建非聚集列存储索引的详细信息，请参阅 [CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md)。