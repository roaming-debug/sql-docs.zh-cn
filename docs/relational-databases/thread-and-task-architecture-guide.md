---
title: 线程和任务体系结构指南 | Microsoft Docs
description: 了解 SQL Server 中的线程和任务体系结构，包括任务计划、热添加 CPU 以及使用超过 64 个 CPU 的计算机的最佳做法。
ms.custom: ''
ms.date: 09/23/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- guide, thread and task architecture
- thread and task architecture guide
- task scheduling
- working threads
- Large Deficit First scheduling
- LDF scheduling
- scheduling, SQL Server
- tasks, SQL Server
- threads, SQL Server
- quantum, SQL Server
ms.assetid: 925b42e0-c5ea-4829-8ece-a53c6cddad3b
author: pmasl
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3eb9fd5897029fedb0e53378ecbe5f6c12f635ef
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237654"
---
# <a name="thread-and-task-architecture-guide"></a>线程和任务体系结构指南
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]

## <a name="operating-system-task-scheduling"></a>操作系统任务计划
线程是操作系统可以执行的最小处理单位，借助线程可以将应用程序逻辑分为多个并行执行路径。 当复杂应用程序包含可同时执行的多个任务时，线程非常有用。 

操作系统执行应用程序实例时，它将创建一个单元（称为进程）来管理该实例。 此进程包含一个执行线程。 它是由应用程序代码执行的一系列编程指令。 例如，如果一个简单应用程序具有一组可串行执行的指令，则会将该组指令作为单个任务处理，并且整个应用程序只有一个执行路径（或线程）。 更复杂的应用程序可能有几个任务，这些任务可以并行执行，而不是串行执行。 应用程序可以通过以下方式实现此操作：启动各个任务的独立进程（这属于资源密集型操作），或启动独立的线程（相对而言它消耗资源较少）。 而且，可以独立于与某进程关联的其他线程来安排每个线程的执行。

线程使复杂的应用程序能够更有效地利用处理器 (CPU)，即使在只有一个 CPU 的计算机上也是如此。 如果只有一个 CPU，则每次只能执行一个线程。 如果一个线程执行不使用 CPU 的长时间运行的操作（如磁盘读/写操作），则第一个操作完成之前可以执行另一个线程。 通过在其他线程等待操作完成的同时执行线程，应用程序可以最大限度地利用 CPU。 对于大量占用磁盘 I/O 的多用户应用程序（如数据库服务器），这尤其有效。 具有多个 CPU 的计算机可以同时在每个 CPU 上执行一个线程。 例如，如果某计算机有八个 CPU，则它可以同时执行八个线程。

