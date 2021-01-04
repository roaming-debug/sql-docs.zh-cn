---
title: 性能计数器 - ReportServer 服务，性能对象 | Microsoft Docs
description: 了解作为 SQL Server 2012 部署的一部分的 ReportServer:Service 和 ReportServerSharePoint:Service 性能对象的性能计数器。
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Report Server service, performance counters
ms.assetid: 2bcacab2-3a4f-4aae-b123-19d756b9b9ed
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 8224c126d9f23cb5f13c517400ec79e04d127ff5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466608"
---
# <a name="performance-counters---reportserver-service--performance-objects"></a>性能计数器 - ReportServer 服务，性能对象
  该主题描述了作为 **部署的一部分的** ReportServer:Service **和** ReportServerSharePoint:Service [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 性能对象的性能计数器。  
  
> [!NOTE]  
>  性能对象用于监视本地报表服务器上的事件。 如果是在扩展部署中运行报表服务器，则只对当前服务器（而不是整个扩展部署）进行计数。  
  
 Windows 性能监视器 (**Perfmon.exe**) 中提供了性能对象。 有关详细信息，请参阅 Windows 文档。 [运行时分析](/dotnet/framework/debug-trace-profile/runtime-profiling) (https://msdn.microsoft.com/library/w4bz2147.aspx) 。  
  
 本主题内容：  
  
-   [ReportServer:Service 性能计数器（本机模式报表服务器）](#bkmk_ReportServer)  
  
-   [ReportServerSharePoint:Service（SharePoint 模式报表服务器）](#bkmk_ReportServerSharePoint)  
  
-   [使用 PowerShell Cmdlet 返回列表](#bkmk_powershell)  
  
 [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
##  <a name="reportserverservice-performance-counters-native-mode-report-server"></a><a name="bkmk_ReportServer"></a> ReportServer:Service 性能计数器（本机模式报表服务器）  
 **ReportServer:Service** 性能对象包含一个计数器集合，用于跟踪报表服务器实例的与 HTTP 相关的事件以及与内存相关的事件。 此性能对象对计算机上的每个 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 实例显示一次，可以在每个实例的性能对象中添加或删除计数器。 默认实例的计数器以 **ReportServer:Service** 格式显示。 命名实例的计数器以 *ReportServer$\<**_instance_name_*_>:Service_ 格式显示。  
  
 **ReportServer:Service** 性能对象是 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中的新增功能，它提供 Internet Information Services (IIS) 和以前版本的 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 中的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 所含的计数器的子集。 这些新计数器是特定于 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]，用于跟踪报表服务器中与 HTTP 相关的事件，例如请求、连接和登录尝试。 此外，此性能对象还包括用于跟踪内存管理事件的计数器。  
  
 下表列出了 **ReportServer:Service** 性能对象中包含的计数器。  
  
 ![与 PowerShell 相关的内容](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell 相关内容") 下面的 Windows PowerShell 脚本将返回 CounterSetName 的性能计数器列表  
  
```  
(get-counter -listset "ReportServer:Service").paths  
```  
  
|计数器|说明|  
|-------------|-----------------|  
|**活动连接**|在服务器上当前活动的连接数。|  
|**接收的字节总数**|服务器接收的字节数。 该计数器计数报表管理器和报表服务器接收的原始字节总数。|  
|**Bytes Received/sec**|服务器每秒接收的字节数。 该计数器只在传输完成时更新。 这意味着此计数器开始保持为 0，传输完成后值会增加。|  
|**发送的字节总数**|从服务器发送的字节数。 该计数器计数报表管理器和报表服务器发送的原始字节总数。|  
|**Bytes Sent/sec**|从服务器每秒发送的字节数。 该计数器只在传输完成时更新。 这意味着此计数器开始保持为 0，传输完成后值会增加。|  
|**错误总数**|处理 HTTP 请求过程中发生的错误总数。 这些错误包括 400s 和 500s 形式的 HTTP 状态代码。|  
|**Errors/sec**|处理 HTTP 请求过程中每秒发生的错误总数。 这些错误包括 400s 和 500s 形式的 HTTP 状态代码。|  
|**登录尝试总数**|从 RSWindows 身份验证类型进行的登录尝试次数。 RSWindows 身份验证类型包括 RSWindowsNegotiate、RSWindowsNTLM、RSWindowsKerberos 和 RSWindowsB asic。 值为零 (0) 表示自定义身份验证。|  
|**每秒登录尝试数**|登录尝试的频率。|  
|**登录成功总数**|RSWindows 身份验证类型的成功登录次数。 RSWindows 身份验证类型包括 RSWindowsNegotiate、RSWindowsNTLM、RSWindowsKerberos 和 RSWindowsB asic。 值为零 (0) 表示自定义身份验证。|  
|**每秒登录成功数**|登录成功率。|  
|**内存压力状态**|以下从 1-5 的数字，指示服务器的当前内存状态：<br /><br /> 1：没有压力<br /><br /> 2:低压力<br /><br /> 3：中等压力<br /><br /> 4：高压力<br /><br /> 5：过度压力|  
|**内存收缩量**|服务器请求缩小使用中内存的字节数。|  
|**每秒内存收缩通知数**|服务器上一秒内发出的缩小使用中内存的通知数。 该值表示服务器经历内存压力的频率。|  
|**断开连接的请求数**|由于通信故障而断开连接的请求数。|  
|**执行的请求数**|当前正在处理的请求数。|  
|**未授权的请求数**|失败并返回 HTTP 401 状态代码的请求数。|  
|**拒绝的请求数**|由于服务器资源不足而未处理的请求总数。 该计数器表示返回 HTTP 503 状态代码（指示服务器太忙）的请求数。|  
|**请求总数**|报表服务器服务启动后接收到的请求总数。 此计数器计数发送给报表管理器的请求数以及从报表管理器发送给报表服务器的请求数。|  
|**Requests/sec**|每秒处理的请求数。 该值表示应用程序的当前吞吐量。|  
|**排队的任务**|等待线程变为可供处理的任务数。 向报表服务器发出的每个请求都与一个或多个任务对应。 此计数器只表示可以处理的任务数量，不包括当前正在运行的任务数量。|  
  
##  <a name="reportserversharepointservice-sharepoint-mode-report-server"></a><a name="bkmk_ReportServerSharePoint"></a> ReportServerSharePoint:Service（SharePoint 模式报表服务器）  
 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中添加了 ReportServerSharePoint:Service 性能对象。  
  
 ![与 PowerShell 相关的内容](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell 相关内容") 下面的 Windows PowerShell 脚本将返回 CounterSetName 的性能计数器列表  
  
```  
(get-counter -listset "ReportServerSharePoint:Service").paths  
```  
  
|计数器|说明|  
|-------------|-----------------|  
|**内存压力状态**||  
|**内存收缩量**||  
|**Memory Shrink Notifications/Sec**||  
  
##  <a name="use-powershell-cmdlets-to-return-lists"></a><a name="bkmk_powershell"></a> 使用 PowerShell Cmdlet 返回列表  
 ![与 PowerShell 相关的内容](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell 相关内容") 下面的 Windows PowerShell 脚本将返回 CounterSetName“ReportServerSharePoint:Service”的性能计数器列表：  
  
```  
(get-counter -listset "ReportServerSharePoint:Service").paths  
```  
  
## <a name="see-also"></a>另请参阅  
 [监视报表服务器性能](../../reporting-services/report-server/monitoring-report-server-performance.md)   
 [MSRS 2011 Web Service 和 MSRS 2011 Windows Service 性能对象的性能计数器（本机模式）](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)   
 [MSRS 2011 Web Service SharePoint Mode 性能对象和 MSRS 2011 Windows Service SharePoint Mode 性能对象的性能计数器（SharePoint 模式）](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)  
  
