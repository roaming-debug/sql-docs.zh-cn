---
title: 监视资源使用情况（系统监视器）| Microsoft Docs
description: 使用系统监视器可以测量 SQL Server 对象的性能、性能计数器以及其他对象（例如处理器和内存）的行为。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- monitoring performance [SQL Server], resource usage
- System Monitor [SQL Server], about Windows System Monitor
- resource usage monitoring [SQL Server]
- System Monitor [SQL Server]
- counters [SQL Server], resource usage subjects
- performance counters [SQL Server], resource usage subjects
- Windows System Monitor [SQL Server], about Windows System Monitor
- monitoring [SQL Server], server resource usage
- monitoring resource usage [SQL Server]
- Windows System Monitor [SQL Server]
- database monitoring [SQL Server], resource usage
- database performance [SQL Server], resource usage
- tuning databases [SQL Server], resource usage
- server performance [SQL Server], resource usage
ms.assetid: f2993a28-0b81-46f2-aec0-6877fe990387
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9ba5319dcc0f7a039e6e37ced535c4600f92136d
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505984"
---
# <a name="monitor-resource-usage-system-monitor"></a>监视资源使用情况（系统监视器）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  如果您运行的是 Microsoft Windows 服务器操作系统，则可以使用系统监视器图形工具来测量 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的性能。 可以查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象、性能计数器以及其他对象的行为，这些对象包括处理器、内存、缓存、线程和进程。 每个对象都有一个相关的计数器集，用于测量设备使用情况、队列长度、延时情况，另外还有吞吐量及内部拥塞指示器。  
  
> [!NOTE]  
>  在 Windows NT 4.0 以后的版本，系统监视器替换了性能监视器。  
  
## <a name="benefits-of-system-monitor"></a>系统监视器的优点  
 系统监视器可用于同时监视 Windows 操作系统和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计数器，以便确定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 性能与 Windows 性能之间可能存在的关联。 例如，同时监视 Windows 磁盘输入/输出 (I/O) 计数器和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 缓冲区管理器计数器可以揭示整个系统的行为。  
  
 系统监视器使您可以获取有关当前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 活动和性能的统计信息。 利用系统监视器，可以：  
  
-   同时查看任意多台计算机中的数据。  
  
-   查看和更改图表以反映当前的活动，以及显示按用户定义的频率进行更新的计数器值。  
  
-   从图表、日志、警报日志和报告将数据导出到电子表格或数据库应用程序中，以进行进一步的操作和打印。  
  
-   添加系统警报，这些警报可在警报日志中列出事件，并可以通过发出网络警报来通知您。  
  
-   当计数器值首次或每次超过或低于用户定义的值时，运行预定义的应用程序。  
  
-   创建日志文件，其中包含有关来自不同计算机的各种对象的数据。  
  
-   将其他现有日志文件中的选定区域追加到一个文件上，以形成长期存档。  
  
-   查看当前活动报告，或从现有日志文件创建报告。  
  
-   保存各个图表、警报、日志或报告设置或整个工作空间设置，以备再次使用。  
  
    > [!NOTE]  
    >  在 Windows NT 4.0 之后，系统监视器取代了性能监视器。 既可以使用系统监视器也可以使用性能监视器来执行这些任务。  
  
## <a name="system-monitor-performance"></a>系统监视器性能  
 当监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 Microsoft Windows 操作系统以调查与性能有关的问题时，请首先注意以下三个主要方面：  
  
-   磁盘活动  
  
-   处理器利用率  
  
-   内存使用率  
  
 监视运行系统监视器的系统会轻微地影响计算机性能。 因此，要么将系统监视器数据记录到另一个磁盘或计算机上，以便减少对所监视计算机的影响，要么从远程计算机上运行系统监视器。 只监视您感兴趣的计数器。 如果监视的计数器过多，将会增加监视过程中使用的资源开销，并影响所监视计算机的性能。  
  
## <a name="system-monitor-tasks"></a>系统监视器任务  
  
|任务说明|主题|  
|----------------------|-----------|  
|描述何时使用系统监视器，并讨论使用系统监视器时的性能开销。|[运行系统监视器](../../relational-databases/performance-monitor/run-system-monitor.md)|  
|描述如何监视磁盘计数器，以便确定其 SQL Server 组件生成的磁盘活动和 I/O 量。|[监视磁盘用量](../../relational-databases/performance-monitor/monitor-disk-usage.md)|  
|描述如何监视 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，以便确定 CPU 使用率是否在正常范围内。|[监视 CPU 用量](../../relational-databases/performance-monitor/monitor-cpu-usage.md)|  
|描述如何监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，以便确认内存使用量是否在正常范围内。|[监视内存用量](../../relational-databases/performance-monitor/monitor-memory-usage.md)|  
|描述如何创建一个在达到系统监视器计数器的阈值时发出的警报。|[创建 SQL Server 数据库警报](../../relational-databases/performance-monitor/create-a-sql-server-database-alert.md)|  
|描述如何创建图表、警报、日志和报表，以便监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。|[创建图表、警报、日志和报告](../../relational-databases/performance-monitor/create-charts-alerts-logs-and-reports.md)|  
|列出系统监视器用来在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的计算机中监视活动的对象和计数器。|[使用 SQL Server 对象](../../relational-databases/performance-monitor/use-sql-server-objects.md)|  
|列出系统监视器用于监视内存中 OLTP 活动的对象和计数器。|[SQL Server XTP（内存中 OLTP）性能计数器](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)|  
  
  
