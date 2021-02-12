---
title: 配置 Reporting Services 以使用使用者可选名称 (SAN) | Microsoft Docs
description: 了解如何通过修改 rsreportserver.config 文件并使用 Netsh.exe 工具来配置 SQL Server Reporting Services 和 Power BI 报表服务器以使用 SAN。
ms.date: 09/27/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 25e5fce47d86ea4d544b43f297fcb793d8167a66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065152"
---
# <a name="configure-reporting-services-to-use-a-subject-alternative-name-san"></a>配置 Reporting Services 以使用使用者可选名称 (SAN)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

本主题说明了如何通过修改 rsreportserver.config 文件和使用 Netsh.exe 工具配置 Reporting Services (SSRS) 和 Power BI 报表服务器以使用使用者可选名称 (SAN)。

这些说明适用于在报表服务器配置管理器工具中的 Web 服务 URL 和 Web 门户 URL。

若要使用 SAN，TLS/SSL 证书必须注册在服务器上、进行签名并且有私钥。 不能使用自签名证书。

可以将 Reporting Services 和 Power BI 报表服务器中的 URL 配置为使用 TLS/SSL 证书。 证书通常只包含使用者名称，它只允许对传输层安全性 (TLS)（旧称为“安全套接字层 (SSL)”）会话使用一个 URL。 SAN 是证书中的附加字段，可便于 TLS 服务侦听许多 URL，并与其他应用程序共享 TLS 端口。 例如，SAN 应类似于 `www.myreports.com`。

若要详细了解 Reporting Services 的 TLS 设置，请参阅[在本机模式报表服务器上配置 TLS 连接](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md)。  
  
## <a name="configure-to-use-a-subject-alternative-name-for-web-service-url"></a>配置为使用适用于 Web 服务 URL 的使用者可选名称
  
1.  启动报表服务器配置管理器。  
  
     有关详细信息，请参阅[报表服务器配置管理器（本机模式）](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)。  
  
2.  在“Web 服务 URL”  页上，选择 TLS/SSL 端口和 TLS/SSL 证书。  
  
     ![报表服务器配置管理器](../../reporting-services/report-server-sharepoint/media/reportingservices-configurationmanager.png "报表服务器配置管理器")  
  
     配置管理器为端口注册 TLS/SSL 证书。  
  
3.  打开 rsreportserver.config 文件。  
  
     对于 SSRS 2016 本机模式，默认情况下，该文件位于以下文件夹：  
  
    ```  
    \Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer  
    ```  
  
     对于 SSRS 2017 及更高版本，默认情况下，该文件位于以下文件夹：  
  
    ```  
    \Program Files\Microsoft SQL Server Reporting Services\SSRS\ReportServer  
    ```  
    
     对于 Power BI 报表服务器，默认情况下，该文件位于以下文件夹：  
  
    ```  
    \Program Files\Microsoft Power BI Report Server\PBIRS\ReportServer  
    ```  
  
4.  复制 ReportServerWebService 应用程序的 URL 部分。
  
     例如，以下初始 URL 部分为：  
  
    ```  
        <URL>  
         <UrlString>https://+:443</UrlString>  
         <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>  
         <AccountName>NT Service\ReportServer</AccountName>  
        </URL>  
  
    ```  
  
     以下修改后的 URL 部分为：
  
    ```  
    <URL>  
         <UrlString>https://+:443</UrlString>  
         <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>  
         <AccountName>NT Service\ReportServer</AccountName>  
        </URL>  
        <URL>  
         <UrlString>https://www.myreports.com:443</UrlString>  
         <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051/AccountSid>  
         <AccountName>NT Service\ReportServer</AccountName>  
        </URL>  
  
    ```  
  
  > [!TIP]  
>  * 对于 SSRS 2017 及更高版本，`AccountSid` 值为 `S-1-5-80-4050220999-2730734961-1537482082-519850261-379003301`，`AccountName` 值为 `NT SERVICE\SQLServerReportingServices`。
>  * 对于 Power BI 报表服务器，`AccountSid` 值为 `S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663`，`AccountName` 值为 `NT SERVICE\PowerBIReportServer`。
  
5.  保存 rsreportserver.config 文件。  
  
6.  使用“以管理员身份运行”启动一个命令提示符并运行 Netsh.exe 工具。  
  
    ```  
    C:\windows\system32\netsh  
    ```  
  
7.  键入以下内容切换至 http 上下文。  
  
    ```  
    Netsh>http  
    ```  
  
8.  键入以下内容显示现有 urlacl：
  
    ```  
    Netsh http>show urlacl  
    ```  
  
     出现如下条目。  
  
    ```  
    Reserved URL            : https://+:443/ReportServer/  
        User: NT SERVICE\ReportServer  
            Listen: Yes  
            Delegate: No  
            SDDL: D:(A;;GX;;;S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051)  
    ```  
  
     一个 urlacl 是一个保留的 URL 的 DACL（自由访问控制列表）。  
  
9. 通过键入以下内容，为使用者可选名称创建一个用户和 SDDL 与现有条目相同的新条目：  
  
    ```  
    netsh http>add urlacl  url=https://www.myreports.com:443/ReportServer    
    user="NT Service\ReportServer" sddl=D:(A;;GX;;;S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051)  
  
    ```  
  
10. 对于“Web 门户 URL”，键入以下内容，为使用者可选名称创建一个新条目：

    ```  
    netsh http>add urlacl  url=https://www.myreports.com:443/Reports  
    user="NT Service\ReportServer" sddl=D:(A;;GX;;;S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051)  
  
    ```  
> [!TIP]  
>  * 对于 SSRS 2017 及更高版本，`user` 值为 `NT SERVICE\SQLServerReportingServices`，`sddl` 值为 `D:(A;;GX;;;S-1-5-80-4050220999-2730734961-1537482082-519850261-379003301)`。
>  * 对于 Power BI 报表服务器，`user` 值为 `NT SERVICE\PowerBIReportServer`，`sddl` 值为 `S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663`

> [!NOTE]  
> 对于 Power BI 报表服务器，需要为使用者可选名称创建两个附加条目，方法是键入以下内容：
>  * `add urlacl url=https://www.myreports.com:443/PowerBI user="NT SERVICE\PowerBIReportServer" sddl=D:(A;;GX;;;S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663)`
>  * `add urlacl url=https://www.myreports.com:443/wopi user="NT SERVICE\PowerBIReportServer" sddl=D:(A;;GX;;;S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663)`

11. 在报表服务器配置管理器的“报表服务器状态”页面上单击“停止”，然后单击“启动”以重启报表服务器。  
  
## <a name="see-also"></a>另请参阅

 [RsReportServer.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [报表服务器配置管理器](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [修改 Reporting Services 配置文件](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)   
 [配置报表服务器 URL](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
