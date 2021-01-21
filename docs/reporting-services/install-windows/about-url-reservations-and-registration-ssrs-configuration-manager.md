---
title: URL 预留和注册 (Configuration Manager)| Microsoft Docs
description: Reporting Services 应用程序的 URL 在 HTTP.SYS 中定义为 URL 预留。
ms.date: 01/16/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
helpviewer_keywords:
- URL reservations
- URL registration
- Report Server service, URL reservations
ms.assetid: c2c460c3-e749-4efd-aa02-0f8a98ddbc76
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1203c1190b0c14a4d5d9c3f7218adc8d6ab5d57a
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595287"
---
# <a name="about-url-reservations-and-registration--report-server-configuration-manager"></a>关于 URL 预留和注册（报表服务器配置管理器）
  Reporting Services 应用程序的 URL 在 HTTP.SYS 中定义为 URL 预留。 URL 预留定义了指向 Web 应用程序的 URL 端点的语法。 在报表服务器上配置应用程序时，URL 预留是同时针对报表服务器 Web 服务和 Web 门户进行定义。 通过安装程序或 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具配置 URL 时，会自动为你创建 URL 预留：  
  
-   安装程序将使用默认值创建 URL 预留。 如果安装程序安装默认配置，它将预留两个 URL：一个用于报表服务器 Web 服务，另一个用于 Web 门户。 您可以使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具添加更多 URL 或修改安装程序创建的默认 URL。  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具将根据你在该工具的 **Web 服务 URL** 或 **Web 门户 URL** 页中指定的 URL 创建 URL 预留。  
  
 安装程序和此工具都还会为报表服务器服务分配对 URL 的权限、检查有无重复实例并向 HTTP.SYS 添加 URL 预留。 切勿使用 HttpCfg.exe 或其他工具直接创建或修改 Reporting Services URL 预留。 如果您跳过了某一步骤或设置了无效的值，您将会遇到可能难以诊断或修复的问题。  
  
> [!NOTE]  
> HTTP.SYS 是一个操作系统组件，用于侦听网络请求并将它们路由至请求队列。 在此版 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中，HTTP.SYS 为报表服务器 Web 服务和 Web 门户建立和维护请求队列。 Internet Information Services (IIS) 不再用于承载或访问 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 应用程序。 若要详细了解 HTTP.SYS 功能，请参阅 [HTTP 服务器 API](/windows/win32/http/http-api-start-page)。  
  
##  <a name="urls-in-reporting-services"></a><a name="ReportingServicesURLs"></a> Reporting Services 中的 URL  
 在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装中，可以通过 URL 访问以下工具、应用程序和项：  
  
-   报表服务器 Web 服务  
  
-   Web 门户  
  
-   已发布到报表服务器的报表  
  
 不应通过 URL 将其他已发布的、可通过 URL 寻址的项（如共享数据源）作为独立项进行访问。 当在浏览器窗口中查看这些项时，报表服务器不会以有意义的格式显示这些项。  
  
> [!NOTE]  
> 本主题不介绍如何对报表服务器中存储的特定报表进行 URL 访问。 若要详细了解如何对这些项进行 URL 访问，请参阅[使用 URL 访问来访问报表服务器项](../../reporting-services/access-report-server-items-using-url-access.md)。  
  
##  <a name="url-reservation-and-registration"></a><a name="URLreservation"></a> URL 预留和注册  
 URL 预留定义了可用于访问 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 应用程序的 URL。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 将在 HTTP.SYS 中为报表服务器 Web 服务和 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 保留一个或多个 URL，然后在服务启动时注册它们。 通过向 URL 追加参数，可以通过 Web 服务打开报表。 预留和注册功能是由 HTTP.SYS 提供。 有关详细信息，请参阅[命名空间预留、注册和路由](/windows/win32/http/namespace-reservations-registrations-and-routing)。  
  
 *URL 预留* 是指创建指向 Web 应用程序的 URL 端点并将其存储在 HTTP.SYS 中的过程。 HTTP.SYS 是计算机上定义的所有 URL 预留的公共存储库，它定义了一组保证 URL 预留唯一的公共规则。  
  
  URL 注册在服务启动时进行。 此时会创建请求队列，并且 HTTP.SYS 开始将请求路由到该队列。 必须先注册 URL 端点，然后再将定向至该端点的请求添加到该队列。 当报表服务器服务启动时，它将注册已为所有启用的应用程序保留的所有 URL。 也就是说，必须启用 Web 服务，才能进行注册。 如果在外围应用配置中，对于基于策略的管理的 Reporting Services 方面将 **WebServiceAndHTTPAccessEnabled** 属性设置为 **False** ，则 Web 服务的 URL 在该服务启动时将不会注册。  
  
 如果停止服务或回收 Web 服务或 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 应用程序域，则会撤消注册 URL。 如果在服务正在运行时修改 URL 预留，报表服务器将会立即回收应用程序域，以便可以撤消注册旧的 URL，开始注册新的 URL。  
  
 下面几个简单的示例说明了 URL 预留的概念以及它与用于 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 应用程序的 URL 地址之间的关联。 需要注意一个关键点，即 URL 预留与用于访问应用程序的 URL 具有不同的语法：  
  
