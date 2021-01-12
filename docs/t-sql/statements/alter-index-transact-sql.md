---
description: ALTER INDEX (Transact-SQL)
title: ALTER INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER INDEX
- ALTER_INDEX_TSQL
dev_langs:
- t-sql
helpviewer_keywords:
- indexes [SQL Server], reorganizing
- ALTER INDEX statement
- indexes [SQL Server], disabling
- online index operations
- index reorganization [SQL Server]
- ALLOW_ROW_LOCKS option
- ALL keyword
- reorganizing indexes
- constraints [SQL Server], indexes
- row locks [SQL Server]
- index rebuilding [SQL Server]
- rebuilding indexes
- locking [SQL Server], indexes
- partitioned indexes [SQL Server], rebuilding
- defragmenting indexes
- disabling indexes
- XML indexes [SQL Server], modifying
- index modifications [SQL Server]
- indexes [SQL Server], modifying
- index options [SQL Server]
- modifying indexes
- index disabling [SQL Server]
- MAXDOP index option, ALTER INDEX statement
- spatial indexes [SQL Server], modifying
- indexes [SQL Server], options
- ALLOW_PAGE_LOCKS option
- page locks [SQL Server]
- index rebuild [SQL Server]
- index reorganize [SQL Server]
ms.assetid: b796c829-ef3a-405c-a784-48286d4fb2b9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7854c78f0643294b1b5111c1b6f2e0b0ef07afa6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099565"
---
# <a name="alter-index-transact-sql"></a>ALTER INDEX (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  通过禁用、重新生成或重新组织索引，或通过设置索引选项，修改现有的表索引或视图索引（行存储、列存储或 XML）。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
ALTER INDEX { index_name | ALL } ON <object>  
{  
      REBUILD {  
            [ PARTITION = ALL ] [ WITH ( <rebuild_index_option> [ ,...n ] ) ]   
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_index_option> ) [ ,...n ] ]  
      }  
    | DISABLE  
    | REORGANIZE  [ PARTITION = partition_number ] [ WITH ( <reorganize_option>  ) ]  
    | SET ( <set_index_option> [ ,...n ] )   
    | RESUME [WITH (<resumable_index_options>,[...n])]
    | PAUSE
    | ABORT
}  
[ ; ]  
  
<object> ::=   
{  
    { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
}  
  
<rebuild_index_option > ::=  
{  
      PAD_INDEX = { ON | OFF }  
    | FILLFACTOR = fillfactor   
    | SORT_IN_TEMPDB = { ON | OFF }  
    | IGNORE_DUP_KEY = { ON | OFF }  
    | STATISTICS_NORECOMPUTE = { ON | OFF }  
    | STATISTICS_INCREMENTAL = { ON | OFF }  
    | ONLINE = {   
          ON [ ( <low_priority_lock_wait> ) ]   
        | OFF } 
    | RESUMABLE = { ON | OFF } 
    | MAX_DURATION = <time> [MINUTES}     
    | ALLOW_ROW_LOCKS = { ON | OFF }  
    | ALLOW_PAGE_LOCKS = { ON | OFF }  
    | MAXDOP = max_degree_of_parallelism  
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }   
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]  
}  
  
<single_partition_rebuild_index_option> ::=  
{  
      SORT_IN_TEMPDB = { ON | OFF }  
    | MAXDOP = max_degree_of_parallelism  
    | RESUMABLE = { ON | OFF } 
    | MAX_DURATION = <time> [MINUTES}     
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE } }  
    | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<reorganize_option>::=  
{  
       LOB_COMPACTION = { ON | OFF }  
    |  COMPRESS_ALL_ROW_GROUPS =  { ON | OFF}  
}  
  
<set_index_option>::=  
{  
      ALLOW_ROW_LOCKS = { ON | OFF }  
    | ALLOW_PAGE_LOCKS = { ON | OFF }  
    | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
    | IGNORE_DUP_KEY = { ON | OFF }  
    | STATISTICS_NORECOMPUTE = { ON | OFF }  
    | COMPRESSION_DELAY= { 0 | delay [Minutes] }  
}  

<resumable_index_option> ::=
 { 
    MAXDOP = max_degree_of_parallelism
    | MAX_DURATION =<time> [MINUTES]
    | <low_priority_lock_wait>  
 }
 
<low_priority_lock_wait>::=  
{  
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ] ,   
                          ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )  
}  

