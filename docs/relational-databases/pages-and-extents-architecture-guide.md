---
description: 页和区体系结构指南
title: 页和区体系结构指南 | Microsoft Docs
ms.custom: ''
ms.date: 03/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- page and extent architecture guide
- guide, page and extent architecture
ms.assetid: 83a4aa90-1c10-4de6-956b-7c3cd464c2d2
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 56bd6740a6b016bd06084b2e44958e61adc7ca89
ms.sourcegitcommit: fb8724fb99c46ecf3a6d7b02a743af9b590402f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92439391"
---
# <a name="pages-and-extents-architecture-guide"></a>页和区体系结构指南
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中数据存储的基本单位是页。 区是由八个物理上连续的页构成的集合。 区有助于有效管理页。 本指南介绍用于管理所有版本的 SQL Server 中的页和区的数据结构。 要设计和开发高效执行的数据库，了解页和区的体系结构是很重要的。

## <a name="pages-and-extents"></a>页和区

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中数据存储的基本单位是页。 为数据库中的数据文件（.mdf 或 .ndf）分配的磁盘空间可以从逻辑上划分成页（从 0 到 n 连续编号）。 磁盘 I/O 操作在页级执行。 也就是说，SQL Server 读取或写入所有数据页。

区是八个物理上连续的页的集合，用来有效地管理页。 所有页面都组织为盘区。

### <a name="pages"></a>页

与常规书籍做类比：常规书籍中的所有内容都是写在书页上的。 与书籍类似，SQL Server 所有数据行都写在页面上。 书中的所有页都具有相同的物理大小。 同样，SQL Server 所有数据页大小均相同 - 8 KB。 书中的大多数页都包含数据（书的主要内容），某些页面包含有关内容的元数据（例如目录和索引）。 SQL Server 也是如此：大多数页包含由用户存储的实际数据行；这些称为数据页面和文本/图像页面（在特殊情况下）。 索引页包含有关数据位置的索引引用，最后有一些系统页，它们存储有关数据组织的各种元数据（PFS、GAM、SGAM、IAM、DCM、BCM 页）。 请参阅下表了解页面类型及其说明。

如前所述，在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中，页的大小为 8-KB。 这意味着 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库中每 MB 有 128 页。 每页的开头是 96 字节的标头，用于存储有关页的系统信息。 此信息包括页码、页类型、页的可用空间以及拥有该页的对象的分配单元 ID。

下表说明了 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库的数据文件中所使用的页类型。

|页类型 | 目录 |
|-------|-------|
|数据 |当行中的文本设置为 ON 时，具有除 text、ntext、image、nvarchar(max)、varchar(max)、varbinary(max) 和 xml 数据以外的所有数据的数据行。 |
|索引 |索引条目。 |
|Text/Image |大型对象数据类型：（text、ntext、image、nvarchar(max)、varchar(max)、varbinary(max) 和 xml 数据） <br> 数据行超过 8 KB 时为可变长度数据类型列：（varchar、nvarchar、varbinary 和 sql_variant） |
|Global Allocation Map、Shared Global Allocation Map |有关区是否分配的信息。 |
|页可用空间 (PFS) |有关页分配和页的可用空间的信息。 |
|索引分配映射 (Index Allocation Map) |有关每个分配单元中表或索引所使用的区的信息。 |
|大容量更改映射表 |有关每个分配单元中自最后一条 BACKUP LOG 语句之后的大容量操作所修改的区的信息。 |
|差异更改映射表 |有关每个分配单元中自最后一条 BACKUP DATABASE 语句之后更改的区的信息。 |

> [!NOTE]
> 日志文件不包含页，而是包含一系列日志记录。

在数据页上，数据行紧接着标头按顺序放置。 页的末尾是行偏移表，对于页中的每一行，每个行偏移表都包含一个条目。 每个行偏移量条目记录对应行的第一个字节与页首的距离。 因此，行偏移量的功能有助于 SQL Server 快速在页面上定位行。 行偏移表中的条目的顺序与页中行的顺序相反。

![page_architecture](../relational-databases/media/page-architecture.gif)

#### <a name="large-row-support"></a>大型行支持  

