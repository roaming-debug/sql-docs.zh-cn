---
title: 标识执行状态 | Microsoft Docs
description: 了解如何使用 Reporting Services 来标识执行状态，以便可以通过多种方式与报表进行交互。
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-soap-headers
ms.topic: reference
helpviewer_keywords:
- session states [Reporting Services]
- lifetimes [Reporting Services]
- sessions [Reporting Services]
- SessionHeader SOAP header
ms.assetid: d8143a4b-08a1-4c38-9d00-8e50818ee380
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: fbd189a85121d2ef18c69a1d7886f6b3c3294978
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045056"
---
# <a name="identifying-execution-state"></a>标识执行状态
  超文本传输协议 (HTTP) 是一个无连接且无状态协议，这意味着它不自动指示不同请求是否来自同一个客户端，甚至也不指示单个浏览器实例是否仍在查看页面或站点。 会话创建逻辑连接，以通过 HTTP 在服务器与客户端之间维护状态。 与特定会话相关的用户特定的信息称为会话状态。  
  
 会话管理涉及将 HTTP 请求与从同一个会话生成的其他先前请求相关。 如果没有会话管理，则由于 HTTP 协议的无连接和无状态性质，因此这些请求将与报表服务器 Web 服务无关。  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 不公开会话状态的总体概念，例如，由 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 公开的这类概念。 然而，当执行报表时，报表服务器以 execution 的形式维护方法调用之间的状态  。 执行允许用户通过多种方式与报表交互 - 包括从报表服务器加载报表，为报表设置凭据和参数，以及呈现报表。  
  
 当客户端与报表服务器通信时，它们使用执行来管理报表查看和用户在报表中导航到其他页的过程，以及显示或隐藏报表的各个部分。 对于客户端应用程序正在运行的每个报表，都存在一个唯一执行。  
  
 通常，当用户导航到浏览器或客户端应用程序并选择要查看的报表时，执行的生存期开始。 在收到最后一个执行请求后的很短超时时段（默认超时值为 20 分钟）之后，将放弃执行。  
  
 从 Web 服务角度来看，当调用报表服务器 Web 服务 <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A>、<xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A> 或 <xref:ReportExecution2005.ReportExecutionService.Render%2A> 方法时，生存期开始。 应用程序可以使用其他方法来控制处于活动状态的执行（例如，设置参数和设置数据源）。 在收到最后一个执行请求后的很短超时时段（默认超时值为 20 分钟）之后，将放弃执行。  
  
 应用程序通过保存 <xref:ReportExecution2005.ReportExecutionService.Render%2A>（从 <xref:ReportExecution2005.ReportExecutionService.RenderStream%2A> 和 <xref:ReportExecution2005.ExecutionHeader.ExecutionID%2A> 方法的 SOAP 标头中返回），对针对 Web 服务 <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A> 和 <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A> 方法的调用之间的多个处于活动状态的执行保持跟踪。  
  
 下图显示报表的处理和呈现路径。  
  
 ![报表处理/呈现路径](../../reporting-services/report-server-web-service-net-framework-soap-headers/media/rs-render-process-diagram.gif "报表处理/呈现路径")  
  
 为了支持上面介绍的函数，当前 SOAP Render 方法已被拆分为多个方法，其中包括执行初始化阶段、处理阶段和呈现阶段。  
  
 若要以编程方式呈现报表，您必须：  
  
-   使用 <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A> 或 <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A> 加载报表或报表定义。  
  
-   通过检查由 <xref:ReportExecution2005.ExecutionInfo.CredentialsRequired%2A> 或 <xref:ReportExecution2005.ExecutionInfo.ParametersRequired%2A> 返回的 <xref:ReportExecution2005.ExecutionInfo> 对象的 <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A> 和 <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A> 属性，检查报表是否需要凭据或参数。  
  
-   如果需要，则使用 <xref:ReportExecution2005.ReportExecutionService.SetExecutionCredentials%2A> 和 <xref:ReportExecution2005.ReportExecutionService.SetExecutionParameters%2A> 方法设置凭据和/或参数。  
  
-   调用 <xref:ReportExecution2005.ReportExecutionService.Render%2A> 方法以呈现报表。  
  
 当报表处于会话中时，存储在报表服务器数据库中的基础报表可能发生变化。 例如，报表定义可能发生变化，可能删除或移动报表，用户权限可能变化。 如果报表处于活动的会话中，则它不受对基础报表（也即，存储在报表服务器数据库中的报表）所做更改的影响。  
  
 您还可以使用 URL 访问命令管理报表会话。  
  
## <a name="see-also"></a>另请参阅  
 <xref:ReportExecution2005.ReportExecutionService.Render%2A>   
 [技术参考 (SSRS)](../../reporting-services/technical-reference-ssrs.md)   
 [使用 Reporting Services SOAP 标头](../../reporting-services/report-server-web-service-net-framework-soap-headers/using-reporting-services-soap-headers.md)  
  
  
