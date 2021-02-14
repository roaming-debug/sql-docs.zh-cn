---
title: SharePoint 集成中的报表查看器 Web 部件可编程性 | Microsoft Docs
description: 了解如何创建自定义 Web 部件，以使用 Web 部件连接向报表查看器 Web 部件提供报表路径和参数。
ms.date: 03/04/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: reference
ms.assetid: 714017b7-1bd6-4950-a3c6-d0df8450a877
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d7054e2af081bd76505f3adebbae4cc7541f8a44
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023271"
---
# <a name="report-viewer-web-part-programmability-in-sharepoint-integration"></a>SharePoint 集成中的报表查看器 Web 部件可编程性
  报表查看器 Web 部件是一个服务器控件，其中包含一组公共应用程序编程接口 (API)，使开发人员能够创建自定义 SharePoint 应用程序。 您可以创建自定义 Web 部件，以使用 Web 部件连接向报表查看器 Web 部件提供报表路径和参数。 还可以在自定义 SharePoint Web 部件页中嵌入 Web 部件，并使用该公共 API 对其进行自定义。  
  
## <a name="connecting-to-report-viewer-web-part-with-custom-web-parts"></a>使用自定义 Web 部件连接到报表查看器 Web 部件  
 报表查看器 Web 部件是实现 <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> 或 T:Microsoft.SharePoint.WebPartPages.IFilterValues 的 SharePoint Web 部件的连接使用者。 将某个 <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> Web 部件（如“文档”Web 部件）放在与报表查看器 Web 部件所在的同一 Web 部件页上时，它可以向报表查看器 Web 部件提供报表路径。 同样，将 T:Microsoft.SharePoint.WebPartPages.IFilterValues Web 部件（如“文本筛选器”或“选择筛选器”）放在与报表查看器 Web 部件所在的同一 Web 部件页上时，它可以向报表查看器 Web 部件提供报表参数。  
  
### <a name="implementing-a-report-path-provider-with-iwebpartrow"></a>使用 IWebPartRow 实现报表路径访问接口  
 若要通过 Web 部件连接向报表查看器 Web 部件提供报表路径，请执行以下操作：  
  
1.  创建一个实现 <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> 接口的 Web 部件。  
  
2.  将 Web 部件添加到与报表查看器 Web 部件所在的相同 Web 部件页。  
  
3.  在基于 Web 的 Web 部件设计用户界面中将 Web 部件连接到报表查看器 Web 部件。  
  
    > [!NOTE]  
    >  一次只能将一个 <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> Web 部件连接到报表查看器 Web 部件，并且不能同时将一个 <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> Web 部件和一个 T:Microsoft.SharePoint.WebPartPages.IFilterValues Web 部件连接到报表查看器 Web 部件。  
  
 若要使 <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> Web 部件能够与 T:Microsoft.ReportingServices.SharePoint.UI.WebParts.ReportViewerWebPart 正常工作，必须执行 <xref:System.Web.UI.WebControls.WebParts.IWebPartRow.GetRowData%2A> 方法中的以下操作：  
  
-   使用 <xref:System.Data.DataRowView> 对象作为输入参数调用回调方法。  
  
-   确保 <xref:System.Data.DataRowView> 对象包含一个名为“DocUrl”的列，该列包含报表路径。  
  
    > [!NOTE]  
    >  [!INCLUDE[offSPServ](../includes/offspserv-md.md)] 2010 的外接程序中的报表查看器 Web 部件还支持使用“FileRef”列来接收报表路径。  
  
### <a name="implementing-a-report-parameter-provider-with-ifiltervalues"></a>使用 IfilterValues 实现报表参数访问接口  
 一个实现 T:Microsoft.SharePoint.WebPartPages.IFilterValues 的 Web 部件可以向报表查看器 Web 部件提供一个参数值。 发送到报表查看器 Web 部件的参数值可能会受到与在报表定义中指定的报表参数相同的限制，如数据类型、有效值等。  
  
 若要向报表查看器 Web 部件提供报表参数，请执行以下操作：  
  
1.  创建实现 T:Microsoft.SharePoint.WebPartPages.IFilterValues 接口的 Web 部件。  
  
2.  将 Web 部件添加到与 T:Microsoft.ReportingServices.SharePoint.UI.WebParts.ReportViewerWebPart 所在的同一页面中。  
  
3.  在基于 Web 的 Web 部件设计用户界面中，将 T:Microsoft.SharePoint.WebPartPages.IFilterValues Web 部件连接到报表查看器 Web 部件。  
  
    > [!NOTE]  
    >  可一次将多个 T:Microsoft.SharePoint.WebPartPages.IFilterValues Web 部件连接到报表查看器 Web 部件。 但是，无法同时将一个 <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> Web 部件和一个 T:Microsoft.SharePoint.WebPartPages.IFilterValues Web 部件连接到报表查看器 Web 部件。  
  
  
