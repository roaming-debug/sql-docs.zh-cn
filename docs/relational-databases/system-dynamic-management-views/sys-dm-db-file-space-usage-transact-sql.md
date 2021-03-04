---
description: sys.dm_db_file_space_usage (Transact-SQL)
title: sys.dm_db_file_space_usage (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_file_space_usage
- sys.dm_db_file_space_usage_TSQL
- sys.dm_db_file_space_usage
- dm_db_file_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_file_space_usage dynamic management view
ms.assetid: 148a5276-a8d5-49d2-8146-3c63d24c2144
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 855573b289bca455d8acdfbb60220c4e5f95a60b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837739"
---
# <a name="sysdm_db_file_space_usage-transact-sql"></a>sys.dm_db_file_space_usage (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回数据库中每个数据文件的空间使用情况信息。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_db_file_space_usage**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**smallint**|数据库 ID。|  
|file_id|**smallint**|文件 ID。<br /><br /> file_id 映射到[sys.sys文件](../../relational-databases/system-compatibility-views/sys-sysfiles-transact-sql.md)的[sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md)和 fileid 中 file_id。|  
|filegroup_id|**smallint**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 文件组 ID。|  
|total_page_count|**bigint**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 数据文件中的总页数。|  
|allocated_extent_page_count|**bigint**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 数据文件的已分配区中的总页数。|  
|unallocated_extent_page_count|**bigint**|数据文件的未分配区中的总页数。<br /><br /> 不包括已分配区中的未使用页。|  
|version_store_reserved_page_count|**bigint**|为版本存储分配的统一区中的总页数。 永远不会从混合区分配版本存储页。<br /><br /> 不包括 IAM 页，因为 IAM 页始终从混合区进行分配。 如果 PFS 页是从统一区分配的，则包括 PFS 页。<br /><br /> 有关详细信息，请参阅 [sys.dm_tran_version_store (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md)。|  
|user_object_reserved_page_count|**bigint**|从统一区为数据库中的用户对象分配的总页数。 计数中包括已分配区中未使用的页。<br /><br /> 不包括 IAM 页，因为 IAM 页始终从混合区进行分配。 如果 PFS 页是从统一区分配的，则包括 PFS 页。<br /><br /> 您可以使用 [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md) 目录视图中的 total_pages 列来返回用户对象中每个分配单元的保留页计数。 但请注意，total_pages 列包括 IAM 页。|  
|internal_object_reserved_page_count|**bigint**|从统一区为文件中的内部对象分配的总页数。 计数中包括已分配区中未使用的页。<br /><br /> 不包括 IAM 页，因为 IAM 页始终从混合区进行分配。 如果 PFS 页是从统一区分配的，则包括 PFS 页。<br /><br /> 不存在可返回每个内部对象的页计数的目录视图或动态管理对象。|  
|mixed_extent_page_count|**bigint**|文件的已分配混合区中的已分配和未分配总页数。 混合区包含分配给不同对象的页。 此计数包含文件中的所有 IAM 页。|
|modified_extent_page_count|**bigint**|**适用对象**：[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 和更高版本。<br /><br />自上次完整数据库备份以来，在文件分配的范围内修改的总页数。 修改后的页计数可用于跟踪自上次完整备份以来数据库的差异变化量，以确定是否需要进行差异备份。|
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
|distribution_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 与分布关联的唯一数字 id。|  
  
## <a name="remarks"></a>备注  
 页计数始终为区级计数。 所以，页计数的值始终为八的倍数。 包含全局分配映射表 (GAM) 和共享全局分配映射表 (SGAM) 分配页的区是已分配的统一区。 它们不包含在上文所述的页计数中。 有关页和区的详细信息，请参阅 [页和区体系结构指南](../../relational-databases/pages-and-extents-architecture-guide.md)。 
  
 当前版本存储区的内容处于 [sys.dm_tran_version_store](../../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md)。 在文件级而不是会话级和任务级跟踪版本存储页，因为它们是全局资源。 会话会生成版本，但在会话结束时不能删除版本。 版本存储清除必须考虑需要访问特定版本的运行时间最长的事务。 可以通过查看 [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md)中的 "elapsed_time_seconds" 列来发现与版本存储清除相关的运行时间最长的事务。  
  
 mixed_extent_page_count 列频繁更改可能指示大量使用 SGAM 页。 如出现此情况，您会看到多个 PAGELATCH_UP 等待，且正在等待 SGAM 页资源。 有关详细信息，请参阅 [sys.dm_os_waiting_tasks &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md)、 [sys.dm_os_wait_stats &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)和 sys.dm_os_latch_stats [&#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)。  
  
## <a name="user-objects"></a>用户对象  
 用户对象页计数器中包括下列对象：  
  
-   用户定义的表和索引  
  
-   系统表和索引  
  
-   全局临时表和索引  
  
-   局部临时表和索引  
  
-   表变量  
  
-   表值函数中返回的表  
  
## <a name="internal-objects"></a>内部对象  
 内部对象只包含在 tempdb 中。 内部对象页计数器中包括下列对象：  
  
-   用于游标或假脱机操作以及临时大型对象 (LOB) 存储的工作表  
  
-   用于哈希联接等操作的工作文件  
  
-   排序段  
  
## <a name="relationship-cardinalities"></a>关系基数  
  
|From|功能|Relationship|  
|----------|--------|------------------|  
|sys.dm_db_file_space_usage.database_id、file_id|sys.dm_io_virtual_file_stats.database_id、file_id|一对一|  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="examples"></a>示例  
  
### <a name="determing-the-amount-of-free-space-in-tempdb"></a>确定 tempdb 中的可用空间量  
 以下查询将返回 **tempdb** 中所有数据文件中可用的可用页总数和总可用空间（MB (mb）) 。  
  
```sql
USE tempdb;  
GO  
SELECT SUM(unallocated_extent_page_count) AS [free pages],   
(SUM(unallocated_extent_page_count)*1.0/128) AS [free space in MB]  
FROM sys.dm_db_file_space_usage;  
```  

### <a name="determining-the-amount-of-space-used-by-user-objects"></a>确定用户对象使用的空间量  
 下面的查询将返回 tempdb 中用户对象使用的总页数和总空间量。  
  
```sql  
USE tempdb;  
GO  
SELECT SUM(user_object_reserved_page_count) AS [user object pages used],  
(SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]  
FROM sys.dm_db_file_space_usage;
```  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_db_task_space_usage &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md)   
 [sys.dm_db_session_space_usage (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)