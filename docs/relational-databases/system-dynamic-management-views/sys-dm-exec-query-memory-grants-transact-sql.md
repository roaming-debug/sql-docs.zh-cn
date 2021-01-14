---
description: sys.dm_exec_query_memory_grants (Transact-SQL)
title: sys.dm_exec_query_memory_grants (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_memory_grants_TSQL
- sys.dm_exec_query_memory_grants
- sys.dm_exec_query_memory_grants_TSQL
- dm_exec_query_memory_grants
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_memory_grants dynamic management view
ms.assetid: 2c417747-2edd-4e0d-8a9c-e5f445985c1a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 147bf57a568460c0b1df68fc4e232432bef3b834
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170279"
---
# <a name="sysdm_exec_query_memory_grants-transact-sql"></a>sys.dm_exec_query_memory_grants (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回有关已请求并正在等待内存授予或已获得内存授予的所有查询的信息。 不需要内存授予的查询将不会出现在此视图中。 例如，排序和哈希联接操作具有用于执行查询的内存授予，而没有 **ORDER by** 子句的查询将不具有内存授予。  
  
 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，动态管理视图不能公开将影响数据库包含的信息，也不能公开有关用户可以访问的其他数据库的信息。 为了避免公开此信息，每个包含不属于所连接的租户的数据的行都将被筛选掉。此外，筛选列中的值 **scheduler_id**、 **wait_order**、 **pool_id** 和 **group_id** 。列值设置为 NULL。  
  
> [!NOTE]  
> 若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_exec_query_memory_grants**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**session_id**|**smallint**|正在运行查询的会话 ID (SPID)。|  
|request_id|**int**|请求的 ID。 在会话的上下文中是唯一的。|  
|**scheduler_id**|**int**|正在计划查询的计划程序的 ID。|  
|**dop**|**smallint**|查询的并行度。|  
|**request_time**|**datetime**|查询请求内存授予的日期和时间。|  
|**grant_time**|**datetime**|向查询授予内存的日期和时间。 如果尚未授予内存，则此值为 NULL。|  
|**requested_memory_kb**|**bigint**|请求的内存总量 (KB)。|  
|**granted_memory_kb**|**bigint**|实际授予的内存总量 (KB)。 如果尚未授予内存，该值可以为 NULL。 在典型情况下，该值应与 **requested_memory_kb** 相同。 创建索引时，除了初始授予的内存外，服务器还允许增加按需分配的内存。|  
|**required_memory_kb**|**bigint**|运行查询所需的最小内存 (KB)。 **requested_memory_kb** 与此数量相同或更大。|  
|**used_memory_kb**|**bigint**|此刻使用的物理内存 (KB)。|  
|**max_used_memory_kb**|**bigint**|到此刻为止所用的最大物理内存 (KB)。|  
|**query_cost**|**float**|估计查询开销。|  
|**timeout_sec**|**int**|查询放弃内存授予请求前的超时时间（秒）。|  
|**resource_semaphore_id**|**smallint**|此查询正在等待的资源信号量的非唯一 ID。<br /><br /> **注意：** 此 ID 在早于的版本中是唯一的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 。 此更改会对故障排除查询执行造成影响。 有关详细信息，请参阅本主题后面的“备注”部分。|  
|**queue_id**|**smallint**|查询等待内存授予时所在等待队列的 ID。 如果已授予内存，则为 NULL。|  
|**wait_order**|**int**|等待查询在指定的 **queue_id** 中的顺序。 如果其他查询获得内存授予或超时，则给定查询的此值可能会发生变化。如果已授予内存，则为 NULL。|  
|**is_next_candidate**|**bit**|下一个内存授予的候选对象。<br /><br /> 1 = 是<br /><br /> 0 = 否<br /><br /> NULL = 已授予内存。|  
|**wait_time_ms**|**bigint**|等待时间（毫秒）。 如果已授予内存，则为 NULL。|  
|**plan_handle**|**varbinary(64)**|查询计划的标识符。 使用 **sys.dm_exec_query_plan** 可提取实际的 XML 计划。|  
|**sql_handle**|**varbinary(64)**|查询的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 文本标识符。 使用 **sys.dm_exec_sql_text** 可获取实际的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 文本。|  
|**group_id**|**int**|在其中运行此查询的工作负荷组的 ID。|  
|**pool_id**|**int**|该工作负荷组所属的资源池的 ID。|  
|**is_small**|**tinyint**|如果设置为 1，则指示此授予使用小型资源信号量。 如果设置为 0，则指示使用常规信号量。|  
|**ideal_memory_kb**|**bigint**|将所有内容存放在物理内存中所需的内存授予的大小（以 KB 为单位）。 这基于基数估计。|  
|pdw_node_id|**int**|此分发所在的节点的标识符。<br /><br /> **适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] |  
|**reserved_worker_count**|**bigint**|保留的 [工作线程](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling)数。<br /><br />**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |  
|**used_worker_count**|**bigint**|目前使用的 [工作线程](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) 数。<br /><br />**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**max_used_worker_count**|**bigint**|当前使用的最大 [工作线程](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) 数。<br /><br />**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**reserved_node_bitmap**|**bigint**|保留 [工作线程](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) 的 NUMA 节点的位图。<br /><br />**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 上，需要在数据库中拥有 `VIEW DATABASE STATE` 权限。   
   
