---
description: Reporting Services 针对验证的扩展保护
title: Reporting Services 针对验证的扩展保护 | Microsoft Docs
ms.date: 06/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
ms.assetid: eb5c6f4a-3ed5-430b-a712-d5ed4b6b9b2b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 72e29a5be172bdf00f9c57358cbbaa1e9bae5a09
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594817"
---
# <a name="extended-protection-for-authentication-with-reporting-services"></a>Reporting Services 针对验证的扩展保护

  扩展保护是针对最新版本的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 操作系统的一组增强功能。 扩展保护增强了应用程序可用来保护凭据和身份验证的方式。 此功能自身并不直接提供保护机制来防止特定攻击（如凭据转发），但它为诸如 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的应用程序提供了一种基础结构来实施针对验证的扩展保护。  
  
 作为扩展保护一部分的主要身份验证增强功能是服务绑定和渠道绑定。 渠道绑定使用渠道绑定标记 (CBT)，以验证在两个端点之间建立的渠道不会受到危害。 服务绑定使用服务器主体名称 (SPN) 来验证身份验证标记的预期目的地。 有关扩展保护的详细背景信息，请参阅 [Integrated Windows Authentication with Extended Protection（将 Windows 身份验证与扩展保护相集成）](/previous-versions/visualstudio/visual-studio-2008/dd639324(v=vs.90))。  
  
