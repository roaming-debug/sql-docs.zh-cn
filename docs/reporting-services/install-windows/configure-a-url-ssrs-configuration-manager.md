---
description: 配置 URL（报表服务器配置管理器）
title: 配置 URL (Configuration Manager) | Microsoft Docs
ms.date: 05/26/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- URL access [Reporting Services], syntax
ms.assetid: 851e163a-ad2a-491e-bc1e-4df92327092f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bd4d8deaa9be114ffb2ce806f67be3b96babb8b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015277"
---
# <a name="configure-a-url--report-server-configuration-manager"></a>配置 URL（报表服务器配置管理器）
  必须为每个应用程序配置至少一个 URL 才能使用 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 或报表服务器 Web 服务。 如果 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 是在“仅文件”模式下安装的（即通过在安装向导的“报表服务器安装选项”页上选择“安装但不配置服务器”选项），则必须配置 URL  。 如果 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 是采用默认配置安装的，则已经为每个应用程序配置了 URL。  
  
 可以使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具来配置 URL。 URL 的所有部分均在此工具中定义。 与早期版本不同，Internet Information Services (IIS) 网站不再提供对 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 和更高版本中的 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 应用程序的访问。  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 提供了在多数部署方案下均能发挥良好作用的默认值，这些部署方案包括与其他 Web 服务和应用程序的并行部署。 默认 URL 包含实例名称，如果您在同一台计算机上运行多个报表服务器实例，则这可以最大限度地降低 URL 冲突的风险。  
  
 本主题提供有关以下任务的说明：  
  
-   为报表服务器 Web 服务创建 URL。  
  
-   为 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]创建 URL。  
  
-   设置高级 URL 属性以定义其他 URL。  
  
 要详细了解如何存储和维护 URL 或详细了解互操作性问题，请参阅[关于 URL 预留和注册（报表服务器配置管理器）](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md)和[并行安装 Reporting Services 和 Internet Information Services（SSRS 本机模式）](../../reporting-services/install-windows/install-reporting-and-internet-information-services-side-by-side.md)。 若要查看 Reporting Services 安装中经常使用的 URL 示例，请参阅本主题中的 [URL 示例](#URLExamples) 。  
  
## <a name="prerequisites"></a>先决条件  
 在创建或修改 URL 之前，请注意以下几点：  
  
-   您必须是报表服务器计算机上本地 Administrators 组的成员。  
  
-   如果在同一台计算机上安装了 IIS，请检查使用端口 80 的任何网站上的虚拟目录的名称。 如果发现任何虚拟目录使用默认 Reporting Services 虚拟目录名称（即“Reports”和“ReportServer”），请为您要配置的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] URL 选择不同的虚拟目录名称。  
  
-   必须使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具来配置 URL， 而不要使用系统实用工具。 切勿直接在 RSReportServer.config 文件的 **URLReservations** 部分中修改 URL 预留。 若要同时更新内部存储的基础 URL 预留并同步存储在 RSReportServer.config 文件中的 URL 设置，则有必要使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具。  
  
-   选择一个报表活动较少的时间。 每次 URL 预留发生更改时，都可能会回收报表服务器 Web 服务和 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 的应用程序域。  
  
-   有关 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中 URL 构造和用法的概述，请参阅[配置报表服务器 URL（报表服务器配置管理器）](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)。  
  
### <a name="to-configure-a-url-for-the-report-server-web-service"></a>为报表服务器 Web 服务配置 URL  
  
1.  启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具，然后连接到某个本地报表服务器实例。  
  
2.  单击 **“Web 服务 URL”** 。  
  
3.  指定虚拟目录。 虚拟目录名称用于标识接收请求的应用程序。 由于 IP 地址和端口可由多个应用程序共享，因而使用虚拟目录名称来指定接收请求的应用程序。  
  
     此值必须唯一，以确保请求到达预期目标。 此值是必需的。 且区分大小写。 虚拟目录名称和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 应用程序实例之间是一一对应的。 如果创建多个指向同一应用程序实例的 URL，则必须在为此应用程序实例定义的所有 URL 中使用相同的虚拟目录名称。  
  
     对于报表服务器 Web 服务，默认虚拟目录名称为 **ReportServer**。  
  
