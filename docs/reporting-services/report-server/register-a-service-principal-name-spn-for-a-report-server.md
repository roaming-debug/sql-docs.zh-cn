---
title: 为报表服务器注册服务主体名称 (SPN) | Microsoft Docs
description: 了解当网络使用 Kerberos 进行身份验证时，如果 Report Server 服务作为域用户运行应如何创建它的 SPN。
ms.date: 09/24/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: dda91d4f-77cc-4898-ad03-810ece5f8e74
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 38985454ae83a73fd27dac886fd0f4ee10e5ad55
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125579"
---
# <a name="register-a-service-principal-name-spn-for-a-report-server"></a>为报表服务器注册服务主体名称 (SPN)
  如果要在使用 Kerberos 协议进行相互身份验证的网络中部署 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ，并且将报表服务器服务配置为以域用户帐户身份运行，则必须为报表服务器服务创建服务主体名称 (SPN)。  
  
## <a name="about-spns"></a>关于 SPN  
 SPN 是服务在使用 Kerberos 身份验证的网络上的唯一标识符。 它包含一个服务类、一个主机名，有时也包含一个端口。 HTTP SPN 不需要端口。 在使用 Kerberos 身份验证的网络中，必须在内置计算机帐户（如 NetworkService 或 LocalSystem）或用户帐户下为服务器注册 SPN。 对于内置帐户，SPN 将自动进行注册。 但是，如果在域用户帐户下运行服务，则必须为要使用的帐户手动注册 SPN。  
  
 若要创建 SPN，可以使用 **SetSPN** 命令行实用工具。 有关详细信息，请参阅以下主题：  
  
-   [Setspn](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) (https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) 。  
  
-   [服务主体名称 (SPN) SetSPN 语法 (Setspn.exe)](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx) (https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx) 。  
  
 您必须具有域管理员身份，才能在域控制器上运行该实用工具。  
  
## <a name="syntax"></a>语法  

使用 setspn 处理 SPN 时，必须以正确的格式输入 SPN。 HTTP SPN 的格式为 `http/host`。 使用 SetSPN 实用工具为报表服务器创建 SPN 的命令语法类似如下所示：  
  
```  
Setspn -s http/<computer-name>.<domain-name> <domain-user-account>  
```  
  
 **SetSPN** 随 Windows Server 一起提供。 **-s** 参数在验证不存在重复项后添加一个 SPN。 **注意：-s** 从 Windows Server 2008 开始已在 Windows Server 中提供。  
  
 **HTTP** 为服务类。 报表服务器 Web 服务在 HTTP.SYS 中运行。 在为 HTTP 创建 SPN 时，将同时对在 HTTP.SYS（包括承载在 IIS 中的应用程序）中运行的位于同一台计算机上的所有 Web 应用程序授予基于该域用户帐户的票证。 如果这些服务在其他帐户下运行，则身份验证请求将失败。 为避免此问题，请务必将所有 HTTP 应用程序配置为在同一帐户下运行，或考虑为每个应用程序创建主机头，然后为每个主机头单独创建一个 SPN。 配置主机标头时，无论 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置如何都必须更改 DNS。  
  
 为 \<*computername*> 和 \<*domainname*> 指定的值将标识承载 Report Server 的计算机的唯一网络地址。 此地址可以是本地主机名，或者完全限定的域名 (FQDN)。 如果只有一个域，可从命令行中省略 \<*domainname*>。 \<*domain-user-account*> 是 Report Server 服务运行时使用的用户帐户，也是必须注册 SPN 的用户帐户。  
  
## <a name="register-an-spn-for-domain-user-account"></a>为域用户帐户注册 SPN  
  
### <a name="to-register-an-spn-for-a-report-server-service-running-as-a-domain-user"></a>为以域用户身份运行的报表服务器服务注册 SPN  
  
1.  安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 并将报表服务器服务配置为以域用户帐户身份运行。 请注意，直到完成以下步骤之后，用户才能连接到报表服务器。  
  
2.  以域管理员的身份登录到域控制器。  
  
3.  打开命令提示符窗口。  
  
4.  复制以下命令，并用适用于您的网络的实际值替换占位符值。  
  
    ```  
    Setspn -s http/<computer-name>.<domain-name> <domain-user-account>  
    ```  
  
    例如： `Setspn -s http/MyReportServer.MyDomain.com MyDomainUser`  
  
5.  运行命令。  
  
6.  打开 **RsReportServer.config** 文件并找到 `<AuthenticationTypes>` 部分。  
  
7.  添加 `<RSWindowsNegotiate />` 作为该部分的第一个项，以启用 Kerberos。  
  
## <a name="see-also"></a>另请参阅  
 [配置服务帐户（报表服务器配置管理器）](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)   
 [配置报表服务器服务帐户（报表服务器配置管理器）](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)   
 [管理 Reporting Services 本机模式报表服务器](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)  
  
