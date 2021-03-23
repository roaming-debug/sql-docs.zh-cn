---
description: 'sys.query_store_wait_stats (Transact-sql) '
title: 'sys.query_store_wait_stats (Transact-sql) '
ms.custom: ''
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.query_store_wait_stats
- query_store_wait_stats
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_wait_stats catalog view
- sys.query_store_wait_stats catalog view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f8b5e578c5192114db39d4b3e4c24a75cf9a07a2
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833881"
---
# <a name="sysquery_store_wait_stats-transact-sql"></a>sys.query_store_wait_stats (Transact-sql) 

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  包含有关查询的等待信息的信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**wait_stats_id**|**bigint**|表示 plan_id、runtime_stats_interval_id、execution_type 和 wait_category 的等待统计信息的行的标识符。 只有过去运行时统计信息间隔才是唯一的。 对于当前处于活动状态的时间间隔，可能有多个行表示 plan_id 引用的计划的等待统计信息，并且执行类型由 execution_type 表示，而等待类别由 wait_category 表示。 通常，一行代表刷新到磁盘的等待统计信息，而其他 () 表示内存中状态。 因此，若要获取每个间隔的实际状态，需要聚合指标，按 plan_id、runtime_stats_interval_id、execution_type 和 wait_category 进行分组。 |  
|**plan_id**|**bigint**|外键。 联接到 [&#40;transact-sql&#41;sys.query_store_plan ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)。|  
|**runtime_stats_interval_id**|**bigint**|外键。 联接到 [&#40;transact-sql&#41;sys.query_store_runtime_stats_interval ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)。|  
|**wait_category**|**tinyint**|使用下表对等待类型进行分类，然后在这些等待类别中聚合等待时间。 不同的等待类别需要不同的跟进分析来解决该问题，但在同一类别中等待的类型会导致类似的故障排除体验，并且除了等待外，还提供受影响的查询。|
|**wait_category_desc**|**nvarchar(128)**|有关 "等待类别" 字段的文本说明，请查看下表。|
|**execution_type**|**tinyint**|确定查询执行的类型：<br /><br /> 0-常规执行 (成功完成) <br /><br /> 3-客户端启动的已中止执行<br /><br /> 4-异常中止执行|  
|**execution_type_desc**|**nvarchar(128)**|执行类型字段的文本说明：<br /><br /> 0-常规<br /><br /> 3-已中止<br /><br /> 4-异常|  
|**total_query_wait_time_ms**|**bigint**|`CPU wait`查询计划在聚合间隔和等待类别内的总时间（以毫秒为单位） (报告) 。|
|**avg_query_wait_time_ms**|**float**|聚合间隔和等待类别中每次执行的查询计划的平均等待持续时间，以毫秒)  (报告。|
|**last_query_wait_time_ms**|**bigint**|聚合间隔和等待类别中查询计划的上次等待持续时间，以毫秒)  (报告。|
|**min_query_wait_time_ms**|**bigint**|`CPU wait`查询计划 (报告的聚合间隔和等待类别中的最小时间（以毫秒为单位）) 。|
|**max_query_wait_time_ms**|**bigint**|`CPU wait`查询计划 (报告的聚合间隔和等待类别中的最长时间（以毫秒为单位）) 。|
|**stdev_query_wait_time_ms**|**float**|`Query wait` 聚合间隔和等待类别中查询计划的持续时间标准偏差，以毫秒)  (报告。|

## <a name="wait-categories-mapping-table"></a>等待类别映射表

"%" 用作通配符
  
|整数值|等待类别|等待类型包括在类别中|  
|-----------------|---------------|-----------------|  
|**0**|**Unknown**|未知 |  
|**1**|**CPU**|SOS_SCHEDULER_YIELD|
|**2**|**工作线程**|THREADPOOL|
|**3**|**Lock**|LCK_M_%|
|**4**|**闩锁**|LATCH_%|
|**5**|**缓冲区闩锁**|PAGELATCH_%|
|**6**|**缓冲区 IO**|PAGEIOLATCH_%|
|**7**|**汇编***|RESOURCE_SEMAPHORE_QUERY_COMPILE|
|**8**|**SQL CLR**|CLR%，SQLCLR%|
|**9**|**镜像**|DBMIRROR%|
|**10**|**事务**|事务%，DTC%，TRAN_MARKLATCH_%，MSQL_XACT_%，TRANSACTION_MUTEX|
|**11**|**时间**|SLEEP_%、LAZYWRITER_SLEEP、SQLTRACE_BUFFER_FLUSH、SQLTRACE_INCREMENTAL_FLUSH_SLEEP、SQLTRACE_WAIT_ENTRIES、FT_IFTS_SCHEDULER_IDLE_WAIT、XE_DISPATCHER_WAIT、REQUEST_FOR_DEADLOCK_SEARCH、LOGMGR_QUEUE、ONDEMAND_TASK_QUEUE、CHECKPOINT_QUEUE、XE_TIMER_EVENT|
|**12**|**预防**|PREEMPTIVE_%|
|**13**|**Service Broker**|BROKER_% **(但不 BROKER_RECEIVE_WAITFOR)**|
|**14**|**事务日志 IO**|数据库准备、LOGBUFFER、LOGMGR_RESERVE_APPEND、LOGMGR_FLUSH、LOGMGR_PMM_LOG、CHKPT.、WRITELOG|
|**15**|**网络 IO**|ASYNC_NETWORK_IO、NET_WAITFOR_PACKET、PROXY_NETWORK_IO EXTERNAL_SCRIPT_NETWORK_IOF|
|**16**|**并行度**|CXPACKET、EXCHANGE、HT%、BMP%、BP%|
|**17**|**内存**|RESOURCE_SEMAPHORE、CMEMTHREAD、CMEMPARTITIONED、EE_PMOLOCK、MEMORY_ALLOCATION_EXT、RESERVED_MEMORY_ALLOCATION_EXT、MEMORY_GRANT_UPDATE|
|**18**|**用户等待**|WAITFOR、WAIT_FOR_RESULTS、BROKER_RECEIVE_WAITFOR|
|**19**|**跟踪**|TRACEWRITE、SQLTRACE_LOCK、SQLTRACE_FILE_BUFFER、SQLTRACE_FILE_WRITE_IO_COMPLETION、SQLTRACE_FILE_READ_IO_COMPLETION、SQLTRACE_PENDING_BUFFER_WRITERS、SQLTRACE_SHUTDOWN、QUERY_TRACEOUT、TRACE_EVTNOTIFF|
|**20**|**全文搜索**|FT_RESTART_CRAWL、全文收集、MSSEARCH、FT_METADATA_MUTEX、FT_IFTSHC_MUTEX、FT_IFTSISM_MUTEX、FT_IFTS_RWLOCK、FT_COMPROWSET_RWLOCK、FT_MASTER_MERGE、FT_PROPERTYLIST_CACHE、FT_MASTER_MERGE_COORDINATOR、PWAIT_RESOURCE_SEMAPHORE_FT_PARALLEL_QUERY_SYNC|
|**21**|**其他磁盘 IO**|ASYNC_IO_COMPLETION、IO_COMPLETION、BACKUPIO、WRITE_COMPLETION、IO_QUEUE_LIMIT、IO_RETRY|
|22 |**复制**|SE_REPL_%，REPL_%，HADR_% **(但不 HADR_THROTTLE_LOG_RATE_GOVERNOR**) PWAIT_HADR_、REPLICA_WRITES%、FCB_REPLICA_WRITE、FCB_REPLICA_READ、PWAIT_HADRSIM、|
|23 |**日志速率调控器**|LOG_RATE_GOVERNOR、POOL_LOG_RATE_GOVERNOR、HADR_THROTTLE_LOG_RATE_GOVERNOR INSTANCE_LOG_RATE_GOVERNOR|

\* 查询存储只跟踪查询 *执行* 期间的等待统计信息，而不会在查询 *编译* 期间进行跟踪。 这会限制查询存储跟踪编译等待统计信息的能力。

## <a name="permissions"></a>权限

 需要 `VIEW DATABASE STATE` 权限。  
  
## <a name="see-also"></a>另请参阅

- [sys.database_query_store_options (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)
- [sys.query_context_settings (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)
- [sys.query_store_plan (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)
- [sys.query_store_query (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)
- [sys.query_store_query_text (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)
- [sys.query_store_runtime_stats_interval (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)
- [使用查询存储来监视性能](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [查询存储存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