```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse 
  
ALTER INDEX { index_name | ALL }  
    ON   [ schema_name. ] table_name  
{  
      REBUILD {  
            [ PARTITION = ALL [ WITH ( <rebuild_index_option> ) ] ] 
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_index_option> )] ] 
      }  
    | DISABLE  
    | REORGANIZE [ PARTITION = partition_number ]  
}  
[;]  

<rebuild_index_option > ::=   
{  
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]   
}

<single_partition_rebuild_index_option > ::=   
{  
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
}  
  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

 index_name  
 索引的名称。 索引名称在表或视图中必须唯一，但在数据库中不必唯一。 索引名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)的规则。  
  
 ALL  
 指定与表或视图相关联的所有索引，而不考虑是什么索引类型。 如果有一个或多个索引脱机或不允许对一个或多个索引类型执行只读文件组操作或指定操作，则指定 ALL 将导致语句失败。 下表列出了索引操作和不允许使用的索引类型。  
  
|将关键字 ALL 与此操作一起使用|如果表有一个或多个，语句会失败|  
|----------------------------------------|----------------------------------------|  
|REBUILD WITH ONLINE = ON|XML 索引<br /><br /> 空间索引<br /><br /> 列存储索引：**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|REBUILD PARTITION = *partition_number*|未分区的索引、XML 索引、空间索引或已禁用的索引|  
|REORGANIZE|ALLOW_PAGE_LOCKS 设置为 OFF 的索引。|  
|REORGANIZE PARTITION = *partition_number*|未分区的索引、XML 索引、空间索引或已禁用的索引|  
|IGNORE_DUP_KEY = ON|XML 索引<br /><br /> 空间索引<br /><br /> 列存储索引：**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|ONLINE = ON|XML 索引<br /><br /> 空间索引<br /><br /> 列存储索引：**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|RESUMABLE = ON| **All** 关键字不支持可恢复索引。 <br /><br /> **适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |   
  
> [!WARNING]
> 有关可以联机执行的索引操作的更详细信息，请参阅[联机索引操作准则](../../relational-databases/indexes/guidelines-for-online-index-operations.md)。

 如果将 PARTITION = partition_number 与 ALL 一起指定，则必须对齐所有索引。 这意味着，它们是基于等同的分区函数进行分区的。 将 ALL 与 PARTITION 一起使用可导致重新生成或重新组织所有具有相同 partition_number 的索引分区。 有关已分区索引的详细信息，请参阅 [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md)。  
  
 *database_name*  
 数据库的名称。  
  
 *schema_name*  
 表或视图所属架构的名称。  
  
 table_or_view_name  
 与该索引关联的表或视图的名称。 若要显示对象的索引报表，请使用 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 目录视图。  
  
 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]支持由三部分组成的名称格式 database_name.[schema_name].table_or_view_name，其中 database_name 为当前数据库，或 database_name 为 tempdb，table_or_view_name 以 # 开头。  
  
 REBUILD [ WITH (\<rebuild_index_option> [ ,... n]) ]   
  
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

指定将使用相同的列、索引类型、唯一性属性和排序顺序重新生成索引。 此子句等同于 [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md)。 REBUILD 启用已禁用的索引。 重新生成聚集索引并不重新生成关联的非聚集索引，除非指定了关键字 ALL。 如果未指定索引选项，则应用存储在 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 中的现有索引选项值。 对于未在 **sys.indexes** 中存储值的任何索引选项，应用该选项的参数定义中指示的默认值。  
  
 如果指定 ALL 且基础表为堆，则重新生成操作对表没有任何影响。 重新生成与表相关联的所有非聚集索引。  
  
 如果数据库恢复模式设置为大容量日志记录或简单日志记录，则可以对重新生成操作进行最小日志记录。  
  
> [!NOTE]
> 重新生成主 XML 索引时，基础用户表在索引操作持续期间不可用。  
 
对于列存储索引，重新生成操作：  
  
-  不使用排序顺序。  
-  在重新生成进行时获取表或分区上的排他锁。  数据是“处于脱机状态”，在重新生成期间不可用，即便使用 NOLOCK、RCSI 或 SI 也是如此。  
-  将所有数据重新压缩到列存储中。 在进行重新生成时存在列存储索引的两个副本。 在重新生成完成后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将删除原始列存储索引。  

有关详细信息，请参阅 [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)。 
  
PARTITION  

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 指定只重新生成或重新组织索引的一个分区。 如果 index_name 不是已分区索引，则不能指定 PARTITION。  
  
 PARTITION = ALL 重新生成所有分区。  
  
> [!WARNING]
> 对超过 1,000 个分区的表创建和重新生成非对齐索引是可能的，但不支持。 这样做可能会导致性能下降，或在执行这些操作的过程中占用过多内存。 Microsoft 建议，当分区数超过 1,000 时，只使用对齐索引。  
  
 *partition_number*  
   
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 要重新生成或重新组织已分区索引的分区数。 partition_number 是可以引用变量的常量表达式。 其中包括用户定义类型变量或函数以及用户定义函数，但不能引用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 partition_number 必须存在，否则，该语句将失败。  
  
 WITH (\<single_partition_rebuild_index_option>)   
   
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 `SORT_IN_TEMPDB`、`MAXDOP` 和 `DATA_COMPRESSION` 是可以在重新生成单个分区 (PARTITION = partition_number) 时指定的选项。 不能在单个分区重新生成操作中指定 XML 索引。  
  
 DISABLE  
 将索引标记为已禁用，从而不能由 [!INCLUDE[ssDE](../../includes/ssde-md.md)]使用。 可禁用任何索引。 已禁用的索引的索引定义保留在没有基础索引数据的系统目录中。 禁用聚集索引将阻止用户访问基础表数据。 若要启用索引，请使用 ALTER INDEX REBUILD 或 CREATE INDEX WITH DROP_EXISTING。 有关详细信息，请参阅 [禁用索引和约束](../../relational-databases/indexes/disable-indexes-and-constraints.md)和[启用索引和约束](../../relational-databases/indexes/enable-indexes-and-constraints.md)。  
  
 对行存储索引执行 REORGANIZE  
 对于行存储索引，REORGANIZE 指定要重新组织索引叶级别。 REORGANIZE 操作：  
  
-   始终联机执行。 这意味着不保留长期阻塞的表锁，且对基础表的查询或更新可以在 ALTER INDEX REORGANIZE 事务处理期间继续。
-   不允许用于禁用的索引。
-   在 ALLOW_PAGE_LOCKS 设置为 OFF 时不允许执行。
-   当在事务中执行而事务回滚时不会回滚。

> [!NOTE]
> 当 ALTER INDEX REORGANIZE 使用显式事务（例如，BEGIN TRAN ...COMMIT/ROLLBACK 中的 ALTER INDEX）而不是默认隐式事务模式时，REORGANIZE 的锁定行为会变得更加严格，这可能会导致阻塞。 有关隐式事务的详细信息，请参阅 [SET IMPLICIT_TRANSACTIONS (Transact-SQL)](../../t-sql/statements/set-implicit-transactions-transact-sql.md)。

有关详细信息，请参阅 [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)。 

REORGANIZE WITH **(** LOB_COMPACTION = { **ON** | OFF } **)**  
 适用于行存储索引。  
  
LOB_COMPACTION = ON  
  
-   指定要压缩包含以下这些大型对象 (LOB) 数据类型的数据的所有页面：图像、文本、ntext、varchar(max)、nvarchar(max)、varbinary(max) 和 xml。 压缩这些数据可以减少磁盘上的数据大小。  
-   对于聚集索引，这会压缩表中包含的所有 LOB 列。  
-   对于非聚集索引，这会压缩作为索引中非键（已包括）列的所有 LOB 列。  
-   REORGANIZE ALL 对所有索引执行 LOB_COMPACTION。 对于每个索引，这会压缩聚集索引、基础表中的所有 LOB 列 或是非聚集索引中的包含列。  
  
LOB_COMPACTION = OFF  
  
-   不压缩包含大型对象数据的页。  
-   OFF 对堆没有影响。  
  
 对列存储索引执行 REORGANIZE  
 对于列存储索引，REORGANIZE 将每个 CLOSED 增量行组作为压缩行组压缩到列存储中。 始终联机执行 REORGANIZE 操作。 这意味着不保留长期阻塞的表锁，且对基础表的查询或更新可以在 ALTER INDEX REORGANIZE 事务处理期间继续。 有关详细信息，请参阅 [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)。 
  
-   无需 REORGANIZE 即可将关闭的增量行组移动到压缩行组中。 后台 tuple-mover (TM) 进程会定期唤醒以压缩关闭的增量行组。 建议在 tuple-mover 落后时使用 REORGANIZE。 REORGANIZE 可以更主动地压缩行组。  
-   若要压缩所有 OPEN 和 CLOSED 行组，请参阅本部分中的 `REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS)` 选项。  
  
对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（自 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 起）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]中的列存储索引，REORGANIZE 联机执行以下额外的碎片整理优化：  
  
-   在逻辑删除了 10% 或更多行时从行组中物理移除行。 删除的字节会在物理媒体上进行回收。 例如，如果具有 100 万行的压缩行组删除了 10 万行，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会移除已删除的行，并使用 90 万行重新压缩行组。 它通过移除已删除的行来节省存储。  
  
-   合并一个或多个压缩行组，以将每个行组的行增加到最多 1,048,576 行。 例如，如果批量导入 5 批 102,400 行，则会获得 5 个压缩行组。 如果运行 REORGANIZE，则这些行组会合并为 1 个大小为 512,000 的压缩行组。 这假定不存在任何字典大小或内存限制。  
  
-   对于逻辑上已有 10% 或更多行被删除的行组，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会尝试将此行组与一个或多个行组合并。 例如，行组 1 使用 500,000 行进行压缩，行组 21 使用最大值 1,048,576 行进行压缩。 行组 21 删除了 60% 的行，剩下 409,830 行。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 倾向于合并这两个行组来压缩一个新行组，这个行组有 909,830 行。  
  
REORGANIZE WITH ( COMPRESS_ALL_ROW_GROUPS = { ON | **OFF** } )  
 适用于列存储索引。 

 **适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

COMPRESS_ALL_ROW_GROUPS 提供将打开或关闭的增量行组强制到列存储中的方式。 使用此选项时，无需重新生成列存储索引即可清空增量行组。  此操作与其他移除和合并碎片整理功能相结合，使得在大多数情况下不再需要重新生成索引。    

-   ON 将所有行组都强制到列存储中，而不考虑大小和状态（关闭或打开）。  
-   OFF 将所有关闭的行组强制到列存储中。  

有关详细信息，请参阅 [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)。 

SET ( \<set_index option> [ ,... n] )   
 指定不重新生成或重新组织索引的索引选项。 不能为已禁用的索引指定 SET。  
  
PAD_INDEX = { ON | OFF }  
   
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  

 指定索引填充。 默认为 OFF。  
  
 ON  
 FILLFACTOR 指定的可用空间百分比应用于索引的中间级页。 如果在 PAD_INDEX 设置为 ON 的同时不指定 FILLFACTOR，则使用 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 中存储的填充因子值。  
  
 OFF 或未指定 fillfactor  
 中间级页已填充到接近容量限制。 这样将至少为索引可以基于中间页中的键集拥有的最大大小的一行留出足够的空间。  
  
 有关详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
  
FILLFACTOR = fillfactor  
 
 **适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 指定一个百分比，指示在[!INCLUDE[ssDE](../../includes/ssde-md.md)]创建或修改索引的过程中，应将每个索引页面的叶级填充到什么程度。 fillfactor 必须是 1 到 100 之间的整数。 默认值为 0。 填充因子的值 0 和 100 在所有方面都是相同的。  
  
 显式的 FILLFACTOR 设置只是在索引首次创建或重新生成时应用。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]并不会在页中动态保持指定的可用空间百分比。 有关详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
  
 若要查看填充因子设置，请使用 **sys.indexes**。  
  
> [!IMPORTANT]
> 使用 FILLFACTOR 值创建或更改聚集索引会影响数据占用的存储空间量，因为[!INCLUDE[ssDE](../../includes/ssde-md.md)]在创建聚集索引时会再分发数据。  
  
 SORT_IN_TEMPDB = { ON | OFF }  
 
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 指定是否在 **tempdb** 中存储排序结果。 默认设置为 OFF，但“超大规模 Azure SQL 数据库”除外。 对于“超大规模”中的所有索引重新生成操作，无论指定什么选项，SORT_IN_TEMPDB 始终为 ON，除非使用可恢复索引重新生成。  
  
 ON  
 在 tempdb 中存储用于生成索引的中间排序结果。 如果 **tempdb** 位于不同于用户数据库的磁盘集中，这样可能会缩短创建索引所需的时间。 但是，这会增加索引生成期间所使用的磁盘空间量。  
  
 OFF  
 中间排序结果与索引存储在同一数据库中。  
  
 如果不需要执行排序操作，或者可以在内存中进行排序，则忽略 SORT_IN_TEMPDB 选项。  
  
 有关详细信息，请参阅[用于索引的 SORT_IN_TEMPDB 选项](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md)。  
  
 IGNORE_DUP_KEY **=** { ON | OFF }  
 指定在插入操作尝试向唯一索引插入重复键值时的错误响应。 IGNORE_DUP_KEY 选项仅适用于创建或重新生成索引后发生的插入操作。 默认为 OFF。  
  
 ON  
 向唯一索引插入重复键值时将出现警告消息。 只有违反唯一性约束的行才会失败。  
  
 OFF  
 向唯一索引插入重复键值时将出现错误消息。 整个 INSERT 操作将被回滚。  
  
 对于对视图上创建的索引、非唯一索引、XML 索引、空间索引以及筛选的索引，IGNORE_DUP_KEY 不能设置为 ON。  
  
 若要查看 IGNORE_DUP_KEY，请使用 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)。  
  
 在向后兼容的语法中，WITH IGNORE_DUP_KEY 等效于 WITH IGNORE_DUP_KEY = ON。  
  
 STATISTICS_NORECOMPUTE **=** { ON | OFF }  
 指定是否重新计算分布统计信息。 默认为 OFF。  
  
 ON  
 不会自动重新计算过时的统计信息。  
  
 OFF  
 启用统计信息自动更新功能。  
  
 若要恢复统计信息自动更新，请将 STATISTICS_NORECOMPUTE 设置为 OFF，或执行 UPDATE STATISTICS 但不包含 NORECOMPUTE 子句。  
  
> [!IMPORTANT]
> 如果禁用自动重新计算分发统计信息，可能会阻止查询优化器为涉及表的查询挑选最佳执行计划。  
  
STATISTICS_INCREMENTAL = { ON | OFF }  

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

为 ON 时，根据分区统计信息创建统计信息。 为 OFF 时，删除统计信息树并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 重新计算统计信息。 默认为 **OFF**。  
  
 如果不支持每个分区统计信息，将忽略该选项并生成警告。 对于以下统计信息类型，不支持增量统计信息：  
  
-   使用与基表不分区对齐的索引创建的统计信息  
-   对 AlwaysOn 可读辅助数据库创建的统计信息  
-   对只读数据库创建的统计信息  
-   对筛选的索引创建的统计信息  
-   对视图创建的统计信息  
-   对内部表创建的统计信息  
-   使用空间索引或 XML 索引创建的统计信息  
  
 ONLINE = { ON | OFF } \<as applies to rebuild_index_option>   
 指定在索引操作期间基础表和关联的索引是否可用于查询和数据修改操作。 默认为 OFF。  
  
 对于 XML 索引或空间索引，仅支持 `ONLINE = OFF`；如果 ONLINE 设置为 ON，便会引发错误。  
  
> [!IMPORTANT]
> 在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的各版本中均不提供联机索引操作。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 各版本支持的功能列表，请参阅 [[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 的版本和支持的功能](../../sql-server/editions-and-supported-features-for-sql-server-2016.md)和 [SQL Server 2017 的版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)。  
  
 ON  
 在索引操作期间不持有长期表锁。 在索引操作的主要阶段，源表上只使用意向共享 (IS) 锁。 这样，即可继续对基础表和索引进行查询或更新。 操作开始时，将对源对象保持极短时间的共享 (S) 锁。 操作结束时，如果创建非聚集索引，将对源持有极短时间的 S 锁；当联机创建或删除聚集索引时，或者重新生成聚集或非聚集索引时，将获取 SCH-M（架构修改）锁。 对本地临时表创建索引时，ONLINE 不能设置为 ON。  
  
 OFF  
 在索引操作期间应用表锁。 创建、重新生成或删除聚集索引、空间索引或 XML 索引或者重新生成或删除非聚集索引的脱机索引操作将获得对表的架构修改 (Sch-M) 锁。 这样可以防止所有用户在操作期间访问基础表。 创建非聚集索引的脱机索引操作将对表获取共享 (S) 锁。 这样可以防止更新基础表，但允许读操作（如 SELECT 语句）。  
  
有关详细信息，请参阅 [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md)。  
  
可以联机重新生成索引（包括全局临时表中的索引），但以下情况除外：
- XML 索引
- 对本地临时表的索引
- 视图的初始唯一聚集索引
- 列存储索引
- 聚集索引，前提是基础表包含 LOB 数据类型（image、ntext、text）和空间数据类型
- varchar(max) 和 varbinary(max) 列不能是索引的一部分 。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（自 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 起）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，当表包含 varchar(max) 或 varbinary(max) 列时，可以使用 ONLINE 选项生成或重新生成包含其他列的聚集索引。 当基表包含 varchar(max) 或 varbinary(max) 列时，[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 不允许使用 ONLINE 选项

有关详细信息，请参阅[联机索引操作的工作方式](../../relational-databases/indexes/how-online-index-operations-work.md)。

RESUMABLE **=** { ON | **OFF**}

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   

 指定联机索引操作是否可恢复。

 ON 索引操作可恢复。

 OFF 索引操作不可恢复。

MAX_DURATION **=** *time* [**MINUTES**]，与 **RESUMABLE = ON** 一起使用（要求 **ONLINE = ON**）。

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

指示可恢复联机索引操作在暂停之前执行的时间（以分钟为单位指定的整数值）。 

> [!IMPORTANT]
> 有关可以联机执行的索引操作的更详细信息，请参阅[联机索引操作准则](../../relational-databases/indexes/guidelines-for-online-index-operations.md)。

> [!NOTE] 
> 列存储索引不支持可恢复联机索引重新生成。

ALLOW_ROW_LOCKS = { ON | OFF }   

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 指定是否允许行锁。 默认值为 ON。  
  
 ON  
 在访问索引时允许使用行锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]确定何时使用行锁。  
  
 OFF  
 不使用行锁。  
  
ALLOW_PAGE_LOCKS = { ON | OFF }   
  
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 指定是否允许使用页锁。 默认值为 ON。  
  
 ON  
 访问索引时允许使用页锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]确定何时使用页锁。  
  
 OFF  
 不使用页锁。  
  
> [!NOTE]
> ALLOW_PAGE_LOCKS 设置为 OFF 时，无法重新组织索引。  

 OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

指定是否针对最后一页插入争用进行优化。 默认为 OFF。 有关详细信息，请参阅“CREATE INDEX”页的[顺序键](./create-index-transact-sql.md#sequential-keys)部分。

 MAXDOP = max_degree_of_parallelism  
 
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 在索引操作期间替代 max degree of parallelism 配置选项。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。 使用 MAXDOP 可以限制在执行并行计划的过程中使用的处理器数量。 最大数量为 64 个处理器。  
  
> [!IMPORTANT]
>  虽然所有 XML 索引在语法上都支持 MAXDOP 选项，但对于空间索引或主 XML 索引，ALTER INDEX 当前只使用一个处理器。  
  
 max_degree_of_parallelism 可以是：  
  
 1  
 取消生成并行计划。  
  
 \>1  
 将并行索引操作中使用的最大处理器数量限制为指定数量。  
  
 0（默认值）  
 根据当前系统工作负荷使用实际的处理器数量或更少数量的处理器。  
  
 有关详细信息，请参阅 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)。  
  
> [!NOTE]
> 并非在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的每个版本中均提供并行索引操作。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 各版本支持的功能列表，请参阅 [SQL Server 2016 的版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)。  
  
COMPRESSION_DELAY = { 0 |duration [Minutes] }   

**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）  
  
 对于基于磁盘的表，延迟指定增量行组中处于关闭状态的增量行组在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以将它压缩为压缩行组之前必须保持为增量行组的最小分钟数。 由于基于磁盘的表不对单个行跟踪插入和更新时间，因此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将此延迟应用于处于关闭状态的增量行组。  
  
 默认为 0 分钟。  
  
 有关何时使用 COMPRESSION_DELAY 的建议，请参阅[开始使用列存储进行实时运行分析](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)。  
  
 DATA_COMPRESSION  
 为指定的索引、分区号或分区范围指定数据压缩选项。 选项如下：  
  
 无  
 不压缩索引或指定的分区。 这不适用于列存储索引。  
  
 ROW  
 使用行压缩来压缩索引或指定的分区。 这不适用于列存储索引。  
  
 PAGE  
 使用页压缩来压缩索引或指定的分区。 这不适用于列存储索引。  
  
 COLUMNSTORE  
   
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 仅适用于列存储索引，包括非聚集列存储索引和聚集列存储索引。 COLUMNSTORE 指定对使用 COLUMNSTORE_ARCHIVE 选项压缩的索引或指定分区进行解压缩。 在还原数据时，将继续通过用于所有列存储索引的列存储压缩对数据进行压缩。  
  
 COLUMNSTORE_ARCHIVE  
  
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 仅适用于列存储索引，包括非聚集列存储索引和聚集列存储索引。 COLUMNSTORE_ARCHIVE 会进一步将指定分区压缩为更小的大小。 这可用于存档，或者用于要求更小存储大小并且可以付出更多时间来进行存储和检索的其他情形。  
  
 有关压缩的详细信息，请参阅[数据压缩](../../relational-databases/data-compression/data-compression.md)。  
  
 ON PARTITIONS ( { \<partition_number_expression> | \<range> } [,...n] )    
    
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 指定对其应用 DATA_COMPRESSION 设置的分区。 如果索引未分区，则 ON PARTITIONS 参数将产生错误。 如果不提供 ON PARTITIONS 子句，则 DATA_COMPRESSION 选项将应用于分区索引的所有分区。  
  
 可以通过下列方式指定 \<partition_number_expression>：  
  
-   提供分区号（例如：`ON PARTITIONS (2)`）。  
-   提供多个单独分区的分区号，并用逗号分隔（例如：`ON PARTITIONS (1, 5)`）。  
-   提供范围和各个分区：`ON PARTITIONS (2, 4, 6 TO 8)`。  
  
 可以将 \<range> 指定为由单词 TO 隔开的分区号，例如：`ON PARTITIONS (6 TO 8)`。  
  
 若要为不同分区设置不同的数据压缩类型，请多次指定 DATA_COMPRESSION 选项，例如：  
  
```sql  
REBUILD WITH   
(  
DATA_COMPRESSION = NONE ON PARTITIONS (1),   
DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),   
DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)  
);  
```  
  
 ONLINE = { ON  | OFF } \<as applies to single_partition_rebuild_index_option>   
 指定索引或基础表的索引分区是否可以联机或脱机重新生成。 如果 **REBUILD** 联机执行 (**ON**)，则索引操作期间可以用此表中的数据进行查询和修改数据。  默认为 **OFF**。  
  
 ON  
 在索引操作期间不持有长期表锁。 在索引操作的主要阶段，源表上只使用意向共享 (IS) 锁。 索引重新生成开始时表上需要一个 S 锁，联机重新生成索引结束时表上需要一个 Sch-M 锁。 不过两个锁都是短的元数据锁，特别是 Sch-M 锁必须等待所有阻塞事务完成。 在等待期间，Sch-M 锁在访问同一表时阻止在此锁后等待的所有其他事务。  
  
> [!NOTE]
>  联机索引重新生成可以设置本节稍后介绍的 low_priority_lock_wait 选项。  
  
 OFF  
 在索引操作期间应用表锁。 这样可以防止所有用户在操作期间访问基础表。  
  
 WAIT_AT_LOW_PRIORITY，仅与 **ONLINE=ON** 一起使用。  
 
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 联机索引重新生成必须等待对此表执行的阻塞操作。 **WAIT_AT_LOW_PRIORITY** 表示联机索引重新生成操作将等待低优先级锁，从而允许其他操作在该联机索引生成操作正在等待时继续进行。 省略 WAIT AT LOW PRIORITY 选项等效于 WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)。 有关详细信息，请参阅 [WAIT_AT_LOW_PRIORITY](alter-index-transact-sql.md)。 
  
 MAX_DURATION = time [MINUTES]  
  
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 联机索引重新生成锁将在执行 DDL 命令时以低优先级等待的等待时间（以分钟为单位指定的整数值）。 如果操作被阻塞 MAX_DURATION 时间，则将执行某一 ABORT_AFTER_WAIT 操作 。 MAX_DURATION 时间始终以分钟为单位，MINUTES 一词可以省略 。  
 
 ABORT_AFTER_WAIT = [NONE | SELF | BLOCKERS } ]    
   
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 无  
 继续以普通（常规）优先级等待锁。  
  
 SELF  
 不采取任何操作，直接退出当前执行的联机索引重新生成 DDL 操作。  
  
 BLOCKERS  
 终止阻塞联机索引重新生成 DDL 操作的所有用户事务以使操作可以继续。 **BLOCKERS** 选项要求登录名拥有 **ALTER ANY CONNECTION** 权限。  
 
 RESUME 
 
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  

恢复已手动或由于失败而暂停的索引操作。

MAX_DURATION，与 **RESUMABLE=ON** 一起使用
 
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

恢复后执行可恢复联机索引操作的时间（以分钟为单位指定的整数值）。 该时间过后，如果可恢复操作仍在运行，则它会暂停。

WAIT_AT_LOW_PRIORITY，与 **RESUMABLE=ON** 和 **ONLINE = ON** 一起使用。  
  
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 在暂停之后恢复联机索引必须等待对此表执行的阻塞操作。 **WAIT_AT_LOW_PRIORITY** 表示联机索引重新生成操作将等待低优先级锁，从而允许其他操作在该联机索引生成操作正在等待时继续进行。 省略 WAIT AT LOW PRIORITY 选项等效于 WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)。 有关详细信息，请参阅 [WAIT_AT_LOW_PRIORITY](alter-index-transact-sql.md)。 

PAUSE
 
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
暂停可恢复联机索引重新生成操作。

ABORT

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   

中止已声明为可恢复的正在运行或已暂停的索引操作。 必须显式执行 **ABORT** 命令才能终止可恢复索引重新生成操作。 失败或暂停可恢复索引操作不会终止其执行；而是将操作停留在无限期暂停状态。
  
## <a name="remarks"></a>备注  
ALTER INDEX 不能用于对索引重新分区或将索引移到其他文件组。 此语句不能用于修改索引定义，如添加或删除列，或更改列的顺序。 使用带有 DROP_EXISTING 子句的 CREATE INDEX 执行这些操作。  
  
未显式指定选项时，则应用当前设置。 例如，如果未在 REBUILD 子句中指定 FILLFACTOR 设置，将在重新生成过程中使用系统目录中存储的填充因子值。 若要查看当前索引选项设置，请使用 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)。  
  
`ONLINE`、`MAXDOP` 和 `SORT_IN_TEMPDB` 的值不存储在系统目录中。 除非在索引语句中指定，否则，将使用选项的默认值。
  
在多处理器计算机中，就像其他查询那样，`ALTER INDEX REBUILD` 自动使用更多处理器来执行与修改索引相关联的扫描和排序操作。 运行 `ALTER INDEX REORGANIZE` 时，无论是否有 LOB_COMPACTION，“最大并行度”值都是单线程操作。 有关详细信息，请参阅 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)。  
  
> [!IMPORTANT]
> 如果索引所在的文件组脱机或设置为只读，则无法重新组织或重新生成索引。 如果指定了关键字 ALL，但有一个或多个索引位于脱机文件组或只读文件组中，该语句将失败。  
  
## <a name="rebuilding-indexes"></a><a name="rebuilding-indexes"></a> 重新生成索引  
重新生成索引将会删除并重新创建索引。 这将根据指定的或现有的填充因子设置压缩页来删除碎片、回收磁盘空间，然后对连续页中的索引行重新排序。 如果指定 ALL，将删除表中的所有索引，然后在单个事务中重新生成。 不必预先删除外键约束。 重新生成具有 128 个区或更多区的索引时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]延迟实际的页释放及其关联的锁，直到事务提交。  
 
有关详细信息，请参阅 [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)。 
  
## <a name="reorganizing-indexes"></a><a name="reorganizing-indexes"></a> 重新组织索引
使用最少系统资源重新组织索引。 通过对叶级页以物理方式重新排序，使之与叶节点的从左到右的逻辑顺序相匹配，进而对表和视图中的聚集索引和非聚集索引的叶级进行碎片整理。 重新组织还会压缩索引页。 压缩基于现有的填充因子值。 
  
如果指定了 `ALL`，将重新组织表中的关系索引（包括聚集索引和非聚集索引）和 XML 索引。 指定 ALL 时应用某些限制，请参阅本文“参数”部分的 ALL 定义。  
  
有关详细信息，请参阅 [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)。  

> [!IMPORTANT]
> 对于具有有序聚合列存储索引的 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 表，`ALTER INDEX REORGANIZE` 不会对数据重新排序。 要对数据重新排序，可使用 `ALTER INDEX REBUILD`。
  
## <a name="disabling-indexes"></a><a name="disabling-indexes"></a> 禁用索引  
禁用索引可防止用户访问该索引，对于聚集索引，还可防止用户访问基础表数据。 索引定义保留在系统目录中。 对视图禁用非聚集索引或聚集索引会以物理方式删除索引数据。 禁用聚集索引将阻止对数据的访问，但在删除或重新生成索引之前，数据在 B 树中一直保持未维护的状态。 若要查看已启用索引或已禁用的索引的状态，请查询 **sys.indexes** 目录视图中的 **is_disabled** 列。  
  
如果表位于事务复制发布中，则无法禁用任何与主键列关联的索引。 复制需要使用这些索引。 若要禁用索引，必须先从发布中删除该表。 有关详细信息，请参阅[发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)。  
  
使用 `ALTER INDEX REBUILD` 语句或 `CREATE INDEX WITH DROP_EXISTING` 语句来启用索引。 重新生成已禁用聚集索引不能在 ONLINE 选项设置为 ON 时执行。 有关详细信息，请参阅 [禁用索引和约束](../../relational-databases/indexes/disable-indexes-and-constraints.md)。  
  
## <a name="setting-options"></a>“设置选项”  
可以为指定索引设置选项 `ALLOW_ROW_LOCKS`、`ALLOW_PAGE_LOCKS`、`OPTIMIZE_FOR_SEQUENTIAL_KEY`、`IGNORE_DUP_KEY` 和 `STATISTICS_NORECOMPUTE`，而无需重新生成或重新组织相应索引。 修改的值立即应用于索引。 若要查看这些设置，请使用 **sys.indexes**。 有关详细信息，请参阅 [设置索引选项](../../relational-databases/indexes/set-index-options.md)。  
  
### <a name="row-and-page-locks-options"></a>行锁和页锁选项  
如果 `ALLOW_ROW_LOCKS = ON` 并且 `ALLOW_PAGE_LOCK = ON`，则当访问索引时将允许行级别、页级别和表级别的锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]将选择相应的锁，并且可以将锁从行锁或页锁升级到表锁。  
  
如果 `ALLOW_ROW_LOCKS = OFF` 并且 `ALLOW_PAGE_LOCK = OFF`，则当访问索引时将仅允许表级别的锁。  
  
设置行锁或页锁选项时，如果指定 ALL，这些设置将应用于所有索引。 基础表为堆时，通过以下方式应用这些设置：  
  
|选项|详细信息|
|------|-------|
|ALLOW_ROW_LOCKS = ON 或 OFF|应用于堆和任何关联的非聚集索引。|  
|ALLOW_PAGE_LOCKS = ON|应用于堆和任何关联的非聚集索引。|  
|ALLOW_PAGE_LOCKS = OFF|完全针对非聚集索引。 这意味着不允许对非聚集索引使用所有页锁。 在堆中，仅不允许使用有页的共享 (S) 锁、更新 (U) 锁和排他 (X) 锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]仍然可以获取意向页锁（IS、IU 或 IX），供内部使用。|  
  
## <a name="online-index-operations"></a><a name="online-index-operations"></a> 联机索引操作  
重新生成索引且 ONLINE 选项设置为 ON 时，基础对象、表和关联的索引均可用于查询和数据修改。 您也可以联机重新生成单个分区上某索引的一部分。 更改过程中，排他表锁只保留非常短的时间。  
  
重新组织索引始终联机执行。 该进程不长期保留锁，因此，不阻塞正在运行的查询或更新。  
  
只有在执行以下操作时，才能对同一个表或表部分执行并发联机索引操作：  
  
-   创建多个非聚集索引。  
-   在同一个表中重新组织不同索引。  
-   在同一个表中重新生成不重叠的索引时，重新组织不同的索引。  
  
同一时间执行的所有其他联机索引操作都将失败。 例如，您不能在同一个表中同时重新生成两个索引或更多索引，也不能在同一个表中重新生成现有索引时创建新的索引。  

### <a name="resumable-index-operations"></a><a name="resumable-indexes"></a> 可恢复索引操作

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Online index rebuild 可使用 RESUMABLE = ON 选项指定为可恢复。 
-  RESUMABLE 选项对于给定索引在元数据不持久，并且仅适用于当前 DDL 语句的持续时间。 因此，必须显式指定 RESUMABLE = ON 子句才能启用可恢复性。
-  RESUMABLE = ON 选项或 low_priority_lock_wait 参数选项支持 MAX_DURATION 选项。 
   -  用于 RESUMABLE 选项的 MAX_DURATION 为重新生成的索引指定时间间隔。 使用此时间之后，索引重新生成会暂停或完成其执行。 由用户确定何时可以恢复暂停的索引的重新生成。 MAX_DURATION 时间（以分钟为单位）必须大于 0 分钟，且小于等于一周（7 \* 24 \* 60 = 10080 分钟）。 让索引操作长时间暂停可能会影响特定表的 DML 性能以及数据库磁盘容量，因为原始索引和新创建的索引需要磁盘空间并且需要在 DML 操作期间更新。 如果省略 MAX_DURATION 选项，则索引操作会继续，直到其完成或发生失败。 
   -  通过 \<low_priority_lock_wait> 参数选项可以确定在 SCH-M 锁上阻塞时，索引操作如何才能继续。
 
-  使用相同参数重新执行原始 ALTER INDEX REBUILD 语句会恢复暂停的索引重新生成操作。 还可以通过执行 ALTER INDEX RESUME 语句来恢复暂停的索引重新生成操作。
-  可恢复索引不支持 SORT_IN_TEMPDB=ON 选项 
-  无法在显式事务（不能属于 tran ... commit 块）中执行具有“RESUMEABLE = ON”的 DDL 命令。
-  只有暂停的索引操作才可恢复。
-  恢复暂停的索引操作时，可以将 MAXDOP 值更改为新值。  如果在恢复暂停的索引操作时未指定 MAXDOP，则采用最后一个 MAXDOP 值。 如果对于索引重新生成操作完全未指定 MAXDOP 选项，则采用默认值。
- 若要立即暂停索引操作，则可以停止正在进行的命令 (CTRL-C)，也可以执行 ALTER INDEX PAUSE 命令或 KILL session_id 命令。 暂停命令之后，可以使用 RESUME 选项恢复它。
-  ABORT 命令可终止承载原始索引重新生成的会话，并中止索引操作  
-  除了以下情况，可恢复索引重新生成无需额外资源
   -    使索引保持生成所需的附加空间，包括索引暂停的时间
   -    阻止任何 DDL 修改的 DDL 状态
-  虚影清除会在索引暂停阶段期间运行，但是它会在索引运行期间暂停   
对于可恢复索引重新生成操作会禁用以下功能
   -    RESUMABLE=ON 不支持重新生成已禁用的索引
   -    ALTER INDEX REBUILD ALL 命令
   -    使用索引重新生成的 ALTER TABLE  
   -    无法在显式事务（不能属于 tran ... commit 块）中执行具有“RESUMEABLE = ON”的 DDL 命令
   -    重新生成具有已计算或 TIMESTAMP 列（作为键列）的索引。
-   如果基表包含 LOB 列，则可恢复聚集索引重新生成在此操作开始时需要 Sch-M 锁 

> [!NOTE]
> DDL 命令会运行到完成、暂停或失败。 如果命令暂停，则会发出错误，指示操作已暂停并且索引创建未完成。 可以从 [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md) 获取有关当前索引状态的详细信息。 如同之前一样，发生失败时，也会发出错误。 

 有关详细信息，请参阅 [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md)。  
  
 ### <a name="wait_at_low_priority-with-online-index-operations"></a>具有联机索引操作的 WAIT_AT_LOW_PRIORITY  
  
 要执行联机索引重新生成的 DDL 语句，必须完成对某一特定表运行的所有活动阻塞事务。 在联机索引重新生成执行时，它会阻塞准备对此表执行的所有新事务。 尽管联机索引重新生成锁的持续时间非常短，但等待某一给定表的所有打开的事务完成并阻塞新事务启动可能对吞吐量造成很大影响，导致工作负荷变慢或超时，并严重限制对基础表的访问。 **WAIT_AT_LOW_PRIORITY** 选项允许 DBA 管理联机索引重新生成需要的 S 锁和 Sch-M 锁，并允许他们选择 3 个选项之一。 在所有 3 种情况下，如果等待期间 ( (MAX_DURATION = n [minutes]) ) 没有阻塞活动，则联机索引重新生成会立即执行，而不等待 DDL 语句完成。  
  
## <a name="spatial-index-restrictions"></a>空间索引限制  
 重新生成空间索引时，基础用户表在索引操作持续期间不可用，因为空间索引持有架构锁。  
  
 对用户表的某一列定义了空间索引时，无法修改该表中的 PRIMARY KEY 约束。 若要更改 PRIMARY KEY 约束，首先要删除该表的每个空间索引。 修改 PRIMARY KEY 约束后，您可以重新创建每个空间索引。  
  
 在单个分区重新生成操作中，无法指定任何空间索引。 但是，您可以在完整的分区重新生成过程中指定空间索引。  
  
 若要更改特定于某个空间索引的选项（例如 BOUNDING_BOX 或 GRID），您可以使用 CREATE SPATIAL INDEX 语句指定 DROP_EXISTING = ON，或删除该空间索引并创建一个新的空间索引。 有关示例，请参阅 [CREATE SPATIAL INDEX (Transact-SQL)](../../t-sql/statements/create-spatial-index-transact-sql.md)中的“备注”部分。  
  
## <a name="data-compression"></a>数据压缩  
 有关数据压缩的详细信息，请参阅 [数据压缩](../../relational-databases/data-compression/data-compression.md)。  
  
 若要评估更改 PAGE 和 ROW 压缩将对表、索引或分区有何影响，请使用 [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) 存储过程。  
  
以下限制适用于已分区索引：  
  
-   使用 ALTER INDEX ALL ... 时，如果相应表具有非对齐索引，则无法更改单个分区的压缩设置。  
-   ALTER INDEX \<index> ...REBUILD PARTITION ... 语法可重新生成索引的指定分区。  
-   ALTER INDEX \<index> ...REBUILD WITH ... 语法可重新生成索引的所有分区。  
  
## <a name="statistics"></a>统计信息  
 在对某个表执行 ALTER INDEX ALL ... 时，只更新与索引相关联的统计信息。 针对表（而不是索引）自动或手动创建的统计信息不会更新。  
  
## <a name="permissions"></a>权限  
 若要执行 ALTER INDEX，至少需要对表或视图具有 ALTER 权限。  
  
## <a name="version-notes"></a>版本说明  
  
-  [!INCLUDE[ssSDS](../../includes/sssds-md.md)]不使用文件组和文件流选项。  
-  列存储索引在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 之前不可用。 
-  自 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 起，支持可恢复索引操作   
  
## <a name="basic-syntax-example"></a>基本语法示例：   
  
```sql 
ALTER INDEX index1 ON table1 REBUILD;  
  
