---
title: Reporting Services 和 Power View 的浏览器支持 | Microsoft Docs
description: 了解管理和查看 SQL Server Reporting Services、ReportViewer 控件和 Power View 支持的浏览器版本。
ms.date: 01/28/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- displaying reports
- scripts [Reporting Services], requirements
- viewing reports
- browsers [Reporting Services]
- Web browsers [Reporting Services], about browser support
- browsing reports [Reporting Services]
- components [Reporting Services], browsers
- Web browsers [Reporting Services]
ms.assetid: 48a75bbb-0029-4c43-891d-dc8f4fc0ebe1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e246db4f2a7b2a94ce17f8a48acf05b16aebbdf4
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049015"
---
# <a name="browser-support-for-reporting-services-and-power-view"></a>Reporting Services 和 Power View 的浏览器支持

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

了解管理和查看 SQL Server Reporting Services、ReportViewer 控件和 Power View 支持的浏览器版本。

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

## <a name="browser-requirements-for-the-web-portal"></a>Web 门户的浏览器要求

以下是 Web 门户当前支持的浏览器列表。

**Microsoft Windows**  
*Windows 7、8.1、10；Windows Server 2008 R2、2012、2012 R2*
- Microsoft Edge (+)
- Microsoft Internet Explorer 10 或 11
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple iOS**  
*iPhone 和 iPad (iOS 9)*

- Apple Safari (+)

**Google Android**  
*Android 4.4 (KitKat) 或更高版本的手机或平板电脑*

- Google Chrome (+)

 **(+)** 最新公开发布的版本

## <a name="browser-requirements-for-the-reportviewer-web-control-2015"></a>ReportViewer Web 控件 (2015) 的浏览器要求

 以下是 ReportViewer Web 控件 (2015) 当前支持的浏览器的列表。 报表查看器支持从 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] Web 门户和 SharePoint 库中查看报表。  

**Microsoft Windows**  
*Windows 7、8.1、10；Windows Server 2008 R2、2012、2012 R2*

- Microsoft Edge (+)
- Microsoft Internet Explorer 10 或 11
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)

 **(+)** 最新公开发布的版本

::: moniker range="=sql-server-2016"

 如果当前使用与 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]相集成的 SharePoint 产品，请参阅  [在 SharePoint 2016 中计划浏览器支持](https://technet.microsoft.com//library/cc263526\(v=office.16\).aspx)。

::: moniker-end

### <a name="authentication-requirements"></a>身份验证要求

 浏览器支持必须由报表服务器处理的特定身份验证方案，以使客户端请求获得成功。 下表标识 Windows 操作系统上运行的各浏览器支持的默认身份验证类型。

|**浏览器类型**|**支持**|**浏览器默认值**|**服务器默认值**|
|----------------------|------------------|-------------------------|------------------------|
|**Microsoft Edge** (+)|协商、NTLM、基本|Negotiate|是的。 默认身份验证设置使用 Edge。|
|**Microsoft Internet Explorer**|协商、NTLM、基本|Negotiate|是的。 默认身份验证设置使用 Internet Explorer。|
|**Google Chrome**(+)|协商、NTLM、基本|Negotiate|是的。 默认身份验证设置使用 Chrome。|
|**Mozilla Firefox**(+)|NTLM、基本|NTLM|是的。 默认身份验证设置使用 Firefox。|
|**Apple Safari**(+)|NTLM、基本|基本|是的。 默认身份验证设置使用 Safari。|

 **(+)** 最新公开发布的版本

### <a name="script-requirements-for-viewing-reports"></a>查看报表的脚本要求

 若要使用报表查看器，请将浏览器配置为可以运行脚本。

 如果未启用脚本功能，则打开报表时将显示如下错误消息：

- **您的浏览器不支持脚本或已配置为不允许脚本运行。请单击此处在没有脚本的情况下查看此报表**。

 如果选择查看不支持脚本的报表，则报表将会以 HTML 格式呈现，同时不具有报表工具栏和文档结构图等报表查看器功能。

> [!NOTE]
> 报表工具栏是 HTML 查看器组件的一部分。 默认情况下，该工具栏显示在浏览器窗口中呈现的每个报表的返回页首。 报表查看器提供的功能包括能够在报表中搜索信息、滚动到特定的页以及调整页大小以便查看。 有关报表工具栏或 HTML 查看器的详细信息，请参阅 [HTML Viewer and the Report Toolbar](../reporting-services/html-viewer-and-the-report-toolbar.md)。

## <a name="browser-support-for-reportviewer-web-server-controls-in-visual-studio"></a>Visual Studio 中 ReportViewer Web 服务器控件的浏览器支持

 ReportViewer Web 服务器控件用于在 ASP.NET Web 应用程序中嵌入报表功能。 这些控件随 Visual Studio 一起提供，并且支持与本主题中所述的其他组件不同的浏览器和浏览器版本。 用于查看应用程序的浏览器类型确定您可以在应用程序中提供的 ReportViewer 功能类型。 使用本主题中提供的表确定哪些支持的浏览器受报表功能的限制以及支持的平台。  

 使用启用了脚本支持的浏览器。 如果浏览器无法运行脚本，则不能查看报表。

**Microsoft Windows**  
*Windows 7、8.1、10；Windows Server 2008 R2、2012、2012 R2*

- Microsoft Edge (+)
- Microsoft Internet Explorer 10 或 11
- Google Chrome (+)
- Mozilla Firefox (+)

 **(+)** 最新公开发布的版本

## <a name="power-view-browser-support"></a>Power View 浏览器支持

**Microsoft Windows**  
*Windows 7、8.1、10；Windows Server 2008 R2、2012、2012 R2*

- Microsoft Internet Explorer 10 或 11
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)

 **(+)** 最新公开发布的版本

::: moniker range="=sql-server-2016"

 有关 SharePoint 2016 浏览器支持的详细信息，请参阅 [在 SharePoint 2013 中计划浏览器支持](https://technet.microsoft.com//library/cc263526\(v=office.16\).aspx)。

::: moniker-end

## <a name="next-steps"></a>后续步骤

[在 Web 门户中查找和查看报表](report-builder/finding-and-viewing-reports-in-the-web-portal-report-builder-and-ssrs.md)  
[Reporting Services 工具](../reporting-services/tools/reporting-services-tools.md)  
[Web 门户（SSRS 本机模式）](./web-portal-ssrs-native-mode.md)  
[HTML 查看器和报表工具栏](../reporting-services/html-viewer-and-the-report-toolbar.md)  
[URL 访问参数引用](../reporting-services/url-access-parameter-reference.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
