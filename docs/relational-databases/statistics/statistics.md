---
title: 统计信息
description: 查询优化器使用统计信息来创建可提高查询性能的查询计划。 了解使用查询优化的概念和指导原则。
ms.custom: ''
ms.date: 11/23/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- statistical information [SQL Server], query optimization
- query performance [SQL Server], statistics
- query optimization statistics [SQL Server]
- statistical information [SQL Server], database options
- query optimization statistics [SQL Server], about query optimization statistics
- statistical information [SQL Server], guidelines
- statistical information [SQL Server]
- using statistics [SQL Server]
- statistical information [SQL Server], indexes
- index statistics [SQL Server]
- query optimizer [SQL Server], statistics
- statistics [SQL Server]
ms.assetid: b86a88ba-4f7c-4e19-9fbd-2f8bcd3be14a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a7e1a11993c3c4eb1f5ebc6470c2e0f8387d539a
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506517"
---
# <a name="statistics"></a>统计信息

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  查询优化器使用统计信息来创建可提高查询性能的查询计划。 对于大多数查询，查询优化器已为高质量查询计划生成必要的统计信息；但在一些情况下，需要创建附加的统计信息或修改查询设计以得到最佳结果。 本主题讨论用于高效使用查询优化统计信息的统计信息概念并提供指南。  
  
##  <a name="components-and-concepts"></a><a name="DefinitionQOStatistics"></a> 组件和概念  
### <a name="statistics"></a>统计信息  
 查询优化的统计信息是二进制大型对象 (BLOB)，这些对象包含与值在表或索引视图的一列或多列中的分布有关的统计信息。 查询优化器使用这些统计信息来估计查询结果中的基数或行数。 通过这些基数估计，查询优化器可以创建高质量的查询计划。 例如，查询优化器可以根据谓词使用基数估计选择索引查找运算符而不是更耗资源的索引扫描运算符，假如这样做能提高查询性能。  
  
 每个统计信息对象都在包含一个或对个表列的列表上创建，并且包括将值的分布显示在第一列的直方图。 在多列上的统计信息对象也存储与各列中的值的相关性有关的统计信息。 这些相关性统计信息（或 *密度*）根据列值的不同行的数目派生。 

#### <a name="histogram"></a><a name="histogram"></a>直方图  
直方图度量数据集中每个非重复值的出现频率。 查询优化器根据统计信息对象第一个键列中的列值来计算直方图，它选择列值的方法是以统计方式对行进行抽样或对表或视图中的所有行执行完全扫描。 如果直方图是根据一组抽样行创建的，存储的总行数和非重复值总数则为估计值，且不必为整数。

> [!NOTE]
> <a name="frequency"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的直方图仅为单个列生成 - 统计信息对象键列集的第一列。
  
若要创建直方图，查询优化器将对列值进行排序，计算与每个非重复列值匹配的值数，然后将列值聚合到最多 200 个连续直方图梯级中。 每个直方图梯级都包含一个列值范围，后跟上限列值。 该范围包括介于两个边界值之间的所有可能列值，但不包括边界值自身。 最小排序列值是第一个直方图梯级的上限值。

有关详细信息，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过以下三个步骤从已排序的列值集创建直方图：

- **直方图初始化**：在第一步中，处理始于排序集开始处的一个值序列，并收集 range_high_key、equal_rows、range_rows 和 distinct_range_rows 的最多 200 个值（在此步骤中，range_rows 和 distinct_range_rows 始终为零）     。 已用尽所有输入或已找到 200 个值时，结束第一步。 
- **使用 Bucket 合并进行扫描**：第二步中，按排序顺序处理从统计信息键前导列算起的每个额外值；将每个相继值添加到最后一个范围或在末尾创建一个新范围（这可能是因输入值已排序所致）。 如果创建了一个新范围，则一对现有相邻范围折叠为单个范围。 选择这对范围以最大限度减少信息丢失。 此方法使用最大差异算法使直方图中的梯级数减至最少，并同时最大化边界值之间的差异。 折叠后，梯级数在整个步骤中保持为 200。
- **直方图合并**：第三步中，如果未丢失大量信息，可能折叠更多范围。 直方图梯级数可以少于非重复值的数目，即使对于边界点少于 200 的列也是如此。 因此，即使列包含超过 200 个唯一值，直方图具有的梯级数可能少于 200。 对于仅包含唯一值的列，合并的直方图具有三个梯级（最小梯级数）。

> [!NOTE]
> 如果是使用示例而非全扫描生成直方图，则估计 equal_rows、range_rows、distinct_range_rows 和 average_range_rows 的值，因此它们无需为整数。

下面的关系图显示包含六个梯级的直方图。 第一个上限值左侧的区域是第一个梯级。
  
![直方图](../../relational-databases/system-dynamic-management-views/media/histogram-2.svg "直方图") 
  