ALTER INDEX ALL ON table1 REBUILD;  
  
ALTER INDEX ALL ON dbo.table1 REBUILD;  
```

## <a name="examples-columnstore-indexes"></a>示例：列存储索引  
 这些示例适用于列存储索引。  
  
### <a name="a-reorganize-demo"></a>A. REORGANIZE 演示  
 此示例演示 ALTER INDEX REORGANIZE 命令的工作原理。  它创建一个具有多个行组的表，然后演示 REORGANIZE 如何合并行组。  
  
```sql  
-- Create a database   
CREATE DATABASE [ columnstore ];  
GO  
  
-- Create a rowstore staging table  
CREATE TABLE [ staging ] (  
     AccountKey              int NOT NULL,  
     AccountDescription      nvarchar (50),  
     AccountType             nvarchar(50),  
     AccountCodeAlternateKey     int  
     )  
  
-- Insert 10 million rows into the staging table.   
DECLARE @loop int  
DECLARE @AccountDescription varchar(50)  
DECLARE @AccountKey int  
DECLARE @AccountType varchar(50)  
DECLARE @AccountCode int  
  
SELECT @loop = 0  
BEGIN TRAN  
    WHILE (@loop < 300000)   
      BEGIN  
        SELECT @AccountKey = CAST (RAND()*10000000 as int);  
        SELECT @AccountDescription = 'accountdesc ' + CONVERT(varchar(20), @AccountKey);  
        SELECT @AccountType = 'AccountType ' + CONVERT(varchar(20), @AccountKey);  
        SELECT @AccountCode =  CAST (RAND()*10000000 as int);  
  
        INSERT INTO  staging VALUES (@AccountKey, @AccountDescription, @AccountType, @AccountCode);  
  
        SELECT @loop = @loop + 1;  
    END  
