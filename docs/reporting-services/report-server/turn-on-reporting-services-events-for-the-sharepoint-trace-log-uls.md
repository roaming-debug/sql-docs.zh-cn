---
title: 为 SharePoint 跟踪日志 (ULS) 启用 Reporting Services 事件 | Microsoft Docs
description: 了解如何为 SharePoint 模式下的 Reporting Services 服务器启用将 Reporting Services 事件写入 SharePoint ULS 跟踪日志。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: 81110ef6-4289-405c-a931-e7e9f49e69ba
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7ce362356424cb0fdaf991faa27d90f10ef36899
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597132"
---
# <a name="turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls"></a>为 SharePoint 跟踪日志 (ULS) 启用 Reporting Services 事件

  从 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]开始，SharePoint 模式下的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务器可以将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 事件写入 SharePoint 统一日志记录服务 (ULS) 跟踪日志。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的类别在 SharePoint 管理中心的“监视”页上提供。  
  
 本主题内容：  
  
-   [一般 ULS 日志建议](#bkmk_general)  
  
-   [在 Reporting Services 类别中打开和关闭 Reporting Services 事件](#bkmk_turnon)  
  
-   [建议配置](#bkmk_recommended)  
  
-   [读取日志条目](#bkmk_readentries)  
  
-   [SQL Server Reporting Services 事件列表](#bkmk_list)  
  
-   [使用 PowerShell 查看日志文件](#bkmk_powershell)  
  
-   [跟踪日志位置](#bkmk_trace)  
  
##  <a name="general-uls-log-recommendations"></a><a name="bkmk_general"></a> 一般 ULS 日志建议  
 下表列出了监视 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 环境的推荐事件类别和级别。 记录事件时，每一项都包括记录事件的时间、进程名和线程 ID。  
  
|类别|级别|说明|  
|--------------|-----------|-----------------|  
|数据库|“详细”|记录涉及数据库访问的事件。|  
|常规|“详细”|记录涉及访问以下各项的事件：<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 网页<br /><br /> 报表查看器 HTTP 处理程序<br /><br /> 报表访问（.rdl 文件）<br /><br /> 数据源（.rsds 文件）<br /><br /> SharePoint 网站上的 URL（.smdl 文件）|  
|Office Server 常规|异常|记录登录失败。|  
|拓扑|“详细”|记录当前用户信息。|  
|Web 部件|“详细”|记录涉及访问报表查看器 Web 部件的事件。|  
  
##  <a name="to-turn-on-and-off-reporting-services-events-in-the-reporting-services-category"></a><a name="bkmk_turnon"></a> 在 Reporting Services 类别中打开和关闭 Reporting Services 事件  
  
1.  从 SharePoint 管理中心  
  
2.  单击 **“监视”** 。  
  
3.  在 **“报告”** 组中单击 **“配置诊断日志记录”** 。  
  
4.  在类别列表中找到 **SQL Server Reporting Services** 。  
  
5.  单击加号 (+) 以展开 **SQL Server Reporting Services** 下的子类别。  
  
6.  选择要添加到跟踪日志中的子类别。  
  
7.  在类别列表的底部，为 **“要报告给跟踪日志的严重程度最低的事件”** 选择一个事件级别。 选择 **“无”** 以禁用跟踪。  
  
> [!NOTE]  
>  **不支持选项** “要报告给事件日志的严重程度最低的事件” [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]。 已忽略该选项。  
  
##  <a name="recommended-configuration"></a><a name="bkmk_recommended"></a> 建议配置  
 以下日志记录选项建议用作标准配置：  
  
-   **HTTP 重定向程序**  
  
-   **SOAP 客户端代理**  
  
-   如果您遇到配置问题，则添加 **“配置页”** 。  
  
 您可以使用以下 PowerShell cmdlet 检查所有当前的场诊断日志设置：  
  
```  
Get-SPDiagnosticConfig  
```  
  
##  <a name="reading-the-logs-entries"></a><a name="bkmk_readentries"></a> 读取日志条目  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 日志中的条目采用以下方式进行格式化。  
  
1.  **产品：SQL Server Reporting Services**  
  
2.  **类别：** 与服务器相关的事件将在名称的开头带有字符“Report Server”。 例如“Report Server Alerting Runtime”，这些事件还将记录到报表服务器日志文件。  
  
3.  **类别：** 与 Web 前端组件相关或从中进行通信的事件不包含“Report Server”。 例如“服务应用程序代理”、“报表服务器警报运行时”。 WFE 条目包含 CorrelationID，而服务器条目不包含。  
  
##  <a name="list-of-sql-server-reporting-services-events"></a><a name="bkmk_list"></a> SQL Server Reporting Services 事件列表  
 下表是 SQL Server Reporting Services 类别中事件的列表：  
  
|区域名称|说明或示例条目|  
|---------------|-----------------------------------|  
|“配置页”||  
|HTTP 重定向程序||  
|本地模式处理||  
|本地模式呈现||  
|SOAP 客户端代理||  
|UI 页||  
|Power View|写入 **LogClientTraceEvents** API 的日志条目。 这些条目来源于客户端应用程序，包括 Power View（SQL Server Reporting Services 加载项的功能）。<br /><br /> 来自 LogClientTraceEvents API 的所有日志条目将记录在“SQL Server Reporting Services”的“类别”和“Power View”的“区域”下。<br /><br /> 使用“Power View”区域记录的条目内容由客户端应用程序决定。|  
|报表服务器警报运行时||  
|报表服务器应用程序域管理器||  
|报表服务器缓冲响应||  
|报表服务器缓存||  
|报表服务器目录||  
|报表服务器块区||  
|报表服务器清除||  
|报表服务器配置管理器|示例条目：<br /><br /> MediumUsing 报表服务器内部 URL `https://localhost:80/ReportServer`。<br /><br /> UnexpectedMissing or Invalid ExtendedProtectionLevel setting|  
|报表服务器 Crypto||  
|报表服务器数据扩展插件||  
|报表服务器数据库轮询||  
|报表服务器默认值||  
|报表服务器电子邮件扩展插件||  
|报表服务器 Excel 呈现器||  
|报表服务器扩展插件工厂||  
|报表服务器 HTTP 运行时||  
|报表服务器图像呈现器||  
|报表服务器内存监视||  
|报表服务器通知||  
|报表服务器处理||  
|报表服务器提供程序||  
|报表服务器呈现||  
|报表服务器报表预览||  
|报表服务器资源实用工具|示例条目：<br /><br /> MediumReporting Services starting SKU:计算<br /><br /> MediumEvaluation copy:180 days left|  
|报表服务器运行作业||  
|报表服务器运行请求||  
|报表服务器计划||  
|报表服务器安全性||  
|报表服务器服务控制器||  
|报表服务器会话||  
|报表服务器订阅||  
|报表服务器 WCF 运行时||  
|报表服务器 Web 服务||  
|服务应用程序代理||  
|共享服务|示例条目：<br /><br /> MediumUpdating ReportingWebServiceApplication<br /><br /> MediumGranting access to content databases.<br /><br /> MediumProvisioning instances for ReportingWebServiceApplication<br /><br /> MediumProcessing service account change for ReportingWebServiceApplication<br /><br /> MediumSetting database permissions.|  
  
##  <a name="view-a-log-file-with-powershell"></a><a name="bkmk_powershell"></a> 使用 PowerShell 查看日志文件  
 ![PowerShell 相关内容](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell 相关内容")可以使用 PowerShell 从 ULS 日志文件中返回 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 相关事件列表。 从 SharePoint 2010 Management Shell 运行以下命令以便从该文件（ULS 日志文件 UESQL11SPOINT-20110606-1530.log）中返回包含“sql server reporting services”的行的筛选后列表：  
  
```  
Get-content -path "C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\14\LOGS\UESQL11SPOINT-20110606-1530.log" | select-string "sql server reporting services"  
```  
  
 具有可以下载的工具，通过这些工具可以读取 ULS 日志。 例如，GitHub 上提供的 [SharePoint 日志查看器](https://github.com/hasankhan/SharePointLogViewer)。 
  
 有关如何使用 PowerShell 查看日志数据的详细信息，请参阅 [查看诊断日志 (SharePoint Server 2010)](/SharePoint/administration/view-diagnostic-logs)  
  
##  <a name="trace-log-location"></a><a name="bkmk_trace"></a> 跟踪日志位置  
 跟踪日志文件通常位于文件夹 **c:\Program Files\Common files\Microsoft Shared\Web Server Extensions\14\logs** 中，但您可以从 SharePoint 管理中心的 **“诊断日志记录”** 页中验证或更改此路径。  
  
 有关在 SharePoint 2010 管理中心配置 SharePoint 服务器上的诊断日志记录的详细信息和步骤，请参阅 [配置诊断日志记录设置 (Windows SharePoint Services)](/previous-versions/office/sharepoint-2007-products-and-technologies/cc288649(v=office.12))。  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)