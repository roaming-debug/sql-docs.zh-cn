---
title: 向 Reporting Services 移动报表添加导航器 | Microsoft Docs
description: 在移动报表发布服务器中，添加导航器以按时间或所选内容筛选数据。 导航器类似于 Power BI 和 Excel 数据透视表中的切片器。
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: e141f50e-49a9-46c6-983c-f656013aa07c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 640f8a5b04f9045f905f86ef6d34924ea849bd80
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907079"
---
# <a name="add-navigators-to-reporting-services-mobile-reports"></a>Add navigators to Reporting Services mobile reports
在 [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)]中，你将添加 *导航器* 来按时间或按选定内容筛选可视化效果中的数据。 

导航器类似于 Power BI 和 Excel 数据透视表中的切片器，但导航器也有一些独特的特征。

**基于时间的导航器** 通过选择属于特定时间范围内的行对表进行筛选。 

**基于所选内容的导航器** 通过选择特定列值与所选键值一致的行对表进行筛选，或者在层次结构树下（特定列值属于所选键值的子树）对表进行筛选。 选择导航器有两种：
* 选择列表是可用于筛选移动报表的单列表，类似于 Power BI 和 Excel 中的切片器。
* 记分卡网格也可以筛选移动报表，同时还可以包含 
  
## <a name="time-navigators"></a>时间导航器   
  
正如其名，时间导航器用于筛选某个时间范围所绑定的数据范围。   
  
![SSMRP_TimeNav](../../reporting-services/mobile-reports/media/ssmrp-timenav.png)  
左侧的四个折线图在“时间范围预设”中设置。  右侧的折线图是筛选器。  
  
当你在预览模式下或在 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] Web 门户中查看报表时，可拖动时间导航器中的箭头来筛选报表的其余部分。  
  
![SSMRP_TimeNavPreview](../../reporting-services/mobile-reports/media/ssmrp-timenavpreview.png)  
  
默认情况下，时间导航器会筛选报表中连接到基于时间的数据的所有视觉对象。 如果表中包含多个基于时间的列，则只有第一列用于筛选。 系列表推动嵌入的可视化效果，并确定移动报表的所有日期范围。  
  
你可以从时间导航器断开某个可视化效果的连接。   
1. 选择该可视化效果，然后选择“数据”选项卡  。  
2. 在“数据属性”中，选择“选项”   。  
3. 清除“筛选依据”复选框  。  
  
   ![SSMRP_ClearTimeFilter](../../reporting-services/mobile-reports/media/ssmrp-cleartimefilter.png)  
  
## <a name="selection-lists"></a>选择列表   
  
选择列表通过将列表中的选定值与筛选表中每行的指定列的值相匹配来筛选移动报表中的数据。 

1. 从“布局”选项卡中，将“选择列表”拖至设计图面并根据需要重设其大小   。

2. 选择“数据”选项卡，并在“数据属性”窗格中的“键”下面选择表以及将作为筛选器的列    。 

3. 在“标签”下面，选择将显示该标签的列  。 键列和标签列可以相同。  
  
   对于层次结构树数据，请选择父键列。  
  
4. 设置数据属性后，在“按选择列表筛选表”下面，选择要筛选的表以及要作为筛选依据的列  。 此列需要匹配选择列表的键列中的值。 

对于希望选择列表筛选的移动报表中的每个可视化效果：

1. 选择该可视化效果，选择“数据”选项卡，然后在“数据属性”窗格中，选择字段名称旁边的“选项”    。

   ![展开的“类别列选项”的屏幕截图，其中选择了“按州/地区筛选”选项。](../../reporting-services/mobile-reports/media/mobile-report-set-selection-list.png)

2. 在“筛选依据”下面，选择选择列表  。

当你在预览模式下或在 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] Web 门户中查看移动报表并在选择列表中选择某个值时，它将筛选移动报表中的其他可视化效果。

![屏幕截图显示了移动报表，其中从“州/地区”列表中选择了“华盛顿”并在返回结果中显示了华盛顿的各个城市。](../../reporting-services/mobile-reports/media/mobile-report-selection-list-filtering.png) 
     
## <a name="scorecard-grid"></a>记分卡网格  
  
记分卡网格筛选器的功能与选择列表筛选器非常类似，但它还会显示值列和分数指示器（与指示器数据网格中的指示器相同）。 在选择键列、标签列和可选父键列后，需选择输入表和输入列以向记分卡提供数据。 记分卡数据列应该可以根据键列进行筛选。  

1. 从“布局”选项卡中，将“记分卡网格”拖至设计图面并根据需要重设其大小   。

2. 选择“数据”选项卡，并在“数据属性”窗格中的“键”下面选择表以及将作为筛选器的列    。 

3. 在“标签”下面，选择将显示该标签的列  。 键列和标签列可以相同。  
  
4. 若要添加分数指示器，请在“数据列”窗格中，选择“添加得分”   。   
  
5. 为分数指示器命名，并选择“选项”以设置与你为数据网格中的指示器所设置的同一属性  ：  
  
   * 仪表类型
   * 值字段
   * 比较字段
   * 值方向
  
6. 若要添加值指示器，请在“数据列”窗格中，选择“添加值”   。

7. 根据需要命名值指示器，从表中选择其源列，并选择如何对其进行格式处理。  

   ![屏幕截图显示了适用于新评分的选项。](../../reporting-services/mobile-reports/media/mobile-report-scorecard-grid-data-properties.png)

8. 设置数据属性后，在“按选择列表筛选表”下面，选择要筛选的表以及要作为筛选依据的列  。 此列需要匹配选择列表的键列中的值。 

当你在预览模式下或在 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] Web 门户中查看移动报表并在记分卡网格中选择某个值时，它将筛选移动报表中的其他可视化效果。

![移动报表记分卡网格的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-scorecard-grid.png)
    
## <a name="set-which-visualizations-are-filtered"></a>设置要筛选的可视化效果  
  
配置库元素，以通过单击针对数据视图中特定输入的“选项”按钮使用筛选器。 通过切换相应的复选框启用筛选器。  

你可以决定移动报表中导航器将筛选的可视化效果：

1. 选择该可视化效果，选择“数据”选项卡，然后在“数据属性”窗格中，选择字段名称旁边的“选项”    。

   ![展开的“类别列选项”的另一个屏幕截图，其中选择了“按州/地区筛选”选项。](../../reporting-services/mobile-reports/media/mobile-report-set-selection-list.png)

2. 在“筛选依据”下面，选择导航器  。 每个可视化效果可以按多个导航器进行筛选。
  
## <a name="cascading-filters"></a>级联筛选器   
  
还可以将筛选器级联在一起以便其中一个筛选器的所选值可以在另一筛选器中筛选可用值。 若要级联筛选器，请向键列应用筛选器，其操作方式与应用常规库元素相同。  

### <a name="see-also"></a>另请参阅 
  
* [Reporting Services 移动报表中的地图](../../reporting-services/mobile-reports/maps-in-reporting-services-mobile-reports.md)
* [Reporting Services 移动报表中的可视化效果](../../reporting-services/mobile-reports/add-visualizations-to-reporting-services-mobile-reports.md)
* [Reporting Services 移动报表中的仪表](../../reporting-services/mobile-reports/add-gauges-to-mobile-reports-reporting-services.md)
* [Reporting Services 移动报表中的数据网格](../../reporting-services/mobile-reports/add-data-grids-to-mobile-reports-reporting-services.md)  
