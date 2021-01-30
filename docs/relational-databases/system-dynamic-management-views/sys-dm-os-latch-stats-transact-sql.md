---
description: sys.dm_os_latch_stats (Transact-SQL)
title: sys.dm_os_latch_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_latch_stats_TSQL
- dm_os_latch_stats_TSQL
- dm_os_latch_stats
- sys.dm_os_latch_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_latch_stats dynamic management view
ms.assetid: 2085d9fc-828c-453e-82ec-b54ed8347ae5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 84858ab6559e98aa4f343f6a53d497d60ffac0f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184926"
---
# <a name="sysdm_os_latch_stats-transact-sql"></a>sys.dm_os_latch_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回有关按类组织的所有闩锁等待的信息。 
  
> [!NOTE]  
> 若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_latch_stats**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|latch_class|**nvarchar(60)**|闩锁类的名称。|  
|waiting_requests_count|**bigint**|此类中的闩锁等待的个数。 此计数器在闩锁等待启动时递增。|  
|wait_time_ms|**bigint**|此类中闩锁的总计等待时间（毫秒）。<br /><br /> **注意：** 此列每五分钟更新一次，在闩锁等待期间，在闩锁等待结束时进行更新。|  
|max_wait_time_ms|**bigint**|内存对象已等待此闩锁的最大时间。 如果此值异常高，则可能指示有内部死锁。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限  
在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="remarks"></a>备注  
 通过检查不同闩锁类的相对等待数和等待时间，sys.dm_os_latch_stats 可以用来标识闩锁争用源。 在某些情况中，可能能够解决或减少闩锁争用。 但是，在某些情况下可能需要与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 客户支持服务部门联系。  
  
按如下所示使用 `DBCC SQLPERF`，可以重置 sys.dm_os_latch_stats 的内容：  
  
```sql  
DBCC SQLPERF ('sys.dm_os_latch_stats', CLEAR);  
GO  
```  
  
 这将使所有计数器重置为 0。  
  
> [!NOTE]  
>  如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 重新启动，则这些统计信息不会持久化。 自从上次统计信息重置以来，或自从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动以来，所有数据都是累积的。  
  
## <a name="latches"></a><a name="latches"></a> 栓  
 闩锁是类似于锁的内部轻型同步对象，由各种 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件使用。 闩锁主要用于在操作（如缓冲区或文件访问）期间同步数据库页。 每个闩锁与单个分配单元关联。 
  
 由于闩锁由冲突模式中的另一个线程持有，所以当无法立即满足闩锁请求时，就会发生闩锁等待。 与锁不同，在操作之后，甚至在写入操作中，会立即释放闩锁。  
  
 闩锁根据组件和用法划分为不同的类。 特定类的零个或更多个闩锁可以存在于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的任何时点上。  
  
> [!NOTE]  
> `sys.dm_os_latch_stats` 不跟踪被立即授予的或失败而不等待的闩锁请求。  
  
 下表包含对各种闩锁类的简短说明。  
  