SQL Server Reporting Services (SSRS) 支持和实行已在操作系统中启用并在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中配置的扩展保护。 默认情况下， [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 接受指定 Negotiate 或 NTLM 身份验证的请求，因此，可能因操作系统中支持扩展保护以及 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 扩展保护功能而受益。  
  
> [!IMPORTANT]  
>  默认情况下，Windows 不启用扩展保护。 有关如何在 Windows 中启用扩展保护的信息，请参阅 [身份验证的扩展保护](https://go.microsoft.com/fwlink/?LinkID=178431)。 操作系统和客户端身份验证堆栈必须同时支持扩展保护，身份验证才能成功。 对于较早的操作系统，您可能需要安装多个更新，才能获得完整的支持扩展保护功能的计算机。 有关扩展保护的最新开发动态的信息，请参阅 [updated information with Extended Protection（扩展保护的更新信息）](/previous-versions/sql/sql-server-2008/dd146365(v=sql.100))。  

## <a name="reporting-services-extended-protection-overview"></a>Reporting Services 扩展保护概述

SSRS 支持和实行已在操作系统中启用的扩展保护功能。 如果操作系统不支持扩展保护或者尚未启用操作系统中的此功能， [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 扩展保护功能将无法进行身份验证。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 扩展保护还需要 TLS/SSL 证书。 有关详细信息，请参阅[在本机模式报表服务器上配置 TLS 连接](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md)  
  
> [!IMPORTANT]  
>  默认情况下， [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 不启用扩展保护。 可以通过修改 **rsreportserver.config** 配置文件或使用 WMI API 来更新此配置文件，以便启用此功能。 SSRS 不能提供用于修改或查看扩展保护设置的用户界面。 有关详细信息，请参阅本主题中的 [配置设置](#ConfigurationSettings) 部分。  
  
 因更改扩展保护设置或所配置的设置不正确而导致的共同问题并不显示明显的错误消息或对话框窗口。 与扩展保护配置和兼容性相关的问题会导致身份验证失败并在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 跟踪日志中记录错误。  
  
> [!IMPORTANT]
>  某些数据访问技术可能不支持扩展保护。 数据访问技术用于连接到 SQL Server 数据源和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 目录数据库。 未能实现支持扩展保护的数据访问技术将在以下方面影响 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ：  
> 
>  -   运行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 目录数据库的 SQL Server 无法启用扩展保护，或者，报表服务器将无法成功地连接到目录数据库并且将返回身份验证错误。  
> -   所有用作 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表数据源的 SQL Server 都无法启用扩展保护，或者报表服务器尝试连接到报表数据源时将失败并且返回身份验证错误。  
> 
>  用于数据访问技术的文档应该具有与支持扩展保护有关的信息。  
  
### <a name="upgrade"></a>升级  
  
-   将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务器升级到 SQL Server 2016 可将具有默认值的配置设置添加到 rsreportserver.config  文件。 如果设置已经存在，SQL Server 2016 安装将在 rsreportserver.config  文件中保留它们。  
  
-   将配置设置添加到 **rsreportserver.config** 配置文件时，默认行为是针对 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 将扩展保护功能设置为关闭，必须按本主题所述启用此功能。 有关详细信息，请参阅本主题中的 [配置设置](#ConfigurationSettings) 部分。  
  
-   **RSWindowsExtendedProtectionLevel** 设置的默认值为 **Off**。  
  
-   **RSWindowsExtendedProtectionScenario** 设置的默认值为 **Proxy**。  
  
-   升级顾问不验证操作系统或 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的当前安装是否启用了扩展保护支持。  
  
### <a name="what-reporting-services-extended-protection-does-not-cover"></a>Reporting Services 扩展保护不涵盖的内容  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 扩展保护功能不支持以下功能区和方案：  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 自定义安全扩展插件的作者必须将对于扩展保护的支持添加到其自定义安全扩展插件。  
  
-   添加到 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装或它使用的第三方组件必须由第三方供应商进行更新以支持扩展保护功能。 有关详细信息，请与第三方供应商联系。  
  
## <a name="deployment-scenarios-and-recommendations"></a>部署方案和建议  
 以下各方案说明各种不同部署和拓扑，以及使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 扩展保护来对它们进行保护的建议配置。  
  
### <a name="direct"></a>直接  
 此方案介绍直接连接到报表服务器（例如 Intranet 环境）。  
  
|场景|方案图|保护方式|  
|--------------|----------------------|-------------------|  
|直接 TLS 通信。<br /><br /> 报表服务器将实行“客户端到报表服务器”渠道绑定。|![RS_ExtendedProtection_DirectSSL](../../reporting-services/security/media/rs-extendedprotection-directssl.gif "RS_ExtendedProtection_DirectSSL")<br /><br /> 1) 客户端应用程序<br /><br /> 2) 报表服务器|将 **RSWindowsExtendedProtectionLevel** 设置为 **Allow** 或 **Require**。<br /><br /> 将 **RSWindowsExtendedProtectionScenario** 设置为 **Direct**。<br /><br /> <br /><br /> \- 服务绑定不是必需的，因为 TLS 通道将用于通道绑定。|  
|直接 HTTP 通信。 报表服务器将实行“客户端到报表服务器”服务绑定。|![RS_ExtendedProtection_Direct](../../reporting-services/security/media/rs-extendedprotection-direct.gif "RS_ExtendedProtection_Direct")<br /><br /> 1) 客户端应用程序<br /><br /> 2) 报表服务器|将 **RSWindowsExtendedProtectionLevel** 设置为 **Allow** 或 **Require**。<br /><br /> 将 **RSWindowsExtendedProtectionScenario** 设置为 **Any**。<br /><br /> <br /><br /> \- 没有 TLS 通道，因此无法强制执行通道绑定。<br /><br /> \- 可以使服务绑定生效，但是，由于没有渠道绑定，它将成为不完善的防御机制，服务绑定本身只能防止基本威胁。|  
  
### <a name="proxy-and-network-load-balancing"></a>代理和网络负载平衡  
 客户端应用程序连接到设备或软件，设备或软件执行 TLS，并向服务器传递凭据，以进行身份验证（例如，Extranet、Internet 或安全 Intranet）。 客户端连接到代理，或者所有客户端都使用代理。  
  
 当您使用网络负载平衡 (NLB) 设备时，情形是相同的。  
  
|场景|方案图|保护方式|  
|--------------|----------------------|-------------------|  
|HTTP 通信。 报表服务器将实行“客户端到报表服务器”服务绑定。|![RS_ExtendedProtection_Indirect](../../reporting-services/security/media/rs-extendedprotection-indirect.gif "RS_ExtendedProtection_Indirect")<br /><br /> 1) 客户端应用程序<br /><br /> 2) 报表服务器<br /><br /> 3) 代理|将 **RSWindowsExtendedProtectionLevel** 设置为 **Allow** 或 **Require**。<br /><br /> 将 **RSWindowsExtendedProtectionScenario** 设置为 **Any**。<br /><br /> <br /><br /> \- 没有 TLS 通道，因此无法强制执行通道绑定。<br /><br /> \- 必须对报表服务器进行配置，使之识别代理服务器的名称，以确保正确实行服务绑定。|  
|HTTP 通信。<br /><br /> 报表服务器将实行“客户端到代理”渠道绑定和“客户端到报表服务器”服务绑定。|![RS_ExtendedProtection_Indirect_SSL](../../reporting-services/security/media/rs-extendedprotection-indirect-ssl.gif "RS_ExtendedProtection_Indirect_SSL")<br /><br /> 1) 客户端应用程序<br /><br /> 2) 报表服务器<br /><br /> 3) 代理|设置 <br />                    **RSWindowsExtendedProtectionLevel** 设置为 **Allow** 或 **Require**。<br /><br /> 将 **RSWindowsExtendedProtectionScenario** 设置为 **Proxy**。<br /><br /> <br /><br /> \- 可以使用到代理的 TLS 通道，因此能够强制执行到代理的通道绑定。<br /><br /> \- 还可以实行服务绑定。<br /><br /> \- 报表服务器必须了解代理名称，报表服务器管理员应使用主机标头为它创建一个 URL 预留，或在 Windows 注册表项 **BackConnectionHostNames** 中配置代理名称。|  
|与安全代理之间的间接 HTTPS 通信。 报表服务器将实行“客户端到代理”渠道绑定和“客户端到报表服务器”服务绑定。|![RS_ExtendedProtection_IndirectSSLandHTTPS](../../reporting-services/security/media/rs-extendedprotection-indirectsslandhttps.gif "RS_ExtendedProtection_IndirectSSLandHTTPS")<br /><br /> 1) 客户端应用程序<br /><br /> 2) 报表服务器<br /><br /> 3) 代理|设置 <br />                    **RSWindowsExtendedProtectionLevel** 设置为 **Allow** 或 **Require**。<br /><br /> 将 **RSWindowsExtendedProtectionScenario** 设置为 **Proxy**。<br /><br /> <br /><br /> \- 可以使用到代理的 TLS 通道，因此能够强制执行到代理的通道绑定。<br /><br /> \- 还可以实行服务绑定。<br /><br /> \- 报表服务器必须了解代理名称，报表服务器管理员应使用主机标头为它创建一个 URL 预留，或在 Windows 注册表项 **BackConnectionHostNames** 中配置代理名称。|  
  