对于以上每个直方图步骤：
-   粗线表示上限值 (range_high_key) 和上限值的出现次数 (equal_rows)  
  
-   range_high_key 左侧的纯色区域表示列值范围和每个列值的平均出现次数 (average_range_rows)。 第一个直方图梯级的 average_range_rows 始终是 0。  
  
-   点线表示用于估计范围中的非重复值总数 (distinct_range_rows) 和范围中的总指数 (range_rows)。 查询优化器使用 range_rows 和 distinct_range_rows 计算 average_range_rows，且不存储抽样值。   
  
#### <a name="density-vector"></a><a name="density"></a>密度向量  
密度 是有关给定列或列组合中重复项数目的信息，其计算公式为 1/（非重复值数目）。 查询优化器使用密度提高根据相同表或索引视图返回多个列的查询的基数估计。 密度与值的选择性成反比，密度越小，值的选择性越大。 例如，在一个代表汽车的表中，很多汽车出自同一制造商，但每辆车都有唯一的车牌号 (VIN)。 因为 VIN 的密度比制造商低，所以 VIN 索引比制造商索引更具选择性。 

> [!NOTE]
> 频率是有关统计信息对象第一个键列中每个非重复值出现次数的信息，其计算公式为行计数 * 密度。 最大频率 1 出现在具有唯一值的列中。

密度向量针对统计信息对象中的列的每个前缀包含一个密度。 例如，如果统计信息对象包含键列 `CustomerId`、`ItemId` 和 `Price`，则根据以下每个列前缀计算密度。
  
|列前缀|计算密度所基于的对象|  
|---|---|
|(CustomerId)|具有与 CustomerId 匹配的值的行|  
|(CustomerId, ItemId)|具有与 CustomerId 和 ItemId 匹配的值的行|  
|(CustomerId, ItemId, Price)|具有与 CustomerId、ItemId 和 Price 匹配的值的行| 