## <a name="sql-server-task-scheduling"></a>SQL Server 任务计划
在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的作用域中，请求是查询或批处理的逻辑表示形式。 请求还表示系统线程所需的操作，如检查点或日志编写器。 请求在其整个生存期期间以不同状态存在，并且在执行请求所需的资源不可用时请求可以累积等待，如[锁](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md#locks)或[闩锁](../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md#latches)。 有关请求状态的详细信息，请参阅 [sys.dm_exec_requests](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)。

任务表示满足请求所需完成的工作单元。 可以向单个请求分配一个或多个任务。 
-  并行请求将包含并行执行（而不是串行执行）的多个活动任务，其中包含一个“父任务”（或协调任务）和多个“子任务” 。 并行请求的执行计划可能具有串行分支（计划中包含不并行执行的运算符的区域）。 父任务还负责执行这些串行运算符。
-  在执行期间的任何给定时间点，串行请求都将只有一个活动任务。     
任务在其整个生存期期间以不同状态存在。 有关任务状态的详细信息，请参阅 [sys.dm_os_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)。 处于“挂起”状态的任务正在等待执行任务所需的资源可用。 有关正在等待的任务的详细信息，请参阅 [sys.dm_os_waiting_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md)。

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 工作线程（又称为工作器或线程）是操作系统线程的逻辑表现形式。 执行串行请求时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 将生成工作线程，以执行活动任务 (1:1)。 在[行模式](../relational-databases/query-processing-architecture-guide.md#execution-modes)下执行并行请求时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 将分配一个工作线程来协调负责完成已有任务的子线程（也为一对一的关系），该进程被称为“父线程”（或协调线程） 。 父线程具有与之关联的父任务。 父线程是请求的入口点，甚至在引擎分析查询之前就存在。 父线程主要负责： 
-  协调并行扫描。
-  启动子并行线程。
-  从并行线程收集行并发送到客户端。
-  执行本地和全局聚合。    

> [!NOTE]
> 如果查询计划具有串行和并行分支，则其中一个并行任务将负责执行串行分支。 

为每个任务生成的工作线程数取决于以下因素：
-   请求是否符合并行要求（由查询优化器确定）。
-   根据当前负载，系统的实际可用的[并行度 (DOP)](../relational-databases/query-processing-architecture-guide.md#DOP) 是多少。 它可能不同于估计的 DOP，后者基于最大并行度 (MAXDOP) 的服务器配置。 例如，MAXDOP 的服务器配置可能是 8，但在运行时可用的 DOP 可能仅为 2，这样则会影响查询性能。 

> [!NOTE]
> 将按任务设置最大并行度 (MAXDOP) 限制，而非按请求限制。 这意味着在并行查询执行期间，单个请求可以生成多个任务（数目最多达 MAXDOP 限制），并且每个任务将使用一个工作线程。 有关 MAXDOP 的详细信息，请参阅[配置最大并行度服务器配置选项](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。

计划程序（又称为 SOS 计划程序）管理需要处理时间来代表任务执行工作的工作线程。 每个计划程序映射单个处理器 (CPU)。 线程可以在计划程序内活动的时间称为 OS 量程，最长为 4 毫秒。 量程时间到期后，线程将其时间转让给需要访问 CPU 资源的其他线程，并更改自己的状态。 用于将 CPU 资源访问最大化的线程之间的协作称为“协作计划”（又称为非抢先计划）。 反之，线程状态更改会传播到与该线程关联的任务，并会传播到与相应任务关联的请求。 有关线程状态的详细信息，请参阅 [sys.dm_os_workers](../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md)。 有关计划程序的详细信息，请参阅 [sys.dm_os_schedulers](../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md)。 

总之，请求可能会生成一个或多个任务来执行工作单元 。 每个任务都分配给负责完成任务的“工作线程”。 必须对每个工作线程进行计划（置于计划程序上）以主动执行任务。 

> [!NOTE]
> 请参考以下方案：   
> -  辅助进程 1 是一项长时间运行的任务，例如对基于内存的表使用预读的读取查询。 辅助进程 1 会发现其所需的数据页已在缓冲池中，因此不必暂停以等待 I/O 操作，而可以在暂停之前使用其整个量程。   
> -  辅助进程 2 执行不足 1 毫秒的短时间运行任务，因此需要在其完整量程用尽之前暂停。     
>
> 在这种情况下，直到 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)]，辅助进程 1 都可以通过拥有更多的总量程时间来独占计划程序。   
> 从 [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] 开始，协作式计划包括大缺口优先 (LDF) 计划。 使用 LDF 计划时，会监视量程使用模式，不会让一个工作线程独占计划程序。 在同样的情况下，在为辅助进程 1 分配更多量程之前，允许辅助进程 2 使用重复的量程，从而阻止辅助进程 1 以不友好的模式独占计划程序。

### <a name="scheduling-parallel-tasks"></a>计划并行任务
假设使用 MaxDOP 8 配置 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]，并且跨 NUMA 节点 0 和 1 为 24 个 CPU（计划程序）配置了 CPU 关联。 计划程序 0 到 11 属于 NUMA 节点 0，计划程序 12 到 23 属于 NUMA 节点 1。 应用程序向 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 发送以下查询（请求）：

```sql
SELECT h.SalesOrderID, h.OrderDate, h.DueDate, h.ShipDate
FROM Sales.SalesOrderHeaderBulk AS h 
INNER JOIN Sales.SalesOrderDetailBulk AS d ON h.SalesOrderID = d.SalesOrderID 
WHERE (h.OrderDate >= '2014-3-28 00:00:00');
```

> [!TIP]
> 示例查询可以使用 [AdventureWorks2016_EXT sample database](../samples/adventureworks-install-configure.md) 数据库执行。 表 `Sales.SalesOrderHeader` 和 `Sales.SalesOrderDetail` 放大了 50 倍，并重命名为 `Sales.SalesOrderHeaderBulk` 和 `Sales.SalesOrderDetailBulk`。

执行计划显示两个表之间的[哈希联接](../relational-databases/performance/joins.md#hash)，并且每个运算符并行执行，如带有两个箭头的黄色圆圈所示。 每个并行运算符都是计划中不同的分支。 因此，以下执行计划中有三个分支。 

![并行查询计划](../relational-databases/media/schedule-parallel-query-plan.png)

> [!NOTE]
> 如果将执行计划视为树，则“分支”是计划的一个区域，它将并行运算符之间的一个或多个运算符分组，也称为“交换迭代器”。 有关计划运算符的详细信息，请参阅 [Showplan 逻辑运算符和物理运算符参考](../relational-databases/showplan-logical-and-physical-operators-reference.md)。 

虽然执行计划中有三个分支，但在执行期间的任何时候，只有两个分支可以在该执行计划中同时执行：
1.  在 `Sales.SalesOrderHeaderBulk`（联接的生成输入）上使用聚集索引扫描的分支单独执行。
2.  然后，在 `Sales.SalesOrderDetailBulk`（联接的探测输入）上使用聚集索引扫描的分支与已创建位图且当前正在执行哈希匹配的分支同时执行  。

Showplan XML 显示预留了 16 个工作线程，并且这些线程用于 NUMA 节点 0：

```xml
<ThreadStat Branches="2" UsedThreads="16">
  <ThreadReservation NodeId="0" ReservedThreads="16" />
</ThreadStat>
```

线程预留确保 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 有足够的工作线程来执行请求所需的所有任务。 可跨多个 NUMA 节点预留线程，也可仅在一个 NUMA 节点中预留。 线程预留在执行开始前于运行时完成，并且取决于计划程序的负载。 预留的工作线程数通常通过公式“并发分支数 * 运行时 DOP”得出，并排除父工作线程 。 每个分支限制为等于 MaxDOP 的工作线程数。 在此示例中有两个并发分支，MaxDOP 设置为 8，因此 2 * 8 = 16。

作为参考，请从[实时查询统计信息](../relational-databases/performance/live-query-statistics.md)（其中一个分支已完成，两个分支同时执行）观察实时执行计划。

![实时并行查询计划](../relational-databases/media/schedule-parallel-query-live-plan.png)

[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 将分配一个工作线程来执行活动任务 (1:1)，在查询执行期间，可以通过查询 [sys.dm_os_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md) DMV 来观察到该任务，如以下示例所示：

```sql
SELECT parent_task_address, task_address, 
       task_state, scheduler_id, worker_address
FROM sys.dm_os_tasks
WHERE session_id = <insert_session_id>
ORDER BY parent_task_address, scheduler_id;
```

> [!TIP]
> 对于父任务，列 `parent_task_address` 始终为 NULL。 

> [!TIP]
> 在非常繁忙的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 上，可以看到许多活动任务超出了预留线程设置的限制。 这些任务可以属于不再使用的分支，并且处于等待清理的暂时性状态。 

[!INCLUDE[ssResult](../includes/ssresult-md.md)] 请注意，当前正在执行的分支有 17 个活动任务：16 个子任务对应于预留的线程以及父任务或协调任务。

|parent_task_address|task_address|task_state|scheduler_id|worker_address|
|--------|--------|--------|--------|--------|
|Null|0x000001EF4758ACA8|SUSPENDED|3|0x000001EFE6CB6160|
|0x000001EF4758ACA8|0x000001EFE43F3468|SUSPENDED|0|0x000001EF6DB70160|
|0x000001EF4758ACA8|0x000001EEB243A4E8|SUSPENDED|0|0x000001EF6DB7A160|
|0x000001EF4758ACA8|0x000001EC86251468|SUSPENDED|5|0x000001EEC05E8160|
|0x000001EF4758ACA8|0x000001EFE3023468|SUSPENDED|5|0x000001EF6B46A160|
|0x000001EF4758ACA8|0x000001EFE3AF1468|SUSPENDED|6|0x000001EF6BD38160|
|0x000001EF4758ACA8|0x000001EFE4AFCCA8|SUSPENDED|6|0x000001EF6ACB4160|
|0x000001EF4758ACA8|0x000001EFDE043848|SUSPENDED|7|0x000001EEA18C2160|
|0x000001EF4758ACA8|0x000001EF69038108|SUSPENDED|7|0x000001EF6AEBA160|
|0x000001EF4758ACA8|0x000001EFCFDD8CA8|SUSPENDED|8|0x000001EFCB6F0160|
|0x000001EF4758ACA8|0x000001EFCFDD88C8|SUSPENDED|8|0x000001EF6DC46160|
|0x000001EF4758ACA8|0x000001EFBCC54108|SUSPENDED|9|0x000001EFCB886160|
|0x000001EF4758ACA8|0x000001EC86279468|SUSPENDED|9|0x000001EF6DE08160|
|0x000001EF4758ACA8|0x000001EFDE901848|SUSPENDED|10|0x000001EFF56E0160|
|0x000001EF4758ACA8|0x000001EF6DB32108|SUSPENDED|10|0x000001EFCC3D0160|
|0x000001EF4758ACA8|0x000001EC8628D468|SUSPENDED|11|0x000001EFBFA4A160|
|0x000001EF4758ACA8|0x000001EFBD3A1C28|SUSPENDED|11|0x000001EF6BD72160|

请注意，16 个子任务中的每一个任务都分配了不同的工作线程（参见 `worker_address` 列），但是所有的工作线程都分配到由 8 个计划程序（0、5、6、7、8、9、10、11）组成的相同池，并且父任务将分配给此池之外的计划程序 (3)。

> [!IMPORTANT]
> 计划给定分支上的第一组并行任务后，[!INCLUDE[ssde_md](../includes/ssde_md.md)] 将对其他分支上的任何其他任务使用此相同的计划程序池。 这意味着同一组计划程序将用于整个执行计划中的所有并行任务，仅受 MaxDOP 的限制。  
> [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 将始终尝试从同一 NUMA 节点分配计划程序来执行任务，并在计划程序可用时（以轮循方式）按顺序分配它们。 但是，分配给父任务的工作线程可能被置于与其他任务不同的 NUMA 节点中。

工作线程只能在其量程（4 毫秒）期间在计划程序中保持活动状态，并且必须在该时间段结束后生成其计划程序，以便分配给另一个任务的工作线程可以变为活动状态。 当工作线程的量程到期且不再活动时，相应任务将处于 RUNNABLE 状态的 FIFO 队列中，直到再次进入 RUNNING 状态，假设该任务不需要访问当前不可用的资源（例如闩锁或锁），在这种情况下，该任务将处于 SUSPENDED 状态而不是 RUNNABLE 状态，直到这些资源可用为止。  

> [!TIP] 
> 对于上述 DMV 的输出，所有活动任务都处于 SUSPENDED 状态。 有关正在等待的任务的详细信息，请查询 [sys.dm_os_waiting_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md) DMV。 

总而言之，并行请求将生成多个任务。 必须将每个任务分配到单个工作线程。 必须将每个工作线程分配到单个计划程序。 因此，正在使用的计划程序的数量不能超过每个分支的并行任务数，这是 MaxDOP 配置或查询提示设置的。 协调线程不影响 MaxDOP 限制。 

### <a name="allocating-threads-to-a-cpu"></a>为 CPU 分配线程
默认情况下，每个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例启动每个线程，操作系统根据负载从计算机上的处理器 (CPU) 中的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例分发线程。 如果已在操作系统级别启用了进程关联，则操作系统会将每个线程分配给特定的 CPU。 与之相反，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 工作线程分配给在 CPU 之间以轮询方式平均分发线程的计划程序 。
    
为了执行多任务处理，例如当多个应用程序访问同一组 CPU 时，操作系统有时会在不同 CPU 之间移动工作线程。 虽然从操作系统方面而言，这种活动是高效的，但是在高系统负荷的情况下，该活动会降低 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的性能，因为每个处理器缓存都会不断地重新加载数据。 如果将各个 CPU 分配给特定线程，则通过消除处理器的重新加载需要以及减少 CPU 之间的线程迁移（因而减少上下文切换），可以提高在这些条件下的性能；线程与处理器之间的这种关联称为“处理器关联”。 如果已经启用了关联，则操作系统会将每个线程分配给一个特定的 CPU。 

[关联掩码选项](../database-engine/configure-windows/affinity-mask-server-configuration-option.md)通过使用 [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md) 进行设置。 如果未设置关联掩码，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例会在未应用掩码的计划程序之间平均分配工作线程。

> [!CAUTION]
> 请勿在操作系统中配置 CPU 关联后，还在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中配置关联掩码。 这些设置实现的效果相同，如果配置不一致，则可能会得到意外的结果。 有关详细信息，请参阅[关联掩码选项](../database-engine/configure-windows/affinity-mask-server-configuration-option.md)。

当服务器上连接有大量客户端时，线程池有助于优化性能。 一般情况下，会为每个查询请求创建一个单独的操作系统线程。 但是，当到服务器的连接达到数以百计时，为每个查询请求使用一个线程会占用大量的系统资源。 [最大工作线程数选项](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md)使 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可以为更大数量的查询请求创建一个工作线程池，从而提高性能。 

### <a name="using-the-lightweight-pooling-option"></a>使用 lightweight pooling 选项
切换线程上下文的开销可能不是很大。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的大多数实例不会发现轻型池选项设置为 0 或 1 时性能有何差别。 只有运行在具有下列特征的计算机上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例可能从[轻型池](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)受益：    
* 大型多 CPU 服务器
* 所有 CPU 都以接近最大容量运行
* 存在高级别的上下文切换

如果将轻型池的值设为 1，则可能会略微提高这些系统的性能。

> [!IMPORTANT]
> 请勿使用纤程模式计划日常操作。 这会抑制上下文切换优点的正常发挥，从而降低性能，因为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的某些组件在纤程模式下不能正常工作。 有关详细信息，请参阅[轻型池](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)。

## <a name="thread-and-fiber-execution"></a>线程和纤程的执行
Microsoft Windows 使用从 1 到 31 的数值优先级系统计划线程的执行优先级。 零是保留值，专供操作系统使用。 当有多个线程等待执行时，Windows 将分派优先级最高的线程。

默认情况下，每个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例的优先级都是 7，这个优先级称为正常优先级。 此默认值为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 线程提供了足够高的优先级以获得足够的 CPU 资源，而不会对其他应用程序造成负面影响。 

> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../includes/ssnotedepfuturedontuse-md.md)]  

[优先级提升](../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md)配置选项可用于将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例的线程优先级提高到 13。 此优先级称为高优先级。 此设置为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 线程提供了一个比其他大多数应用程序高的优先级。 因此，通常当 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 线程准备好运行时便获得分派，而不会被其他应用程序的线程抢先。 这可以在服务器只运行 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例而未运行其他应用程序时提高性能。 但是，如果 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中发生占用大量内存的操作，其他应用程序便不可能有足够高的优先级而抢先于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 线程。 

如果在一台计算机上运行多个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例，而只为其中的某些实例打开了 priority boost 选项，那么任何以正常优先级运行的实例的性能都将受到负面影响。 而且，如果打开了优先级提升选项，服务器上的其他应用程序和组件的性能将会降低。 因此，必须在严格控制下使用此选项。


## <a name="hot-add-cpu"></a>热添加 CPU
热添加 CPU 是指能够动态向运行中的系统添加 CPU。 添加 CPU 时，可以通过添加新硬件来进行物理添加，或者通过联机硬件分区进行逻辑添加，或者通过虚拟化层进行虚拟添加。 从 [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] 开始，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 支持热添加 CPU。

热添加 CPU 的要求：  
* 要求使用支持热添加 CPU 的硬件。
* 要求使用 Windows Server 2008 Datacenter 的 64 位版本或 Windows Server 2008 Enterprise Edition for Itanium-Based Systems 操作系统。
* 要求具有 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise。
* [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 无法配置为使用软 NUMA。 有关软 NUMA 的详细信息，请参阅 [Soft-NUMA (SQL Server)](../database-engine/configure-windows/soft-numa-sql-server.md)。

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 不会在添加 CPU 后自动开始使用它们。 这可以防止 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 使用可能是为其他用途而添加的 CPU。 添加 CPU 后，请执行 [RECONFIGURE](../t-sql/language-elements/reconfigure-transact-sql.md) 语句以便 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将新 CPU 识别为可用资源。

> [!NOTE]
> 如果配置了 [affinity64 掩码](../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) ，则必须修改 affinity64 掩码以使用新 CPU。
 
## <a name="best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus"></a>在具有超过 64 个 CPU 的计算机上运行 SQL Server 的最佳做法

### <a name="assigning-hardware-threads-with-cpus"></a>给硬件线程分配 CPU
不要使用 affinity mask 和 affinity64 mask 服务器配置选项来将处理器绑定到特定线程。 这些选项限制为 64 个 CPU。 请改用 [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md) 的 `SET PROCESS AFFINITY` 选项。

### <a name="managing-the-transaction-log-file-size"></a>管理事务日志文件大小
不要依赖于自动增长来增加事务日志文件的大小。 增加事务日志必须是一个串行的过程。 扩展日志可能会阻止事务继续写操作，直到完成日志扩展。 请通过将文件大小设置为足够支持环境中典型工作负荷的值来预分配日志文件的空间。

### <a name="setting-max-degree-of-parallelism-for-index-operations"></a>设置索引操作的最大并行度
可以通过暂时将数据库的恢复模式设置为大容量日志恢复模式或简单恢复模式，以在具有许多 CPU 的计算机上改进索引操作（如创建或重新创建索引）的性能。 这些索引操作可以导致重大的日志活动和日志争用，从而影响 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 所做的最佳并行度 (DOP) 选择。

除调整最大并行度 (MAXDOP) 服务器配置选项以外，请考虑使用 [MAXDOP 选项](../t-sql/statements/alter-index-transact-sql.md)调整索引操作的并行度。 有关详细信息，请参阅 [配置并行索引操作](../relational-databases/indexes/configure-parallel-index-operations.md)。 有关调整最大并行度服务器配置选项的详细信息和指南，请参阅[配置最大并行度服务器配置选项](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。

### <a name="setting-the-maximum-number-of-worker-threads"></a>设置最大工作线程数
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将在启动时动态配置最大工作线程数服务器配置选项。 在启动时，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 使用可用 CPU 数和系统体系结构通过经过证实的[公式](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md#Recommendations)确定此服务器配置。

此选项是一个高级选项，仅应由有经验的数据库管理员或认证的 SQL Server 专业人员更改。 如果怀疑存在性能问题，可能不是由于工作线程不可用。 很有可能是导致工作线程等待的因素造成的，例如 I/O。 在更改最大工作线程设置之前，最好找到导致性能问题的根本原因。 但是，如果需要手动设置最大工作线程数，必须始终将此配置值设置为至少为系统上存在的 CPU 数的七倍的值。 有关详细信息，请参阅[配置最大工作线程数](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md)。

### <a name="using-sql-trace-and-sql-server-profiler"></a>使用 SQL 跟踪和 SQL Server Profiler
不建议在生产环境中使用 SQL 跟踪和 SQL Profiler。 运行这些工具的系统开销也会随着 CPU 的数目的增加而增加。 如果您必须在生产环境中使用 SQL 跟踪，请将跟踪事件的数目限制为最少。 请在负荷下仔细探查和测试每个跟踪事件，并且避免使用显著影响性能的事件组合。

> [!IMPORTANT]
> 已弃用 SQL 跟踪和 [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]。 包含 Microsoft SQL Server 跟踪和重播对象的“Microsoft.SqlServer.Management.Trace”命名空间也已遭弃用。 
> [!INCLUDE[ssNoteDepFutureAvoid](../includes/ssnotedepfutureavoid-md.md)] 
> 请改用扩展事件。 有关[扩展事件](../relational-databases/extended-events/extended-events.md)的详细信息，请参阅[快速入门：SQL Server 中的扩展事件](../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md)和 [SSMS XEvent 探查器](../relational-databases/extended-events/use-the-ssms-xe-profiler.md)。

> [!NOTE]
> 针对 Analysis Services 工作负荷的 [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] 尚未弃用，我们将继续提供支持。

### <a name="setting-the-number-of-tempdb-data-files"></a>设置 tempdb 数据文件的数目
文件数取决于计算机上的（逻辑）处理器数。 一般而言，如果逻辑处理器数目小于或等于 8，则使用的数据文件数与逻辑处理器数相同。 如果逻辑处理器数目大于 8，则使用 8 个数据文件，如果仍然存在争用，则以 4 的倍数增加数据文件的数量，直到争用减少到可接受的级别或对工作负荷/代码进行更改。 另外，请注意其他 tempdb 建议，可从[在 SQL Server 中优化 tempdb 性能](../relational-databases/databases/tempdb-database.md#optimizing-tempdb-performance-in-sql-server)查看它们。 

但是，通过仔细考虑 tempdb 的并发需要，可以减少数据库管理开销。 例如，如果一个系统具有 64 个 CPU 并且通常只有 32 个查询使用 tempdb，则将 tempdb 文件的数目增加到 64 将不会提高性能。

### <a name="sql-server-components-that-can-use-more-than-64-cpus"></a>可以使用超过 64 个 CPU 的 SQL Server 组件
下表列出了 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 组件，并指示它们是否可以使用超过 64 个 CPU。

|进程名称   |可执行程序 |是否可使用超过 64 个 CPU |  
|----------|----------|----------|  
|SQL Server 数据库引擎 |Sqlserver.exe  |是 |  
|Reporting Services |Rs.exe |否 |  
|Analysis Services  |As.exe |否 |  
|Integration Services   |Is.exe |否 |  
|Service Broker |Sb.exe |否 |  
|全文搜索   |Fts.exe    |否 |  
|SQL Server 代理   |Sqlagent.exe   |否 |  
|SQL Server Management Studio   |Ssms.exe   |否 |  
|SQL Server 安装程序   |Setup.exe  |否 |  
