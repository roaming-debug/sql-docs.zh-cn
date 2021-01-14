---
description: 已分区表和已分区索引
title: 已分区表和已分区索引 | Microsoft Docs
ms.custom: ''
ms.date: 1/5/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- partitioned tables [SQL Server], about partitioned tables
- partitioned indexes [SQL Server], architecture
- partitioned tables [SQL Server], architecture
- partitioned indexes [SQL Server], about partitioned indexes
- index partitions
ms.assetid: cc5bf181-18a0-44d5-8bd7-8060d227c927
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dcc5d8e3602261c975f5517ea859e19fc7902936
ms.sourcegitcommit: 629229a7c33a3ed99db63b89127bb016449f7d3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2021
ms.locfileid: "97952046"
---
# <a name="partitioned-tables-and-indexes"></a>已分区表和已分区索引
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持表和索引分区。 已分区表和已分区索引的数据划分为可以选择分布于一个数据库中多个文件组的单元。 数据是按水平方式分区的，因此多组行映射到单个的分区。 单个索引或表的所有分区都必须位于同一个数据库中。 对数据进行查询或更新时，表或索引将被视为单个逻辑实体。 在 [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] SP1 之前，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的各版本中均不提供已分区的表和索引。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 各版本支持的功能列表，请参阅 [SQL Server 2016 的版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)。  
  
