---
title: SOAP 在 Reporting Services 中的作用 | Microsoft Docs
description: 报表服务器 Web 服务使用 HTTP 上的 SOAP。 报表管理器向报表服务器数据库（其中存储报表和相关的内容）提供了一个接口。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- Web service [Reporting Services], SOAP
- SOAP [Reporting Services], role in Reporting Services
- Report Server Web service, SOAP
- XML Web service [Reporting Services], SOAP
ms.assetid: f229c3ef-f2ca-448f-98f1-b8df350b9992
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 616171eb0b641149cf7b27d7c45c04c20daa8c64
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021709"
---
# <a name="the-role-of-soap-in-reporting-services"></a>The Role of SOAP in Reporting Services
  报表服务器 Web 服务使用简单对象访问协议 (SOAP) 消息通过网络发送基于文本的命令。 这些命令采用的形式为使用 HTTP 通过万维网发送的 XML 文本。 通过将 SOAP 用作其通信协议，报表服务器 Web 服务使应用程序和组件能够使用开放和广为接受的基础结构与报表服务器交换数据。 SOAP 标准在 www.w3.org/TR/SOAP 中定义。  
  
 对于任何客户端应用程序，只要它识别 SOAP 且可以发送 SOAP 请求，它就可以充当 SOAP 客户端。 报表管理器就是一个此类 SOAP 客户端。 它向报表服务器数据库（其中存储所有报表和与报表相关的内容）提供了一个接口。 每个用户都可以使用此应用程序对报表服务器命名空间中的报表和文件夹进行浏览和管理。 报表管理器建立在报表服务器 Web 服务基础结构之上。  
  
 报表服务器充当 SOAP 服务器，它是一项识别 SOAP 的服务，可以从 SOAP 客户端接受请求并创建适当的响应。 此服务器处理请求并将经过编码的响应发回到客户端。  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中的 SOAP 消息采取许多不同的形式，具体取决于客户端发出的请求类型。 以下示例表示一个简单 SOAP 客户端请求，它请求从报表服务器数据库中删除某个项。  
  
```  
<soap:Envelope xmlns:soap="https://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
    <soap:Body>  
        <DeleteItem xmlns="https://www.microsoft.com/sql/ReportingServer">  
            <item>/Samples/Report1</item>  
        </DeleteItem>  
    </soap:Body>  
</soap:Envelope>  
```  
  
 SOAP 自身要求将消息放到 Envelope  元素中，并且将消息的主体放入 Body  元素内。 在本示例中，主体包含对于 <xref:ReportService2010.ReportingService2010.DeleteItem%2A> 方法的调用，该方法采用一个表示待删除项的路径的字符串参数。 可以创建 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 客户端代理类，以便将所有 SOAP 操作都封装到方法中。 下面的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[csprcs](../../includes/csprcs-md.md)] 方法表示前面给出的 SOAP 示例。  
  
```  
public void DeleteItem(string item);  
```  
  
 来自服务器的响应可能如下所示：  
  
```  
<soap:Envelope xmlns:soap="https://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
    <soap:Body>  
        <DeleteItemResponse xmlns="https://www.microsoft.com/sql/ReportingServer" />  
    </soap:Body>  
</soap:Envelope>  
```  
  
 <xref:ReportService2010.ReportingService2010.DeleteItem%2A> 方法不具备返回值，因此返回空响应。  
  
## <a name="see-also"></a>另请参阅  
 [访问 SOAP API](../../reporting-services/report-server-web-service/accessing-the-soap-api.md)   
 [报表管理器（SSRS 本机模式）](../web-portal-ssrs-native-mode.md)   
 [Reporting Services 报表服务器](../../reporting-services/report-server-sharepoint/reporting-services-report-server.md)   
 [报表服务器 Web 服务](../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
