---
title: 数据压缩 | Microsoft Docs
description: 使用 SQL Server 和 Azure SQL 数据库应用行和页面数据压缩，或列存储和列存储存档压缩。
ms.custom: ''
ms.date: 02/11/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- page compression [Database Engine]
- indexes [SQL Server], compressed
- compressed indexes [SQL Server]
- storage compression [Database Engine]
- tables [SQL Server], compressed
- storage [SQL Server], compressed
- compression [SQL Server]
- row compression [Database Engine]
- compression [SQL Server], about compressed tables and indexes
- data compression [Database Engine]
- compressed tables [SQL Server]
ms.assetid: 5f33e686-e115-4687-bd39-a00c48646513
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b8f28c1d2aaa3e1e1e431feaf265d7b1bc22f9d9
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525185"
---
# <a name="data-compression"></a>数据压缩

[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 和 [!INCLUDE [sssdsmifull-md](../../includes/sssdsmifull-md.md)] 支持针对行存储表和索引的行和页压缩，并且支持针对列存储表和索引的列存储和列存储存档压缩。  
  
 对于行存储表和索引，使用数据压缩功能可帮助减小数据库的大小。 除了节省空间之外，数据压缩还可以帮助提高 I/O 密集型工作负荷的性能，因为数据存储在更少的页中，查询需要从磁盘读取的页更少。 但是，在与应用程序交换数据时，在数据库服务器上需要额外的 CPU 资源来压缩和解压缩数据。 您可以在以下数据库对象上配置行和页压缩：   
-   存储为堆的整个表。  
-   存储为聚集索引的整个表。  
-   整个非聚集索引。  
-   整个索引视图。  
-   对于已分区表和已分区索引，可为每个分区配置压缩选项，且对象的各个分区的压缩设置不必相同。  
  
对于列存储表和索引，所有列存储表和索引始终使用列存储压缩，并且用户无法对此进行配置。 在您能够付出额外的时间和 CPU 资源来存储和检索数据的情况下，使用列存储存档压缩可进一步减少数据大小。  您可以在以下数据库对象上配置列存储存档压缩：  
-   整个列存储表或整个聚集列存储索引。  因为列存储表存储为聚集列存储索引，所有两种方法具有相同的结果。  
-   整个非聚集列存储索引。  
-   对于列存储表和列存储索引，可为每个分区配置存档压缩选项，且各个分区的存档压缩设置不必相同。  
  
> [!NOTE]  
> 此外，还可以使用 GZIP 算法格式来压缩数据。 这是一个附加步骤，最适合压缩部分数据时归档旧数据以进行长期存储。 无法为使用 `COMPRESS` 函数压缩的数据创建索引。 有关详细信息，请参阅 [COMPRESS (Transact-SQL)](../../t-sql/functions/compress-transact-sql.md)。  
  
## <a name="row-and-page-compression"></a>行压缩和页压缩

使用行压缩和页压缩时，应注意以下事项：  
-   在 Service Pack 或后续版本中，有关数据压缩的详细信息如有更改，恕不另行通知。
-   在 [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] 中提供压缩功能  
-   不是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的每个版本都提供压缩功能。 有关详细信息，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
-   压缩功能不可用于系统表。  
-   通过压缩可在一页上存储更多的行，但不会更改表或索引的最大行大小。  
-   当最大行大小加上压缩开销超过最大行大小 8060 个字节时，不能对表启用压缩功能。 例如，不能压缩具有 `c1 CHAR(8000)` 和 `c2 CHAR(53)` 列的表，因为存在额外的压缩开销。 当使用 vardecimal 存储格式时，会在启用此格式时执行行大小检查。 对于行压缩和页压缩，在最初压缩对象时会执行行大小检查，以后在每插入或修改一行时也都会执行这一检查。 压缩功能要求遵循下面两条规则：  
    -   固定长度类型的更新必须始终成功。  
    -   禁用数据压缩必须总是成功。 即使已压缩的行可以容纳在页面中（意味着它小于 8060 个字节）， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 也不允许对哪些在未压缩时无法容纳在行中的更新。  
-   当指定分区列表时，可以将各个分区的压缩类型设置为 ROW、PAGE 或 NONE。 如果未指定分区列表，将使用语句中指定的数据压缩属性来设置所有分区。 创建表或索引时，除非指定了其他压缩设置，否则数据压缩将设置为 NONE。 修改表时，除非指定了其他压缩设置，否则将保留现有压缩设置。  
-   如果指定分区列表或分区超出范围，将生成错误。  
-   非聚集索引不继承表的压缩属性。 若要压缩索引，必须显式设置索引的压缩属性。 默认情况下，创建索引时，索引的压缩设置会设置为 NONE。  
-   对堆创建聚集索引时，聚集索引会继承该堆的压缩状态，除非指定了另一压缩状态。  
-   如果堆配置为页级压缩，则只有在以下情况下，页才会进行页级压缩：  
    -   在启用大容量优化的情况下大容量导入数据。  
    -   数据是使用 `INSERT INTO ... WITH (TABLOCK)` 语法插入的，并且表没有非聚集索引。  
    -   表是通过执行带 PAGE 压缩选项的 `ALTER TABLE ... REBUILD` 语句重新生成的。  
-   通过 DML 操作被分配到堆中的新页面不会使用 PAGE 压缩，除非重新生成该堆。 重新生成堆的方法有：删除压缩然后重新应用压缩，或者创建聚集索引然后再删除聚集索引。  
-   若要更改堆的压缩设置，要求对表重新生成所有非聚集索引，以便它们具有指向堆中的新行位置的指针。  
-   可以联机或脱机启用或禁用 ROW 或 PAGE 压缩功能。 当执行联机操作时，对堆启用压缩功能是单线程的。  
-   启用或禁用行压缩或页压缩的磁盘空间要求与创建或重新生成索引时的磁盘空间要求相同。 对于已分区数据，可以通过每次对一个分区启用或禁用压缩功能来减少所需的空间。  
-   若要确定已分区表中分区的压缩状态，请查询 sys.partitions 目录视图的 data_compression 列。  
-   压缩索引时，可以使用行压缩和页压缩来压缩叶级页。 非叶级页不接收页压缩。  
-   由于大小的关系，大值数据类型有时不与普通行数据存储在一起，而是存储在特殊用途的页上。 对于单独存储的数据，数据压缩不可用。  
-   [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中实现 vardecimal 存储格式的表升级时保留该设置。 可以向具有 vardecimal 存储格式的表应用行压缩。 但是，因为行压缩是 vardecimal 存储格式的超集，所以不必保留 vardecimal 存储格式。 将 vardecimal 存储格式与行压缩一起使用时，十进制值不会进一步压缩。 可以向具有 vardecimal 存储格式的表应用页压缩；但是，vardecimal 存储格式列可能不会实现进一步的压缩。  
  
    > [!NOTE]  
    > [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 的所有受支持版本都支持 vardecimal 存储格式；但是，由于数据压缩可实现同样的目标，因此不推荐使用 vardecimal 存储格式。 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
## <a name="columnstore-and-columnstore-archive-compression"></a>列存储和列存储存档压缩  

列存储表和索引始终使用列存储压缩进行存储。 您可以通过配置称作存档压缩的附加压缩，进一步减少列存储数据的大小。  为了执行存档压缩， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将对数据运行 Microsoft XPRESS 压缩算法。 通过使用以下数据压缩类型添加或删除存档压缩：  
-   通过 **COLUMNSTORE_ARCHIVE** 数据压缩可使用存档压缩来压缩列存储数据。  
-   使用 **COLUMNSTORE** 数据压缩可对存档压缩执行解压缩。 这样生成的数据可以继续使用列存储压缩进行压缩。  
  
若要添加存档压缩，请使用具有 REBUILD 选项且 DATA COMPRESSION = COLUMNSTORE_ARCHIVE 的 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) 或者 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) 。  
  
例如：

```sql  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =  COLUMNSTORE_ARCHIVE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE_ARCHIVE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE_ARCHIVE ON PARTITIONS (2,4)) ;  
```  
  
若要删除存档压缩并且将数据还原为列存储压缩，请使用带有 REBUILD 选项并且 DATA COMPRESSION = COLUMNSTORE 的 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) 或者 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) 。  

