---
description: sys.dm_db_wait_stats（Azure SQL 数据库）
title: Azure SQL Database (sys.dm_db_wait_stats) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- dm_db_wait_stats_TSQL
- dm_db_wait_stats
- sys.dm_db_wait_stats
- sys.dm_db_wait_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_wait_stats dynamic management view
- dm_db_wait_stats
ms.assetid: 00abd0a5-bae0-4d71-b173-f7a14cddf795
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: 3d711b3eb0cb99c65582d8feacf8678fbca264c7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092903"
---
# <a name="sysdm_db_wait_stats-azure-sql-database"></a>sys.dm_db_wait_stats（Azure SQL 数据库）
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  返回在操作期间执行的线程所遇到的所有等待的相关信息。 可以使用此聚合视图来诊断 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 以及特定查询和批处理的性能问题。  
  
 执行查询期间的特定等待时间类型可以说明查询中存在瓶颈或失效点。 同样，如果服务器级的等待时间较长或等待计数较多，说明服务器实例内交互查询交互中存在瓶颈或热点。 例如，锁等待指示查询争用数据；页 IO 闩锁等待指示 IO 响应时间较慢；页闩锁更新指示表示文件布局不正确。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|wait_type|**nvarchar(60)**|等待类型的名称。 有关详细信息，请参阅本主题后面[等待类型](#WaitTypes)部分的内容。|  
|waiting_tasks_count|**bigint**|该等待类型的等待数。 该计数器在每开始一个等待时便会增加。|  
|wait_time_ms|**bigint**|该等待类型的总等待时间（毫秒）。 该时间包括 signal_wait_time_ms。|  
|max_wait_time_ms|**bigint**|该等待类型的最长等待时间。|  
|signal_wait_time_ms|**bigint**|正在等待的线程从收到信号通知到其开始运行之间的时差。|  
  
## <a name="remarks"></a>备注  
  
-   此动态管理视图只显示当前数据库的数据。  
  
-   此动态管理视图显示已完成的等待的时间。 它不显示当前等待。  
  
-   只要数据库转入或进入离线状态，计数器就会重置为零。  
  
-   如果出现下列任一情况，则不认为 SQL Server 工作线程处于等待状态：  
  
    -   资源变得可用。  
  
    -   查询非空。  
  
    -   外部进程完成。  
  
-   这些统计信息在 SQL Database 故障转移事件间不能持续存在，所有数据均为自上次重置统计信息以来累积的数据。  
  
## <a name="permissions"></a>权限  
 要求对服务器具有 VIEW DATABASE STATE 权限。  
  
##  <a name="types-of-waits"></a><a name="WaitTypes"></a> 等待类型  
 资源等待  
 当某个工作线程请求访问某个不可用的资源（因为该资源正在由其他某个工作线程使用，或者该资源尚不可用）时，便会发生资源等待。 资源等待的示例包括锁等待、闩锁等待、网络等待以及磁盘 I/O 等待。 锁等待和闩锁等待是指等待同步对象。  
  
 队列等待  
 当工作线程空闲，等待分配工作时便会发生队列等待。 队列等待通常发生在系统后台任务（如监视死锁以及清除已删除的记录等任务）中。 这些任务将等待工作请求被放入工作队列。 即使没有新数据包放入队列，队列等待也可能定期处于活动状态。  
  
 外部等待  
 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工作线程正在等待外部事件（如扩展存储过程调用或链接服务器查询）完成时，便会发生外部等待。 当诊断有妨碍的问题时，请记住，外部等待不会始终表示工作线程处于空闲状态，因为工作线程可能处于活动状态且正在运行某些外部代码。  
  
 尽管线程不再处于等待状态，但是它不必立即开始运行。 这是因为此类线程首先放入可运行工作线程的队列中，并且必须等待量程在计划程序中运行。  
  
 在中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 等待时间计数器是 **bigint** 值，因此，在早期版本的中，它们并不像等效的计数器那样易于作为计数器滚动更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 下表列出各任务所遇到的等待类型。  
  
|等待类型|说明|  
|---------------|-----------------|  
|ABR|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|ASSEMBLY_LOAD|在以独占的方式访问程序集加载时出现。|  
|ASYNC_DISKPOOL_LOCK|当尝试同步并行的线程（执行创建或初始化文件等任务）时出现。|  
|ASYNC_IO_COMPLETION|当某任务正在等待 I/O 完成时出现。|  
|ASYNC_NETWORK_IO|当任务被阻止在网络之后时出现在网络写入中。 验证客户端是否正在处理来自服务器的数据。|  
|AUDIT_GROUPCACHE_LOCK|当等待控制对某个特殊缓存的访问的锁时出现。 该缓存包含正在使用哪些审核来审核每个审核操作组的相关信息。|  
|AUDIT_LOGINCACHE_LOCK|当等待控制对某个特殊缓存的访问的锁时出现。 该缓存包含正在使用哪些审核来审核登录审核操作组的相关信息。|  
|AUDIT_ON_DEMAND_TARGET_LOCK|当等待用于确保扩展事件目标相关审核的单一初始化的锁时出现。|  
|AUDIT_XE_SESSION_MGR|当等待用于同步扩展事件会话相关审核的启动和停止的锁时出现。|  
|BACKUP|当任务作为备份处理的一部分被阻止时出现。|  
|BACKUP_OPERATOR|当任务正在等待磁带装入时出现。 若要查看磁带状态，请查询 [sys.dm_io_backup_tapes](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md)。 如果装入操作没有挂起，则该等待类型可能指示磁带机发生硬件问题。|  
|BACKUPBUFFER|在备份任务等待数据或等待用来存储数据的缓冲区时发生。 此类型不常见，只有当任务等待装入磁带时才会出现。|  
|BACKUPIO|在备份任务等待数据或等待用来存储数据的缓冲区时发生。 此类型不常见，只有当任务等待装入磁带时才会出现。|  
|BACKUPTHREAD|当某任务正在等待备份任务完成时出现。 等待时间可能较长，从几分钟到几个小时。 如果被等待的任务正处于 I/O 进程中，则该类型不指示发生问题。|  
|BAD_PAGE_PROCESS|当后台可疑页记录器正在尝试避免每隔五秒以上的时间运行时出现。 过多的可疑页会导致记录器频繁运行。|  
|BROKER_CONNECTION_RECEIVE_TASK|在等待访问以便在连接端点上接收消息时出现。 已序列化对端点的接收访问。|  
|BROKER_ENDPOINT_STATE_MUTEX|当存在访问 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 连接端点状态的争用时出现。 已序列化对更改状态的访问。|  
|BROKER_EVENTHANDLER|当某任务正在 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 的主事件处理程序中等待时出现。 出现时间应该非常短暂。|  
|BROKER_INIT|当初始化每个活动数据库中的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 时出现。 该状态应当频繁出现。|  
|BROKER_MASTERSTART|当某任务正在等待 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 的主事件处理程序启动时出现。 出现时间应该非常短暂。|  
|BROKER_RECEIVE_WAITFOR|当 RECEIVE WAITFOR 正在等待时出现。 如果没有准备接收的消息，则通常出现该状态。|  
|BROKER_REGISTERALLENDPOINTS|在初始化 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 连接端点的过程中出现。 出现时间应该非常短暂。|  
|BROKER_SERVICE|当与目标服务关联的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 目标列表更新或重新设定优先顺序时出现。|  
|BROKER_SHUTDOWN|当按计划关闭 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 时出现。 该状态出现的时间应当尽量短暂。|  
|BROKER_TASK_STOP|当 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 队列任务处理程序尝试关闭任务时出现。 已序列化状态检查，并且必须预先处于运行状态。|  
|BROKER_TO_FLUSH|当 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 惰性刷新器将内存中传输对象刷新到工作表时出现。|  
|BROKER_TRANSMITTER|当 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 发送器正在等待工作时出现。|  
|BUILTIN_HASHKEY_MUTEX|可能在实例启动之后而在初始化内部数据结构时出现。 数据结构初始化之后将不会再次出现。|  
|CHECK_PRINT_RECORD|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|CHECKPOINT_QUEUE|当检查点任务正在等待下一个检查点请求时出现。|  
|CHKPT|在服务器启动时出现以通知检查点线程可以启动。|  
|CLEAR_DB|在执行会更改数据库状态的操作过程中发生，例如打开或关闭数据库。|  
|CLR_AUTO_EVENT|当某任务当前正在执行公共语言运行时 (CLR) 执行并且正在等待特殊的自动事件启动时出现。 通常会出现长时间等待，这并不意味着出现问题。|  
|CLR_CRST|当某任务当前正在执行 CLR 执行并且正在等待输入当前由另一项任务正在使用的任务的关键部分时出现。|  
|CLR_JOIN|当某任务当前正在执行 CLR 执行并且正在等待另一项任务结束时出现。 当两任务之间具有联接时出现该等待状态。|  
|CLR_MANUAL_EVENT|当某任务当前正在执行 CLR 执行并且正在等待特定手动事件启动时出现。|  
|CLR_MEMORY_SPY|当为用于记录来自 CLR 的所有虚拟内存分配的数据结构等待获取锁时出现。 如果存在并行访问，该数据结构将被锁定以维护其完整性。|  
|CLR_MONITOR|当某任务当前正在执行 CLR 执行并且正在等待获取用于监视器的锁时出现。|  
|CLR_RWLOCK_READER|当某任务当前正在执行 CLR 执行并且正在等待读取器锁时出现。|  
|CLR_RWLOCK_WRITER|当某任务当前正在执行 CLR 执行并且正在等待编写器锁时出现。|  
|CLR_SEMAPHORE|当某任务当前正在执行 CLR 执行并且正在等待信号量时出现。|  
|CLR_TASK_START|在等待 CLR 任务完成启动时出现。|  
|CLRHOST_STATE_ACCESS|当等待获取对 CLR 宿主数据结构的独占访问时出现。 当设置或关闭 CLR 运行时时出现此等待类型。|  
|CMEMTHREAD|当某任务正在等待线程安全内存对象时出现。 当多项任务尝试分配来自同一个内存对象的内存而导致出现争用时，便可能延长等待时间。|  
|CXPACKET|当尝试同步查询处理器交换迭代器时出现。 如果针对该等待类型的争用成为问题时，可以考虑降低并行度。|  
|CXROWSET_SYNC|在并行范围扫描期间出现。|  
|DAC_INIT|当正在初始化专用管理员连接时出现。|  
|DBMIRROR_DBM_EVENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|DBMIRROR_DBM_MUTEX|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|DBMIRROR_EVENTS_QUEUE|在数据库镜像等待处理事件时出现。|  
|DBMIRROR_SEND|当某任务正在等待清除网络层的通信积压以便能够发送消息时出现。 指示通信层正在开始重载并影响数据库镜像数据吞吐量。|  
|DBMIRROR_WORKER_QUEUE|指示数据库镜像工作线程任务正在等待更多的工作。|  
|DBMIRRORING_CMD|当某任务正在等待日志记录刷新到磁盘时出现。 该等待状态应当保留较长的时间。|  
|DEADLOCK_ENUM_MUTEX|在死锁监视器和 sys.dm_os_waiting_tasks 尝试确保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不同时运行多个死锁搜索时出现。|  
|DEADLOCK_TASK_SEARCH|长时间等待此资源指示服务器正在 sys.dm_os_waiting_tasks 之上执行查询，并且这些查询正在阻止死锁监视器运行死锁搜索。 该等待类型仅供死锁监视器使用。 sys.dm_os_waiting_tasks 之上的查询使用 DEADLOCK_ENUM_MUTEX。|  
|DEBUG|在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和 CLR 调试内部同步期间出现。|  
|DISABLE_VERSIONING|当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 轮询版本事务管理器，以查看最早的活动事务的时间戳是否晚于状态开始更改时的时间戳时出现。 如果是，则所有在 ALTER DATABASE 语句运行之前启动的快照事务都已完成。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过 ALTER DATABASE 语句禁用版本控制时使用该等待状态。|  
|DISKIO_SUSPEND|当某任务正在等待访问文件（外部备份处于活动状态）时出现。 针对每个正在等待的用户进程报告该状态。 每个用户进程大于五的计数可能指示外部备份需要太长时间才能完成。|  
|DISPATCHER_QUEUE_SEMAPHORE|当调度程序池中的线程正在等待更多要处理的工作时出现。 当调度程序处于空闲状态时，此等待类型的等待时间预计要增加。|  
|DLL_LOADING_MUTEX|在等待 XML 分析器 DLL 加载时出现。|  
|DROPTEMP|在上次尝试删除临时对象失败后再进行下次尝试之前出现。 对于每一次失败的删除尝试，等待持续时间都以指数形式增长。|  
|DTC|当某任务正在等待用于管理状态转换的事件时出现。 该状态控制当 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 接收到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式事务处理协调器 (MS DTC) 服务不可用的通知之后执行 MS DTC 事务恢复的时间。<br /><br /> 该状态还说明在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动了 MS DTC 事务提交并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 正在等待 MS DTC 提交完成时进行等待的任务。|  
|DTC_ABORT_REQUEST|当 MS DTC 工作线程会话正在等待获得 MS DTC 事务的所有权时，在该会话中出现。 当 MS DTC 拥有了事务后，该会话可以回滚事务。 通常，该会话将等待另一个正在使用事务的会话。|  
|DTC_RESOLVE|当恢复任务正在等待跨数据库事务中的 master 数据库以查询该事务的结果时出现。|  
|DTC_STATE|当某任务正在等待对内部 MS DTC 全局状态对象的更改进行保护的事件时出现。 该状态应当保持非常短的时间。|  
|DTC_TMDOWN_REQUEST|当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 接收到 MS DTC 服务不可用的通知时，在 MS DTC 工作线程会话中出现。 首先，工作线程将等待 MS DTC 恢复进程启动。 然后，工作线程等待获取其正在处理的分布式事务的结果。 此过程可能一直执行，直到重新建立与 MS DTC 服务的连接。|  
|DTC_WAITFOR_OUTCOME|当恢复任务等待 MS DTC 处于活动状态以启用准备好的事务的解决方法时出现。|  
|DUMP_LOG_COORDINATOR|当主任务正在等待子任务生成数据时出现。 该状态通常不会出现。 长时间的等待指示出现意外的阻塞。 应当对子任务进行调查。|  
|DUMPTRIGGER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|EC|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|EE_PMOLOCK|在语句执行过程中特定的内存分配类型同步期间出现。|  
|EE_SPECPROC_MAP_INIT|在对内部过程哈希表创建进行同步期间发生。 此等待只能发生在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启动之后对哈希表的初始访问期间。|  
|ENABLE_VERSIONING|当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在声明数据库可以转换到快照隔离允许的状态之前，等待该数据库中的所有更新事务完成时出现。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过 ALTER DATABASE 语句启用快照隔离时使用该状态。|  
|ERROR_REPORTING_MANAGER|在对多个并发错误日志初始化进行同步期间发生。|  
|EXCHANGE|在并行查询过程中查询处理器交换迭代器同步期间出现。|  
|EXECSYNC|在并行查询过程中同步与交换迭代器无关的区域内的查询处理器期间出现。 例如，此类区域包括位图、二进制大型对象 (LOB) 以及假脱机迭代器等。 LOB 可能会经常使用该等待状态。|  
|EXECUTION_PIPE_EVENT_INTERNAL|当同步通过连接上下文提交的批处理执行的创建器和使用者部件期间出现。|  
|FAILPOINT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|FCB_REPLICA_READ|当同步快照（或 DBCC 创建的临时快照）稀疏文件的读取时出现。|  
|FCB_REPLICA_WRITE|当同步快照（或 DBCC 创建的临时快照）稀疏文件的页推送或页请求时出现。|  
|FS_FC_RWLOCK|当 FILESTREAM 垃圾收集器等待执行下列操作之一时出现：<br /><br /> 禁用垃圾收集（由备份和还原使用）。<br /><br /> 执行 FILESTREAM 垃圾收集器的一个周期。|  
|FS_GARBAGE_COLLECTOR_SHUTDOWN|当 FILESTREAM 垃圾收集器等待清除任务完成时出现。|  
|FS_HEADER_RWLOCK|当等待获取对 FILESTREAM 数据容器的 FILESTREAM 标头的访问，以便读取或更新 FILESTREAM 标头文件 (Filestream.hdr) 中的内容时出现。|  
|FS_LOGTRUNC_RWLOCK|当等待获取对 FILESTREAM 日志截断的访问以执行下列操作之一时出现：<br /><br /> 临时禁用 FILESTREAM 日志 (FSLOG) 截断（由备份和还原使用）。<br /><br /> 执行 FSLOG 截断的一个周期。|  
|FSA_FORCE_OWN_XACT|当 FILESTREAM 文件 I/O 操作需要绑定到关联的事务，但该事务当前由另一个会话拥有时出现。|  
|FSAGENT|当 FILESTREAM 文件 I/O 操作等待的 FILESTREAM 代理资源正由另一个文件 I/O 操作使用时出现。|  
|FSTR_CONFIG_MUTEX|当等待另一个 FILESTREAM 功能重新配置完成时出现。|  
|FSTR_CONFIG_RWLOCK|当等待序列化对 FILESTREAM 配置参数的访问时出现。|  
|FT_METADATA_MUTEX|记录为仅供参考。 不支持。 不保证以后的兼容性。|  
|FT_RESTART_CRAWL|在全文爬网需要从上一个已知可用点重新启动以便从暂时故障中恢复时出现。 等待使当前正在此总体中工作的工作线程任务完成或退出当前步骤。|  
|FULLTEXT GATHERER|在同步全文操作期间发生。|  
|GUARDIAN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|HTTP_ENUMERATION|在启动时出现，以枚举 HTTP 端点以启动 HTTP。|  
|HTTP_START|当连接正在等待 HTTP 完成初始化时出现。|  
|IMPPROV_IOWAIT|当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 等待 Bulkload I/O 完成时出现。|  
|INTERNAL_TESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|IO_AUDIT_MUTEX|在跟踪事件缓冲区同步期间出现。|  
|IO_COMPLETION|在等待 I/O 操作完成时出现。 通常，该等待类型表示非数据页 I/O。 数据页 I/O 完成等待显示为 PAGEIOLATCH_* waits。|  
|IO_QUEUE_LIMIT|在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 的异步 IO 队列具有过多 IO 挂起时出现。 在挂起的 IO 数降低到阈值以下之前，此等待类型上尝试发出另一个 IO 的任务会被阻止。 该阈值与分配给数据库的 DTU 成正比。|  
|IO_RETRY|当 I/O 操作（例如读取磁盘或写入磁盘）由于资源不足而失败，然后重试时出现。|  
|IOAFF_RANGE_QUEUE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|KSOURCE_WAKEUP|在等待来自服务控制管理器的请求期间由服务控制任务使用。 可能会出现长时间等待，这并不指示出现问题。|  
|KTM_ENLISTMENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|KTM_RECOVERY_MANAGER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|KTM_RECOVERY_RESOLUTION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LATCH_DT|等待 DT（破坏）闩锁时出现。 它不包括缓冲区闩锁或事务标记闩锁。 sys.dm_os_latch_stats 中提供了 LATCH_* waits 的列表。 请注意，sys.dm_os_latch_stats 将 LATCH_NL、LATCH_SH、LATCH_UP、LATCH_EX 以及 LATCH_DT 等待分到一组。|  
|LATCH_EX|等待 EX（排他）闩锁时出现。 它不包括缓冲区闩锁或事务标记闩锁。 sys.dm_os_latch_stats 中提供了 LATCH_* waits 的列表。 请注意，sys.dm_os_latch_stats 将 LATCH_NL、LATCH_SH、LATCH_UP、LATCH_EX 以及 LATCH_DT 等待分到一组。|  
|LATCH_KP|等待 KP（保持）闩锁时出现。 它不包括缓冲区闩锁或事务标记闩锁。 sys.dm_os_latch_stats 中提供了 LATCH_* waits 的列表。 请注意，sys.dm_os_latch_stats 将 LATCH_NL、LATCH_SH、LATCH_UP、LATCH_EX 以及 LATCH_DT 等待分到一组。|  
|LATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LATCH_SH|等待 SH（共享）闩锁时出现。 它不包括缓冲区闩锁或事务标记闩锁。 sys.dm_os_latch_stats 中提供了 LATCH_* waits 的列表。 请注意，sys.dm_os_latch_stats 将 LATCH_NL、LATCH_SH、LATCH_UP、LATCH_EX 以及 LATCH_DT 等待分到一组。|  
|LATCH_UP|等待 UP（更新）闩锁时出现。 它不包括缓冲区闩锁或事务标记闩锁。 sys.dm_os_latch_stats 中提供了 LATCH_* waits 的列表。 请注意，sys.dm_os_latch_stats 将 LATCH_NL、LATCH_SH、LATCH_UP、LATCH_EX 以及 LATCH_DT 等待分到一组。|  
|LAZYWRITER_SLEEP|当惰性编写器被挂起时出现。 正在等待的后台任务所用时间的度量值。 在查找用户阻隔点所时不要考虑该状态。|  
|LCK_M_BU|当某任务正在等待获取大容量更新 (BU) 锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_IS|当某任务正在等待获取意向共享 (IS) 锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_IU|当某任务正在等待获取意向更新 (IU) 锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_IX|当某任务正在等待获取意向排他 (IX) 锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_RIn_NL|当某任务正在等待获取当前键值上的 NULL 锁以及当前键和上一个键之间的插入范围锁时出现。 键上的 NULL 锁是指立即释放的锁。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_RIn_S|当某任务正在等待获取当前键值上的共享锁以及当前键和上一个键之间的插入范围锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_RIn_U|任务正在等待获取当前键值上的更新锁以及当前键和上一个键之间的插入范围锁。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_RIn_X|当某任务正在等待获取当前键值上的排他锁以及当前键和上一个键之间的插入范围锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_RS_S|当某任务正在等待获取当前键值上的共享锁以及当前键和上一个键之间的共享范围锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_RS_U|当某任务正在等待获取当前键值上的更新锁以及当前键和上一个键之间的更新范围锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_RX_S|当某任务正在等待获取当前键值上的共享锁以及当前键和上一个键之间的排他范围锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_RX_U|当某任务正在等待获取当前键值上的更新锁以及当前键和上一个键之间的排他范围锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_RX_X|当某任务正在等待获取当前键值上的排他锁以及当前键和上一个键之间的排他范围锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_S|当某任务正在等待获取共享锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_SCH_M|当某任务正在等待获取架构修改锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_SCH_S|当某任务正在等待获取架构共享锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_SIU|当某任务正在等待获取共享意向更新锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_SIX|当某任务正在等待获取共享意向排他锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_U|当某任务正在等待获取更新锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_UIX|当某任务正在等待获取更新意向排他锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LCK_M_X|当某任务正在等待获取排他锁时出现。 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。|  
|LOG_RATE_GOVERNOR|在 DB 正在等待写入日志的配额时发生。|  
|LOGBUFFER|当某任务正在等待日志缓冲区的空间以存储日志记录时出现。 连续的高值可能指示日志设备无法跟上服务器生成的日志量。|  
|LOGGENERATION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LOGMGR|在数据库关闭过程中，当某任务正在等待任何未完成的日志 I/O 在关闭日志之前完成时出现。|  
|LOGMGR_FLUSH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LOGMGR_QUEUE|在日志编写器任务等待工作请求时出现。|  
|LOGMGR_RESERVE_APPEND|当某任务正在等待查看日志截断是否能释放日志空间以使该任务能写入新的日志记录时出现。 请考虑为受影响的数据库增加日志文件的大小以减少该等待时间。|  
|LOWFAIL_MEMMGR_QUEUE|在等待可用内存期间出现。|  
|MSQL_DQ|当某任务正在等待分布式查询操作完成时出现。 它用于检测潜在的多个活动的结果集 (MARS) 应用程序死锁。 该等待将在分布式查询调用完成时结束。|  
|MSQL_XACT_MGR_MUTEX|当某任务正在等待获取会话事务管理器的所有权以执行会话级别事务操作时出现。|  
|MSQL_XACT_MUTEX|在事务使用同步期间出现。 请求必须先获取互斥体才可以使用事务。|  
|MSQL_XP|当某任务正在等待扩展存储过程结束时出现。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用该等待状态检测潜在的 MARS 应用程序死锁。 该等待将在扩展存储过程调用结束时停止。|  
|MSSEARCH|在全文搜索调用期间出现。 该等待在全文操作完成时结束。 它不指示争用，而指示全文操作的持续时间。|  
|NET_WAITFOR_PACKET|在网络读取过程中连接正在等待网络数据包时出现。|  
|OLEDB|在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 调用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口时出现。 该等待类型不用于同步。 而是用于指示调用 OLE DB 访问接口的持续时间。|  
|ONDEMAND_TASK_QUEUE|在后台任务等待高优先级系统任务请求时出现。 长时间的等待指示一直没有要处理的高优先级请求，不应引起关注。|  
|PAGEIOLATCH_DT|在任务等待 I/O 请求中缓冲区的闩锁时发生。 闩锁请求处于“破坏”模式。 长时间的等待可能指示磁盘子系统出现问题。|  
|PAGEIOLATCH_EX|在任务等待 I/O 请求中缓冲区的闩锁时发生。 闩锁请求处于“独占”模式。 长时间的等待可能指示磁盘子系统出现问题。|  
|PAGEIOLATCH_KP|在任务等待 I/O 请求中缓冲区的闩锁时发生。 闩锁请求处于“保持”模式。 长时间的等待可能指示磁盘子系统出现问题。|  
|PAGEIOLATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PAGEIOLATCH_SH|在任务等待 I/O 请求中缓冲区的闩锁时发生。 闩锁请求处于“共享”模式。 长时间的等待可能指示磁盘子系统出现问题。|  
|PAGEIOLATCH_UP|在任务等待 I/O 请求中缓冲区的闩锁时发生。 闩锁请求处于“更新”模式。 长时间的等待可能指示磁盘子系统出现问题。|  
|PAGELATCH_DT|在任务等待不处于 I/O 请求中的缓冲区闩锁时发生。 闩锁请求处于“破坏”模式。|  
|PAGELATCH_EX|在任务等待不处于 I/O 请求中的缓冲区闩锁时发生。 闩锁请求处于“独占”模式。|  
|PAGELATCH_KP|在任务等待不处于 I/O 请求中的缓冲区闩锁时发生。 闩锁请求处于“保持”模式。|  
|PAGELATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PAGELATCH_SH|在任务等待不处于 I/O 请求中的缓冲区闩锁时发生。 闩锁请求处于“共享”模式。|  
|PAGELATCH_UP|在任务等待不处于 I/O 请求中的缓冲区闩锁时发生。 闩锁请求处于“更新”模式。|  
|PARALLEL_BACKUP_QUEUE|在序列化由 RESTORE HEADERONLY、RESTORE FILELISTONLY 或 RESTORE LABELONLY 生成的输出时出现。|  
|PREEMPTIVE_ABR|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_AUDIT_ACCESS_EVENTLOG|在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 操作系统 (SQLOS) 计划程序切换到抢先模式时发生，以便将审核事件写入 Windows 事件日志。|  
|PREEMPTIVE_AUDIT_ACCESS_SECLOG|在 SQLOS 计划程序切换到抢先模式时发生，以便将审核事件写入 Windows 安全日志。|  
|PREEMPTIVE_CLOSEBACKUPMEDIA|在 SQLOS 计划程序切换到抢先模式时发生，以便关闭备份介质。|  
|PREEMPTIVE_CLOSEBACKUPTAPE|在 SQLOS 计划程序切换到抢先模式时发生，以便关闭磁带备份设备。|  
|PREEMPTIVE_CLOSEBACKUPVDIDEVICE|在 SQLOS 计划程序切换到抢先模式时发生，以便关闭虚拟备份设备。|  
|PREEMPTIVE_CLUSAPI_CLUSTERRESOURCECONTROL|在 SQLOS 计划程序切换到抢先模式时发生，以便执行故障转移群集操作。|  
|PREEMPTIVE_COM_COCREATEINSTANCE|在 SQLOS 计划程序切换到抢先模式时发生，以便创建 COM 对象。|  
|PREEMPTIVE_HADR_LEASE_MECHANISM|为 CSS 诊断 Always On 可用性组租赁管理器计划。|  
|PREEMPTIVE_SOSTESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_STRESSDRIVER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_TESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_XETESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PRINT_ROLLBACK_PROGRESS|用于等待用户进程在已通过 ALTER DATABASE 终止子句完成转换的数据库中结束。 有关详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。|  
|PWAIT_HADR_CHANGE_NOTIFIER_TERMINATION_SYNC|在后台任务正在等待终止接收（通过轮询）Windows Server 故障转移群集通知的后台任务时发生。  仅限内部使用。|  
|PWAIT_HADR_CLUSTER_INTEGRATION|追加、替换和/或删除操作正在等待获取 Always On 内部列表上的写入锁 (例如网络、网络地址或可用性组侦听器) 列表。  仅限内部使用。|  
|PWAIT_HADR_OFFLINE_COMPLETED|Always On drop 可用性组操作在销毁 Windows Server 故障转移群集对象之前等待目标可用性组脱机。|  
|PWAIT_HADR_ONLINE_COMPLETED|Always On 创建或故障转移可用性组操作正在等待目标可用性组联机。|  
|PWAIT_HADR_POST_ONLINE_COMPLETED|Always On drop 可用性组操作正在等待作为之前命令的一部分计划的任何后台任务的终止。 例如，可能有正在将可用性数据库转换为主要角色的后台任务。 DROP AVAILABILITY GROUP DDL 必须等待此后台任务终止，以免出现争用情况。|  
|PWAIT_HADR_WORKITEM_COMPLETED|正在等待异步工作任务完成，这是线程执行的内部等待。 这是预期的等待，用于 CSS。|  
|PWAIT_MD_LOGIN_STATS|在登录统计信息的元数据内部同步期间发生。|  
|PWAIT_MD_RELATION_CACHE|在表或索引的元数据内部同步期间发生。|  
|PWAIT_MD_SERVER_CACHE|在链接服务器的元数据内部同步期间发生。|  
|PWAIT_MD_UPGRADE_CONFIG|在升级服务器范围的配置时进行内部同步期间发生。|  
|PWAIT_METADATA_LAZYCACHE_RWLOCk|在元数据缓存内部同步期间以及迭代表中的索引或统计信息时发生。|  
|QPJOB_KILL|指示异步统计信息自动更新在开始运行时通过调用 KILL 命令而取消。 终止线程处于挂起状态，等待它开始侦听 KILL 命令。 正常情况下，该值不到一秒钟。|  
|QPJOB_WAITFOR_ABORT|指示异步统计信息自动更新在运行时通过调用 KILL 命令而取消。 目前更新已完成，但是在终止线程消息协调完成之前一直于挂起状态。 这是一个普通而少见的状态，应当非常短暂。 正常情况下，该值不到一秒钟。|  
|QRY_MEM_GRANT_INFO_MUTEX|当查询执行内存管理尝试控制对静态授予信息列表的访问时出现。 该状态列出当前已批准的内存请求以及正在等待的内存请求的有关信息。 该状态是一个简单的访问控制状态。 该状态始终不应当等待较长的时间。 如果未释放互斥体，则所有占用内存的新查询都将停止响应。|  
|QUERY_ERRHDL_SERVICE_DONE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|QUERY_EXECUTION_INDEX_SORT_EVENT_OPEN|当脱机创建索引生成以并行方式运行，并且正在排序的不同工作线程同步访问排序文件时出现。|  
|QUERY_NOTIFICATION_MGR_MUTEX|在查询通知管理器中的垃圾收集队列同步期间出现。|  
|QUERY_NOTIFICATION_SUBSCRIPTION_MUTEX|在查询通知中事务的状态同步期间出现。|  
|QUERY_NOTIFICATION_TABLE_MGR_MUTEX|在查询通知管理器中的内部同步期间出现。|  
|QUERY_NOTIFICATION_UNITTEST_MUTEX|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|QUERY_OPTIMIZER_PRINT_MUTEX|在查询优化器诊断信息输出生成的同步期间出现。 该等待类型仅在诊断设置已根据 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 产品支持的说明启用后出现。|  
|QUERY_TRACEOUT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|QUERY_WAIT_ERRHDL_SERVICE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|RECOVER_CHANGEDB|在备用数据库中同步数据库状态期间出现。|  
|REPL_CACHE_ACCESS|在同步复制项目缓存的期间出现。 在这些等待期间，复制日志读取器将停止，已发布表中的数据定义语言 (DDL) 语句也将被阻止。|  
|REPL_SCHEMA_ACCESS|在同步复制架构版本信息的期间出现。 该状态在下列情况下存在：针对复制对象执行 DDL 语句时，以及日志读取器根据 DDL 出现次数生成或使用版本控制架构时。|  
|REPLICA_WRITES|在任务等待将页写入数据库快照或 DBCC 副本的操作完成时出现。|  
|REQUEST_DISPENSER_PAUSE|在任务等待所有未完成的 I/O 完成时出现，以便可以为快照备份冻结文件的 I/O。|  
|REQUEST_FOR_DEADLOCK_SEARCH|在死锁监视器等待开始下一次死锁搜索时出现。 在两次死锁检测之间可能出现该等待，长时间等待此资源并不指示出现问题。|  
|RESMGR_THROTTLED|在有新请求传入并且基于 GROUP_MAX_REQUESTS 设置而中止时出现。|  
|RESOURCE_QUEUE|在同步不同的内部资源队列期间出现。|  
|RESOURCE_SEMAPHORE|当由于存在其他并发查询而无法立即批准查询内存请求时出现。 等待时间较长或等待次数较多可能指示并发查询的数量过多或内存请求的数量过多。|  
|RESOURCE_SEMAPHORE_MUTEX|在查询等待其预留线程的请求完成时出现。 它也在同步查询编译和内存授予请求时出现。|  
|RESOURCE_SEMAPHORE_QUERY_COMPILE|在并发查询编译的数量达到中止限制时出现。 等待时间较长或等待次数较多可能指示编译、重新编辑或不可缓存的计划过多。|  
|RESOURCE_SEMAPHORE_SMALL_QUERY|当由于存在其他并发查询而无法立即批准较小查询的内存请求时出现。 等待时间不应超过几秒钟，因为如果服务器无法在几秒钟内给予请求的内存，则会将请求传输到主查询内存池中。 等待时间较长可能指示当主内存池被等待的查询阻塞时并发小查询的数量过多。|  
|SE_REPL_CATCHUP_THROTTLE|在事务等待其中一个辅助数据库取得进展时发生。|  
|SE_REPL_COMMIT_ACK|当事务等待从辅助副本收到仲裁提交确认时发生。|  
|SE_REPL_COMMIT_TURN|当事务等待在收到仲裁提交确认后提交时发生。|  
|SE_REPL_ROLLBACK_ACK|当事务等待从辅助副本收到仲裁回滚确认时发生。|  
|SE_REPL_SLOW_SECONDARY_THROTTLE|当线程等待其中一个数据库辅助副本时发生。|  
|SEC_DROP_TEMP_KEY|在尝试删除临时安全密钥失败之后并在重试之前出现。|  
|SECURITY_MUTEX|当等待互斥体时出现，这些互斥体控制对可扩展的密钥管理 (EKM) 加密提供程序的全局列表以及 EKM 会话的会话作用域列表的访问。|  
|SEQUENTIAL_GUID|当正在获取新的连续 GUID 时出现。|  
|SERVER_IDLE_CHECK|当资源监视器正在尝试将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例声明为空闲或正在尝试唤醒时，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例空闲状态的同步期间出现。|  
|SHUTDOWN|在关闭语句等待活动连接退出时出现。|  
|SLEEP_BPOOL_FLUSH|当检查点为了避免磁盘子系统泛滥而中止新 I/O 的发布时出现。|  
|SLEEP_DBSTARTUP|在等待所有数据库恢复时数据库的启动期间出现。|  
|SLEEP_DCOMSTARTUP|通常在等待 DCOM 初始化完成时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的启动期间出现。|  
|SLEEP_MSDBSTARTUP|在 SQL 跟踪等待 msdb 数据库完成启动时出现。|  
|SLEEP_SYSTEMTASK|在等待 tempdb 完成启动时后台任务的启动期间出现。|  
|SLEEP_TASK|当任务在等待一般事件出现期间睡眠时出现。|  
|SLEEP_TEMPDBSTARTUP|在任务等待 tempdb 完成启动时出现。|  
|SNI_CRITICAL_SECTION|在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 网络组件中进行内部同步期间出现。|  
|SNI_HTTP_WAITFOR_0_DISCON|在等待未完成的 HTTP 连接退出的过程中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的关闭期间出现。|  
|SNI_LISTENER_ACCESS|当等待非一致性内存访问 (NUMA) 节点更新状态更改时出现。 已序列化对状态更改的访问。|  
|SNI_TASK_COMPLETION|当在 NUMA 节点状态更改期间等待所有任务完成时出现。|  
|SOAP_READ|在等待 HTTP 网络读取完成时出现。|  
|SOAP_WRITE|在等待 HTTP 网络写入完成时出现。|  
|SOS_CALLBACK_REMOVAL|在为了删除回调而对回调列表执行同步期间出现。 服务器初始化完成之后，此计数器可能不会更改。|  
|SOS_DISPATCHER_MUTEX|在调度程序池进行内部同步期间出现。 包括调整该池时。|  
|SOS_LOCALALLOCATORLIST|在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存管理器中进行内部同步期间出现。|  
|SOS_MEMORY_USAGE_ADJUSTMENT|在池之间调整内存使用情况时出现。|  
|SOS_OBJECT_STORE_DESTROY_MUTEX|当破坏池中的对象时在内存池中进行内部同步期间出现。|  
|SOS_PROCESS_AFFINITY_MUTEX|在同步访问进程关联设置期间出现。|  
|SOS_RESERVEDMEMBLOCKLIST|在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存管理器中进行内部同步期间出现。|  
|SOS_SCHEDULER_YIELD|在任务自愿为要执行的其他任务生成计划程序时出现。 在该等待期间任务正在等待其量程更新。|  
|SOS_SMALL_PAGE_ALLOC|在分配和释放由某些内存对象管理的内存时出现。|  
|SOS_STACKSTORE_INIT_MUTEX|在内部存储初始化同步期间出现。|  
|SOS_SYNC_TASK_ENQUEUE_EVENT|在任务以同步方式启动时出现。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的大多数任务都以同步方式启动，在此方式中控制权在任务请求放置在工作队列之后立即返回到启动器。|  
|SOS_VIRTUALMEMORY_LOW|在内存分配等待资源管理器释放虚拟内存时出现。|  
|SOSHOST_EVENT|当宿主组件（如 CLR）在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件同步对象中等待时出现。|  
|SOSHOST_INTERNAL|在宿主组件（如 CLR）使用的内存管理器回调同步期间出现。|  
|SOSHOST_MUTEX|当宿主组件（如 CLR）在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 互斥体同步对象中等待时出现。|  
|SOSHOST_RWLOCK|当宿主组件（如 CLR）在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 读取器编写器同步对象中等待时出现。|  
|SOSHOST_SEMAPHORE|当宿主组件（如 CLR）在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 信号量同步对象中等待时出现。|  
|SOSHOST_SLEEP|当宿主任务在等待一般事件出现期间睡眠时出现。 宿主任务由宿主组件（如 CLR）使用。|  
|SOSHOST_TRACELOCK|在同步访问跟踪流期间出现。|  
|SOSHOST_WAITFORDONE|在宿主组件（如 CLR）等待任务完成时出现。|  
|SQLCLR_APPDOMAIN|在 CLR 等待应用程序域完成启动时出现。|  
|SQLCLR_ASSEMBLY|在等待访问 appdomain 中已加载的程序集列表时出现。|  
|SQLCLR_DEADLOCK_DETECTION|在 CLR 等待死锁检测完成时出现。|  
|SQLCLR_QUANTUM_PUNISHMENT|在 CLR 任务由于已经超过了其执行量程而中止时出现。 此中止已完成，以便减小此大量消耗资源的任务对其他任务的影响。|  
|SQLSORT_NORMMUTEX|在初始化内部排序结构时进行内部同步期间出现。|  
|SQLSORT_SORTMUTEX|在初始化内部排序结构时进行内部同步期间出现。|  
|SQLTRACE_BUFFER_FLUSH|当某任务正在等待后台任务将跟踪缓冲区每隔四秒刷新到磁盘时出现。|  
|SQLTRACE_LOCK|在文件跟踪过程中同步跟踪缓冲区期间出现。|  
|SQLTRACE_SHUTDOWN|在跟踪关闭等待未完成的跟踪事件完成时出现。|  
|SQLTRACE_WAIT_ENTRIES|在 SQL 跟踪事件队列等待数据包到达队列时出现。|  
|SRVPROC_SHUTDOWN|在关闭进程等待内部资源释放以完全关闭时出现。|  
|TEMPOBJ|在临时对象删除同步时出现。 该等待很少出现，仅在任务已请求 temp 表的独占访问删除时出现。|  
|THREADPOOL|当某任务正在等待工作线程运行时出现。 这可能指示最大工作线程数设置过低，或批处理执行时间过长，从而减少可满足其他批处理的工作线程数。|  
|TIMEPRIV_TIMEPERIOD|在扩展事件计时器进行内部同步期间出现。|  
|TRACEWRITE|当 SQL 跟踪行集跟踪提供程序等待可用缓冲区或可处理事件的缓冲区时出现。|  
|TRAN_MARKLATCH_DT|在等待事务标记闩锁中的破坏模式闩锁时出现。 事务标记闩锁用于同步提交与标记的事务。|  
|TRAN_MARKLATCH_EX|在等待标记事务中的排他模式闩锁时出现。 事务标记闩锁用于同步提交与标记的事务。|  
|TRAN_MARKLATCH_KP|在等待标记事务中的保持模式闩锁时出现。 事务标记闩锁用于同步提交与标记的事务。|  
|TRAN_MARKLATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|TRAN_MARKLATCH_SH|在等待标记事务中的共享模式闩锁时出现。 事务标记闩锁用于同步提交与标记的事务。|  
|TRAN_MARKLATCH_UP|在等待标记事务中的更新模式闩锁时出现。 事务标记闩锁用于同步提交与标记的事务。|  
|TRANSACTION_MUTEX|在同步多个批处理访问事务期间出现。|  
|UTIL_PAGE_ALLOC|在内存不足期间事务日志扫描等待可用内存时出现。|  
|VIA_ACCEPT|当在启动过程中完成虚拟接口适配器 (VIA) 提供程序连接时出现。|  
|VIEW_DEFINITION_MUTEX|在同步访问已缓存的视图定义期间出现。|  
|WAIT_FOR_RESULTS|在等待查询通知触发时出现。|  
|WAITFOR|显示为 WAITFOR [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的结果。 等待持续时间由此语句的参数确定。 它是用户启动的等待。|  
|WAITFOR_TASKSHUTDOWN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|WAITSTAT_MUTEX|在同步访问用于填充 sys.dm_os_wait_stats 的统计信息集期间出现。|  
|WCC|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|WORKTBL_DROP|在删除出现故障的工作表之后，重试之前的暂停期间出现。|  
|WRITE_COMPLETION|当正在进行写操作时出现。|  
|WRITELOG|等待日志刷新完成时出现。 导致日志刷新的常见操作是检查点和事务提交。|  
|XACT_OWN_TRANSACTION|在等待获取事务的所有权时出现。|  
|XACT_RECLAIM_SESSION|在等待会话的当前所有者释放会话的所有权时出现。|  
|XACTLOCKINFO|在同步访问事务锁列表期间出现。 除事务本身之外，在页拆分过程中死锁检测和锁迁移等操作也可访问锁列表。|  
|XACTWORKSPACE_MUTEX|在同步事务中的脱离以及事务登记成员之间的数据库锁数时出现。|  
|XE_BUFFERMGR_ALLPROCESSED_EVENT|在扩展事件会话缓冲区刷新到目标时发生。 此等待在后台线程上发生。|  
|XE_BUFFERMGR_FREEBUF_EVENT|当下列任一条件成立时发生：<br /><br /> 扩展事件会话配置为无事件损失，且会话中的所有缓冲区当前已满。 这表明扩展事件会话缓冲区太小，或应对其进行分区。<br /><br /> 审核遇到延迟。 这表明写入审核的驱动器上存在磁盘瓶颈。|  
|XE_DISPATCHER_CONFIG_SESSION_LIST|在使用异步目标的扩展事件会话启动或停止时发生。 此等待表明发生了以下某一情况：<br /><br /> 扩展事件会话正在向后台线程池注册。<br /><br /> 后台线程池正在根据当前负荷计算需要的线程数量。|  
|XE_DISPATCHER_JOIN|在用于扩展事件会话的后台线程终止时发生。|  
|XE_DISPATCHER_WAIT|在用于扩展事件会话的后台线程等待事件缓冲区进行处理时发生。|  
|XE_MODULEMGR_SYNC|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|XE_OLS_LOCK|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|XE_PACKAGE_LOCK_BACKOFF|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|FT_COMPROWSET_RWLOCK|全文正在等待片段元数据操作。 记录为仅供参考。 不支持。 不保证以后的兼容性。|  
|FT_IFTS_RWLOCK|全文正在等待内部同步。 记录为仅供参考。 不支持。 不保证以后的兼容性。|  
|FT_IFTS_SCHEDULER_IDLE_WAIT|全文计划程序睡眠等待类型。 计划程序空闲。|  
|FT_IFTSHC_MUTEX|全文正在等待 fdhost 控制操作。 记录为仅供参考。 不支持。 不保证以后的兼容性。|  
|FT_IFTSISM_MUTEX|全文正在等待通信操作。 记录为仅供参考。 不支持。 不保证以后的兼容性。|  
|FT_MASTER_MERGE|全文正在等待主合并操作。 记录为仅供参考。 不支持。 不保证以后的兼容性。|  
  
  
