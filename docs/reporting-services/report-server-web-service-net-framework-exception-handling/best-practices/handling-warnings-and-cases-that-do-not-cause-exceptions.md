---
title: 处理不导致异常的警告和事例 | Microsoft Docs
description: 了解如何处理不导致异常的警告和事例，以便采取适当的操作。
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-exception-handling
ms.topic: reference
helpviewer_keywords:
- exceptions [Reporting Services], warnings that don't cause
- warnings [Reporting Services]
ms.assetid: 475c0713-6265-44e7-9ebc-ebdd1b89e0af
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 41fce911539fdae4e1d0655fe488fa1e0bc7ee9e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023626"
---
# <a name="handling-warnings-and-cases-that-do-not-cause-exceptions"></a>处理不导致异常的警告和事例
  对于警告和某些错误，[!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 不引发异常。 例如，在您使用 <xref:ReportService2010.ReportingService2010.CreateCatalogItem%2A> 方法以便将新的报表发布到报表服务器时，出现的任何警告都将以 <xref:ReportService2010.Warning> 对象的数组形式返回。 应处理和显示这些警告，以便采取相应操作。  
  
```vb  
Try  
   rs.CreateCatalogItem(name, parentFolder, False, definition, Nothing, warnings)  
  
   If Not (warnings.Length = 0) Then  
      Dim warning As Warning  
      For Each warning In warnings  
         Console.WriteLine(warning.Message)  
      Next warning  
   Else  
      Console.WriteLine("Report {0} created successfully with no warnings", name)  
   End If  
  
Catch ex As SoapException  
   Console.WriteLine(ex.Detail("Message").InnerXml)  
End Try  
```  
  
```csharp  
try  
{  
   rs.CreateCatalogItem("Report", name, parentFolder, false, definition, null, out warnings);  
  
   if (warnings.Length != 0)  
   {  
      foreach (Warning warning in warnings)  
      {  
         Console.WriteLine(warning.Message);  
      }  
   }  
   else  
      Console.WriteLine("Report {0} created successfully with no warnings", name);  
}  
  
catch (SoapException ex)  
{  
   Console.WriteLine(ex.Detail["Message"].InnerXml);  
}  
```  
  
 处理错误的另一个方法是评估某些方法的返回值。 例如，<xref:ReportService2010.ReportingService2010.FindItems%2A> 方法可用于搜索报表服务器数据库中的特定项。 如果未找到匹配搜索条件的项，则返回空的 <xref:ReportService2010.CatalogItem> 对象的数组。 应对该数组进行评估、检查 null 并让用户知道未找到任何项  。  
  
## <a name="see-also"></a>另请参阅  
 <xref:ReportService2010.CatalogItem>   
 [介绍 Reporting Services 中的异常处理](../../../reporting-services/report-server-web-service-net-framework-exception-handling/introducing-exception-handling-in-reporting-services.md)   
 [Reporting Services SoapException 类](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/reporting-services-soapexception-class.md)  
  
  
