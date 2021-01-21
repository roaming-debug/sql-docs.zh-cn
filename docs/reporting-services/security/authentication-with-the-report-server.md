---
description: 针对报表服务器的身份验证
title: 针对报表服务器的身份验证 | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- connections [Reporting Services], configuring
- connections [Reporting Services], accounts
- Windows authentication [Reporting Services]
- authentication [Reporting Services]
- Forms authentication
ms.assetid: 753c2542-0e97-4d8f-a5dd-4b07a5cd10ab
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8f682e468bffe704df57cb63b811682488909605
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595200"
---
# <a name="authentication-with-the-report-server"></a>针对报表服务器的身份验证

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] (SSRS) 提供了几个可配置的选项，用于对报表服务器验证用户和客户端应用程序。 默认情况下，报表服务器使用 Windows 集成身份验证并且假定信任关系，其中，客户端和网络资源处于同一域中或处于信任域中。 根据你的网络拓扑和组织需要，你可以自定义用于 Windows 集成身份验证的身份验证协议，使用基本身份验证，或者使用你提供的基于窗体的自定义身份验证扩展插件。 每种身份验证类型都可以单独打开或关闭。 如果您希望报表服务器接受多种类型的请求，则可启用多种身份验证类型。
  
 请求对报表服务器内容或操作进行访问的所有用户或应用程序都必须首先进行身份验证，然后才允许访问。  
  
## <a name="authentication-types"></a>身份验证类型  
 请求对报表服务器内容或操作进行访问的所有用户或应用程序都必须首先使用对报表服务器配置的身份验证类型进行身份验证，然后才允许访问。 下表介绍了 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]支持的身份验证类型。  
  
|AuthenticationType 名称|HTTP 身份验证层值|默认情况下是否使用|说明|  
|-----------------------------|-------------------------------------|---------------------|-----------------|  
|RSWindowsNegotiate|Negotiate|是|尝试首先将 Kerberos 用于 Windows 集成的身份验证，但是如果 Active Directory 无法将客户端请求的票证授予报表服务器，则回退到 NTLM。 仅当票证不可用时，Negotiate 才会回退到 NTLM。 如果第一次尝试导致出错而不是缺少票证，则报表服务器不会进行再次尝试。|  
|RSWindowsNTLM|NTLM|是|将 NTLM 用于 Windows 集成的身份验证。<br /><br /> 将不会在其他请求中对凭据进行委托或模拟。 后续请求将遵循新的质询响应顺序。 根据网络安全设置的不同，系统可能提醒用户输入凭据，或者会以透明方式处理身份验证请求。|  
|RSWindowsKerberos|Kerberos|否|将 Kerberos 用于 Windows 集成的身份验证。 您必须通过为您的服务帐户设置服务主体名称 (SPN) 来配置 Kerberos，这要求域管理员权限。 如果使用 Kerberos 来设置标识委托，则还可在为报表提供数据的外部数据源的其他连接中使用请求报表的用户的令牌。<br /><br /> 在指定 RSWindowsKerberos 之前，请确保您所使用的浏览器类型确实支持该值。 如果使用的是 Microsoft Edge 或 Internet Explorer，则 Kerberos 身份验证只能通过 Negotiate 进行支持。 Microsoft Edge 或 Internet Explorer 将不表述直接指定 Kerberos 的身份验证请求。|  
|RSWindowsBasic|基本|否|基本身份验证是在 HTTP 协议中定义的，并只能用于对向报表服务器发出的 HTTP 请求进行身份验证。<br /><br /> 凭据以 base64 编码形式在 HTTP 请求中传递。 如果使用基本身份验证，请先使用传输层安全性 (TLS)（旧称为“安全套接字层 (SSL)”）对用户帐户信息进行加密，再通过网络发送这些信息。 SSL 提供一个加密通道，可借助此通道通过 HTTP TCP/IP 连接将连接请求从客户端发送到报表服务器。 有关详细信息，请参阅 [TechNet 网站中的](/previous-versions/windows/it-pro/windows-server-2003/cc738495(v=ws.10)) Using SSL to Encrypt Confidential Data [!INCLUDE[msCoName](../../includes/msconame-md.md)] （使用 SSL 加密机密数据）。|  
|自定义|(Anonymous)|否|匿名身份验证将指示报表服务器忽略 HTTP 请求中的身份验证标头。 报表服务器接受所有请求，但不接受您所提供用来对用户进行身份验证的自定义 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 窗体身份验证的调用请求。<br /><br /> 仅当要在报表服务器上部署处理所有身份验证请求的自定义身份验证模块时，才指定 **Custom** 。 不能将 Custom 身份验证类型与默认的 Windows 身份验证扩展插件一起使用。|  
  
