---
description: 'sys.dm_db_column_store_row_group_physical_stats (Transact-sql) '
title: sys.dm_db_column_store_row_group_physical_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_column_store_row_group_physical_stats_TSQL
- sys.dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_column_store_row_group_physical_stats
- sys.dm_db_column_store_row_group_physical_stats dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b47d4bd97718bab9b25c6cb6d7772b08cd7251f2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172119"
---
# <a name="sysdm_db_column_store_row_group_physical_stats-transact-sql"></a>sys.dm_db_column_store_row_group_physical_stats (Transact-sql) 


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

提供当前数据库中所有列存储索引的当前行组级信息。  

这将 [sys.column_store_row_groups &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)扩展目录视图。  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|基础表的 ID。|  
|index_id|**int**|此列存储索引 *object_id* 表的 ID。|  
|**partition_number**|**int**|保存 *row_group_id* 的表分区的 ID。 您可以使用 partition_number 将此 DMV 联接到 sys.partitions。|  
|**row_group_id**|**int**|此行组的 ID。 对于已分区表，值在分区中是唯一的。<br /><br /> 对于内存尾，为-1。|  
|**delta_store_hobt_id**|**bigint**|增量存储中行组的 hobt_id。<br /><br /> 如果行组不在增量存储中，则为 NULL。<br /><br /> 对于内存中表尾，为 NULL。|  
|**state**|**tinyint**|与 *state_description* 关联的 ID 号。<br /><br /> 0 = INVISIBLE<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED<br /><br /> 4 = 逻辑删除<br /><br /> 压缩是适用于内存中表的唯一状态。|  
|**state_desc**|**nvarchar(60)**|行组状态的说明：<br /><br /> 0-不可见-正在生成的行组。 例如： <br />压缩数据时，列存储中的行组是不可见的。 压缩完成后，元数据开关会将列存储行组的状态从不可见改为已压缩，并将增量存储行组的状态从 "已关闭" 更改为 "逻辑删除"。<br /><br /> 1-打开-正在接受新行的增量存储行组。 开放的行组仍采用行存储格式，并且尚未压缩成列存储格式。<br /><br /> 2-已关闭-增量存储中的行组，其中包含最大行数，正在等待元组移动进程将其压缩到列存储中。<br /><br /> 3压缩-使用列存储压缩进行压缩并存储在列存储中的行组。<br /><br /> 4-逻辑删除-行组，该组以前位于增量存储中，不再使用。|  
|**total_rows**|**bigint**|以物理方式存储在行组中的行数。 对于压缩的行组。 包括标记为已删除的行。|  
|**deleted_rows**|**bigint**|以物理方式存储在压缩行组中且标记为要删除的行数。<br /><br /> 对于增量存储中的行组，值为 0。|  
|**size_in_bytes**|**bigint**|此行组中所有页面的组合大小（以字节为单位）。 此大小不包括存储元数据或共享字典所需的大小。|  
|**trim_reason**|**tinyint**|触发压缩行组的行数小于最大行数的原因。<br /><br /> 0-UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION<br /><br /> 1-NO_TRIM<br /><br /> 2-BULKLOAD<br /><br /> 3-REORG<br /><br /> 4-DICTIONARY_SIZE<br /><br /> 5-MEMORY_LIMITATION<br /><br /> 6-RESIDUAL_ROW_GROUP<br /><br /> 7-STATS_MISMATCH<br /><br /> 8-溢出<br /><br /> 9-AUTO_MERGE|  
|**trim_reason_desc**|**nvarchar(60)**|*Trim_reason* 的说明。<br /><br /> 0-UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION：从的以前版本升级时出现 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br /> 1-NO_TRIM：未剪裁行组。 行组已压缩，最多包含1048576行。  如果在增量行组关闭后删除了行的子集，则行数可能会减少<br /><br /> BULKLOAD：大容量加载批大小限制的行数。<br /><br /> 3-REORG：作为 REORG 命令的一部分的强制压缩。<br /><br /> 4-DICTIONARY_SIZE：字典大小增长太大，无法同时压缩所有行。<br /><br /> 5-MEMORY_LIMITATION：没有足够的可用内存来压缩所有行。<br /><br /> 6-RESIDUAL_ROW_GROUP：在索引生成操作期间作为 < 1000000 行的最后一个行组的一部分关闭<br /><br /> 7-STATS_MISMATCH：仅适用于内存中表上的列存储。 如果统计错误地指明了尾部 >= 1000000 限定行，但找到的行数较少，则压缩行组将具有 < 1000000 行<br /><br /> 8-溢出：仅适用于内存中表的列存储。 如果 tail 具有 > 1000000 个限定行，则在计数介于10万到1000000<br /><br /> 9 AUTO_MERGE：在后台中运行的元组移动器合并操作将一个或多个行组合并到此行组。|  
|**transition_to_compressed_state**|tinyint|显示此行组如何从增量存储移到列存储中的压缩状态。<br /><br /> 1-NOT_APPLICABLE<br /><br /> 2-INDEX_BUILD<br /><br /> 3-TUPLE_MOVER<br /><br /> 4-REORG_NORMAL<br /><br /> 5-REORG_FORCED<br /><br /> 6-BULKLOAD<br /><br /> 7-合并|  
|**transition_to_compressed_state_desc**|nvarchar(60)| 1-NOT_APPLICABLE-操作不适用于增量存储。 或者，行组在升级到之前已经过压缩， [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 这种情况下不会保留历史记录。<br /><br /> 2-INDEX_BUILD 索引创建或索引重新生成压缩行组。<br /><br /> 3-TUPLE_MOVER-在后台压缩行组的元组移动器。 当行组将状态从 "已关闭" 更改<br /><br /> 4-REORG_NORMAL-重组操作，更改索引 .。。REORG，将关闭的行组从增量存储移到列存储中。 发生此错误之前，元组移动器有时间移动行组。<br /><br /> 5-REORG_FORCED-此行组在增量存储中处于打开状态，并且已强制转换为列存储，然后才有完整的行数。<br /><br /> 6-BULKLOAD-大容量加载操作直接压缩行组而不使用增量存储。<br /><br /> 7-合并操作合并了一个或多个行组到此行组，然后执行了列存储压缩。|  
|**has_vertipaq_optimization**|bit|VertiPaq 优化通过对行组中的行顺序重新排列，以实现更高的压缩，从而提高列存储压缩。 大多数情况下，此优化会自动进行。 在以下两种情况下不使用 VertiPaq 优化：<br/>  a. 当增量行组移到列存储中，并且列存储索引上有一个或多个非聚集索引时-在此情况下，将跳过 VertiPaq 优化，以最大程度地减少对映射索引的更改;<br/> b. 用于内存优化表上的列存储索引。 <br /><br /> 0 = 否<br /><br /> 1 = 是|  
|**产生**|bigint|与此行组关联的行组生成。|  
|**created_time**|datetime2|此行组的创建时间。<br /><br /> NULL-内存中表上的列存储索引。|  
|**closed_time**|datetime2|此行组的关闭时间的时钟时间。<br /><br /> NULL-内存中表上的列存储索引。|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="results"></a>结果  
 为当前数据库中的每个行组返回一行。  
  
## <a name="permissions"></a>权限  
要求对 `CONTROL` 表具有权限，对数据库具有权限 `VIEW DATABASE STATE` 。  
  
## <a name="examples"></a>示例  
  
### <a name="a-calculate-fragmentation-to-decide-when-to-reorganize-or-rebuild-a-columnstore-index"></a>A. 计算碎片以决定何时重新组织或重新生成列存储索引。  
 对于列存储索引，已删除行的百分比是对行组中碎片的良好度量。 当碎片为20% 或更大时，删除删除的行。 有关更多示例，请参阅 [重新组织和重新生成索引](~/relational-databases/indexes/reorganize-and-rebuild-indexes.md)。  
  
 此示例将 **sys.dm_db_column_store_row_group_physical_stats** 与其他系统表联接，然后将 `Fragmentation` 该列计算为当前数据库中每个行组的效率的估计值。 若要查找单个表的信息，请删除 **WHERE** 子句前面的注释连字符，并提供表名称。  
  
```sql  
SELECT i.object_id,   
    object_name(i.object_id) AS TableName,   
    i.name AS IndexName,   
    i.index_id,   
    i.type_desc,   
    CSRowGroups.*,  
    100*(ISNULL(deleted_rows,0))/NULLIF(total_rows,0) AS 'Fragmentation'
FROM sys.indexes AS i  
JOIN sys.dm_db_column_store_row_group_physical_stats AS CSRowGroups  
    ON i.object_id = CSRowGroups.object_id AND i.index_id = CSRowGroups.index_id   
-- WHERE object_name(i.object_id) = 'table_name'   
ORDER BY object_name(i.object_id), i.name, row_group_id;  
```  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)      
 [列存储索引体系结构](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)         
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)  
 [sys.column_store_dictionaries &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)   
 [sys.column_store_segments (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
