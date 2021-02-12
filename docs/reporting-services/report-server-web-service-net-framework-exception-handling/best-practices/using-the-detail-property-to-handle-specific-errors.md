---
title: 使用 Detail 属性处理特定错误 | Microsoft Docs
description: 了解如何通过使用 Detail 属性处理特定错误来访问消息子元素的内部文本。
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-exception-handling
ms.topic: reference
helpviewer_keywords:
- exceptions [Reporting Services], Detail property
- Detail property
- InnerText property
ms.assetid: 4392633d-b46b-41e6-bc12-efb64e166704
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d22bfdf0659b17e7730c25aef7369d4fa323bce6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023453"
---
# <a name="using-the-detail-property-to-handle-specific-errors"></a>使用 Detail 属性处理特定错误
  为了进一步对异常进行分类，[!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 在 SOAP 异常的 Detail 属性的子元素的 InnerText 属性中返回附加错误信息   。 因为该 Detail 属性是 XmlNode 对象，所以，可以使用以下代码访问 Message 子元素的内部文本    。  
  
 有关在 Detail 属性中包含的所有可用子元素的列表，请参阅 [Detail 属性](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/detail-property.md)  。 有关详细信息，请参阅 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] SDK 文档中的“Detail 属性”。  
  
```vb  
Try  
' Code for accessing the report server  
Catch ex As SoapException  
   ' The exception is a SOAP exception, so use  
   ' the Detail property's Message element.  
   Console.WriteLine(ex.Detail("Message").InnerXml)  
End Try  
```  
  
```csharp  
try  
{  
   // Code for accessing the report server  
}  
catch (SoapException ex)  
{  
   // The exception is a SOAP exception, so use  
   // the Detail property's Message element.  
   Console.WriteLine(ex.Detail["Message"].InnerXml);  
}  
```  
  
```vb  
Try  
' Code for accessing the report server  
Catch ex As SoapException  
   If ex.Detail("ErrorCode").InnerXml = "rsInvalidItemName" Then  
   End If ' Perform an action based on the specific error code  
End Try  
```  
  
```csharp  
try  
{  
   // Code for accessing the report server  
}  
catch (SoapException ex)  
{  
   if (ex.Detail["ErrorCode"].InnerXml == "rsInvalidItemName")  
   {  
      // Perform an action based on the specific error code  
   }  
}  
```  
  
 下面的代码行将在 SOAP 异常中返回的特定错误代码写入控制台。 您还可以对错误代码进行评估并执行特定操作。  
  
```vb  
Console.WriteLine(ex.Detail("ErrorCode").InnerXml)  
```  
  
```csharp  
Console.WriteLine(ex.Detail["ErrorCode"].InnerXml);  
```  
  
## <a name="see-also"></a>另请参阅  
 [介绍 Reporting Services 中的异常处理](../../../reporting-services/report-server-web-service-net-framework-exception-handling/introducing-exception-handling-in-reporting-services.md)   
 [Reporting Services SoapException 类](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/reporting-services-soapexception-class.md)   
 [SoapException 错误表](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/soapexception-errors-table.md)  
  
  
