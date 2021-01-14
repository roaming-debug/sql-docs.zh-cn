---
description: sys.dm_db_index_operational_stats (Transact-SQL)
title: sys.dm_db_index_operational_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_index_operational_stats
- sys.dm_db_index_operational_stats_TSQL
- sys.dm_db_index_operational_stats
- dm_db_index_operational_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_index_operational_stats dynamic management function
ms.assetid: 13adf2e5-2150-40a6-b346-e74a33ce29c6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 819d0388ca7b358bb9b6cf455cfb061a1a38fd9a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172169"
---
# <a name="sysdm_db_index_operational_stats-transact-sql"></a>sys.dm_db_index_operational_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回数据库中表或索引的每个分区的当前低级 i/o、锁定、闩锁和访问方法活动。    
    
 内存优化索引不会出现在此 DMV 中。    
    
> [!NOTE]    
>  **sys.dm_db_index_operational_stats** 不返回有关内存优化索引的信息。 有关内存优化索引使用的信息，请参阅 [&#40;transact-sql&#41;sys.dm_db_xtp_index_stats ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md)。    
        
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>语法    
    
```    
sys.dm_db_index_operational_stats (    
    { database_id | NULL | 0 | DEFAULT }    
  , { object_id | NULL | 0 | DEFAULT }    
  , { index_id | 0 | NULL | -1 | DEFAULT }    
  , { partition_number | NULL | 0 | DEFAULT }    
)    
```    
    
## <a name="arguments"></a>参数    

*database_id* |NULL |0 |缺省值

  数据库 ID。 *database_id* 为 **smallint**。 有效的输入包括数据库的 ID 号、NULL、0 或 DEFAULT。 默认值为 0。 在此上下文中，NULL、0 和 DEFAULT 是等效值。    
    
 指定 NULL 可返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中所有数据库的信息。 如果为 *database_id* 指定 null，则还必须为 *object_id*、 *index_id* 和 *partition_number* 指定 null。    
    
 可以指定内置函数 [DB_ID](../../t-sql/functions/db-id-transact-sql.md)。    

*object_id* |NULL |0 |缺省值

 索引所基于的表或视图的对象 ID。 *object_id* 是 **int**。    
    
 有效的输入包括表和视图的 ID 号、NULL、0 或 DEFAULT。 默认值为 0。 在此上下文中，NULL、0 和 DEFAULT 是等效值。    
    
 指定 NULL 可返回指定数据库中的所有表和视图的缓存信息。 如果为 *object_id* 指定 null，则还必须为 *index_id* 和 *partition_number* 指定 null。    

*index_id* |0 |NULL |-1 |缺省值

 索引的 ID。 *index_id* 是 **int**。有效输入包括索引的 ID 号、0（如果 *object_id* 为堆）、NULL、-1 或默认值。 默认值为 -1。在此上下文中，NULL、-1 和 DEFAULT 是等价值。    
    
 指定 NULL 可返回基表或视图的所有索引的缓存信息。 如果为 *index_id* 指定 null，则还必须为 *partition_number* 指定 null。    

*partition_number* |NULL |0 |缺省值

 对象中的分区号。 *partition_number* 是 **int**。有效输入包括索引或堆的 *partion_number* 、NULL、0或 DEFAULT。 默认值为 0。 在此上下文中，NULL、0 和 DEFAULT 是等效值。    
    
 指定 NULL 可返回索引或堆的所有分区的缓存信息。    
    
 *partition_number* 从1开始。 未分区索引或堆 *partition_number* 设置为1。    
    
## <a name="table-returned"></a>返回的表    
    
|列名称|数据类型|说明|    
|-----------------|---------------|-----------------|    
|database_id|**smallint**|数据库 ID。|    
|object_id|**int**|表或视图的 ID。|    
|index_id|**int**|索引或堆的 ID。<br /><br /> 0 = 堆| 
|**partition_number**|**int**|索引或堆中从 1 开始的分区号。| 
|**hobt_id**|**bigint**|**适用** 于： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 通过 [当前版本](../../sql-server/what-s-new-in-sql-server-2016.md))  ([!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。<br /><br /> 跟踪列存储索引的内部数据的堆或B 树数据行集的 ID。<br /><br /> NULL-这不是内部列存储行集。<br /><br /> 有关更多详细信息，请参阅 [sys.internal_partitions &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)|       
|**leaf_insert_count**|**bigint**|叶级插入的累积计数。|    
|**leaf_delete_count**|**bigint**|叶级删除的累积计数。 仅对未标记为 ghost 的已删除记录递增 leaf_delete_count。 对于首先会幻像的已删除记录，将改为递增 **leaf_ghost_count** 。|    
|**leaf_update_count**|**bigint**|叶级更新的累积计数。|    
|**leaf_ghost_count**|**bigint**|标记为删除但尚未删除的叶级行的累积计数。 此计数不包括未标记为 ghost 的立即删除的记录。 清除线程会按设置的间隔删除这些行。 此值不包括由于某个快照隔离事务未完成而保留的行。|    
|**nonleaf_insert_count**|**bigint**|叶级以上的插入累积计数。<br /><br /> 0 = 堆或列存储|    
|**nonleaf_delete_count**|**bigint**|叶级以上的删除累积计数。<br /><br /> 0 = 堆或列存储|    
|**nonleaf_update_count**|**bigint**|叶级以上的更新累积计数。<br /><br /> 0 = 堆或列存储|    
|**leaf_allocation_count**|**bigint**|索引或堆中的叶级页分配的累积计数。<br /><br /> 对于索引，页分配与页拆分对应。|    
|**nonleaf_allocation_count**|**bigint**|叶级以上由页拆分引起的页分配的累积计数。<br /><br /> 0 = 堆或列存储|    
|**leaf_page_merge_count**|**bigint**|叶级页合并的累积计数。 对列存储索引始终为 0。|    
|**nonleaf_page_merge_count**|**bigint**|叶级以上页合并的累积计数。<br /><br /> 0 = 堆或列存储|    
|**range_scan_count**|**bigint**|从索引或堆开始的范围和表扫描的累积计数。|    
|**singleton_lookup_count**|**bigint**|对索引或堆的单行检索的累积计数。|    
|**forwarded_fetch_count**|**bigint**|通过前推记录提取的行计数。<br /><br /> 0 = 索引|    
|**lob_fetch_in_pages**|**bigint**|从 LOB_DATA 分配单元检索到的大型对象 (LOB) 页的累积计数。 这些页包含存储在类型为 **text**、 **ntext**、 **image**、 **varchar (max)**、 **nvarchar (max)**、 **varbinary (max)** 和 **xml** 的列中的数据。 有关详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。|    
|**lob_fetch_in_bytes**|**bigint**|检索到的 LOB 数据字节数的累积计数。|    
|**lob_orphan_create_count**|**bigint**|为大容量操作创建的孤立 LOB 值的累积计数。<br /><br /> 0 = 非聚集索引|    
|**lob_orphan_insert_count**|**bigint**|大容量操作期间插入的孤立 LOB 值的累积计数。<br /><br /> 0 = 非聚集索引|    
|**row_overflow_fetch_in_pages**|**bigint**|从 ROW_OVERFLOW_DATA 分配单元检索到的行溢出数据页数的累积计数。<br /><br /> 这些页包含存储在类型 **varchar (n)**、 **nvarchar (n)**、 **varbinary (n)** 和已被推送到行外的 **sql_variant** 的列中的数据。|    
|**row_overflow_fetch_in_bytes**|**bigint**|检索到的行溢出数据字节数的累积计数。|    
|**column_value_push_off_row_count**|**bigint**|已推出行外以使插入或更新的行可容纳在页中的 LOB 数据和行溢出数据的列值累积计数。|    
|**column_value_pull_in_row_count**|**bigint**|已请求到行内的 LOB 数据和行溢出数据的列值的累积计数。 当更新操作释放记录中的空间，并提供将一个或多个行外值从 LOB_DATA 或 ROW_OVERFLOW_DATA 分配单元请求到 IN_ROW_DATA 分配单元中的机会时，就会出现此计数。|    
|**row_lock_count**|**bigint**|请求的行锁的累积数量。|    
|**row_lock_wait_count**|**bigint**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]等待行锁的累积次数。|    
|**row_lock_wait_in_ms**|**bigint**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]等待行锁的总毫秒数。|    
|**page_lock_count**|**bigint**|请求的页锁的累积数量。|    
|**page_lock_wait_count**|**bigint**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]等待页锁的累积次数。|    
|**page_lock_wait_in_ms**|**bigint**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]等待页锁的总毫秒数。|    
|**index_lock_promotion_attempt_count**|**bigint**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]尝试升级锁的累积次数。|    
|**index_lock_promotion_count**|**bigint**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]升级锁的累积次数。|    
|**page_latch_wait_count**|**bigint**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]由于闩锁争用而等待的累积次数。|    
|**page_latch_wait_in_ms**|**bigint**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]由于闩锁争用而等待的累积毫秒数。|    
|**page_io_latch_wait_count**|**bigint**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]等待 I/O 页闩锁的累积次数。|    
|**page_io_latch_wait_in_ms**|**bigint**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]等待页 I/O 闩锁的累积毫秒数。|    
|**tree_page_latch_wait_count**|**bigint**|**page_latch_wait_count** 的子集，仅包括较高级别的 B 树页。 对堆或列存储索引始终为 0。|    
|**tree_page_latch_wait_in_ms**|**bigint**|**page_latch_wait_in_ms** 的子集，仅包括较高级别的 B 树页。 对堆或列存储索引始终为 0。|    
|**tree_page_io_latch_wait_count**|**bigint**|**page_io_latch_wait_count** 的子集，仅包括较高级别的 B 树页。 对堆或列存储索引始终为 0。|    
|**tree_page_io_latch_wait_in_ms**|**bigint**|**page_io_latch_wait_in_ms** 的子集，仅包括较高级别的 B 树页。 对堆或列存储索引始终为 0。|    
|**page_compression_attempt_count**|**bigint**|对于表、索引或索引视图的特定分区，针对 PAGE 级压缩计算的页数。 因为未能极大地节省空间，所以将包括未压缩的页。 对列存储索引始终为 0。|    
|**page_compression_success_count**|**bigint**|对于表、索引或索引视图的特定分区，使用 PAGE 压缩功能压缩的数据页数。 对列存储索引始终为 0。|    
    
