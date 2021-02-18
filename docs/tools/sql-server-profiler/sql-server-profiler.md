---
title: SQL Server Profiler
titleSuffix: SQL Server Profiler
description: 探索 SQL Server Profiler 的功能。 获取帮助以排查问题，方法是使用此工具创建跟踪并分析和重播跟踪结果。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: profiler
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 05/01/2020
ms.openlocfilehash: 6208c880dd234a0e6a62f61e6f6ae87567213c12
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354451"
---
# <a name="sql-server-profiler"></a>SQL Server Profiler

[!INCLUDE[sql-asdbmi](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 是一个界面，用于创建和管理跟踪并分析和重播跟踪结果。 这些事件保存在一个跟踪文件中，稍后诊断问题时，可以对该文件进行分析或用它来重播一系列特定的步骤。

> [!IMPORTANT]
> 已弃用 SQL 跟踪和 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]。 包含 Microsoft SQL Server 跟踪和重播对象的“Microsoft.SqlServer.Management.Trace”命名空间也已遭弃用。
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]
> 请改用扩展事件。 有关[扩展事件](../../relational-databases/extended-events/extended-events.md)的详细信息，请参阅[快速入门：SQL Server 中的扩展事件](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md)和 [SSMS XEvent 探查器](../../relational-databases/extended-events/use-the-ssms-xe-profiler.md)。

> [!NOTE]
> 不支持针对 Analysis Services 工作负载的 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]。

> [!NOTE]
> 尝试从 SQL Server Profiler 连接到 Azure SQL 数据库时，会错误地引发误导性错误消息，如下所示：
>
> - 若要对 SQL Server 运行跟踪，你必须是 sysadmin 固定服务器角色的成员或拥有 ALTER TRACE 权限。
>
> 该消息应说明了 SQL Server Profiler 不支持 Azure SQL 数据库。

## <a name="where-is-the-profiler"></a>在哪里可以启动探查器？

可以在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中通过多种方式启动 Profiler。 [下面的主题列出了启动探查器的方式。](start-sql-server-profiler.md)

## <a name="capture-and-replay-trace-data"></a>捕获和重播跟踪数据

下表显示我们在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中推荐使用的用于捕获和重播跟踪数据的功能。

||||
|-|-|-|
|**功能\目标工作负荷**|**关系引擎**|**Analysis Services**|  
|**跟踪捕获**|[扩展事件](../../relational-databases/extended-events/extended-events.md) [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的图形用户界面|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]|  
|**跟踪重播**|[Distributed Replay](../distributed-replay/sql-server-distributed-replay.md)|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]|

## <a name="use-sql-server-profiler"></a>使用 SQL Server Profiler

Microsoft [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 是 SQL 跟踪的图形用户界面，用于监视 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 或 Analysis Services 的实例。 您可以捕获有关每个事件的数据并将其保存到文件或表中供以后分析。 例如，可以对生产环境进行监视，了解哪些存储过程由于执行速度太慢影响了性能。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 用于如下活动：

- 逐步分析有问题的查询以找到问题的原因。

- 查找并诊断运行慢的查询。

- 捕获导致某个问题的一系列 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 然后用所保存的跟踪在某台测试服务器上复制此问题，接着在该测试服务器上诊断问题。

- 监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的性能以优化工作负荷。 有关为数据库工作负荷而优化物理数据库设计的信息，请参阅 [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)。

- 使性能计数器与诊断问题关联。

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 还支持对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上执行的操作进行审核。 审核将记录与安全相关的操作，供安全管理员以后复查。

## <a name="sql-server-profiler-concepts"></a>SQL Server Profiler 概念

若要使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]，您需要了解描述该工具工作方式的术语。

> [!NOTE]
> 使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 时，了解 SQL 跟踪非常有帮助。 有关详细信息，请参阅 [SQL Trace](../../relational-databases/sql-trace/sql-trace.md)。

### <a name="event"></a>事件

事件是在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]实例中生成的操作。 示例包括：  
  
- 登录连接、失败和断开。    
- [!INCLUDE[tsql](../../includes/tsql-md.md)]、`SELECT`、`INSERT`、`UPDATE` 和 `DELETE` 语句。
- 远程过程调用 (RPC) 批处理状态。  
- 存储过程的开始或结束。  
- 存储过程中的语句的开始或结束。  
- SQL 批处理的开始或结束。  
- 写入到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志的错误。  
- 在数据库对象上获取或释放的锁。  
- 打开的游标。  
- 安全权限检查。  
  
