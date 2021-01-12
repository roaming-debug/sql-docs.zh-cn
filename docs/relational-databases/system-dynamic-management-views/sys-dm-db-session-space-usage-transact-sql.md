---
description: sys.dm_db_session_space_usage (Transact-SQL)
title: sys.dm_db_session_space_usage (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/16/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_session_space_usage_TSQL
- dm_db_session_space_usage
- sys.dm_db_session_space_usage
- sys.dm_db_session_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_session_space_usage dynamic management view
ms.assetid: a67a6045-8e14-460a-9fe3-912b846c08c1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 45fc95bdfa5a1dabe3b8b50e5acb4b05a09d0998
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096644"
---
# <a name="sysdm_db_session_space_usage-transact-sql"></a>sys.dm_db_session_space_usage (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回每个会话为数据库分配和释放的页数。  
  
> [!NOTE]  
>  此视图仅适用于 [tempdb 数据库](../../relational-databases/databases/tempdb-database.md)。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_db_session_space_usage**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**session_id**|**smallint**|会话 ID。<br /><br /> **session_id** 映射到 [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)中 **session_id** 。|  
|database_id|**smallint**|数据库 ID。|  
|**user_objects_alloc_page_count**|**bigint**|由该会话为用户对象保留或分配的页数。|  
|**user_objects_dealloc_page_count**|**bigint**|由该会话释放并不再为用户对象保留的页数。|  
|**internal_objects_alloc_page_count**|**bigint**|由该会话为内部对象保留或分配的页数。|  
|**internal_objects_dealloc_page_count**|**bigint**|由该会话释放并不再为内部对象保留的页数。|  
|**user_objects_deferred_dealloc_page_count**|**bigint**|已标记为要延迟释放的页数。<br /><br /> **注意：** 在和的 service pack 中引入 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="remarks"></a>备注  
 由该视图报告的分配或释放的计数不包括 IAM 页。  
  
 在会话开始时，页计数器被初始化为零 (0)。 计数器跟踪为会话中已经完成的任务分配或释放的总页数。 仅当任务结束时，该计数器才更新；它们不反映正在运行的任务。  
  
 会话可同时包含多个活动请求。 如果是并行查询，则请求可启动多个线程和任务。  
  
 有关会话、请求和任务的详细信息，请参阅 [sys.dm_exec_sessions &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)、 [sys.dm_exec_requests &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)和 sys.dm_os_tasks [&#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)。  
  
## <a name="user-objects"></a>用户对象  
 用户对象页计数器中包括下列对象：  
  
-   用户定义的表和索引  
  
-   系统表和索引  
  
-   全局临时表和索引  
  
-   局部临时表和索引  
  
-   表变量  
  
-   表值函数中返回的表  
  
## <a name="internal-objects"></a>内部对象  
 内部对象仅在 **tempdb** 中。 内部对象页计数器中包括下列对象：  
  
-   用于游标或假脱机操作以及临时大型对象 (LOB) 存储的工作表  
  
-   用于哈希联接等操作的工作文件  
  
-   排序段  
  
## <a name="physical-joins"></a>物理联接  
 ![sys.dm_db_session_space_usage 的物理联接](../../relational-databases/system-dynamic-management-views/media/join-dm-db-session-space-usage-1.gif "sys.dm_db_session_space_usage 的物理联接")  
  
## <a name="relationship-cardinalities"></a>关系基数  
  
|From|目标|Relationship|  
|----------|--------|------------------|  
|dm_db_session_space_usage.session_id|dm_exec_sessions.session_id|一对一|  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_exec_sessions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
 [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_os_tasks &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)   
 [sys.dm_db_task_space_usage &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md)   
 [sys.dm_db_file_space_usage (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md)  
  
  