行不能跨页，但是行的部分可以移出行所在的页，因此行实际可能非常大。 页的单个行中的最大数据量和开销是 8,060 字节 (8-KB)。 但是，这不包括用 Text/Image 页类型存储的数据。 

对于包含 varchar、nvarchar、varbinary 或 sql_variant 列的表，可以放宽此限制。 当表中的所有固定列和可变列的行的总大小超过限制的 8,060 字节时，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将从最大长度的列开始以动态方式将一个或多个可变长度列移动到 ROW_OVERFLOW_DATA 分配单元中的页。 

每当插入或更新操作将行的总大小增大到超过限制的 8,060 字节时，将会执行此操作。 将列移动到 ROW_OVERFLOW_DATA 分配单元中的页后，将在 IN_ROW_DATA 分配单元中的原始页上维护 24 字节的指针。 如果后续操作减小了行的大小，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 会动态将列移回到原始数据页。 

##### <a name="row-overflow-considerations"></a>行溢出注意事项 

如前所述，如果可变长度数据类型字段的组合大小超过 8060 字节的限制，则行不能驻留在多个页面上并且可能溢出。 举例说明，可以使用两列创建一个表：一个 varchar(7000)，另一个 varchar(2000)。 每列都不会超过 8060 字节，但如果每列的整个宽度被填充，它们会合并在一起，这就超过了该限制。 SQL Server 可以将 varchar(7000) 可变长列动态移动到 ROW_OVERFLOW_DATA 分配单元中的页面。 合并每行超过 8,060 字节的 varchar、nvarchar、varbinary、sql_variant 或 CLR 用户定义类型的列时，请注意下列事项：
-  如果更新操作使记录变长，大型记录将被动态移动到另一页。 如果更新操作使记录变短，记录可能会移回 IN_ROW_DATA 分配单元中的原始页。 执行查询和其他选择操作（例如，对包含行溢出数据的大型记录进行排序或合并）将延长处理时间，因为这些记录将同步处理，而不是异步处理。   
   因此，当要设计的表中包含多个 varchar、nvarchar、varbinary、sql_variant 或 CLR 用户定义类型的列时，请考虑可能溢出的行的百分比，以及可能查询这些溢出数据的频率。 如果可能需要经常查询行溢出数据中的许多行，请考虑对表格进行规范化处理，以使某些列移动到另一个表中。 然后可以在异步 JOIN 操作中执行查询。 
-  对于 varchar、nvarchar、varbinary、sql_variant 或 CLR 用户定义类型的列，单个列的长度仍然必须在 8,000 字节的限制之内。 只有它们的合并长度可以超过表的 8,060 字节的行限制。
-  其他数据类型列的和（包括 char 和 nchar 数据）必须在 8,060 字节的行限制之内。 大型对象数据也不受 8,060 字节行限制的制约。 
-  聚集索引的索引键不能包含在 ROW_OVERFLOW_DATA 分配单元中具有现有数据的 varchar 列。 如果对 varchar 列创建了聚集索引，并且 IN_ROW_DATA 分配单元中存在现有数据，则对该列执行的将数据推送到行外的后续插入或更新操作将会失败。 有关分配单元的详细信息，请参阅表和索引组织。
-  可以包括包含行溢出数据的列，作为非聚集索引的键列或非键列。
-  对于使用稀疏列的表，记录大小限制为 8,018 字节。 转换后的数据加上现有记录数据超过 8,018 字节时，会返回 [MSSQLSERVER ERROR 576](../relational-databases/errors-events/database-engine-events-and-errors.md)。 在稀疏和非稀疏类型之间转换列时，数据库引擎会保存当前记录数据的副本。 这样，记录所需的存储会临时加倍。
-  若要获得有关可能包含行溢出数据的表或索引的信息，请使用 [sys.dm_db_index_physical_stats](../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) 动态管理函数。

### <a name="extents"></a>Extents 

区是管理空间的基本单位。 一个区是八个物理上连续的页（即 64 KB）。 这意味着 SQL Server 数据库中每兆字节有 16 个区。

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 有两种盘区类型： 

* 统一盘区，由单个对象所有。盘区中的所有八页只能由所属对象使用。
* 混合盘区，最多可由八个对象共享。 区中八页的每页可由不同的对象所有。

