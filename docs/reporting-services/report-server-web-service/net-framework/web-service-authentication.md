---
title: Web 服务身份验证 | Microsoft Docs
description: 如果客户端向报表服务器发出 SOAP 请求，则实现身份验证的客户端部分。 了解如何实现 Web 服务的身份验证。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- Web service [Reporting Services], authentication
- XML Web service [Reporting Services], authentication
- Report Server Web service, authentication
ms.assetid: 852b4947-a090-4e54-8555-5a503945ceab
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f3dfb2b1f3806d9e00e309b45ebf90182c83cab5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021736"
---
# <a name="web-service-authentication"></a>Web 服务身份验证
  可以使用 Windows 身份验证或基本身份验证对针对报表服务器 Web 服务进行的调用进行身份验证。 对报表服务器发出 SOAP 请求的任何客户端都必须实现其中一种支持的身份验证协议的客户端部分。 如果使用的是 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]，则可以使用托管代码 HTTP 类来实现身份验证。 通过使用这些 API，可以轻松地随 SOAP 请求一起发送身份验证信息。  
  
 如果在对报表服务器 Web 服务进行调用之前不具备适当的凭据，则调用将失败。 在运行时，可以通过在调用 Web 服务的方法之前设置表示该 Web 服务的客户端对象的“Credentials”  属性，向该 Web 服务传递凭据。  
  
 以下各节包含使用 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 发送凭据的示例代码。  
  
## <a name="windows-authentication"></a>Windows 身份验证  
 以下代码将 Windows 凭据传递到 Web 服务。  
  
```vb  
Dim rs As New ReportingService()  
rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
```  
  
```csharp  
ReportingService rs = new ReportingService();  
rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
```  
  
## <a name="basic-authentication"></a>基本身份验证  
 以下代码将基本身份验证凭据传递到 Web 服务。  
  
```vb  
Dim rs As New ReportingService()  
rs.Credentials = New System.Net.NetworkCredential("username", "password", "domain")  
```  
  
```csharp  
ReportingService service = new ReportingService();  
service.Credentials = new System.Net.NetworkCredential("username", "password", "domain");  
```  
  
 必须在调用报表服务器 Web 服务的任何方法之前设置凭据。 如果您没有设置凭据，将收到错误代码“HTTP 401 错误: 拒绝访问”。 必须在使用服务之前对其进行身份验证，但在设置凭据之后，只要你继续使用同一个服务变量（如 rs  ），就不需要再次设置这些凭据。  
  
## <a name="custom-authentication"></a>自定义身份验证  
 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 包含一个编程 API，它向开发人员提供了设计和开发自定义身份验证扩展插件（称为安全扩展插件）的机会。 有关详细信息，请参阅 [Implementing a Security Extension](../../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)。  
  
## <a name="see-also"></a>另请参阅  
 [使用 Web 服务和 .NET Framework 生成应用程序](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [报表服务器 Web 服务](../../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
  
