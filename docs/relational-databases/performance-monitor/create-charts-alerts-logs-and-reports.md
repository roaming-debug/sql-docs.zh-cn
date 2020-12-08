---
title: 创建图表、警报、日志和报表 | Microsoft Docs
description: 了解如何使用系统监视器创建图表、警报、日志和报表，以监视 SQL Server 的实例。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- System Monitor [SQL Server], charts and reports
- charts [SQL Server]
- reports [SQL Server]
- reports [SQL Server], creating
- Windows System Monitor [SQL Server], charts and reports
- logs [SQL Server], System Monitor
- System Monitor [SQL Server], logs
- Windows System Monitor [SQL Server], logs
ms.assetid: c9162b37-e5dc-43d1-a3aa-1e9ebc69fecc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3339d77f0515f85853f406bfd4cc318dce2e33ec
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506021"
---
# <a name="create-charts-alerts-logs-and-reports"></a>创建图表、警报、日志和报表
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用系统监视器可以创建图表、警报、日志和报表，以监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。  
  
## <a name="charts"></a>图表  
 图表可以监视所选对象和计数器的当前性能，例如，CPU 使用率或磁盘 I/O。 您可以向图表添加系统监视器对象和计数器的各种组合。 还可以向图表中添加 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 对象和计数器。  
  
 每个图表表示要监视的信息的一个子集。 例如，第一个图表可跟踪内存使用统计信息，第二个图表可跟踪磁盘 I/O 统计信息。  
  
 对于以下任务使用图表将非常有用：  
  
-   分析计算机或应用程序运行缓慢或效率低下的原因。  
  
-   连续监视系统，以找出间歇性性能问题。  
  
-   找出需提高性能的原因。  
  
-   用折线图显示一种趋势。  
  
-   用柱状图显示比较结果。  
  
 图表对于短期、实时监视本地或远程计算机很有用，例如，希望在事件发生时对其进行监视。  
  
## <a name="alerts"></a>警报  
 利用警报，系统监视器可以跟踪特定的事件，并按要求向您通知这些事件。 警报日志可以监视所选计数器和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中对象的实例的性能。 当计数器超过给定值时，日志记录下这一事件的日期和时间。 事件也可产生网络警报。 可在一个事件第一次出现或每次出现时运行一个特定的程序。 例如，一个警报可以向所有系统管理员发送一条网络消息，报告 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的磁盘空间不足。  
  
## <a name="logs"></a>日志  
 日志可以记录选定对象和计算机的当前活动信息，以便日后查看和分析。 可以将多个系统的数据收集到一个日志文件中。 例如，可以创建不同的日志，以积累不同计算机上所选对象的性能信息，供将来进行分析。 可以在一个文件名下保存这些选择，当要创建另一用于比较的、具有相似信息的日志时再次使用它们。  
  
 日志文件提供丰富的信息，用于故障处理和系统规划。 当前活动的图表、警报和报告可提供当前的即时反馈，而使用日志文件则可以对计数器进行长时间跟踪。 这样，便可以更彻底地检查信息和文档系统的性能。  
  
## <a name="reports"></a>报表  
 报表可对选定对象显示不断变化的计数器和实例值。 每个实例的值记录在列中。 可调整报表时间间隔、打印快照和导出数据。 当需要显示原始数据时可使用报表。  
  
 有关创建图表、警报、日志和报表，或 Windows 对象和计数器的详细信息，请参阅 Windows 文档。  
  
## <a name="see-also"></a>另请参阅  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