|HTTP.SYS 中的 URL 预留|代码|说明|  
|---------------------------------|---------|-----------------|  
|`https://+:80/reportserver`|`https://<computername>/reportserver`<br /><br /> `https://<IPAddress>/reportserver`<br /><br /> `https://localhost/reportserver`|此 URL 预留针对端口 80 指定了一个通配符 (+)。 这会将指定主机（在端口 80 上解析为报表服务器计算机）的任何传入请求放入报表服务器队列中。 请注意，借助此 URL 预留，可以使用任意数目的 URL 访问报表服务器。<br /><br /> 对于大多数操作系统而言，这是 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表服务器的默认 URL 预留。|  
|`https://123.45.67.0:80/reportserver`|`https://123.45.67.0/reportserver`|此 URL 预留指定了一个 IP 地址，与通配符 URL 预留相比，其限制性要强很多。 只能使用包含此 IP 地址的 URL 连接到报表服务器。 给定此 URL 预留，对 `https://<computername>/reportserver` 或 `https://localhost/reportserver` 的报表服务器请求将失败。|  
  
##  <a name="default-urls"></a><a name="DefaultURLs"></a> 默认 URL  
 如果在默认配置中安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ，安装程序将为报表服务器 Web 服务和 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]保留 URL。 在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具中定义 URL 预留时，也可以接受这些默认值。 如果安装 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 或安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 作为命名实例，则默认 URL 将包含实例名称。  
  
> [!IMPORTANT]  
> 实例字符为下划线字符 ( **_** )。  
  
 URL 预留包含一个端口号。 以下操作系统将允许多个 Web 应用程序共享一个端口：  
  
-   [!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2  
  
-   [!INCLUDE[win8srv](../../includes/win8srv-md.md)]  
  
-   [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)]  
  
-   [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)]  
  
-   [!INCLUDE[win7](../../includes/win7-md.md)]  
  
-   [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)]  
  
|实例类型|应用程序|默认 URL|HTTP.SYS 中的实际 URL 预留|  
|-------------------|-----------------|-----------------|----------------------------------------|  
|默认实例|报表服务器 Web 服务|`https://<servername>/reportserver`|`https://<servername>:80/reportserver`|  
|默认实例|Web 门户|`https://<servername>/reports`|`https://<servername>:80/reports`|  
|命名实例|报表服务器 Web 服务|`https://<servername>/reportserver_<instancename>`|`https://<servername>:80/reportserver_<instancename>`|  
|命名实例|Web 门户|`https://<servername>/reports_<instancename>`|`https://<servername>:80/reports_<instancename>`|  
|SQL Server Express|报表服务器 Web 服务|`https://<servername>/reportserver_SQLExpress`|`https://<servername>:80/reportserver_SQLExpress`|  
|SQL Server Express|Web 门户|`https://<servername>/reports_SQLExpress`|`https://<servername>:80/reports_SQLExpress`|  
  
##  <a name="authentication-and-service-identity-for-reporting-services-urls"></a><a name="URLPermissionsAccounts"></a> 用于 Reporting Services URL 的身份验证和服务标识  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] URL 预留显示 URL 预留的帐户。 虚拟服务帐户用于为运行在同一实例上的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 应用程序创建的所有 URL。
  
 
 因为默认安全性为 **RSWindowsNegotiate**，所以匿名访问已禁用。 对于 Intranet 访问，报表服务器 URL 使用网络计算机名称。 如果要为 Internet 连接配置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ，必须使用其他设置。 若要详细了解身份验证，请参阅[向报表服务器进行身份验证](../../reporting-services/security/authentication-with-the-report-server.md)。  
  
##  <a name="urls-for-local-administration"></a><a name="URLlocalAdmin"></a> 用于本地管理的 URL  
 如果为 URL 预留指定了强通配符或弱通配符，则可使用 `https://localhost/reportserver` 或 `https://localhost/reports`。  
  
 `https://localhost` URL 将解释为 `https://127.0.0.1`。 如果你将 URL 预留限定为计算机名称或单个 IP 地址，则除非在本地计算机上为 127.0.0.1 创建附加预留，否则将无法使用 localhost。 同样，如果您的计算机上禁用 localhost 或 127.0.0.1，也无法使用该 URL。  
  
 [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)]， [!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] 和更高版本包括新的安全功能，可以将偶然使用提升的权限运行程序的风险降到最低。 还需要执行一些其他步骤，才能在这些操作系统上启用本地管理。 有关详细信息，请参阅 [为本地管理配置本机模式报表服务器 (SSRS)](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md)。  
  
## <a name="see-also"></a>另请参阅  
 [配置 URL（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)   
 [URL 预留语法（报表服务器配置管理器）](../../reporting-services/install-windows/url-reservation-syntax-ssrs-configuration-manager.md)  

