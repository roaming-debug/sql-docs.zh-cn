---
description: 在本机模式报表服务器上配置 TLS 连接
title: 在本机模式报表服务器上配置 TLS 连接 | Microsoft Docs
ms.date: 09/27/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Secure Sockets Layer (SSL)
ms.assetid: 212f2042-456a-4c0a-8d76-480b18f02431
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0532c42a5bd080b7f686c1663c6f88d8066bc9ff
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076259"
---
# <a name="configure-tls-connections-on-a-native-mode-report-server"></a>在本机模式报表服务器上配置 TLS 连接

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式使用 HTTP SSL（安全套接字层）服务建立到报表服务器的加密连接。 传输层安全性 (TLS) 旧称为“安全套接字层 (SSL)”。 如果在报表服务器计算机的本地证书存储区中安装证书 (.cer) 文件，则可将该证书绑定到 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] URL 预留，以支持通过加密通道建立报表服务器连接。  
  
> [!TIP]  
>  如果您在使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式，请参阅 SharePoint 文档了解有关的详细信息。 例如，[如何在 SharePoint 2010 Web 应用程序上启用 TLS](/archive/blogs/sowmyancs/how-to-enable-ssl-on-a-sharepoint-2010-web-application)。  
  
 由于 Internet Information Services (IIS) 也使用 HTTP SSL，因此在同一计算机上运行 IIS 和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 时，必须考虑到一些重要的互操作性问题。 请务必查看“与 IIS 的互操作性问题”部分以获取有关如何解决这些问题的指南。  
  
## <a name="server-certificate-requirements"></a>服务器证书要求  
 您必须在计算机上安装服务器证书（不支持客户端证书）。 Reporting Services 不提供用于请求、生成、下载或安装证书的功能。 Windows Server 2012 和更高版本提供可用于从可信证书颁发机构请求证书的证书管理单元。  
  
 出于测试目的，可以在本地生成证书。 如果将 **MakeCert** 实用工具和示例命令用作模板，请确保在运行该命令之前将服务器名称指定为主机并删除所有换行符。 如果在 DOS 窗口中运行命令，则可能需要增加窗口的缓冲区大小以容纳整条命令。  
  
 如果正在同一台计算机上一起运行 IIS 和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ，则可以使用 IIS 管理器控制台应用程序获取计算机上安装的证书。 IIS 管理器包含可用于创建和打包证书请求 (.crt) 文件的选项，以便可信证书颁发机构进行后续处理。 您使用的证书颁发机构将生成一个证书 (.cer) 文件，并将它发送回给您。 您可以使用 IIS 管理控制台在本地存储区中安装该证书文件。 有关详细信息，请参阅 Technet 上的 [Using SSL to Encrypt Confidential Data](/previous-versions/windows/it-pro/windows-server-2003/cc738495(v=ws.10)) （使用 SSL 加密机密数据）。  
  
## <a name="interoperability-issues-with-iis"></a>与 IIS 的互操作性问题  
 如果同一计算机上同时有 IIS 和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]，则会严重影响与报表服务器的 TLS 连接：  
  
-   如果安装了 IIS，则必须始终运行万维网 (W3SVC) 服务。 如果 HTTP SSL 服务检测到 IIS 正在运行，则将生成 IIS 的一个依赖项。 也就是说，只要 IIS 和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装在同一计算机上，且你要为报表服务器 URL 配置 TLS 连接，万维网服务 (W3SVC) 就必须处于运行状态。  
  
-   卸载 IIS 可能会暂时中断服务连接到与 TLS 绑定的报表服务器 URL。 因此，建议你在卸载 IIS 后重启计算机。  
  
     必须重新启动计算机，才能从缓存中清除所有 TLS 会话。 一些操作系统最长可以将 TLS 会话缓存 10 小时，导致 https:// URL 继续有效，即使在从 HTTP.SYS 的 URL 预留中删除 TLS 绑定后，也继续有效。 重新启动计算机会关闭使用通道的所有已打开的连接。  
  
## <a name="bind-tls-to-a-reporting-services-url-reservation"></a>将 TLS 绑定到 Reporting Services URL 预留  
 下列步骤不包括有关请求、生成、下载或安装证书的说明。 证书必须已安装并可供使用。 指定的证书属性、从中获取证书的证书颁发机构以及用于请求和安装证书的工具和实用工具都由您决定。  
  
 可以使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具绑定证书。 如果在本地计算机存储区正确安装了证书，则 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具将检测到该证书并将它显示在“Web 服务 URL”  页和“Web 门户 URL”  页上的“SSL 证书”  列表中。  
  
