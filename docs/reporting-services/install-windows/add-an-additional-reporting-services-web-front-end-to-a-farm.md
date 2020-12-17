---
description: 向场中添加另一个 Reporting Services Web 前端
title: 向场中添加另一个 Reporting Services Web 前端 | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
ms.assetid: d7a11bda-ae26-49ac-b071-37d83cae5afe
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 914e4fb3254458579e20b81d8ef079ad2fed592b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472448"
---
# <a name="add-an-additional-reporting-services-web-front-end-to-a-farm"></a>向场中添加另一个 Reporting Services Web 前端
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式包括应用程序服务器和 Web 前端 (WFE) 服务器所需的组件。 本主题主要介绍如何为 WFE 服务器安装所需的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 组件，包括 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 功能（例如订阅、数据警报和 [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)]）使用的应用程序页。 WFE 所需的主要 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装是安装用于 SharePoint 2016 产品的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序。  
  
## <a name="prerequisites"></a>必备条件  
  
-   您必须是本地管理员才能运行 SQL Server 安装程序。  
  
-   必须将计算机加入到域中。  
  
-   您需要知道承载 SharePoint 配置和内容数据库的现有数据库服务器的名称。  
  
-   该数据库服务器必须配置为允许远程数据库连接。  如果没有这样配置，将无法将新的服务器连接到场，因为这个新服务器将无法建立与 SharePoint 配置数据库的连接。  
  
-   新服务器将需要安装有与当前场服务器正在运行的 SharePoint 相同的版本。 例如，如果该场已安装了 SharePoint 2013 Service Pack 1 (SP1)，则你还需要在新的服务器上安装 SP1，然后才能将其联接到该场。  
  
## <a name="steps"></a>步骤  
 本主题中的步骤假定 SharePoint 场管理员正在安装和配置服务器。 下图说明一个典型的三层环境，下面的列表中将说明图中的编号项：  
  
-   (1) 多个 Web 前端 (WFE) 服务器。 WFE 服务器要求用于 SharePoint 2010 的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序。 以下步骤将第二个应用程序服务器添加到这一层。  
  
-   (2) 运行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 和网站的两个应用程序服务器，例如管理中心。  
  
-   (3) 两个 SQL Server 数据库服务器。  
  
-   (4) 表示软件或硬件的网络负载平衡解决方案 (NLB)  
  
 ![将 SSRS 添加到新 SharePoint WFE](../../reporting-services/install-windows/media/rs-sharepointscale-wfe.gif "将 SSRS 添加到新 SharePoint WFE")  
  
 下面的步骤假定管理员正在安装和配置服务器。  
  
|步骤|说明和链接|  
|----------|--------------------------|  
|将 SharePoint 服务器添加到场。|你需要安装 SharePoint 以部署其他 Reporting Services 应用程序。<br/><br/>有关 SharePoint 2013 的详细信息，请参阅 [在 SharePoint 2013 中将 SharePoint 服务器添加到场](/SharePoint/install/add-web-or-application-server-to-the-farm)。<br/><br/>有关 SharePoint 2016 的详细信息，请参阅 [在 SharePoint 2016 中将 SharePoint 服务器添加到场](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm)。|  
|安装用于 SharePoint 2016 产品的 SQL Server Reporting Services 外接程序。|有几种方法可安装外接程序。 以下步骤使用 SQL Server 安装向导。 有关安装该外接程序的详细信息，请参阅 [安装或卸载用于 SharePoint 的 Reporting Services 外接程序](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)<br /><br /> 1) 运行 SQL Server 安装。<br /><br /> 2) 在“安装角色”页上，选择“SQL Server 功能安装”<br /><br /> 3) 在“功能选择”页上，选择“用于 SharePoint 产品的 Reporting Services 外接程序”<br /><br /> 4) 在随后出现的多个页面上，单击“下一步”，完成安装选项。<br /><br/>若要详细了解如何安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]，请参阅[在 SharePoint 模式下安装第一个报表服务器](install-the-first-report-server-in-sharepoint-mode.md)|  
|验证新服务器是否正常运行。|1) 在 SharePoint 管理中心的“系统设置”组中，单击“管理此场中的服务器”   。<br /><br /> 2) 验证列表中是否包含新的服务器。|  
|更新您的 NLB 解决方案。|如果需要，更新您的硬件或软件的 NLB 环境，以便包括新的服务器。|  

## <a name="next-steps"></a>后续步骤

[在 SharePoint 2016 中将 SharePoint 服务器添加到场](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm)  
[在 SharePoint 2013 中将 SharePoint 服务器添加到场](/SharePoint/install/add-web-or-application-server-to-the-farm)

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)