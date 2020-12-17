---
title: Reporting Services 网站设置和网站功能（SharePoint 模式）| Microsoft Docs
description: 在 Reporting Services SharePoint 模式下，可以通过“SharePoint 网站设置”页管理一些网站级别自定义功能和网站功能。
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: feabf5c1a0847983af4a10dea5d4c15a5f51199d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461428"
---
# <a name="reporting-services-site-settings-and-site-features-sharepoint-mode"></a>Reporting Services 网站设置和网站功能（SharePoint 模式）

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Reporting Services SharePoint 模式具有几个网站级自定义功能和可以从“SharePoint 网站设置”页管理的网站功能。 这些设置适用于整个网站并影响所有 Reporting Services 服务应用程序。 必须拥有“内容管理员”和“系统管理员”权限才能查看此页。  

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

|网站设置|说明|  
|------------------|-----------------|  
|Reporting Services 网站设置|本主题中介绍了适用于网站的设置。|  
|管理数据警报|管理数据警报功能。|  
|报表服务器文件同步|默认情况下停用的网站级功能。<br /><br /> 在文档库中直接添加或更新文件时，将报表服务器文件（.rdl、.rsds、.smdl、.rsd、.rsc、.rdlx）从 SharePoint 文档库同步到报表服务器。<br /><br /> 有关详细信息，请参阅 [在 SharePoint 管理中心中激活报表服务器文件同步功能](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md)|  
  
## <a name="open-the-reporting-services-site-settings-page"></a>打开 Reporting Services“网站设置”页
  
1.  在 SharePoint 站点的“站点操作”菜单中，选择“站点设置” 。  
  
2.  在 Reporting Services 部分中，选择“Reporting Services 网站设置” 。  
  
## <a name="options-for-reporting-services-site-settings"></a>Reporting Services 网站设置选项
  
|选项|描述|  
|------------|-----------------|  
|**启用 RSClientPrint ActiveX 控件下载**|该控件显示一个自定义打印对话框，它支持其他打印对话框常见的功能，包括打印预览、指定特定页和范围的页面选择、页边距和打印方向等功能。 有关控件的详细信息，请参阅 [Using the RSClientPrint Control in Custom Applications](../../reporting-services/report-server-web-service/net-framework/using-the-rsclientprint-control-in-custom-applications.md)|  
|**启用本地模式下的远程错误**|在本地模式下运行时，在远程计算机上显示或隐藏详细的错误消息。 如果看到类似于以下内容的错误消息，则启用远程错误可能很有用：<br /><br /> `For more information about this error navigate to the report server on the local server machine or enable remote errors`|  
|**启用报表的辅助功能元数据**|启用报表的 HTML 输出中的辅助功能元数据|  
|**启用报表的精确数据可视化调整大小**|配置 tablix 中数据可视化调整大小行为以完全适合。 这包括图表、仪表和地图。 禁用该行为（即数据可视化对象为大致适合时），可能留下一些空格。 此设置仅适用于在报表查看器 Web 部件中呈现。 要为服务器端呈现管理此行为，需要修改 rsreportserver.config 文件。 有关详细信息，请参阅以下主题：<br /><br /> [RsReportServer.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)。<br /><br /> [Customize Rendering Extension Parameters in RSReportServer.Config](../../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md)。<br /><br /> [HTML Device Information Settings](../../reporting-services/html-device-information-settings.md)。<br /><br /> 启用“精确”可能会影响性能，因为确定精确大小所需的处理时间可能比大致适合时所需的时间长。|  
  
## <a name="see-also"></a>另请参阅

 [管理 Reporting Services SharePoint 服务应用程序](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)  
  
  
