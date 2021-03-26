---
title: 排查 Reporting Services 报表问题
description: 本文介绍如何解决在本机或 SharePoint 模式下设计、预览和导出报表，以及将报表发布到报表服务器或在报表服务器上查看报表时遇到的问题。
ms.date: 02/27/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
ms.assetid: a705d103-85b1-49b5-b27f-332b1040d029
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8812cfc8ac70da47da0414534ff380dd07686037
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104792947"
---
# <a name="troubleshoot--reporting-services-report-issues"></a>排查 Reporting Services 报表问题
本主题可以帮助解决在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion.md)] 报表设计、预览报表、将报表发布到本机模式或 SharePoint 集成模式下的报表服务器、在报表服务器上查看报表或以其他文件格式导出报表时遇到的问题。  
## <a name="monitor-report-servers"></a>监视报表服务器  
可以使用系统和数据库工具来监视报表服务器活动。 还可以查看报表服务器跟踪日志文件，或在报表服务器执行日志中查询有关特定报表的详细信息。 如果正在使用性能监视器，则可以为报表服务器 Web 服务和 Windows 服务添加性能计数器来标识按需或计划处理中的瓶颈。  
有关详细信息，请参阅 [监视报表服务器性能](../report-server/monitoring-report-server-performance.md)。  
  
  
## <a name="view-the-report-server-logs"></a>查看报表服务器日志  
[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion.md)] 会在日志文件中记录很多内部事件和外部事件，此类文件记录有关特定报表、调试信息、HTTP 请求和响应以及报表服务器事件的数据。 您还可以创建性能日志，然后选择性能计数器来指定要收集的数据。 默认安装的日志文件默认目录为 `<drive>\Program Files\Microsoft SQL Server\MSRS130.MSSQLSERVER\Reporting Services\LogFiles`。   
  
有关详细信息，请参阅 [Reporting Services 日志文件和源](../report-server/reporting-services-log-files-and-sources.md)。  
  
若要具体确定是由于数据检索、报表处理还是报表呈现导致报表等待，请使用执行日志。 有关详细信息，请参阅[报表服务器 ExecutionLog 和 ExecutionLog3 视图](../report-server/report-server-executionlog-and-the-executionlog3-view.md)。   
  
## <a name="view-the-call-stack-for-report-processing-error-messages-on-the-report-server"></a>在报表服务器上查看报表处理错误消息的调用堆栈  
在报表管理器中查看已发布的报表时，您可能会看到一条指示常规处理或呈现错误的错误消息。 若要查看详细信息，可以查看调用堆栈。   
  
若要查看调用堆栈，请用本地管理员凭据登录到报表服务器，右键单击“报表管理器”页，然后单击“查看源”  。 调用堆栈会提供有关错误消息的详细上下文。  
  
## <a name="use-ssmanstudiofull-to-verify-queries-and-credentials"></a>使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull.md)] 验证查询和凭据  
在将复杂查询包含在报表中之前，可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull.md)] 进行验证。   
  
有关详细信息，请参阅[数据库引擎查询编辑器](../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md) 和[通过使用对象资源管理器管理对象](../../ssms/object/manage-objects-by-using-object-explorer.md)。  
  
## <a name="analyze-problem-reports-with-report-data-cached-on-the-client"></a>使用客户端上缓存的报表数据分析问题报表  
报表作者在 Business Intelligence Development Studio 中创建报表时，创作客户端会将数据缓存为一个 .rdl 数据文件，预览报表时会使用该文件。 每次查询更改时，缓存都会更新。 有时候为了调试报表问题，防止刷新报表数据会很有用，这样进行调试时数据将不会更改。   
  
若要控制 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull.md)] 是否只能使用缓存数据，请将以下部分添加到 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio.md)]的 devenv.exe.config 中。 默认目录的位置为 `<drive>:Program Files\Microsoft Visual Studio 10.0\Common7\IDE`。   
  
```  
<system.diagnostics>  
      <switches>  
         <add name="Microsoft.ReportDesigner.ReportPreviewStore.ForceCache" value="1" />  
      </switches>  
   </system.diagnostics>  
```  
只要将该值设置为 1，将只使用缓存的报表数据。 请确保在完成报表调试后删除此部分内容。  
  
## <a name="see-also"></a>另请参阅  
[错误和事件 (Reporting Services)](errors-and-events-reference-reporting-services.md)  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect-md.md)]
