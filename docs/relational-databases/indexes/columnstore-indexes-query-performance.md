---
description: 列存储索引 - 查询性能
title: 列存储索引 - 查询性能 | Microsoft Docs
ms.custom: ''
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 83acbcc4-c51e-439e-ac48-6d4048eba189
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a7e2aaa0e01a5ca5295bc9f315c44cd7358b1d9f
ms.sourcegitcommit: 9c6130d498f1cfe11cde9f2e65c306af2fa8378d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93036104"
---
# <a name="columnstore-indexes---query-performance"></a>列存储索引 - 查询性能

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  有助于实现列存储索引旨在提供的快速查询性能的建议。    
    
 列存储索引对于分析和数据仓库工作负荷最多可提高 100 倍性能，并且比传统行存储索引最多可提高 10 倍的数据压缩率。 这些建议可帮助查询实现列存储索引旨在提供的快速查询性能。 结尾处提供了有关列存储性能的进一步说明。    
    
## <a name="recommendations-for-improving-query-performance"></a>提高查询性能的建议    
 以下是有助于实现列存储索引旨在提供的高性能的一些建议。    
    
### <a name="1-organize-data-to-eliminate-more-rowgroups-from-a-full-table-scan"></a>1.组织数据使更多行组不用进行全表扫描    
    
-   **利用插入顺序。** 通常情况下，在传统数据仓库中，数据实际上是按时间顺序插入的，而分析是在时间维度中完成的。 例如，按季度分析销售额。 对于此类型的工作负荷，行组消除自动发生。 在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中，可以找出在查询处理过程中跳过的数字行组。    
    
-   **利用行存储聚集索引。** 如果常见的查询谓词在某一列（例如 C1）上，而该列与行的插入顺序无关，则可在列 C1 上创建一个行存储聚集索引，再通过删除行存储聚集索引来创建聚集列存储索引。 如果使用 `MAXDOP = 1` 显式创建聚集列存储索引，则得到的聚集列存储索引会在 C1 列上完美排序。 如果指定了 `MAXDOP = 8`，则值会在 8 个行组中重叠。 此策略的一个常见示例是当你最初使用大型数据集创建列存储索引时。 请注意，对于非聚集列存储索引 (NCCI)，如果基本行存储表具有聚集索引，则行已经排序。 在这种情况下，生成的非聚集列存储索引将自动进行排序。 要注意的重要一点是，列存储索引本身不维护行的顺序。 当插入新行或更新旧行时，你可能需要重复该过程，因为分析查询性能可能会降低    
    
-   **利用表分区。** 可以对列存储索引进行分区，然后使用分区排除来减少要扫描的行组数。 例如，事实数据表存储客户的购买情况，常见的查询模式是查找特定客户的季度购买情况，可以将插入顺序与对客户列进行分区结合使用。 每个分区都会包含特定客户的按时间顺序排序的行。 此外，如果需要从列存储中删除数据，请考虑使用表分区。 切出并截断不再需要的分区是一种高效的策略，可以删除数据而不会产生由较小的行组引入的碎片。    

-   避免删除大量数据。 从行组中删除压缩行不是同步操作。 解压缩行组，删除行，然后重新压缩可能会产生很高的系统开销。 因此，如果从压缩的行组中删除数据，这些行组仍会被扫描，即使它们返回的行数较少也是如此。 如果多个行组的已删除行数足以合并为较少的行组，则重新组织列存储会提高索引的质量，并提高查询性能。 如果数据删除过程通常会清空整个行组，请考虑使用表分区，切出不再需要的分区，并将其截断，而不是删除行。 

    > [!NOTE]
    > 从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始，tuple-mover 通过后台合并任务获得帮助，该任务会自动压缩较小的已存在一段时间（由内部阈值确定）的 OPEN 增量行组，或者合并已从中删除大量行的 COMPRESSED 行组。 随着时间的推移，这会提高列存储索引的质量。   
    > 如果需要从列存储索引中删除大量数据，请考虑随着时间的推移将操作拆分为更小的删除批处理，以允许后台合并任务来处理较小行组的合并任务并提高索引质量，从而无需在删除数据后计划索引重组维护时段。    
    > 有关列存储术语和概念的详细信息，请参阅[列存储索引：概述](../../relational-databases/indexes/columnstore-indexes-overview.md)。
    
