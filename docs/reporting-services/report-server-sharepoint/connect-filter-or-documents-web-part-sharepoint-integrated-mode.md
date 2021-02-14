---
title: 将筛选器或文档 Web 部件与 Reporting Services 报表查看器 Web 部件连接 | Microsoft Docs
description: 对于 SharePoint 产品，学习创建一个包含筛选器 Web 部件或文档 Web 部件和报表查看器 Web 部件的仪表板或 Web 部件页。
ms.date: 11/26/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0e58600c502aa6ebb0a031480ab5bb66f48833ca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074838"
---
# <a name="connect-filter-or-documents-web-part-with-a-reporting-services-report-viewer-web-part"></a>将筛选器或文档 Web 部件与 Reporting Services 报表查看器 Web 部件连接

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

如果在使用 SharePoint 产品，可以创建一个包含筛选器 Web 部件或文档 Web 部件和报表查看器 Web 部件的仪表板或 Web 部件页。 支持的版本为 [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] 或 [!INCLUDE[SPS2010](../../includes/sps2010-md.md)]。 还支持 [!INCLUDE[winSPServ3](../../includes/winspserv3-md.md)] 或 [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] 2007。 通过连接筛选器 Web 部件，在筛选器 Web 部件中选择筛选器值的用户可以将值发送至同一页中的参数化报表。 通过连接文档 Web 部件，在文档库中单击报表的用户可以在相邻的报表查看器 Web 部件中查看报表。

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

 筛选器 Web 部件可用于向报表中的一个或多个参数发送值。 若要使用筛选器 Web 部件，报表必须具有为其定义的参数并且参数与 Web 部件发送的值、数据类型及格式兼容。  
  
 文档 Web 部件与主文件夹站点的文档库相关联。 若要从文档库查看、添加或删除项，请单击 **“查看所有站点内容”** 。 在“库”中，单击 **“文档”** 。 可以使用 **“新建”** 、 **“上载”** 和 **“操作”** 菜单来管理文档库中的项。  
  
## <a name="connect-a-filter-web-part"></a>连接筛选器 Web 部件
  
1.  打开或创建 Web 部件页或仪表板。  
  
2.  在 **“站点操作”** 菜单上，单击 **“编辑页面”** 。  
  
3.  单击“添加 Web 部件”。  
  
4.  在“所有 Web 部件”的“杂项”类别中，选择“SQL Server Reporting Services 报表查看器”。  
  
5.  单击“添加”。 Web 部件将添加到区域的顶部。  
  
6.  在同一 Web 部件页或仪表板中的另一区域上，单击“添加 Web 部件”。  
  
7.  在“所有 Web 部件”的“筛选器”部分，选择一个 Web 部件。  
  
8.  单击“添加”。 Web 部件将添加到区域的顶部。  
  
9. 在包含 Web 部件的区域中，单击 Web 部件的“编辑”菜单，依次指向“连接”、“发送筛选值到”，然后选择“报表查看器” - 报表名称。  
  
10. 签入更改并保存此页。  
  
## <a name="connect-a-documents-web-part"></a>连接文档 Web 部件  
  
1.  打开或创建 Web 部件页或仪表板。  
  
2.  在 **“站点操作”** 菜单上，单击 **“编辑页面”** 。  
  
3.  单击“添加 Web 部件”。  
  
4.  在“所有 Web 部件”的“列表和库”部分中，选择“文档”。  
  
5.  单击“添加”。 Web 部件将添加到区域的顶部。  
  
6.  在工具窗格的底部，单击 **“应用”** ，然后单击 **“确定”** 关闭窗格。  
  
7.  在同一 Web 部件页或仪表板中的另一区域上，单击“添加 Web 部件”。  
  
8.  在“所有 Web 部件”的“杂项”类别中，选择“SQL Server Reporting Services 报表查看器”。  
  
9. 单击“添加”。 Web 部件将添加到区域的顶部。  
  
10. 在包含 Web 部件的区域中，单击 Web 部件的“编辑”菜单，依次指向“连接”、“获取报表定义于”，然后选择“文档”。  
  
11. 签入更改并保存此页。  
  
## <a name="see-also"></a>另请参阅

 [将报表查看器 Web 部件添加到网页](../../reporting-services/report-server-sharepoint/add-the-report-viewer-web-part-to-a-web-page.md)   
 [SharePoint 站点上的报表查看器 Web 部件](./report-viewer-web-part-sharepoint-site.md)   
 [自定义报表查看器 Web 部件](../../reporting-services/report-server-sharepoint/customize-the-report-viewer-web-part.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)