## <a name="remarks"></a>备注    
 此动态管理对象不接受来自和的关联 `CROSS APPLY` 参数 `OUTER APPLY` 。    
    
 可使用 **sys.dm_db_index_operational_stats** 跟踪用户读取或写入表、索引或分区前必须等待的时间长度，并标识遇到大型 I/O 活动或热点的表或索引。    
    
 使用以下各列可标识争用区。    
    
 若要分析表或索引分区的通用访问模式，请使用这些列：    
    
-   **leaf_insert_count**    
    
-   **leaf_delete_count**    
    
-   **leaf_update_count**    
    
-   **leaf_ghost_count**    
    
-   **range_scan_count**    
    
-   **singleton_lookup_count**    
    
 若要标识闩锁和锁争用，请使用这些列：    
    
-   **page_latch_wait_count** 和 **page_latch_wait_in_ms**    
    
     这些列指示索引或堆上是否存在闩锁争用以及争用的意义。    
    
-   **row_lock_count** 和 **page_lock_count**    
    
     这些列指示[!INCLUDE[ssDE](../../includes/ssde-md.md)]尝试获取行锁和页锁的次数。    
    
-   **row_lock_wait_in_ms** 和 **page_lock_wait_in_ms**    
    
     这些列指示索引或堆上是否存在锁争用以及争用的意义。    
    
 **分析索引或堆分区的物理 I/O 的统计信息**    
    