COMMIT  
  
-- Create a table for the clustered columnstore index  
  
CREATE TABLE cci_target (  
     AccountKey              int NOT NULL,  
     AccountDescription      nvarchar (50),  
     AccountType             nvarchar(50),  
     AccountCodeAlternateKey int  
     )  
  
-- Convert the table to a clustered columnstore index named inxcci_cci_target;  
CREATE CLUSTERED COLUMNSTORE INDEX idxcci_cci_target ON cci_target;  
```  
  
 使用 TABLOCK 选项并行插入行。 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，INSERT INTO 操作可以在使用 TABLOCK 时并行运行。  
  
```sql  
INSERT INTO cci_target WITH (TABLOCK) 
SELECT TOP 300000 * FROM staging;  
```  
  
 运行此命令可查看打开的增量行组。 行组数取决于并行度。  
  
```sql  
SELECT *   
FROM sys.dm_db_column_store_row_group_physical_stats   
WHERE object_id  = object_id('cci_target');  
```  
  
 运行此命令以将所有关闭和打开的行组强制到列存储中。  
  
```sql  
ALTER INDEX idxcci_cci_target ON cci_target REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
 再次运行此命令，你会看到较小行组合并为一个压缩行组。  
  
```sql  
ALTER INDEX idxcci_cci_target ON cci_target REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
### <a name="b-compress-closed-delta-rowgroups-into-the-columnstore"></a>B. 将关闭的增量行组压缩到列存储中  
 此示例使用 REORGANIZE 选项将每个关闭的增量行组作为压缩行组压缩到列存储中。   这不是必需的，但是在 tuple-mover 压缩关闭的行组的速度不够快时非常有用。  
  
```sql  
-- Uses AdventureWorksDW  
-- REORGANIZE all partitions  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE;  
  
