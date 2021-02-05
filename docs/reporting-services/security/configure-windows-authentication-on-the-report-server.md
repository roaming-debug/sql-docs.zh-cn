---
description: 在报表服务器上配置 Windows 身份验证
title: 在报表服务器上配置 Windows 身份验证 | Microsoft Docs
ms.date: 06/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [Reporting Services]
- Reporting Services, configuration
ms.assetid: 4de9c3dd-0ee7-49b3-88bb-209465ca9d86
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 22a9134ed3caf220dbe895d7e4de8b9d52290e08
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536328"
---
# <a name="configure-windows-authentication-on-the-report-server"></a>在报表服务器上配置 Windows 身份验证
  默认情况下， [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 接受指定 Negotiate 或 NTLM 身份验证的请求。 如果部署中包括使用这些安全提供程序的客户端应用程序和浏览器，则可以使用这些默认值，而无需附加配置。 如果要使用不同的安全提供程序来获取 Windows 集成安全性（例如，如果要直接使用 Kerberos）或者修改了默认值并且要还原原始设置，则可以使用本主题中的信息来指定报表服务器上的身份验证设置。  
  
 若要使用 Windows 集成安全性，则每个需要访问报表服务器的用户都必须拥有有效的 Windows 本地或域用户帐户，或者必须是 Windows 本地或域组帐户的成员。 对于其他域，只要这些域可信，您也可以包括其中的帐户。 这些帐户必须具有报表服务器计算机的访问权，并且必须随后分配给相应的角色以便获得特定报表服务器操作的访问权。  
  
 同时，必须满足下列附加要求：  
  
-   RSReportServer.config 文件必须具有设置为 RSWindowsNegotiate、RSWindowsKerberos 或 RSWindowsNTLM 的 AuthenticationType     。 默认情况下，如果报表服务器服务帐户是 NetworkService 或 LocalSystem，则 RSReportServer.config 文件包含 **RSWindowsNegotiate** 设置；否则使用 **RSWindowsNTLM** 设置。 如果拥有仅使用 Kerberos 身份验证的应用程序，则可以添加 **RSWindowsKerberos** 。  
  
    > [!IMPORTANT]  
    >  如果将报表服务器服务配置为在域用户帐户下运行，并且没有为该帐户注册服务主体名称 (SPN)，使用 **RSWindowsNegotiate** 将导致 Kerberos 身份验证错误。 有关详细信息，请参阅本主题中的 [连接到报表服务器时纠正 Kerberos 身份验证错误](#proxyfirewallRSWindowsNegotiate) 。  
  
-   [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 。 默认情况下，报表服务器 Web 服务的 Web.config 文件包括 \<authentication mode="Windows"> 设置。 如果将其更改为 \<authentication mode="Forms">，则 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的 Windows 身份验证将失败。  
  
-   报表服务器 Web 服务的 Web.config 文件必须具有 \<identity impersonate= "true" />。  
  
-   客户端应用程序或浏览器必须支持 Windows 集成安全性。  

- [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 不需要其他配置。
  
 若要更改报表服务器身份验证设置，需要在 RSReportServer.config 文件中编辑 XML 元素和值。 可以复制并粘贴本主题中的示例来实现特定的组合。  
  
 如果所有客户端计算机和服务器计算机均位于相同的域或某个可信域中，并且报表服务器是针对企业防火墙之后的 Intranet 访问而部署的，则默认设置将达到最佳效果。 可信域和单个域是传递 Windows 凭据所必需的。 如果为服务器启用 Kerberos 版本 5 协议，则可以多次传递凭据。 否则，凭据在过期之前只能传递一次。 有关为多计算机连接配置凭据的详细信息，请参阅 [指定报表数据源的凭据和连接信息](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)。  
  
 以下说明针对本机模式报表服务器。 如果在 SharePoint 集成模式下部署报表服务器，则必须使用指定 Windows 集成安全性的默认身份验证设置。 报表服务器使用默认 Windows 身份验证扩展插件中的内部功能支持 SharePoint 集成模式下的报表服务器。  
  
## <a name="extended-protection-for-authentication"></a>身份验证的扩展保护  
 自 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]开始，提供了对针对验证的扩展保护的支持。 此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能支持使用渠道绑定和服务绑定来加强对身份验证的保护。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 功能需要用于支持扩展保护的操作系统。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置由 RSReportServer.config 文件中的设置确定。 可以通过编辑此文件或使用 WMI API 来更新此文件。 有关详细信息，请参阅 [Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)。  
  
### <a name="to-configure-a-report-server-to-use-windows-integrated-security"></a>将报表服务器配置为使用 Windows 集成安全性  
  
1.  在文本编辑器中打开 RSReportServer.config。  
  
2.  查找 \<**Authentication**>。  
  
3.  复制以下最能满足您需要的一个 XML 结构。 可以按任意顺序指定 **RSWindowsNegotiate**、 **RSWindowsNTLM** 和 **RSWindowsKerberos** 。 如果要对连接而非每一单个请求进行身份验证，则应启用身份验证持久性。 在身份验证持久性下，在连接期间将允许所有要求身份验证的请求。  
  
     当报表服务器服务帐户是 NetworkService 或 LocalSystem 时，第一个 XML 结构是默认配置：  
  
    ```  
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsNegotiate />  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    </Authentication>  
    ```  
  
     当报表服务器服务帐户不是 NetworkService 或 LocalSystem 时，第二个 XML 结构是默认配置：  
  
    ```  
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsNTLM />  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    ```  
  
     \</Authentication>  
  
     第三个 XML 结构指定 Windows 集成安全性中使用的所有安全包：  
  
    ```  
          <AuthenticationTypes>  
                 <RSWindowsNegotiate />  
                 <RSWindowsKerberos />  
                 <RSWindowsNTLM />  
          </AuthenticationTypes>  
    ```  
  
     第四个 XML 结构仅为不支持 Kerberos 的部署指定 NTLM，或解决 Kerberos 身份验证错误：  
  
    ```  
          <AuthenticationTypes>  
                 <RSWindowsNTLM />  
          </AuthenticationTypes>  
    ```  
  
4.  将其粘贴在 \<**Authentication**> 的现有条目上。  
  
     注意，不能将 **Custom** 与 **RSWindows** 类型一起使用。  
  
5.  根据需要适当修改扩展保护的设置。 默认情况下，扩展保护处于禁用状态。  如果这些条目不存在，则当前计算机运行的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 版本可能不支持扩展保护。 有关详细信息，请参阅 [Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)  
  
    ```  
          <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>  
          <RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionScenario>  
    ```  
  
6.  保存文件。  
  
7.  如果配置了扩展部署，请对该部署中的其他报表服务器重复这些步骤。  
  
8.  重新启动报表服务器以清除当前打开的任何会话。  
  
##  <a name="resolving-kerberos-authentication-errors-when-connecting-to-a-report-server"></a><a name="proxyfirewallRSWindowsNegotiate"></a> 连接到报表服务器时解决 Kerberos 身份验证错误  
 在为 Negotiate 或 Kerberos 身份验证配置的报表服务器上，如果出现 Kerberos 身份验证错误，则客户端与报表服务器的连接将失败。 已知在以下情况下会出现 Kerberos 身份验证错误：  
  
-   报表服务器服务作为 Windows 域用户帐户运行并且您没有为该帐户注册服务主体名称 (SPN)。  
  
-   报表服务器是使用 **RSWindowsNegotiate** 设置配置的。  
  
-   浏览器选择它发送给报表服务器的请求的身份验证标头中 NTLM 上的 Kerberos。  
  
 如果启用了 Kerberos 日志记录，则可以检测到该错误。 另一错误症状是多次提示您输入凭据，然后您看到一个空的浏览器窗口。  
  
 可以通过从配置文件中删除 < RSWindowsNegotiate /> 并重新尝试建立连接来确认你遇到了 Kerberos 身份验证错误。  
  
 确认遇到该问题之后，可以采用下列方法解决它：  
  
-   在域用户帐户下为报表服务器服务注册 SPN。 有关详细信息，请参阅[为报表服务器注册服务主体名称 (SPN)](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md)。  
  
-   将服务帐户更改为在网络服务等内置帐户下运行。 内置帐户将 HTTP SPN 映射到将计算机联网时定义的 Host SPN。 有关详细信息，请参阅[配置服务帐户（报表服务器配置管理器）](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)。
  
-   使用 NTLM。 通常，NTLM 将在 Kerberos 身份验证失败时发挥作用。 若要使用 NTLM，从 RSReportServer.config 文件中删除 **RSWindowsNegotiate** 并验证仅指定了 **RSWindowsNTLM** 。 如果选择此方法，则可以继续将域用户帐户用于报表服务器服务（即使没有为其定义 SPN）。  
  
#### <a name="logging-information"></a>日志记录信息  
 多种日志记录信息源可以帮助解决与 Kerberos 相关的问题。  
  
##### <a name="user-account-control-attribute"></a>用户-帐户-控制属性  
 确定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务帐户在 Active Directory 中是否具有足够的属性集。 检查 Reporting Services 服务跟踪日志文件，以找到为 UserAccountControl 属性记录的值。 记录的值为十进制格式。 您需要将十进制值转换为十六进制格式，然后在描述用户-帐户-控制属性的 MSDN 主题中找到该值。  
  
-   Reporting Services 服务跟踪日志条目类似于以下内容：  
  
    ```  
    appdomainmanager!DefaultDomain!8f8!01/14/2010-14:42:28:: i INFO: The UserAccountControl value for the service account is 590336  
    ```  
  
-   用于将十进制值转换为十六进制格式的一个选项是使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 计算器。 Windows 计算器支持多种显示“十进制”选项和“十六进制”选项的模式。 选择“十进制”选项，粘贴或键入在日志文件中找到的十进制值，然后选择“十六进制”选项。  
  
-   然后，参阅主题 [用户-帐户-控制属性](/windows/win32/adschema/a-useraccountcontrol) 以获得服务帐户的属性。  
  
##### <a name="spns-configured-in-active-directory-for-the-reporting-services-service-account"></a>在 Active Directory 中为 Reporting Services 服务帐户配置的 SPN。  
 若要在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务跟踪日志文件中记录 SPN，可以临时启用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 扩展保护功能。  
  
-   通过设置以下内容修改配置文件 **rsreportserver.config** ：  
  
    ```  
    <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>   
    <RSWindowsExtendedProtectionScenario>Any</RSWindowsExtendedProtectionScenario>  
    ```  
  
-   重新启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务。

 如果不希望继续使用扩展保护，则将配置值重新设置为默认值，然后重新启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务帐户。  
  
```  
<RSWindowsExtendedProtectionLevel>Off</RSWindowsExtendedProtectionLevel>  
<RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionScenario>  
```  
  
 有关详细信息，请参阅 [Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)。  
  
#### <a name="how-the-browser-chooses-negotiated-kerberos-or-negotiated-ntlm"></a>浏览器如何选择协商 Kerberos 或协商 NTLM  
 使用 Internet Explorer 连接到报表服务器时，它将在身份验证标头中指定协商 Kerberos 或 NTLM。 在以下情况下，使用 NTLM 而非 Kerberos：  
  
-   将请求发送到本地报表服务器。  
  
-   将请求发送到报表服务器计算机的 IP 地址而非主机标头或服务器名称。  
  
-   防火墙软件阻塞了用于 Kerberos 身份验证的端口。  
  
-   特定服务器的操作系统不启用 Kerberos。  
  
-   域中包括旧版 Windows 客户端和服务器操作系统，不支持新版操作系统中内置的 Kerberos 功能。  
  
 此外，Internet Explorer 还可能选择协商 Kerberos 或 NTLM，这取决于您如何配置的 URL、LAN 和代理设置。  
  
###### <a name="report-server-url"></a>报表服务器 URL  
 如果该 URL 包括完全限定域名，则 Internet Explorer 将选择 NTLM。 如果该 URL 指定本地主机，则 Internet Explorer 选择 NTLM。 如果该 URL 指定计算机的网络名称，则 Internet Explorer 将选择协商，这将会成功或失败，取决于报表服务器服务帐户是否存在 SPN。  
  
###### <a name="lan-and-proxy-settings-on-the-client"></a>客户端上的 LAN 和代理设置  
 在 Internet Explorer 中设置的 LAN 和代理设置可以确定是否在 Kerberos 上选择 NTLM。 但是，由于各组织的 LAN 和代理设置会有所不同，因此不可能准确确定造成 Kerberos 身份验证错误的设置。 例如，您的组织可能会强制实施将 URL 从 Intranet URL 转换为通过 Internet 连接解析的完全限定域名 URL 的代理设置。 如果将不同的身份验证提供程序用于不同类型的 URL，则您可能会发现原本预计会失败的某些连接竟然成功了。  
  
 如果遇到您认为因身份验证失败而引起的连接错误，则可以尝试其他 LAN 与代理设置的组合来隔离该问题。 在 Internet Explorer 中，LAN 和代理设置位于“局域网 (LAN) 设置”对话框中，可以通过单击“Internet 选项”的“连接”选项卡上的“LAN 设置”打开该对话框。  
  
## <a name="external-resources"></a>外部资源  
  
-   有关 Kerberos 和报表服务器的更多信息，请参阅 [Deploying a Business Intelligence Solution Using SharePoint, Reporting Services, and PerformancePoint Monitoring Server with Kerberos](https://www.kasperonbi.com/deploying-a-business-intelligence-solution-using-sharepoint-reporting-services-and-performancepoint-monitoring-server-with-kerberos/)（通过 Kerberos 部署使用 SharePoint、Reporting Services 和 PerformancePoint Monitoring Server 的商业智能解决方案）。  
  
## <a name="see-also"></a>另请参阅  
 [针对报表服务器的身份验证](../../reporting-services/security/authentication-with-the-report-server.md)   
 [授予对本机模式报表服务器的权限](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
 [RsReportServer.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [在报表服务器上配置基本身份验证](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)   
 [在报表服务器上配置自定义身份验证或窗体身份验证](../../reporting-services/security/configure-custom-or-forms-authentication-on-the-report-server.md)   
 [Reporting Services 针对验证的扩展保护](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)  
  
