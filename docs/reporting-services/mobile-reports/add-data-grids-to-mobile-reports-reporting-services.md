---
title: 向移动报表添加数据网格 | Reporting Services | Microsoft Docs
description: 可以在 SQL Server 移动报表发布服务器的网格中显示数据。 选择简单的数据网格、指示器数据网格或图表数据网格。
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: fe98a970-90d3-44d1-9189-9141c237f141
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e346ff2f6d5b4951e7ce0e3af81914b15b34ac15
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907315"
---
# <a name="add-data-grids-to-mobile-reports--reporting-services"></a>向移动报表添加数据网格 | Reporting Services
有时最佳可视化对象是数据本身。 了解用于在 [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)] 中显示数据的三种数据网格（或表）  ：
* 简单数据网格
* 指示器数据网格
* 图表数据网格

## <a name="simple-data-grid"></a>简单数据网格
最基本的简单数据网格可以通过自定义格式和标头显示多个列的数据。 

![移动报表简单数据网格的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-simple-data-grid.png)

将数据网格添加到设计图面之后，可以将它连接到实际数据。

1. 将简单数据网格从“布局”选项卡拖至设计网格，并将其调整为所需大小  。

2. [从 Excel 或共享数据集获取数据](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)。

3. 选择“数据”选项卡，并在“数据属性”窗格的“网格视图的数据”    下选择一个数据表。

4. 在“列”窗格中，选择所需列  。 重新排列和重命名它们，并设置其格式和聚合。 

 
##  <a name="indicator-data-grid"></a>指示器数据网格
可以向指示器数据网格添加具有仪表的列。

![移动报表指示器数据网格的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-indicator-data-grid.png)

1. 将指示器数据网格从“布局”选项卡拖至设计网格，并将其调整为所需大小  。

2. 在“列”窗格中的“数据”选项卡上，选择“添加仪表列”    。 

3. 选择“选项”，然后选择“仪表类型”   。 

4. 设置“值”和“比较”字段以及“值方向”（如[直接向移动报表添加的仪表](../../reporting-services/mobile-reports/add-gauges-to-mobile-reports-reporting-services.md)中所述）    。

数据网格会自动向仪表仅馈送特定于该数据网格行的数据。  

## <a name="chart-data-grid"></a>图表数据网格
可以向图表数据网格中添加具有仪表或图表的列。 

![移动报表图表数据网格的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-chart-data-grid.png)

将图表列添加到数据网格时，需要添加单独的数据表以便为每行中的图表提供数据。 此第二个数据表需要与主数据表共享一个字段，以便将每行链接到其关联图表数据。 

1. 将图表数据网格从“布局”选项卡拖至设计网格，并将其调整为所需的大小  。

2. 在“列”窗格中的“数据”选项卡上，选择“添加图表列”    。 

3. [从 Excel 或共享数据集获取数据](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md) 以添加与主数据表共享一个字段的第二个数据表（如果尚未这样做）。

4. 在“数据属性”下，在“网格视图的数据”中选择主数据表，然后在“图表可视化效果的引用数据”中选择第二个表    。

5. 选择“选项”，然后选择“图表类型”   。
 
6. 依次选择“图表数据字段”、“源查找”和“目标查找”    。 
   这三个属性决定了数据网格向列中的每个图表提供数据的方式。
   
   *   “源查找”设置为“网格视图的数据”的数据表中的字段   。 此字段充当按行进行的筛选器，该筛选器应用于图表引用数据表，以便为嵌入式表的每行提供数据。 
   * “目标查找”是“图表可视化效果的引用数据”的数据表中的字段   。 将在这两个字段对每个行中的图表数据进行联接。   
   * “图表数据字段”决定了“图表可视化效果的引用数据”数据表中的哪个度量值要用作 Y 轴值或图表中每行中的系列   。  

## <a name="see-also"></a>另请参阅 
* [Reporting Services 移动报表中的地图](../../reporting-services/mobile-reports/maps-in-reporting-services-mobile-reports.md)
* [Reporting Services 移动报表中的导航器](../../reporting-services/mobile-reports/add-navigators-to-reporting-services-mobile-reports.md)
* [Reporting Services 移动报表中的可视化效果](../../reporting-services/mobile-reports/add-visualizations-to-reporting-services-mobile-reports.md)
* [Reporting Services 移动报表中的仪表](../../reporting-services/mobile-reports/add-gauges-to-mobile-reports-reporting-services.md)  
 
  