-- REORGANIZE a specific partition  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE PARTITION = 0;  
```  
  
### <a name="c-compress-all-open-and-closed-delta-rowgroups-into-the-columnstore"></a>C. 将所有打开和关闭的增量行组压缩到列存储中  
 **适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 命令 REORGANIZE WITH ( COMPRESS_ALL_ROW_GROUPS = ON ) 将每个打开和关闭的增量行组作为压缩行组压缩到列存储中。 这会清空增量存储，并强制所有行压缩到列存储中。 这在执行许多插入操作之后特别有用，因为这些操作将行存储在一个或多个增量行组中。  
  
 REORGANIZE 会合并行组以填充最大行数 \<= 1,024,576 的行组。 因此，在压缩所有打开和关闭的行组时，最后不会得到与大量其中只包含少量行的压缩行组。 需要行组尽可能满，以减少压缩大小并提高查询性能。  
  
```sql  
-- Uses AdventureWorksDW2016  
-- Move all OPEN and CLOSED delta rowgroups into the columnstore.  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
  
-- For a specific partition, move all OPEN AND CLOSED delta rowgroups into the columnstore  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE PARTITION = 0 WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
### <a name="d-defragment-a-columnstore-index-online"></a>D. 对列存储索引进行联机碎片整理  
 不适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]。  
  
 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，REORGANIZE 不仅仅会将增量行组压缩到列存储中。 它还执行联机碎片整理。 首先，它会在删除了行组中 10% 或更多行时物理移除已删除的行，从而减少列存储的大小。  然后，它将行组合并在一起以形成更大行组，每个行组最多包含 1,024,576 行。  更改的所有行组都会重新压缩。  
  
