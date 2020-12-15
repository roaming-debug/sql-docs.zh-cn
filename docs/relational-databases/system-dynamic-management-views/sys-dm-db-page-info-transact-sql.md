---
description: sys.dm_db_page_info (Transact-SQL)
title: sys.dm_db_page_info (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/18/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_page_info
- sys.dm_db_page_info_TSQL
- dm_db_page_info
- dm_db_page_info_TSQL
- dbcc page
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_page_info dynamic management view
author: bluefooted
ms.author: pamela
manager: amitban
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 429f8049ef0b92168be5e3e0fc90c91e3d37224e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472808"
---
# <a name="sysdm_db_page_info-transact-sql"></a>sys.dm_db_page_info (Transact-SQL)

[!INCLUDE[tsql-appliesto-ssver15-asdb-xxxx-xxx](../../includes/tsql-appliesto-ssver15-asdb-xxxx-xxx.md)]

返回有关数据库中的页的信息。  函数返回一行，其中包含页中的标头信息，包括 `object_id` 、 `index_id` 和 `partition_id` 。  在大多数情况下，此函数取代了使用 `DBCC PAGE` 的需要。

> [!NOTE]
> `sys.dm_db_page_info` 当前仅在 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 和更高版本中受支持。


## <a name="syntax"></a>语法   
```  
sys.dm_db_page_info ( DatabaseId, FileId, PageId, Mode )  
``` 

## <a name="arguments"></a>自变量  
*DatabaseId* |NULL |缺省值     
数据库的 ID。 *DatabaseId* 为 **smallint**。 有效输入是数据库的 ID 号。 默认值为 NULL，但对于此参数发送 NULL 值将导致错误。
 
*FileId* |NULL |缺省值   
文件的 ID。 *FileId* 为 **int**。 有效输入是由 *DatabaseId* 指定的数据库中文件的 ID 号。 默认值为 NULL，但对于此参数发送 NULL 值将导致错误。

*PageId* |NULL |缺省值   
页面的 ID。  *PageId* 为 **int**。 有效输入是 *FileId* 指定的文件中的页面 ID 号。 默认值为 NULL，但对于此参数发送 NULL 值将导致错误。

*模式* |NULL |缺省值   
确定函数的输出中的详细信息级别。 "受限" 将为所有说明列返回 NULL 值，"详细信息" 将填充说明列。  默认值为 "受限"。

## <a name="table-returned"></a>返回的表  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id |int |数据库 ID |
|file_id |int |文件 ID |
|page_id |int |页面 ID |
|page_header_version |int |页面页眉版本 |
|page_type |int |页面类型 |
|page_type_desc |nvarchar (64)  |页类型的说明 |
|page_type_flag_bits |nvarchar (64)  |在页眉中键入标志位 |
|page_type_flag_bits_desc |nvarchar (64)  |在页眉中键入标志位说明 |
|page_flag_bits |nvarchar (64)  |标志页眉中的位 |
|page_flag_bits_desc |nvarchar(256) |标志页眉中的位说明 |
|page_lsn |nvarchar (64)  |日志序列号/时间戳 |
|page_level |int |索引 (叶级中的页级别 = 0)  |
|object_id |int |拥有该页的对象的 ID |
|index_id |int |堆数据页的索引 (0 的 ID)  |
|partition_id |bigint |分区的 ID |
|alloc_unit_id |bigint |分配单元的 ID |
|is_encrypted |bit |用于指示该页是否已加密的位 |
|has_checksum |bit |用于指示页面是否具有校验和值的位 |
|校验和 (checksum) |int |存储用于检测数据损坏的校验和值 |
|is_iam_pg |bit |用于指示该页是否为 IAM 页的位  |
|is_mixed_ext |bit |用于指示在混合区中分配的位 |
|has_ghost_records |bit |用于指示该页是否包含虚影记录的位 <br> 幻像记录是已标记为要删除但尚未删除的记录。|
|has_version_records |bit |用于指示该页是否包含用于[加速数据库恢复](../backup-restore/restore-and-recovery-overview-sql-server.md#adr)的版本记录的位 |
|pfs_page_id |int |对应的 PFS 页的页 ID |
|pfs_is_allocated |bit |指示是否在相应的 PFS 页中将该页标记为已分配的位 |
|pfs_alloc_percent |int |按相应的 PFS 字节指示的分配百分比 |
|pfs_status |nvarchar (64)  |PFS 字节 |
|pfs_status_desc |nvarchar (64)  |PFS 字节的说明 |
|gam_page_id |int |对应的 GAM 页面的页面 ID |
|gam_status |bit |要指示是否已在 GAM 中分配的位 |
|gam_status_desc |nvarchar (64)  |GAM 状态位的说明 |
|sgam_page_id |int |对应的 SGAM 页的页 ID |
|sgam_status |bit |要指示是否在 SGAM 中分配的位 |
|sgam_status_desc |nvarchar (64)  |SGAM 状态位的说明 |
|diff_map_page_id |int |对应的差异位图页面的页面 ID |
|diff_status |bit |用于指示差异状态是否更改的位 |
|diff_status_desc |nvarchar (64)  |差异状态位的说明 |
|ml_map_page_id |int |对应的最小日志记录位图页的页 ID |
|ml_status |bit |用于指示是否按最小方式记录页面的位 |
|ml_status_desc |nvarchar (64)  |最小日志记录状态位的说明 |
|prev_page_file_id |smallint |上一页文件 ID |
|prev_page_page_id |int |上一页页面 ID |
|next_page_file_id |smallint |下一页文件 ID |
|next_page_page_id |int |下一页页 ID |
|fixed_length |smallint |固定大小行的长度 |
|slot_count |smallint | (使用的和未使用的槽总数)  <br> 对于数据页，此数字与行数等效。 |
|ghost_rec_count |smallint |页面上标记为 ghost 的记录数 <br> 幻像记录是已标记为要删除但尚未删除的记录。 |
|free_bytes |smallint |页面上的可用字节数 |
|free_data_offset |int |数据区域末尾的可用空间偏移量 |
|reserved_bytes |smallint |如果堆) ，则为所有事务 (保留的可用字节数 <br> 索引叶)  (的幻像行数 |
|reserved_bytes_by_xdes_id |smallint |M_xdesID 分配给 m_reservedCnt 的空间 <br> 仅用于调试目的 |
|xdes_id |nvarchar (64)  |M_reserved 提供的最新事务 <br> 仅用于调试目的 |
||||

## <a name="remarks"></a>备注
`sys.dm_db_page_info`动态管理函数将返回 `page_id` 页面信息，如、、等 `file_id` `index_id` `object_id` 。 此信息可用于故障排除和调试各种性能 (锁和闩锁争用) 和损坏问题。

`sys.dm_db_page_info``DBCC PAGE`在许多情况下，可以使用来替代语句，但它仅返回页眉信息，而不返回页面的正文。 `DBCC PAGE` 对于需要页面的全部内容的用例，仍需要。

## <a name="using-in-conjunction-with-other-dmvs"></a>与其他 Dmv 结合使用
的一个重要用例是将 `sys.dm_db_page_info` 它与公开页信息的其他 dmv 结合起来。  为了便于此用例，添加了一个名为的新列， `page_resource` 它以8字节十六进制格式公开页面信息。 此列已添加到 `sys.dm_exec_requests` 中， `sys.sysprocesses` 并将根据需要添加到将来的其他 dmv。

新函数将 `sys.fn_PageResCracker` 采用 `page_resource` 作为输入，并输出包含、和的单个行 `database_id` `file_id` `page_id` 。  然后，可以使用此函数来简化或之间的联接 `sys.dm_exec_requests` `sys.sysprocesses` `sys.dm_db_page_info` 。

## <a name="permissions"></a>权限  
需要 `VIEW DATABASE STATE` 数据库中的权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-displaying-all-the-properties-of-a-page"></a>A. 显示页的所有属性
下面的查询将返回一行，其中包含给定的 `database_id` `file_id` 、、 `page_id` 与默认模式 ( "受限" 的组合的所有页面信息 ) 

```sql
SELECT *  
FROM sys.dm_db_page_info (5, 1, 15, DEFAULT)
```

### <a name="b-using-sysdm_db_page_info-with-other-dmvs"></a>B. 将 sys.dm_db_page_info 与其他 Dmv 配合使用 

`wait_resource` `sys.dm_exec_requests` 当行包含非 null 时，以下查询将返回每个由公开的行`page_resource`

```sql
SELECT page_info.* 
FROM sys.dm_exec_requests AS d  
CROSS APPLY sys.fn_PageResCracker (d.page_resource) AS r  
CROSS APPLY sys.dm_db_page_info(r.db_id, r.file_id, r.page_id, 'LIMITED') AS page_info
```

## <a name="see-also"></a>另请参阅  
[动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)     
[sys.fn_PageResCracker](../../relational-databases/system-functions/sys-fn-pagerescracker-transact-sql.md)


