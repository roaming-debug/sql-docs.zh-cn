---
title: 性能监视和优化工具 | Microsoft Docs
description: 了解 SQL Server 监视和优化工具，以及如何根据监视类型和要监视的事件选择正确的工具。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- tools [SQL Server], monitoring performance
- monitoring server performance [SQL Server], tools
- monitoring performance [SQL Server], tools
- database performance [SQL Server], tools
- tuning databases [SQL Server], tools
- database monitoring [SQL Server], tools
- performance [SQL Server], monitoring tools
- server performance [SQL Server], tools
ms.assetid: 31529dfe-68e7-49f7-b3c2-39fcecf33a95
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: dcdfba8947333b6856485848b5597b766f71edbe
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505080"
---
# <a name="performance-monitoring-and-tuning-tools"></a>性能监视和优化工具
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供了一套综合的工具，用于监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的事件和优化物理数据库的设计。 工具的选择取决于要执行的监视或优化类型和要监视的具体事件。  
  
 以下是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 监视和优化工具：  
  
|工具|说明|  
|----------|-----------------|  
|[内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)|内置函数可显示自启动服务器以来有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 活动的快照统计信息，这些统计信息存储在预定义的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计数器中。 例如，\@\@CPU_BUSY 包含 CPU 执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代码所持续的时间；\@\@CONNECTIONS 包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接或尝试连接的次数；\@\@PACKET_ERRORS 包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接上出现的网络数据包数  。|  
|[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)|DBCC（数据库控制台命令）语句使您能够检查性能统计信息以及数据库的逻辑与物理一致性。|  
|[数据库引擎优化顾问 (DTA)](../../relational-databases/performance/database-engine-tuning-advisor.md)|数据库引擎优化顾问可分析所执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句对要优化的数据库性能的影响。 数据库引擎优化顾问提供了添加、删除或修改索引、索引视图及分区的建议。|  
|[数据库实验助手 (DEA)](https://www.microsoft.com/download/details.aspx?id=54090)|数据库实验助手 (DEA) 是 SQL Server 的新的 A/B 测试解决方案。 它会帮助评估给定工作负荷的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的目标版本。 从上一 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本（以 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 开始）升级到任何更新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本时，DEA 将能够提供比较性的分析指标。|
|错误日志|Windows 应用程序事件日志全面描述了 Windows Server 和 Windows 操作系统上发生的事件，以及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理和全文搜索中的事件。 它包含有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中独有的事件的信息。 您可以利用错误日志中的信息来解决与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]有关的问题。|  
|[扩展事件](../../relational-databases/extended-events/extended-events.md)|扩展事件是使用非常少的性能资源的轻型性能监视系统。 扩展事件提供三个图形用户界面（新建会话向导、新建会话和 XE Profiler），用于创建、修改、显示和分析会话数据。|  
|[与执行相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)|通过与执行相关的 DMV 可查看与执行相关的信息。|
|[实时查询统计信息 (LQS)](../../relational-databases/performance/live-query-statistics.md)|显示有关查询执行步骤的实时统计信息。 此数据在执行查询时可用，因此这些执行统计信息对于调试查询性能问题非常有用。|  
|[监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)|系统监视器主要用于跟踪资源的使用情况（如正在使用的缓冲区管理器页请求数），使您能够使用预定义的对象和计数器或用户定义的计数器来监视事件，从而监视服务器的性能与活动。 系统监视器（Microsoft Windows NT 4.0 中的性能监视器）将收集计数和比率而不是与事件相关的数据（例如，内存使用量、活动的事务数、阻塞的锁数或 CPU 活动）。 您可以在特定的计数器上设置阈值以生成要发送给操作员的警告。<br /><br /> 系统监视器在 Microsoft Windows Server 和 Windows 操作系统上运行。 它可以从远程或本地监视 Windows NT 4.0 或更高版本上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。<br /><br /> [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 与系统监视器之间的主要差别在于 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 用于监视数据库引擎事件，而系统监视器用于监视与服务器进程相关的资源使用情况。|  
|[打开活动监视器 (SQL Server Management Studio)](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)|[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的活动监视器对于当前活动的特别视图很有用，并以图形方式显示有关信息︰<br /><br />- 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例上运行的进程<br />- 被阻塞的进程<br />- 锁<br />- 用户活动|  
|[性能仪表板](../../relational-databases/performance/performance-dashboard.md)|[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的性能仪表板有助于快速确定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中是否存在任何当前性能瓶颈。|  
|[查询优化助手 (QTA)](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)|查询优化助手 (QTA) 功能将指导用户完成推荐的工作流，以便在升级到更新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本时保持性能稳定，正如[查询存储使用方案](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade)中的“在升级到新版 SQL Server 期间保持性能稳定”部分所述。 |  
|[查询存储](../..//relational-databases/performance/monitoring-performance-by-using-the-query-store.md)|查询存储功能为你提供有关查询计划选择和性能的见解。 它可帮助你快速找到查询计划更改所造成的性能差异，从而简化了性能疑难解答。 查询存储将自动捕获查询、计划和运行时统计信息的历史记录，并保留它们以供查阅。 它按时间窗口将数据分割开来，使你可以查看数据库使用模式并了解服务器上何时发生了查询计划更改。|
|[SQL 跟踪](../../relational-databases/sql-trace/sql-trace.md)|[!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程：<br /><br /> [sp_trace_create (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)<br />[sp_trace_generateevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)<br />[sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)<br />[sp_trace_setfilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)<br />[sp_trace_setstatus (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)|  
|[SQL Server 分布式重播](../../tools/distributed-replay/sql-server-distributed-replay.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式重播可以使用多台计算机重播跟踪数据，并模拟任务关键型工作负荷。|  
|[sp_trace_setfilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 用于跟踪引擎进程事件（如批处理或事务的开始），使您能够监视服务器和数据库的活动（例如，死锁、错误或登录活动）。 您可以将 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据捕获到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表或文件中供以后分析，还可以逐步重播在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上捕获的事件以确切了解所发生的事件。|  
|[系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)|下列 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统存储过程可以作为许多监视任务的一种功能强大的备选方法：<br /><br /> [sp_who (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)：<br />                    报告有关当前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户和进程的快照信息，包括当前正在执行的语句以及该语句是否被阻塞。<br /><br /> [sp_lock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)：<br />                    报告有关锁的快照信息，包括对象 ID、索引 ID、锁的类型以及锁应用于的类型或资源。<br /><br /> [sp_spaceused (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)： <br />                    显示对表（或整个数据库）所用的当前磁盘空间量的估计。<br /><br /> [sp_monitor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)：<br />                    显示统计信息，包括 CPU 使用率、I/O 使用率以及自上次执行 **sp_monitor** 以来的空闲时间。|  
|[跟踪标志 (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)|跟踪标志可显示有关服务器内的特定活动的信息，用于诊断问题或性能问题（例如死锁链）。|  
  
## <a name="choosing-a-monitoring-tool"></a>选择监视工具  
 监视工具的选择取决于要监视的事件或活动。  
  
|事件或活动|扩展事件|SQL Server Profiler|分布式重播|系统监视器|活动监视器|Transact-SQL|错误日志|性能仪表板|  
|-----------------------|-----------------------|-------------------------|------------------------|--------------------|----------------------|-------------------|----------------|----------------|   
|走向分析|是|是||是|||||  
|重播捕获的事件||是（从单台计算机）|是（从多台计算机）||||||  
|临时监视|是<sup>1</sup>|是|||是|是|是|是|  
|生成警报||||是|||||  
|图形界面|是|是||是|是||是|是|  
|在自定义应用程序内使用|是|是<sup>2</sup>||||是|||  
  
 <sup>1</sup> 使用 [SQL Server Management Studio XEvent Profiler](../../relational-databases/extended-events/use-the-ssms-xe-profiler.md)    
 <sup>2</sup> 使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 系统存储过程。  
  
## <a name="windows-monitoring-tools"></a>Windows 监视工具  
 Windows 操作系统和 Windows Server 2003 也提供这些监视工具。  
  
|工具|说明|  
|----------|-----------------|  
|任务管理器|显示在系统上运行的进程和应用程序的提要。|  
|网络监视器代理|用于监视网络流量。|  
  
 有关 Windows 操作系统或 Windows Server 工具的详细信息，请参阅 Windows 文档。  
  
  