> [!NOTE]
> 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，在多数情况下不再需要重新生成列存储索引，因为 REORGANIZE 会物理移除已删除的行并合并行组。 COMPRESS_ALL_ROW_GROUPS 选项将所有打开或关闭的增量行组强制到列存储中，这以前只能通过重新生成来进行。 REORGANIZE 联机执行，并在后台进行，因此在进行该操作时可以继续进行查询。  
  
```sql  
-- Uses AdventureWorks  
-- Defragment by physically removing rows that have been logically deleted from the table, and merging rowgroups.  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE;  
```  
  
### <a name="e-rebuild-a-clustered-columnstore-index-offline"></a>E. 脱机重新生成聚集列存储索引  
适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）   
  
> [!TIP]
> 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始并且在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]中，建议使用 ALTER INDEX REORGANIZE 而不是 ALTER INDEX REBUILD。  
  
> [!NOTE]
> 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中，REORGANIZE 仅用于将关闭的行组压缩到列存储中。 执行碎片整理操作以及将所有增量行组都强制到列存储中的唯一方法是重新生成索引。  
  
 此示例演示如何重新生成聚集列存储索引以及将所有增量行组都强制到列存储中。 这个第一步将准备具有一个聚集列存储索引的表 FactInternetSales2 并且插入来自前四列的数据。  
  