由事件生成的所有数据显示在单个行中的跟踪内。 该行与详细说明事件的数据列相交。  

### <a name="eventclass"></a>EventClass

事件类是可跟踪的事件类型。 事件类包含所有可由事件报告的数据。 下面是事件类的示例：

- **SQL:BatchCompleted**
- **审核登录**
- **审核注销**
- Lock:  Acquired
- Lock:  Released

### <a name="eventcategory"></a>EventCategory

事件类别定义 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]中的事件的分组方法。 例如，所有锁事件类都分组在 **Locks** 事件类别中。 但是，事件类别仅存在于 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]中。 该术语不反映引擎事件的分组方法。

### <a name="datacolumn"></a>DataColumn

数据列是在跟踪中捕获的事件类的属性。 由于事件类决定了可收集的数据类型，因此并不是所有数据列都适用于所有事件类。 例如，在捕获 Lock:  Acquired 事件类的跟踪中，BinaryData  数据列包含锁定的页 ID 或行的值，但 Integer Data  数据列不包含任何值，因为该数据列不适用于被捕获的事件类。

### <a name="template"></a>模板

模板定义跟踪的默认配置。 具体地说，它包括您要使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]监视的事件类。 例如，可以创建一个指定了要使用的事件、数据列和筛选器的模板。 模板不会被执行，而是用 .tdf 扩展名保存为文件。 保存后，模板就可以在启动基于此模板的跟踪时控制捕获的跟踪数据。

### <a name="trace"></a>跟踪

跟踪基于选定的事件、数据列和筛选器捕获数据。 例如，可创建跟踪来监视异常错误。 为此，请选择 **Exception** 事件类以及 **Error**、 **State** 和 **Severity** 数据列。 需要收集这三列的数据，以使跟踪结果可提供有意义的数据。 然后，可运行以此方式配置的跟踪，并可收集有关服务器中发生的任何 **Exception** 事件的数据。 可以保存跟踪数据，也可以立刻将其用于分析。 尽管某些事件（如 **Exception** 事件）永远不会被重播，但跟踪以后可以被重播。 还可以将跟踪保存为模板，以便在将来生成类似的跟踪。  

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供了以下两种用于跟踪 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的方法：使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 进行跟踪，或使用系统存储过程进行跟踪。  

### <a name="filter"></a>筛选器

当创建跟踪或模板时，可以定义筛选由事件收集的数据的准则。 若要避免跟踪过大，可以筛选跟踪，以便只收集一部分事件数据。 例如，可以在跟踪中将 Microsoft Windows 用户名限制为特定的用户，从而减少输出的数据。  

如果没有设置筛选器，则跟踪输出中将返回选定事件类的所有事件。

## <a name="sql-server-profiler-tasks"></a>SQL Server Profiler 任务

