---
title: 创建文档结构图（报表生成器）| Microsoft Docs
description: 了解如何使用文档结构图提供一组导航链接，这些链接指向报表生成器中所呈现报表中的报表项。
ms.date: 05/24/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: c200a97b-67f2-499f-8374-3ed1ebe3f33c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: cd6c70f92cb409ee6742590709743897bb3e77f7
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364439"
---
# <a name="create-a-document-map-report-builder-and-ssrs"></a>创建文档结构图（报表生成器和 SSRS）

文档结构图提供了一组指向所呈现报表中的报表项的导航链接。 当您查看包含文档结构图的报表时，将在报表旁显示一个单独的侧窗格。 用户通过单击文档结构图中的链接，可跳至显示报表项的报表页。 报表的各区域和组将按一定层次结构的链接形式排列。 单击文档结构图中的项会刷新报表，并显示与文档结构图中所单击项对应的报表区域。  
  
 若要向文档结构图添加链接，则需要将报表项的 **DocumentMapLabel** 属性设置为您所创建的文本，或者设置为计算结果为您要在文档结构图中显示的文本的表达式。 还可以向文档结构图添加表或矩阵组的唯一值。 例如，对于基于颜色的组，每个唯一颜色都是指向显示该颜色组实例的报表页的一个链接。  
  
 您还可以创建指向报表的 URL 来覆盖文档结构图，这样在运行报表时可以不显示文档结构图，然后通过单击报表查看器工具栏中的 **“显示/隐藏文档结构图”** 按钮，可切换到显示文档结构图。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="document-maps-and-rendering-extensions"></a><a name="DocMapRenderExtensions"></a> 文档结构图和呈现扩展插件  
 文档结构图专门用于 HTML 呈现扩展插件（如预览和报表查看器）。 其他呈现扩展插件具有不同的呈现文档结构图的方式：  
  
-   PDF 将文档结构图呈现为“书签”窗格。  
  
-   Excel 将文档结构图呈现为包含链接层次结构的命名工作表。 各报表区域在不同的工作表中呈现，这些工作表与文档结构图处于同一工作簿中。  
  
-   Word 中包含作为目录的文档结构图。  
  
-   Atom、TIFF、XML 和 CSV 将忽略文档结构图。  
  
 有关详细信息，请参阅[不同报表呈现扩展插件的交互功能（报表生成器和 SSRS）](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)。  
  
##  <a name="to-add-a-report-item-to-a-document-map"></a><a name="AddRptItemToMap"></a> 向文档结构图添加报表项  
  
1.  在“设计”视图中，选择要添加到文档结构图中的报表项，如表、矩阵或仪表。 报表项属性将显示在“属性”窗格中。  
  
    > [!NOTE]  
    >  若要选择 Tablix 数据区域，请在任意单元内单击以显示行控点和列控点，然后单击角控点。  
  
2.  在“属性”窗格中，在 **DocumentMapLabel** 属性中键入要在文档结构图中显示的文本，或输入计算结果为标签的表达式。 例如，键入 **Sales Chart** 。  
  
    > [!NOTE]  
    >  如果看不到“属性”窗格，请在 **“视图”** 选项卡的 **“显示/隐藏”** 组中选择 **“属性”** 。  
  
3.  对要在文档结构图中显示的每个报表项重复步骤 1 和 2。  
  
4.  单击 **“运行”** 。 将运行报表，并且文档结构图会显示您创建的标签。 单击任一链接，可跳至显示该报表项的报表页。  

  
##  <a name="to-add-unique-group-values-to-a-document-map"></a><a name="AddUniqueValuesToMap"></a> 向文档结构图添加唯一组值  
  
1.  在“设计”视图中，选择包含要在文档结构图中显示的组的表、矩阵或列表。 “分组”窗格随即显示行组和列组。  
  
2.  在“行组”窗格中，右键单击相应的组，然后单击 **“编辑组”** 。 将打开 **“Tablix 组属性”** 对话框的 **“常规”** 页。  
  
3.  单击“高级”。  
  
4.  在 **“文档结构图”** 列表框中，键入或选择与组表达式匹配的表达式。  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  对要在文档结构图中显示的每个组重复步骤 1-4。  
  
7.  单击 **“运行”** 。 将运行报表，并且文档结构图会显示组值。 单击任一链接，可跳至显示该报表项的报表页。  
  
##  <a name="to-hide-the-document-map-when-you-view-a-report"></a><a name="HideMapWhenViewRpt"></a> 在查看报表时隐藏文档结构图  
  
1.  在 Web 门户中，浏览到包含文档结构图的报表。  
  
     例如，对于 [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] 示例报表，下面的 URL 指定名为 Product Catalog 的报表。  
  
    ```  
    https://localhost/Reports/Pages/Report.aspx?ItemPath=%2fAdventureWorks2012+Sample+Reports%2fProduct+Catalog  
    ```  
  
2.  复制服务器中的报表路径。 在该示例中，报表路径为 `%2fAdventureWorks2012+Sample+Reports%2fProduct+Catalog`。  
  
3.  使用以下三个组件创建一个新 URL：  
  
    -   报表服务器中的报表查看器： `https://localhost/ReportServer/Pages/ReportViewer.aspx?`  
  
    -   步骤 1 中复制的报表的名称，例如： `%2fAdventureWorks2012+Sample+Reports%2fProduct+Catalog`  
  
    -   指定隐藏文档结构图的设备信息参数： `&rs%3aCommand=Render&rc%3aFormat=HTML4.0&rc%3aDocMap=False`  
  
     下面的 URL 由此三个组件以其所列顺序追加而成。  
  
    ```  
    https://localhost/ReportServer/Pages/ReportViewer.aspx?  
    %2fAdventureWorks2012+Sample+Reports%2fProduct+Catalog  
    &rs%3aCommand=Render&rc%3aFormat=HTML4.0&rc%3aDocMap=False  
    ```  
  
     若要使用此 URL，请复制此 URL，并删除所有换行符。  
  
4.  将此 URL 粘贴到 Web 门户中，然后按 Enter。 将运行报表，并隐藏文档结构图。  
  
> [!NOTE]  
>  有关下载示例报表的详细信息，请参阅 [报表生成器和报表设计器示例报表](https://social.technet.microsoft.com/wiki/contents/articles/1093.reporting-services-samples-on-codeplex-sql-server-reporting-services-ssrs.aspx)。  
>   
  >  有关详细信息，请参阅 [URL 访问](../url-access-ssrs.md)。 


更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