4.  指定用于在网络中唯一地标识该报表服务器计算机的 IP 地址。 如果希望指定主机标头或者为同一应用程序实例定义其他 URL，则必须单击 **“高级”** 。 有关如何设置 URL 的高级属性的说明，请参阅本主题后面的说明。 否则，请使用 **“Web 服务 URL”** 页从下列值中进行选择：  
  
    -   **“所有已分配的”** 指定分配给计算机的任何 IP 地址均可用在指向报表服务器应用程序的 URL 中。 此值还包含友好主机名（如计算机名），域名服务器可将该主机名解析为分配给该计算机的 IP 地址。 此为 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] URL 的默认值。  
  
    -   **“所有未分配的”** 指定报表服务器将接收任何尚未由其他应用程序处理的请求。 建议避免使用此选项。 如果选择此选项，则具有更强 URL 预留的另一个应用程序可能会截获发往该报表服务器的请求。  
  
    -   **127.0.0.1** 是用于访问 localhost 的 IPv4 地址。 它支持对报表服务器计算机进行本地管理。 如果仅选择此值，则只有在本地登录到报表服务器计算机的用户可以访问应用程序。  
  
    -   **::1** 是 IPv6 格式的环回地址。  
  
    -   此列表中还显示特定 IP 地址。 IP 地址可以采用 IPv4 和 IPv6 格式。 *Nnn.nnn.nnn.nnn* 是计算机网络适配器的 32 位 IPv4 地址。 IPv6 地址为 128 位，包含八个由冒号分隔的 4 字节字段：\<prefix>:nnnn:nnnn:nnnn:nnnn:nnnn:nnnn  
  
         如果有多个网络适配器，或者如果网络同时支持 IPv4 和 IPv6 地址，则会看到多个 IP 地址。 如果只选择一个 IP 地址，则会将应用程序限制为只能访问该 IP 地址（以及域名服务器映射到该地址的任何主机名）。 您不能使用 localhost 访问报表服务器，也不能使用安装在报表服务器计算机上的其他网络适配器的 IP 地址。 如果选择此值，则通常是因为你要配置多个还指定显式 IP 地址或主机名的 URL 预留（例如，一个针对用于 Intranet 连接的网络适配器，另一个用于 Extranet 连接）。  
  
5.  指定端口。 端口 80 是默认端口，因为它可以与其他应用程序共享。 如果希望使用自定义端口号，请记住必须始终在用于访问报表服务器的 URL 中指定它。 可以使用以下方法来查找可用端口：  
  
    -   在命令提示符下键入以下命令以返回正在使用的 TCP 端口的列表：  
  
         `netstat -anp tcp`  
  
    -   查阅 Microsoft 支持文章 [关于 TCP/IP 端口分配的信息](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/174904)，了解 TCP 端口分配以及已知端口（0 到 1023）、注册端口（1024 到 49151）和动态或专用端口（49152 到 65535）之间的区别。  
  
    -   如果您正在使用 Windows 防火墙，则必须打开该端口。 有关说明，请参阅 [Configure a Firewall for Report Server Access](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)。  
  
6.  如果尚未进行验证，请验证 IIS（如果已安装）的虚拟目录名称是否与您要使用的名称不同。  
  
7.  如果安装了 TLS/SSL 证书，现在可以选择它来将 URL 绑定到计算机上安装的 TLS/SSL 证书。  
  
8.  （可选）如果选择了 TLS/SSL 证书，可以指定自定义端口。 默认端口为 443，但您可以使用任何可用端口。  
  
9. 单击 **“应用”** 创建 URL。  
  
10. 通过单击页面 **URL** 部分中的链接来测试该 URL。 请注意，必须先创建并配置报表服务器数据库，然后才能测试 URL。 有关指导，请参阅[创建本机模式报表服务器数据库（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md)。  

> [!NOTE]
>  如果你已有 TLS 绑定和 URL 预留，且要更改 TLS 绑定（例如使用不同的证书或主机头），建议按顺序完成以下步骤：  
> 
>  1.  首先删除所有 URL 预留。  
> 2.  然后，删除所有 TLS 绑定。  
> 3.  接下来，重新创建 URL 和 TLS 绑定。  
> 
>  可以使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器完成上述步骤。  
> 
>  Microsoft Windows 对于每个 IP 地址到端口的组合支持一个绑定。 如果您配置报表服务器以使用特定主机标头值并且还将端口到 IP 地址组合上的证书签发给不同的主机标头值，在浏览器中将看到一个警告，指示证书与使用的 URL 不匹配。  
> 
>  为了解决此问题，请删除所有绑定，然后使用唯一设置创建新的绑定，或使用通配符配置 Reporting Services URL 注册。
  
### <a name="to-create-a-url-reservation-for-the-ssrswebportal"></a>为 Web 门户创建 URL 预留 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]  
  
1.  启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具并连接到报表服务器实例。  
  
2.  单击“Web 门户 URL”  。  
  
3.  指定虚拟目录。 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 与报表服务器 Web 服务侦听相同的 IP 地址和端口。 如果将 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 配置为指向不同的报表服务器 Web 服务，则必须在 RSReportServer.config 文件中修改 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] URL 设置。  
  
