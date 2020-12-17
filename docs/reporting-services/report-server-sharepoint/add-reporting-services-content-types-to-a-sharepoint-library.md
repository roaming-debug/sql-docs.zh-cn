---
title: 将 Reporting Services 内容类型添加到 SharePoint 库 | Microsoft Docs
description: 将“报表生成器报表”、“报表模型”和“报表数据源”内容类型添加到库中后，便可使用“新建”命令，从而可创建对应类型的新文档。
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
ms.assetid: ac9136c8-9ef4-484c-8e9d-05008a186db5
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: e2988bfecf08ad5987519ae0c4e166089973d745
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97424852"
---
# <a name="add-reporting-services-content-types-to-a-sharepoint-library"></a>将 Reporting Services 内容类型添加到 SharePoint 库

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 提供预定义的 SharePoint 内容类型，用于管理共享数据源 (.rsds) 文件、报表模型 (.smdl) 和报表生成器报表定义 (.rdl) 文件。 将 **“报表生成器报表”**、 **“报表模型”** 和 **“报表数据源”** 内容类型添加到库中将启用 **“新建”** 命令，以便创建对应类型的新文档。

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

 若要将内容类型添加到库，您必须为站点管理员或拥有“完全控制”级权限。  
  
 将自动在所有文档库中为从以下站点模板类型创建的现有网站集启用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 内容类型和内容类型管理：  
  
-   **商业智能中心**  
  
 在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 集成之后创建的站点将不启用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 内容类型。  
  