> [!IMPORTANT]  
> [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 在默认情况下支持多达 15,000 个分区。 在早于 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 的版本中，默认情况下，分区数限制为 1000 个。 在基于 x86 的系统上，可以创建具有超过 1000 个分区的表或索引，但不支持这样做。  
  
## <a name="benefits-of-partitioning"></a>分区的优点  
 通过对大型表或索引进行分区，可以具有以下可管理性和性能优点。  
  
-   可以快速、高效地传输或访问数据的子集，同时又能维护数据收集的完整性。 例如，将数据从 OLTP 加载到 OLAP 系统之类的操作仅需几秒钟即可完成，而如果不对数据进行分区，执行此操作需要几分钟或几小时。  
  
-   您可以更快地对一个或多个分区执行维护操作。 这些操作的效率更高，因为它们仅针对这些数据子集，而非整个表。 例如，您可以选择在一个或多个分区中压缩数据，或者重新生成索引的一个或多个分区。  
  
-   您可以根据经常执行的查询类型和硬件配置，提高查询性能。 例如，在分区依据列与表联接的列相同时，查询优化器可以更快地处理两个或多个已分区表之间的相等联接查询。 有关详细信息，请参阅下面的[查询](#queries)。
  
当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 针对 I/O 操作执行数据排序时，它会首先按分区对数据进行排序。 为了提高数据排序性能，可以通过设置 RAID 将多个磁盘中的分区数据文件条带化。 这样一来，尽管 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 仍按分区对数据进行排序，但它可以同时访问每个分区的所有驱动器。  
  
此外，您可以通过对在分区级别而不是整个表启用锁升级来提高性能。 这可以减少表上的锁争用。 若要通过允许对分区进行锁升级来减少锁争用，请将 `ALTER TABLE` 语句的 `LOCK_ESCALATION` 选项设置为“自动”。 

> [!TIP]
> 可以将表或索引的分区置于一个文件组（例如 `PRIMARY` 文件组）或多个文件组上。 使用分层存储时，可以使用多个文件组将特定分区分配给特定存储层。 所有其他分区权益均适用，无需考虑使用的文件组数量或特定文件组上的分区位置。
  
## <a name="components-and-concepts"></a>组件和概念  
以下术语适用于表和索引分区。  
  
### <a name="partition-function"></a>分区函数  
一种数据库对象，它定义如何根据某个列（称为分区列）的值将表或索引的行映射到一组分区。 分区列中的每个值都是分区函数的输入，该函数会返回分区值。 分区函数定义表将具有的分区数和分区边界。 例如，如果有一个包含销售订单数据的表，你可能想要基于“日期时间”  列（如销售日期）将表划分为 12 个（按月）分区。  
  
### <a name="partition-scheme"></a>分区方案 
将分区函数的分区映射到一组文件组的数据库对象。 在各个文件组上放置分区的主要原因是为了确保可以在分区上独立执行备份操作。 这是因为您可以在各个文件组上执行备份。  

> [!NOTE]
> Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 中仅支持主文件组。  
  
### <a name="partitioning-column"></a>分区列  
分区函数对表或索引进行分区时所使用的表或索引列。 参与分区函数的计算列必须显式标记为 PERSISTED。 用作索引列时有效的所有数据类型都可以用作分区依据列， **timestamp** 除外。 无法指定 **ntext**、 **text**、 **image**、 **xml**、 **varchar(max)** 、 **nvarchar(max)** 或 **varbinary(max)** 数据类型。 此外，无法指定 Microsoft .NET Framework 公共语言运行时 (CLR) 用户定义类型和别名数据类型列。  
  
### <a name="aligned-index"></a>对齐的索引  
与其对应的表建立在同一个分区方案之上的一种索引。 如果表与其索引对齐，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 则可以快速高效地切换分区，同时又能维护表及其索引的分区结构。 索引要与其基表对齐，并不需要与基表参与相同的命名分区函数。 但是，索引和基表的分区函数实质上必须是相同的，因为：
 1. 分区函数的参数具有相同的数据类型。
 2. 定义了相同数量的分区。
 3. 定义了相同的分区边界值。  

#### <a name="partitioning-clustered-indexes"></a>对聚集索引进行分区
对聚集索引进行分区时，聚集键必须包含分区依据列。 对非唯一的聚集索引进行分区时，如果未在聚集键中明确指定分区依据列，默认情况下 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在聚集索引键列表中添加分区依据列。 如果聚集索引是唯一的，则必须明确指定聚集索引键包含分区依据列。        

#### <a name="partitioning-nonclustered-indexes"></a>对非聚集索引进行分区
对唯一的非聚集索引进行分区时，索引键必须包含分区依据列。 对非唯一的非聚集索引进行分区时，默认情况下 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将分区依据列添加为索引的非键（包含性）列，以确保索引与基表对齐。 如果索引中已经存在分区依据列，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将不会向索引中添加分区依据列。 

### <a name="non-aligned-index"></a>非对齐索引  
独立于其相应的表进行分区的一种索引。 也就是说，索引具有不同的分区方案或者放置于不同于基表的单独文件组中。 在下列情况下，设计非对齐的分区索引可能会很有用：  
-   基表未分区。  
-   索引键是唯一的，不包含表的分区依据列。  
-   您希望基表与使用不同联接列的多个表一起参与并置联接。  

### <a name="partition-elimination"></a>分区排除
查询优化器用来仅访问相关分区以便满足查询的筛选条件的过程。  

## <a name="performance-guidelines"></a>性能准则  
 这个新的、更高的 15,000 个分区的限制将影响内存、分区的索引操作、DBCC 命令和查询。 本节介绍将分区数目增加到超过 1,000 个的性能影响并根据需要提供解决方法。 由于对分区最大数目的限制已增加到 15,000 个，因此您可以存储更长时间的数据。 不过，您应该仅保留所需时长的数据，并且在性能和分区数目之间保持平衡。  
  
### <a name="processor-cores-and-number-of-partitions-guidelines"></a>处理器核心和分区数指导方针  
 为了最大发挥并行操作的性能，建议将分区数设置为与处理器核心数相同，最大为 64（这是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以利用的最大并行处理器数）。  
  
### <a name="memory-usage-and-guidelines"></a>内存使用情况和指导方针  
 如果正在使用大量分区，我们建议您使用至少 16 GB 的 RAM。 如果系统没有足够的内存，则数据操作语言 (DML) 语句、数据定义语言 (DDL) 语句和其他操作可能会由于内存不足而失败。 如果系统具有 16 GB 的 RAM 并且运行许多大量占用内存的进程，则在运行大量分区的操作时，可能会出现内存不足的情况。 因此，您具有超过 16 GB 的内存越多，您遇到性能和内存问题的可能性就越低。  
  
 内存限制可能会影响 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成已分区索引的性能或能力。 如果表中已应用聚集索引，当索引未与其基表或聚集索引对齐时更是如此。 在这种情况下，可能有助于增加 index create memory 服务器配置选项。 有关详细信息，请参阅 [配置 index create memory 服务器配置选项](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md)。 
  
### <a name="partitioned-index-operations"></a>已分区索引操作  
对超过 1,000 个分区的表创建和重新生成非对齐索引是可能的，但不支持。 这样做可能会导致性能下降，或在执行这些操作的过程中占用过多内存。  
  
随着分区数目的增加，创建和重新生成对齐索引的执行时间可能会更长。 我们建议您不要同时运行多个创建和重新生成索引命令，因为可能会遇到性能和内存问题。  
  
 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 执行排序以生成已分区索引时，它首先为每个分区生成一个排序表。 然后在每个分区各自的文件组中生成排序表，或者在 tempdb  中生成排序表（如果指定了 SORT_IN_TEMPDB 索引选项）。 每个排序表都需要一个最小内存量才能生成。 在生成与其基表对齐的已分区索引时，将一次生成一个排序表，因此使用的内存较少。 但是，在生成非对齐的已分区索引时，将同时生成排序表。 因此，必须有足够的内存来处理这些并发的排序。 分区数越多，所需的内存越多。 每个分区的每个排序表的最小大小为 40 页，每页 8 KB。 例如，具有 100 个分区的非对齐已分区索引需要足够的内存才能同时连续地对 4,000 (40 * 100) 页进行排序。 如果有这么多的可用内存，生成操作将成功，但性能可能会降低。 如果没有这么多可用内存，生成操作将失败。 而具有 100 个分区的对齐已分区索引只需要具有对 40 页进行排序的内存就足够了，因为不会同时执行排序。  
  
 无论是对齐索引还是非对齐索引，如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对多处理器计算机上的生成操作应用了并行度，需要的内存可能会更多。 这是因为并行度越高，需要的内存就越多。 例如，如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将并行度设置为 4，那么具有 100 个分区的非对齐已分区索引将需要使四个处理器同时分别对 4,000 页（即，共 16,000 页）进行排序的足够内存。 如果已分区索引是对齐的，需要的内存将减少，只要够四个处理器分别对 40 页（共 160 页，即 4 * 40）进行排序就行了。 您可以使用 MAXDOP 索引选项手动降低并行度。  

### <a name="dbcc-commands"></a>DBCC 命令  
在具有较多分区的情况下，随着分区数目的增加，DBCC 命令可能需要更长的时间来执行。  
  
### <a name="queries"></a>查询  
与具有大量分区的查询相比，使用分区排除的查询在性能上相当或更高。 随着分区数目的增加，未使用分区排除的查询可能需要更长的时间来执行。  
  
例如，假定一个表具有 1 亿行和列的 `A`、 `B`和 `C`。 
-  在方案 1 中，该表在列 `A`上划分为 1000 个分区。 
-  在方案 2 中，该表在列 `A`上划分为 10,000 个分区。 针对该表的一个查询（该查询对列 `A` 具有 `WHERE` 子句筛选）将执行分区排除并且扫描一个分区。 同一个查询在方案 2 中的运行速度可能会更快，因为在分区中要扫描的行数更少。 对列 B 具有 `WHERE` 子句筛选的查询将扫描所有分区。 与在方案 2 中相比，该查询在方案 1 中的运行速度会更快，因为要扫描更少的分区。  

在分区列之外的其他列上使用运算符（如 TOP 或 MAX/MIN）的查询可能会遇到分区性能降低的情况，因为所有分区都必须进行评估。  

如果经常执行的查询涉及两个或多个已分区表之间的同等联接，则这些查询的分区依据列应该与联接表所基于的列相同。 另外，应当已并置了这些表或它们的索引。 这意味着它们要么使用同一命名分区函数，要么使用在以下方面基本相同的不同函数：
-  具有相同数量的用于进行分区的参数，对应的参数具有相同的数据类型。
-  定义了相同数量的分区。
-  定义了相同的分区边界值。
这样一来，查询优化器就可以更快地处理联接，因为分区可以自行联接。 如果查询联接的是联接字段未并置或未分区的两个表，则分区的出现实际上会降低而不是提高查询处理速度。

有关查询处理中的分区处理的详细信息，请参阅[关于已分区表和索引的查询处理增强功能](../../relational-databases/query-processing-architecture-guide.md#query-processing-enhancements-on-partitioned-tables-and-indexes)。

## <a name="behavior-changes-in-statistics-computation-during-partitioned-index-operations"></a>已分区索引操作期间统计信息计算中的行为更改  
 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]开始，当创建或重新生成已分区索引时，不会通过扫描表中的所有行来创建统计信息。 相反，查询优化器使用默认采样算法来生成统计信息。 在升级具有已分区索引的数据库后，您可以在直方图数据中注意到针对这些索引的差异。 此行为更改可能不会影响查询性能。 若要通过扫描表中所有行的方法获得有关已分区索引的统计信息，请使用 `CREATE STATISTICS` 或 `UPDATE STATISTICS` 以及 `FULLSCAN` 子句。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务|主题|  
|-|-|   
|说明如何创建分区函数和分区方案，然后如何将它们应用于表和索引。|[创建已分区表和索引](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md)|  
|||  
  
## <a name="related-content"></a>相关内容  
 对于已分区表和索引策略以及实现方式，以下白皮书可能对您很有用。  
-   [使用 SQL Server 2008 的已分区表和索引策略](https://msdn.microsoft.com/library/dd578580\(SQL.100\).aspx)    
-   [如何实现自动滑动窗口](https://msdn.microsoft.com/library/aa964122\(SQL.90\).aspx)    
-   [大容量加载到已分区表](/previous-versions/sql/sql-server-2005/administrator/cc966380(v=technet.10))    
-   [Project REAL:Data Lifecycle -- Partitioning](/previous-versions/sql/sql-server-2005/administrator/cc966424(v=technet.10))（Project REAL：数据生命周期 - 分区）    
-   [关于已分区表和索引的查询处理增强功能](/previous-versions/sql/sql-server-2008-r2/ms345599(v=sql.105))    
-   [生成大型关系数据仓库的 10 大最佳做法](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/SQLCAT's%20Guide%20to%20Relational%20Engine.pdf)（来自 _SQLCAT 的指南：关系工程_）
  
