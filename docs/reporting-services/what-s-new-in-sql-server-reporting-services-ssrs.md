---
title: Reporting Services 中的新增功能 | Microsoft Docs
description: 了解 SQL Server Reporting Services 不同版本的新增功能，包括对主要功能区域的更改。
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: ''
ms.date: 12/05/2019
ms.openlocfilehash: fd8f612727e860f9797a5af46eb77154a19ff5aa
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237157"
---
# <a name="whats-new-in-sql-server-reporting-services-ssrs"></a>SQL Server Reporting Services (SSRS) 中的新增功能

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../includes/ssrs-appliesto-not-pbirs.md)]

了解 SQL Server [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 不同版本的新增功能。 本文包括主要功能区域，在发布新项时进行更新。

有关 Power BI 报表服务器的信息，请参阅[什么是 Power BI 报表服务器？](/power-bi/report-server/get-started)。

::: moniker range=">=sql-server-ver15"

## <a name="sql-server-2019-reporting-services"></a>SQL Server 2019 Reporting Services

**下载** :::image type="icon" source="https://docs.microsoft.com/analysis-services/analysis-services/media/download.png":::

可通过 Microsoft 下载中心下载 [SQL Server 2019 Reporting Services](https://www.microsoft.com/download/details.aspx?id=100122)。

### <a name="azure-sql-managed-instance-support"></a>Azure SQL 托管实例支持

现在可以在 VM 或数据中心托管的 Azure SQL 托管实例 (MI) 中承载用于 SQL Server Reporting Services (SSRS) 的数据库目录。 支持仅限于使用数据库凭据连接到 SQL MI。

### <a name="power-bi-premium-dataset-support"></a>Power BI Premium 数据集支持

可以使用 Microsoft 报表生成器或 SQL Server 数据工具 (SSDT) 连接到 Power BI 数据集。 然后，可以使用 SQL Server Analysis Services 连接将这些报表发布到 SSRS 2019。 用户需要使用存储的 Windows 用户名和密码来启用方案。

### <a name="alttext-alternative-text-support-for-report-elements"></a>报表元素支持 AltText（替代文本）

在创作报表时，可以使用工具提示为报告中的每个元素指定文本。 屏幕阅读器技术正确识别这些工具提示。

### <a name="azure-active-directory-application-proxy-support"></a>Azure Active Directory 应用程序代理支持

使用 Azure Active Directory 应用程序代理，你不再需要管理自己的 Web 应用程序代理即可允许通过 Web 或移动应用进行安全访问。

### <a name="custom-headers"></a>自定义标头

为所有与指定的正则表达式模式匹配的 URL 设置标头值。 用户可以使用有效的 XML 更新自定义标头值，以设置所选请求 URL 的标头值。 管理员可以在 XML 中添加任意数量的标头。 有关详细信息，请参阅“服务器属性高级”页中的[自定义标头](tools/server-properties-advanced-page-reporting-services.md#customheaders)。

### <a name="transparent-database-encryption"></a>透明数据库加密

SQL Server 2019 现支持适用于企业版和标准版的 SSRS 目录数据库的透明数据库加密。 

### <a name="microsoft-report-builder-update"></a>Microsoft 报表生成器更新

新发布的报表生成器版本与 Reporting Services 的 2016、2017 和 2019 版本完全兼容。 它还与 Power BI 报表服务器的所有发行版和受支持的版本兼容。

::: moniker-end

::: moniker range=">=sql-server-2017"

## <a name="sql-server-2017-reporting-services"></a>SQL Server 2017 Reporting Services

**下载** :::image type="icon" source="https://docs.microsoft.com/analysis-services/analysis-services/media/download.png":::

若要下载 SQL Server 2017 Reporting Services，请转到 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=55252)。

### <a name="comments-on-reports"></a>注释报表

注释现在可用于报表，以增加视角并与他人协作。 还可包含带有批注的附件。

![在报表服务器中注释](media/what-s-new-in-sql-server-reporting-services-ssrs/report-server-comments.png)

有关详细信息，请参阅[在报表服务器中将注释添加到报表](https://powerbi.microsoft.com/documentation/reportserver-add-comments/)。

### <a name="dax-queries-in-reporting-tools"></a>在报表工具进行 DAX 查询

在最新版本的报表生成器和 SQL Server Data Tools 中，可针对 SQL Server Analysis Services 表格数据模型创建本机 DAX 查询。 可以在查询设计器中拖放字段。 请参阅 [Reporting Services 博客](/archive/blogs/sqlrsteamblog/query-designer-support-for-dax-now-available-in-report-builder-and-sql-server-data-tools)。

### <a name="rest-api-support"></a>REST API 支持

为了实现开发新式应用程序和自定义，SQL Server Reporting Services 现在支持完全符合 OpenAPI 标准的 RESTful API。 现在可在 [SwaggerHub](https://app.swaggerhub.com/apis/microsoft-rs/SSRS/2.0) 上找到完整的 API 规范和文档。

### <a name="query-designer-support-for-dax-now-in-report-builder-and-sql-server-data-tools"></a>报表生成器和 SQL Server Data Tools 中现提供对 DAX 的查询设计器支持

在报表生成器和 SQL Server Data Tools 中，现可针对支持的 SQL Server Analysis Services 表格数据模型创建本机 DAX 查询。 可以使用两种工具中的查询设计器来拖放所需字段。 然后就会为你生成 DAX 查询。

在 [Reporting Services 博客](/archive/blogs/sqlrsteamblog/query-designer-support-for-dax-now-available-in-report-builder-and-sql-server-data-tools)上了解详细信息。

* 下载 [SQL Server 报表生成器](https://go.microsoft.com/fwlink/?LinkId=734968)。
* 下载 [SQL Server Data Tools - 候选发布](../ssdt/download-sql-server-data-tools-ssdt.md)。

> [!NOTE]
> 只能在 SQL Server 2016 及更高版本中内置的 SSAS 表格数据源中使用 DAX 查询设计器。
::: moniker-end

## <a name="ssrs-2016"></a>SSRS 2016

### <a name="reporting-services-ssrswebportal-non-markdown"></a>Reporting Services [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)]  

新的 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 已推出。 已更新的 Web 门户包括
- KPI
- 移动报表
- 分页报表
- Excel 文件
- Power BI Desktop 文件

[!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] 取代了以前版本中的报表管理器。

若要创建移动报表，则需使用 [!INCLUDE[SS_MobileReptPub_Short](../includes/ss-mobilereptpub-short.md)]。

有关 [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)]的详细信息，请参阅 [Web 门户（SSRS 本机模式）](../reporting-services/web-portal-ssrs-native-mode.md)。  

![显示 SQL Server Reporting Services 门户的屏幕截图。](../reporting-services/media/ssrsportal.png "ssRSPortal")  

#### <a name="custom-branding-for-the-ssrswebportal-non-markdown"></a>[!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 

你可以通过品牌包使用组织的徽标和颜色来自定义 [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 。  

有关自定义品牌的详细信息，请参阅 [设置 Web 门户的品牌](./branding-the-web-portal.md)

#### <a name="key-performance-indicators-kpi-in-the-ssrswebportal-non-markdown"></a>[!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 中的关键绩效指标 (KPI) 

可以在 [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 中直接创建与当前文件夹有上下文关系的 KPI。 在创建 KPI 时，可以选择数据集字段并汇总它们的值。 还可以选择相关内容以钻取到更多详细信息。

![屏幕截图：SQL Server Reporting Services 门户中的 KPI。](../reporting-services/media/ssrs-webportal-kpi.png)

有关详细信息，请参阅[使用 Web 门户中的 KPI](./working-with-kpis-in-reporting-services.md)

### <a name="mobile-reports"></a>移动报表

Reporting Services 移动报表是针对各种外形规格进行了优化的专用报表，可以在用户访问移动设备上的报表时为其提供最佳体验。 移动报表提供各式各样的可视化效果：从时间、类别和比较图表，到树状地图和自定义地图。 将移动报表连接到各种数据源，包括本地 SQL Server Analysis Services 多维数据和表格数据。 可以将移动报表的字段放置在带有可调整网格行和列的设计图面上。 灵活的移动报表元素会自动缩放，从而适合任何屏幕大小。 将移动报表保存到 Reporting Service 服务器，并在浏览器或 Power BI 移动应用中进行查看和交互。 支持的设备包括：

- iPad
- iPhone
- Android 手机
- 或任何 Windows 10 设备

#### <a name="mobile-report-publisher"></a>移动报表发布服务器  

[!INCLUDE[SS_MobileReptPub_Long](../includes/ss-mobilereptpub-long.md)] 允许你创建 SQL Server 移动报表并将其发布到 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)]。  

![SS_MRP_LayoutTabSmall](../reporting-services/media/ss-mrp-layouttabsm.png "SS_MRP_LayoutTabSmall")  

有关详细信息，请参阅 [使用 SQL Server 移动报表发布服务器创建移动报表](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md)。  

#### <a name="sql-server-mobile-reports-hosted-in-reporting-services-available-in-power-bi-mobile-app"></a>在 Reporting Services 中托管的 SQL Server 移动报表可以在 Power BI 移动应用中使用  

在 iPad 和 iPhone 上使用的 iOS 版 Power BI 移动应用现在可以显示在本地报表服务器上托管的 SQL Server 移动报表。  

![SS_MRP_iPad_HomeSm](../reporting-services/media/ss-mrp-ipad-homesm.png "SS_MRP_iPad_HomeSm")  

默认情况下，如果没有进行某些配置更改，你将无法进行连接。 有关如何才能让 Power BI 移动应用连接到报表服务器的详细信息，请参阅 [为报表服务器启用 Power BI 移动访问权限](../reporting-services/report-server/enable-a-report-server-for-power-bi-mobile-access.md)。

### <a name="support-of-sharepoint-mode-and-sharepoint-2016"></a>SharePoint 模式和 SharePoint 2016 的支持  

[!INCLUDE[sssql15-md](../includes/sssql16-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 支持与 SharePoint 2013 和 SharePoint 2016 集成。

有关详细信息，请参阅：  

- [支持的 SharePoint 和 Reporting Services 服务器及外接程序的组合 (SQL Server 2016)](../reporting-services/install-windows/supported-combinations-of-sharepoint-and-reporting-services-server.md)  

- [在何处查找用于 SharePoint 产品的 Reporting Services 外接程序](../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)  

- [安装 Reporting Services SharePoint 模式](../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)  

### <a name="microsoft-net-framework-4-support"></a>Microsoft .NET Framework 4 支持  

[!INCLUDE[ssRSCurrent-md](../includes/ssrscurrent-md.md)] 支持最新版本的 Microsoft .NET Framework 4，包括版本 4.0 和 4.5.1。 如果没有安装版本 4.x 的 .Net Framework，[!INCLUDE[ssNoVersion-md](../includes/ssnoversion-md.md)] 安装程序会在功能安装步骤安装 .NET 4.0。  

### <a name="report-improvements"></a>报表改进

**HTML 5 呈现引擎：** 新的 HTML5 呈现引擎，针对的是新式 Web“完整”标准模式和新式浏览器。  新的呈现引擎不再依赖于一些老式浏览器使用的 Quirks 模式。

有关浏览器支持的详细信息，请参阅 [Reporting Services 和 Power View 的浏览器支持](../reporting-services/browser-support-for-reporting-services-and-power-view.md)。  

**新式分页报表：** 设计美观的新型分页报表，提供适用于图表、仪表、映射和其他数据可视化对象的各种新奇且现代的样式。

**树形图和旭日图：** 使用树状图 ![ssrs_treemap_icon](../reporting-services/media/ssrs-treemap-icon.png "ssrs_treemap_icon") 和旭日图 ![ssrs_sunburst_icon](../reporting-services/media/ssrs-sunburst-icon.png "ssrs_sunburst_icon") 增强报表，非常适合显示分层数据。 有关详细信息，请参阅 [Tree Map and Sunburst Charts in Reporting Services](../reporting-services/report-design/tree-map-and-sunburst-charts-in-reporting-services.md)。  

**报表嵌入：** 现可结合使用 iframe 和 URL 参数将移动报表和分页报表嵌入其他网页和应用程序中。  

**将报表项固定到 Power BI 仪表板：** 在 [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]中查看报表时，可以选择报表项并将其固定到 [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] 仪表板。   可以固定的项为图表、仪表面板、映射和映像。 可以：

1. 选择包含要固定的仪表板的组。
2. 选择要在其中固定项的仪表板。
3. 选择要在仪表板中更新磁贴的频率。

![说明](/analysis-services/analysis-services/instances/install-windows/media/ssrs-fyi-note.png "备注") 刷新由 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 订阅管理，固定项之后，可编辑该订阅并配置不同的刷新计划。

![显示“固定到 Power BI 仪表板”对话框的屏幕截图。](../reporting-services/media/ssrs-pin-to-powerbi.png) 

有关详细信息，请参阅 [Power BI 报表服务器集成 (Configuration Manager)](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md)和[将 Reporting Services 项固定到 Power BI 仪表板](../reporting-services/pin-reporting-services-items-to-power-bi-dashboards.md)。  

**PowerPoint 呈现和导出：** Microsoft PowerPoint (PPTX) 格式是一种新的 [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] 呈现扩展插件。 你可以采用 PPTX 格式从以下常用应用程序导出报表：报表生成器、报表设计器（在 SSDT 中）和 [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]。 如需示例，请参阅下图，其中显示了 [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]中的导出菜单。 

![屏幕截图：已调出“PowerPoint”选项的“导出”下拉列表。](../reporting-services/media/ssrs-export-powerpoint.png) 

你还可以选择适用于订阅输出的 PPTX 格式，并使用 Report Server URL 访问权限来呈现和导出报表。 例如，浏览器中的以下 URL 命令从报表服务器的命名实例导出报表。  

```https
https://servername/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  

有关详细信息，请参阅 [Export a Report Using URL Access](../reporting-services/export-a-report-using-url-access.md)。

**PDF 替换 ActiveX 进行远程打印：** 报表查看器工具栏现在可通过 PDF 而不是 ActiveX 控件进行打印。 大多数新式浏览器（包括 Microsoft Edge）都支持新的报表查看器。 不需要下载 ActiveX 控件！ 根据你所使用的浏览器以及所安装的 PDF 查看应用程序和服务，[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 会打开一个打印对话框以打印报表，或者提示你下载 .PDF 文件。 作为管理员，你仍然可以在 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 中禁用客户端打印。

有关详细信息，请参阅 [启用和禁用 Reporting Services 的客户端打印](../reporting-services/report-server/enable-and-disable-client-side-printing-for-reporting-services.md)。

![“打印”对话框的屏幕截图。](../reporting-services/media/ssrs-pdf-printing.png)

### <a name="subscription-improvements"></a>订阅改进  

|Feature|支持的服务器模式|  
|-------------|---------------------------|  
|**启用和禁用订阅**。 新用户界面选项可以快速禁用和启用订阅。 已禁用的订阅将维持自身的其他配置属性（例如计划），并且可以轻松地启用。<br /><br /> ![显示“启用”、“禁用”和“删除”选项的屏幕截图。](../reporting-services/media/ssrs-enable-disable-subscriptions.png)<br /><br /> 有关详细信息，请参阅 [Disable or Pause Report and Subscription Processing](../reporting-services/subscriptions/disable-or-pause-report-and-subscription-processing.md)。|本机模式|  
|**订阅说明**。 你现在可以在创建新订阅时提供报表说明，作为订阅属性的一部分。 说明在订阅摘要页中提供。|SharePoint 和本机模式|  
|**更改订阅所有者**。 增强的用户界面，可以快速更改订阅的所有者。 旧版 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 允许管理员使用脚本来更改订阅所有者。 从 [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] 版本开始，你可以使用用户界面或脚本更改订阅所有者。 当用户离开组织或者更改其在组织中的角色时，就需要更改订阅所有者，这是一项常规管理任务。|SharePoint 和本机模式|  
|**文件共享订阅的共享凭据**。 现在，两个工作流与 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 文件共享订阅共存：<br /><br /> 使用此版本中的新增功能，[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 管理员可以配置单个文件共享帐户，该帐户可用于多个订阅。 文件共享帐户在 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 本机模式配置管理器“指定文件共享帐户”中配置。 在“订阅配置”页面上，用户可选择“使用文件共享帐户”。<br /><br /> 使用目标文件共享的特定凭据配置单独的订阅。<br /><br /> 你还可以混合使用这两种方法，使一些文件共享订阅使用中央文件共享帐户，而其他订阅使用特定的凭据。|本机模式|

### <a name="sql-server-data-tools-ssdt"></a>SQL Server Data Tools (SSDT)

新版 SSDT 包括用于 [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] 的项目模板：报表服务器项目向导和报表服务器项目。 有关如何下载 SSDT 的信息，请参阅 [SQL Server Data Tools for Visual Studio 2015](../ssdt/download-sql-server-data-tools-ssdt.md)（适用于 Visual Studio 2015 的 SQL Server Data Tools）。  

### <a name="report-builder-improvements"></a>报表生成器改进

**新的报表生成器用户界面：** 核心 [!INCLUDE[ssRBnoversion](../includes/ssrbnoversion.md)] 用户界面现在使用简化的 UI 元素，给你一种现代化的观感。  

|新建|Previous|  
|-|-|  
|![ssrs_rbfacelift_new](../reporting-services/media/ssrs-rbfacelift-new.png "ssrs_rbfacelift_new")|![ssrs_rbfacelift_old](../reporting-services/media/ssrs-rbfacelift-old.png "ssrs_rbfacelift_old")|  

**自定义参数窗格：** 你现在可以自定义参数窗格。 利用报表生成器中的设计图面，可以将参数拖到参数窗格中的特定列和行。 你可以通过添加和删除列来更改窗格的布局。 有关详细信息，请参阅 [自定义报表中的参数窗格（报表生成器）](../reporting-services/report-design/customize-the-parameters-pane-in-a-report-report-builder.md)中所创建的移动报表中使用。  

![报表数据窗格和参数窗格中的参数列表](../reporting-services/media/ssrs-customizeparameter-parameterlist-reportdatapane.png "报表数据窗格和参数窗格中的参数列表")  

**高 DPI 支持：** [!INCLUDE[ssRBnoversion](../includes/ssrbnoversion.md)] 支持高 DPI（每英寸点数）缩放功能和设备。  有关高 DPI 的详细信息，请参阅以下内容：  

- [Windows 8.1 DPI 缩放增强功能](https://blogs.windows.com/windowsexperience/2013/07/15/windows-8-1-dpi-scaling-enhancements/)  

- [高 DPI 和 Windows 8.1](/previous-versions/windows/it-pro/windows-8.1-and-8/dn528848(v=win.10))  

## <a name="next-steps"></a>后续步骤

[Analysis Services 中的新增功能](/analysis-services/what-s-new-in-sql-server-analysis-services?viewFallbackFrom=sql-server-ver15)  
[向后兼容性](reporting-services-backward-compatibility.md)  
[SQL Server 各个版本支持的 Reporting Services 功能](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md)  
[升级和迁移 Reporting Services](../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)  
[Reporting Services](../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)