![统一盘区和混合盘区](../relational-databases/media/extents.gif)

一直到（并且包括）[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 不会将所有盘区分配给包含少量数据的表。 新表或索引通常从混合区分配页。 当表或索引增长到 8 页时，将变成使用统一区进行后续分配。 如果对现有表创建索引，并且该表包含的行足以在索引中生成 8 页，则对该索引的所有分配都使用统一区进行。 

从 [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] 开始，用户数据库和 tempdb 中大多数分配的默认值都是使用统一盘区，但属于 [IAM 链](#IAM)的前八页的分配除外。 master、msdb 和 model 数据库的分配仍保留以前的行为。 

> [!NOTE]
> 一直到，并且包括 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)]，跟踪标志 1118 可用于将默认分配更改为始终使用统一区。 有关此跟踪标志的详细信息，请参阅 [DBCC TRACEON - 跟踪标志](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。   
>   
> 从 [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] 开始，将为 tempdb 和所有用户数据库自动启用 TF 1118 提供的功能。 对于用户数据库，此行为受 `ALTER DATABASE` 的 `SET MIXED_PAGE_ALLOCATION` 选项控制，同时默认值设置为禁用，且跟踪标志 1118 无效。 有关详细信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-set-options.md)。

从 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 开始，`sys.dm_db_database_page_allocations` 系统函数可以报告数据库、表、索引和分区的页分配信息。

> [!IMPORTANT]
> 未记录 `sys.dm_db_database_page_allocations` 系统函数，并且可能会发生更改。 不保证兼容性。 

从 [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] 开始，[sys.dm_db_page_info](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md) 系统函数可用，并返回有关数据库中的页的信息。 该函数将返回包含页中标头信息的一行，包括 object_id、index_id 和 partition_id。 在大多数情况下，此函数取代了使用 `DBCC PAGE` 的需要。

## <a name="managing-extent-allocations-and-free-space"></a>管理区分配和可用空间 

用来管理盘区分配情况并跟踪可用空间的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据结构相对而言比较简单。 该功能有以下优点： 

* 可用空间信息被紧密压缩，因此包含此信息的页相对较少。   
  这样，可提高速度，因为它减少了检索分配信息时所需的磁盘读取量。 同时还可增加分配页保留在内存中的机会并且不需要更多的读操作。 

* 大多数分配信息不是链在一起的。 这就简化了对分配信息的维护。    
  可以快速执行每个页的分配或释放。 这将减少需要分配页或释放页的并发任务之间的争用。 

### <a name="managing-extent-allocations"></a>管理区分配

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 使用两种类型的分配映射表来记录盘区的分配： 

- 全局分配映射表 (GAM)   
  GAM 页记录已分配的区。 每个 GAM 包含 64,000 个区，相当于近 4 GB 的数据。 GAM 用 1 个位来表示所涵盖区间内的每个区的状态。 如果位为 1，则区可用；如果位为 0，则区已分配。 

- 共享全局分配映射表 (SGAM)   
  SGAM 页记录当前用作混合区且至少有一个未使用的页的区。 每个 SGAM 包含 64,000 个区，相当于近 4-GB 的数据。 SGAM 用 1 个位来表示所涵盖区间内的每个区的状态。 如果位为 1，则区正用作混合区且有可用页。 如果位为 0，则区未用作混合区，或者虽然用作混合区但其所有页均在使用中。 

根据区当前的使用情况，GAM 和 SGAM 中每个区具有以下位模式。 

|区的当前使用情况 | GAM 位设置 | SGAM 位设置 |
|---------|----------|------| 
|可用，未使用 |1 |0 |
|统一区或已满的混合区 |0 |0 |
|具有可用页的混合区 |0 |1 |
 
这将简化区管理算法。 
-   若要分配统一区，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]将搜索 GAM 以查找为 1 的位并将其设置为 0。 
-   为了查找具有可用页的混合区，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]搜索 SGAM 以查找为 1 的位。 
-   若要分配混合区，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]在 GAM 中搜索为 1 的位，并将其设置为 0；然后将 SGAM 中的对应位设置为 1。 
-   若取消分配某个区，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]确保 GAM 位设置为 1，而 SGAM 位设置为 0。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 在内部实际使用的算法比本文介绍的内容更复杂，因为 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 在数据库中平均分布数据。 但是，由于无需管理区分配信息链，因此即使是实际算法也会被简化。

