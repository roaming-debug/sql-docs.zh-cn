---
description: 向场中添加另一个报表服务器（SSRS 扩展）
title: 向场中添加另一个报表服务器（SSRS 横向扩展）| Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
ms.assetid: c1a6b683-15cf-44ae-ac60-ceee63a60aaf
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: e0f1807d8050408ca7e6153d9f7b9189b004bc8a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101968"
---
# <a name="add-an-additional-report-server-to-a-farm-ssrs-scale-out"></a>向场中添加另一个报表服务器（SSRS 扩展）

  将第二个或更多的 SharePoint 模式报表服务器添加到您的 SharePoint 场可改进报表服务器处理的性能和响应时间。 如果您在将更多的用户、报表和其他应用程序添加到报表服务器时发现性能下降，则添加其他报表服务器可改进性能。 在存在硬件问题或者您在对环境中的单独服务器执行一般性的维护时，也建议添加第二个报表服务器以便提高报表服务器的可用性。 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 版本开始，用于在 SharePoint 模式中扩展 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 环境的步骤遵循标准 SharePoint 场部署并且利用 SharePoint 负载平衡功能。  
  
> [!IMPORTANT]  
>  并非所有 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 版本都支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的扩展。 有关详细信息，请参阅 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]SQL Server 各个版本支持的功能[中的 ](~/sql-server/editions-and-components-of-sql-server-2017.md#SSRS) 部分。  
  
> [!TIP]  
>  从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，您将不使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器添加服务器和扩展报表服务器。 将带有 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务的 SharePoint 服务器添加到场中时，SharePoint 产品管理 Reporting Services 的扩展。  
  
 有关如何扩展本机模式报表服务器的信息，请参阅[配置本机模式报表服务器横向扩展部署（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)。  
  
##  <a name="load-balancing"></a><a name="bkmk_loadbalancing"></a> 负载平衡  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序的负载平衡将由 SharePoint 自动管理，除非您的环境具有自定义或第三方负载平衡解决方案。 默认 SharePoint 负载平衡行为是，每个 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序都将在您启动了 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务的所有应用程序服务器之间保持平衡。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 若要确认  服务是否安装和启动，请在 SharePoint 管理中心中单击“管理服务器上的服务”。  
  
##  <a name="prerequisites"></a><a name="bkmk_prerequisites"></a>先决条件  
  
-   您必须是本地管理员才能运行 SQL Server 安装程序。  
  
-   必须将计算机加入到域中。  
  
-   您需要知道承载 SharePoint 配置和内容数据库的现有数据库服务器的名称。  
  
-   该数据库服务器必须配置为允许远程数据库连接。  如果没有这样配置，将无法将新的服务器连接到场，因为这个新服务器将无法建立与 SharePoint 配置数据库的连接。  
  
-   新服务器将需要安装有与当前场服务器正在运行的 SharePoint 相同的版本。 例如，如果该场已安装了 SharePoint 2013 Service Pack 1 (SP1)，则你还需要在新的服务器上安装 SP1，然后才能将其联接到该场。  
  
##  <a name="steps"></a><a name="bkmk_steps"></a> 步骤  
 本主题中的步骤假定 SharePoint 场管理员正在安装和配置服务器。 下图说明一个典型的三层环境，下面的列表中将说明图中的编号项：  
  
-   (1) 多个 Web 前端 (WFE) 服务器。 WFE 服务器需要用于 SharePoint 2016 的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序。  
  
-   (2) 运行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 和网站的单个应用程序服务器，例如管理中心。 以下步骤将第二个应用程序服务器添加到这一层。  
  
-   (3) 两个 SQL Server 数据库服务器。  
  
-   (4) 表示软件或硬件的网络负载平衡解决方案 (NLB)  
  
 ![添加 Reporting Services 应用程序服务器](../../reporting-services/install-windows/media/rs-sharepointscale.gif "添加 Reporting Services 应用程序服务器")  
  
 下面的步骤假定管理员正在安装和配置服务器。 服务器将设置为场中的新的应用程序服务器，并且不用作 Web 前端 (WFE)。  
  
|步骤|说明和链接|  
|----------|--------------------------|  
|将 SharePoint 服务器添加到场。|你需要安装 SharePoint，以部署其他 Reporting Services 应用程序。<br/><br/>有关 SharePoint 2013 的详细信息，请参阅 [在 SharePoint 2013 中将 SharePoint 服务器添加到场](/SharePoint/install/add-web-or-application-server-to-the-farm)。<br/><br/>有关 SharePoint 2016 的详细信息，请参阅 [在 SharePoint 2016 中将 SharePoint 服务器添加到场](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm)。|  
|安装和配置 Reporting Services SharePoint 模式。|运行 SQL Server 安装。 有关安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式的详细信息，请参阅[在 SharePoint 模式下安装第一个报表服务器](install-the-first-report-server-in-sharepoint-mode.md)<br /><br /> 如果该服务器将仅用作应用程序服务器并且将不用作 WFE，则无需选择“用于 SharePoint 产品的 Reporting Services 外接程序”  。<br /><br /> 1) 在“设置角色”页上，选择“SQL Server 功能安装”  <br /><br /> 2) 在“功能选择”页上，选择“Reporting Services - SharePoint”  <br /><br /> 3) 在“Reporting Services 配置”页上，确认为“Reporting Services SharePoint 模式”选择了“仅安装”选项    。|  
|验证 Reporting Services 是否正常运行。|1) 在 SharePoint 管理中心的“系统设置”组中，单击“管理此场中的服务器”   。<br /><br /> 2) 验证“SQL Server Reporting Services 服务”服务  。<br /><br />有关详细信息，请参阅 [Verify a Reporting Services Installation](../../reporting-services/install-windows/verify-a-reporting-services-installation.md)|  
  
##  <a name="additional-configuration"></a><a name="bkmk_additional"></a> 附加配置  
 可以优化扩展部署中的单个 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务器以仅执行后台处理，从而不与交互式报表执行争用资源。 后台处理包括计划、订阅和数据警报。  
  
 若要更改单个 Report Server 的行为，请在 RSreportServer.config 配置文件中将 \<IsWebServiceEnable> 设为 false 。  
  
 默认情况下，配置 Report Server 时 \<IsWebServiceEnable> 设为 TRUE。 当所有服务器都配置为 TRUE 时，将在场中的所有节点上均衡交互式操作和后台处理的负载。  
  
 如果配置所有 Report Server 时 \<IsWebServiceEnable> 设为 False，则在尝试使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 功能时将看到类似以下内容的一条错误消息：  
  
```output
The Reporting Services Web Service is not enabled. Configure at least one instance of the Reporting Services SharePoint Service to have <IsWebServiceEnable> set to true.
```
 
 有关详细信息，请参阅[修改 Reporting Services 配置文件 (RSreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)  

## <a name="next-steps"></a>后续步骤

[在 SharePoint 2016 中将 SharePoint 服务器添加到场](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm)  
[在 SharePoint 2013 中将 SharePoint 服务器添加到场](/SharePoint/install/add-web-or-application-server-to-the-farm)

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)