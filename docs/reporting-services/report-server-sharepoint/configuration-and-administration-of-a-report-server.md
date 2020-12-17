---
title: 配置和管理报表服务器 | Microsoft Docs
description: 借助 SQL Server Reporting Services，可以将报表环境与 SharePoint 产品集成，以使用 SharePoint 网站提供的协作。
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 4ba7f67892a1d579991cd8a244c80295c7fb2ea4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474488"
---
# <a name="configuration-and-administration-of-a-sql-server-reporting-services-ssrs-report-server"></a>配置和管理 SQL Server Reporting Services (SSRS) 报表服务器

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

SQL Server Reporting Services 是基于服务器的报表平台，提供了各种现成可用的工具和服务，帮助你创建、部署和管理组织的报表，并提供了使你能扩展和自定义报表功能的编程功能。 您可以将您的报表环境与 SharePoint 产品相集成，体验使用 SharePoint 站点提供的协作环境所带来的好处。

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

以下各节中的内容可以帮助你理解与将 Reporting Services 环境与 SharePoint 产品或技术相集成有关的概念、部署方案、过程等等：  
  
-   SharePoint 文档库中的菜单选项  
  
    -   [SharePoint 用户的数据警报管理器](../../reporting-services/data-alert-manager-for-sharepoint-users.md)  
  
    -   [创建和管理 SharePoint 模式报表服务器的订阅](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-sharepoint-mode-report-servers.md)  
  
    -   [从 SharePoint 站点更新报表数据源的凭据](../../reporting-services/report-data/update-credentials-in-report-data-sources-from-a-sharepoint-site.md)  
  
    -   [管理共享数据集](../../reporting-services/report-data/manage-shared-datasets.md)  
  
    -   [设置已发布报表的参数（SharePoint 集成模式下的 Reporting Services）](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md)  
  
    -   [设置处理选项（SharePoint 集成模式下的 Reporting Services）](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md)  
  
-   [Reporting Services 网站集功能](../../reporting-services/report-server-sharepoint/site-collection-features-reporting-services.md)  
  
-   [在 SharePoint 中激活报表服务器和 Power View 集成功能](../../reporting-services/report-server-sharepoint/site-collection-features-report-server-and-power-view.md)  
  
-   [Reporting Services 网站设置和网站功能（SharePoint 模式）](../../reporting-services/report-server-sharepoint/site-settings-and-features-reporting-services.md)  
  
-   [在 SharePoint 管理中心中激活报表服务器文件同步功能](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md)  
  
-   [将 Reporting Services 内容类型添加到 SharePoint 库](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md)  
  
-   [报表查看器中的本地模式和报表查看器中的连接模式报表（SharePoint 模式下的 Reporting Services）](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md)  
  
-   [将文档上传到 SharePoint 库（SharePoint 模式下的 Reporting Services）](../../reporting-services/report-server-sharepoint/upload-documents-to-a-sharepoint-library-reporting-services-in-sharepoint-mode.md)  
  
-   [设置处理选项（SharePoint 集成模式下的 Reporting Services）](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md)  
  
更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