例如：

```sql  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =  COLUMNSTORE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE ON PARTITIONS (2,4) ) ;  
```  
  
下一示例将数据压缩设置为对于某些分区是列存储，对于其他分区是列存储存档。  
  
```sql  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (  
    DATA_COMPRESSION =  COLUMNSTORE ON PARTITIONS (4,5),  
    DATA COMPRESSION = COLUMNSTORE_ARCHIVE ON PARTITIONS (1,2,3)  
) ;  
```  
  
### <a name="performance"></a>性能

 使用存档压缩对列存储索引进行压缩，导致索引执行速度慢于未进行存档压缩的列存储索引。 仅在您能够付出额外的时间和 CPU 资源来压缩和检索数据时，才使用存档压缩。  
  
 存档压缩的优点在于可减少存储，这对于不经常访问的数据很有用。 例如，如果您对每个月的数据都具有一个分区，并且您的大多数活动是针对最近月份的，则可以将较早月份的数据存档以便降低存储要求。  
  
### <a name="metadata"></a>元数据

下面的系统视图包含有关聚集索引的数据压缩的信息：  
-   [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) - **type** 和 **type_desc** 列包括 CLUSTERED COLUMNSTORE 和 NONCLUSTERED COLUMNSTORE。  
-   [sys.partitions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md) - data_compression 和 data_compression_desc 列包括 COLUMNSTORE 和 COLUMNSTORE_ARCHIVE 。  
  
过程 [sp_estimate_data_compression_savings &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) 也可应用于列存储索引。  
  
