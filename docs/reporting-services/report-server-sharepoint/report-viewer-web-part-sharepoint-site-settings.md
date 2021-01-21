---
title: 用于报表查看器 Web 部件的 SharePoint 网站设置 - SSRS | Microsoft Docs
description: 了解如何在 SQL Server 报表服务器的报表查看器 Web 部件中配置 SharePoint 站点设置。
ms.date: 11/15/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: jt000
ms.author: jasontre
ms.openlocfilehash: dfb8a3a33d56660bd8a7bdeeffb88824a3960dce
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596917"
---
# <a name="sharepoint-site-settings-for-the-report-viewer-web-part---reporting-services"></a>用于报表查看器 Web 部件的 SharePoint 网站设置 - Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

报表查看器 Web 部件有一些可配置的设置。 可由网站管理员在 SharePoint 网站设置页上启用或禁用这些设置。 每个站点都有其自己的设置。 另外，重新安装报表查看器 Web 部件之后，不会重置这些设置。

## <a name="accessing-the-site-settings-page"></a>访问“站点设置”页

可通过以下操作访问网站设置：

1. 在 SharePoint 网站中，选择左上角的“齿轮”图标，然后选择“网站设置” 。

    ![齿轮图标中的“网站设置”。](media/sharepoint-site-settings.png)

2. 单击“Reporting Services”网站设置组中的“报表查看器 Web 部件设置”。

    > [!NOTE]
    > 还可通过直接导航到 `<site>/_layouts/15/ReportViewerWebPart/ReportViewerWebPartSettings.aspx` 访问网站设置

## <a name="report-viewer-web-part-settings"></a>报表查看器 Web 部件设置

|设置|注释|  
|-------------|--------------|  
|收集使用情况数据|启用要发送给 Microsoft 的错误和使用情况信息，以帮助改进产品。 有关 Microsoft 错误报表数据收集策略的信息，请参阅 [Microsoft SQL Server 隐私声明](../../sql-server/sql-server-privacy.md)。|  
|启用报表的辅助功能元数据|为呈现的报表设置 [`AccessibleTablix` 设备信息](../html-device-information-settings.md)。|