### <a name="to-configure-a-report-server-url-for-tls"></a>为报表服务器 URL 配置 TLS 的具体步骤  
  
1.  启动 Reporting Services 配置工具，然后连接到报表服务器。  
  
2.  选择“Web 服务 URL”  。  
  
3.  展开 TLS/SSL 证书列表。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 将在本地存储区中检测服务器身份验证证书。 如果已安装证书，但在列表中看不到该证书，则可能需要重新启动服务。 可以使用 Reporting Services 配置工具中“报表服务器状态”  页的“停止”  和“启动”  按钮重启服务（首页）。  
  
4.  选择证书。  
  
5.  单击“应用”  。  
  
6.  单击该 URL，验证其是否有效。  
  
 报表服务器数据库配置是测试 URL 所必需的。 如果您尚未创建报表服务器数据库，请在测试 URL 之前先创建该数据库。  
  
 Web 门户 URL 和报表服务器 Web 服务 URL 的 URL 预留都是单独配置的。 如果还要为 Web 门户配置通过 TLS 加密的通道进行访问，请继续执行以下步骤：  
  
1.  访问“Web 门户 URL”  。
  
2.  选择“高级”。   
  
3.  在“当前 Reporting Services 功能的多个 HTTPS 标识”  中，选择“添加”  。  
  
4.  选择证书，选择“确定”  ，然后选择“应用”  。  
  
5.  测试该 URL，验证其是否有效。  
  
## <a name="how-certificate-bindings-are-stored"></a>证书绑定的存储方式  
 证书绑定将存储在 HTTP.SYS 中。 已定义的绑定的表示形式还将存储在 RSReportServer.config 文件的 **URLReservations** 部分。 配置文件中的设置只是在其他地方指定的实际值的表示形式。 请不要直接在该配置文件中修改这些值。 仅当您使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具或报表服务器 Windows Management Instrumentation (WMI) 提供程序绑定证书后，才会在文件中显示配置设置。  
  
> [!NOTE]  
>  如果在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中为绑定配置 TLS/SSL 证书，但稍后又想要从计算机中删除证书，请务必先从 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中删除绑定，再从计算机中删除证书。 否则，将无法使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具或 WMI 删除绑定，您将收到“参数无效”错误。 如果已从计算机删除了证书，则可以使用 Httpcfg.exe 工具从 HTTP.SYS 删除绑定。 有关 Httpcfg.exe 的详细信息，请参阅 Windows 产品文档。  
  
 TLS 绑定是 Microsoft Windows 中的共享资源。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器或诸如 IIS 管理器之类的其他工具所做的更改可能会影响同一计算机上的其他应用程序。 编辑绑定所用的工具最好与创建绑定所用的工具相同。  例如，如果使用配置管理器创建了 TLS 绑定，建议使用配置管理器来管理绑定的生命周期。 如果使用 IIS 管理器来创建绑定，建议使用 IIS 管理器来管理绑定的生命周期。 如果在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装前在计算机上安装了 IIS，最好在配置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 前审阅 IIS 中的 TLS 配置。  
  
 如果使用报表服务器配置管理器删除了 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的 TLS 绑定，则 TLS 可能对正在运行 Internet Information Services (IIS) 的服务器或其他 HTTP.SYS 服务器上的网站不再有效。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器删除以下注册表项：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443  如果此注册表项被删除，IIS 的 TLS 绑定也会随之删除。 如果没有此绑定，将不为 HTTPS 协议提供 TLS。 若要诊断此问题，请使用 IIS 管理器或 HTTPCFG.exe 命令行实用工具。 若要解决此问题，请使用 IIS 管理器还原网站的 TLS 绑定。 为了防止将来出现此问题，请先使用 IIS 管理器来删除 TLS 绑定，再使用 IIS 管理器来还原相应网站的绑定。 有关详细信息，请参阅知识库文章 [删除 SSL 绑定后 SSL 将不再工作 (https://support.microsoft.com/kb/956209/n)](https://web.archive.org/web/20150215042139/http://support.microsoft.com:80/kb/956209)。  
  
## <a name="see-also"></a>另请参阅  
 [针对报表服务器的身份验证](../../reporting-services/security/authentication-with-the-report-server.md)   
 [配置和管理报表服务器（SSRS 本机模式）](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md)   
 [RsReportServer.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [配置报表服务器 URL（报表服务器配置管理器）](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)  
  
