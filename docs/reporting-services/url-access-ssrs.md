---
title: URL 访问 | Microsoft Docs
description: 了解如何借助 SQL Server Reporting Services (SSRS) 中报表服务器的 URL 访问，通过 URL 请求将命令发送到报表服务器。
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- URL access [Reporting Services]
- links [Reporting Services], URL access
- URL access [Reporting Services], about URL access
- parameters [Reporting Services], URL access
- report servers [Reporting Services], URL access
- hyperlinks [Reporting Services]
ms.assetid: 52c3f2a3-3d6d-4fee-9c46-83f366919398
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 54c41a9b4a735d5136cf86e805836305b2031bb7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078898"
---
# <a name="url-access-ssrs"></a>URL 访问 (SSRS)
  通过 SQL Server Reporting Services (SSRS) 中报表服务器的 URL 访问，您可以通过 URL 请求将命令发送到报表服务器。 例如，您可以自定义报表在本机模式报表服务器上或 SharePoint 库中的呈现。 您可能已经使用了一组特定的报表参数值查看了报表，或者可能正在查看报表中感兴趣的特定页。 您可以使用预定义的 URL 访问参数在 URL 中封装这些信息。 您可以通过为呈现格式嵌入参数，或者为报表查看器的外观嵌入参数，进一步自定义报表服务器处理报表的方式。 然后，您可以将此 URL 直接粘贴到电子邮件或网页中，让他人在浏览器中采用相同的方式访问您的报表。  
  
 您可以通过 URL 访问执行的其他操作包括：  
  
-   将命令发送到 HTML 查看器，例如调整其外观  
  
-   列出目录文件夹的子级  
  
-   检索目录项的 XML 定义  
  
-   呈现特定的报表历史记录快照  
  
-   管理报表会话  
  
 有关可通过 URL 访问提供的命令和设置的完整列表，请参阅 [URL 访问参数引用](../reporting-services/url-access-parameter-reference.md)。  
  
## <a name="url-access-concepts"></a>URL 访问概念  
 对报表服务器的 URL 请求包含报表服务器处理的参数。 报表服务器处理 URL 请求的方式取决于在 URL 中包括的参数、参数前缀和项的类型。 报表服务器 URL 符合万维网联合会 W3C/IETF 标准草案建议的 URL 格式准则。 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] URL 功能与支持标准 URL 寻址的大多数 Internet 浏览器或应用程序兼容。  
  
### <a name="url-access-syntax"></a>URL 访问语法  
 URL 请求可包含以任何顺序列出的多个参数。 参数通过“与”符号 (&) 分隔开，名称/值对通过等号 (=) 分隔开。  
  
```  
  
rswebserviceurl  
?  
reportpath  
      [&prefix:param=value]...n]  
  
```  
  
### <a name="syntax-description"></a>语法说明  
 *rswebserviceurl*  
 报表服务器的 Web 服务 URL。 对于本机模式，它是在 Reporting Services 配置管理器中配置的报表服务器实例的 Web 服务 URL（请参阅[配置报表服务器 URL（报表服务器配置管理器）](../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)）。 例如：  
  
```  
https://myrshost/reportserver  
https://machine.adventure-works.com/reportserver_MYNAMEDINSTANCE  
```  
  
 对于 SharePoint 集成模式，它是位于与 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 集成的 SharePoint 站点处的 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]代理的 URL。 例如：  
  
```  
https://myspsite/subsite/_vti_bin/reportserver  
```  
  
> [!TIP]  
>  非常重要的一点是，URL 包括用于通过 SharePoint 和 `_vti_bin` HTTP 代理路由请求的 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 代理语法。 该代理会向 HTTP 请求中添加某一上下文，该上下文是确保为 SharePoint 模式报表服务器正确执行报表所需要的。  
  
 *pathinfo*  
 本机模式报表服务器数据库中该项的相对路径名称，或者是 SharePoint 目录中该项的完全限定 URL。  
  
 目录项的路径。 对于本机模式，它是报表服务器数据库中该项的相对路径，以斜杠 ( **/** ) 开头。 例如：  
  
```  
/AdventureWorks 2008R2/Employee_Sales_Summary_2008R2  
```  
  
 对于 SharePoint 集成模式，它是 SharePoint 库中的项的完全限定 URL，包括项扩展名。 例如：  
  
```  
https://myspsite/subsite/AdventureWorks 2008R2/Employee_Sales_Summary_2008R2.rdl  
```  
  
 **&**  
 用于分隔 URL 访问参数的名称和值对。  
  
 **prefix**  
 可选。 访问在报表服务器内正运行的特定进程的 URL 访问参数的前缀（例如 `rs:` 或 `rc:`）。  
  
> [!NOTE]  
>  如果不包括用于某一 URL 访问参数的前缀，则该参数将被报表服务器作为报表参数处理。 报表参数不使用参数前缀并且区分大小写。  
  
 **param**  
 参数名称。  
  
 *value*  
 与要使用的参数值相对应的 URL 文本。  
  
 **注意：** 有关可用 URL 访问参数的列表，请参阅 [URL 访问参数参考](../reporting-services/url-access-parameter-reference.md)。 有关在 URL 中传递报表参数的示例，请参阅 [Pass a Report Parameter Within a URL](../reporting-services/pass-a-report-parameter-within-a-url.md)。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|链接|  
|-----------------------|-----------|  
|访问报表服务器项，例如报表、共享数据源和资源。|[使用 URL 访问来访问报表服务器项](../reporting-services/access-report-server-items-using-url-access.md)|  
|将报表参数传递到报表。|[在 URL 内传递报表参数](../reporting-services/pass-a-report-parameter-within-a-url.md)|  
|设置 URL 访问字符串中报表参数的区域设置，它定义日期、货币等特定于区域设置的解释。|[设置 URL 中的报表语言参数](../reporting-services/set-the-language-for-report-parameters-in-a-url.md)|  
|发送自定义报表呈现方式的报表扩展插件特定的设置。|[在 URL 中指定设备信息设置](../reporting-services/specify-device-information-settings-in-a-url.md)|  
|将报表直接导出到某一文件格式而无需在浏览器中查看它。|[使用 URL 访问导出报表](../reporting-services/export-a-report-using-url-access.md)|  
|打开报表并且直接导航到某一字符串位置。|[使用 URL 访问搜索报表](../reporting-services/search-a-report-using-url-access.md)|  
|呈现特定的报表历史记录快照。|[使用 URL 访问呈现报表历史记录快照](../reporting-services/render-a-report-history-snapshot-using-url-access.md)|  
  
## <a name="see-also"></a>另请参阅  
 [Pass a Report Parameter Within a URL](../reporting-services/pass-a-report-parameter-within-a-url.md)   
 [URL 访问参数引用](../reporting-services/url-access-parameter-reference.md)   
 [使用 URL 访问集成 Reporting Services](../reporting-services/application-integration/integrating-reporting-services-using-url-access.md)   
 [查找、查看和管理报表（报表生成器和 SSRS）](../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md)  
  
  