### <a name="gateway"></a>网关  
 此方案介绍了连接到设备或软件的客户端应用程序，设备或软件执行 TLS 并对用户进行身份验证。 接着，此设备或软件模拟用户上下文或其他用户上下文，之后对报表服务器发出请求。  
  
|场景|方案图|保护方式|  
|--------------|----------------------|-------------------|  
|间接 HTTP 通信。<br /><br /> 网关将实行“客户端到网关”渠道绑定。 此时具有“网关到报表服务器”服务绑定。|![RS_ExtendedProtection_Indirect_SSL](../../reporting-services/security/media/rs-extendedprotection-indirect-ssl.gif "RS_ExtendedProtection_Indirect_SSL")<br /><br /> 1) 客户端应用程序<br /><br /> 2) 报表服务器<br /><br /> 3) 网关设备|将 **RSWindowsExtendedProtectionLevel** 设置为 **Allow** 或 **Require**。<br /><br /> 将 **RSWindowsExtendedProtectionScenario** 设置为 **Any**。<br /><br /> <br /><br /> \- 从客户端到报表服务器的渠道绑定是不可能的，因为网关模拟上下文，并因此创建一个新的 NTLM 标记。<br /><br /> \- 从网关到报表服务器没有 TLS，因此无法强制执行通道绑定。<br /><br /> \- 可以实行服务绑定。<br /><br /> \- 应由管理员配置网关设备以实行渠道绑定。|  
|与安全网关之间的间接 HTTPS 通信。 网关将实行“客户端到网关”渠道绑定，报表服务器将实行“网关到报表服务器”渠道绑定。|![RS_ExtendedProtection_IndirectSSLandHTTPS](../../reporting-services/security/media/rs-extendedprotection-indirectsslandhttps.gif "RS_ExtendedProtection_IndirectSSLandHTTPS")<br /><br /> 1) 客户端应用程序<br /><br /> 2) 报表服务器<br /><br /> 3) 网关设备|将 **RSWindowsExtendedProtectionLevel** 设置为 **Allow** 或 **Require**。<br /><br /> 将 **RSWindowsExtendedProtectionScenario** 设置为 **Direct**。<br /><br /> <br /><br /> \- 从客户端到报表服务器的渠道绑定是不可能的，因为网关模拟上下文，并因此创建一个新的 NTLM 标记。<br /><br /> \- 从网关到报表服务器的 TLS 意味着可以强制执行通道绑定。<br /><br /> \- 不需要服务绑定。<br /><br /> \- 应由管理员配置网关设备以实行渠道绑定。|  
  
### <a name="combination"></a>组合  
 此方案介绍客户端连接到代理的 Extranet 或 Internet 环境。 这种环境与客户端在其中连接到报表服务器的 Intranet 环境相结合。  
  