### <a name="2-plan-for-enough-memory-to-create-columnstore-indexes-in-parallel"></a>2.计划足够的内存以便并行创建列存储索引    
 创建列存储索引默认情况下是一种并行操作，除非内存受到约束。 并行创建索引要求比按顺序创建索引更多的内存。 在内存充足的情况下，创建列存储索引相当于在同一列上生成 B 树所用时间的 1.5 倍。    
    
 创建列存储索引所需的内存取决于列数、字符串列的数目、并行度 (DOP) 和数据特性。 例如，如果表包含的行不到 100 万行，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 仅使用一个线程创建列存储索引。    
    
 如果表中包含的行超过 100 万行，但 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法获得足够大的内存授予来使用 MAXDOP 创建索引，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会根据需要自动减少 `MAXDOP`，以便适合可用内存授予。  在某些情况下，DOP 必须减小到一个以便在受到约束的内存下生成索引。    
    
 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，查询始终以批处理模式运行。 在以前版本中，仅当 DOP 大于 1 时，才使用批处理执行。    
    
## <a name="columnstore-performance-explained"></a>说明的列存储性能    
 列存储索引通过将高速内存中批处理模式处理与可极大减少 I/O 要求的技术组合使用来实现高查询性能。 由于分析查询扫描大量行，它们通常进行 IO 绑定，因此在查询执行过程中减少 I/O 对于列存储索引的设计至关重要。 数据读取到内存中后，减少内存中的操作数量很重要。    
    
 列存储索引通过高数据压缩率、列存储消除、行组消除和批处理来减少 I/O 和优化内存中操作。    
    
### <a name="data-compression"></a>数据压缩    
 列存储索引可实现比行存储索引最多高 10 倍的数据压缩率。 这极大地减少了执行分析查询所需的 I/O，并因此可以提高查询性能。    
    
-   列存储索引从磁盘读取压缩的数据，这意味着需要将更少字节的数据读取到内存。    
    
-   列存储索引以压缩形式将数据存储在内存中，这可减少将相同数据读取到内存的次数，从而减少 I/O。 例如，与以未压缩形式存储数据相比，列存储索引使用 10 倍压缩率可以在内存中保留 10 倍更多数据。 内存中的数据更多，列存储索引则更有可能无需从磁盘进行更多读取，而是在内存中找到所需的数据。    
    
-   列存储索引按列（而不是按行）压缩数据，从而实现高压缩率并减少磁盘上存储的数据的大小。 每个列独自压缩和存储。  列中的数据将始终具有相同的数据类型，并往往具有相似的值。 当值相似时，数据压缩技术可以很好地实现更高的压缩率。    
    
-   例如，如果事实数据表存储客户地址，并且有一列用于国家/地区，则可能值的总数少于 200。 其中的某些值将重复多次。 如果事实数据表具有 1 亿行，则“国家/地区”列将轻松压缩，并需要很少的存储空间。 按行压缩就不能以这种方式利用列值的相似性，并在压缩“国家/地区”列中的值时将使用更多字节。    
    
### <a name="column-elimination"></a>列消除    
 列存储索引会跳过读取查询结果不需要的列。 这种功能（称为“列消除”）可进一步减少执行查询的 I/O，因此可以提高查询性能。    
    
-   列消除之所以可能是因为数据是按列组织和压缩的。 与此相反，当数据按行存储时，每行中的列值以物理方式存储在一起，并且不能轻松分离。 查询处理器要检索特定列值需要读取整个行，这会增加 I/O，因为会将不必要的额外数据读取到内存。    
    
-   例如，如果表有 50 列，而查询仅使用其中 5 列，列存储索引仅从磁盘中提取这 5 列。 它会跳过读取其他 45 列。 假定所有列都具有相似的大小，这可另外减少 I/O 90%。 如果相同的数据以行存储形式存储，查询处理器需要读取其他 45 列。    
    
