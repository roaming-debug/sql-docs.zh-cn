---
title: 适用于警报管理员的数据警报管理器 | Microsoft Docs
description: 了解如何在 SQL Server Reporting Services 中通过 SharePoint 的数据警报管理器管理警报。
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- managing, alerts
- managing, data alerts
ms.assetid: 32fd968f-1c0c-4ba8-851c-8a3b5e1fbbf2
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 7bd656cc4b543ecb2c291adcd9745f3c4b1c79bf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484339"
---
# <a name="data-alert-manager-for-alerting-administrators"></a>向管理员提出警报的数据警报管理器

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../includes/ssrs-appliesto-2016.md)] [!INCLUDE [ssrs-appliesto-not-2017](../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../includes/ssrs-previous-versions.md)]

SQL Server Reporting Services 为 SharePoint 警报管理员提供了数据警报管理器以便管理数据警报。 警报管理员可以查看与保存到站点的所有警报有关的信息和删除警报。 下图显示数据警报管理器中可用于 SharePoint 警报管理员的功能。

![SharePoint 网站管理员的警报管理器](../reporting-services/media/rs-alertmanagersite.gif "SharePoint 网站管理员的警报管理器")

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

 在为数据警报启用站点时，将创建两个 SharePoint 页（MyDataAlerts.aspx 和 SiteDataAlerts.aspx）并且将这两个 SharePoint 页将添加到该 SharePoint 站点中。 SiteDataAlerts.aspx 是用于向管理员提出警报的数据警报管理器。 警报管理员可以从“站点设置”SharePoint 页打开数据警报管理器。 警报管理员必须具有 SharePoint 管理警报权限才能打开数据警报管理器。  
  
 您还可以直接通过使用 URL 打开数据警报管理器。 下面的内容显示 URL 的语法：  
  
 `http: //<site name>/_layouts/ReportServer/ SiteDataAlerts.aspx`  
  
> [!NOTE]  
>  作为警报管理员，您可以向信息工作者授予访问 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 数据警报功能的权限。 有关所需权限的详细信息，请参阅 [Reporting Services 数据警报](../reporting-services/reporting-services-data-alerts.md)。  
  
##  <a name="viewing-data-alert-information"></a><a name="ViewingAlerts"></a> 查看数据警报信息  
 在 SharePoint 中安装和配置 Reporting Services 时，“站点设置”SharePoint 页将包括 **Reporting Services** 选项。 警报管理员可在 Reporting Service 中单击 **“管理数据警报”** 选项以便打开数据警报管理器。 下图显示您可以从“站点设置”页上的何处打开数据警报管理器。  
  
 ![“站点设置”页的 Reporting Services 部分](../reporting-services/media/rs-sitesettings.gif "“站点设置”页的 Reporting Services 部分")  
  
 数据警报管理器包括一个表，该表列出警报名称、报表名称、警报所有者的姓名、发送警报消息的数目、上次运行警报的时间、上次修改警报定义的时间以及警报消息的状态。 如果无法生成或发送警报，则状态列将包含有关该错误的信息并且帮助您纠正该警报问题。 有关详细信息，请参阅 [在数据警报管理器中管理 SharePoint 站点上的所有数据警报](../reporting-services/manage-all-data-alerts-on-a-sharepoint-site-in-data-alert-manager.md)。  
  
 下表在数据警报管理器中显示某个表中的示例数据。 发生错误时，错误消息和日志中条目的标识符 (GUID) 包含在表的“状态”字段中。  
  
|警报名称|报表名称|创建者|发送的警报数|上次运行时间|上次修改时间|状态|  
|----------------|-----------------|----------------|-----------------|--------------|-------------------|------------|  
|SalesQTR|SalesByTerritoryAndQTR|Lauren Johnson|4|6/12/2011|6/1/2011|最后一个警报已成功运行且已发送。|  
|UnitsSold|ProductsSalesByQTR|Michael Blythe|2|7/1/2011|6/28/2011|已成功运行最后一个警报，但由于数据未发生更改，因此未发送任何警报。|  
|InventoryCount|StockStatusByQTR|Lauren Johnson|7|7/10/2011|7/2/2011|\<error message>该日志文件包含有关错误的详细信息。 请参考具有以下标识符的日志条目：\<GUID>。|  
|TopPromotion|PromotionTracking|Cristian Petculescu|0||5/23/2011|创建了警报。|  
  
 有关详细信息，请参阅 [在数据警报管理器中管理 SharePoint 站点上的所有数据警报](../reporting-services/manage-all-data-alerts-on-a-sharepoint-site-in-data-alert-manager.md)。  
  
 您可以查看站点用户所创建的所有警报。 您选择某一用户，然后选择是查看其所有警报还是仅查看特定报表的警报。  
  
  
##  <a name="delete-data-alerts"></a><a name="DeleteAlerts"></a> 删除数据警报  
 您从数据警报管理器中删除警报定义。 每个数据警报定义都有一个所有者，即创建了该警报的 SharePoint 用户。 所有者只能删除他们创建的警报定义。 有关详细信息，请参阅 [在数据警报管理器中管理我的数据警报](../reporting-services/manage-my-data-alerts-in-data-alert-manager.md)。  
  
 SharePoint 警报管理员可以列出和删除站点的所有用户创建的警报定义。 有关详细信息，请参阅 [在数据警报管理器中管理 SharePoint 站点上的所有数据警报](../reporting-services/manage-all-data-alerts-on-a-sharepoint-site-in-data-alert-manager.md)。  
  
 删除警报定义后，将不会发送进一步的警报。 但是，如果您查询警报数据库，可能会发现该警报定义仍存在。 警报服务将按照计划执行清除，因此该警报定义将在下次清除中被永久删除。 默认清除间隔为 20 分钟。 此设置以及其他清除间隔都是可配置的。 有关详细信息，请参阅 [Reporting Services 数据警报](../reporting-services/reporting-services-data-alerts.md)。  
  
  
##  <a name="related-tasks"></a><a name="HowTo"></a> 相关任务  
 本节列出了说明如何管理您的警报的过程。  
  
-   [在数据警报管理器中管理 SharePoint 站点上的所有数据警报](../reporting-services/manage-all-data-alerts-on-a-sharepoint-site-in-data-alert-manager.md)  

## <a name="see-also"></a>另请参阅

[Reporting Services 数据警报](../reporting-services/reporting-services-data-alerts.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