|场景|方案图|保护方式|  
|--------------|----------------------|-------------------|  
|从客户端间接和直接访问报表服务器服务，其中客户端没有与代理或报表服务器建立 TLS 连接。|1) 客户端应用程序<br /><br /> 2) 报表服务器<br /><br /> 3) 代理<br /><br /> 4) 客户端应用程序|将 **RSWindowsExtendedProtectionLevel** 设置为 **Allow** 或 **Require**。<br /><br /> 将 **RSWindowsExtendedProtectionScenario** 设置为 **Any**。<br /><br /> <br /><br /> \- 可以实行从客户端到报表服务器的服务绑定。<br /><br /> \- 报表服务器必须了解代理名称，报表服务器管理员应使用主机标头为它创建一个 URL 预留，或在 Windows 注册表项 **BackConnectionHostNames** 中配置代理名称。|  
|从客户端间接和直接访问报表服务器，其中客户端与代理或报表服务器建立 TLS 连接。|![RS_ExtendedProtection_CombinationSSL](../../reporting-services/security/media/rs-extendedprotection-combinationssl.gif "RS_ExtendedProtection_CombinationSSL")<br /><br /> 1) 客户端应用程序<br /><br /> 2) 报表服务器<br /><br /> 3) 代理<br /><br /> 4) 客户端应用程序|将 **RSWindowsExtendedProtectionLevel** 设置为 **Allow** 或 **Require**。<br /><br /> 将 **RSWindowsExtendedProtectionScenario** 设置为 **Proxy**。<br /><br /> <br /><br /> \- 可以使用渠道绑定<br /><br /> \- 报表服务器必须了解代理名称，报表服务器管理员应使用主机标头为代理创建一个 URL 预留，或在 Windows 注册表项 **BackConnectionHostNames** 中配置代理名称。|  
  
