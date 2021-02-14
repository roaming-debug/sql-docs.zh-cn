---
title: 在 Windows 应用程序中使用 SOAP API
description: 可以使用 Reporting Services SOAP API 访问报表服务器的功能。 通过调用 Windows 应用中的 Web 服务来访问该服务。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
helpviewer_keywords:
- rendered reports [Reporting Services]
- Windows applications [Reporting Services]
- Windows Forms [Reporting Services]
- SOAP [Reporting Services], Windows applications
ms.assetid: e4804792-20cd-4df2-9257-fb958ff447b4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a91f7cd3a95b722db1c595767ed71900bc55af55
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082548"
---
# <a name="integrating-reporting-services-using-soap---windows-application"></a>使用 SOAP 集成 Reporting Services - Windowsb 应用程序
  可以通过 Reporting Services SOAP API 访问报表服务器的完整功能。 SOAP API 是一个 Web 服务，同样可以轻松地访问此服务以向自定义业务应用程序提供企业报表功能。 只需通过编写对此 Web 服务进行调用的代码，即可在 Windows 应用程序中访问此服务。 使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]，可以生成一个代理类，该类公开此 Web 服务的属性和方法，并使你能够使用熟悉的基础结构和工具来生成建立在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 技术之上的业务应用程序。  
  
## <a name="integrating-report-management-functionality-using-windows-forms"></a>使用 Windows 窗体集成报表管理功能  
 与 URL 访问不同，SOAP API 公开可用于整个报表服务器的一组完整的管理功能。 这意味着，借助于 SOAP，开发人员可以使用报表管理器的完整管理功能。 同样，您可以使用 Windows 窗体开发完整的控制和管理工具。 例如，在 Windows 应用程序中，您可能希望使得用户能够检索报表服务器命名空间的内容。 为此，您可以使用 Web 服务 <xref:ReportService2010.ReportingService2010.ListChildren%2A> 方法列出报表服务器数据库中的所有项，然后使用 Listview、Treeview 或 Combobox 控件向用户显示这些项。 以下 Web 服务代码可用于在用户单击窗体上的某个按钮时，检索用户的“我的报表”文件夹中可用报表的当前列表：  
  
```vb  
' Button click event that retrieves a list of reports from  
' the My Reports folder and displays them in a combo box  
Private Sub listReportsButton_Click(sender As Object, e As System.EventArgs)  
   ' Create a new Web service object and set credentials  
   ' to Windows Authentication  
   Dim rs As New ReportingService2010()  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
  
   ' Return the list of items in My Reports  
   Dim items As CatalogItem() = rs.ListChildren("/Adventureworks 2008 Sample Reports", False)  
  
   Dim ci As CatalogItem  
   For Each ci In  items  
      ' If the item is a report, add it to   
      ' a combo box  
      If ci.TypeName = "Report" Then  
         catalogComboBox.Items.Add(ci.Name)  
      End If  
   Next ci  
End Sub 'listReportsButton_Click  
```  
  
```csharp  
// Button click event that retrieves a list of reports from  
// the My Reports folder and displays them in a combo box  
private void listReportsButton_Click(object sender, System.EventArgs e)  
{  
   // Create a new Web service object and set credentials  
   // to Windows Authentication  
   ReportingService2010 rs = new ReportingService2010();  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
  
   // Return the list of items in My Reports  
   CatalogItem[] items = rs.ListChildren("/Adventureworks 2008 Sample Reports", false);  
  
   foreach (CatalogItem ci in items)  
   {  
      // If the item is a report, add it to   
      // a combo box  
      if (ci.TypeName == "Report")  
         catalogComboBox.Items.Add(ci.Name);  
   }  
}  
```  
  
 从此处，您可以让用户通过 Web 浏览器控件或图像控件，从组合框中选择报表并在窗体上预览此报表。  
  
## <a name="enabling-report-viewing-and-navigation-using-windows-forms"></a>使用 Windows 窗体启用报表查看和导航  
 可以通过两种方法将报表集成到 Windows 窗体应用程序中。  
  
 可以使用 SOAP API 通过 <xref:ReportExecution2005.ReportExecutionService.Render%2A> 方法将报表呈现为所支持的任何呈现格式。 通过 SOAP 启用报表查看和导航也有一些缺点，但这些缺点微不足道：  
  
-   您无法利用报表工具栏（通过 URL 访问随 HTML 查看器提供）的内置功能。  
  
-   如果您呈现到 HTML，则必须使用 <xref:ReportExecution2005.ReportExecutionService.RenderStream%2A> 方法将任何图像或资源单独呈现为其他流。  
  
-   使用 URL 访问呈现报表比使用 SOAP API 在性能方面具有轻微的优势。  
  
 然而，可以使用 SOAP API 的 <xref:ReportExecution2005.ReportExecutionService.Render%2A> 方法以编程方式呈现报表并将它们保存为不同的输出格式。 这是强于 URL 访问的一个优势，它要求用户交互。 当您使用 SOAP API <xref:ReportExecution2005.ReportExecutionService.Render%2A> 方法呈现报表时，您可以呈现为所支持的任何输出格式。  
  
 还可以使用随 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsOrcas](../../includes/vsorcas-md.md)] 提供的免费分发的报表查看器控件。 通过报表查看器控件，可以轻松地将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 功能嵌入到自定义应用程序中。 报表查看器控件供开发人员使用，以提供预先设计并编写完全的报表，作为应用程序功能集的一部分（例如，网站管理应用程序可能包含显示对公司网站的点击流分析的报表）。 在应用程序中嵌入控件为在应用程序部署中包含 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务器组件提供了简化的替代方法。 这些控件提供报表功能，但不提供可在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中找到的其他对报表创作、发布或分发和传递的支持。  
  
 报表查看器控件有两个版本，一个用于各种 Windows 客户端应用程序，另一个用于 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 应用程序。 这两个控件都支持本地处理模式和远程处理模式。 在本地处理模式中，应用程序提供报表定义以及数据集报表处理和触发器报表处理。 在远程处理模式中，数据检索和报表处理在报表服务器上进行，而 ReportViewer 控件用于显示和报表导航。 使用此模式可以生成丰富的应用程序，小至桌面应用程序，大到企业级应用程序。  
  
 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 联机帮助中有关于报表查看器控件的记载。 有关详细信息，请参阅 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 产品文档。  
  
## <a name="see-also"></a>另请参阅  
 [使用 Web 服务和 .NET Framework 生成应用程序](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [将 Reporting Services 集成到应用程序中](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md)   
 [在 Web 应用程序中使用 SOAP API](../../reporting-services/application-integration/integrating-reporting-services-using-soap-web-application.md)  
  
  
