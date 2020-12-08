---
title: SQL Server - Resource Pool Stats 对象 | Microsoft Docs
description: 了解 SQLServer:Resource Pool Stats 对象，该对象包含报告 Resource Governor 资源池统计信息的性能计数器。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Reosurce Pool Stats object
- 'SQLServer: Resource Pool Stats object'
ms.assetid: bb46e029-fcf9-4aeb-a066-be41e7668fb9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2c0a5a4f4f75d6a585faf35d94d98f7c08997d6f
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505591"
---
# <a name="sql-server-resource-pool-stats-object"></a>SQL Server，Resource Pool Stats 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  SQLServer:Resource Pool Stats 对象包含报告资源调控器资源池统计相关信息的性能计数器。  
  
 每个活动资源池都创建一个 SQLServer:Resource Pool Stats 性能对象实例，实例的名称与资源调控器资源池的名称相同。 下表介绍了此实例支持的计数器。  
  
|计数器名称|说明|  
|------------------|-----------------|  
|**Active memory grant amount (KB)**|当前授予的内存总量，以千字节 (KB) 为单位。 此信息还可在 [sys.dm_exec_query_resource_semaphores](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-resource-semaphores-transact-sql.md)中获得。| 
|**Active memory grants count**|当前内存授予总数。 此信息还可在 [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)中获得。|  
|**Avg Disk Read IO (ms)**|从磁盘进行读取操作的平均时间（毫秒）。|  
|**Avg Disk Read IO (ms) Base**|仅供内部使用。|
|**Avg Disk Write IO (ms)**|对磁盘进行写入操作的平均时间（毫秒）。|  
|**Avg Disk Write IO (ms) Base**|仅供内部使用。|
|**Cache memory target (KB)**|缓存的当前内存代理目标值，以千字节 (KB) 为单位。|  
|**Compile memory target (KB)**|查询编译的当前内存代理目标值，以千字节 (KB) 为单位。|  
|**CPU control effect %**|资源调控器对资源池的控制效果。 计算公式为：（CPU 使用率 %）/（无资源调控器情况下的 CPU 使用率 %）。|  
|**CPU delayed %**|以占总活动时间的百分比表示的指定的性能对象实例中所有请求的系统 CPU 延迟。|
|**CPU delayed % base**|仅供内部使用。|
|**CPU effective %**|以占总活动时间的百分比表示的指定的性能对象实例中所有请求的系统 CPU 使用率。|
|**CPU effective % base**|仅供内部使用。|
|**CPU usage %**|属于此池的所有工作负荷组中所有请求的 CPU 带宽使用量。 此值是相对于计算机度量的，并针对系统中的所有 CPU 进行规范化。 此值将随着可用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的 CPU 量的变化而变化。 它不会针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程接收的信息进行规范化。|  
|**CPU usage % base**|仅供内部使用。|
|**CPU usage target %**|资源池基于资源池配置设置和系统负荷的目标 CPU 使用率 (%)。|  
|**CPU violated %**|CPU 预留和有效计划百分比之差。|
|**Disk Read Bytes/sec**|在上一秒中从磁盘读取的字节数。|  
|**Disk Read IO Throttled/sec**|在上一秒中中止的读取操作数。|  
|**Disk Read IO/sec**|在上一秒中从磁盘读取的操作数。| 
|**Disk Write Bytes/sec**|在上一秒中写入磁盘的字节数。|  
|**Disk Write IO Throttled/sec**|在上一秒中中止的写入操作数。| 
|**Disk Write IO/sec**|在上一秒中写入磁盘的操作数。|
|**Max memory (KB)**|资源池基于资源池设置和服务器状态可获得的最大内存量，以千字节 (KB) 为单位。| 
|**Memory grant timeouts/sec**|每秒内存授予超时数。|
|**Memory grants/sec**|每秒此资源池中发生的内存授予数。| 
|**Pending memory grant count**|队列中挂起的内存授予请求数。 此信息还可在 [sys.dm_exec_query_resource_semaphores](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-resource-semaphores-transact-sql.md)中获得。|
|**Query exec memory target (KB)**|查询执行内存授予的当前内存代理目标值，以千字节 (KB) 为单位。 此信息还可在 [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)中获得。|  
|**Target memory (KB)**|资源池基于资源池设置和服务器状态尝试获得的目标内存量，以千字节 (KB) 为单位。|   
|**Used memory (KB)**|用于资源池的内存量，以千字节 (KB) 为单位。|  

  
## <a name="see-also"></a>另请参阅  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQLServer，Workload Group Stats 对象](../../relational-databases/performance-monitor/sql-server-workload-group-stats-object.md)   
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)  
  
  