## <a name="unsupported-authentication-methods"></a>不支持的身份验证方法  
 不支持下列身份验证方法和请求。  
  
|身份验证方法|说明|  
|---------------------------|-----------------|  
|匿名|报表服务器将不接受来自匿名用户的未经身份验证的请求，但包含自定义身份验证扩展插件的那些部署除外。<br /><br /> 如果对配置为使用基本身份验证的报表服务器启用报表生成器访问，则报表生成器将接受未经身份验证的请求。<br /><br /> 对于所有其他情况，在匿名请求到达 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]前将拒绝该请求，同时生成 HTTP 状态 401 拒绝访问错误。 客户端收到 401 拒绝访问错误后必须使用有效的身份验证类型重新表述该请求。|  
|单一登录技术 (SSO)|在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]中不提供对单一登录技术的本机支持。 如果希望使用单一登录技术，则必须创建自定义身份验证扩展插件。<br /><br /> 报表服务器宿主环境不支持 ISAPI 筛选器。 如果您使用的 SSO 技术以 ISAPI 筛选器形式实现，请考虑使用 RSASecueID 或 RADIUS 协议的 ISA Server 内置支持。 另外，还可以创建 ISA Server ISAPI 或 RS 的 HTTPModule，但是建议您直接使用 ISA Server。|  
|Passport|在 SQL Server Reporting Services 中不受支持。|  
|摘要|在 SQL Server Reporting Services 中不受支持。|  
  
## <a name="configuration-of-authentication-settings"></a>身份验证设置的配置  
 保留报表服务器 URL 后，身份验证设置将配置为使用默认的安全设置。 如果错误地修改了这些设置，则报表服务器将返回 HTTP 401 拒绝访问错误，该错误针对无法通过身份验证的 HTTP 请求。 选择身份验证类型要求您已了解在网络中是如何支持 Windows 身份验证。 必须指定至少一种身份验证类型。 可为 RSWindows 指定多种身份验证类型。 RSWindows 身份验证类型（即 **RSWindowsBasic**、 **RSWindowsNTLM**、 **RSWindowsKerberos** 和 **RSWindowsNegotiate**）与自定义身份验证类型相互排斥。  
  
> [!IMPORTANT]  
>  Reporting Services 不验证您指定的设置以确定它们对于您的计算环境来说是否正确。 有可能默认安全设置将对您的安装无效，也有可能将指定对安全基础结构无效的配置设置。 因此，使报表服务器部署在较大的单位中可用之前，先在受控的测试环境中仔细测试该报表服务器部署非常重要。  
  
 报表服务器 Web 服务和 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 始终使用相同的身份验证类型。 不能为报表服务器服务的各功能区配置不同的身份验证类型。 如果您具有扩展部署，请确保在部署的所有节点上重复所有更改。 不能将同一扩展中的不同节点配置为使用不同的身份验证类型。  
  
 后台处理不接受来自最终用户的请求，但是会对所有请求进行身份验证以发现无人参与的执行。 后台处理始终使用 Windows 身份验证，并使用报表服务器服务或无人参与的执行帐户（如果已配置）对请求进行身份验证。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [在报表服务器上配置 Windows 身份验证](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md)  
  
-   [在报表服务器上配置基本身份验证](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)  
  
-   [在报表服务器上配置自定义身份验证或窗体身份验证](../../reporting-services/security/configure-custom-or-forms-authentication-on-the-report-server.md)  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|链接|  
|-----------------------|-----------|  
|配置 Windows 集成身份验证类型。|[在报表服务器上配置 Windows 身份验证](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md)|  
|配置基本身份验证类型。|[在报表服务器上配置基本身份验证](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)|  
|配置窗体身份验证或自定义身份验证类型。|[在报表服务器上配置自定义身份验证或窗体身份验证](../../reporting-services/security/configure-custom-or-forms-authentication-on-the-report-server.md)|  
|启用 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 来处理自定义的身份验证方案。|[配置 Web 门户以传递自定义身份验证 Cookie](configure-the-web-portal-to-pass-custom-authentication-cookies.md)|  

## <a name="next-steps"></a>后续步骤

[授予对本机模式报表服务器的权限](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
[RsReportServer.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[创建和管理角色分配](../../reporting-services/security/create-and-manage-role-assignments.md)   
[为报表数据源指定凭据和连接信息](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)   
[实现安全扩展插件](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)   
[在本机模式报表服务器上配置 TLS 连接](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md)   
[安全扩展插件概述](../../reporting-services/extensions/security-extension/security-extensions-overview.md)   
[Reporting Services 中的身份验证](../../reporting-services/extensions/security-extension/authentication-in-reporting-services.md)   
[Reporting Services 中的授权](../../reporting-services/extensions/security-extension/authorization-in-reporting-services.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)