## <a name="remarks"></a>备注  
 查询超时的典型调试情况可能如下所示：  
  
-   使用 [sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)、[sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) 和各种性能计数器检查总体系统内存状态。  
  
-   检查 **sys.dm_os_memory_clerks**（其中 `type = 'MEMORYCLERK_SQLQERESERVATIONS'`）中的查询执行内存预留。  
  
-   使用 **sys.dm_exec_query_memory_grants** 检查正在等待 <sup>1</sup>的请求。  
  
    ```sql  
    --Find all queries waiting in the memory queue  
    SELECT * FROM sys.dm_exec_query_memory_grants where grant_time is null  
    ```  
    
    <sup>1</sup>在此方案中，等待类型通常是 RESOURCE_SEMAPHORE。 有关详细信息，请参阅 [sys.dm_os_wait_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)。 
  
-   使用内存授予的查询的搜索缓存使用 [sys.dm_exec_cached_plans &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md) ，并 [sys.dm_exec_query_plan &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)  
  
    ```sql  
    -- retrieve every query plan from the plan cache  
    USE master;  
    GO  
    SELECT * FROM sys.dm_exec_cached_plans cp CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
    GO  
    ```  
  
-   使用 [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) 进一步检查要占用大量内存的查询。  
  
    ```sql  
    --Find top 5 queries by average CPU time  
    SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
      plan_handle, query_plan   
    FROM sys.dm_exec_query_stats AS qs  
    CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
    ORDER BY total_worker_time/execution_count DESC;  
    GO  
    ```  
  
-   如果怀疑有失控查询，请检查来自 [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md) 的显示计划和来自 [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) 的批处理文本。  
  
 使用包含或聚合的动态管理视图的查询 `ORDER BY` 可能会增加内存占用，从而导致其故障排除的问题。  
  
 数据库管理员可以使用资源调控器功能在多个资源池之间分发服务器资源，最多可为 64 个池。 从开始 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ，每个池的行为类似于小型独立的服务器实例，并且需要2个信号量。 从 **sys.dm_exec_query_resource_semaphores** 返回的行数可以比中返回的行多20倍 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_exec_query_resource_semaphores &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-resource-semaphores-transact-sql.md)     
 [sys.dm_os_wait_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)     
 [与执行相关的动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)    
 [线程和任务体系结构指南](../../relational-databases/thread-and-task-architecture-guide.md)   
  
