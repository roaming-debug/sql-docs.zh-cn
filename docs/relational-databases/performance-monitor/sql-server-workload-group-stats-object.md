---
title: SQL Server - Workload Group Stats 对象 | Microsoft Docs
description: 了解 SQLServer:Workload Group Stats 对象包含报告，该对象包含报告 Resource Governor 工作负荷组统计信息的性能计数器。
ms.custom: ''
ms.date: 12/04/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Workload Group Stats object
- 'SQLServer: Workload Group Stats'
ms.assetid: ca20e4f6-50ec-4456-900d-87d280fde2b3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cf82172eaa811d9992588e1615e55509c31be803
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505526"
---
# <a name="sql-server-workload-group-stats-object"></a>SQLServer，Workload Group Stats 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  SQLServer:Workload Group Stats 对象包含报告资源调控器工作负荷组统计相关信息的性能计数器。  
  
 每个活动工作负荷组都创建一个 SQLServer:Workload Group Stats 性能对象实例，实例的名称与资源调控器工作负荷组的名称相同。 下表介绍了此实例支持的计数器。  
  
|计数器名称|说明|  
|------------------|-----------------|  
|**Active parallel threads**|当前使用的并行线程数。|  
|**Active requests**|此工作负荷组中当前运行的请求数。 此值应该等于按组 ID 筛选的 sys.dm_exec_requests 的行数。|  
|**Blocked requests**|工作负荷组中当前被禁止的请求数。 此值可用于确定工作负荷特征。|  
|**CPU delayed %**|以占总活动时间的百分比表示的指定的性能对象实例中所有请求的系统 CPU 延迟。| 
|**CPU delayed % base**|仅供内部使用。| 
|**CPU effective %**|以占总活动时间的百分比表示的指定的性能对象实例中所有请求的系统 CPU 使用率。| 
|**CPU effective % base**|仅供内部使用。| 
|**CPU usage %**|此工作负荷组中所有请求的 CPU 带宽使用量，该值是相对于计算机度量的，并且针对系统中的所有 CPU 进行规范化。 此值将随着可用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的 CPU 量的变化而变化。 它不会针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程接收的信息进行规范化。| 
|**CPU usage % base**|仅供内部使用。| 
|**CPU violated %**|CPU 预留和有效计划百分比之差。|  
|**Max request CPU time (ms)**|此工作负荷组中当前运行的请求所用的最长 CPU 时间，以毫秒为单位。|  
|**Max request memory grant (KB)**|查询的最大内存授予值，以千字节 (KB) 为单位。|  
|**Query optimizations/sec**|每秒此工作负荷组中发生的查询优化次数。 此值可用于确定工作负荷特征。|  
|**Queued requests**|当前正在等待拾取的排队请求数。 如果达到 GROUP_MAX_REQUESTS 限制后操作中止，则此计数可为非零值。|  
|**Reduced memory grants/sec**|每秒所获内存量小于理想内存授予量的查询数。|  
|**Requests completed/sec**|此工作负荷组中已完成的请求数。 此数值可累计。|  
|**Suboptimal plans/sec**|每秒此工作负荷组中生成的非最优计划数。|  
  
## <a name="see-also"></a>另请参阅  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server，Resource Pool Stats 对象](../../relational-databases/performance-monitor/sql-server-resource-pool-stats-object.md)   
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)  
  
  