### <a name="rowgroup-elimination"></a>行组消除    
 在全表扫描中，大部分数据通常不匹配查询谓词条件。 列存储索引通过使用元数据能够跳过读取不包含查询结果所需数据的行组，所有这些都不需要实际 I/O。 这种功能（称为“行组消除”）可减少全表扫描的 I/O，因此可以提高查询性能。    
    
 **列存储索引何时需要执行全表扫描？**    
    
 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，可以在聚集列存储索引上创建一个或多个常规非聚集 B 树索引，就像可以在行存储堆上创建一样。 非聚集 B 树索引可以加快具有相等谓词或包含小范围值的谓词的查询速度。 对于更复杂的谓词，查询优化器可以选择全表扫描。 如果没有跳过行组的功能，全表扫描会非常耗时，特别是对于大型表更是如此。    
    
 **分析查询何时从全表扫描的行组消除受益？**    
    
 例如，一个零售企业已使用具有聚集列存储索引的事实数据表来为其销售数据建模。 每条新的销售数据均存储交易的各个属性，包括产品销售日期。 有趣的是，尽管列存储索引不保证排序顺序，但此表中的行会按排序日期顺序加载。 随着时间的推移，此表将会增长。 虽然零售企业可能会保留过去 10 年的销售数据，但分析查询可能只需要计算上一季度的聚合。 列存储索引只需查看日期列的元数据就可避免访问前 39 个季度的数据。 这额外地减少了 97% 读入内存并进行处理的数据量。    
    
 **在全表扫描中跳过哪些行组？**    
    
 为了确定要消除哪些行组，列存储索引在每个行组中使用元数据来存储每个列段的最小值和最大值。 当列段范围不满足查询谓词条件时，就会跳过整个行组而不执行任何实际 IO。 这样做之所以有效是因为数据通常是按排序顺序加载的，虽然行不一定进行排序，但相似的数据值通常位于同一行组或相邻行组中。    
    
 有关行组的更多详细信息，请参阅[列存储索引设计指南](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)。    
    
### <a name="batch-mode-execution"></a>批处理模式执行    
 [批处理模式执行](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution)是指为提高执行效率将一组行（通常最多 900 行）一起处理。 例如，查询 `SELECT SUM (Sales) FROM SalesData` 从表 SalesData 聚合了总销售额。 以批处理模式执行时，查询执行引擎以 900 个值为一组计算聚合。 这样会将元数据（访问成本和其他类型的开销）分布到批处理的所有行中，而不是支付每行的成本，从而大大减少了代码路径。 批处理模式处理在可能的情况下会对压缩数据运行，并消除了行模式处理所用的一些交换运算符。 这可以将分析查询的执行速度提高好几个数量级。    
    
 并非所有查询执行运算符都可以在批处理模式下执行。 例如，DML 操作（如 Insert、Delete 或 Update）一次执行一行。 批处理模式运算符面向可提高查询性能的运算符（如 Scan、Join、Aggregate、sort 等）。 由于列存储索引是在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中引入的，因此需要持续扩展可以在批处理模式下执行的运算符。 下表按照产品版本显示了以批处理模式运行的运算符。    
    
|批处理模式运算符|何时使用此项？|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 和 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]<sup>1</sup>|注释|    
|---------------------------|------------------------|---------------------|---------------------|---------------------------------------|--------------|    
|DML 操作（insert、delete、update、merge）||否|否|否|DML 不是批处理模式操作，因为它不是并行的。 即使我们启用串行模式批处理操作，允许 DML 以批处理模式处理，我们也看不到明显的收益。|    
|columnstore index scan|扫描|NA|是|是|对于列存储索引，我们可以将谓词推送到 SCAN 节点。|    
|columnstore Index Scan（非聚集）|扫描|是|是|是|是|    
|index seek||NA|NA|否|我们以行模式通过非聚集 B 树索引执行查找操作。|    
|compute scalar|计算结果为标量值的表达式。|是|是|是|数据类型存在一些限制。 这适用于所有批处理模式运算符。|    
|串联 (concatenation)|UNION 和 UNION ALL|否|是|是||    
|filter|应用谓词|是|是|是||    
|hash match|基于哈希的聚合函数、外部哈希联接、右哈希联接、左哈希联接、右内部联接、左内部联接|是|是|是|聚合的限制：不能对字符串执行 min/max。 可用的聚合函数是 sum/count/avg/min/max。<br />联接的限制：不能对非整数类型执行任何不匹配的类型联接。|    
|merge join||否|否|否||    
|多线程查询||是|是|是||    
|嵌套循环||否|否|否||    
|单线程查询，在 MAXDOP 1 下运行||否|否|是||    
|带有串行查询计划的单线程查询||否|否|是||    
|sort|使用列存储索引的 SCAN 中的 Order by 子句。|否|否|是||    
|top sort||否|否|是||    
|window aggregates||NA|NA|是|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中的新运算符。|    
    
