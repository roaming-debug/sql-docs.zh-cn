---
title: 向移动报表添加仪表 | Reporting Services | Microsoft Docs
description: 你可以向 Reporting Services 移动报表添加仪表。 仪表可以单独显示数据集中的单个值，也可以与目标进行比较。
ms.date: 03/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: 76d8fc8f-c37f-44d3-ab44-45fbeed4e064
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 204885c83b400be7f134a7c8a5e622f3c3488797
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907305"
---
# <a name="add-gauges-to-mobile-reports--reporting-services"></a>向移动报表添加仪表 | Reporting Services
仪表是移动报表中最基本且使用最广泛的视觉对象。 它们显示数据集中的单个值 - 值本身，或相比于目标的值。

![PBI_SSMRP_Gauges](../../reporting-services/mobile-reports/media/pbi-ssmrp-gauges.png)  
  
*“布局”选项卡中的仪表可视化效果*  
  
在 SQL Server 移动报表发布服务器中，所有仪表具有至少一个共同属性：一个主值，设置为移动报表某个数据表中的数值字段。  

除数字仪表外的所有仪表还可以显示比较值或 *差异* 值 — 主值和比较值之间的关系。 比较值通常是目标，而仪表是达到该目标的进度或实际值与目标值之间的差异的可视指示器。

仪表只能表示其主值的一个聚合值以及其比较值的一个聚合值。 仪表聚合为标准聚合 — 总和、平均值、最小值、最大值等等。 默认情况下，仪表值为总和，用于显示包含在当前已筛选数据（供仪表控件使用）中的所有值的总计。 

可以通过将仪表值连接到移动报表上的导航器来筛选仪表值。 

## <a name="set-the-main-and-comparison-values-for-a-gauge"></a>设置仪表的主值和比较值

1. 将仪表从“布局”选项卡拖至设计网格，并将其调整为所需的大小  。

2. [从 Excel 或共享数据集获取数据](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)。

3. 选择“数据”选项卡，并在“数据属性”窗格的“主值”下面选择数据表和数值字段    。

3. 在除数字仪表外的任何仪表中，在“数据属性”窗格的“比较值”下面选择数据表和数值字段   。

4. [可选] 若要更改聚合，请选择“选项”，然后选择其他聚合  。
   
   >**注意** ：如果更改主值的聚合，可能还需要更改比较值的聚合，尽管在某些情况下，可能需要混用聚合方法。  

## <a name="filter-a-gauge"></a>筛选仪表
  
如果移动报表有任何导航器，则可以将仪表绑定到其中一个或多个导航器以对其进行筛选。 可以将仪表的值和比较值绑定到一个或多个不同的导航器，从而为仪表提供几乎无限的选项。  

1. 选择仪表，然后在“数据”选项卡的“数据属性”窗格中，选择“主值”或“比较值”旁边的”选项”      。

2. 在“筛选依据”下面选择要筛选仪表的导航器。

   ![展开的“主值选项”的屏幕截图，其中选择了“按产品列表筛选”选项。](../../reporting-services/mobile-reports/media/mobile-report-gauge-navigator.png)
 
## <a name="set-visual-properties-for-a-gauge"></a>设置仪表的视觉对象属性
  
除了将仪表元素连接到数据字段的数据属性，还可以自定义多种功能属性和视觉对象属性。 

### <a name="set-value-direction-high-or-low-is-better"></a>设置值方向：“越高越好”或“越低越好”
* 选择仪表，然后在“布局”选项卡的“视觉对象属性”窗格中，将“值方向”设置为“值越大越好”或“值越小越好”      。 

“值越大越好”将正值显示为绿色（表示会有好结果的理想更改），将较小的值显示为红色（表示会有坏结果的不理想更改）  。 

“值越小越好”的颜色正好相反  。

值方向属性仅涉及支持比较值的仪表元素。 仪表的颜色由差异整数的符号和值方向属性设置确定。  
  
### <a name="set-range-stops-for-a-gauge"></a>设置仪表的区域停止点
第二项特定于仪表的非数据属性为数据区域停止点。 

* 选择仪表，然后在“布局”选项卡的“视觉对象属性”窗格中，选择“区域停止点”    。

利用区域停止点，可设置应该按比较值的多少百分比将可视化效果呈现为符合目标（绿色）、不确定（琥珀色）或脱离目标（红色）— 仪表的比较值为目标。 同样，只有具有比较值的仪表才支持区域停止点。  

### <a name="format-the-numbers-in-the-gauge"></a>格式化仪表中的数字  
仪表元素的另一非数据属性（也是供许多其他元素共享的属性）为数字格式。 

* 选择仪表，然后在“布局”选项卡的“视觉对象属性”窗格中，选择“区域停止点”    。

它决定了如何格式化仪表中显示的数字，例如，货币、百分比、时间或常规。 可对移动报表的每个元素设置数字格式。
  
### <a name="see-also"></a>另请参阅 

* [使用 SQL Server 移动报表发布服务器创建移动报表](../../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md)
* [Reporting Services 移动报表中的地图](../../reporting-services/mobile-reports/maps-in-reporting-services-mobile-reports.md)
* [Reporting Services 移动报表中的导航器](../../reporting-services/mobile-reports/add-navigators-to-reporting-services-mobile-reports.md)
* [Reporting Services 移动报表中的可视化效果](../../reporting-services/mobile-reports/add-visualizations-to-reporting-services-mobile-reports.md)
* [Reporting Services 移动报表中的数据网格](../../reporting-services/mobile-reports/add-data-grids-to-mobile-reports-reporting-services.md) 
