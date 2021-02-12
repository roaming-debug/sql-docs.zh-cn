---
title: 在 Web 应用程序中使用 URL 访问
description: Reporting Services 中的 URL 访问允许通过网络访问报表，这支持将报表查看和导航集成到自定义 Web 应用程序中。
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
helpviewer_keywords:
- links [Reporting Services], URL access
- URL access [Reporting Services], Web applications
- POST requests
- direct addressing [Reporting Services]
- Web applications [Reporting Services]
- hyperlinks [Reporting Services]
ms.assetid: 39e7918c-ad2d-4ca6-b099-2dd4dbdb83dc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f2df0e4bd2a78b52fcf08587ffed2eff219282b1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074878"
---
# <a name="integrating-reporting-services-using-url-access---web-application"></a>使用 URL 访问集成 Reporting Services - Web 应用程序
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中的 URL 访问是为实现通过网络访问单独报表而专门设计的。 此类型的访问最适合于将报表查看和导航集成到自定义 Web 应用程序中。 为了在 Web 应用程序中使用 URL 访问，您可以：  
  
-   将 URL 从某一网站或门户寻址到特定的报表服务器。  
  
-   使用窗体 POST 方法并使用窗体字段将查询字符串参数传递到报表服务器 URL。  
  
## <a name="url-access-through-direct-addressing"></a>通过直接寻址进行 URL 访问  
 若要使用某一 URL 访问某个报表服务器或报表服务器数据库项，只需从某一 Web 浏览器或应用程序内提供该 URL 地址。 还可以向 URL 提供可能影响所访问的报表或资源的外观的参数。 URL 可以通过 Web 浏览器的地址栏以某一报表服务器为目标，或者 URL 可以是作为更大的 Web 应用程序或门户的一部分的 IFrame 的来源  。 您可以在门户的不同网页上包括指向报表的超链接，以及以报表的特定框架为目标或在这一过程中打开新的浏览器窗口。  
  
 在以下示例中，超链接以名为“main”的框架为目标，该框架可能不同于包括该超链接的框架。 该超链接可能是 Web 门户的一部分。  
  
```  
<a href="https://server/reportserver?/SampleReports/Territory Sales   
Drilldown&rs:Command=Render&rc:LinkTarget=main" target="main" >  
   Click here for the Territory Sales Drilldown sample report  
</a>  
```  
  
 在上例中，设备信息设置 LinkTarget 与值“main”一起传递到 URL 的查询字符串中  。 这确保报表中的所有钻取超链接也以名为“main”的框架为目标。  
  
 有关设备信息设置的详细信息，请参阅[将设备信息设置传递给呈现扩展插件](../../reporting-services/report-server-web-service/net-framework/passing-device-information-settings-to-rendering-extensions.md)。  
  
 请注意，许多服务器和浏览器都限制 URL 中允许的字符数。 在某些情况下，这一限制是 256 字符。 若要避免这一限制，可通过窗体提交使用 POST 请求。  
  
> [!NOTE]  
>  Internet Explorer 具有 2,083 个字符的最大 URL 长度。 这一限制应用于 POST 和 GET 请求 URL。 但是，POST 不受将名称/值对作为窗体的一部分提交的 URL 大小的限制，因为它们在标头中传输，而非在 URL 中传输。  
  
## <a name="url-access-through-a-form-post-method"></a>通过窗体 POST 方法的 URL 访问  
 在某一用户使用 URL 访问从报表服务器请求数据时，该 HTTP 请求使用 GET 方法。 这等效于 METHOD="GET" 的窗体提交。 使用 METHOD="GET" 的 URL 请求或窗体提交受到服务器或 Web 浏览器可处理的最大字符数的限制。  
  
 对于 POST 请求（METHOD="POST" 和输入字段），名称/值对在标头中传输，而非在 URL 中传输。 因此，查询字符串的名称/值对不是 URL 的一部分，这使您可以提供长得多和更复杂的参数列表。  
  
 使用直接访问，用户可以看到报表服务器的 URL，并且可能能够修改查询字符串，或者能够记录特定 URL 请求和报表服务器参数以供以后使用。  
  
 以下 HTML 示例以某一窗体为例，阐释如何使用该窗体将具有特定 URL 的报表服务器为目标并将查询字符串参数作为该窗体的输入字段的一部分传递。  
  
```  
<FORM id="frmRender" action="https://server/reportserver?/SampleReports/  
   Territory Sales Drilldown" method="post" target="_self">  
   <INPUT type="hidden" name="rs:Command" value="Render">   
   <INPUT type="hidden" name="rc:LinkTarget" value="main">  
   <INPUT type="hidden" name="rs:Format" value="HTML4.0">  
   <INPUT type="submit" value="Button">  
</FORM>  
```  
  
 在前一示例中，如果某一用户单击该窗体上的按钮，则报表服务器返回以当前框架为目标的 HTML 呈现的报表。 类似的 URL 访问字符串如下：  
  
```  
https://server/reportserver?/SampleReports/Territory Sales   
Drilldown&rs:Command=Render&rc:LinkTarget=main&rs:Format=HTML4.0  
```  
  
## <a name="see-also"></a>另请参阅  
 [将 Reporting Services 集成到应用程序中](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md)   
 [使用 URL 访问集成 Reporting Services](../../reporting-services/application-integration/integrating-reporting-services-using-url-access.md)   
 [在 Windows 应用程序中使用 URL 访问](../../reporting-services/application-integration/integrating-reporting-services-using-url-access-windows-application.md)   
 [URL 访问 (SSRS)](../../reporting-services/url-access-ssrs.md)  
  
  
