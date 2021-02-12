---
description: 为 GetProperties 方法设置项命名空间
title: 为 GetProperties 方法设置项命名空间 | Microsoft Docs
Description: 了解如何使用 GetProperties 方法和 ItemNamespaceHEADER SOAP 标头检索基于项的路径或 ID 的属性。
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-soap-headers
ms.topic: reference
helpviewer_keywords:
- item properties [Reporting Services]
- ItemNamespaceHeader SOAP header
- GetProperties method
ms.assetid: b0a08639-3101-40a2-abe2-3a41753826d1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 45edea2b8f4b489f7681ded859a4f81a06047b5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044967"
---
# <a name="setting-the-item-namespace-for-the-getproperties-method"></a>为 GetProperties 方法设置项命名空间
  可以根据两个不同的项标识符在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中使用 <xref:ReportService2010.ItemNamespaceHeader> SOAP 标头：项的完整路径或项的 ID。  
  
 当您调用 <xref:ReportService2010.ReportingService2010.GetProperties%2A> 方法时，通常将要检索其属性的项的完整路径作为参数传递。 使用 <xref:ReportService2010.ItemNamespaceHeader>，您可以为方法调用设置 SOAP 标头，以便通过将项的 ID 作为标识符传递来使用 <xref:ReportService2010.ReportingService2010.GetProperties%2A>。  
  
 以下代码示例根据项的 ID 检索项属性的值。  
  
> [!NOTE]  
>  默认情况下，如果您将完整路径名称作为项标识符传递到 <xref:ReportService2010.ItemNamespaceHeader> 方法，则不需要为 <xref:ReportService2010.ReportingService2010.GetProperties%2A> 设置值。  
  
```vb  
Imports System  
Imports System.Collections  
  
Class Sample  
   Sub Main()  
      Dim rs As New ReportingService2010()  
      rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
      rs.Url = "https://<Server Name>/reportserver/ReportService2010.asmx"  
  
      Dim items() As CatalogItem  
  
      Try  
         ' Need the ID property of items. Normally, you would already have   
         ' this stored somewhere.  
         items = rs.ListChildren("/AdventureWorks Sample Reports", False)  
  
         ' Set the item namespace header to be GUID-based  
         rs.ItemNamespaceHeaderValue = New ItemNamespaceHeader()  
         rs.ItemNamespaceHeaderValue.ItemNamespace = ItemNamespaceEnum.GUIDBased  
  
         ' Call GetProperties with item ID.  
         If Not (items Is Nothing) Then  
            Dim item As CatalogItem  
            For Each item In  items  
               Dim properties As [Property]() = rs.GetProperties(item.ID, Nothing)  
               Dim property As [Property]  
               For Each property In  properties  
                  Console.WriteLine(([property].Name + ": " + [property].Value))  
               Next property  
               Console.WriteLine()  
            Next item  
         End If  
  
      Catch e As Exception  
         Console.WriteLine((e.Message + ": " + e.StackTrace))  
      End Try  
   End Sub 'Main  
End Class 'Sample  
```  
  
```csharp  
using System;  
using System.Collections;  
  
class Sample  
{  
   static void Main()  
   {  
   ReportingService2010 rs = new ReportingService2010();  
      rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
      rs.Url = "https://<Server Name>/reportserver/ReportService2010.asmx";  
  
      CatalogItem[] items;  
  
      try  
      {  
         // Need the ID property of items. Normally, you would already have   
         // this stored somewhere.  
         items = rs.ListChildren("/AdventureWorks Sample Reports", false);  
  
         // Set the item namespace header to be GUID-based  
         rs.ItemNamespaceHeaderValue = new ItemNamespaceHeader();  
         rs.ItemNamespaceHeaderValue.ItemNamespace = ItemNamespaceEnum.GUIDBased;  
  
         // Call GetProperties with item ID.  
         if (items != null)  
         {  
            foreach( CatalogItem item in items)  
            {  
               Property[] properties = rs.GetProperties(item.ID, null);  
               foreach (Property property in properties)  
               {  
                  Console.WriteLine(property.Name + ": " + property.Value);  
               }  
               Console.WriteLine();  
            }  
         }  
      }  
  
      catch (Exception e)  
      {  
         Console.WriteLine(e.Message);  
      }  
   }  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [技术参考 (SSRS)](../../reporting-services/technical-reference-ssrs.md)   
 [使用 Reporting Services SOAP 标头](../../reporting-services/report-server-web-service-net-framework-soap-headers/using-reporting-services-soap-headers.md)  
  
  