<sup>1</sup>适用于 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]、[!INCLUDE[ssSDS](../../includes/sssds-md.md)] 高级层、标准层（S3 及更高）和所有 vCore 层，以及 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]    

有关详细信息，请参阅[查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution)。
    
### <a name="aggregate-pushdown"></a>聚合下推    
 聚合计算的常规执行路径是从 SCAN 节点提取符合条件的行，然后以批处理模式聚合值。 尽管这提供了良好的性能，但使用 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]，可以将聚合运算推送到 SCAN 节点，以便在批处理模式执行的基础上将聚合计算的性能提高好几个数量级，前提是要满足以下条件： 
 
-    聚合为 `MIN`、`MAX`、`SUM`、`COUNT` 和 `COUNT(*)`。 
-  聚合运算符必须基于 SCAN 节点或包含 `GROUP BY` 的 SCAN 节点。
-  此聚合不是非重复聚合。
-  聚合列不是字符串列。
-  聚合列不是虚拟列。 
-  输入和输出数据类型必须是以下类型之一，并且必须适合 64 位。
    -  `tinyint`, `int`, `bigint`, `smallint`, `bit`
    -  精度 <= 18 的 `smallmoney`、`money`、`decimal` 和 `numeric`
    -  `smalldate`, `date`, `datetime`, `datetime2`, `time`
    
 通过在对缓存友好的执行中对压缩/编码数据进行高效聚合并利用 SIMD 可进一步加快聚合下推速度    
    
 ![聚合下推](../../relational-databases/indexes/media/aggregate-pushdown.jpg "聚合下推")    
    
例如，在以下两个查询中完成了聚合下推：    
    
```sql     
SELECT  productkey, SUM(TotalProductCost)    
FROM FactResellerSalesXL_CCI    
GROUP BY productkey;    
    
SELECT  SUM(TotalProductCost)    
FROM FactResellerSalesXL_CCI;    
```    
    
### <a name="string-predicate-pushdown"></a>字符串谓词下推    
在设计数据仓库架构时，建议的架构建模是使用星型架构或雪花型架构，其中包括一个或多个事实数据表和多个维度表。 [事实数据表](https://wikipedia.org/wiki/Fact_table) 存储业务度量值或事务，而 [维度表](https://wikipedia.org/wiki/Dimension_table) 存储分析事实数据需要跨越的维度。    
    
例如，事实可以是一条表示某一特定区域中某一特定产品的销售额的记录，而维度则表示一组区域、产品等。 事实数据表和维度表通过主键/外键关系进行连接。 最常用的分析查询将一个或多个维度表与事实数据表进行联接。    
    
让我们设想一个维度表 `Products`。 典型的主键是通常用字符串数据类型表示的 `ProductCode`。 为提高查询性能，最佳做法是创建代理键（通常为整数列），从事实数据表引用维度表中的行。 
    
列存储索引非常高效地运行具有联接/涉及数值的谓词或基于整数的键的分析查询。 但是，在很多客户工作负荷中，我们发现使用基于字符串的列链接事实/维度表，结果是使用列存储索引的查询性能并不如预期。 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 通过将包含字符串列的谓词下推到 SCAN 节点，使用基于字符串的列显著提高了分析的性能。    
    
字符串谓词下推利用为列创建的主/辅助字典来提高查询性能。 例如，让我们考虑在行组中创建一个包含 100 个不同字符串值的字符串列段。 假定有 100 万行，这意味着平均每个不同的字符串值被引用了 10,000 次。    
    
使用字符串谓词下推，执行查询时针对字典中的值计算谓词，如果它符合条件，引用字典值的所有行都将自动符合条件。 这在两个方面提高了性能：
1.  仅返回符合条件的行，从而减少了需要传递出 SCAN 节点的行数。 
2.  显著减少了字符串比较次数。 在此示例中，只需要 100 次字符串比较，而不用比较 100 万次。 如下所述，有一些限制：    

    -   不能对增量行组执行字符串谓词下推。 增量行组中的列没有字典。    
    -   如果字典大小超过 64 KB，则不能执行字符串谓词下推。    
    -   不支持计算结果为 NULL 的表达式。    
    
## <a name="see-also"></a>另请参阅    
 [列存储索引设计指南](../../relational-databases/sql-server-index-design-guide.md#columnstore_index) [列存储索引数据加载指南](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [开始使用列存储进行实时运行分析](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)     
 [针对数据仓库的列存储索引](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)    
 [列存储索引体系结构](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)    
 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)     
  