```sql  
-- Uses AdventureWorksDW  
  
CREATE TABLE dbo.FactInternetSales2 (  
    ProductKey [int] NOT NULL,   
    OrderDateKey [int] NOT NULL,   
    DueDateKey [int] NOT NULL,   
    ShipDateKey [int] NOT NULL);  
  
CREATE CLUSTERED COLUMNSTORE INDEX cci_FactInternetSales2  
ON dbo.FactInternetSales2;  
  
INSERT INTO dbo.FactInternetSales2  
SELECT ProductKey, OrderDateKey, DueDateKey, ShipDateKey  
FROM dbo.FactInternetSales;  
  
SELECT * FROM sys.column_store_row_groups;  
```  
  
 结果表明有一个打开的行组，这意味着 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在关闭行组并且将数据移到列存储之前将等待添加更多的行。 下一个语句重新生成聚集列存储索引，这会将所有行强制到列存储中。  
  
```sql  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REBUILD;  
SELECT * FROM sys.column_store_row_groups;  
```  
  
 SELECT 语句的结果表明行组被压缩 (COMPRESSED)，这意味着行组的列段现在被压缩并且存储于列存储中。  
  
### <a name="f-rebuild-a-partition-of-a-clustered-columnstore-index-offline"></a>F. 脱机重新生成聚集列存储索引的分区  
 **适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）  
 
 若要重新生成大型聚集列存储索引的分区，请使用带有分区选项的 ALTER INDEX REBUILD。 此示例重新生成分区 12。 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，建议将 REBUILD 替换为 REORGANIZE。  
  
```sql  
ALTER INDEX cci_fact3   
ON fact3  
REBUILD PARTITION = 12;  
```  
  
### <a name="g-change-a-clustered-columstore-index-to-use-archival-compression"></a>G. 更改聚集列存储索引以使用存档压缩  
 不适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
 可以选择使用 COLUMNSTORE_ARCHIVE 数据压缩选项甚至进一步减少聚集列存储索引的大小。 这对于要保留在较廉价存储上的较旧数据十分实用。 建议仅对通常不访问的数据使用此方法，因为解压缩的速度低于正常 COLUMNSTORE 压缩。  
  
 下面的示例重新生成一个聚集列存储索引以便使用存档压缩，然后显示如何删除该存档压缩。 最后的结果将仅使用列存储压缩。  
  
```sql  
--Prepare the example by creating a table with a clustered columnstore index.  
CREATE TABLE SimpleTable (  
    ProductKey [int] NOT NULL,   
    OrderDateKey [int] NOT NULL,   
    DueDateKey [int] NOT NULL,   
    ShipDateKey [int] NOT NULL  
);  
  
CREATE CLUSTERED INDEX cci_SimpleTable ON SimpleTable (ProductKey);  
  
CREATE CLUSTERED COLUMNSTORE INDEX cci_SimpleTable  
ON SimpleTable  
WITH (DROP_EXISTING = ON);  
  
--Compress the table further by using archival compression.  
ALTER INDEX cci_SimpleTable ON SimpleTable  
REBUILD  
WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE);  
  
--Remove the archive compression and only use columnstore compression.  
ALTER INDEX cci_SimpleTable ON SimpleTable  
REBUILD  
WITH (DATA_COMPRESSION = COLUMNSTORE);  
GO  
```  
  
## <a name="examples-rowstore-indexes"></a>示例：行存储索引  
  
### <a name="a-rebuilding-an-index"></a>A. 重新生成索引  
 下面的示例在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `Employee` 表中重新生成单个索引。  
  
```sql  
ALTER INDEX PK_Employee_EmployeeID ON HumanResources.Employee REBUILD;  
```  
  