## <a name="impact-on-partitioned-tables-and-indexes"></a>对已分区表和索引的影响

 如果对已分区表和已分区索引使用数据压缩，则应注意以下事项：  
-   如果使用 `ALTER PARTITION` 语句拆分分区，则两个分区均继承原始分区的数据压缩属性。  
-   合并两个分区时，生成的分区将继承目标分区的数据压缩属性。  
-   若要切换分区，该分区的数据压缩属性必须与表的压缩属性匹配。  
-   可以使用两种语法变体来修改已分区表或已分区索引的压缩：  
    -   下面的语法仅重新生成被引用分区：  
    
        ```sql  
        ALTER TABLE <table_name>   
        REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =  <option>)  
        ```  
    
    -   下面的语法通过对未引用的任何分区使用现有压缩设置来重新生成整个表：  
    
        ```sql  
        ALTER TABLE <table_name>   
        REBUILD PARTITION = ALL   
        WITH (DATA_COMPRESSION = PAGE ON PARTITIONS(<range>),  
        ... )  
        ```  
  
     已分区索引使用 `ALTER INDEX` 遵循同样的原则。  
  
-   删除聚集索引时，除非修改了分区方案，否则相应的堆分区将保留其数据压缩设置。 如果分区方案已更改，则所有分区都将重新生成为未压缩状态。 若要删除聚集索引并更改分区方案，需要执行下列步骤：  
     1. 删除聚集索引。  
     2. 使用指定压缩选项的 `ALTER TABLE ... REBUILD` 选项来修改表。  
  
     若要删除聚集索引，脱机操作的执行速度很快，因为只删除较高级别的聚集索引。 如果联机删除聚集索引，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须重新生成堆两次，一次针对步骤 1，一次针对步骤 2。  
  
## <a name="how-compression-affects-replication"></a>压缩对复制的影响

如果将数据压缩与复制一起使用，则应注意以下事项：  
-   当快照代理生成初始架构脚本时，新架构对表及其索引采用相同的压缩设置。 不能仅对表启用压缩，而不对索引启用压缩。  
-   对于事务复制，项目架构选项决定了必须对哪些依赖对象和属性编写脚本。 有关详细信息，请参阅 [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)。  
     分发代理在应用脚本时，不对下级订阅服务器进行检查。 如果选择了压缩副本，则无法在下级订阅服务器上创建表。 在混合拓扑中，不启用压缩的复制。  
-   对于合并复制，发布兼容性级别会替代架构选项，并确定脚本化的架构对象。  
     在混合拓扑中，如果不是必须支持新的压缩选项，则发布兼容级别应设置为下级订阅服务器版本。 否则，应在创建表后在订阅服务器上压缩表。  
  
下表列出了在复制期间控制压缩的复制设置。  
  
|用户意图|为表或索引复制分区方案|复制压缩设置|脚本编写行为|  
|-----------------|-----------------------------------------------------|------------------------------------|------------------------|  
|复制分区方案并在该分区上的订阅服务器上启用压缩。|True|True|对分区方案和压缩设置均编写脚本。|  
|复制分区方案，但不压缩订阅服务器上的数据。|True|错误|对分区方案编写脚本，但不对分区的压缩设置编写脚本。|  
|不复制分区方案，也不压缩订阅服务器上的数据。|错误|False|不对分区和压缩设置编写脚本。|  
|如果发布服务器上的所有分区均压缩，则压缩订阅服务器上的表，但不复制分区方案。|False|True|检查是否对所有分区均启用了压缩。<br /><br /> 在表级别对压缩编写脚本。|  
  
## <a name="impact-on-other-sql-server-components"></a>对 其他 SQL Server 组件的影响

[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
   
 压缩发生在存储引擎中，数据以未压缩状态呈现给 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的其他大部分组件。 这决定了其他组件上的压缩效果仅限于以下方面：  
-   大容量导入和导出操作  
     导出数据时，即使采用本机格式，数据也以未压缩的行格式输出。 这会导致导出的数据文件的大小比源数据要大得多。  
     导入数据时，如果已对目标表启用压缩，则存储引擎会将数据转换为压缩的行格式。 这样所使用的 CPU 资源会比将数据导入未压缩表时使用的 CPU 资源多。  
     当使用页面压缩将数据批量导入堆时，批量导入操作尝试在插入数据时使用页面压缩来压缩数据。  
-   压缩对备份和还原没有影响。  
-   压缩对日志传送没有影响。  
-   数据压缩与稀疏列不兼容。 因此，无法压缩包含稀疏列的表，也不能将稀疏列添加到压缩表。  
-   启用压缩可以导致查询计划更改，因为数据是用不同的页数和每页不同的行数存储的。  
  
## <a name="see-also"></a>请参阅  
 [行压缩的实现](../../relational-databases/data-compression/row-compression-implementation.md)   
 [页压缩的实现](../../relational-databases/data-compression/page-compression-implementation.md)   
 [Unicode 压缩的实现](../../relational-databases/data-compression/unicode-compression-implementation.md)   
 [CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)  
  
  