-   **page_io_latch_wait_count** 和 **page_io_latch_wait_in_ms**    
    
     这些列指示是否已发出物理 I/O 以便将索引或堆页载入内存以及发出的 I/O 数。    
    
## <a name="column-remarks"></a>列备注    
 **lob_orphan_create_count** 和 **lob_orphan_insert_count** 中的值应始终相等。    
    
 对于包含一个或多个 LOB 列作为包含列的非聚集索引，**lob_fetch_in_pages** 和 **lob_fetch_in_bytes** 列中的值可以大于零。 有关详细信息，请参阅 [Create Indexes with Included Columns](../../relational-databases/indexes/create-indexes-with-included-columns.md)。 同样，对于非聚集索引，如果索引包含可推送到行外的列，则 **row_overflow_fetch_in_pages** 和 **row_overflow_fetch_in_bytes** 列中的值也可以大于 0。    
    
## <a name="how-the-counters-in-the-metadata-cache-are-reset"></a>如何重置元数据缓存中的计数器    
 仅当表示堆或索引的元数据缓存对象可用时，**sys.dm_db_index_operational_stats** 返回的数据才存在。 此数据既不是持久性数据，也不是事务上一致的数据。 这意味着，不能使用这些计数器确定是否已使用索引，或确定上次使用索引的时间。 有关此方面的信息，请参阅 [&#40;transact-sql&#41;sys.dm_db_index_usage_stats ](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)。    
    
 只要堆或索引的元数据被载入元数据缓存，每列中的值就会被设置为零，且在从元数据缓存中删除缓存对象前会累积统计信息。 所以，活动堆或索引可能始终将其元数据放在缓存中，且累积计数可能反映自上次启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以来的活动。 活动较少的堆或索引的元数据将在使用时移入和移出缓存。 因此，它可能有、也可能没有可用值。 删除索引将导致从内存中删除对应统计信息，且函数不再报告这些统计信息。 对索引执行的其他 DDL 操作可能导致统计信息的值被重置为零。    
    
## <a name="using-system-functions-to-specify-parameter-values"></a>使用系统函数指定参数值    
 您可以使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数 [DB_ID](../../t-sql/functions/db-id-transact-sql.md) 和 [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md) 来指定 *database_id* 和 *object_id* 参数的值。 但是，将无效的值传递给这些函数可能会导致意外结果。 请始终确保使用 DB_ID 或 OBJECT_ID 时返回了有效的 ID。 有关详细信息，请参阅 [sys.dm_db_index_physical_stats &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)中的 "备注" 部分。    
    
## <a name="permissions"></a>权限    
 需要下列权限：    
    
-   `CONTROL` 对数据库中指定对象的权限    
    
-   `VIEW DATABASE STATE` 使用对象通配符 @*object_id* = NULL 返回有关指定数据库中所有对象的信息的权限    
    
-   `VIEW SERVER STATE` 使用数据库通配符 @*database_id* = NULL 返回有关所有数据库的信息的权限    
    
 授予 `VIEW DATABASE STATE` 会允许返回数据库中的所有对象，而不管对特定对象拒绝的任何控制权限。    
    
 拒绝 `VIEW DATABASE STATE` 禁止返回数据库中的所有对象，而不管对特定对象授予的任何控制权限。 另外，在指定数据库通配符时 `@database_id=NULL` ，将忽略数据库。    
    
 有关详细信息，请参阅 [&#40;transact-sql&#41;中的动态管理视图和函数 ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)。    
    
## <a name="examples"></a>示例    
    
### <a name="a-returning-information-for-a-specified-table"></a>A. 返回指定表的信息    
 下面的示例返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中 `Person.Address` 表的所有索引和分区的信息。 执行此查询至少需要对 `Person.Address` 表具有 CONTROL 权限。    
    
> [!IMPORTANT]    
> 在使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数 DB_ID 和 OBJECT_ID 返回参数值时，请始终确保返回了有效的 ID。 如果找不到数据库或对象的名称，例如相应名称不存在或拼写不正确，则两个函数都会返回 NULL。 **sys.dm_db_index_operational_stats** 函数将 NULL 解释为指定所有数据库或所有对象的通配符值。 由于这可能是无心之举，所以此部分中的示例说明了确定数据库 ID 和对象 ID 的安全方法。    
    
```sql    
DECLARE @db_id int;    
DECLARE @object_id int;    
SET @db_id = DB_ID(N'AdventureWorks2012');    
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');    
IF @db_id IS NULL     
  BEGIN;    
    PRINT N'Invalid database';    
  END;    
ELSE IF @object_id IS NULL    
  BEGIN;    
    PRINT N'Invalid object';    
  END;    
ELSE    
  BEGIN;    
    SELECT * FROM sys.dm_db_index_operational_stats(@db_id, @object_id, NULL, NULL);    
  END;    
GO    
```    
    
### <a name="b-returning-information-for-all-tables-and-indexes"></a>B. 返回所有表和索引的信息    
 下面的示例返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中所有表和索引的信息。 执行此查询需要 VIEW SERVER STATE 权限。    
    
```sql    
SELECT * FROM sys.dm_db_index_operational_stats( NULL, NULL, NULL, NULL);    
GO        
```    
    
## <a name="see-also"></a>另请参阅    
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)     
 [与索引相关的动态管理视图和函数 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)     
 [监视和优化性能](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [sys.dm_db_index_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)     
 [sys.dm_db_index_usage_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)     
 [sys.dm_os_latch_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)     
 [sys.dm_db_partition_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)     
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)     
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)    
    