### <a name="b-rebuilding-all-indexes-on-a-table-and-specifying-options"></a>B. 重新生成表的所有索引并指定选项  
 下面的示例指定了 ALL 关键字。 这将重新生成与 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的表 Production.Product 相关联的所有索引。 其中指定了三个选项。  
  
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
ALTER INDEX ALL ON Production.Product  
REBUILD WITH (FILLFACTOR = 80, SORT_IN_TEMPDB = ON, STATISTICS_NORECOMPUTE = ON);  
```  
  
下面的示例添加包含低优先级锁选项的 ONLINE 选项，并添加行压缩选项。  
  
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
ALTER INDEX ALL ON Production.Product  
REBUILD WITH   
(  
    FILLFACTOR = 80,   
    SORT_IN_TEMPDB = ON,  
    STATISTICS_NORECOMPUTE = ON,  
    ONLINE = ON ( WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 4 MINUTES, ABORT_AFTER_WAIT = BLOCKERS ) ),   
    DATA_COMPRESSION = ROW  
);  
```  
  
### <a name="c-reorganizing-an-index-with-lob-compaction"></a>C. 通过 LOB 压缩重新组织索引  
 下面的示例重新整理 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的单个聚集索引。 因为该索引在叶级别包含 LOB 数据类型，所以该语句还会压缩所有包含该大型对象数据的页。 注意，不需要指定 WITH (LOB_COMPACTION = ON) 选项，因为默认值为 ON。  
  
```sql  
ALTER INDEX PK_ProductPhoto_ProductPhotoID ON Production.ProductPhoto REORGANIZE WITH (LOB_COMPACTION = ON);  
```  
  
### <a name="d-setting-options-on-an-index"></a>D. 设置索引的选项。  
 下面的示例为 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的索引 `AK_SalesOrderHeader_SalesOrderNumber` 设置了几个选项。  
  
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
ALTER INDEX AK_SalesOrderHeader_SalesOrderNumber ON  
    Sales.SalesOrderHeader  
SET (  
    STATISTICS_NORECOMPUTE = ON,  
    IGNORE_DUP_KEY = ON,  
    ALLOW_PAGE_LOCKS = ON  
    ) ;  
GO
```  
  
### <a name="e-disabling-an-index"></a>E. 禁用索引。  
 下面的示例禁用了对 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 `Employee` 表的非聚集索引。  
  
```sql  
ALTER INDEX IX_Employee_ManagerID ON HumanResources.Employee DISABLE;
```  
  
### <a name="f-disabling-constraints"></a>F. 禁用约束  
 下面的示例通过禁用 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 PRIMARY KEY 索引来禁用 PRIMARY KEY 约束。 自动禁用对基础表的 FOREIGN KEY 约束，并显示警告消息。  
  
```sql  
ALTER INDEX PK_Department_DepartmentID ON HumanResources.Department DISABLE;  
```  
  
结果集返回此警告消息。  
  
```  
Warning: Foreign key 'FK_EmployeeDepartmentHistory_Department_DepartmentID'  
on table 'EmployeeDepartmentHistory' referencing table 'Department'  
was disabled as a result of disabling the index 'PK_Department_DepartmentID'.
```  
  
### <a name="g-enabling-constraints"></a>G. 启用约束  
 下面的示例启用在示例 F 中禁用的 PRIMARY KEY 和 FOREIGN KEY 约束。  
  
通过重新生成 PRIMARY KEY 索引启用 PRIMARY KEY 约束。  
  
```sql  
ALTER INDEX PK_Department_DepartmentID ON HumanResources.Department REBUILD;  
```  
  
此时，将启用 FOREIGN KEY 约束。  
  
```sql  
ALTER TABLE HumanResources.EmployeeDepartmentHistory  
CHECK CONSTRAINT FK_EmployeeDepartmentHistory_Department_DepartmentID;  
GO  
```  
  
### <a name="h-rebuilding-a-partitioned-index"></a>H. 重新生成分区索引  
 下面的示例在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中重新生成一个分区索引为 `5` 的分区，分区号为 `IX_TransactionHistory_TransactionDate`。 分区 5 是联机重新生成的，并且对索引重新生成操作获取的每个锁分别应用低优先级锁的 10 分钟等待时间。 如果在此时间无法获取锁来完成索引重新生成，重新生成操作语句就会中止。  
  
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
-- Verify the partitioned indexes.  
SELECT *  
FROM sys.dm_db_index_physical_stats (DB_ID(),OBJECT_ID(N'Production.TransactionHistory'), NULL , NULL, NULL);  
GO  
--Rebuild only partition 5.  
ALTER INDEX IX_TransactionHistory_TransactionDate  
ON Production.TransactionHistory  
REBUILD Partition = 5   
   WITH (ONLINE = ON (WAIT_AT_LOW_PRIORITY (MAX_DURATION = 10 minutes, ABORT_AFTER_WAIT = SELF)));  
GO  
```  
  
### <a name="i-changing-the-compression-setting-of-an-index"></a>I. 更改索引的压缩设置  
 下面的示例重新生成未分区行存储表的索引。  
  
```sql
ALTER INDEX IX_INDEX1   
ON T1  
REBUILD   
WITH (DATA_COMPRESSION = PAGE);  
GO  
```  
  
有关其他数据压缩示例，请参阅[数据压缩](../../relational-databases/data-compression/data-compression.md)。  
 
### <a name="j-online-resumable-index-rebuild"></a>J. 联机可恢复索引重新生成

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   

 下面的示例演示如何使用联机可恢复索引重新生成。 

1. 作为 MAXDOP=1 的可恢复操作执行联机索引重新生成。

   ```sql
   ALTER INDEX test_idx on test_table REBUILD WITH (ONLINE=ON, MAXDOP=1, RESUMABLE=ON) ;
   ```

2. 在索引操作暂停之后再次执行相同命令（请参阅上文）会自动恢复索引重新生成操作。

3. 作为 MAX_DURATION 设置为 240 分钟的可恢复操作执行联机索引重新生成。

   ```sql
   ALTER INDEX test_idx on test_table REBUILD WITH (ONLINE=ON, RESUMABLE=ON, MAX_DURATION=240) ; 
   ```
4. 暂停正在运行的可恢复联机索引重新生成。

   ```sql
   ALTER INDEX test_idx on test_table PAUSE ;
   ```   
5. 对作为可恢复操作（为设置为 4 的 MAXDOP 指定新值）执行的索引重新生成，恢复联机索引重新生成。

   ```sql
   ALTER INDEX test_idx on test_table RESUME WITH (MAXDOP=4) ;
   ```
6. 对作为可恢复操作执行的索引联机重新生成恢复联机索引重新生成操作。 将 MAXDOP 设置为 2，将作为可恢复操作运行的索引的执行时间设置为 240 分钟，如果索引在锁上受阻，等待 10 分钟，在此之后终止所有阻塞程序。 

   ```sql
      ALTER INDEX test_idx on test_table  
         RESUME WITH (MAXDOP=2, MAX_DURATION= 240 MINUTES, 
         WAIT_AT_LOW_PRIORITY (MAX_DURATION=10, ABORT_AFTER_WAIT=BLOCKERS)) ;
   ```      
7. 中止正在运行或暂停的可恢复索引重新生成操作。

   ```sql
   ALTER INDEX test_idx on test_table ABORT ;
   ``` 
  
## <a name="see-also"></a>另请参阅  
[SQL Server 索引体系结构和设计指南](../../relational-databases/sql-server-index-design-guide.md)     
[联机执行索引操作](../../relational-databases/indexes/perform-index-operations-online.md)    
[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)     
[CREATE SPATIAL INDEX (Transact-SQL)](../../t-sql/statements/create-spatial-index-transact-sql.md)   
[CREATE XML INDEX (Transact-SQL)](../../t-sql/statements/create-xml-index-transact-sql.md)   
[DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)   
[禁用索引和约束](../../relational-databases/indexes/disable-indexes-and-constraints.md)   
[XML 索引 (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md)   
[重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)   
[sys.dm_db_index_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
[EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)    
