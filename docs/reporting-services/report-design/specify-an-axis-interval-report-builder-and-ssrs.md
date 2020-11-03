---
title: 指定轴间隔（报表生成器）| Microsoft Docs
description: 了解如何设置报表生成器中的轴间隔，以更改图表中类别 (x) 轴上的标签数目和刻度线数目。
ms.date: 09/02/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: ae46712d-a5bf-44c0-9929-e30ccc1e7e33
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6b756d7db3a41787da2a9a1a30f1c82b9a72317c
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907175"
---
# <a name="specify-an-axis-interval-report-builder-and-ssrs"></a>指定轴间隔（报表生成器和 SSRS）
了解如何设置 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 分页报表中的轴间隔，以更改图表中类别 (x) 轴上的标签数目和刻度线数目。
 
在值轴（通常为 y 轴）上，轴间隔提供图表上数据点的一致度量。 

但在类别轴（通常为 x 轴）上，自动轴间隔有时会产生不带轴标签的类别。 可以在轴 Interval 属性中指定所需间隔数。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 根据结果集中的数据在运行时计算间隔数。 有关轴间隔计算方式的详细信息，请参阅 [设置图表上轴标签的格式](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md)。  

若要尝试使用示例数据设置轴间隔，请参阅[教程：向报表添加柱形图（报表生成器）](../tutorial-add-a-column-chart-to-your-report-report-builder.md)。
  
> [!NOTE]  
>  类别轴通常是水平轴（或 x 轴）。 但对于条形图来说，类别轴是指垂直轴（或 y 轴）。  
>
> 本主题不适用于：
>-   类别轴上的日期或时间值。 默认情况下， **DateTime** 值显示为天数。 可以指定不同的日期或时间间隔，例如月间隔或时间间隔。 有关详细信息，请参阅 [将轴标签的格式设置为日期或货币](../../reporting-services/report-design/format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs.md)。  
>-  饼图、圆环图、漏斗图或棱锥图，这些图表没有轴。 
  
## <a name="to-show-all-the-category-labels-on-the-x-axis"></a>在 x 轴上显示所有类别标签  

在此柱形图中，水平标签间隔设置为“自动”。

![报表生成器柱形图预览视图的屏幕截图，其中 X 轴间隔设为“自动”。](../../reporting-services/report-design/media/report-builder-column-chart-preview-x-axis-interval-auto.png)
  
1.  右键单击类别轴，然后单击“水平轴属性”。   

    ![报表生成器柱形图的屏幕截图，其中显示了如何设置 X 轴标签。](../../reporting-services/report-design/media/report-builder-column-chart-x-axis-labels.png)
  
2.  在“水平轴属性”对话框 >“轴选项”选项卡中，将“间隔”设置为“1”，以显示每个类别组标签。 若要在 x 轴上显示所有其他类别组标签，请键入 **2** 。 

     ![报表生成器柱形图的屏幕截图，其中显示了如何为柱形图设置 X 轴间隔。](../../reporting-services/report-design/media/report-builder-column-chart-x-axis-interval-one.png)
  
3. [!INCLUDE[clickOK](../../includes/clickok-md.md)]
     
     现在，柱形图显示其所有水平轴标签。
     
     ![报表生成器柱形图预览视图的屏幕截图，其中显示了 X 轴标签。](../../reporting-services/report-design/media/report-builder-column-chart-preview-x-axis-interval-one.png)
     
     > [!NOTE]  
     >  设置轴间隔后，将禁用所有自动标签。 如果指定轴间隔的值，则可能会遇到不可知的标签行为，具体取决于类别轴上的类别数。  

## <a name="change-the-label-interval-in-properties-pane"></a>在“属性”窗格中更改标签间隔

还可以在“属性”窗格中设置标签间隔。

1.  在报表设计视图中，单击图表，然后选择水平轴标签。

3. 在“属性”窗格中，将 LabelInterval 设置为“1”。

    ![报表生成器柱形图的屏幕截图，其中显示了如何设置标签间隔。](../../reporting-services/media/report-builder-column-chart-set-label-interval.png)

    该图表同设计视图中一样。 
    
5.  单击 **“运行”** 以预览报表。

    ![报表生成器柱形图预览视图的屏幕截图，其中显示了柱形图的标签间隔。](../../reporting-services/media/report-builder-column-chart-label-interval-one-preview.png)
    
    现在，图表显示其所有标签。
  
## <a name="to-enable-a-variable-interval-calculation-on-an-axis"></a>启用轴上的可变间隔计算  

默认情况下， [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 将轴间隔设置为“自动”。此过程说明如何将其设置回默认值。 
  
1.  右键单击要更改的图表轴，然后单击 **“轴属性”** 。 
  
2.  在“水平轴属性”对话框 >“轴选项”选项卡中，将“间隔”设置为“自动”。该图表将显示适合该轴的最佳类别标签数。  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [设置图表格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [设置图表上数据点的格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-data-points-on-a-chart-report-builder-and-ssrs.md)   
 [对数据区域中的数据排序（报表生成器和 SSRS）](../../reporting-services/report-design/sort-data-in-a-data-region-report-builder-and-ssrs.md)   
 [“轴属性”对话框 ->“轴选项”（报表生成器和 SSRS）](/previous-versions/sql/)   
 [指定对数刻度（报表生成器和 SSRS）](../../reporting-services/report-design/specify-a-logarithmic-scale-report-builder-and-ssrs.md)   
 [在辅助轴上绘制数据（报表生成器和 SSRS）](../../reporting-services/report-design/plot-data-on-a-secondary-axis-report-builder-and-ssrs.md)  
  