## <a name="configuring-reporting-services-extended-protection"></a>配置 Reporting Services 扩展保护  
 **rsreportserver.config** 文件包含的配置值用于控制 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 扩展保护的行为。  
  
 有关使用和编辑 **rsreportserver.config** 文件的详细信息，请参阅 [RsReportServer.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)。 还可以使用 WMI API 更改和检查扩展保护设置。 有关详细信息，请参阅 [SetExtendedProtectionSettings 方法 (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setextendedprotectionsettings.md)。  
  
 当对配置设置进行验证失败时，将在报表服务器上禁用身份验证类型 **RSWindowsNTLM**、 **RSWindowsKerberos** 和 **RSWindowsNegotiate** 。  
  
###  <a name="configuration-settings-for-reporting-services-extended-protection"></a><a name="ConfigurationSettings"></a> Reporting Services 扩展保护的配置设置  
 下表提供有关在 **rsreportserver.config** 中显示的扩展保护配置设置的信息。  
  
|设置|说明|  
|-------------|-----------------|  
|**RSWindowsExtendedProtectionLevel**|指定扩展保护的实行程度。 有效值是：<br /><br /> **OFF**：默认。 指定没有渠道绑定或服务绑定验证。<br /><br /> **Allow** 支持扩展保护，但并不需要此功能。  指定：<br /><br /> \- 将为在支持扩展保护的操作系统上运行的客户端应用程序实行扩展保护。 实行保护的方式通过设置 **RsWindowsExtendedProtectionScenario** 来确定<br /><br /> \- 对于在不支持扩展保护的操作系统上运行的应用程序而言，将允许进行身份验证。<br /><br /> **Require** 指定：<br /><br /> \- 将为在支持扩展保护的操作系统上运行的客户端应用程序实行扩展保护。<br /><br /> \- 对于在不支持扩展保护的操作系统上运行的应用程序而言，将 **不** 允许进行身份验证。|  
|**RsWindowsExtendedProtectionScenario**|指定要验证哪些形式的扩展保护：渠道绑定、服务绑定，或两者。 有效值是：<br /><br /> **代理**：默认。 指定：<br /><br /> \- 当存在渠道绑定标记时，使用 Windows NTLM、Kerberos 和 Negotiate 身份验证。<br /><br /> \- 实行服务绑定。<br /><br /> **Any** 指定：<br /><br /> \- 不要求 Windows NTLM、Kerberos 和 Negotiate 身份验证以及渠道绑定。<br /><br /> \- 实行服务绑定。<br /><br /> **Direct** 指定：<br /><br /> \- 如果 CBT 存在、与当前服务建立了 TLS 连接，且用于 TLS 连接的 CBT 与 NTLM、Kerberos 或协商令牌的 CBT 一致，应使用 Windows NTLM、Kerberos 和协商身份验证。<br /><br /> \- 不实行服务绑定。<br /><br /> <br /><br /> 注意：如果 RsWindowsExtendedProtectionLevel 设置为 OFF，则忽略 RsWindowsExtendedProtectionScenario 设置    。|  
  
 **rsreportserver.config** 配置文件中的示例条目：  
  
```  
<Authentication>  
         <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>  
         <RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionLevel>  
</Authentication>  
```  
  
## <a name="service-binding-and-included-spns"></a>服务绑定和所包含的 SPN  
 服务绑定使用服务器主体名称或 SPN 来验证身份验证标记的预期目标。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 使用现有的 URL 预留信息来生成视为有效的 SPN 的列表。 使用 URL 预留信息来验证 SPN 和 URL 预留使系统管理员能够从单个位置管理这两者。  
  
 当报表服务器启动、扩展保护的配置设置更改或回收应用程序域时，将更新有效 SPN 的列表。  
  
 SPN 的有效列表特定于每个应用程序。 例如，报表管理器和报表服务器每个都将计算不同的有效 SPN 列表。  
  
 为应用程序计算的有效 SPN 列表由以下元素确定：  
  
-   每个 URL 预留。  
  
-   从域控制器为 Reporting Services 服务帐户检索的每个 SPN。  
  
-   如果 URL 预留包含通配符（“*”或“+”），则报表服务器将从 Hosts 集合添加每个条目。  
  
### <a name="hosts-collection-sources"></a>Hosts 集合来源。  
 下表列出了 Hosts 集合的潜在来源。  
  
|源类型|说明|  
|--------------------|-----------------|  
|ComputerNameDnsDomain|分配给本地计算机的 DNS 域的名称。 如果本地计算机为群集中的节点，则将使用群集虚拟服务器的 DNS 域名。|  
|ComputerNameDnsFullyQualified|唯一标识本地计算机的完全限定 DNS 名称。 此名称是 DNS 主机名和 DNS 域名的组合，格式为 *HostName*.*DomainName*。 如果本地计算机为群集中的节点，则将使用群集虚拟服务器的完全限定 DNS 名称。|  
|ComputerNameDnsHostname|本地计算机的 DNS 主机名。 如果本地计算机为群集中的节点，则将使用群集虚拟服务器的 DNS 主机名。|  
|ComputerNameNetBIOS|本地计算机的 NetBIOS 名称。 如果本地计算机为群集中的节点，则将使用群集虚拟服务器的 NetBIOS 主机名。|  
|ComputerNamePhysicalDnsDomain|分配给本地计算机的 DNS 域的名称。 如果本地计算机为群集中的节点，则将使用本地计算机的 DNS 域名，而不使用群集虚拟服务器的名称。|  
|ComputerNamePhysicalDnsFullyQualified|唯一标识计算机的完全限定 DNS 名称。 如果本地计算机为群集中的节点，则将使用本地计算机的完全限定 DNS 名称，而不使用群集虚拟服务器的名称。<br /><br /> 此完全限定的 DNS 名称是 DNS 主机名和 DNS 域名的组合，格式为 *HostName*.*DomainName*。|  
|ComputerNamePhysicalDnsHostname|本地计算机的 DNS 主机名。 如果本地计算机为群集中的节点，则将使用本地计算机的 DNS 主机名，而不使用群集虚拟服务器的名称。|  
|ComputerNamePhysicalNetBIOS|本地计算机的 NetBIOS 名称。 如果本地计算机为群集中的节点，则将使用本地计算机的 NetBIOS 主机名，而不使用群集虚拟服务器的名称。|  
  
有关详细信息，请参阅[为报表服务器注册服务主体名称 (SPN)](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md) 和[关于 URL 预留和注册（报表服务器配置管理器）](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md)。  
  
## <a name="next-steps"></a>后续步骤

[使用扩展保护连接到数据库引擎](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)   
[针对验证的扩展保护概述](/previous-versions/dotnet/netframework-3.5/dd767318(v=vs.90))   
[Integrated Windows Authentication with Extended Protection（将 Windows 身份验证与扩展保护相集成）](/previous-versions/visualstudio/visual-studio-2008/dd639324(v=vs.90))   
[Microsoft 安全公告：身份验证的扩展保护](https://go.microsoft.com/fwlink/?LinkId=179923)   
[报表服务器服务跟踪日志](../../reporting-services/report-server/report-server-service-trace-log.md)   
[RsReportServer.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[SetExtendedProtectionSettings 方法 (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setextendedprotectionsettings.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)