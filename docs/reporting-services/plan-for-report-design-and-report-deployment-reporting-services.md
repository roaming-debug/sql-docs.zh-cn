---
title: 报表设计和报表部署的规划 | Reporting Services | Microsoft Docs
description: 了解如何使用 Reporting Services 计划协同工作的报表创作环境和报表服务器环境。
ms.date: 09/12/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 1c1e265e-52a2-4de3-96fd-ca4abae01c02
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: cdaaaa55756860e4b62f90cb7e01c88c63ad4b61
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477058"
---
# <a name="plan-for-report-design-and-report-deployment--reporting-services"></a>报表设计和报表部署的规划 | Reporting Services
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 提供了几种创作和部署分页报表的方法。 了解如何计划协同工作的报表创作环境和报表服务器环境。

本主题概述了 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 组件对报表定义的支持。 报表定义是用报表定义语言 (RDL) 或客户端报表定义语言 (RDLC) 编写的 XML 文件。 每个报表定义都符合位于该文件开头的特定架构版本的要求。  
  
 RDL 文件是在 [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)] 项目的报表设计器以及报表生成器中创作的。 RDLC 文件是使用包括在 [!INCLUDE[vsprvs](../includes/vsprvs-md.md)]中的 ReportViewer 控件创作的。
  
##  <a name="rdl-schema-versions"></a><a name="bkmk_rdl_schema_versions"></a> RDL 架构版本  
 下表列出了可用的每个架构版本及其缩写，这些缩写要在本主题的其余部分通篇使用：  
  
|缩写|架构版本|  
|------------------|--------------------|  
|2016 RDL|`https://schemas.microsoft.com/sqlserver/reporting/2016/01/reportdefinition`|
|2010 RDL|`https://schemas.microsoft.com/sqlserver/reporting/2010/01/reportdefinition`|  
|2008 RDL|`https://schemas.microsoft.com/sqlserver/reporting/2008/01/reportdefinition`|  
|2005 RDL<br /><br /> 2005 RDLC|`https://schemas.microsoft.com/sqlserver/reporting/2005/01/reportdefinition`|  
|2000 RDL|`https://schemas.microsoft.com/sqlserver/reporting/2003/10/reportdefinition`|  
  
 有关 RDL 和 RDL 架构的详细信息，请参阅以下内容：  
  
-   [Microsoft SQL Server XML 架构](https://go.microsoft.com/fwlink/?LinkId=31850)  
  
-   [报表定义语言规范](https://go.microsoft.com/fwlink/?linkid=116865)  
  
-   [报表定义语言 (SSRS)](../reporting-services/reports/report-definition-language-ssrs.md)  
  
 有关 ReportViewer 控件的详细信息，请参阅 [ReportViewer 控件 (Visual Studio)](https://msdn.microsoft.com/library/ms251671.aspx)。  
  
##  <a name="report-server-and-rdl-schema-support"></a><a name="bkmk_report_server_rdl_schema_support"></a> 报表服务器和 RDL 架构支持  
 可以使用以下方法将报表定义文件部署到 [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] 报表服务器：  
  
-   **报表设计器：** 使用 [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)] 中的报表设计器来部署报表。  
  
-   **报表生成器：** 使用报表生成器将报表保存到报表服务器。  
  
-   **Web 门户：** 从 [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] 将报表上传到本机模式报表服务器。  

::: moniker range="=sql-server-2016"
  
-   **SharePoint：** 将报表上传到配置有 SharePoint 模式报表服务器的 SharePoint 站点。  

::: moniker-end
  
-   **以编程方式：** 使用 SOAP API 接口以编程方式将报表发布到报表服务器。 有关详细信息，请参阅 [Report Server Web Service](../reporting-services/report-server-web-service/report-server-web-service.md)。  
  
 下表按报表服务器的版本列出了支持的 rdl 架构版本。  
  
|报表服务器版本|RDL 架构版本|  
|---------------------------|------------------------|  
|SQL Server 2016|2016 RDL<br /><br />2010 RDL<br /><br /> 2008 RDL<br /><br /> 2005 RDL<br /><br /> 2000 RDL
|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]<br /><br /> 或<br /><br /> [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]<br /><br /> 或<br /><br /> [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|2010 RDL<br /><br /> 2008 RDL<br /><br /> 2005 RDL<br /><br /> 2000 RDL|  
|[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)]|2008 RDL<br /><br /> 2005 RDL<br /><br /> 2000 RDL|  
  
 如果您将报表定义上载到报表服务器或者升级包含现有报表的报表服务器，报表服务器将保留原格式的报表定义。 **首次使用时**，报表服务器会将报表服务器数据库中的报表升级到二进制格式，并保留这种格式以便以后查看。 报表定义 (.rdl) 本身不升级。  
  
 可以从报表服务器提取报表定义文件 (.rdl) 的只读副本。 在本机模式报表服务器上，浏览到 [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]，选择报表并单击“下载”。 

::: moniker range="=sql-server-2016"

在 SharePoint 模式部署中，浏览到文档库，选择该报表并单击 **“下载副本”** 。  

::: moniker-end
  
 若要升级报表定义，必须在报表创作环境（例如 SQL Server Data Tools 或报表生成器）中打开报表，然后保存它。  
  
 有关报表升级以及支持的架构版本的详细信息，请参阅 [升级报表](../reporting-services/install-windows/upgrade-reports.md)。  
  
##  <a name="report-authoring-and-deployment-support"></a><a name="bkmk_report_authoring_and_deployment"></a> 报表创作和部署支持  
 报表创作环境为 [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)] 项目中的报表设计器以及报表生成器。 报表创作环境对报表升级、报表设计、在本地模式下预览报表、在报表服务器上预览报表以及报表部署提供各种支持。  
  
 下表汇总了对不同架构版本的报表定义的创作和部署支持：  
  
