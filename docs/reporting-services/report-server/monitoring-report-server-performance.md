---
title: 监视报表服务器性能 | Microsoft Docs
description: 了解如何监视报表服务器性能以评估服务器活动、监视趋势、诊断瓶颈以及收集有关系统配置的数据。
ms.date: 06/20/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- report servers [Reporting Services], performance
- counters [Reporting Services]
- monitoring performance [Reporting Services]
- SQL Server Reporting Services, performance
- performance [Reporting Services]
- Reporting Services, performance
ms.assetid: c1bc13d4-8297-4daf-bb19-4c1e5ba292a6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 12afa9794e4d48e2c7b16620ce845660f61801bc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461408"
---
# <a name="monitoring-report-server-performance"></a>监视报表服务器性能
  使用性能监视工具可监视报表服务器的性能以评估服务器活动，查看趋势，诊断系统瓶颈以及收集可以帮您确定当前系统配置是否充分的数据。 若要优化服务器性能，可指定回收报表服务器应用程序域的频率。 有关详细信息，请参阅 [为报表服务器应用程序配置可用内存](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)。  
  
## <a name="sources-of-performance-data"></a>性能数据的来源  
 结合使用以下技术和工具，可以获得有关系统运行情况的综合信息。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows Server 操作系统通过以下工具提供性能信息：  
  
-   任务管理器  
  
-   事件查看器  
  
-   性能控制台  
  
 任务管理器提供了有关计算机上运行的程序和进程的信息。 您可以使用任务管理器来监视报表服务器性能的关键指标。 还可以评估运行中进程的活动，以及查看有关 CPU 和内存使用情况的图表及数据。 有关使用任务管理器的信息，请参阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 产品文档。  
  
 您可以使用性能控制台和事件查看器来创建有关报表处理与资源消耗情况的日志和警报。 有关由 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]生成的 Windows 事件的信息，请参阅 [Windows 应用程序日志](../../reporting-services/report-server/windows-application-log.md)。 若要了解性能控制台，请参阅本主题稍后将介绍的“Windows 性能计数器”。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实用工具还提供有关用于缓存和会话管理的报表服务器数据库和临时数据库的信息。  
  
## <a name="windows-performance-counters"></a>Windows 性能计数器  
 通过监视各具体的性能计数器，您可以：  
  
-   估计支持预测的工作负荷所需的系统需求。  
  
-   创建性能基准来度量配置更改或应用程序升级的效果。  
  
-   监视在真实或人为生成的环境中特定负荷下的应用程序性能。  
  
-   验证硬件升级是否具有所需的性能效果。  
  
-   验证对系统配置所做的更改是否具有所需的性能效果。  

::: moniker range=">=sql-server-2017"
  
## <a name="reporting-services-performance-objects"></a>Reporting Services 性能对象  
SQL Server 2016 Reporting Services 或更高版本 (SSRS) 包括以下性能对象：  
  
-   用于监视报表服务器性能的 **MSRS 2011 Web Service** 和 **MSRS 2011 SharePoint Mode Web Service** 。 这些性能对象包括一系列用于跟踪报表服务器处理的计数器，这些处理通常通过交互式报表查看操作启动。 只要 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止报表服务器 Web 服务，这些计数器就会重置。  
  
-   用于监视计划操作和报表传递的 **MSRS 2011 Windows Service** 和 **MSRS 2011 Windows Service SharePoint Mode** to monitor scheduled operations 和 report delivery. 这些性能对象包括一系列用于跟踪报表处理的计数器，这些处理通过计划操作启动。 计划操作包括订阅和传递、报表执行快照以及报表历史记录。  
  
-   ReportServer Service 和 ReportServerSharePoint Service 用于监视与 HTTP 相关的事件和内存管理。 这些计数器是 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 专用，用于为报表服务器跟踪与 HTTP 相关的事件，如请求、连接和登录尝试。 此性能对象还包括与内存管理相关的计数器。  
  
 如果单台计算机上有多个报表服务器实例，则可以同时监视多个实例或分别监视各个实例。 选择要在添加计数器时包括的实例。 有关使用性能控制台 (perfmon.msc) 和添加计数器的详细信息，请参阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 产品文档。  
  
## <a name="other-performance-counters"></a>其他性能计数器  
 自定义 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 性能计数器仅用于 MSRS 2008 Web 服务、MSRS 2008 Windows 服务和 ReportServer 服务。 下列性能对象为报表服务器提供附加的性能监视数据。  
  
|性能对象|说明|  
|------------------------|-----------|  
|**.NET CLR Data** 和 **.NET CLR Memory**|Web 门户使用 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 性能计数器。 有关详细信息，请参阅 MSDN 中的“提高 .NET 应用程序的性能和可伸缩性”。|  
|**处理**|为 ReportingServicesService 实例添加 **Elapsed Time** 和 **ID Process** 性能计数器，以便按进程 ID 跟踪进程运行时间。|  
  
## <a name="sharepoint-events"></a>SharePoint 事件  
 除了 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 性能对象之外，如果您在 SharePoint 集成模式下运行报表服务器并配置了报表环境以使用 SharePoint 产品，您可能还想配置 SharePoint 事件。 在本节中，将使用 SharePoint 集成模式下的报表服务器事件来查看可能提供有用信息（如果您的报表环境与 SharePoint 集成）的诊断事件。  
  
## <a name="in-this-section"></a>在本节中  
 [MSRS 2011 Web Service 和 MSRS 2011 Windows Service 性能对象的性能计数器（本机模式）](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)  
 介绍报表服务器 Web 服务所使用的性能计数器。  
  
 [MSRS 2011 Web Service SharePoint Mode 性能对象和 MSRS 2011 Windows Service SharePoint Mode 性能对象的性能计数器（SharePoint 模式）](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)  
 介绍报表服务器 Windows 服务所使用的性能计数器。  
  
 [ReportServer:Service 和 ReportServerSharePoint:Service 性能对象的性能计数器](../../reporting-services/report-server/performance-counters-reportserver-service-performance-objects.md)  
 介绍 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]中与 HTTP 相关的性能计数器和与内存相关的性能计数器。  

::: moniker-end
  
## <a name="see-also"></a>另请参阅  
 [为报表服务器应用程序配置可用内存](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)   
 [Reporting Services 报表服务器（本机模式）](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [Reporting Services 工具](../../reporting-services/tools/reporting-services-tools.md)  
  