> [!TIP]  
>  如果您 **不** 具有为某个库以前配置的内容类型，则首先启用内容类型的管理，然后启用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 内容类型。 请参阅在单个文档库中启用内容类型管理的过程。  
  
 **短视频：** [(SSRS) 在 SharePoint2010.wmv 中启用内容类型](https://www.youtube.com/watch?v=yqhm3DrtT1w) (https://www.youtube.com/watch?v=yqhm3DrtT1w) 。  
  
 **本主题内容：**  
  
-   [在现有商业智能中心的所有文档库中启用内容类型](#bkmk_enable_all)  
  
-   [为单个文档库启用内容类型管理 (SharePoint 2013)](#bkmk_enable_content_management)  
  
-   [添加 Reporting Services 内容类型 (SharePoint 2013)](#bkmk_add_single)  
  
-   [为单个文档库启用内容类型管理 (SharePoint 2010)](#bkmk_enable_content_management_2010)  
  
-   [添加报表服务器内容类型 (SharePoint 2010)](#bkmk_add_single_2010)  
  
-   [为多个商业智能站点启用内容类型和内容管理](#bkmk_enable_multiple_sites)  
  
##  <a name="enable-content-types-in-all-document-libraries-in-an-existing-bi-center"></a><a name="bkmk_enable_all"></a> 在现有商业智能中心的所有文档库中启用内容类型  
  
1.  若要在现有 **“商业智能中心站点”** 中的所有文档库中启用内容类型和内容管理，您可以切换 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 集成功能。  
  
2.  转到 **“站点设置”** 。  
  
    -   在 SharePoint 2013 中，单击 **“设置”** 图标。 ![SharePoint 设置](/analysis-services/analysis-services/media/as-sharepoint2013-settings-gear.gif "SharePoint 设置")  
  
    -   在 SharePoint 2010 中，单击 **“网站操作”** ，然后单击 **“网站设置”** 。  
  
3.  单击 **“网站集功能”** 。  
  
4.  找到 **“报表服务器集成功能”** ，然后单击 **“停用”** 。  
  
     ![报表服务器集成功能的屏幕截图，其中包括“停用”和“激活”选项。](../../reporting-services/report-server-sharepoint/media/rs-reportserver-integration-active.gif "rs_reportserver_integration_active")  
  
5.  刷新浏览器，然后为 **“报表服务器集成功能”** 单击 **“激活”** 。  
  
     ![报表服务器集成功能的屏幕截图，其中显示“激活”选项。](../../reporting-services/report-server-sharepoint/media/rs-reportserver-integration-deactivate.gif)  
  
##  <a name="to-enable-content-type-management-for-a-single-document-library-sharepoint-2013"></a><a name="bkmk_enable_content_management"></a> 为单个文档库启用内容类型管理 (SharePoint 2013)  
  
1.  打开要为其启用多个内容类型的库。  
  
2.  在功能区中单击 **“库”** 。  
  
     ![rs_SharePoint2013_LibraryRibbon](../../reporting-services/report-server-sharepoint/media/rs-sharepoint2013-libraryribbon.gif "rs_SharePoint2013_LibraryRibbon")  
  
3.  在 **“库”** 功能区上单击 **“库设置”** 。 如果未看到 **“库设置”** 或按钮被禁用，则说明您无权配置库设置，包括内容类型。  
  
     ![rs_SharePoint2013_LibrarySettings](../../reporting-services/report-server-sharepoint/media/rs-sharepoint2013-librarysettings.gif "rs_SharePoint2013_LibrarySettings")  
  
4.  在 **“常规设置”** 部分中，单击 **“高级设置”** 。  
  
     ![rs_SharePoint2013_LibrarySettings_AdvancedSettings](../../reporting-services/report-server-sharepoint/media/rs-sharepoint2013-librarysettings-advancedsettings.gif "rs_SharePoint2013_LibrarySettings_AdvancedSettings")  
  
5.  在 **“内容类型”** 部分中，选择 **“是”** 以允许内容类型管理。  
  
6.  单击“确定”。  
  
##  <a name="to-add-reporting-services-content-types-sharepoint-2013"></a><a name="bkmk_add_single"></a> 添加 Reporting Services 内容类型 (SharePoint 2013)  
  
1.  打开要添加 Reporting Services 内容类型的库。  
  
2.  在功能区上，单击 **“库”** 。  
  
3.  单击 **“库设置”** 。  
  
4.  在 **“内容类型”** 的下面，单击 **“从现有网站内容类型添加”** 。  
  
5.  在 **“从以下列表中选择网站内容类型”** 中，选择 **“SQL Server Reporting Services 内容类型”** 。  
  
6.  在 **“可用网站内容类型”** 列表中，单击 **“报表生成器”**，然后单击 **“添加”** ，将选定内容类型移到 **“要添加的内容类型”** 列表中。  
  
7.  若要添加“ **报表模型”** 和 **“报表数据源”** 内容类型，请重复执行前面的步骤。  
  
8.  添加完内容类型后，单击 **“确定”** 。  
  
    > [!NOTE]  
    >  如果 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 内容类型组“SQL Server Reporting Services 内容类型”在“添加内容类型”页上不可见，则以下条件之一成立：  
  
    -   尚未安装用于 SharePoint 产品的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序。 有关详细信息，请参阅 [安装或卸载适用于 SharePoint 的 Reporting Services 外接程序](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)。 本主题包括有关安装外接程序和单步执行外接程序的“仅文件”安装以解决问题的信息。  
  
    -   外接程序已安装，但网站集功能“报表服务器集成功能”未处于活动状态。 在“站点设置” 中验证网站集功能。  
  
    -   所有 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 内容类型都已添加到该库中。 如果所有内容类型都是某个库的一部分，则从 **“添加内容类型”** 页中删除该组。 如果您删除一个或多个 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 内容类型，则组 **“SQL Server Reporting Services 内容类型”** 将在 **“添加内容类型”** 页上可见。  
  
##  <a name="to-enable-content-type-management-for-a-single-document-library-sharepoint-2010"></a><a name="bkmk_enable_content_management_2010"></a> 为单个文档库启用内容类型管理 (SharePoint 2010)  
  
1.  打开要为其启用多个内容类型的库。 在库菜单栏上，应会看到以下菜单：“新建”、“上载”、“操作”和“设置”。 如果未看到 **“设置”** ，则说明您没有添加内容类型的权限。  
  
2.  在 **“库工具”** 功能区上单击 **“库”** 。  
  
     ![rs_SharePoint2010_LibraryRibbon](../../reporting-services/report-server-sharepoint/media/rs-sharepoint2010-libraryribbon.gif "rs_SharePoint2010_LibraryRibbon")  
  
3.  在 **“设置”** 功能区组中，单击 **“库设置”** 。  
  
4.  在 **“常规设置”** 的下面，单击 **“高级设置”** 。  
  
5.  在 **“内容类型”** 部分中，选择 **“是”** 以允许内容类型管理。  
  
6.  单击“确定”。  
  
##  <a name="to-add-report-server-content-types-sharepoint-2010"></a><a name="bkmk_add_single_2010"></a> 添加报表服务器内容类型 (SharePoint 2010)  
  
1.  打开要添加 Reporting Services 内容类型的库。  
  
2.  在 **“库工具”** 功能区选项卡上，单击 **“库”** 选项卡。  
  
3.  在 **“设置”** 功能区组中，单击 **“库设置”** 。  
  
4.  在 **“内容类型”** 的下面，单击 **“从现有网站内容类型添加”** 。  
  
5.  在 **“选择内容类型”** 部分的 **“从列表选择网站内容类型”** 中，单击箭头选择 **“SQL Server Reporting Services 内容类型”** 。  
  
6.  在 **“可用网站内容类型”** 列表中，单击 **“报表生成器”**，然后单击 **“添加”** ，将选定内容类型移到 **“要添加的内容类型”** 列表中。  
  
7.  若要添加“ **报表模型”** 和 **“报表数据源”** 内容类型，请重复执行前面的步骤。  
  
8.  添加完内容类型后，单击 **“确定”** 。  
  
##  <a name="to-enable-content-types-and-content-management-for-multiple-bi-sites"></a><a name="bkmk_enable_multiple_sites"></a>为多个商业智能站点启用内容类型和内容管理  
  
1.  对于 SQL Server Reporting Services 2008 和 2008 R2 报表服务器，您可以为多个商业智能中心站点启用内容类型和内容管理。  
  
2.  在 SharePoint 管理中心中，单击 **“常规应用程序设置”** 。 在“SQL Server Reporting Services (2008 和 2008 R2)”部分中，单击“Reporting Services 集成” 。  
  
     ![rs_general_app_settings](../../reporting-services/report-server-sharepoint/media/rs-general-app-settings.gif "rs_general_app_settings")  
  
3.  单击 **“激活现有网站集中的所有功能”** 。  
  
     ![rs_general_app_settings_old_integrations](../../reporting-services/report-server-sharepoint/media/rs-general-app-settings-old-integrations.gif "rs_general_app_settings_old_integrations")  
  
4.  单击“确定” 。  
  
## <a name="see-also"></a>另请参阅  
 [报表服务器项的 SharePoint 站点和列表权限参考](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md)   
 [启动报表生成器](../../reporting-services/report-builder/start-report-builder.md)  
  