### <a name="filtered-statistics"></a>筛选的统计信息  
 筛选统计信息可以提高以下从定义完善的数据子集选择数据的查询的查询性能。 筛选统计信息使用筛选器谓词来选择统计信息中包括的数据子集。 与全表统计信息相比，设计完美的筛选统计信息可以改进查询执行计划。 有关筛选器谓词的详细信息，请参阅 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)。 有关何时创建筛选的统计信息的详细信息，请参阅本主题中的 [何时创建统计信息](#CreateStatistics) 部分。  
 
### <a name="statistics-options"></a>统计信息选项  
 可以设置三个选项来影响何时以及如何创建和更新统计信息。 这些选项仅在数据库级别设置。  
  
#### <a name="auto_create_statistics-option"></a><a name="AutoUpdateStats"></a>AUTO_CREATE_STATISTICS 选项  
 在自动创建统计信息选项 [AUTO_CREATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_create_statistics) 为 ON 时，查询优化器将根据需要在查询谓词中的单独列上创建统计信息，以便改进查询计划的基数估计。 这些单列统计信息在现有统计信息对象中尚未具有[直方图](#histogram)的列上创建。 AUTO_CREATE_STATISTICS 选项不确定是否为索引创建了统计信息。 此选项也不生成筛选统计信息。 它严格应用于全表的单列统计信息。  
  
 查询优化器通过使用 AUTO_CREATE_STATISTICS 选项创建统计信息时，统计信息名称以 `_WA` 开头。 可以使用下面的查询来确定查询优化器是否为查询谓词列创建了统计信息。  
  
```sql  
SELECT OBJECT_NAME(s.object_id) AS object_name,  
    COL_NAME(sc.object_id, sc.column_id) AS column_name,  
    s.name AS statistics_name  
FROM sys.stats AS s 
INNER JOIN sys.stats_columns AS sc  
    ON s.stats_id = sc.stats_id AND s.object_id = sc.object_id  
WHERE s.name like '_WA%'  
ORDER BY s.name;  
```  
  
#### <a name="auto_update_statistics-option"></a>AUTO_UPDATE_STATISTICS 选项  
 在自动更新统计信息选项 [AUTO_UPDATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics) 为 ON 时，查询优化器将确定统计信息何时可能过期，然后在查询使用这些统计信息时更新它们。 统计信息将在插入、更新、删除或合并操作更改表或索引视图中的数据分布后过期。 查询优化器通过计算自最后统计信息更新后数据修改的次数并且将这一修改次数与某一阈值进行比较，确定统计信息何时可能过期。 该阈值基于表中或索引视图中的行数。  
  
* 直到 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 基于更改行的百分比使用阈值。 这与表中的行数无关。 阈值是：
    * 如果在评估时间统计信息时表基数为 500 或更低，则每达到 500 次修改时更新一次。
    * 如果在评估时间统计信息时表基数大于 500，则每达到 500 + 修改次数的百分之二十时更新一次。

* 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，如果[数据库兼容性级别](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)为 130，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将使用递减的动态统计信息更新阈值，此阈值将根据表中的行数进行调整。 它的计算方式为 1000 与当前的表基数乘积的平方根。 例如，如果表中包含 200 万行，则计算为 sqrt(1000 * 2000000) = 44721.359。 进行此更改后，将会更频繁地更新大型表的统计信息。 但是，如果数据库的兼容性级别低于 130，则 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 阈值适用。 ?

> [!IMPORTANT]
> 在 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 到 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]，或者在[数据库兼容性级别](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) 120 和更低级别下的 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 和更高版本中，启用[跟踪标志 2371](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)，以便 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用降低的动态统计信息更新阈值。

可以使用以下指导在预 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 环境中启用跟踪标志 2371：

 - 如果你没有看到因统计信息过期而导致的性能问题，则无需启用此跟踪标志。
 - 如果你在 SAP 系统上，请启用此跟踪标志。  请参阅此[博客](/archive/blogs/saponsqlserver/changes-to-automatic-update-statistics-in-sql-server-traceflag-2371)获取其他信息。
 - 如果因为当前的自动更新触发频率不足而必须依赖于夜间作业来更新统计信息，请考虑启用跟踪标志 2371 以降低阈值。
  
查询优化器在编译查询和执行缓存查询计划前，检查是否存在过期的统计信息。 在编译某一查询前，查询优化器使用查询谓词中的列、表和索引视图确定哪些统计信息可能过期。 在执行缓存查询计划前， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 确认该查询计划引用最新的统计信息。  
  
AUTO_UPDATE_STATISTICS 选项适用于为索引创建的统计信息对象、查询谓词中的单列以及使用 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 语句创建的统计信息。 此选项也适用于筛选的统计信息。  
 
可以使用 [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) 准确地跟踪表中更改的行数，并决定是否要手动更新统计信息。



  
#### <a name="auto_update_statistics_async"></a>AUTO_UPDATE_STATISTICS_ASYNC  
异步统计信息更新选项 [AUTO_UPDATE_STATISTICS_ASYNC](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics_async) 将确定查询优化器是使用同步统计信息更新还是异步统计信息更新。 默认情况下，异步统计信息更新选项为 OFF 状态，并且查询优化器以同步方式更新统计信息。 AUTO_UPDATE_STATISTICS_ASYNC 选项适用于为索引创建的统计信息对象、查询谓词中的单列以及使用 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 语句创建的统计信息。  
 
> [!NOTE]
> 若要在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中设置异步统计信息更新选项，需在“数据库属性”窗口的“选项”页中同时将“自动更新统计信息”和“自动异步更新统计信息”选项设置为“True”。
  
统计信息更新可以是同步（默认设置）或异步的。 对于同步统计信息更新，查询将始终用最新的统计信息编译和执行；在统计信息过期时，查询优化器将在编译和执行查询前等待更新的统计信息。 对于异步统计信息更新，查询将用现有的统计信息编译，即使现有统计信息已过期。如果在查询编译时统计信息过期，查询优化器可以选择非最优查询计划。 在异步更新完成后编译的查询将从使用更新的统计信息中受益。  
  
执行更改数据分布的操作（例如截断表或对很大百分比的行执行大容量更新）时，考虑使用同步统计信息。 如果您在完成该操作后未更新统计信息，则使用同步统计信息将确保对更改的数据执行查询前统计信息是最新的。  
  
在以下情况下，考虑使用异步统计信息来实现可预测性更高的查询响应时间：  
  
* 您的应用程序频繁执行相同的查询、类似的查询或类似的缓存查询计划。 与同步统计信息更新相比，使用异步统计信息更新时你的查询响应时间可能具有更高的可预测性，因为查询优化器可以执行传入的查询而不必等待最新的统计信息。 这避免延迟某些查询，而不延迟其他查询。  
  
* 您的应用程序遇到了客户端请求超时，这些超时是由于一个或多个查询正在等待更新后的统计信息所导致的。 在某些情况下，等待同步统计信息可能会导致应用程序因过长超时而失败。  

> [!NOTE]
> 不管 AUTO_UPDATE_STATISTICS_ASYNC 选项如何，本地临时表上的统计信息都始终同步更新。 而全局临时表上的统计信息会同步或异步更新，具体取决于为用户数据库设置的 AUTO_UPDATE_STATISTICS_ASYNC 选项。

异步统计信息更新由后台请求执行。 当请求准备好将更新后的统计信息写入数据库时，它将尝试获取统计信息元数据对象上的架构修改锁。 如果其他会话已经锁定了同一对象，则将阻止异步统计信息更新，直到可以获取架构修改锁。 类似地，需要获取统计信息元数据对象上架构稳定性锁以编译查询的会话可能被已经持有或正在等待获取架构修改锁的异步统计信息更新后台会话阻止。 因此，对于具有非常频繁的查询编译和频繁统计信息更新的工作负载，使用异步统计信息可能会增加由于锁定阻止而导致并发问题的可能性。

在 Azure SQL 数据库中，如果启用 ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY [数据库范围的配置](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)，可以使用异步统计信息更新来避免潜在并发问题。 启用此配置后，后台请求会在单独的低优先级队列中等待获取架构修改锁，从而允许其他请求继续使用现有统计信息编译查询。 当没有其他会话锁定统计信息元数据对象时，后台请求将获取其架构修改锁和更新统计信息。 如果后台请求在几分钟的超时期限内无法获取锁定（不太可能发生这种情况），将中止异步统计信息更新。在这种情况下，在触发另一次自动统计信息更新或者[手动更新](update-statistics.md)统计信息之前，不会更新统计信息。

#### <a name="incremental"></a>INCREMENTAL  
 CREATE STATISTICS 的 INCREMENTAL 选项为 ON 时，创建的统计信息为每个分区的统计信息。 为 OFF 时，删除统计信息树并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 重新计算统计信息。 默认为 OFF。 此设置覆盖数据库级别 INCREMENTAL 属性。 要深入了解如何创建增量统计信息，请参阅 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)。 要深入了解如何自动创建每个分区的统计信息，请参阅[数据库属性（“选项”页）](../../relational-databases/databases/database-properties-options-page.md#automatic)和 [ALTER DATABASE SET 选项 &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)。 
  
 在将新分区添加到某个大型表时，应更新统计信息以便包括这些新分区。 但是，浏览整个表（FULLSCAN 或 SAMPLE 选项）所需的时间可能会相当长。 此外，因为可能只需针对新分区的统计信息，所以，扫描整个表不是必需的。 该增量选项将在每个分区的基础上创建和存储统计信息，并且在更新时，只刷新需要新统计信息的那些分区上的统计信息  
  
 如果不支持每个分区统计信息，将忽略该选项并生成警告。 对于以下统计信息类型，不支持增量统计信息：  
  
* 使用未与基表的分区对齐的索引创建的统计信息。  
* 对 Always On 可读辅助数据库创建的统计信息。  
* 对只读数据库创建的统计信息。  
* 对筛选的索引创建的统计信息。  
* 对视图创建的统计信息。  
* 对内部表创建的统计信息。  
* 使用空间索引或 XML 索引创建的统计信息。  
  
**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。 
  
## <a name="when-to-create-statistics"></a><a name="CreateStatistics"></a> 何时创建统计信息  
 查询优化器已通过以下方式创建统计信息：  
  
1.  在创建索引时，查询优化器为表或视图上的索引创建统计信息。 这些统计信息将创建在索引的键列上。 如果索引是一个筛选索引，则查询优化器将在为该筛选索引指定的行的同一子集上创建筛选统计信息。 有关筛选索引的详细信息，请参阅[创建筛选索引](../../relational-databases/indexes/create-filtered-indexes.md)和 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  

    > [!NOTE]
    > 从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]开始，当创建或重新生成已分区索引时，不会通过扫描表中的所有行来创建统计信息。 相反，查询优化器使用默认采样算法来生成统计信息。 在升级具有已分区索引的数据库后，您可以在直方图数据中注意到针对这些索引的差异。 此行为更改可能不会影响查询性能。 若要通过扫描表中所有行的方法获得有关已分区索引的统计信息，请使用 `CREATE STATISTICS` 或 `UPDATE STATISTICS` 以及 `FULLSCAN` 子句。 
  