|创作环境|创作的 RDL 版本|部署 RDL 版本|部署到的报表服务器的版本|  
|---------------------------|--------------------------|------------------------|--------------------------------------|  
|SQL Server 2016 报表生成器|Authors 2016 RDL<br /><br /> 将较旧的 RDL 版本升级到 2016 RDL|2016 RDL|SQL Server 2016|
|SQL Server 2016 Data Tools 中的报表设计器 — Business Intelligence for Microsoft Visual Studio 2015|Authors 2016 RDL<br /><br /> 将较旧的 RDL 版本升级到 2016 RDL|2016 RDL|SQL Server 2016|
|SQL Server 2014 Data Tools 中的报表设计器 — Business Intelligence for Microsoft Visual Studio 2012<br /><br /> 或<br /><br /> SQL Server 2012 Data Tools 中的报表设计器 — Business Intelligence for Microsoft Visual Studio 2012<br /><br /> 或<br /><br /> 包含在 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 中的 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]Data Tools 中的报表设计器。|Authors 2010 RDL<br /><br /> 将较旧的 RDL 版本升级到 2010 RDL|2010 RDL|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]<br /><br /> [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]<br /><br /> [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|  
|[!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] Business Intelligence Development Studio 中的报表设计器|Authors 2010 RDL<br /><br /> 将较旧的 RDL 版本升级到 2010 RDL|2010 RDL|[!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|  
|[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] Business Intelligence Development Studio 中的报表设计器|Authors 2008 RDL<br /><br /> 将较旧的 RDL 版本升级到 2008 RDL|2008 RDL|[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)]|
  
 有关 SQL Server Data Tools (SSDT) 的详细信息，请参阅以下内容：  
  
-   [SQL Server Data Tools 中的部署和版本支持 (SSRS)](../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)  
  
-   [用于 Visual Studio 2015 的 SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md)。  
  
##  <a name="reportviewer-controls"></a><a name="bkmk_reportviewer"></a> ReportViewer 控件  
 [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] ReportViewer 控件可在本地预览模式或远程模式下显示 .rdlc 报表，该控件还可以显示在 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 报表服务器上托管的 .rdl 文件。 下表提供 ReportViewer 控件为进行本地处理 (.rdlc) 支持的 RDL 版本的列表。 [报表服务器和 RDL 架构支持](#bkmk_report_server_rdl_schema_support)一节中总结了服务器端 RDL 支持。  
  
|产品中的 ReportViewer 控件|用于本地预览的 RDL 版本|  
|-------------------------------------|--------------------------------------|  
|[!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2015 <br/><br/>或<br/><br/>[!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2013<br /><br /> 或<br /><br /> [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2012<br /><br /> 或<br /><br /> [!INCLUDE[vs_dev10_long](../includes/vs-dev10-long-md.md)]|2008 RDL|  
|[!INCLUDE[vsprvslong](../includes/vsprvslong-md.md)]<br /><br /> 或<br /><br /> [!INCLUDE[vsOrcas](../includes/vsorcas-md.md)]|2005 RDL|  
  
 有关详细信息，请参阅以下主题：  
  
-   [将 RDLC 文件转换为 RDL 文件](https://msdn.microsoft.com/library/ms252109.aspx)  
  
-   [ReportViewer 控件 (Visual Studio)](https://msdn.microsoft.com/library/ms251671.aspx)  
  
-   [添加并配置 ReportViewer 控件](https://msdn.microsoft.com/library/ms252104.aspx)  
  
## <a name="see-also"></a>另请参阅  
 [报表、报表部件和报表定义（报表生成器和 SSRS）](../reporting-services/report-design/reports-report-parts-and-report-definitions-report-builder-and-ssrs.md)   
 [Reporting Services 工具](../reporting-services/tools/reporting-services-tools.md)   
 [报表定义语言 (SSRS)](../reporting-services/reports/report-definition-language-ssrs.md)  
  
  