### <a name="tracking-free-space"></a>跟踪可用空间

“页可用空间 (PFS)”页记录每页的分配状态，是否已分配单个页以及每页的可用空间量。 PFS 对每页都有 1 个字节，记录该页是否已分配。如果已分配，则记录该页是为空、已满 1% 到 50%、已满 51% 到 80%、已满 81% 到 95% 还是已满 96% 到 100%。

将区分配给对象后，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]将使用 PFS 页来记录区中的哪些页已分配或哪些页可用。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]必须分配新页时，将使用此信息。 保留的页中的可用空间量仅用于堆和 Text/Image 页。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]必须找到一个具有可用空间的页来保存新插入的行时，使用此信息。 索引不要求跟踪页的可用空间，因为插入新行的点是由索引键值设置的。

它将在数据文件中按各自的区域间隔添加一个新的 PFS、GAM 或 SGAM 页面。 因此，将出现一个新的 PFS 页面，在第一个 PFS 页面之后为 8,088 页，在间隔 8,088 页后为另一个 PFS 页面。 举例说明，第 1 页为 PFS 页面，第 8088 页为 PFS 页面，第 16176 页为 PFS 页面，以此类推。 也将出现一个新的 GAM 页面，在第一个 GAM 页面之后为 64,000 区，在间隔 64,000 区后为另一个 GAM 页面，以此类推。 同样，将出现一个新的 SGAM 页面，在第一个 SGAM 页面之后为 64,000 区，在间隔 64,000 区后为另一个 SGAM 页面。 下图显示了[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]用来分配和管理区的页顺序。

![manage_extents](../relational-databases/media/manage-extents.gif)

## <a name="managing-space-used-by-objects"></a><a name="IAM"></a> 管理对象使用的空间 

“索引分配映射 (IAM)”页将映射分配单元使用的数据库文件中 4-GB 部分中的盘区。 分配单元有下列三种类型：

- IN_ROW_DATA   
    用于存储堆分区或索引分区。

- LOB_DATA   
   包含大型对象 (LOB) 数据类型，如 XML、VARBINARY(max) 和 VARCHAR(max)。

- ROW_OVERFLOW_DATA   
   包含超过 8,060 字节行大小限制的 VARCHAR、NVARCHAR、VARBINARY 或 SQL_VARIANT 列中存储的可变长度数据。 

堆或索引的每个分区至少包含一个 IN_ROW_DATA 分配单元。 根据堆或索引的架构，可能还包含一个 LOB_DATA 或 ROW_OVERFLOW_DATA 分配单元。

一个 IAM 页在文件中的范围为 4 GB，与 GAM 或 SGAM 页的范围相同。 如果分配单元包含来自多个文件的区，或者超过一个文件的 4 GB 范围，那么一个 IAM 链中将链接多个 IAM 页。 因此，每个分配单元在有区的每个文件中至少有一个 IAM 页。 如果分配给分配单元的文件中的区的范围超过了一个 IAM 页能够记录的范围，一个文件中也可能会有多个 IAM 页。 

![iam_pages](../relational-databases/media/iam-pages.gif)

IAM 页根据需要分配给每个分配单元，在文件中的位置也是随机的。 `sys.system_internals_allocation_units` 系统视图指向分配单元的第一个 IAM 页。 该分配单元的所有 IAM 页都链接到一个 IAM 链中。

> [!IMPORTANT]
> `sys.system_internals_allocation_units` 系统视图仅供内部使用，随时可能更改。 不保证兼容性。 此视图在 [!INCLUDE[ssSDSfull](../includes/sssdsfull-md.md)]中不可用。 

![iam_chain](../relational-databases/media/iam-chain.gif)
 
每个分配单元的链中所链接的 IAM 页 IAM 页有一个标头，指明 IAM 页所映射的区范围的起始区。 IAM 页中还有一个大位图，其中每个位代表一个区。 位图中的第一个位代表范围内的第一个区，第二个位代表第二个区，依此类推。 如果某个位是 0，它所代表的区将不会分配给拥有该 IAM 页的分配单元。 如果这个位是 1，它所代表的区将被分配给拥有该 IAM 页的分配单元。