|闩锁类|说明|  
|-----------------|-----------------|  
|ALLOC_CREATE_RINGBUF|供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部使用，用于初始化对分配环形缓冲区创建过程的同步。|  
|ALLOC_CREATE_FREESPACE_CACHE|用来初始化对堆的内部可用空间缓存的同步。|  
|ALLOC_CACHE_MANAGER|用于同步内部一致性测试。|  
|ALLOC_FREESPACE_CACHE|用于同步对其空间可用于堆和二进制大型对象 (BLOB) 的页缓存的访问。 如果多个连接试图同时将行插入堆或 BLOB 中，则会发生对此类闩锁的争用。 通过将该对象分区，可以减少此争用。 每个分区都有它自己的闩锁。 分区将插入操作分散到多个闩锁。|  
|ALLOC_EXTENT_CACHE|用于同步对包含未分配页的区数缓存的访问。 如果多个连接试图同时在相同的分配单元中分配数据页，则会发生对此类闩锁的争用。 通过将此分配单元属于其一部分的对象进行分区，可以减少此争用。|  
|ACCESS_METHODS_DATASET_PARENT|用于同步在并行操作期间对父数据集进行的子数据集访问。|  
|ACCESS_METHODS_HOBT_FACTORY|用于同步对内部哈希表的访问。|  
|ACCESS_METHODS_HOBT|用于同步对 HoBt 的内存中表示的访问。|  
|ACCESS_METHODS_HOBT_COUNT|用于同步对 HoBt 页和行计数器的访问。|  
|ACCESS_METHODS_HOBT_VIRTUAL_ROOT|用于同步对内部 B 树的根页抽象的访问。|  
|ACCESS_METHODS_CACHE_ONLY_HOBT_ALLOC|用于同步工作表访问。|  
|ACCESS_METHODS_BULK_ALLOC|用于同步大容量分配器中的访问。|  
|ACCESS_METHODS_SCAN_RANGE_GENERATOR|用于同步在并行扫描期间对范围生成器的访问。|  
|ACCESS_METHODS_KEY_RANGE_GENERATOR|用于同步在关键范围并行扫描期间对预读操作的访问。|  
|APPEND_ONLY_STORAGE_INSERT_POINT|用于同步在快速仅限追加存储单元中的插入。|  
|APPEND_ONLY_STORAGE_FIRST_ALLOC|用于同步仅限追加存储单元的第一次分配。|  
|APPEND_ONLY_STORAGE_UNIT_MANAGER|用于在快速仅限追加存储单元管理器中进行内部数据结构访问同步。|  
|APPEND_ONLY_STORAGE_MANAGER|用于同步在快速仅限追加存储单元管理器中的收缩操作。|  
|BACKUP_RESULT_SET|用于同步并行备份结果集。|  
|BACKUP_TAPE_POOL|用于同步备份磁带池。|  
|BACKUP_LOG_REDO|用于同步备份日志重做操作。|  
|BACKUP_INSTANCE_ID|用于同步为备份性能监视计数器生成实例 ID 的过程。|  
|BACKUP_MANAGER|用于同步内部备份管理器。|  
|BACKUP_MANAGER_DIFFERENTIAL|用于同步用 DBCC 执行的差异备份操作。|  
|BACKUP_OPERATION|用于备份操作（例如，数据库、日志或文件备份）中的内部数据结构同步。|  
|BACKUP_FILE_HANDLE|用于同步在还原操作期间的文件打开操作。|  
|BUFFER|用于同步对数据库页的短期访问。 读取或修改任何数据库页之前，必须使用缓冲区闩锁。 缓冲区闩锁争用可以指示出现了几个问题，包括热页和缓慢 I/O。<br /><br /> 此闩锁类覆盖所有可能的页闩锁使用。 sys.dm_os_wait_stats 在页闩锁等待期间（即，对页面上的 i/o 操作和读和写操作造成了不同）。|  
|BUFFER_POOL_GROW|用于在缓冲区池增长操作期间的内部缓冲区管理器同步。|  
|DATABASE_CHECKPOINT|用于序列化数据库中的检查点。|  
|CLR_PROCEDURE_HASHTABLE|仅限内部使用。|  
|CLR_UDX_STORE|仅限内部使用。|  
|CLR_DATAT_ACCESS|仅限内部使用。|  
|CLR_XVAR_PROXY_LIST|仅限内部使用。|  
|DBCC_CHECK_AGGREGATE|仅限内部使用。|  
|DBCC_CHECK_RESULTSET|仅限内部使用。|  
|DBCC_CHECK_TABLE|仅限内部使用。|  
|DBCC_CHECK_TABLE_INIT|仅限内部使用。|  
|DBCC_CHECK_TRACE_LIST|仅限内部使用。|  
|DBCC_FILE_CHECK_OBJECT|仅限内部使用。|  
|DBCC_PERF|用于同步内部性能监视器计数器。|  
|DBCC_PFS_STATUS|仅限内部使用。|  
|DBCC_OBJECT_METADATA|仅限内部使用。|  
|DBCC_HASH_DLL|仅限内部使用。|  
|EVENTING_CACHE|仅限内部使用。|  
|FCB|用于同步对文件控制块的访问。|  
|FCB_REPLICA|仅限内部使用。|  
|FGCB_ALLOC|用于同步对文件组中循环法分配信息的访问。|  
|FGCB_ADD_REMOVE|用于同步对文件组的访问，以便添加、删除、增大和收缩文件操作。|  
|FILEGROUP_MANAGER|仅限内部使用。|  
|FILE_MANAGER|仅限内部使用。|  
|FILESTREAM_FCB|仅限内部使用。|  
|FILESTREAM_FILE_MANAGER|仅限内部使用。|  
|FILESTREAM_GHOST_FILES|仅限内部使用。|  
|FILESTREAM_DFS_ROOT|仅限内部使用。|  
|LOG_MANAGER|仅限内部使用。|  
|FULLTEXT_DOCUMENT_ID|仅限内部使用。|  
|FULLTEXT_DOCUMENT_ID_TRANSACTION|仅限内部使用。|  
|FULLTEXT_DOCUMENT_ID_NOTIFY|仅限内部使用。|  
|FULLTEXT_LOGS|仅限内部使用。|  
|FULLTEXT_CRAWL_LOG|仅限内部使用。|  
|FULLTEXT_ADMIN|仅限内部使用。|  
|FULLTEXT_AMDIN_COMMAND_CACHE|仅限内部使用。|  
|FULLTEXT_LANGUAGE_TABLE|仅限内部使用。|  
|FULLTEXT_CRAWL_DM_LIST|仅限内部使用。|  
|FULLTEXT_CRAWL_CATALOG|仅限内部使用。|  
|FULLTEXT_FILE_MANAGER|仅限内部使用。|  
|DATABASE_MIRRORING_REDO|仅限内部使用。|  
|DATABASE_MIRRORING_SERVER|仅限内部使用。|  
|DATABASE_MIRRORING_CONNECTION|仅限内部使用。|  
|DATABASE_MIRRORING_STREAM|仅限内部使用。|  
|QUERY_OPTIMIZER_VD_MANAGER|仅限内部使用。|  
|QUERY_OPTIMIZER_ID_MANAGER|仅限内部使用。|  
|QUERY_OPTIMIZER_VIEW_REP|仅限内部使用。|  
|RECOVERY_BAD_PAGE_TABLE|仅限内部使用。|  
|RECOVERY_MANAGER|仅限内部使用。|  
|SECURITY_OPERATION_RULE_TABLE|仅限内部使用。|  
|SECURITY_OBJPERM_CACHE|仅限内部使用。|  
|SECURITY_CRYPTO|仅限内部使用。|  
|SECURITY_KEY_RING|仅限内部使用。|  
|SECURITY_KEY_LIST|仅限内部使用。|  
|SERVICE_BROKER_CONNECTION_RECEIVE|仅限内部使用。|  
|SERVICE_BROKER_TRANSMISSION|仅限内部使用。|  
|SERVICE_BROKER_TRANSMISSION_UPDATE|仅限内部使用。|  
|SERVICE_BROKER_TRANSMISSION_STATE|仅限内部使用。|  
|SERVICE_BROKER_TRANSMISSION_ERRORS|仅限内部使用。|  
|SSBXmitWork|仅限内部使用。|  
|SERVICE_BROKER_MESSAGE_TRANSMISSION|仅限内部使用。|  
|SERVICE_BROKER_MAP_MANAGER|仅限内部使用。|  
|SERVICE_BROKER_HOST_NAME|仅限内部使用。|  
|SERVICE_BROKER_READ_CACHE|仅限内部使用。|  
|SERVICE_BROKER_WAITFOR_MANAGER| 用于同步等待队列的实例级别映射。 每个数据库 ID、数据库版本和队列 ID 元组存在一个队列。 如果有多个连接，则会发生此类闩锁的争用：在 WAITFOR (接收) 等待状态;调用 WAITFOR (接收) ;超过 WAITFOR 超时;接收消息;提交或回滚包含 WAITFOR (接收) 的事务;可以通过减少 WAITFOR (接收) 等待状态中的线程数来减少争用。 |  
|SERVICE_BROKER_WAITFOR_TRANSACTION_DATA|仅限内部使用。|  
|SERVICE_BROKER_TRANSMISSION_TRANSACTION_DATA|仅限内部使用。|  
|SERVICE_BROKER_TRANSPORT|仅限内部使用。|  
|SERVICE_BROKER_MIRROR_ROUTE|仅限内部使用。|  
|TRACE_ID|仅限内部使用。|  
|TRACE_AUDIT_ID|仅限内部使用。|  
|TRACE|仅限内部使用。|  
|TRACE_CONTROLLER|仅限内部使用。|  
|TRACE_EVENT_QUEUE|仅限内部使用。|  
|TRANSACTION_DISTRIBUTED_MARK|仅限内部使用。|  
|TRANSACTION_OUTCOME|仅限内部使用。|  
|NESTING_TRANSACTION_READONLY|仅限内部使用。|  
|NESTING_TRANSACTION_FULL|仅限内部使用。|  
|MSQL_TRANSACTION_MANAGER|仅限内部使用。|  
|DATABASE_AUTONAME_MANAGER|仅限内部使用。|  
|UTILITY_DYNAMIC_VECTOR|仅限内部使用。|  
|UTILITY_SPARSE_BITMAP|仅限内部使用。|  
|UTILITY_DATABASE_DROP|仅限内部使用。|  
|UTILITY_DYNAMIC_MANAGER_VIEW|仅限内部使用。|  
|UTILITY_DEBUG_FILESTREAM|仅限内部使用。|  
|UTILITY_LOCK_INFORMATION|仅限内部使用。|  
|VERSIONING_TRANSACTION|仅限内部使用。|  
|VERSIONING_TRANSACTION_LIST|仅限内部使用。|  
|VERSIONING_TRANSACTION_CHAIN|仅限内部使用。|  
|VERSIONING_STATE|仅限内部使用。|  
|VERSIONING_STATE_CHANGE|仅限内部使用。|  
|KTM_VIRTUAL_CLOCK|仅限内部使用。|  
  
## <a name="see-also"></a>另请参阅  
[DBCC SQLPERF &#40;Transact-sql&#41;](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)       
[&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)       
[SQL Server Latches 对象](../../relational-databases/performance-monitor/sql-server-latches-object.md)      
