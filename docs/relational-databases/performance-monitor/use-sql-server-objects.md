---
title: 使用 SQL Server 对象 | Microsoft Docs
description: 了解系统监视器用来在运行 SQL Server 实例的计算机中监视活动的 SQL Server 对象和计数器。
ms.custom: ''
ms.date: 03/17/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- server performance [SQL Server], objects for monitoring
- database monitoring [SQL Server], objects for monitoring
- charts [SQL Server]
- System Monitor [SQL Server], counters
- counters [SQL Server], listed
- objects [SQL Server], performance monitoring
- objects [SQL Server], Windows System Monitor
- performance counters [SQL Server], about performance counters
- System Monitor [SQL Server], objects
- performance counters [SQL Server]
- counters [SQL Server], about performance counters
- tuning databases [SQL Server], objects for monitoring
- database performance [SQL Server], objects for monitoring
- SQL Server, objects
- monitoring performance [SQL Server], objects for monitoring
- Windows System Monitor [SQL Server], objects
- Windows System Monitor [SQL Server], counters
- counters [SQL Server]
- performance counters [SQL Server], listed
ms.assetid: bcd731b1-3c4e-4086-b58a-af7a3af904ad
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: aa2384f95fee9dd40ae795eeed1a8a1a41f0d793
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237392"
---
# <a name="use-sql-server-objects"></a>使用 SQL Server 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供了对象和计数器，系统监视器可以使用它们监视运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的计算机中的活动。 对象可以是任何 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源，例如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 锁或 Windows 进程。 每个对象有一个或多个计数器，用于确定所要监视对象的各方面信息。 例如， **SQL Server Locks** 对象包含名为 **Number of Deadlocks/sec** 和 **Lock Timeouts/sec** 的计数器。  
  
 如果计算机上有某一个给定资源类型的多个资源，则一些对象会有几个实例。 例如，如果一个系统有多个处理器，则 **Processor** 对象类型会有多个实例。 对于 **上的每个数据库，** Databases [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]对象类型都有一个实例。 某些对象类型（例如， **Memory Manager** 对象）只有一个实例。 如果一个对象类型有多个实例，则可以增加计数器以跟踪每个实例的统计信息，另外在许多情况下，同时跟踪所有实例的统计信息。 默认实例的计数器以 SQLServer:\<object name> 格式显示。 命名实例的计数器以 MSSQL$\<instance name>:\<counter name> 或 SQLAgent$\<instance name>:\<counter name> 格式显示。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 性能计数器值是使用 Windows 性能计数器 (WPC) 引擎生成的。 某些计数器值不会直接由 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 计算。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会向 WPC 引擎提供基值，而引擎会执行所需计算（如百分比）。 [sys.dm_os_performance_counters (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) 动态管理视图为所有计数器提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成的原始值。 `cntr_type` 列指示计数器的类型。 WPC 引擎处理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计数器值的方式取决于此类型。 有关性能计数器类型的详细信息，请参阅 [WMI 文档](/windows/win32/wmisdk/wmi-performance-counter-types)。
  
 通过在图表中添加或删除计数器并保存图表设置，可以指定系统监视器启动后监视的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象和计数器。  
  
 可以配置系统监视器显示任何 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计数器中的统计信息。 另外，可以为任何 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计数器设置一个阈值，当计数器超过阈值时生成一个警报。 有关设置警报的详细信息，请参阅 [创建 SQL Server 数据库警报](../../relational-databases/performance-monitor/create-a-sql-server-database-alert.md)。  
    
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例后，才会显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 统计信息。 如果停止并重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例，统计信息的显示将中断，然后自动恢复。 还请注意，即使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 没有运行，您也会在系统监视器管理单元中看到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计数器。 在群集实例中，性能计数器只在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 运行的节点上发挥作用。  
  
 本主题包含以下各节：  
  
-   [SQL Server 代理性能对象](#SQLServerAgentPOs)  
  
-   [Service Broker 性能对象](#ServiceBrokerPOs)  
  
-   [SQL Server 性能对象](#SQLServerPOs)  
  
-   [SQL Server 复制性能对象](#SQLServerReplicationPOs)  
  
-   [SSIS 管道计数器](#SsisPipelineCounters)  
  
-   [所需的权限](#RequiredPermissions)  
  
##  <a name="sql-server-agent-performance-objects"></a><a name="SQLServerAgentPOs"></a> SQL Server 代理性能对象  
 下表列出了为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理提供的性能对象：  
  
|性能对象|说明|  
|------------------------|-----------------|  
|[SQLAgent:Alerts](../../relational-databases/performance-monitor/sql-server-agent-alerts-object.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理警报的信息。|  
|[SQLAgent:Jobs](../../relational-databases/performance-monitor/sql-server-agent-jobs-object.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业的信息。|  
|[SQLAgent:JobSteps](../../relational-databases/performance-monitor/sql-server-agent-jobsteps-object.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤的信息。|  
|[SQLAgent:Statistics](../../relational-databases/performance-monitor/sql-server-agent-statistics-object.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的常规信息。|  
  
##  <a name="service-broker-performance-objects"></a><a name="ServiceBrokerPOs"></a> Service Broker 性能对象  
 下表列出了为 [!INCLUDE[ssSB](../../includes/sssb-md.md)]代理提供的性能对象：  
  
|性能对象|说明|  
|------------------------|-----------------|  
|[SQLServer:Broker Activation](../../relational-databases/performance-monitor/sql-server-broker-activation-object.md)|提供有关已激活 [!INCLUDE[ssSB](../../includes/sssb-md.md)]的任务的信息。|  
|[SQLServer:Broker Statistics](../../relational-databases/performance-monitor/sql-server-broker-statistics-object.md)|提供 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 的常规信息。|  
|[SQLServer:Broker Transport](../../relational-databases/performance-monitor/sql-server-broker-dbm-transport-object.md)|提供有关 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 网络的信息。|  
  
##  <a name="sql-server-performance-objects"></a><a name="SQLServerPOs"></a> SQL Server 性能对象  
 下表介绍 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象：  
  
|性能对象|说明|  
|------------------------|-----------------|  
|[SQLServer:Access Methods](../../relational-databases/performance-monitor/sql-server-access-methods-object.md)|搜索并度量 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库对象的分配（例如，索引搜索数或分配给索引和数据的页数）。|  
|[SQLServer:Backup Device](../../relational-databases/performance-monitor/sql-server-backup-device-object.md)|提供有关备份和还原操作使用的备份设备的信息，如备份设备的吞吐量。|  
|[SQLServer:Batch Resp Statistics](../../relational-databases/performance-monitor/sql-server-batch-resp-statistics-object.md)|用于跟踪 SQL 批处理响应时间的计数器。| 
|[SQLServer:Buffer Manager](../../relational-databases/performance-monitor/sql-server-buffer-manager-object.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用的内存缓冲区的信息，如 **freememory** 和 **buffer cache hit ratio**。|  
|[SQL Server:Buffer Node](../../relational-databases/performance-monitor/sql-server-buffer-node.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 请求和访问可用页的频率的信息。|  
|[SQLServer:Catalog Metadata](../../relational-databases/performance-monitor/sql-server-catalog-metadata-object.md)|此项定义 SQL Server 的目录元数据管理器对象。| 
|[SQLServer:CLR](../../relational-databases/performance-monitor/sql-server-clr-object.md)|提供有关公共语言运行时 (CLR) 的信息。|  
|[SQLServer:Columnstore](../../relational-databases/performance-monitor/sql-server-columnstore-object.md)|**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 及更高版本）。<br /><br /> 提供有关列存储索引的行组和段的信息。|  
|[SQLServer:Cursor Manager by Type](../../relational-databases/performance-monitor/sql-server-cursor-manager-by-type-object.md)|提供游标信息。|  
|[SQLServer:Cursor Manager Total](../../relational-databases/performance-monitor/sql-server-cursor-manager-total-object.md)|提供游标信息。|  
|[SQLServer:Database Mirroring](../../relational-databases/performance-monitor/sql-server-database-mirroring-object.md)|提供有关数据库镜像的信息。|  
|[SQLServer:Databases](../../relational-databases/performance-monitor/sql-server-databases-object.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的信息，如可用的日志空间量或数据库中的活动事务数。 这个对象可有多个实例。|  
|[SQL Server:Deprecated Features](../../relational-databases/performance-monitor/sql-server-deprecated-features-object.md)|对使用不推荐使用的功能的次数进行计数。|  
|[SQLServer:Exec Statistics](../../relational-databases/performance-monitor/sql-server-execstatistics-object.md)|提供了有关执行统计信息的信息。|  
|[SQL Server:External Scripts](../../relational-databases/performance-monitor/sql-server-external-scripts-object.md)|**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 及更高版本）。<br /><br /> 提供有关外部脚本执行的信息。|  
|[SQLServer:FileTable](../../relational-databases/performance-monitor/sql-server-filetable-object.md)|与 FileTable 和非事务访问关联的统计信息。|  
|[SQLServer:General Statistics](../../relational-databases/performance-monitor/sql-server-general-statistics-object.md)|提供有关服务器范围内的常规活动的信息，如连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的用户数。|  
|[SQL Server：HADR 可用性副本](../../relational-databases/performance-monitor/sql-server-availability-replica.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 可用性副本的信息。|  
|[SQL Server：HADR 数据库副本](../../relational-databases/performance-monitor/sql-server-database-replica.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 数据库副本的信息。|  
|[SQL Server:HTTP Storage](../../relational-databases/performance-monitor/sql-server-http-storage-object.md)|提供信息，以便在使用 [Microsoft Azure 中的 SQL Server 数据文件](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)时监视 Microsoft Azure 存储帐户|  
|[SQLServer:Latches](../../relational-databases/performance-monitor/sql-server-latches-object.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]所用内部资源（如数据库页）上的闩锁的信息。|  
|[SQLServer:Locks](../../relational-databases/performance-monitor/sql-server-locks-object.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]执行的单个锁请求的信息，如锁超时和死锁。 这个对象可有多个实例。|  
|[SQLServer:LogPool FreePool](../../relational-databases/performance-monitor/sql-server-logpool-freepool-object.md)|描述了日志池内的可用池的统计信息。|
|[SQLServer:Memory Broker Clerks](../../relational-databases/performance-monitor/sql-server-memory-broker-clerks-object.md)|与 Memory Broker Clerk 相关的统计信息。|
|[SQLServer:Memory Manager](../../relational-databases/performance-monitor/sql-server-memory-manager-object.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存使用量的信息，如当前分配的锁结构总数。|  
|[SQLServer:Plan Cache](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md)|提供有关用于存储对象（如存储过程、触发器和查询计划）的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 缓存的信息。|  
|[SQLServer：查询存储](../../relational-databases/performance-monitor/sql-server-query-store-object.md)|提供有关查询存储的信息。|  
|[SQLServer：资源池统计信息](../../relational-databases/performance-monitor/sql-server-resource-pool-stats-object.md)|提供了有关资源调控器资源池统计的信息。|  
|[SQLServer:SQL Errors](../../relational-databases/performance-monitor/sql-server-sql-errors-object.md)|提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误的信息。|  
|[SQLServer:SQL Statistics](../../relational-databases/performance-monitor/sql-server-sql-statistics-object.md)|提供有关 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询各个方面的信息，如 [!INCLUDE[tsql](../../includes/tsql-md.md)] 收到的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]语句的批数。|  
|[SQLServer:Transactions](../../relational-databases/performance-monitor/sql-server-transactions-object.md)|提供了有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中活动事务的信息，如事务总数和快照事务数。|  
|[SQLServer:User Settable](../../relational-databases/performance-monitor/sql-server-user-settable-object.md)|执行自定义监视。 每个计数器可以是一个自定义的存储过程，也可以是任何返回一个被监视值的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。|  
|[SQLServer：等待统计信息](../../relational-databases/performance-monitor/sql-server-wait-statistics-object.md)|提供有关等待的信息。|  
|[SQLServer：工作负载组统计信息](../../relational-databases/performance-monitor/sql-server-workload-group-stats-object.md)|提供了有关资源调控器工作负荷组统计的信息。|  
  
##  <a name="sql-server-replication-performance-objects"></a><a name="SQLServerReplicationPOs"></a> SQL Server 复制性能对象  
 下表列出了为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 复制提供的性能对象：  
  
|性能对象|说明|  
|------------------------|-----------------|  
|**SQLServer:Replication Agents**<br /><br /> **SQLServer:Replication Snapshot**<br /><br /> **SQLServer:Replication Logreader**<br /><br /> **SQLServer:Replication Dist.**<br /><br /> **SQLServer:Replication Merge**<br /><br /> 有关详细信息，请参阅 [Monitoring Replication with System Monitor](../../relational-databases/replication/monitor/monitoring-replication-with-system-monitor.md)。|提供有关复制代理活动的信息。|  
  
##  <a name="ssis-pipeline-counters"></a><a name="SsisPipelineCounters"></a> SSIS 管道计数器  
 有关 **SSIS 管道** 计数器的信息，请参阅 [性能计数器](../../integration-services/performance/performance-counters.md)。  
  
##  <a name="required-permissions"></a><a name="RequiredPermissions"></a> 所需的权限  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象的使用取决于 Windows 权限（ **SQLAgent:Alerts** 除外）。 只有 **sysadmin** 固定服务器角色的成员可以使用 **SQLAgent:Alerts**。  
  
## <a name="see-also"></a>另请参阅  
 [使用性能对象](../../ssms/agent/use-performance-objects.md)   
 [sys.dm_os_performance_counters (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
