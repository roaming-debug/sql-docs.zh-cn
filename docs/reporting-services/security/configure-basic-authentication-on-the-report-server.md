---
description: 在报表服务器上配置基本身份验证
title: 在报表服务器上配置基本身份验证 | Microsoft Docs
ms.date: 02/10/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, configuration
- Basic authentication
ms.assetid: 8faf2938-b71b-4e61-a172-46da2209ff55
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d3b875eafb10e4234df2cde35bb61bbecfebad94
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489321"
---
# <a name="configure-basic-authentication-on-the-report-server"></a>在报表服务器上配置基本身份验证
  默认情况下，Reporting Services 接受指定 Negotiate 和 NTLM 身份验证的请求。 如果部署中包括使用基本身份验证的客户端应用程序或浏览器，则必须将基本身份验证添加到支持的类型列表中。 此外，若要使用报表生成器，必须启用对报表生成器文件的匿名访问。  
  
 若要对报表服务器配置基本身份验证，则需要在 RSReportServer.config 文件中编辑 XML 元素和值。 可以复制并粘贴本主题中的示例来替换默认值。  
  
 启用基本身份验证之前，请验证您的安全基础结构是否支持。 在基本身份验证模式下，报表服务器 Web 服务会将凭据传递给本地安全机构。 如果凭据指定本地用户帐户，则该用户将由报表服务器计算机上的本地安全机构进行身份验证，并获取对本地资源有效的安全令牌。 域用户帐户的凭据将转发给域控制器并由它进行身份验证。 生成的票证对网络资源有效。  
  
 若要降低凭据在传输到网络中的域控制器时被截获的风险，必须使用通道加密，如传输层安全性 (TLS)（旧称为“安全套接字层 (SSL)”）。 基本身份验证本身以明文形式传输用户名，以 Base-64 编码格式传输密码。 添加信道加密会使数据包不可读。 有关详细信息，请参阅[在本机模式报表服务器上配置 TLS 连接](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md)。  
  
 启用基本身份验证后，请注意在将连接属性设置为向报表提供数据的外部数据源时，用户不能选择 **“Windows 集成安全性”** 选项。 该选项将在数据源属性页中显示为灰色。  
  
> [!NOTE]  
>  以下说明针对本机模式报表服务器。 如果在 SharePoint 集成模式下部署报表服务器，则必须使用指定 Windows 集成安全性的默认身份验证设置。 报表服务器使用默认 Windows 身份验证扩展插件中的内部功能支持 SharePoint 集成模式下的报表服务器。  
  
### <a name="to-configure-a-report-server-to-use-basic-authentication"></a>将报表服务器配置为使用基本身份验证  
  
1. 在文本编辑器中打开 RSReportServer.config。  
  
     若要查找配置文件，请参阅“RsReportServer.config 配置文件”一文中的[文件位置](../report-server/rsreportserver-config-configuration-file.md#bkmk_file_location)部分。
  
2. 查找 \<**Authentication**>。  
  
3. 复制以下最能满足您需要的一个 XML 结构。 第一个 XML 结构提供了用于指定所有元素的占位符，将在下一部分对这些元素进行介绍：  

    [!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)]

    ```xml
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsBasic>  
                       <LogonMethod>3</LogonMethod>  
                       <Realm></Realm>  
                       <DefaultDomain></DefaultDomain>  
                 </RSWindowsBasic>  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    </Authentication>  
    ```  
  
    如果使用的是默认值，则可复制最小元素结构：  
  
    ```xml
          <AuthenticationTypes>  
                 <RSWindowsBasic/>  
          </AuthenticationTypes>  
    ```  

    [!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)] [!INCLUDE [ssrs-appliesto-pbirs](../../includes/ssrs-appliesto-pbirs.md)]

    ```xml
      <Authentication>
          <AuthenticationTypes>
                      <RSWindowsBasic/>
          </AuthenticationTypes>
          <EnableAuthPersistence>true</EnableAuthPersistence>
      <RSWindowsExtendedProtectionLevel>Off</RSWindowsExtendedProtectionLevel>
      <RSWindowsExtendedProtectionScenario>Any</RSWindowsExtendedProtectionScenario>
      </Authentication>
    ```

4. 将其粘贴在 \<**Authentication**> 的现有条目上。  
  
     如果使用的是多个身份验证类型，则只能添加 **RSWindowsBasic** 元素，而不能删除 **RSWindowsNegotiate**、 **RSWindowsNTLM** 或 **RSWindowsKerberos** 的条目。  
  
     请注意，不能将 **Custom** 与其他身份验证类型一起使用。  
  
5. 使用对环境有效的值替换 \<**Realm**> 或 \<**DefaultDomain**> 的空值。  
  
6. 保存文件。  
  
7. 如果配置了扩展部署，请对该部署中的其他报表服务器重复这些步骤。  
  
8. 重新启动报表服务器以清除当前打开的任何会话。  
  
## <a name="rswindowsbasic-reference"></a>RSWindowsBasic 引用  
 配置基本身份验证时，可以指定以下元素。  
  
|元素|必选|有效值|  
|-------------|--------------|------------------|  
|LogonMethod|是<br /><br /> 如果不指定值，将使用 3。|**2** = 网络登录，针对要对纯文本密码进行身份验证的高性能服务器。<br /><br /> **3** = 明文登录，在此情况下，登录凭据保留在随各 HTTP 请求一起发送的身份验证包中，这样，该服务器在连接到网络中的其他服务器时可以模拟该用户。 （默认值）<br /><br /> 注意：[!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] 不  支持值 0（对于交互登录）和 1（对于批处理登录）。|  
|领域|可选|指定包含授权和身份验证功能的资源分区，这些功能用于控制对组织中受保护资源的访问。|  
|默认域|可选|指定服务器用来对用户进行身份验证的域。 此值是可选的。但如果忽略此值，报表服务器会将计算机名称用作域。 如果计算机是域的成员，则该域是默认域。 如果在域控制器上安装了报表服务器，则所用的域为该计算机控制的域。|  
  
## <a name="see-also"></a>另请参阅  
 [报表服务器应用程序的应用程序域](../../reporting-services/report-server/application-domains-for-report-server-applications.md)   
 [Reporting Services 安全性和保护](../../reporting-services/security/reporting-services-security-and-protection.md)  
  
  