|任务说明|主题|  
|----------------------|-----------|  
|列出 SQL Server 提供的用于监视特定类型事件的预定义模板，以及重播跟踪所需使用的权限。|[SQL Server Profiler 模板和权限](../../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)|  
|介绍了如何运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler。|[运行 SQL Server Profiler 所需的权限](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)|  
|介绍如何创建跟踪。|[创建跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)|  
|说明如何指定跟踪文件的事件和数据列。|[指定跟踪文件的事件和数据列 (SQL Server Profiler)](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md)|  
|说明如何将跟踪结果保存到文件。|[将跟踪结果保存到文件 (SQL Server Profiler)](../../tools/sql-server-profiler/save-trace-results-to-a-file-sql-server-profiler.md)|  
|说明如何将跟踪结果保存到表。|[将跟踪结果保存到表 (SQL Server Profiler)](../../tools/sql-server-profiler/save-trace-results-to-a-table-sql-server-profiler.md)|  
|说明如何筛选跟踪中的事件。|[在跟踪中筛选事件 (SQL Server Profiler)](../../tools/sql-server-profiler/filter-events-in-a-trace-sql-server-profiler.md)|  
|说明如何查看筛选信息。|[查看筛选器信息 (SQL Server Profiler)](../../tools/sql-server-profiler/view-filter-information-sql-server-profiler.md)|  
|说明如何修改筛选器。|[修改筛选器 (SQL Server Profiler)](../../tools/sql-server-profiler/modify-a-filter-sql-server-profiler.md)|  
|说明如何设置跟踪文件的文件大小上限 ([!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)])。|[设置跟踪文件的最大文件大小 (SQL Server Profiler)](../../tools/sql-server-profiler/set-a-maximum-file-size-for-a-trace-file-sql-server-profiler.md)|  
|说明如何设置跟踪表的最大表大小。|[设置跟踪表的最大表大小 (SQL Server Profiler)](../../tools/sql-server-profiler/set-a-maximum-table-size-for-a-trace-table-sql-server-profiler.md)|  
|说明如何启动跟踪。|[启动跟踪](../../tools/sql-server-profiler/start-a-trace.md)|  
|说明如何在连接到服务器后自动启动跟踪。|[连接到服务器后自动启动跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/start-a-trace-automatically-after-connecting-to-a-server-sql-server-profiler.md)|  
|说明如何基于事件开始时间筛选事件。|[基于事件开始时间筛选事件 (SQL Server Profiler)](../../tools/sql-server-profiler/filter-events-based-on-the-event-start-time-sql-server-profiler.md)|  
|说明如何基于事件结束时间筛选事件。|[基于事件结束时间筛选事件 (SQL Server Profiler)](../../tools/sql-server-profiler/filter-events-based-on-the-event-end-time-sql-server-profiler.md)|  
|说明如何筛选跟踪中的服务器进程 ID (SPID)。|[在跟踪中筛选服务器进程 ID (SPID) (SQL Server Profiler)](../../tools/sql-server-profiler/filter-server-process-ids-spids-in-a-trace-sql-server-profiler.md)|  
|介绍如何暂停跟踪。|[暂停跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/pause-a-trace-sql-server-profiler.md)|  
|介绍如何停止跟踪。|[停止跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/stop-a-trace-sql-server-profiler.md)|  
|说明如何在跟踪暂停或停止之后运行跟踪。|[在跟踪暂停或停止之后运行跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/run-a-trace-after-it-has-been-paused-or-stopped-sql-server-profiler.md)|  
|说明如何清除跟踪窗口。|[清除跟踪窗口 (SQL Server Profiler)](../../tools/sql-server-profiler/clear-a-trace-window-sql-server-profiler.md)|  
|说明如何关闭跟踪窗口。|[关闭跟踪窗口 (SQL Server Profiler)](../../tools/sql-server-profiler/close-a-trace-window-sql-server-profiler.md)|  
|说明如何设置跟踪定义默认值。|[设置跟踪定义默认值 (SQL Server Profiler)](../../tools/sql-server-profiler/set-trace-definition-defaults-sql-server-profiler.md)|  
|说明如何设置跟踪显示默认值。|[设置跟踪显示默认值 (SQL Server Profiler)](../../tools/sql-server-profiler/set-trace-display-defaults-sql-server-profiler.md)|  
|说明如何打开跟踪文件。|[打开跟踪文件 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)|  
|说明如何打开跟踪表。|[打开跟踪表 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)|  
|说明如何重播跟踪表。|[重播跟踪表 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-trace-table-sql-server-profiler.md)|  
|说明如何重播跟踪文件。|[重播跟踪文件 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-trace-file-sql-server-profiler.md)|  
|说明如何每次重播一个事件。|[每次重播一个事件 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-single-event-at-a-time-sql-server-profiler.md)|  
|说明如何重播到断点。|[重播到断点 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-to-a-breakpoint-sql-server-profiler.md)|  
|介绍如何重播至光标处。|[重播至光标处 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-to-a-cursor-sql-server-profiler.md)|  
|介绍了如何重播 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。|[重播 Transact-SQL 脚本 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-transact-sql-script-sql-server-profiler.md)|  
|说明如何创建跟踪模板。|[创建跟踪模板 (SQL Server Profiler)](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md)|  
|说明如何修改跟踪模板。|[修改跟踪模板 (SQL Server Profiler)](./modify-trace-templates.md)|  
|说明如何设置全局跟踪选项。|[设置全局跟踪选项 (SQL Server Profiler)](../../tools/sql-server-profiler/set-global-trace-options-sql-server-profiler.md)|  
|说明如何在跟踪时查找值或数据列。|[在跟踪时查找值或数据列 (SQL Server Profiler)](../../tools/sql-server-profiler/find-a-value-or-data-column-while-tracing-sql-server-profiler.md)|  
|说明如何从正在运行的跟踪派生模板。|[从正在运行的跟踪中派生模板 (SQL Server Profiler)](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md)|  
|说明如何从跟踪文件或跟踪表派生模板。|[从跟踪文件或跟踪表派生模板 (SQL Server Profiler)](../../tools/sql-server-profiler/derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler.md)|  
|介绍了如何创建 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本来运行跟踪。|[创建 Transact-SQL 脚本来运行跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/create-a-transact-sql-script-for-running-a-trace-sql-server-profiler.md)|  
|说明如何导出跟踪模板。|[导出跟踪模板 (SQL Server Profiler)](../../tools/sql-server-profiler/export-a-trace-template-sql-server-profiler.md)|  
|说明如何导入跟踪模板。|[导入跟踪模板 (SQL Server Profiler)](../../tools/sql-server-profiler/import-a-trace-template-sql-server-profiler.md)|  
|说明如何从跟踪提取脚本。|[从跟踪提取脚本 (SQL Server Profiler)](../../tools/sql-server-profiler/extract-a-script-from-a-trace-sql-server-profiler.md)|  
|说明如何将跟踪与 Windows 性能日志数据关联。|[将跟踪与 Windows 性能日志数据关联 (SQL Server Profiler)](./correlate-a-trace-with-windows-performance-log-data.md)|  
|说明如何组织跟踪中显示的列。|[组织跟踪中显示的列 (SQL Server Profiler)](../../tools/sql-server-profiler/organize-columns-displayed-in-a-trace-sql-server-profiler.md)|  
|介绍了如何启动 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]。|[启动 SQL Server Profiler](../../tools/sql-server-profiler/start-sql-server-profiler.md)|  
|说明如何保存跟踪和跟踪模板。|[保存跟踪和跟踪模板](../../tools/sql-server-profiler/save-traces-and-trace-templates.md)|  
|说明如何修改跟踪模板。|[修改跟踪模板](../../tools/sql-server-profiler/modify-trace-templates.md)|  
|说明如何将跟踪与 Windows 性能日志数据关联。|[将跟踪与 Windows 性能日志数据关联](../../tools/sql-server-profiler/correlate-a-trace-with-windows-performance-log-data.md)|  
|介绍了如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 查看和分析跟踪。|[使用 SQL Server Profiler 查看和分析跟踪](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md)|  
|介绍了如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 分析死锁。|[使用 SQL Server Profiler 分析死锁](../../tools/sql-server-profiler/analyze-deadlocks-with-sql-server-profiler.md)|  
|说明如何在 SQL Server Profiler 中使用 SHOWPLAN 结果来分析查询。|[在 SQL Server Profiler 中使用 SHOWPLAN 结果来分析查询](../../tools/sql-server-profiler/analyze-queries-with-showplan-results-in-sql-server-profiler.md)|  
|介绍了如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 筛选跟踪。|[使用 SQL Server Profiler 筛选跟踪](../../tools/sql-server-profiler/filter-traces-with-sql-server-profiler.md)|  
|说明如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 的重播功能。|[重播跟踪](../../tools/sql-server-profiler/replay-traces.md)|  
|列出了 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 的上下文相关帮助主题。|[SQL Server Profiler 的 F1 帮助](../../tools/sql-server-profiler/sql-server-profiler-f1-help.md)|  
|列出由 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 用于监视性能和活动的系统存储过程。|[SQL Server Profiler 存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)|  

## <a name="see-also"></a>另请参阅

- [“锁”事件类别](../../relational-databases/event-classes/locks-event-category.md)
- [“会话”事件类别](../../relational-databases/event-classes/sessions-event-category.md)
- [“存储过程”事件类别](../../relational-databases/event-classes/stored-procedures-event-category.md)
- [TSQL 事件类别](../../relational-databases/event-classes/tsql-event-category.md)
- [服务器性能和活动监视](../../relational-databases/performance/server-performance-and-activity-monitoring.md)