2.  在 [AUTO_CREATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_create_statistics) 为 ON 时，查询优化器为查询谓词中的单列创建统计信息。  

对于大多数查询，用于创建统计信息的这两个方法就可以确保高质量的查询计划；但在很少的情况下，可以通过使用 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 语句创建附加的统计信息来改进查询计划。 这些附加的统计信息可以捕获查询优化器在为索引或单列创建统计信息时并未考虑的统计关联。 应用程序可能在表数据中具有附加的统计关联，如果在统计信息对象中计入这些关联，可能会令查询优化器改进查询计划。 例如，针对数据行子集的筛选统计信息或针对查询谓词列的多列统计信息可改进查询计划。  
  
在使用 CREATE STATISTICS 语句创建统计信息时，我们建议保持 AUTO_CREATE_STATISTICS 选项为 ON，以便查询优化器继续为查询谓词列定期创建单列统计信息。 有关查询谓词的详细信息，请参阅[搜索条件 (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md)。  
  
在以下任何情况适用时，考虑使用 CREATE STATISTICS 语句创建统计信息：  

* [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问建议创建统计信息。 
* 查询谓词包含尚不位于相同索引中的多个相关列。  
* 查询从数据的子集中选择数据。  
* 查询缺少统计信息。  
  
### <a name="query-predicate-contains-multiple-correlated-columns"></a>查询谓词包含多个相关列  
在某一查询谓词包含具有跨列关系和相关性的多列时，针对多列的统计信息可能会改进查询计划。 针对多列的统计信息包含称作密度的跨列相关统计信息，这些统计信息不可用于单列统计信息。 在查询结果依赖于多列之间的数据关系时，密度可以改进基数估计。  
  
如果多个列已处于同一索引中，则多列统计信息对象已存在并且不必手动创建它。 如果这些列尚未处于同一索引中，则可以通过对多个列创建索引或通过使用 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 语句，创建多列统计信息。 与统计信息对象相比，它要求更多的系统资源来维护索引。 如果应用程序不要求多列索引，您可以通过创建统计信息对象但不创建索引，有效地利用系统资源。  
  
在创建多列统计信息时，统计信息对象定义中列的顺序将影响生成基数估计的密度的效率。 统计信息对象在统计信息对象定义中存储键列的每个前缀的密度。 有关密度的详细信息，请参阅此页中的[密度](#density)部分。  
  
为了创建用于基数估计的密度，查询谓词中的列必须匹配统计信息对象定义中列的前缀之一。 例如，以下内容在列 `LastName`、 `MiddleName`和 `FirstName`上创建多列统计信息对象。  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.stats  
    WHERE name = 'LastFirst'  
    AND object_ID = OBJECT_ID ('Person.Person'))  
DROP STATISTICS Person.Person.LastFirst;  
GO  
CREATE STATISTICS LastFirst ON Person.Person (LastName, MiddleName, FirstName);  
GO  
```  
  
在此示例中，统计信息对象 `LastFirst` 具有以下列前缀的密度：`(LastName)`、`(LastName, MiddleName)` 和 `(LastName, MiddleName, FirstName)`。 密度不可用于 `(LastName, FirstName)`。 如果查询使用 `LastName` 和 `FirstName` ，并且没有使用 `MiddleName`，则密度不可用于基数估计。  
  
### <a name="query-selects-from-a-subset-of-data"></a>查询从数据的子集中选择数据  
在查询优化器为单个列和索引创建统计信息时，它为所有行中的值创建统计信息。 在查询从行的某一子集中选择数据时，这一行子集具有唯一的数据分布，筛选统计信息可以改进查询计划。 可以通过使用 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 语句并在此语句中用 [WHERE](../../t-sql/queries/where-transact-sql.md) 子句定义筛选器谓词表达式来创建筛选统计信息。  
  
例如，使用 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 时，`Production.Product` 表中的每种产品都属于 `Production.ProductCategory` 表中的以下四个类别之一：自行车、组件、服装或附件。 上述每个类别在重量方面的数据分布均不同：自行车的重量范围为 13.77 到 30.0，部件的重量范围为 2.12 到 1050.00 且有些部件的重量为 NULL 值，服装的重量全部为 NULL，附件的重量也为 NULL。  
  
使用自行车为例，针对所有自行车重量的筛选统计信息将向查询优化器提供更精确的统计信息，并且与全表统计信息或者针对 Weight 列的不存在的统计信息相比，可以改进查询计划质量。 该自行车重量列很适合于筛选统计信息，但如果重量查找的数目相对较少，则不见得适合于筛选索引。 筛选索引所提供的在查找方面的性能提升可能抵不上将筛选索引添加到数据库所导致的额外维护和存储成本。  
  
以下语句对自行车的所有子类别创建 `BikeWeights` 筛选统计信息。 筛选的谓词表达式通过使用比较 `Production.ProductSubcategoryID IN (1,2,3)`枚举所有自行车子类别，对自行车进行定义。 该谓词无法使用“自行车”类别名称，因为它存储于 Production.ProductCategory 表中，并且筛选表达式中的所有列都必须位于相同的表中。  
  
[!code-sql[StatisticsDDL#FilteredStats2](../../relational-databases/statistics/codesnippet/tsql/statistics_1.sql)]  
  
查询优化器可使用 `BikeWeights` 筛选的统计信息来改进下面这个查询的查询计划，此查询选择重量超过 `25` 的所有自行车。  
  
```sql  
SELECT P.Weight AS Weight, S.Name AS BikeName  
FROM Production.Product AS P  
    JOIN Production.ProductSubcategory AS S   
    ON P.ProductSubcategoryID = S.ProductSubcategoryID  
WHERE P.ProductSubcategoryID IN (1,2,3) AND P.Weight > 25  
ORDER BY P.Weight;  
GO  
```  
  
### <a name="query-identifies-missing-statistics"></a>查询识别缺少的统计信息  
如果错误或其他事件阻止查询优化器创建统计信息，则查询优化器将不使用统计信息创建查询计划。 查询优化器将统计信息标记为缺失，并且在下次执行查询时尝试重新生成统计信息。  
  
在使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]以图形方式显示查询的执行计划时，缺少的统计信息将予以警告显示（表名称以红色文本显示）。 另外，使用 **监视** Missing Column Statistics [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 事件类可以指明何时缺少统计信息。 有关详细信息，请参阅 [Errors and Warnings 事件类别（数据库引擎）](../../relational-databases/event-classes/errors-and-warnings-event-category-database-engine.md)。  
  
 如果缺少统计信息，则执行以下步骤：  
  
* 确认 [AUTO_CREATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_create_statistics) 和 [AUTO_UPDATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics) 为 ON。  
* 请确保数据库不是只读的。 如果数据库是只读的，则无法保存新统计信息对象。  
* 通过使用 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 语句创建缺少的统计信息。  
  
当有关只读数据库或只读快照的统计信息丢失或变得陈旧时， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将创建临时统计信息并在 **tempdb** 中进行维护。 当 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 创建临时统计信息时，将在统计信息名称后追加后缀 _readonly_database_statistic，以便将临时统计信息与永久统计信息加以区分。 后缀 _readonly_database_statistic 是为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成的统计信息预留的。 可以在读写数据库上创建和重新生成临时统计信息的脚本。 编写脚本时，[!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 将统计信息名称的后缀从 _readonly_database_statistic 更改为 _readonly_database_statistic_scripted。  
  
只有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以创建和更新临时统计信息。 但是，您可以使用用于永久统计信息的相同工具来删除临时统计信息和监视统计信息属性：  
  
* 使用 [DROP STATISTICS](../../t-sql/statements/drop-statistics-transact-sql.md) 语句删除临时统计信息。  
* 使用 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) 和 [sys.stats_columns](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md) 目录视图监视统计信息 。 **sys_stats** 包含 **is_temporary** 列，用于指示哪些统计信息是永久的，哪些统计信息是临时的。  
  
 因为临时统计信息存储于 **tempdb** 中，所以重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务将导致所有临时统计信息消失。  
    
## <a name="when-to-update-statistics"></a><a name="UpdateStatistics"></a> 何时更新统计信息  
 查询优化器确定统计信息何时可能过期，然后在查询计划需要统计信息时更新它们。 在某些情况下，将 [AUTO_UPDATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics) 设置为 ON 时，可以通过更频繁地更新统计信息来优化查询计划，并因此提高查询性能。 可以使用 UPDATE STATISTICS 语句或存储过程 sp_updatestats 来更新统计信息。  
  
 更新统计信息可确保查询使用最新的统计信息进行编译。 不过，更新统计信息会导致查询重新编译。 我们建议不要太频繁地更新统计信息，因为需要在改进查询计划和重新编译查询所用时间之间权衡性能。 具体的折衷方案取决于你的应用程序。  
  
 在使用 UPDATE STATISTICS 或 sp_updatestats 更新统计信息时，我们建议保持将 AUTO_UPDATE_STATISTICS 设置为 ON，以便查询优化器继续定期更新统计信息。 有关如何更新列、索引、表或索引视图的统计信息的详细信息，请参阅 [UPDATE STATISTICS (Transact-SQL)](../../t-sql/statements/update-statistics-transact-sql.md)。 有关如何为数据库中的所有用户定义表和内部表更新统计信息的信息，请参阅存储过程 [sp_updatestats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)。  
  
 若要确定最近一次更新统计信息的时间，请使用 [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) 或 [STATS_DATE](../../t-sql/functions/stats-date-transact-sql.md) 函数。  
  
 在以下情况下考虑更新统计信息：  
  
* 查询执行时间很长。  
* 在升序或降序键列上发生插入操作。  
* 在维护操作后。  

### <a name="query-execution-times-are-slow"></a>查询执行时间很长  
 如果查询响应时间很长或不可预知，则在执行其他故障排除步骤前，确保查询具有最新的统计信息。  
  
### <a name="insert-operations-occur-on-ascending-or-descending-key-columns"></a>在升序或降序键列上发生插入操作  
 与查询优化器执行的统计信息更新相比，升序或降序键列（例如 IDENTITY 或实时时间戳列）上的统计信息可能要求更频繁地更新。 插入操作将新值追加到升序或降序键列上。 添加的行的数目可能过小，以致无法触发统计信息更新。 如果统计信息不是最新的并且查询从最频繁添加的行中选择数据，则当前统计信息将没有这些新值的基数估计。 这可能导致不精确的基数估计和查询性能低下。  
  
 例如，如果统计信息未更新以包括最近销售订单日期的基数估计，则从最近销售订单日期选择的查询将具有不精确的基数估计。  
  
### <a name="after-maintenance-operations"></a>在维护操作后  
 考虑在执行维护过程（例如截断表或对很大百分比的行执行大容量插入）后更新统计信息。 这可以避免在将来查询等待自动统计信息更新时在查询处理中出现延迟。  
  
 索引的重新生成、碎片整理或重新组织之类的操作不会更改数据的分布。 因此，在执行 [ALTER INDEX REBUILD](../../t-sql/statements/alter-index-transact-sql.md#rebuilding-indexes)、[DBCC REINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md)、[DBCC INDEXDEFRAG](../../t-sql/database-console-commands/dbcc-indexdefrag-transact-sql.md) 或 [ALTER INDEX REORGANIZE](../../t-sql/statements/alter-index-transact-sql.md#reorganizing-indexes) 操作后，无需更新统计信息。 查询优化器将在使用 ALTER INDEX REBUILD 或 DBCC DBREINDEX 对表或视图重新生成索引时更新统计信息，但是，此统计信息更新是重新创建索引的副产品。 在 DBCC INDEXDEFRAG 或 ALTER INDEX REORGANIZE 操作后，查询优化器并不更新统计信息。 
 
> [!TIP]
> 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 CU4 开始，使用 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md) 或 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md) 的 PERSIST_SAMPLE_PERCENT 选项设置和保留未显式指定采样百分比的后续统计信息更新的特定采样百分比。

### <a name="automatic-index-and-statistics-management"></a>自动索引和统计信息管理

利用[自适应索引碎片整理](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)等解决方案，自动管理一个或多个数据库的索引碎片整理和统计信息更新。 此过程根据碎片级别以及其他参数，自动选择是重新生成索引还是重新组织索引，并使用线性阈值更新统计信息。
  
##  <a name="queries-that-use-statistics-effectively"></a><a name="DesignStatistics"></a> 高效使用统计信息的查询  
 某些查询实现（如查询谓词中的局部变量和复杂的表达式）可能导致查询计划不是最佳的。 遵循有关高效使用统计信息的查询设计指导原则可以避免这种情况。 有关查询谓词的详细信息，请参阅[搜索条件 (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md)。  
  
 您可以通过应用查询设计指导原则来改进查询计划，这些查询设计指导原则高效地使用统计信息，以便改进在查询谓词中使用的表达式、变量和函数的 *基数估计* 。 在查询优化器不知道表达式、变量或函数的值时，它并不知道在直方图中要查找的值，因此无法从直方图检索最佳的基数估计。 查询优化器而是为直方图中所有取样行，在每个不同值的平均行数的基础上执行基数估计。 这将导致不是最佳的基数估计并且可能影响查询性能。 有关直方图的详细信息，请参阅本页的[直方图](#histogram)部分或 [sys.dm_db_stats_histogram](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)。
  
 下面的指导原则描述如何编写查询以便通过改进基数估计来改进查询计划。  
  
### <a name="improving-cardinality-estimates-for-expressions"></a>改进表达式的基数估计  
要改进表达式的基数估计，请遵循以下指导原则：  
  
* 只要可能，应简化其中含常量的表达式。 查询优化器在确定基数估计前并不对包含常量的所有函数和表达式进行求值。 例如，将表达式 `ABS(-100)` 简化为 `100`。  
  
* 如果表达式使用多个变量，则考虑为表达式创建一个计算列，然后对该计算列创建统计信息或索引。 例如，如果您为表达式 `WHERE PRICE + Tax > 100` 创建计算列，则查询谓词 `Price + Tax`可能会具有更好的基数估计。  
  
### <a name="improving-cardinality-estimates-for-variables-and-functions"></a>改进变量和函数的基数估计  
要改进变量和函数的基数估计，请遵循以下指导原则：  
  
* 如果查询谓词使用局部变量，则考虑重新编写查询以使用参数，而非局部变量。 在查询优化器创建查询执行计划时，局部变量的值未知。 在查询使用某一参数时，查询优化器将基数估计用于传递到存储过程的第一个实际参数值。  
  
* 考虑使用标准表或临时表来保存多语句表值函数 (mstvf) 的结果。 查询优化器并不为多语句表值函数创建统计信息。 使用此方法，查询优化器可对表列创建统计信息并使用它们创建更好的查询计划。  
  
* 考虑使用标准表或临时表来代替表变量。 查询优化器不会为表变量创建统计信息。 使用此方法，查询优化器可对表列创建统计信息并使用它们创建更好的查询计划。 在确定是使用临时表还是表变量时需要进行一些权衡，与临时表相比，在存储过程中使用的表变量会导致更少的存储过程的重新编译。 根据应用程序，使用临时表来代替表变量可能不会改进性能。  
  
* 如果某一存储过程包含使用某一传入的参数的查询，则在查询中使用该参数值之前，应避免在该存储过程内更改该参数值。 查询的基数估计基于传入的参数值，而非更新的值。 为了避免更改参数值，您可以重新编写查询以使用两个存储过程。  
  
     例如，以下存储过程 `Sales.GetRecentSales` 将在 `@date` 为 NULL 时更改参数 `@date` 的值。  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    IF OBJECT_ID ( 'Sales.GetRecentSales', 'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetRecentSales;  
    GO  
    CREATE PROCEDURE Sales.GetRecentSales (@date datetime)  
    AS BEGIN  
        IF @date IS NULL  
            SET @date = DATEADD(MONTH, -3, (SELECT MAX(ORDERDATE) FROM Sales.SalesOrderHeader))  
        SELECT * FROM Sales.SalesOrderHeader h, Sales.SalesOrderDetail d  
        WHERE h.SalesOrderID = d.SalesOrderID  
        AND h.OrderDate > @date  
    END  
    GO  
    ```  
  
     如果对存储过程 `Sales.GetRecentSales` 的首次调用为 `@date` 参数传递了 NULL，则查询优化器将使用针对 `@date = NULL` 的基数估计编译存储过程，即使查询谓词不是使用 `@date = NULL`调用的。 此基数估计可能与实际查询结果中的行数差别很大。 因此，查询优化器可能会选择非最佳查询计划。 若要避免此情况发生，您可以按如下所示将存储过程重新编写成两个过程：  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    IF OBJECT_ID ( 'Sales.GetNullRecentSales', 'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetNullRecentSales;  
    GO  
    CREATE PROCEDURE Sales.GetNullRecentSales (@date datetime)  
    AS BEGIN  
        IF @date is NULL  
            SET @date = DATEADD(MONTH, -3, (SELECT MAX(ORDERDATE) FROM Sales.SalesOrderHeader))  
        EXEC Sales.GetNonNullRecentSales @date;  
    END  
    GO  
    IF OBJECT_ID ( 'Sales.GetNonNullRecentSales', 'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetNonNullRecentSales;  
    GO  
    CREATE PROCEDURE Sales.GetNonNullRecentSales (@date datetime)  
    AS BEGIN  
        SELECT * FROM Sales.SalesOrderHeader h, Sales.SalesOrderDetail d  
        WHERE h.SalesOrderID = d.SalesOrderID  
        AND h.OrderDate > @date  
    END  
    GO  
    ```  
  
### <a name="improving-cardinality-estimates-with-query-hints"></a>使用查询提示改进基数估计  
 为了改进局部变量的基数估计，可以将 `OPTIMIZE FOR <value>` 或 `OPTIMIZE FOR UNKNOWN` 查询提示与 RECOMPILE 一起使用。 有关详细信息，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。  
  
 对于某些应用程序，每次执行查询时都重新编译查询可能会占用过多时间。 `OPTIMIZE FOR` 查询提示可对此给予帮助，即使不使用 `RECOMPILE` 选项。 例如，可以将 `OPTIMIZE FOR` 选项添加到存储过程 Sales.GetRecentSales，以便指定一个特定的日期。 以下示例将 `OPTIMIZE FOR` 选项添加到 Sales.GetRecentSales 过程。  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ( 'Sales.GetRecentSales', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.GetRecentSales;  
GO  
CREATE PROCEDURE Sales.GetRecentSales (@date datetime)  
AS BEGIN  
    IF @date is NULL  
        SET @date = DATEADD(MONTH, -3, (SELECT MAX(ORDERDATE) FROM Sales.SalesOrderHeader))  
    SELECT * FROM Sales.SalesOrderHeader h, Sales.SalesOrderDetail d  
    WHERE h.SalesOrderID = d.SalesOrderID  
    AND h.OrderDate > @date  
    OPTION ( OPTIMIZE FOR ( @date = '2004-05-01 00:00:00.000'))  
END;  
GO  
```  
  
### <a name="improving-cardinality-estimates-with-plan-guides"></a>使用计划指南改进基数估计  
 对于某些应用程序，查询计划指南可能不适用，因为您无法更改查询，或者使用 RECOMPILE 查询提示可能导致过多的重新编译。 您可以使用计划指南来指定 USE PLAN 之类的其他提示，以便在向应用程序供应商调查应用程序变化的同时，控制查询的行为。 有关计划指南的详细信息，请参阅 [Plan Guides](../../relational-databases/performance/plan-guides.md)。  
  
  
## <a name="see-also"></a>另请参阅  
 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)   
 [UPDATE STATISTICS (Transact-SQL)](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_updatestats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [DROP STATISTICS (Transact-SQL)](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)   
 [创建筛选索引](../../relational-databases/indexes/create-filtered-indexes.md)   
 [控制 SQL Server 中的 Autostat (AUTO_UPDATE_STATISTICS) 行为](https://support.microsoft.com/help/2754171)   
 [STATS_DATE (Transact-SQL)](../../t-sql/functions/stats-date-transact-sql.md)   
 [sys.dm_db_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)  
 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)  
 [sys.stats_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)    
 [自适应索引碎片整理](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)