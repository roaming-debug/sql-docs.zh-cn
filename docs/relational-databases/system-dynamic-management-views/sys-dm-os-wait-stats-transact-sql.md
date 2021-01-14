---
description: sys.dm_os_wait_stats (Transact-SQL)
title: sys.dm_os_wait_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_wait_stats_TSQL
- dm_os_wait_stats
- sys.dm_os_wait_stats
- sys.dm_os_wait_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_wait_stats dynamic management view
ms.assetid: 568d89ed-2c96-4795-8a0c-2f3e375081da
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1dccbed2d872b2cd2973e644f9f02149f88b11d4
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172749"
---
# <a name="sysdm_os_wait_stats-transact-sql"></a>sys.dm_os_wait_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

返回执行的线程所遇到的所有等待的相关信息。 可以使用此聚合视图来诊断 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以及特定查询和批处理的性能问题。 [sys.dm_exec_session_wait_stats &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-session-wait-stats-transact-sql.md) 按会话提供类似的信息。  
  
> [!NOTE] 
> 若要从 **[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 或** 调用此，请使用名称 **sys.dm_pdw_nodes_os_wait_stats**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|wait_type|**nvarchar(60)**|等待类型的名称。 有关详细信息，请参阅本主题后面[等待类型](#WaitTypes)部分的内容。|  
|waiting_tasks_count|**bigint**|该等待类型的等待数。 该计数器在每开始一个等待时便会增加。|  
|wait_time_ms|**bigint**|该等待类型的总等待时间（毫秒）。 该时间包括 signal_wait_time_ms。|  
|max_wait_time_ms|**bigint**|该等待类型的最长等待时间。|  
|signal_wait_time_ms|**bigint**|正在等待的线程从收到信号通知到其开始运行之间的时差。|  
|pdw_node_id|**int**|此分发所在的节点的标识符。 <br/> **适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] |  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

##  <a name="types-of-waits"></a><a name="WaitTypes"></a> 等待类型  
 当某个工作线程请求访问不可用的资源（因为该资源正在由其他某个工作线程使用，或者该资源尚不可用）时，便会发生 **资源等待**。 资源等待的示例包括锁等待、闩锁等待、网络等待以及磁盘 I/O 等待。 锁等待和闩锁等待是指等待同步对象  
  
当工作线程空闲，等待分配工作时，会发生 **队列等待**。 队列等待通常发生在系统后台任务（如监视死锁以及清除已删除的记录等任务）中。 这些任务将等待工作请求被放入工作队列。 即使没有新数据包放入队列，队列等待也可能定期处于活动状态。  
  
 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工作线程正在等待外部事件（如扩展存储过程调用或链接服务器查询）完成时，会发生外部等待。 当诊断有妨碍的问题时，请记住，外部等待不会始终表示工作线程处于空闲状态，因为工作线程可能处于活动状态且正在运行某些外部代码。  
  
 `sys.dm_os_wait_stats` 显示已完成的等待时间。 此动态管理视图不显示当前等待。  
  
 如果出现下列任一情况，则不认为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工作线程处于等待状态：  
  
-   资源变得可用。  
  
-   查询非空。  
  
-   外部进程完成。  
  
 尽管线程不再处于等待状态，但是它不必立即开始运行。 这是因为此类线程首先放入可运行工作线程的队列中，并且必须等待量程在计划程序中运行。  
  
 在中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 等待时间计数器是 **bigint** 值，因此，在早期版本的中，它们并不像等效的计数器那样易于作为计数器滚动更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 执行查询期间的特定等待时间类型可以说明查询中存在瓶颈或失效点。 同样，如果服务器级的等待时间较长或等待计数较多，说明服务器实例内交互查询交互中存在瓶颈或热点。 例如，锁等待指示查询争用数据；页 IO 闩锁等待指示 IO 响应时间较慢；页闩锁更新指示表示文件布局不正确。  
  
 此动态管理视图的内容可通过运行以下命令来重置：  
  
```sql  
DBCC SQLPERF ('sys.dm_os_wait_stats', CLEAR);  
GO  
```  
  
该命令将所有计数器重置为 0。  
  
> [!NOTE]
> 这些统计信息在每次重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时都不能持续存在，并且所有的数据均为自上次重置统计信息或启动服务器以来累积的数据。  
  
 下表列出各任务所遇到的等待类型。  

|type |说明| 
|-------------------------- |--------------------------| 
|ABR |标识为仅供参考。 不支持。 不保证以后的兼容性。| | 
|AM_INDBUILD_ALLOCATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|AM_SCHEMAMGR_UNSHARED_CACHE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|ASSEMBLY_FILTER_HASHTABLE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|ASSEMBLY_LOAD |在以独占的方式访问程序集加载时出现。| 
|ASYNC_DISKPOOL_LOCK |当尝试同步并行的线程（执行创建或初始化文件等任务）时出现。| 
|ASYNC_IO_COMPLETION |当某任务正在等待 I/O 完成时出现。| 
|ASYNC_NETWORK_IO |当任务被阻止在网络之后时出现在网络写入中。 验证客户端是否正在处理来自服务器的数据。| 
|ASYNC_OP_COMPLETION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|ASYNC_OP_CONTEXT_READ |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|ASYNC_OP_CONTEXT_WRITE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|ASYNC_SOCKETDUP_IO |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|AUDIT_GROUPCACHE_LOCK |当等待控制对某个特殊缓存的访问的锁时出现。 该缓存包含正在使用哪些审核来审核每个审核操作组的相关信息。| 
|AUDIT_LOGINCACHE_LOCK |当等待控制对某个特殊缓存的访问的锁时出现。 该缓存包含正在使用哪些审核来审核登录审核操作组的相关信息。| 
|AUDIT_ON_DEMAND_TARGET_LOCK |当等待用于确保扩展事件目标相关审核的单一初始化的锁时出现。| 
|AUDIT_XE_SESSION_MGR |当等待用于同步扩展事件会话相关审核的启动和停止的锁时出现。| 
|BACKUP |当任务作为备份处理的一部分被阻止时出现。| 
|BACKUP_OPERATOR |当任务正在等待磁带装入时出现。 若要查看磁带状态，请查询 sys.dm_io_backup_tapes。 如果装入操作没有挂起，则该等待类型可能指示磁带机发生硬件问题。| 
|BACKUPBUFFER |在备份任务等待数据或等待用来存储数据的缓冲区时发生。 此类型不常见，只有当任务等待装入磁带时才会出现。| 
|BACKUPIO |在备份任务等待数据或等待用来存储数据的缓冲区时发生。 此类型不常见，只有当任务等待装入磁带时才会出现。| 
|BACKUPTHREAD |当某任务正在等待备份任务完成时出现。 等待时间可能较长，从几分钟到几个小时。 如果被等待的任务正处于 I/O 进程中，则该类型不指示发生问题。| 
|BAD_PAGE_PROCESS |当后台可疑页记录器正在尝试避免每隔五秒以上的时间运行时出现。 过多的可疑页会导致记录器频繁运行。| 
|BLOB_METADATA |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|BMPALLOCATION |在同步大位图筛选器的分配时，与并行批处理模式计划一起出现。 如果等待太久，无法通过优化查询（如添加索引）来减少等待时间，请考虑调整并行度的开销阈值或降低并行度。<br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|BMPBUILD |在同步大型位图筛选器的生成时，在并行批处理模式计划中发生。 如果等待太久，无法通过优化查询（如添加索引）来减少等待时间，请考虑调整并行度的开销阈值或降低并行度。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|BMPREPARTITION |在同步大位图筛选器的重新分区时，在并行批处理模式计划中发生。 如果等待太久，无法通过优化查询（如添加索引）来减少等待时间，请考虑调整并行度的开销阈值或降低并行度。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|BMPREPLICATION |当跨工作线程同步大型位图筛选器的复制时，在并行批处理模式计划中发生。 如果等待太久，无法通过优化查询（如添加索引）来减少等待时间，请考虑调整并行度的开销阈值或降低并行度。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|BPSORT |在跨多个线程同步数据集的排序时，与并行批处理模式计划一起出现。 如果等待太久，无法通过优化查询（如添加索引）来减少等待时间，请考虑调整并行度的开销阈值或降低并行度。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|BROKER_CONNECTION_RECEIVE_TASK |在等待访问以便在连接端点上接收消息时出现。 已序列化对端点的接收访问。| 
|BROKER_DISPATCHER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|BROKER_ENDPOINT_STATE_MUTEX |当存在访问 Service Broker 连接终结点状态的争用时出现。 已序列化对更改状态的访问。| 
|BROKER_EVENTHANDLER |当任务正在 Service Broker 的主事件处理程序中等待时出现。 出现时间应该非常短暂。| 
|BROKER_FORWARDER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|BROKER_INIT |在每个活动数据库中初始化 Service Broker 时发生。 该状态应当频繁出现。| 
|BROKER_MASTERSTART |当某任务正在等待 Service Broker 的主事件处理程序启动时出现。 出现时间应该非常短暂。| 
|BROKER_RECEIVE_WAITFOR |当 RECEIVE WAITFOR 正在等待时出现。 这可能意味着在队列中没有消息可以接收，或者锁争用阻止其从队列接收消息。| 
|BROKER_REGISTERALLENDPOINTS |在 Service Broker 连接终结点初始化期间出现。 出现时间应该非常短暂。| 
|BROKER_SERVICE |当与目标服务关联的 Service Broker 目标列表更新或重新确定优先级时出现。| 
|BROKER_SHUTDOWN |当 Service Broker 的计划关闭时发生。 该状态出现的时间应当尽量短暂。| 
|BROKER_START |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|BROKER_TASK_SHUTDOWN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|BROKER_TASK_STOP |Service Broker 队列任务处理程序尝试关闭任务时发生。 已序列化状态检查，并且必须预先处于运行状态。| 
|BROKER_TASK_SUBMIT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|BROKER_TO_FLUSH |Service Broker 懒惰惰性将内存中传输对象刷新到工作表时发生。| 
|BROKER_TRANSMISSION_OBJECT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|BROKER_TRANSMISSION_TABLE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|BROKER_TRANSMISSION_WORK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|BROKER_TRANSMITTER |Service Broker 发送器正在等待工作时出现。 Service Broker 具有一个称为发送器的组件，该组件可在一个或多个连接终结点上计划通过网络发送多个对话框中的消息。 发射器有2个专用线程用于实现此目的。 当这些发送器线程等待使用传输连接发送对话消息时，将对此等待类型收费。 此等待类型的高 waiting_tasks_count 值为这些发送器线程的间歇性工作，并且不会指示任何性能问题。 如果根本不使用 service broker，则对于2个发送器线程，waiting_tasks_count 应为 2 () 并且 wait_time_ms 应为自实例启动后的持续时间的两倍。 请参阅 [Service broker 等待统计](/archive/blogs/sql_service_broker/service-broker-wait-types)信息。|
|BUILTIN_HASHKEY_MUTEX |可能在实例启动之后而在初始化内部数据结构时出现。 数据结构初始化之后将不会再次出现。| 
|CHANGE_TRACKING_WAITFORCHANGES |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|CHECK_PRINT_RECORD |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|CHECK_SCANNER_MUTEX |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|CHECK_TABLES_INITIALIZATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|CHECK_TABLES_SINGLE_SCAN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|CHECK_TABLES_THREAD_BARRIER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|CHECKPOINT_QUEUE |当检查点任务正在等待下一个检查点请求时出现。| 
|CHKPT |在服务器启动时出现以通知检查点线程可以启动。| 
|CLEAR_DB |在执行会更改数据库状态的操作过程中发生，例如打开或关闭数据库。| 
|CLR_AUTO_EVENT |当某任务当前正在执行公共语言运行时 (CLR) 执行并且正在等待特殊的自动事件启动时出现。 通常会出现长时间等待，这并不意味着出现问题。| 
|CLR_CRST |当某任务当前正在执行 CLR 执行并且正在等待输入当前由另一项任务正在使用的任务的关键部分时出现。| 
|CLR_JOIN |当某任务当前正在执行 CLR 执行并且正在等待另一项任务结束时出现。 当两任务之间具有联接时出现该等待状态。| 
|CLR_MANUAL_EVENT |当某任务当前正在执行 CLR 执行并且正在等待特定手动事件启动时出现。| 
|CLR_MEMORY_SPY |当为用于记录来自 CLR 的所有虚拟内存分配的数据结构等待获取锁时出现。 如果存在并行访问，该数据结构将被锁定以维护其完整性。| 
|CLR_MONITOR |当某任务当前正在执行 CLR 执行并且正在等待获取用于监视器的锁时出现。| 
|CLR_RWLOCK_READER |当某任务当前正在执行 CLR 执行并且正在等待读取器锁时出现。| 
|CLR_RWLOCK_WRITER |当某任务当前正在执行 CLR 执行并且正在等待编写器锁时出现。| 
|CLR_SEMAPHORE |当某任务当前正在执行 CLR 执行并且正在等待信号量时出现。| 
|CLR_TASK_START |在等待 CLR 任务完成启动时出现。| 
|CLRHOST_STATE_ACCESS |当等待获取对 CLR 宿主数据结构的独占访问时出现。 当设置或关闭 CLR 运行时时出现此等待类型。| 
|CMEMPARTITIONED |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|CMEMTHREAD |当某任务正在等待线程安全内存对象时出现。 当多项任务尝试分配来自同一个内存对象的内存而导致出现争用时，便可能延长等待时间。| 
|COLUMNSTORE_BUILD_THROTTLE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|COLUMNSTORE_COLUMNDATASET_SESSION_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|COMMIT_TABLE |仅限内部使用。| 
|CONNECTION_ENDPOINT_LOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|COUNTRECOVERYMGR |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|CREATE_DATINISERVICE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|CXCONSUMER <a name="cxconsumer"></a>|当使用者线程 (父) 等待制造者线程发送行时，并行查询计划发生。 CXCONSUMER 等待是由从其制造者线程中用尽行的 Exchange 迭代器导致的。 这是并行查询执行的正常部分。 <br /><br /> **适用** 于： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从 SP2 开始 ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] ， [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3) ， [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ， [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]|
|CXPACKET <a name="cxpacket"></a>|当等待同步查询处理器 [交换迭代器](../../relational-databases/showplan-logical-and-physical-operators-reference.md)时，以及在生成和使用行时，在并行查询计划中发生。 如果等待情况过大，并且无法通过优化查询来降低 (例如添加索引) ，请考虑调整并行的开销阈值或降低最大并行度 (MaxDOP) 。 <br /><br /> **注意：** 从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始，CXPACKET 仅指等待同步 Exchange 迭代器和生成行。 使用行的线程在 CXCONSUMER 等待类型中单独跟踪。 如果使用者线程速度太慢，则 Exchange 迭代器缓冲区可能会变满并导致 CXPACKET 等待。 <br /><br /> **注意：** 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 和中 [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] ，CXPACKET 仅指等待生成行的线程。 Exchange 迭代器同步在 CXSYNC_PORT 和 CXSYNC_CONSUMER 等待类型中单独跟踪。 使用行的线程在 CXCONSUMER 等待类型中单独跟踪。<br /> | 
|CXSYNC_PORT|在等待打开、关闭和同步制造者和使用者线程之间的 [Exchange 迭代器](../../relational-databases/showplan-logical-and-physical-operators-reference.md) 端口时，在并行查询计划中发生。 例如，如果查询计划具有长排序操作，则 CXSYNC_PORT 等待可能会更高，因为必须先完成排序，然后才能同步 Exchange 迭代器端口。 <br /><br /> **适用** 于： [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 、 [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]| 
|CXSYNC_CONSUMER|在等待在所有使用者线程之间到达 [Exchange 迭代器](../../relational-databases/showplan-logical-and-physical-operators-reference.md) 同步点时使用并行查询计划时发生。 <br /><br /> **适用** 于： [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 、 [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]| 
|CXROWSET_SYNC |在并行范围扫描期间出现。| 
|DAC_INIT |当正在初始化专用管理员连接时出现。| 
|DBCC_SCALE_OUT_EXPR_CACHE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|DBMIRROR_DBM_EVENT |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|DBMIRROR_DBM_MUTEX |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|DBMIRROR_EVENTS_QUEUE |在数据库镜像等待处理事件时出现。| 
|DBMIRROR_SEND |当某任务正在等待清除网络层的通信积压以便能够发送消息时出现。 指示通信层正在开始重载并影响数据库镜像数据吞吐量。| 
|DBMIRROR_WORKER_QUEUE |指示数据库镜像工作线程任务正在等待更多的工作。| 
|DBMIRRORING_CMD |当某任务正在等待日志记录刷新到磁盘时出现。 该等待状态应当保留较长的时间。| 
|DBSEEDING_FLOWCONTROL |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|DBSEEDING_OPERATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|DEADLOCK_ENUM_MUTEX |在死锁监视器和 sys.dm_os_waiting_tasks 尝试确保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不同时运行多个死锁搜索时出现。| 
|DEADLOCK_TASK_SEARCH |长时间等待此资源指示服务器正在 sys.dm_os_waiting_tasks 之上执行查询，并且这些查询正在阻止死锁监视器运行死锁搜索。 该等待类型仅供死锁监视器使用。 sys.dm_os_waiting_tasks 之上的查询使用 DEADLOCK_ENUM_MUTEX。| 
|DEBUG |在 Transact-sql 和 CLR 调试内部同步期间出现。| 
|DIRECTLOGCONSUMER_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|DIRTY_PAGE_POLL |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|DIRTY_PAGE_SYNC |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|DIRTY_PAGE_TABLE_LOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|DISABLE_VERSIONING |当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 轮询版本事务管理器，以查看最早的活动事务的时间戳是否晚于状态开始更改时的时间戳时出现。 如果是，则所有在 ALTER DATABASE 语句运行之前启动的快照事务都已完成。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过 ALTER DATABASE 语句禁用版本控制时使用该等待状态。| 
|DISKIO_SUSPEND |当某任务正在等待访问文件（外部备份处于活动状态）时出现。 针对每个正在等待的用户进程报告该状态。 每个用户进程大于五的计数可能指示外部备份需要太长时间才能完成。| 
|DISPATCHER_PRIORITY_QUEUE_SEMAPHORE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|DISPATCHER_QUEUE_SEMAPHORE |当调度程序池中的线程正在等待更多要处理的工作时出现。 当调度程序处于空闲状态时，此等待类型的等待时间预计要增加。| 
|DLL_LOADING_MUTEX |在等待 XML 分析器 DLL 加载时出现。| 
|DPT_ENTRY_LOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|DROP_DATABASE_TIMER_TASK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|DROPTEMP |在上次尝试删除临时对象失败后再进行下次尝试之前出现。 对于每一次失败的删除尝试，等待持续时间都以指数形式增长。| 
|DTC |当某任务正在等待用于管理状态转换的事件时出现。 此状态控制当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 接收到 MS dtc 服务不可用的通知后，Microsoft 分布式事务处理协调器 (MS) dtc 的恢复发生的时间。| 
|DTC_ABORT_REQUEST |当 MS DTC 工作线程会话正在等待获得 MS DTC 事务的所有权时，在该会话中出现。 当 MS DTC 拥有了事务后，该会话可以回滚事务。 通常，该会话将等待另一个正在使用事务的会话。| 
|DTC_RESOLVE |当恢复任务正在等待跨数据库事务中的 master 数据库以查询该事务的结果时出现。| 
|DTC_STATE |当某任务正在等待对内部 MS DTC 全局状态对象的更改进行保护的事件时出现。 该状态应当保持非常短的时间。| 
|DTC_TMDOWN_REQUEST |当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 接收到 MS DTC 服务不可用的通知时，在 MS DTC 工作线程会话中出现。 首先，工作线程将等待 MS DTC 恢复进程启动。 然后，工作线程等待获取其正在处理的分布式事务的结果。 此过程可能一直执行，直到重新建立与 MS DTC 服务的连接。| 
|DTC_WAITFOR_OUTCOME |当恢复任务等待 MS DTC 处于活动状态以启用准备好的事务的解决方法时出现。| 
|DTCNEW_ENLIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|DTCNEW_PREPARE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|DTCNEW_RECOVERY |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|DTCNEW_TM |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|DTCNEW_TRANSACTION_ENLISTMENT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|DTCPNTSYNC |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|DUMP_LOG_COORDINATOR |当主任务正在等待子任务生成数据时出现。 该状态通常不会出现。 长时间的等待指示出现意外的阻塞。 应当对子任务进行调查。| 
|DUMP_LOG_COORDINATOR_QUEUE |仅限内部使用。| 
|DUMPTRIGGER |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|EC |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|EE_PMOLOCK |在语句执行过程中特定的内存分配类型同步期间出现。| 
|EE_SPECPROC_MAP_INIT |在对内部过程哈希表创建进行同步期间发生。 此等待只能发生在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启动之后对哈希表的初始访问期间。| 
|ENABLE_EMPTY_VERSIONING |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|ENABLE_VERSIONING |当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在声明数据库可以转换到快照隔离允许的状态之前，等待该数据库中的所有更新事务完成时出现。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过 ALTER DATABASE 语句启用快照隔离时使用该状态。| 
|ERROR_REPORTING_MANAGER |在对多个并发错误日志初始化进行同步期间发生。| 
|EXCHANGE |在并行查询过程中查询处理器交换迭代器同步期间出现。| 
|EXECSYNC |在并行查询过程中同步与交换迭代器无关的区域内的查询处理器期间出现。 例如，此类区域包括位图、二进制大型对象 (LOB) 以及假脱机迭代器等。 LOB 可能会经常使用该等待状态。| 
|EXECUTION_PIPE_EVENT_INTERNAL |当同步通过连接上下文提交的批处理执行的创建器和使用者部件期间出现。| 
|EXTERNAL_RG_UPDATE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|EXTERNAL_SCRIPT_NETWORK_IO |仅限内部使用。 <br /><br /> **适用** 于： [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 到当前。| 
|EXTERNAL_SCRIPT_PREPARE_SERVICE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|EXTERNAL_SCRIPT_SHUTDOWN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|EXTERNAL_WAIT_ON_LAUNCHER， |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|FABRIC_HADR_TRANSPORT_CONNECTION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|FABRIC_REPLICA_CONTROLLER_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|FABRIC_REPLICA_CONTROLLER_STATE_AND_CONFIG |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|FABRIC_REPLICA_PUBLISHER_EVENT_PUBLISH |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|FABRIC_REPLICA_PUBLISHER_SUBSCRIBER_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|FABRIC_WAIT_FOR_BUILD_REPLICA_EVENT_PROCESSING |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|FAILPOINT |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|FCB_REPLICA_READ |当同步快照（或 DBCC 创建的临时快照）稀疏文件的读取时出现。| 
|FCB_REPLICA_WRITE |当同步快照（或 DBCC 创建的临时快照）稀疏文件的页推送或页请求时出现。| 
|FEATURE_SWITCHES_UPDATE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|FFT_NSO_DB_KILL_FLAG |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_NSO_DB_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_NSO_FCB |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_NSO_FCB_FIND |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_NSO_FCB_PARENT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_NSO_FCB_RELEASE_CACHED_ENTRIES |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_NSO_FCB_STATE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|FFT_NSO_FILEOBJECT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_NSO_TABLE_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_NTFS_STORE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_RECOVERY |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_RSFX_COMM |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_RSFX_WAIT_FOR_MEMORY |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_STARTUP_SHUTDOWN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_STORE_DB |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_STORE_ROWSET_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FFT_STORE_TABLE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FILE_VALIDATION_THREADS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|FILESTREAM_CACHE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FILESTREAM_CHUNKER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FILESTREAM_CHUNKER_INIT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FILESTREAM_FCB |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FILESTREAM_FILE_OBJECT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FILESTREAM_WORKITEM_QUEUE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FILETABLE_SHUTDOWN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FOREIGN_REDO |仅限内部使用。 <br /><br /> **适用** 于： [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 到当前。| 
|FORWARDER_TRANSITION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|FS_FC_RWLOCK |当 FILESTREAM 垃圾收集器等待执行下列操作之一时出现：| 
|FS_GARBAGE_COLLECTOR_SHUTDOWN |当 FILESTREAM 垃圾收集器等待清除任务完成时出现。| 
|FS_HEADER_RWLOCK |当等待获取对 FILESTREAM 数据容器的 FILESTREAM 标头的访问，以便读取或更新 FILESTREAM 标头文件 (Filestream.hdr) 中的内容时出现。| 
|FS_LOGTRUNC_RWLOCK |当等待获取对 FILESTREAM 日志截断的访问以执行下列操作之一时出现：| 
|FSA_FORCE_OWN_XACT |当 FILESTREAM 文件 I/O 操作需要绑定到关联的事务，但该事务当前由另一个会话拥有时出现。| 
|FSAGENT |当 FILESTREAM 文件 I/O 操作等待的 FILESTREAM 代理资源正由另一个文件 I/O 操作使用时出现。| 
|FSTR_CONFIG_MUTEX |当等待另一个 FILESTREAM 功能重新配置完成时出现。| 
|FSTR_CONFIG_RWLOCK |当等待序列化对 FILESTREAM 配置参数的访问时出现。| 
|FT_COMPROWSET_RWLOCK |全文正在等待片段元数据操作。 记录为仅供参考。 不支持。 不保证以后的兼容性。| 
|FT_IFTS_RWLOCK |全文正在等待内部同步。 记录为仅供参考。 不支持。 不保证以后的兼容性。| 
|FT_IFTS_SCHEDULER_IDLE_WAIT |全文计划程序睡眠等待类型。 计划程序空闲。| 
|FT_IFTSHC_MUTEX |全文正在等待 fdhost 控制操作。 记录为仅供参考。 不支持。 不保证以后的兼容性。| 
|FT_IFTSISM_MUTEX |全文正在等待通信操作。 记录为仅供参考。 不支持。 不保证以后的兼容性。| 
|FT_MASTER_MERGE |全文正在等待主合并操作。 记录为仅供参考。 不支持。 不保证以后的兼容性。| 
|FT_MASTER_MERGE_COORDINATOR |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FT_METADATA_MUTEX |记录为仅供参考。 不支持。 不保证以后的兼容性。| 
|FT_PROPERTYLIST_CACHE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|FT_RESTART_CRAWL |在全文爬网需要从上一个已知可用点重新启动以便从暂时故障中恢复时出现。 等待使当前正在此总体中工作的工作线程任务完成或退出当前步骤。| 
|FULLTEXT GATHERER |在同步全文操作期间发生。| 
|GDMA_GET_RESOURCE_OWNER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|GHOSTCLEANUP_UPDATE_STATS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|GHOSTCLEANUPSYNCMGR |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|GLOBAL_QUERY_CANCEL |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|GLOBAL_QUERY_CLOSE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|GLOBAL_QUERY_CONSUMER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|GLOBAL_QUERY_PRODUCER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|GLOBAL_TRAN_CREATE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|GLOBAL_TRAN_UCS_SESSION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|GUARDIAN |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|HADR_AG_MUTEX |当 Always On DDL 语句或 Windows Server 故障转移群集命令正在等待对可用性组的配置进行独占读/写访问时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_AR_CRITICAL_SECTION_ENTRY |当 Always On DDL 语句或 Windows Server 故障转移群集命令正在等待对关联可用性组的本地副本的运行时状态的独占读/写访问时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_AR_MANAGER_MUTEX |在可用性副本关闭正在等待完成启动或可用性副本启动正在等待完成关闭时发生。 仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_AR_UNLOAD_COMPLETED |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_ARCONTROLLER_NOTIFICATIONS_SUBSCRIBER_LIST |可用性副本事件（例如，状态更改或配置更改）的发布服务器正在等待对事件订阅服务器列表的独占的读/写访问。 仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_BACKUP_BULK_LOCK |Always On 主数据库收到了来自辅助数据库的备份请求，正在等待后台线程完成获取或释放 BulkOp 锁的请求。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_BACKUP_QUEUE |Always On 主数据库的备份后台线程正在等待辅助数据库的新工作请求。 （通常情况下，这在主数据库正持有 BulkOp 日志并且正在等待辅助数据库来指示主数据库可以释放锁时发生。） <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_CLUSAPI_CALL |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]线程正在等待从) 到抢先模式的非抢先模式 (切换 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (由操作系统) 计划，以便调用 Windows Server 故障转移群集 api。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_COMPRESSED_CACHE_SYNC |正在等待访问压缩后日志块的缓存，用来避免发送到多个辅助数据库的日志块的冗余压缩。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_CONNECTIVITY_INFO |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_DATABASE_FLOW_CONTROL |在已达到排队消息的最大数目时正在等待消息发送到伙伴。 指示日志扫描的运行运行速度要快于网络发送速度。 只有当网络发送速度比预期要慢时这种情况才会成为问题。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_DATABASE_VERSIONING_STATE |在 Always On 辅助数据库的版本控制状态更改时发生。 此等待适用于内部数据结构，通常是非常短的，无直接影响数据访问。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_DATABASE_WAIT_FOR_RECOVERY |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HADR_DATABASE_WAIT_FOR_RESTART |正在等待数据库在 Always On 可用性组控制下重新启动。 在一般情况下，这不是客户问题，因为预期在这里就应该发生等待。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING |在 Always On 可用性组的可读辅助数据库中，对对象 (s) 的查询在行版本控制上被阻止，同时等待在辅助副本启用读取工作负荷时提交或回滚所有正在进行的事务。 这一等待类型确保在快照隔离下执行查询前行版本可用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_DB_COMMAND |等待对会话消息的响应 (这种情况下，使用 Always On 会话消息基础结构) ，这需要另一方显式响应。 多种不同的消息类型使用此等待类型。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_DB_OP_COMPLETION_SYNC |等待对会话消息的响应 (这种情况下，使用 Always On 会话消息基础结构) ，这需要另一方显式响应。 多种不同的消息类型使用此等待类型。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_DB_OP_START_SYNC |Always On DDL 语句或 Windows Server 故障转移群集命令正在等待对可用性数据库及其运行时状态的序列化访问。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_DBR_SUBSCRIBER |可用性副本事件（例如，状态更改或配置更改）的发布服务器正在等待对与某一可用性数据库相对应的事件订阅服务器的运行时状态的独占读/写访问。 仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_DBR_SUBSCRIBER_FILTER_LIST |可用性副本事件（例如，状态更改或配置更改）的发布服务器正在等待对与可用性数据库相对应的事件订阅服务器的列表的独占读/写访问。 仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_DBSEEDING |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|HADR_DBSEEDING_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|HADR_DBSTATECHANGE_SYNC |并发控制等待更新数据库副本的内部状态。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_FABRIC_CALLBACK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|HADR_FILESTREAM_BLOCK_FLUSH |FILESTREAM Always On 传输管理器正在等待，直到处理完日志块。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_FILESTREAM_FILE_CLOSE |FILESTREAM Always On 传输管理器正在等待，直到下一个 FILESTREAM 文件得到处理并关闭其句柄。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_FILESTREAM_FILE_REQUEST |Always On 辅助副本正在等待主副本在撤消过程中发送所有请求的 FILESTREAM 文件。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_FILESTREAM_IOMGR |FILESTREAM Always On 传输管理器正在等待在启动或关闭过程中保护 FILESTREAM Always On i/o 管理器的 R/W 锁。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_FILESTREAM_IOMGR_IOCOMPLETION |FILESTREAM Always On i/o 管理器正在等待 i/o 完成。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_FILESTREAM_MANAGER |FILESTREAM Always On 传输管理器正在等待在启动或关闭过程中保护 FILESTREAM Always On 传输管理器的 R/W 锁。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_FILESTREAM_PREPROC |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HADR_GROUP_COMMIT |事务提交处理正在等待允许组提交，以便可将多个提交日志记录放置于单个日志块中。 此等待是一种预期的条件，可用于优化日志 I/O、捕获和发送操作。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_LOGCAPTURE_SYNC |在创建或销毁扫描时围绕日志捕获或应用对象的并发控制。 这是伙伴更改状态或连接状态时的预期等待。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_LOGCAPTURE_WAIT |等待日志记录变得可用。 可在等待连接生成新的日志记录时或在读取未处于缓存中的记录时等待 I/O 完成时发生。 这是日志扫描与日志的结尾几乎保持同步或正在从磁盘读取时的预期的等待。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_LOGPROGRESS_SYNC |在更新数据库副本的日志进度状态时的并发控制等待。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_NOTIFICATION_DEQUEUE |处理 Windows Server 故障转移群集通知的后台任务正在等待下一个通知。 仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_NOTIFICATION_WORKER_EXCLUSIVE_ACCESS |Always On 可用性副本管理器正在等待序列化访问处理 Windows Server 故障转移群集通知的后台任务的运行时状态。 仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_NOTIFICATION_WORKER_STARTUP_SYNC |后台任务正在等待处理 Windows Server 故障转移群集通知的后台任务完成启动。 仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_NOTIFICATION_WORKER_TERMINATION_SYNC |后台任务正在等待处理 Windows Server 故障转移群集通知的后台任务终止。 仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_PARTNER_SYNC |对伙伴列表的并发控制等待。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_READ_ALL_NETWORKS |等待获取对 WSFC 网络列表的读取或写入访问。 仅限内部使用。 注意：引擎保留在动态管理视图中使用的 WSFC 网络列表， (例如 sys.dm_hadr_cluster_networks) 或验证引用 WSFC 网络信息的 Always On Transact-sql 语句。 此列表将在引擎启动、WSFC 相关通知和内部 Always On restart (进行更新，例如，丢失和重新获得 WSFC 仲裁) 。 在该列表中的更新正在进行时，任务通常会被阻止。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_RECOVERY_WAIT_FOR_CONNECTION |正在等待辅助数据库在运行恢复之前连接到主数据库。 这是预期的等待，如果与主数据库的连接的建立速度比较慢，则此等待可能会延长。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_RECOVERY_WAIT_FOR_UNDO |数据库恢复正在等待辅助数据库完成恢复和初始化阶段以便恢复到主数据库的公共日志点。 这是故障转移后的预期的等待。可以通过 Windows 系统监视器 (perfmon.exe) 和动态管理视图来跟踪撤消进度。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_REPLICAINFO_SYNC |正在等待并发控制更新当前副本状态。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_SEEDING_CANCELLATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HADR_SEEDING_FILE_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HADR_SEEDING_LIMIT_BACKUPS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HADR_SEEDING_SYNC_COMPLETION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HADR_SEEDING_TIMEOUT_TASK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HADR_SEEDING_WAIT_FOR_COMPLETION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HADR_SYNC_COMMIT |正在等待针对同步辅助数据库的事务提交处理来强制写入日志。 事务延迟性能计数器也会反映这一等待。 这种类型的等待是针对同步的可用性组的预期等待，指示将日志发送到辅助数据库、写入日志和确认日志的时间。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_SYNCHRONIZING_THROTTLE |正在等待事务提交处理允许同步的辅助数据库几乎与日志的主结尾保持同步，以便转换到同步的状态。 这是辅助数据库几乎保持同步的预期的等待。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_TDS_LISTENER_SYNC |内部 Always On 系统或 WSFC 群集将请求侦听器已启动或已停止。 对此请求的处理始终是异步的，并且存在删除冗余请求的机制。 还存在此进程由于配置更改而挂起的时刻。 与此侦听器同步机制相关的所有等待都使用此等待类型。 仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_TDS_LISTENER_SYNC_PROCESSING |在需要启动和/或停止可用性组侦听器的 Always On Transact-sql 语句的末尾使用。 因为启动/停止操作异步完成，所以，在侦听器的状况为已知时，用户线程将阻止使用此等待类型。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_THROTTLE_LOG_RATE_GOVERNOR |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO | 当使用较小的计算大小配置异地复制辅助数据库时出现 (低于 SLO) 的主要部分。 由于辅助数据库延迟的日志消耗，主数据库将受到限制。 这是因为辅助数据库的计算能力不足以跟上主数据库的变化率。 <br /><br /> **适用** 于： Azure SQL 数据库| 
|HADR_THROTTLE_LOG_RATE_LOG_SIZE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|HADR_THROTTLE_LOG_RATE_SEEDING |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|HADR_TIMER_TASK |正在等待获取计时器任务对象上的锁，并且还可用于正在执行工作的时间之间的实际等待。 例如，对于每10秒运行一次的任务，在一次执行后 Always On 可用性组将等待约10秒来重新计划任务，并在此处提供等待。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_TRANSPORT_DBRLIST |正在等待访问传输层的数据库副本列表。 用于向其授予访问权限的自旋锁。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_TRANSPORT_FLOW_CONTROL |等待未确认的未确认 Always On 消息数超过流控制阈值时。 此等待基于可用性副本到副本（而非数据库到数据库）。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_TRANSPORT_SESSION |Always On 可用性组正在等待更改或访问基础传输状态。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_WORK_POOL |并发控制等待 Always On 可用性组后台工作任务对象。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_WORK_QUEUE |Always On 可用性组后台工作线程正在等待分配新工作。 这是存在正等待新工作的已就绪工作线程时的预期等待，这是正常状态。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HADR_XRF_STACK_ACCESS |访问 Always On 可用性数据库) 的扩展恢复分叉堆栈 (查找、添加和删除。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HCCO_CACHE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HK_RESTORE_FILEMAP |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HKCS_PARALLEL_MIGRATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HKCS_PARALLEL_RECOVERY |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|HTBUILD |在同步哈希联接/聚合的输入端的哈希表生成时，在并行批处理模式计划中发生。 如果等待太久，无法通过优化查询（如添加索引）来减少等待时间，请考虑调整并行度的开销阈值或降低并行度。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HTDELETE |在哈希联接/聚合结束时进行同步时，将在并行批处理模式计划中发生。 如果等待太久，无法通过优化查询（如添加索引）来减少等待时间，请考虑调整并行度的开销阈值或降低并行度。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|HTMEMO |在扫描哈希表以输出哈希联接/聚合中的匹配/不匹配的情况下进行同步时，并行批处理模式计划发生。 如果等待太久，无法通过优化查询（如添加索引）来减少等待时间，请考虑调整并行度的开销阈值或降低并行度。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|HTREINIT |在为下一个部分联接重置哈希联接/聚合之前同步时，在并行批处理模式计划中发生。 如果等待太久，无法通过优化查询（如添加索引）来减少等待时间，请考虑调整并行度的开销阈值或降低并行度。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|HTREPARTITION |在同步哈希联接/聚合的输入端的哈希表的重新分区时，与并行批处理模式计划一起出现。 如果等待太久，无法通过优化查询（如添加索引）来减少等待时间，请考虑调整并行度的开销阈值或降低并行度。<br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|HTTP_ENUMERATION |在启动时出现，以枚举 HTTP 端点以启动 HTTP。| 
|HTTP_START |当连接正在等待 HTTP 完成初始化时出现。| 
|HTTP_STORAGE_CONNECTION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|IMPPROV_IOWAIT |当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 等待 Bulkload I/O 完成时出现。| 
|INSTANCE_LOG_RATE_GOVERNOR |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|INTERNAL_TESTING |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|IO_AUDIT_MUTEX |在跟踪事件缓冲区同步期间出现。| 
|IO_COMPLETION |在等待 I/O 操作完成时出现。 通常，该等待类型表示非数据页 I/O。 数据页 i/o 完成等待显示为 PAGEIOLATCH \_ \* 等待。| 
|IO_QUEUE_LIMIT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|IO_RETRY |当 I/O 操作（例如读取磁盘或写入磁盘）由于资源不足而失败，然后重试时出现。| 
|IOAFF_RANGE_QUEUE |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|KSOURCE_WAKEUP |在等待来自服务控制管理器的请求期间由服务控制任务使用。 可能会出现长时间等待，这并不指示出现问题。| 
|KTM_ENLISTMENT |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|KTM_RECOVERY_MANAGER |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|KTM_RECOVERY_RESOLUTION |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|LATCH_DT |等待 DT（破坏）闩锁时出现。 它不包括缓冲区闩锁或事务标记闩锁。 \_ \* Sys.dm_os_latch_stats 中提供了闩锁等待列表。 请注意，sys.dm_os_latch_stats 将 LATCH_NL、LATCH_SH、LATCH_UP、LATCH_EX 以及 LATCH_DT 等待分到一组。| 
|LATCH_EX |等待 EX（排他）闩锁时出现。 它不包括缓冲区闩锁或事务标记闩锁。 \_ \* Sys.dm_os_latch_stats 中提供了闩锁等待列表。 请注意，sys.dm_os_latch_stats 将 LATCH_NL、LATCH_SH、LATCH_UP、LATCH_EX 以及 LATCH_DT 等待分到一组。| 
|LATCH_KP |等待 KP（保持）闩锁时出现。 它不包括缓冲区闩锁或事务标记闩锁。 \_ \* Sys.dm_os_latch_stats 中提供了闩锁等待列表。 请注意，sys.dm_os_latch_stats 将 LATCH_NL、LATCH_SH、LATCH_UP、LATCH_EX 以及 LATCH_DT 等待分到一组。| 
|LATCH_NL |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|LATCH_SH |等待 SH（共享）闩锁时出现。 它不包括缓冲区闩锁或事务标记闩锁。 \_ \* Sys.dm_os_latch_stats 中提供了闩锁等待列表。 请注意，sys.dm_os_latch_stats 将 LATCH_NL、LATCH_SH、LATCH_UP、LATCH_EX 以及 LATCH_DT 等待分到一组。| 
|LATCH_UP |等待 UP（更新）闩锁时出现。 它不包括缓冲区闩锁或事务标记闩锁。 \_ \* Sys.dm_os_latch_stats 中提供了闩锁等待列表。 请注意，sys.dm_os_latch_stats 将 LATCH_NL、LATCH_SH、LATCH_UP、LATCH_EX 以及 LATCH_DT 等待分到一组。| 
|LAZYWRITER_SLEEP |挂起惰性编写器任务时发生。 正在等待的后台任务所用时间的度量值。 在查找用户阻隔点所时不要考虑该状态。| 
|LCK_M_BU |当某任务正在等待获取大容量更新 (BU) 锁时出现。| 
|LCK_M_BU_ABORT_BLOCKERS |在任务等待获取使用中止阻塞程序的大容量更新 (BU) 锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_BU_LOW_PRIORITY |在任务等待获取低优先级的大容量更新 (BU) 锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_IS |当某任务正在等待获取意向共享 (IS) 锁时出现。| 
|LCK_M_IS_ABORT_BLOCKERS |在任务等待获取使用中止阻塞程序的意向共享 (IS) 锁时出现。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_IS_LOW_PRIORITY |在任务等待获取低优先级的意向共享 (IS) 锁时出现。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_IU |当某任务正在等待获取意向更新 (IU) 锁时出现。| 
|LCK_M_IU_ABORT_BLOCKERS |在任务等待获取使用中止阻塞程序的意向更新 (IU) 锁时出现。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_IU_LOW_PRIORITY |在任务等待获取低优先级的意向更新 (IU) 锁时出现。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_IX |当某任务正在等待获取意向排他 (IX) 锁时出现。| 
|LCK_M_IX_ABORT_BLOCKERS |在任务等待获取使用中止阻塞程序的意向排他 (IX) 锁时出现。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_IX_LOW_PRIORITY |在任务等待获取低优先级的意向排他 (IX) 锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RIn_NL |当某任务正在等待获取当前键值上的 NULL 锁以及当前键和上一个键之间的插入范围锁时出现。 键上的 NULL 锁是指立即释放的锁。| 
|LCK_M_RIn_NL_ABORT_BLOCKERS |在任务等待获取当前键值上使用中止阻塞程序的 NULL 锁以及当前键和上一个键之间使用中止阻塞程序的插入范围锁时发生。 键上的 NULL 锁是指立即释放的锁。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RIn_NL_LOW_PRIORITY |在任务等待获取当前键值上低优先级的 NULL 锁以及当前键和上一个键之间低优先级的插入范围锁时发生。 键上的 NULL 锁是指立即释放的锁。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RIn_S |当某任务正在等待获取当前键值上的共享锁以及当前键和上一个键之间的插入范围锁时出现。| 
|LCK_M_RIn_S_ABORT_BLOCKERS |在任务等待获取当前键值上使用中止阻塞程序的共享锁以及当前键和上一个键之间使用中止阻塞程序的插入范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RIn_S_LOW_PRIORITY |在任务等待获取当前键值上低优先级的共享锁以及当前键和上一个键之间低优先级的插入范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RIn_U |任务正在等待获取当前键值上的更新锁以及当前键和上一个键之间的插入范围锁。| 
|LCK_M_RIn_U_ABORT_BLOCKERS |任务等待获取当前键值上使用中止阻塞程序的更新锁以及当前键和上一个键之间使用中止阻塞程序的插入范围锁。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RIn_U_LOW_PRIORITY |任务等待获取当前键值上低优先级的更新锁以及当前键和上一个键之间低优先级的插入范围锁。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RIn_X |当某任务正在等待获取当前键值上的排他锁以及当前键和上一个键之间的插入范围锁时出现。| 
|LCK_M_RIn_X_ABORT_BLOCKERS |在任务等待获取当前键值上使用中止阻塞程序的排他锁以及当前键和上一个键之间使用中止阻塞程序的插入范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RIn_X_LOW_PRIORITY |在任务等待获取当前键值上低优先级的排他锁以及当前键和上一个键之间低优先级的插入范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RS_S |当某任务正在等待获取当前键值上的共享锁以及当前键和上一个键之间的共享范围锁时出现。| 
|LCK_M_RS_S_ABORT_BLOCKERS |在任务等待获取当前键值上使用中止阻塞程序的共享锁以及当前键和上一个键之间使用中止阻塞程序的共享范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RS_S_LOW_PRIORITY |在任务等待获取当前键值上低优先级的共享锁以及当前键和上一个键之间低优先级的共享范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RS_U |当某任务正在等待获取当前键值上的更新锁以及当前键和上一个键之间的更新范围锁时出现。| 
|LCK_M_RS_U_ABORT_BLOCKERS |在任务等待获取当前键值上使用中止阻塞程序的更新锁以及当前键和上一个键之间使用中止阻塞程序的更新范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RS_U_LOW_PRIORITY |在任务等待获取当前键值上低优先级的更新锁以及当前键和上一个键之间低优先级的更新范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RX_S |当某任务正在等待获取当前键值上的共享锁以及当前键和上一个键之间的排他范围锁时出现。| 
|LCK_M_RX_S_ABORT_BLOCKERS |在任务等待获取当前键值上使用中止阻塞程序的共享锁以及当前键和上一个键之间使用中止阻塞程序的排他范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RX_S_LOW_PRIORITY |在任务等待获取当前键值上低优先级的共享锁以及当前键和上一个键之间低优先级的排他范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RX_U |当某任务正在等待获取当前键值上的更新锁以及当前键和上一个键之间的排他范围锁时出现。| 
|LCK_M_RX_U_ABORT_BLOCKERS |在任务等待获取当前键值上使用中止阻塞程序的更新锁以及当前键和上一个键之间使用中止阻塞程序的排他范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RX_U_LOW_PRIORITY |在任务等待获取当前键值上低优先级的更新锁以及当前键和上一个键之间低优先级的排他范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RX_X |当某任务正在等待获取当前键值上的排他锁以及当前键和上一个键之间的排他范围锁时出现。| 
|LCK_M_RX_X_ABORT_BLOCKERS |在任务等待获取当前键值上使用中止阻塞程序的排他锁以及当前键和上一个键之间使用中止阻塞程序的排他范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_RX_X_LOW_PRIORITY |在任务等待获取当前键值上低优先级的排他锁以及当前键和上一个键之间低优先级的排他范围锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_S |当某任务正在等待获取共享锁时出现。| 
|LCK_M_S_ABORT_BLOCKERS |在任务等待获取使用中止阻塞程序的共享锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_S_LOW_PRIORITY |在任务等待获取低优先级的共享锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_SCH_M |当某任务正在等待获取架构修改锁时出现。| 
|LCK_M_SCH_M_ABORT_BLOCKERS |在任务等待获取使用中止阻塞程序的架构修改锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_SCH_M_LOW_PRIORITY |在任务等待获取低优先级的架构修改锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_SCH_S |当某任务正在等待获取架构共享锁时出现。| 
|LCK_M_SCH_S_ABORT_BLOCKERS |在任务等待获取使用中止阻塞程序的架构共享锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_SCH_S_LOW_PRIORITY |在任务等待获取低优先级的架构共享锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_SIU |当某任务正在等待获取共享意向更新锁时出现。| 
|LCK_M_SIU_ABORT_BLOCKERS |在任务等待获取具有中止阻塞程序的共享意向更新锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_SIU_LOW_PRIORITY |在任务等待获取低优先级的共享意向更新锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_SIX |当某任务正在等待获取共享意向排他锁时出现。| 
|LCK_M_SIX_ABORT_BLOCKERS |在任务等待获取使用中止阻塞程序的共享意向排他锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_SIX_LOW_PRIORITY |在任务等待获取低优先级的意向排他共享锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_U |当某任务正在等待获取更新锁时出现。| 
|LCK_M_U_ABORT_BLOCKERS |在任务等待获取使用中止阻塞程序的更新锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_U_LOW_PRIORITY |在任务等待获取低优先级的更新锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_UIX |当某任务正在等待获取更新意向排他锁时出现。| 
|LCK_M_UIX_ABORT_BLOCKERS |在任务等待获取使用中止阻塞程序的意向排他更新锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_UIX_LOW_PRIORITY |在任务等待获取低优先级的意向排他更新锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_X |当某任务正在等待获取排他锁时出现。| 
|LCK_M_X_ABORT_BLOCKERS |在任务等待获取使用中止阻塞程序的排他锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LCK_M_X_LOW_PRIORITY |在任务等待获取低优先级的排他锁时发生。 与 ALTER TABLE 和 ALTER INDEX 的低优先级等待选项 (相关。 ) ， <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|LOG_POOL_SCAN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|LOG_RATE_GOVERNOR |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|LOGBUFFER |当某任务正在等待日志缓冲区的空间以存储日志记录时出现。 连续的高值可能指示日志设备无法跟上服务器生成的日志量。| 
|LOGCAPTURE_LOGPOOLTRUNCPOINT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|LOGGENERATION |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|LOGMGR |在数据库关闭过程中，当某任务正在等待任何未完成的日志 I/O 在关闭日志之前完成时出现。| 
|LOGMGR_FLUSH |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|LOGMGR_PMM_LOG |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|LOGMGR_QUEUE |在日志编写器任务等待工作请求时出现。| 
|LOGMGR_RESERVE_APPEND |当某任务正在等待查看日志截断是否能释放日志空间以使该任务能写入新的日志记录时出现。 请考虑为受影响的数据库增加日志文件的大小以减少该等待时间。| 
|LOGPOOL_CACHESIZE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|LOGPOOL_CONSUMER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|LOGPOOL_CONSUMERSET |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|LOGPOOL_FREEPOOLS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|LOGPOOL_MGRSET |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|LOGPOOL_REPLACEMENTSET |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|LOGPOOLREFCOUNTEDOBJECT_REFDONE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|LOWFAIL_MEMMGR_QUEUE |在等待可用内存期间出现。| 
|MD_AGENT_YIELD |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|MD_LAZYCACHE_RWLOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|MEMORY_ALLOCATION_EXT |在从内部 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存池或操作系统分配内存时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|MEMORY_GRANT_UPDATE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|METADATA_LAZYCACHE_RWLOCK |仅限内部使用。 <br /><br /> **适用** 于： [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] 仅限。 |  
|MIGRATIONBUFFER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|MISCELLANEOUS |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|MISCELLANEOUS |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|MSQL_DQ |当某任务正在等待分布式查询操作完成时出现。 它用于检测潜在的多个活动的结果集 (MARS) 应用程序死锁。 该等待将在分布式查询调用完成时结束。| 
|MSQL_XACT_MGR_MUTEX |当某任务正在等待获取会话事务管理器的所有权以执行会话级别事务操作时出现。| 
|MSQL_XACT_MUTEX |在事务使用同步期间出现。 请求必须先获取互斥体才可以使用事务。| 
|MSQL_XP |当某任务正在等待扩展存储过程结束时出现。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用该等待状态检测潜在的 MARS 应用程序死锁。 该等待将在扩展存储过程调用结束时停止。| 
|MSSEARCH |在全文搜索调用期间出现。 该等待在全文操作完成时结束。 它不指示争用，而指示全文操作的持续时间。| 
|NET_WAITFOR_PACKET |在网络读取过程中连接正在等待网络数据包时出现。| 
|NETWORKSXMLMGRLOAD |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|NODE_CACHE_MUTEX |仅限内部使用。| 
|OLEDB |当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 调用 SNAC OLE DB 提供程序 (sqlncli.msi) 或用于 SQL Server (MSOLEDBSQL) 的 Microsoft OLE DB 驱动程序时出现。 该等待类型不用于同步。 而是用于指示调用 OLE DB 访问接口的持续时间。| 
|ONDEMAND_TASK_QUEUE |在后台任务等待高优先级系统任务请求时出现。 长时间的等待指示一直没有要处理的高优先级请求，不应引起关注。| 
|PAGEIOLATCH_DT |在任务等待 I/O 请求中缓冲区的闩锁时发生。 闩锁请求处于“破坏”模式。 长时间的等待可能指示磁盘子系统出现问题。| 
|PAGEIOLATCH_EX |在任务等待 I/O 请求中缓冲区的闩锁时发生。 闩锁请求处于“独占”模式。 长时间的等待可能指示磁盘子系统出现问题。| 
|PAGEIOLATCH_KP |在任务等待 I/O 请求中缓冲区的闩锁时发生。 闩锁请求处于“保持”模式。 长时间的等待可能指示磁盘子系统出现问题。| 
|PAGEIOLATCH_NL |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|PAGEIOLATCH_SH |在任务等待 I/O 请求中缓冲区的闩锁时发生。 闩锁请求处于“共享”模式。 长时间的等待可能指示磁盘子系统出现问题。| 
|PAGEIOLATCH_UP |在任务等待 I/O 请求中缓冲区的闩锁时发生。 闩锁请求处于“更新”模式。 长时间的等待可能指示磁盘子系统出现问题。| 
|PAGELATCH_DT |在任务等待不处于 I/O 请求中的缓冲区闩锁时发生。 闩锁请求处于“破坏”模式。| 
|PAGELATCH_EX |在任务等待不处于 I/O 请求中的缓冲区闩锁时发生。 闩锁请求处于“独占”模式。| 
|PAGELATCH_KP |在任务等待不处于 I/O 请求中的缓冲区闩锁时发生。 闩锁请求处于“保持”模式。| 
|PAGELATCH_NL |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|PAGELATCH_SH |在任务等待不处于 I/O 请求中的缓冲区闩锁时发生。 闩锁请求处于“共享”模式。| 
|PAGELATCH_UP |在任务等待不处于 I/O 请求中的缓冲区闩锁时发生。 闩锁请求处于“更新”模式。| 
|PARALLEL_BACKUP_QUEUE |在序列化由 RESTORE HEADERONLY、RESTORE FILELISTONLY 或 RESTORE LABELONLY 生成的输出时出现。| 
|PARALLEL_REDO_DRAIN_WORKER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PARALLEL_REDO_FLOW_CONTROL |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PARALLEL_REDO_LOG_CACHE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PARALLEL_REDO_TRAN_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PARALLEL_REDO_TRAN_TURN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PARALLEL_REDO_WORKER_SYNC |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PARALLEL_REDO_WORKER_WAIT_WORK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PERFORMANCE_COUNTERS_RWLOCK |仅限内部使用。| 
|PHYSICAL_SEEDING_DMV |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|POOL_LOG_RATE_GOVERNOR |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PREEMPTIVE_ABR |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|PREEMPTIVE_AUDIT_ACCESS_EVENTLOG |在 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 操作系统 (SQLOS) 计划程序切换到抢先模式时发生，以便将审核事件写入 Windows 事件日志。 <br /><br /> **适用** 于： [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] 仅限。 |  
|PREEMPTIVE_AUDIT_ACCESS_SECLOG |在 SQLOS 计划程序切换到抢先模式时发生，以便将审核事件写入 Windows 安全日志。 <br /><br /> **适用** 于： [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] 仅限。 |  
|PREEMPTIVE_CLOSEBACKUPMEDIA |在 SQLOS 计划程序切换到抢先模式时发生，以便关闭备份介质。| 
|PREEMPTIVE_CLOSEBACKUPTAPE |在 SQLOS 计划程序切换到抢先模式时发生，以便关闭磁带备份设备。| 
|PREEMPTIVE_CLOSEBACKUPVDIDEVICE |在 SQLOS 计划程序切换到抢先模式时发生，以便关闭虚拟备份设备。| 
|PREEMPTIVE_CLUSAPI_CLUSTERRESOURCECONTROL |在 SQLOS 计划程序切换到抢先模式时发生，以便执行故障转移群集操作。| 
|PREEMPTIVE_COM_COCREATEINSTANCE |在 SQLOS 计划程序切换到抢先模式时发生，以便创建 COM 对象。| 
|PREEMPTIVE_COM_COGETCLASSOBJECT |仅限内部使用。| 
|PREEMPTIVE_COM_CREATEACCESSOR |仅限内部使用。| 
|PREEMPTIVE_COM_DELETEROWS |仅限内部使用。| 
|PREEMPTIVE_COM_GETCOMMANDTEXT |仅限内部使用。| 
|PREEMPTIVE_COM_GETDATA |仅限内部使用。| 
|PREEMPTIVE_COM_GETNEXTROWS |仅限内部使用。| 
|PREEMPTIVE_COM_GETRESULT |仅限内部使用。| 
|PREEMPTIVE_COM_GETROWSBYBOOKMARK |仅限内部使用。| 
|PREEMPTIVE_COM_LBFLUSH |仅限内部使用。| 
|PREEMPTIVE_COM_LBLOCKREGION |仅限内部使用。| 
|PREEMPTIVE_COM_LBREADAT |仅限内部使用。| 
|PREEMPTIVE_COM_LBSETSIZE |仅限内部使用。| 
|PREEMPTIVE_COM_LBSTAT |仅限内部使用。| 
|PREEMPTIVE_COM_LBUNLOCKREGION |仅限内部使用。| 
|PREEMPTIVE_COM_LBWRITEAT |仅限内部使用。| 
|PREEMPTIVE_COM_QUERYINTERFACE |仅限内部使用。| 
|PREEMPTIVE_COM_RELEASE |仅限内部使用。| 
|PREEMPTIVE_COM_RELEASEACCESSOR |仅限内部使用。| 
|PREEMPTIVE_COM_RELEASEROWS |仅限内部使用。| 
|PREEMPTIVE_COM_RELEASESESSION |仅限内部使用。| 
|PREEMPTIVE_COM_RESTARTPOSITION |仅限内部使用。| 
|PREEMPTIVE_COM_SEQSTRMREAD |仅限内部使用。| 
|PREEMPTIVE_COM_SEQSTRMREADANDWRITE |仅限内部使用。| 
|PREEMPTIVE_COM_SETDATAFAILURE |仅限内部使用。| 
|PREEMPTIVE_COM_SETPARAMETERINFO |仅限内部使用。| 
|PREEMPTIVE_COM_SETPARAMETERPROPERTIES |仅限内部使用。| 
|PREEMPTIVE_COM_STRMLOCKREGION |仅限内部使用。| 
|PREEMPTIVE_COM_STRMSEEKANDREAD |仅限内部使用。| 
|PREEMPTIVE_COM_STRMSEEKANDWRITE |仅限内部使用。| 
|PREEMPTIVE_COM_STRMSETSIZE |仅限内部使用。| 
|PREEMPTIVE_COM_STRMSTAT |仅限内部使用。| 
|PREEMPTIVE_COM_STRMUNLOCKREGION |仅限内部使用。| 
|PREEMPTIVE_CONSOLEWRITE |仅限内部使用。| 
|PREEMPTIVE_CREATEPARAM |仅限内部使用。| 
|PREEMPTIVE_DEBUG |仅限内部使用。| 
|PREEMPTIVE_DFSADDLINK |仅限内部使用。| 
|PREEMPTIVE_DFSLINKEXISTCHECK |仅限内部使用。| 
|PREEMPTIVE_DFSLINKHEALTHCHECK |仅限内部使用。| 
|PREEMPTIVE_DFSREMOVELINK |仅限内部使用。| 
|PREEMPTIVE_DFSREMOVEROOT |仅限内部使用。| 
|PREEMPTIVE_DFSROOTFOLDERCHECK |仅限内部使用。| 
|PREEMPTIVE_DFSROOTINIT |仅限内部使用。| 
|PREEMPTIVE_DFSROOTSHARECHECK |仅限内部使用。| 
|PREEMPTIVE_DTC_ABORT |仅限内部使用。| 
|PREEMPTIVE_DTC_ABORTREQUESTDONE |仅限内部使用。| 
|PREEMPTIVE_DTC_BEGINTRANSACTION |仅限内部使用。| 
|PREEMPTIVE_DTC_COMMITREQUESTDONE |仅限内部使用。| 
|PREEMPTIVE_DTC_ENLIST |仅限内部使用。| 
|PREEMPTIVE_DTC_PREPAREREQUESTDONE |仅限内部使用。| 
|PREEMPTIVE_FILESIZEGET |仅限内部使用。| 
|PREEMPTIVE_FSAOLEDB_ABORTTRANSACTION |仅限内部使用。| 
|PREEMPTIVE_FSAOLEDB_COMMITTRANSACTION |仅限内部使用。| 
|PREEMPTIVE_FSAOLEDB_STARTTRANSACTION |仅限内部使用。| 
|PREEMPTIVE_FSRECOVER_UNCONDITIONALUNDO |仅限内部使用。| 
|PREEMPTIVE_GETRMINFO |仅限内部使用。| 
|PREEMPTIVE_HADR_LEASE_MECHANISM |Always On 可用性组 Microsoft 支持部门诊断的租约管理器计划。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PREEMPTIVE_HTTP_EVENT_WAIT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PREEMPTIVE_HTTP_REQUEST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PREEMPTIVE_LOCKMONITOR |仅限内部使用。| 
|PREEMPTIVE_MSS_RELEASE |仅限内部使用。| 
|PREEMPTIVE_ODBCOPS |仅限内部使用。| 
|PREEMPTIVE_OLE_UNINIT |仅限内部使用。| 
|PREEMPTIVE_OLEDB_ABORTORCOMMITTRAN |仅限内部使用。| 
|PREEMPTIVE_OLEDB_ABORTTRAN |仅限内部使用。| 
|PREEMPTIVE_OLEDB_GETDATASOURCE |仅限内部使用。| 
|PREEMPTIVE_OLEDB_GETLITERALINFO |仅限内部使用。| 
|PREEMPTIVE_OLEDB_GETPROPERTIES |仅限内部使用。| 
|PREEMPTIVE_OLEDB_GETPROPERTYINFO |仅限内部使用。| 
|PREEMPTIVE_OLEDB_GETSCHEMALOCK |仅限内部使用。| 
|PREEMPTIVE_OLEDB_JOINTRANSACTION |仅限内部使用。| 
|PREEMPTIVE_OLEDB_RELEASE |仅限内部使用。| 
|PREEMPTIVE_OLEDB_SETPROPERTIES |仅限内部使用。| 
|PREEMPTIVE_OLEDBOPS |仅限内部使用。| 
|PREEMPTIVE_OS_ACCEPTSECURITYCONTEXT |仅限内部使用。| 
|PREEMPTIVE_OS_ACQUIRECREDENTIALSHANDLE |仅限内部使用。| 
|PREEMPTIVE_OS_AUTHENTICATIONOPS |仅限内部使用。| 
|PREEMPTIVE_OS_AUTHORIZATIONOPS |仅限内部使用。| 
|PREEMPTIVE_OS_AUTHZGETINFORMATIONFROMCONTEXT |仅限内部使用。| 
|PREEMPTIVE_OS_AUTHZINITIALIZECONTEXTFROMSID |仅限内部使用。| 
|PREEMPTIVE_OS_AUTHZINITIALIZERESOURCEMANAGER |仅限内部使用。| 
|PREEMPTIVE_OS_BACKUPREAD |仅限内部使用。| 
|PREEMPTIVE_OS_CLOSEHANDLE |仅限内部使用。| 
|PREEMPTIVE_OS_CLUSTEROPS |仅限内部使用。| 
|PREEMPTIVE_OS_COMOPS |仅限内部使用。| 
|PREEMPTIVE_OS_COMPLETEAUTHTOKEN |仅限内部使用。| 
|PREEMPTIVE_OS_COPYFILE |仅限内部使用。| 
|PREEMPTIVE_OS_CREATEDIRECTORY |仅限内部使用。| 
|PREEMPTIVE_OS_CREATEFILE |仅限内部使用。| 
|PREEMPTIVE_OS_CRYPTACQUIRECONTEXT |仅限内部使用。| 
|PREEMPTIVE_OS_CRYPTIMPORTKEY |仅限内部使用。| 
|PREEMPTIVE_OS_CRYPTOPS |仅限内部使用。| 
|PREEMPTIVE_OS_DECRYPTMESSAGE |仅限内部使用。| 
|PREEMPTIVE_OS_DELETEFILE |仅限内部使用。| 
|PREEMPTIVE_OS_DELETESECURITYCONTEXT |仅限内部使用。| 
|PREEMPTIVE_OS_DEVICEIOCONTROL |仅限内部使用。| 
|PREEMPTIVE_OS_DEVICEOPS |仅限内部使用。| 
|PREEMPTIVE_OS_DIRSVC_NETWORKOPS |仅限内部使用。| 
|PREEMPTIVE_OS_DISCONNECTNAMEDPIPE |仅限内部使用。| 
|PREEMPTIVE_OS_DOMAINSERVICESOPS |仅限内部使用。| 
|PREEMPTIVE_OS_DSGETDCNAME |仅限内部使用。| 
|PREEMPTIVE_OS_DTCOPS |仅限内部使用。| 
|PREEMPTIVE_OS_ENCRYPTMESSAGE |仅限内部使用。| 
|PREEMPTIVE_OS_FILEOPS |仅限内部使用。| 
|PREEMPTIVE_OS_FINDFILE |仅限内部使用。| 
|PREEMPTIVE_OS_FLUSHFILEBUFFERS |仅限内部使用。| 
|PREEMPTIVE_OS_FORMATMESSAGE |仅限内部使用。| 
|PREEMPTIVE_OS_FREECREDENTIALSHANDLE |仅限内部使用。| 
|PREEMPTIVE_OS_FREELIBRARY |仅限内部使用。| 
|PREEMPTIVE_OS_GENERICOPS |仅限内部使用。| 
|PREEMPTIVE_OS_GETADDRINFO |仅限内部使用。| 
|PREEMPTIVE_OS_GETCOMPRESSEDFILESIZE |仅限内部使用。| 
|PREEMPTIVE_OS_GETDISKFREESPACE |仅限内部使用。| 
|PREEMPTIVE_OS_GETFILEATTRIBUTES |仅限内部使用。| 
|PREEMPTIVE_OS_GETFILESIZE |仅限内部使用。| 
|PREEMPTIVE_OS_GETFINALFILEPATHBYHANDLE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PREEMPTIVE_OS_GETLONGPATHNAME |仅限内部使用。| 
|PREEMPTIVE_OS_GETPROCADDRESS |仅限内部使用。| 
|PREEMPTIVE_OS_GETVOLUMENAMEFORVOLUMEMOUNTPOINT |仅限内部使用。| 
|PREEMPTIVE_OS_GETVOLUMEPATHNAME |仅限内部使用。| 
|PREEMPTIVE_OS_INITIALIZESECURITYCONTEXT |仅限内部使用。| 
|PREEMPTIVE_OS_LIBRARYOPS |仅限内部使用。| 
|PREEMPTIVE_OS_LOADLIBRARY |仅限内部使用。| 
|PREEMPTIVE_OS_LOGONUSER |仅限内部使用。| 
|PREEMPTIVE_OS_LOOKUPACCOUNTSID |仅限内部使用。| 
|PREEMPTIVE_OS_MESSAGEQUEUEOPS |仅限内部使用。| 
|PREEMPTIVE_OS_MOVEFILE |仅限内部使用。| 
|PREEMPTIVE_OS_NETGROUPGETUSERS |仅限内部使用。| 
|PREEMPTIVE_OS_NETLOCALGROUPGETMEMBERS |仅限内部使用。| 
|PREEMPTIVE_OS_NETUSERGETGROUPS |仅限内部使用。| 
|PREEMPTIVE_OS_NETUSERGETLOCALGROUPS |仅限内部使用。| 
|PREEMPTIVE_OS_NETUSERMODALSGET |仅限内部使用。| 
|PREEMPTIVE_OS_NETVALIDATEPASSWORDPOLICY |仅限内部使用。| 
|PREEMPTIVE_OS_NETVALIDATEPASSWORDPOLICYFREE |仅限内部使用。| 
|PREEMPTIVE_OS_OPENDIRECTORY |仅限内部使用。| 
|PREEMPTIVE_OS_PDH_WMI_INIT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PREEMPTIVE_OS_PIPEOPS |仅限内部使用。| 
|PREEMPTIVE_OS_PROCESSOPS |仅限内部使用。| 
|PREEMPTIVE_OS_QUERYCONTEXTATTRIBUTES |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PREEMPTIVE_OS_QUERYREGISTRY |仅限内部使用。| 
|PREEMPTIVE_OS_QUERYSECURITYCONTEXTTOKEN |仅限内部使用。| 
|PREEMPTIVE_OS_REMOVEDIRECTORY |仅限内部使用。| 
|PREEMPTIVE_OS_REPORTEVENT |仅限内部使用。| 
|PREEMPTIVE_OS_REVERTTOSELF |仅限内部使用。| 
|PREEMPTIVE_OS_RSFXDEVICEOPS |仅限内部使用。| 
|PREEMPTIVE_OS_SECURITYOPS |仅限内部使用。| 
|PREEMPTIVE_OS_SERVICEOPS |仅限内部使用。| 
|PREEMPTIVE_OS_SETENDOFFILE |仅限内部使用。| 
|PREEMPTIVE_OS_SETFILEPOINTER |仅限内部使用。| 
|PREEMPTIVE_OS_SETFILEVALIDDATA |仅限内部使用。| 
|PREEMPTIVE_OS_SETNAMEDSECURITYINFO |仅限内部使用。| 
|PREEMPTIVE_OS_SQLCLROPS |仅限内部使用。| 
|PREEMPTIVE_OS_SQMLAUNCH |仅限内部使用。 <br /><br /> **适用范围**： [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] 到 [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]。 |  
|PREEMPTIVE_OS_VERIFYSIGNATURE |仅限内部使用。| 
|PREEMPTIVE_OS_VERIFYTRUST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PREEMPTIVE_OS_VSSOPS |仅限内部使用。| 
|PREEMPTIVE_OS_WAITFORSINGLEOBJECT |仅限内部使用。| 
|PREEMPTIVE_OS_WINSOCKOPS |仅限内部使用。| 
|PREEMPTIVE_OS_WRITEFILE |仅限内部使用。| 
|PREEMPTIVE_OS_WRITEFILEGATHER |仅限内部使用。| 
|PREEMPTIVE_OS_WSASETLASTERROR |仅限内部使用。| 
|PREEMPTIVE_REENLIST |仅限内部使用。| 
|PREEMPTIVE_RESIZELOG |仅限内部使用。| 
|PREEMPTIVE_ROLLFORWARDREDO |仅限内部使用。| 
|PREEMPTIVE_ROLLFORWARDUNDO |仅限内部使用。| 
|PREEMPTIVE_SB_STOPENDPOINT |仅限内部使用。| 
|PREEMPTIVE_SERVER_STARTUP |仅限内部使用。| 
|PREEMPTIVE_SETRMINFO |仅限内部使用。| 
|PREEMPTIVE_SHAREDMEM_GETDATA |仅限内部使用。| 
|PREEMPTIVE_SNIOPEN |仅限内部使用。| 
|PREEMPTIVE_SOSHOST |仅限内部使用。| 
|PREEMPTIVE_SOSTESTING |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|PREEMPTIVE_SP_SERVER_DIAGNOSTICS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PREEMPTIVE_STARTRM |仅限内部使用。| 
|PREEMPTIVE_STREAMFCB_CHECKPOINT |仅限内部使用。| 
|PREEMPTIVE_STREAMFCB_RECOVER |仅限内部使用。| 
|PREEMPTIVE_STRESSDRIVER |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|PREEMPTIVE_TESTING |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|PREEMPTIVE_TRANSIMPORT |仅限内部使用。| 
|PREEMPTIVE_UNMARSHALPROPAGATIONTOKEN |仅限内部使用。| 
|PREEMPTIVE_VSS_CREATESNAPSHOT |仅限内部使用。| 
|PREEMPTIVE_VSS_CREATEVOLUMESNAPSHOT |仅限内部使用。| 
|PREEMPTIVE_XE_CALLBACKEXECUTE |仅限内部使用。| 
|PREEMPTIVE_XE_CX_FILE_OPEN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|PREEMPTIVE_XE_CX_HTTP_CALL |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|PREEMPTIVE_XE_DISPATCHER |仅限内部使用。| 
|PREEMPTIVE_XE_ENGINEINIT |仅限内部使用。| 
|PREEMPTIVE_XE_GETTARGETSTATE |仅限内部使用。| 
|PREEMPTIVE_XE_SESSIONCOMMIT |仅限内部使用。| 
|PREEMPTIVE_XE_TARGETFINALIZE |仅限内部使用。| 
|PREEMPTIVE_XE_TARGETINIT |仅限内部使用。| 
|PREEMPTIVE_XE_TIMERRUN |仅限内部使用。| 
|PREEMPTIVE_XETESTING |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|PRINT_ROLLBACK_PROGRESS |用于等待用户进程在已通过 ALTER DATABASE 终止子句完成转换的数据库中结束。 有关详细信息，请参阅 ALTER DATABASE (Transact-SQL)。| 
|PRU_ROLLBACK_DEFERRED |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_ALL_COMPONENTS_INITIALIZED |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_COOP_SCAN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_DIRECTLOGCONSUMER_GETNEXT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PWAIT_EVENT_SESSION_INIT_MUTEX |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_FABRIC_REPLICA_CONTROLLER_DATA_LOSS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PWAIT_HADR_ACTION_COMPLETED |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_HADR_CHANGE_NOTIFIER_TERMINATION_SYNC |在后台任务正在等待终止接收（通过轮询）Windows Server 故障转移群集通知的后台任务时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_HADR_CLUSTER_INTEGRATION |追加、替换和/或删除操作正在等待获取 Always On 内部列表上的写入锁 (例如网络、网络地址或可用性组侦听器) 列表。 仅供内部使用， <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_HADR_FAILOVER_COMPLETED |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_HADR_JOIN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|PWAIT_HADR_OFFLINE_COMPLETED |Always On drop 可用性组操作在销毁 Windows Server 故障转移群集对象之前等待目标可用性组脱机。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_HADR_ONLINE_COMPLETED |Always On 创建或故障转移可用性组操作正在等待目标可用性组联机。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_HADR_POST_ONLINE_COMPLETED |Always On drop 可用性组操作正在等待作为之前命令的一部分计划的任何后台任务的终止。 例如，可能有正在将可用性数据库转换为主要角色的后台任务。 DROP AVAILABILITY GROUP DDL 必须等待此后台任务终止，以免出现争用情况。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_HADR_SERVER_READY_CONNECTIONS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_HADR_WORKITEM_COMPLETED |正在等待异步工作任务完成，这是线程执行的内部等待。 这是预期的等待，用于 CSS。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_HADRSIM |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|PWAIT_LOG_CONSOLIDATION_IO |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|PWAIT_LOG_CONSOLIDATION_POLL |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|PWAIT_MD_LOGIN_STATS |在登录统计信息的元数据内部同步期间发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_MD_RELATION_CACHE |在表或索引的元数据内部同步期间发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_MD_SERVER_CACHE |在链接服务器的元数据内部同步期间发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_MD_UPGRADE_CONFIG |在升级服务器范围的配置时进行内部同步期间发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_PREEMPTIVE_APP_USAGE_TIMER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|PWAIT_PREEMPTIVE_AUDIT_ACCESS_WINDOWSLOG |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_QRY_BPMEMORY |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_REPLICA_ONLINE_INIT_MUTEX |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_RESOURCE_SEMAPHORE_FT_PARALLEL_QUERY_SYNC |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|PWAIT_SBS_FILE_OPERATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|PWAIT_XTP_FSSTORAGE_MAINTENANCE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|PWAIT_XTP_HOST_STORAGE_WAIT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_ASYNC_CHECK_CONSISTENCY_TASK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_ASYNC_PERSIST_TASK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_ASYNC_PERSIST_TASK_START |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_ASYNC_QUEUE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|QDS_BCKG_TASK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_BLOOM_FILTER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|QDS_CLEANUP_STALE_QUERIES_TASK_MAIN_LOOP_SLEEP |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_CTXS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_DB_DISK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_DYN_VECTOR |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_EXCLUSIVE_ACCESS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|QDS_HOST_INIT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|QDS_LOADDB |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_PERSIST_TASK_MAIN_LOOP_SLEEP |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_QDS_CAPTURE_INIT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|QDS_SHUTDOWN_QUEUE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_STMT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_STMT_DISK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_TASK_SHUTDOWN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QDS_TASK_START |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QE_WARN_LIST_SYNC |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|QPJOB_KILL |指示异步统计信息自动更新在开始运行时通过调用 KILL 命令而取消。 终止线程处于挂起状态，等待它开始侦听 KILL 命令。 正常情况下，该值不到一秒钟。| 
|QPJOB_WAITFOR_ABORT |指示异步统计信息自动更新在运行时通过调用 KILL 命令而取消。 目前更新已完成，但是在终止线程消息协调完成之前一直于挂起状态。 这是一个普通而少见的状态，应当非常短暂。 正常情况下，该值不到一秒钟。| 
|QRY_MEM_GRANT_INFO_MUTEX |当查询执行内存管理尝试控制对静态授予信息列表的访问时出现。 该状态列出当前已批准的内存请求以及正在等待的内存请求的有关信息。 该状态是一个简单的访问控制状态。 该状态始终不应当等待较长的时间。 如果未释放互斥体，则所有占用内存的新查询都将停止响应。| 
|QRY_PARALLEL_THREAD_MUTEX |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|QRY_PROFILE_LIST_MUTEX |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|QUERY_ERRHDL_SERVICE_DONE |标识为仅供参考。 不支持。 <br /><br /> **适用** 于： [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] 仅限。 |  
|QUERY_WAIT_ERRHDL_SERVICE |标识为仅供参考。  不支持。 <br /><br /> **适用** 于： [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] 仅限。  |  
|QUERY_EXECUTION_INDEX_SORT_EVENT_OPEN |当脱机创建索引生成以并行方式运行，并且正在排序的不同工作线程同步访问排序文件时出现。| 
|QUERY_NOTIFICATION_MGR_MUTEX |在查询通知管理器中的垃圾收集队列同步期间出现。| 
|QUERY_NOTIFICATION_SUBSCRIPTION_MUTEX |在查询通知中事务的状态同步期间出现。| 
|QUERY_NOTIFICATION_TABLE_MGR_MUTEX |在查询通知管理器中的内部同步期间出现。| 
|QUERY_NOTIFICATION_UNITTEST_MUTEX |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|QUERY_OPTIMIZER_PRINT_MUTEX |在查询优化器诊断信息输出生成的同步期间出现。 仅当在 Microsoft 产品支持方向下启用了诊断设置时，才会出现此等待类型。| 
|QUERY_TASK_ENQUEUE_MUTEX |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|QUERY_TRACEOUT |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|RBIO_WAIT_VLF |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|RBIO_RG_STORAGE |当超大规模数据库计算节点由于页面服务器 () 的延迟日志消耗而受到限制时发生。 <br /><br /> **适用** 于： Azure SQL 数据库超大规模。|
|RBIO_RG_DESTAGE |由于长期日志存储延迟日志消耗而正在限制超大规模数据库计算节点时发生。 <br /><br /> **适用** 于： Azure SQL 数据库超大规模。|
|RBIO_RG_REPLICA |当超大规模数据库计算节点由于) 的可读辅助副本节点 (延迟的日志消耗而受到限制时发生。 <br /><br /> **适用** 于： Azure SQL 数据库超大规模。|
|RBIO_RG_LOCALDESTAGE |由于日志服务延迟日志消耗而正在限制超大规模数据库计算节点时发生。 <br /><br /> **适用** 于： Azure SQL 数据库超大规模。|
|RECOVER_CHANGEDB |在备用数据库中同步数据库状态期间出现。| 
|RECOVERY_MGR_LOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|REDO_THREAD_PENDING_WORK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|REDO_THREAD_SYNC |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|REMOTE_BLOCK_IO |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|REMOTE_DATA_ARCHIVE_MIGRATION_DMV |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|REMOTE_DATA_ARCHIVE_SCHEMA_DMV |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|REMOTE_DATA_ARCHIVE_SCHEMA_TASK_QUEUE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|REPL_CACHE_ACCESS |在同步复制项目缓存的期间出现。 在这些等待期间，复制日志读取器将停止，已发布表中的数据定义语言 (DDL) 语句也将被阻止。| 
|REPL_HISTORYCACHE_ACCESS |仅限内部使用。| 
|REPL_SCHEMA_ACCESS |在同步复制架构版本信息的期间出现。 该状态在下列情况下存在：针对复制对象执行 DDL 语句时，以及日志读取器根据 DDL 出现次数生成或使用版本控制架构时。 如果在具有事务复制的单个发布服务器上有很多已发布的数据库并且已发布的数据库非常活跃，则可以在此等待类型上查看争用情况。| 
|REPL_TRANFSINFO_ACCESS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|REPL_TRANHASHTABLE_ACCESS |仅限内部使用。| 
|REPL_TRANTEXTINFO_ACCESS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|REPLICA_WRITES |在任务等待将页写入数据库快照或 DBCC 副本的操作完成时出现。| 
|REQUEST_DISPENSER_PAUSE |在任务等待所有未完成的 I/O 完成时出现，以便可以为快照备份冻结文件的 I/O。| 
|REQUEST_FOR_DEADLOCK_SEARCH |在死锁监视器等待开始下一次死锁搜索时出现。 在两次死锁检测之间可能出现该等待，长时间等待此资源并不指示出现问题。| 
|RESERVED_MEMORY_ALLOCATION_EXT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|RESMGR_THROTTLED |在有新请求传入并且基于 GROUP_MAX_REQUESTS 设置而中止时出现。| 
|RESOURCE_GOVERNOR_IDLE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|RESOURCE_QUEUE |在同步不同的内部资源队列期间出现。| 
|RESOURCE_SEMAPHORE |当由于存在其他并发查询而无法立即批准查询内存请求时出现。 等待时间较长或等待次数较多可能指示并发查询的数量过多或内存请求的数量过多。| 
|RESOURCE_SEMAPHORE_MUTEX |在查询等待其预留线程的请求完成时出现。 它也在同步查询编译和内存授予请求时出现。| 
|RESOURCE_SEMAPHORE_QUERY_COMPILE |在并发查询编译的数量达到中止限制时出现。 高等待时间和等待时间可能表示过多的编译、重新编译或 uncacheable 计划。| 
|RESOURCE_SEMAPHORE_SMALL_QUERY |当由于存在其他并发查询而无法立即批准较小查询的内存请求时出现。 等待时间不应超过几秒钟，因为如果服务器无法在几秒钟内给予请求的内存，则会将请求传输到主查询内存池中。 等待时间较长可能指示当主内存池被等待的查询阻塞时并发小查询的数量过多。 <br /><br /> **适用** 于： [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] 仅限。 |  
|RESTORE_FILEHANDLECACHE_ENTRYLOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|RESTORE_FILEHANDLECACHE_LOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|RG_RECONFIG |仅限内部使用。| 
|ROWGROUP_OP_STATS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|ROWGROUP_VERSION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|RTDATA_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|SATELLITE_CARGO |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|SATELLITE_SERVICE_SETUP |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|SATELLITE_TASK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|SBS_DISPATCH |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|SBS_RECEIVE_TRANSPORT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|SBS_TRANSPORT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|SCAN_CHAR_HASH_ARRAY_INITIALIZATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SEC_DROP_TEMP_KEY |在尝试删除临时安全密钥失败之后并在重试之前出现。| 
|SECURITY_CNG_PROVIDER_MUTEX |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|SECURITY_CRYPTO_CONTEXT_MUTEX |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SECURITY_DBE_STATE_MUTEX |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|SECURITY_KEYRING_RWLOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SECURITY_MUTEX |当等待互斥体时出现，这些互斥体控制对可扩展的密钥管理 (EKM) 加密提供程序的全局列表以及 EKM 会话的会话作用域列表的访问。| 
|SECURITY_RULETABLE_MUTEX |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SEMPLAT_DSI_BUILD |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SEQUENCE_GENERATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SEQUENTIAL_GUID |当正在获取新的连续 GUID 时出现。| 
|SERVER_IDLE_CHECK |当资源监视器正在尝试将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例声明为空闲或正在尝试唤醒时，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例空闲状态的同步期间出现。| 
|SERVER_RECONFIGURE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SESSION_WAIT_STATS_CHILDREN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|SHARED_DELTASTORE_CREATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|SHUTDOWN |在关闭语句等待活动连接退出时出现。| 
|SLEEP_BPOOL_FLUSH |当检查点为了避免磁盘子系统泛滥而中止新 I/O 的发布时出现。| 
|SLEEP_BUFFERPOOL_HELPLW |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|SLEEP_DBSTARTUP |在等待所有数据库恢复时数据库的启动期间出现。| 
|SLEEP_DCOMSTARTUP |通常在等待 DCOM 初始化完成时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的启动期间出现。| 
|SLEEP_MASTERDBREADY |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SLEEP_MASTERMDREADY |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SLEEP_MASTERUPGRADED |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SLEEP_MEMORYPOOL_ALLOCATEPAGES |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|SLEEP_MSDBSTARTUP |在 SQL 跟踪等待 msdb 数据库完成启动时出现。| 
|SLEEP_RETRY_VIRTUALALLOC |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|SLEEP_SYSTEMTASK |在等待 tempdb 完成启动时后台任务的启动期间出现。| 
|SLEEP_TASK |当任务在等待一般事件出现期间睡眠时出现。| 
|SLEEP_TEMPDBSTARTUP |在任务等待 tempdb 完成启动时出现。| 
|SLEEP_WORKSPACE_ALLOCATEPAGE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|SLO_UPDATE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|SMSYNC |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|SNI_CONN_DUP |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|SNI_CRITICAL_SECTION |在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 网络组件中进行内部同步期间出现。| 
|SNI_HTTP_WAITFOR_0_DISCON |在等待未完成的 HTTP 连接退出的过程中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的关闭期间出现。| 
|SNI_LISTENER_ACCESS |当等待非一致性内存访问 (NUMA) 节点更新状态更改时出现。 已序列化对状态更改的访问。| 
|SNI_TASK_COMPLETION |当在 NUMA 节点状态更改期间等待所有任务完成时出现。| 
|SNI_WRITE_ASYNC |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|SOAP_READ |在等待 HTTP 网络读取完成时出现。| 
|SOAP_WRITE |在等待 HTTP 网络写入完成时出现。| 
|SOCKETDUPLICATEQUEUE_CLEANUP |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|SOS_CALLBACK_REMOVAL |在为了删除回调而对回调列表执行同步期间出现。 服务器初始化完成之后，此计数器可能不会更改。| 
|SOS_DISPATCHER_MUTEX |在调度程序池进行内部同步期间出现。 包括调整该池时。| 
|SOS_LOCALALLOCATORLIST |在 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 内存管理器中进行内部同步期间出现。 <br /><br /> **适用** 于： [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] 仅限。 |  
|SOS_MEMORY_TOPLEVELBLOCKALLOCATOR |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SOS_MEMORY_USAGE_ADJUSTMENT |在池之间调整内存使用情况时出现。| 
|SOS_OBJECT_STORE_DESTROY_MUTEX |当破坏池中的对象时在内存池中进行内部同步期间出现。| 
|SOS_PHYS_PAGE_CACHE |考虑到线程等待获得互斥体的时间，它必须在分配物理页前或将这些页返回操作系统前获得。 仅当实例使用 AWE 内存时，才会显示此类型的等待 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SOS_PROCESS_AFFINITY_MUTEX |在同步访问进程关联设置期间出现。| 
|SOS_RESERVEDMEMBLOCKLIST |在内存管理器中进行内部同步期间出现 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 <br /><br /> **适用** 于： [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] 仅限。 |  
|SOS_SCHEDULER_YIELD |在任务自愿为要执行的其他任务生成计划程序时出现。 在该等待期间任务正在等待其量程更新。| 
|SOS_SMALL_PAGE_ALLOC |在分配和释放由某些内存对象管理的内存时出现。| 
|SOS_STACKSTORE_INIT_MUTEX |在内部存储初始化同步期间出现。| 
|SOS_SYNC_TASK_ENQUEUE_EVENT |在任务以同步方式启动时出现。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的大多数任务都以同步方式启动，在此方式中控制权在任务请求放置在工作队列之后立即返回到启动器。| 
|SOS_VIRTUALMEMORY_LOW |在内存分配等待资源管理器释放虚拟内存时发生。| 
|SOSHOST_EVENT |当宿主组件（如 CLR）在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件同步对象中等待时出现。| 
|SOSHOST_INTERNAL |在宿主组件（如 CLR）使用的内存管理器回调同步期间出现。| 
|SOSHOST_MUTEX |当宿主组件（如 CLR）在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 互斥体同步对象中等待时出现。| 
|SOSHOST_RWLOCK |当宿主组件（如 CLR）在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 读取器编写器同步对象中等待时出现。| 
|SOSHOST_SEMAPHORE |当宿主组件（如 CLR）在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 信号量同步对象中等待时出现。| 
|SOSHOST_SLEEP |当宿主任务在等待一般事件出现期间睡眠时出现。 宿主任务由宿主组件（如 CLR）使用。| 
|SOSHOST_TRACELOCK |在同步访问跟踪流期间出现。| 
|SOSHOST_WAITFORDONE |在宿主组件（如 CLR）等待任务完成时出现。| 
|SP_PREEMPTIVE_SERVER_DIAGNOSTICS_SLEEP |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SP_SERVER_DIAGNOSTICS_BUFFER_ACCESS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SP_SERVER_DIAGNOSTICS_INIT_MUTEX |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SP_SERVER_DIAGNOSTICS_SLEEP |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SQLCLR_APPDOMAIN |在 CLR 等待应用程序域完成启动时出现。| 
|SQLCLR_ASSEMBLY |在等待访问 appdomain 中已加载的程序集列表时出现。| 
|SQLCLR_DEADLOCK_DETECTION |在 CLR 等待死锁检测完成时出现。| 
|SQLCLR_QUANTUM_PUNISHMENT |在 CLR 任务由于已经超过了其执行量程而中止时出现。 此中止已完成，以便减小此大量消耗资源的任务对其他任务的影响。| 
|SQLSORT_NORMMUTEX |在初始化内部排序结构时进行内部同步期间出现。| 
|SQLSORT_SORTMUTEX |在初始化内部排序结构时进行内部同步期间出现。| 
|SQLTRACE_BUFFER_FLUSH |当某任务正在等待后台任务将跟踪缓冲区每隔四秒刷新到磁盘时出现。 <br /><br /> **适用** 于： [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] 仅限。 |  
|SQLTRACE_FILE_BUFFER |在文件跟踪过程中同步跟踪缓冲区期间出现。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SQLTRACE_FILE_READ_IO_COMPLETION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SQLTRACE_FILE_WRITE_IO_COMPLETION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SQLTRACE_INCREMENTAL_FLUSH_SLEEP |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SQLTRACE_LOCK |仅限内部使用。 <br /><br /> **适用** 于： [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] 仅限。 |  
|SQLTRACE_PENDING_BUFFER_WRITERS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|SQLTRACE_SHUTDOWN |在跟踪关闭等待未完成的跟踪事件完成时出现。| 
|SQLTRACE_WAIT_ENTRIES |在 SQL 跟踪事件队列等待数据包到达队列时出现。| 
|SRVPROC_SHUTDOWN |在关闭进程等待内部资源释放以完全关闭时出现。| 
|STARTUP_DEPENDENCY_MANAGER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|TDS_BANDWIDTH_STATE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|TDS_INIT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|TDS_PROXY_CONTAINER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|TEMPOBJ |在临时对象删除同步时出现。 该等待很少出现，仅在任务已请求 temp 表的独占访问删除时出现。| 
|TEMPORAL_BACKGROUND_PROCEED_CLEANUP |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|TERMINATE_LISTENER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|THREADPOOL |当某任务正在等待工作线程运行时出现。 这可能指示最大工作线程数设置过低，或批处理执行时间过长，从而减少可满足其他批处理的工作线程数。| 
|TIMEPRIV_TIMEPERIOD |在扩展事件计时器进行内部同步期间出现。| 
|TRACE_EVTNOTIF |仅限内部使用。| 
|TRACEWRITE |当 SQL 跟踪行集跟踪提供程序等待可用缓冲区或可处理事件的缓冲区时出现。| 
|TRAN_MARKLATCH_DT |在等待事务标记闩锁中的破坏模式闩锁时出现。 事务标记闩锁用于同步提交与标记的事务。| 
|TRAN_MARKLATCH_EX |在等待标记事务中的排他模式闩锁时出现。 事务标记闩锁用于同步提交与标记的事务。| 
|TRAN_MARKLATCH_KP |在等待标记事务中的保持模式闩锁时出现。 事务标记闩锁用于同步提交与标记的事务。| 
|TRAN_MARKLATCH_NL |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|TRAN_MARKLATCH_SH |在等待标记事务中的共享模式闩锁时出现。 事务标记闩锁用于同步提交与标记的事务。| 
|TRAN_MARKLATCH_UP |在等待标记事务中的更新模式闩锁时出现。 事务标记闩锁用于同步提交与标记的事务。| 
|TRANSACTION_MUTEX |在同步多个批处理访问事务期间出现。| 
|UCS_ENDPOINT_CHANGE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|UCS_MANAGER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|UCS_MEMORY_NOTIFICATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|UCS_SESSION_REGISTRATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|UCS_TRANSPORT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|UCS_TRANSPORT_STREAM_CHANGE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|UTIL_PAGE_ALLOC |在内存不足期间事务日志扫描等待可用内存时出现。| 
|VDI_CLIENT_COMPLETECOMMAND |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|VDI_CLIENT_GETCOMMAND |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|VDI_CLIENT_OPERATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|VDI_CLIENT_OTHER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|VERSIONING_COMMITTING |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|VIA_ACCEPT |当在启动过程中完成虚拟接口适配器 (VIA) 提供程序连接时出现。| 
|VIEW_DEFINITION_MUTEX |在同步访问已缓存的视图定义期间出现。| 
|WAIT_FOR_RESULTS |在等待查询通知触发时出现。| 
|WAIT_ON_SYNC_STATISTICS_REFRESH |在等待同步统计信息更新完成后，查询编译和执行才能恢复之前发生。<br /><br /> **适用对象**：自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起|
|WAIT_SCRIPTDEPLOYMENT_REQUEST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_SCRIPTDEPLOYMENT_WORKER |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_XLOGREAD_SIGNAL |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|WAIT_XTP_ASYNC_TX_COMPLETION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_XTP_CKPT_AGENT_WAKEUP |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_XTP_CKPT_CLOSE |当等待检查点完成时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_XTP_CKPT_ENABLED |当检查点已禁用并且正在等待启用检查点时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_XTP_CKPT_STATE_LOCK |当同步对检查点状态的检查时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_XTP_COMPILE_WAIT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|WAIT_XTP_GUEST |当数据库内存分配器需要停止接收内存不足通知时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|WAIT_XTP_HOST_WAIT |当数据库引擎触发等待并且主机实现等待时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_XTP_OFFLINE_CKPT_BEFORE_REDO |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_XTP_OFFLINE_CKPT_LOG_IO |当脱机检查点等待日志读取 IO 完成时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_XTP_OFFLINE_CKPT_NEW_LOG |当脱机检查点等待新日志记录扫描时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_XTP_PROCEDURE_ENTRY |当删除过程等待该过程的所有当前执行完成时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_XTP_RECOVERY |当数据库恢复正在等待恢复内存优化对象的操作完成时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAIT_XTP_SERIAL_RECOVERY |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|WAIT_XTP_SWITCH_TO_INACTIVE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|WAIT_XTP_TASK_SHUTDOWN |当等待内存中 OLTP 进程完成时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|WAIT_XTP_TRAN_DEPENDENCY |当等待事务依赖关系时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WAITFOR |作为 WAITFOR Transact-sql 语句的结果发生。 等待持续时间由此语句的参数确定。 它是用户启动的等待。| 
|WAITFOR_PER_QUEUE |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|WAITFOR_TASKSHUTDOWN |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|WAITSTAT_MUTEX |在同步访问用于填充 sys.dm_os_wait_stats 的统计信息集期间出现。| 
|WCC |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|WINDOW_AGGREGATES_MULTIPASS |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|WINFAB_API_CALL |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WINFAB_REPLICA_BUILD_OPERATION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|WINFAB_REPORT_FAULT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|WORKTBL_DROP |在删除出现故障的工作表之后，重试之前的暂停期间出现。| 
|WRITE_COMPLETION |当正在进行写操作时出现。| 
|WRITELOG |等待日志刷新完成时出现。 导致日志刷新的常见操作是检查点和事务提交。| 
|XACT_OWN_TRANSACTION |在等待获取事务的所有权时出现。| 
|XACT_RECLAIM_SESSION |在等待会话的当前所有者释放会话的所有权时出现。| 
|XACTLOCKINFO |在同步访问事务锁列表期间出现。 除事务本身之外，在页拆分过程中死锁检测和锁迁移等操作也可访问锁列表。| 
|XACTWORKSPACE_MUTEX |在同步事务中的脱离以及事务登记成员之间的数据库锁数时出现。| 
|XDB_CONN_DUP_HASH |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|XDES_HISTORY |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|XDES_OUT_OF_ORDER_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|XDES_SNAPSHOT |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|XDESTSVERMGR |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|XE_BUFFERMGR_ALLPROCESSED_EVENT |在扩展事件会话缓冲区刷新到目标时发生。 此等待在后台线程上发生。| 
|XE_BUFFERMGR_FREEBUF_EVENT |当下列任一条件成立时发生：| 
|XE_CALLBACK_LIST |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|XE_CX_FILE_READ |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|XE_DISPATCHER_CONFIG_SESSION_LIST |在使用异步目标的扩展事件会话启动或停止时发生。 此等待表明发生了以下某一情况：| 
|XE_DISPATCHER_JOIN |在用于扩展事件会话的后台线程终止时发生。| 
|XE_DISPATCHER_WAIT |在用于扩展事件会话的后台线程等待事件缓冲区进行处理时发生。| 
|XE_FILE_TARGET_TVF |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|XE_LIVE_TARGET_TVF |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。| 
|XE_MODULEMGR_SYNC |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|XE_OLS_LOCK |标识为仅供参考。 不支持。 不保证以后的兼容性。| 
|XE_PACKAGE_LOCK_BACKOFF |标识为仅供参考。 不支持。 <br /><br /> **适用** 于： [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] 仅限。 |  
|XE_SERVICES_EVENTMANUAL |仅限内部使用。| 
|XE_SERVICES_MUTEX |仅限内部使用。| 
|XE_SERVICES_RWLOCK |仅限内部使用。| 
|XE_SESSION_CREATE_SYNC |仅限内部使用。| 
|XE_SESSION_FLUSH |仅限内部使用。| 
|XE_SESSION_SYNC |仅限内部使用。| 
|XE_STM_CREATE |仅限内部使用。| 
|XE_TIMER_EVENT |仅限内部使用。| 
|XE_TIMER_MUTEX |仅限内部使用。| 
|XE_TIMER_TASK_DONE |仅限内部使用。| 
|XIO_CREDENTIAL_MGR_RWLOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|XIO_CREDENTIAL_RWLOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|XIO_EDS_MGR_RWLOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|XIO_EDS_RWLOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|XIO_IOSTATS_BLOBLIST_RWLOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|XIO_IOSTATS_FCBLIST_RWLOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 及更高版本。| 
|XIO_LEASE_RENEW_MGR_RWLOCK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|XTP_HOST_DB_COLLECTION |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|XTP_HOST_LOG_ACTIVITY |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|XTP_HOST_PARALLEL_RECOVERY |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|XTP_PREEMPTIVE_TASK |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|XTP_TRUNCATION_LSN |仅限内部使用。 <br /><br /> **适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本。| 
|XTPPROC_CACHE_ACCESS |当访问所有本机编译的存储过程缓存对象时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。| 
|XTPPROC_PARTITIONED_STACK_CREATE |当为给定的过程按 NUMA 节点分配本机编译的存储过程缓存结构（必须为单线程）时发生。 <br /><br /> **适用于**：[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] 及更高版本。|

  
 以下 XEvents 与分区 **切换** 和联机索引重新生成相关。 有关语法的信息，请参阅 [ALTER TABLE &#40;transact-sql&#41;](../../t-sql/statements/alter-table-transact-sql.md) 和 [Alter INDEX &#40;transact-sql&#41;](../../t-sql/statements/alter-index-transact-sql.md)。  
  
-   lock_request_priority_state  
  
-   process_killed_by_abort_blockers  
  
-   ddl_with_wait_at_low_priority  
  
 有关锁兼容性矩阵，请参阅 [&#40;transact-sql&#41;sys.dm_tran_locks ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
    
 [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_exec_session_wait_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-session-wait-stats-transact-sql.md)   
 [Azure SQL Database &#40;sys.dm_db_wait_stats&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database.md)  
  
