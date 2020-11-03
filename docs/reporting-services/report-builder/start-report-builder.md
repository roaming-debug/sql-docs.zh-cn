---
title: 启动报表生成器 | Microsoft Docs
description: 报表生成器是独立的报表创作环境。 首次启动它时，Microsoft 下载中心会提示你下载它。
ms.date: 01/03/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
helpviewer_keywords:
- Report Builder, launching
- launching Report Builder
- SharePoint integration [Reporting Services], starting Report Builder
- starting Report Builder
ms.assetid: 8c8c7d2e-b315-418d-bf65-90e7685e4259
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 00a954a23cf9b17a58c3272a03222019400ae891
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907035"
---
# <a name="start-report-builder"></a>启动报表生成器

[!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 是独立的报表创作环境。 使用它，可以创建分页报表并将其发布到在本机或 SharePoint 集成模式下安装的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 。  

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。
  
 从 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] Web 门户或 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中以 SharePoint 集成模式首次启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 时，系统会提示从 Microsoft 下载中心下载。 
 
![“正在打开报表生成器”消息的屏幕截图。](../../reporting-services/report-builder/media/report-builder-get-report-builder.png) 
 
 你或管理员还可以 [从 Microsoft 下载中心将报表生成器安装到你的计算机上](https://go.microsoft.com/fwlink/?LinkID=219138)。 有关详细信息，请参阅 [安装报表生成器](../../reporting-services/install-windows/install-report-builder.md) 中的“使用系统管理器服务器安装报表生成器”。
 
 安装 SQL Server Reporting Services 时未安装 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]；需单独下载并安装它。  
  
 从 Web 门户或 SharePoint 站点启动 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 时，如果打开了较早版本的 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] ，请与你的管理员联系，管理员可以更新 Web 门户或 SharePoint 站点上的版本。  
  
## <a name="to-start-ssrbnoversion-from-the-ssrsnoversion-web-portal"></a>从 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] Web 门户启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]  
  
1.  在 Web 浏览器的地址栏中，键入报表服务器的 URL。 默认情况下，URL 为 https://\<*servername*>/reports。  
  
2.  在 Web 门户的上栏中，选择“新建” > “分页报表”。    
  
     ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png "PBI_SSMRP_NewMenu")  
  
     第一次，将提示你 [安装报表生成器](../../reporting-services/install-windows/install-report-builder.md)。 
  
     之后将打开 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]，然后就可以从报表服务器中创建分页报表或打开报表。  
  
## <a name="to-start-ssrbnoversion-in-sharepoint-integrated-mode"></a>在 SharePoint 集成模式下启动 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]  
  
1.  导航到包含所需库的 SharePoint 站点。  
  
2.  打开库。  
  
3.  单击 **“文档”** 。  
  
4.  在 **“新建文档”** 菜单上，单击 **“报表生成器报表”** 。  
  
     第一次时，此操作将启动 SQL Server [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 向导。 有关详细信息，请参阅 [Install Report Builder](../../reporting-services/install-windows/install-report-builder.md) 。  
  
     [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 将打开，随后可在报表服务器中创建分页报表或打开报表。  
  
     **注意** 如果  “新建文档”菜单未列出  “报表生成器报表”、  “报表生成器模型”或  “报表数据源”，则需要将其内容类型添加到 SharePoint 库中。 有关详细信息，请参阅 [向 SharePoint 库添加 Reporting Services 内容类型](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md)。  

## <a name="next-steps"></a>后续步骤

[SQL Server 中的报表生成器](../../reporting-services/report-builder/report-builder-in-sql-server-2016.md)   
[设置报表生成器的默认选项](../../reporting-services/report-builder/set-default-options-for-report-builder.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