4.  如果安装了 TLS/SSL 证书，可以选择它来要求所有向 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 发出的请求都通过 HTTPS 进行路由。  
  
     （可选）如果选择了 TLS/SSL 证书，可以指定自定义端口。 默认端口为 443，但您可以使用任何可用端口。  
  
5.  单击 **“应用”** 创建 URL。  
  
6.  通过单击页面 **URL** 部分中的链接来测试该 URL。  
  
## <a name="setting-advanced-properties-to-specify-additional-urls"></a>设置高级属性以指定其他 URL  
 你可以通过指定不同的端口或主机名（域名服务器可以解析为分配给计算机的 IP 地址的 IP 地址或主机标头名称）为报表服务器 Web 服务或 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 保留多个 URL。 通过创建多个 URL，可以为同一个报表服务器实例设置不同的访问路径。 例如，若要启用报表服务器的 Intranet 和 Extranet 访问，可以为 Intranet 访问使用默认 URL，并为 Extranet 访问使用另一个完全限定的主机名：  
  
-   `https://myserver01/reportserver`  
  
-   `https://www.adventure-works.com/reportserver`  
  
 不能为同一个应用程序实例设置多个虚拟目录名称。 每个 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 应用程序实例只能映射到一个虚拟目录名称。 如果同一台计算机上有多个 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 实例，则应用程序的虚拟目录名称应包含实例名称以确保每个请求都能到达其预期目标。  
 
  **主机标头**  
 如果已有一个在域名服务器上定义的主机标头解析为您的计算机，则可以在为报表服务器访问配置的 URL 中指定该主机标头。  
  
 主机标头是允许多个网站共享一个 IP 地址和端口的唯一名称。 主机标头名称比 IP 地址和端口号更容易记住和键入。 这里有一个主机标头名称的示例： www.adventure-works.com 。  
  
 **SSL 端口**  
 为 TLS/SSL 连接指定端口。 TLS 的默认端口为 443。  
  
 **SSL 证书**  
 指定在此计算机上安装的 TLS/SSL 证书的证书名称。 如果证书映射到某个通配符，则可以将其用于报表服务器连接。  
  
 指定为其注册证书的完全限定的计算机名称。 指定的名称必须与为其注册证书的名称相同。  
  
 必须先安装证书，才能使用此选项。 还必须修改 RSReportServer.config 文件中的 UrlRoot 配置设置，以便指定要为其注册证书的计算机的完全限定名称。 有关详细信息，请参阅[在本机模式报表服务器上配置 TLS 连接](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md)。  
  
### <a name="to-set-advanced-properties-on-a-url"></a>设置 URL 的高级属性  
  
1.  在“Web 服务 URL”或“Web 门户 URL”页上，单击“高级”    。  
  
2.  单击“添加”  。  
  
3.  单击“IP 地址”或“主机标头名称”。 如果指定主机标头，请确保指定一个 DNS 服务可以解析的名称。 如果要指定公开可用的域名，请包含整个 URL，包括 `https://www`。  
  
4.  指定端口。 如果指定自定义端口，则应用程序的 URL 必须始终包含该端口号。  
  
5.  单击“确定”。   
  
6.  通过打开浏览器窗口并输入 URL 来测试 URL。  
  
## <a name="urls-for-multiple-report-server-instances-on-the-same-computer"></a>同一台计算机上多个报表服务器实例的 URL  
 如果为多个 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]实例保留 URL，则应遵循下列命名约定以避免命名冲突。 有关详细信息，请参阅[多实例报表服务器部署的 URL 预留（报表服务器配置管理器）](../../reporting-services/install-windows/url-reservations-for-multi-instance-report-server-deployments.md)。  
  
##  <a name="examples-of-url-configurations"></a><a name="URLExamples"></a> URL 配置示例  
 下面列出了报表服务器 URL 的某些可能样式示例：  
  
-   `https://localhost/reportserver`  
  
-   `https://localhost/reportserver_SQLEXPRESS`  
  
-   `https://sales01/reportserver`  
  
-   `https://sales01:8080/reportserver`  
  
-   `https://sales.adventure-works.com/reportserver`  
  
-   `https://www.adventure-works.com:8080/reportserver01`  
  
 用来访问 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 的 URL 也具有类似的格式，并通常在托管报表服务器的同一网站下创建。 唯一的不同之处是虚拟目录名称（在此例中为 **reports** ，但你可以将其配置为需要的任意名称）：  
  
-   `https://localhost/reports`  
  
-   `https://localhost/reports_SQLEXPRESS`  
  
-   `https://sales01/reports`  
  
-   `https://sales01:8080/reports`  
  
-   `https://sales.adventure-works.com/reports`  
  
-   `https://www.adventure-works.com:8080/reports`  
  
## <a name="see-also"></a>另请参阅

 [配置报表服务器 URL（报表服务器配置管理器）](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md) [报表服务器配置管理器（本机模式）](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