当 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]必须在当前页中插入新行，而当前页中没有可用空间时，它将使用 IAM 和 PFS 页查找要将该行分配到的页，或者（对于堆或 Text/Image 页）查找具有足够空间容纳该行的页。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]使用 IAM 页查找分配给分配单元的区。 对于每个区，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]将搜索 PFS 页，以查看是否有可用的页。 每个 IAM 和 PFS 页覆盖大量数据页，因此一个数据库内只有很少的 IAM 和 PFS 页。 这意味着 IAM 和 PFS 页通常位于内存中的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 缓冲池中，所以能够很快找到它们。 对于索引，新行的插入点由索引键设置，但是当需要新页面时，将发生先前描述的过程。

仅当 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]不能在现有的区中快速找到足以容纳插入行的页时，才将新区分配给分配单元。 

<a name="ProportionalFill"></a>[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 使用“比例填充分配算法”从文件组的可用盘区中分配盘区。 如果同一文件组内有两个文件，而一个文件的可用空间是另一个文件的两倍，那么每从后一个文件分配一页，就从前一个文件分配两页。 这意味着文件组内的每个文件应该有近似的空间使用百分比。 

## <a name="tracking-modified-extents"></a>跟踪已修改的区 

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 使用两个内部数据结构跟踪被大容量复制操作修改的盘区，以及自上次完整备份后修改的盘区。 这些数据结构极大地加快了差异备份的速度。 当数据库使用大容量日志恢复模式时，这些数据结构也可以加快将大容量复制操作记录至日志的速度。 与全局分配图 (GAM) 和共享全局分配图 (SGAM) 页相同，这些结构也是位图，其中的每一位代表一个单独的区。 

- 差异更改映射表 (DCM)   
   这样便可跟踪自上次执行 `BACKUP DATABASE` 语句后更改过的盘区。 如果扩展盘区的位是 1，则自上次执行 `BACKUP DATABASE` 语句后扩展盘区已被修改。 如果位是 0，则扩展盘区没有被修改。 差异备份只读取 DCM 页便可以确定已修改的区。 这样大大减少了差异备份必须扫描的页数。 运行差异备份所需的时间与自上次执行 BACKUP DATABASE 语句之后修改的区数成正比，而不是与整个数据库的大小成正比。 

- 大容量更改映射表 (BCM)   
   跟踪自上次执行 `BACKUP LOG` 语句后，被大容量日志记录操作修改的盘区。 如果某个扩展盘区的位是 1，表明自上次执行 `BACKUP LOG` 语句后，该扩展盘区已经被有日志记录的大容量复制操作修改。 如果位是 0，则该扩展盘区未被有日志记录的大容量复制操作修改。 尽管所有数据库中都显示 BCM 页，但只有在数据库使用大容量日志记录恢复模式时，才会与 BCM 页有关。 在此恢复模式中，当执行 `BACKUP LOG` 时，备份进程将扫描 BCM 查找已经修改的盘区。 然后，将那些区包括在日志备份中。 如果数据库从数据库备份和一系列事务日志备份恢复，便可以恢复大容量日志记录操作。 在使用简单恢复模式的数据库中，BCM 页是不相关的，因为大容量日志记录操作不记入日志。 在使用完整恢复模式的数据库中，BCM 页同样不相关，因为该恢复模式将大容量日志记录操作视为有完整日志记录的操作。 

DCM 页和 BCM 页的间隔与 GAM 和 SGAM 页的间隔相同，都是 64,000 个区。 在物理文件中，DCM 和 BCM 页位于 GAM 和 SGAM 页之后。

![special_page_order](../relational-databases/media/special-page-order.gif)

## <a name="see-also"></a>另请参阅
[sys.allocation_units &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)     
[堆（没有聚集索引的表）](../relational-databases/indexes/heaps-tables-without-clustered-indexes.md#heap-structures)       
[sys.dm_db_page_info](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md)     
[读取页](../relational-databases/reading-pages.md)   
[写入页](../relational-databases/writing-pages.md)   
