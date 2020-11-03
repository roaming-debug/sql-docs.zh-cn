---
title: 向 Reporting Services 移动报表添加可视化效果 | Microsoft Docs
description: 了解 Reporting Services 移动报表中可以使用的三种基本图表类型：时间、类别和总计，以及它们相应的比较图表。
ms.date: 09/26/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: 3b220b74-9ecd-4084-93fb-545208d5d7a2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: ed45f77f509d3206340a2f52e6d3b31ac6824d87
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907185"
---
# <a name="add-visualizations-to-reporting-services-mobile-reports"></a>向 Reporting Services 移动报表添加可视化效果
图表是数据可视化效果必不可少的一部分。 了解可在 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 移动报表中使用的图表，这些图表可涵盖一系列方案。 

[!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-long.md)] 有三种基本图表类型：时间、类别和总计。 这三种图表类型具有相应的比较图表，可用于比较两组不同的系列。  

## <a name="shared-chart-properties"></a>共享图表属性

某些属性适用于所有图表，而另一些属性则仅适用于特定的图表。 以下是一些共享属性。

### <a name="number-format"></a>数字格式
你可以向 [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] 中的图表中的数字分配各种格式，例如，常规、带小数或不带小数的货币、带小数和不带小数的百分比等等。 在图表中，数字格式应用于轴批注以及数据点弹出窗口。 应对每个图表单独设置数字格式，而不是在移动报表上作为一个整体来设置。 

* 若要设置数字格式，请选择“布局”选项卡，在设计图面上选择图表，然后在“视觉对象属性”窗格中选择“数字格式”    。 
  
### <a name="legend"></a>图例
* 若要显示图表的图例，请选择“布局”选项卡，在设计图面上选择图表，然后在“视觉对象属性”窗格中将“显示图例”设置为“打开”    
  
### <a name="series"></a>系列
显示在图表中的各个值或度量值称为一个系列；多个系列可以共享（并且确实在共享）同一 X 轴和同一 Y 轴。 选择一个或多个数据表和字段即可在数据视图的数据属性面板中定义系列。 每个字段都会在图表可视化效果中生成单个系列的数据点，并呈现出自己的颜色。  

### <a name="change-aggregation"></a>更改聚合 
对于图表中的数值字段，默认聚合为总和。 你可以将其更改为平均值、计数、最小值、最大值、第一个或最后一个。

* 选择“数据”选项卡，然后在“数据属性”中，选择数值字段旁边的“选项”> 选择其他聚合    。

### <a name="set-or-clear-filters"></a>设置或清除筛选器

如果添加导航器来筛选移动报表，可以决定希望其筛选哪些图表。

1. 选择“数据”选项卡，然后在“数据属性”中选择“选项”    。

2. 在“筛选依据”下面，会显示可以选择或清除的导航器  。

了解有关 [添加导航器以筛选移动报表](../../reporting-services/mobile-reports/add-navigators-to-reporting-services-mobile-reports.md)的详细信息。
   
## <a name="time-charts"></a>时间表  
  
时间表是 [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)]中最基本的图表。 图表的时间（和日期）轴将自动设置为数据表中第一个有效的日期/时间字段。  

![移动报表时间表的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-time-chart.png)

1. 从“布局”选项卡将“时间表”拖至设计图面并重设其大小   。

2. 默认情况下，它是堆积条形图。 可以在“系列可视化”中进行更改  。

3. 如果报表中没有图表需要的数据，请选择“数据”选项卡 >“添加数据”以[从 Excel 或共享数据集获取数据](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)   。

3. 在“数据属性”窗格中，“主系列”是“SimulatedTable”    。 选择框中的箭头 > 选择你的表。

5. 如果将“数据结构”设置为“按列”（在“布局”选项卡 >“视觉对象属性”窗格中），那么，在此处的“数据属性”窗格中可以选择多个数值列      。

   如果将“数据结构”设置为“按行”，那么，在此处的“数据属性”窗格中可以选择一个“系列名称字段”和一个数值列     。
   
了解有关 [按列或行对数据分组](../../reporting-services/mobile-reports/group-data-by-columns-or-rows-in-a-mobile-report-reporting-services.md)的详细信息。
  
## <a name="category-charts"></a>类别图表  
  
与时间表不同，在类别图表中，你将在 x 轴上按日期/时间字段以外的字段分组。 此分组称为 *类别坐标* ，必须按字符串而不是数值字段分组。

![移动报表类别表的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-category-chart.png)   

1. 从“布局”选项卡将“类别图表”拖至设计图面、重设其大小，并[为其获取数据](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)（如有必要）   。

2. 选择“数据”选项卡，并在“数据属性”窗格的“类别坐标”下选择表和要作为分组依据的字段    。 此字段将位于所生成图表的 x 轴上。

3. 在“主系列”下面，选择表以及要为每个类别聚合的数值字段  。 
  
## <a name="totals-charts"></a>总计图表  

![移动报表总计表的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-totals-chart.png)
  
总计图表可实现两个不同的目的： 
* 它不显示多个系列，而只显示所定义的主系列的总和或总计。 
* 它允许按列或行对数据分组。 在处理平展数据时，按列分组可能非常有用。 按列分组时，仅主系列属性可用，因为类别列是按为主系列属性选择的字段数自动确定的。  

了解有关 [按列或行对数据分组](../../reporting-services/mobile-reports/group-data-by-columns-or-rows-in-a-mobile-report-reporting-services.md)的详细信息。 
  
## <a name="comparison-charts"></a>比较图  
  
时间表、类别图表和总计图表也可用作 *比较图* 。 在比较图中，不仅可以指定主系列，还可以另外指定一个比较系列。 主系列和比较系列可以用三种不同的方式显示。

![移动报表比较时间表的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-comparison-time-chart.png)

1. 从“布局”选项卡将其中一个“比较图”（时间、类别或总计）拖至设计图面、重设其大小，并[为其获取数据](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)（如有必要）   。

2. 在“视觉对象属性”窗格的“系列可视化”中，选择以下选项之一   ： 
   * 条形图与细条形图
   * 折线图与条形图
   * 条形图与分段面积图 

在比较图中，可以选择对某个系列中的主值和比较值使用相同的图表颜色。

* 在“视觉对象属性”窗格中，将“在比较系列上重复使用颜色”设置为“打开”    。

   如果设置为“打开”，调色板将在绘制主系列与比较系列之间重启，因此，主系列和比较系列中的相关值是相同的  。 

   如果设置为“关闭”，则在绘制比较系列之后又绘制主系列时，调色板会继续按正常方式轮换使用其颜色，避免在两种系列的颜色之间造成可能出现的误导性颜色协调错误  。  
  
## <a name="pie-and-funnel-charts"></a>饼图和漏斗图  
  
饼图和漏斗图带给你最简单的可视化效果。 你可以按行或按列构造数据。 
* **移动报表中的** 饼图 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 可以是饼形、环形或中间为总计的环形。 饼图可用于显示一个整体的不同部分的相对大小。 切片过多的话会难以理解。
* **漏斗图** 通常用于显示某个过程的各个阶段，例如销售。

![移动报表漏斗图的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-funnel-chart.png)

### <a name="structure-pie-and-funnel-chart-data-by-rows-or-by-columns"></a>按行或按列构造饼图和漏斗图数据
1. 从“布局”选项卡将“饼图”或“漏斗图”拖至设计图面、重设其大小，并[为其获取数据](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)（如有必要）    。
2. 在“视觉对象属性”  窗格的“数据结构”  下面，选择以下选项之一：
   * **按列**
   * **按行**
3. 如果选择了“按列”  ，则选择“数据”  选项卡，并在“数据属性”  窗格的“主系列”  下面，选择表以及需要在饼图或漏斗图中聚合的所有字段。 将使用字段名称来标记所生成图表的每个区域。

   如果选择了“按行”，则选择“数据”选项卡，并在“数据属性”窗格的“类别列”下，选择表以及包含饼图中用于分组的值和标签的列     。 在“主系列”列下面，为图表中的值选择一个数值字段。

了解有关 [按列或行对数据分组](../../reporting-services/mobile-reports/group-data-by-columns-or-rows-in-a-mobile-report-reporting-services.md)的详细信息。 

## <a name="treemaps"></a>树状图  
  
树状图在显示指标时，会将其值应用于矩形网格中图块的大小和颜色。 

![移动报表组树状图的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-group-treemap.png)

1. 从“布局”选项卡将“树状图”拖至设计图面、重设其大小，并[为其获取数据](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)（如有必要）   。
2.  选择“数据”选项卡，然后在“数据属性”   窗格中： 

     * 在“大小表示”下，为图块大小选择一个数值字段  。
     * 在“颜色表示”  下面为图块颜色选择一个数值字段。 
     * [可选] **自定义中心值** ：如果可视化类型为 HeatMapWithCustomCenterValue，只能使用自定义中心值  。
     
         中间值用于决定框的颜色。 与中间值相比，指标越好，颜色越绿。 指标越差，颜色越红。
     
     * [可选]若要在查看器选择网格中的某个图块时显示弹出窗口，请在“弹出式标签”  下面选择一个或多个字段。 树状图弹出窗口可以同时显示文本和数值字段。  

默认情况下，树状图采用分层结构，先按类别，然后按大小和颜色对图块分组。
* 仍在“数据”选项卡上，在“分组依据”下选择表和字段   。

可以关闭分组，以便仅按大小和颜色排列图块。 

* 择“布局”  选项卡并将“两级树状图”  设置为“关闭”  。   

## <a name="waterfall-charts"></a>瀑布图

瀑布图显示在值增加或减少时的动态总值。 它可用于理解一系列积极和消极变化如何影响初始值（例如，净收益）。

颜色编码为绿色的列表示增加，为红色的列表示减少，通过这些列，可以快速理解增减情况。 初始值和最终值列通常从零开始，而中间值则是浮动列。 从外观上而言，瀑布图也称为网图。

### <a name="when-to-use-a-waterfall-chart"></a>什么情况下使用瀑布图

在以下情况下，瀑布图是不错的选择：
* 跨时序或不同的类别更改度量值以审核影响总值的主要变化。
* 通过显示各种收入源并得出总利润（或亏损）绘制公司的年利润。
* 显示一年开始和结束时公司的员工总数。
* 直观呈现每月的收支情况以及帐户的动态平衡。 

### <a name="create-a-waterfall-chart"></a>创建瀑布图

1. 从“布局”选项卡将“瀑布图”拖至设计图面、重设其大小，并[为其获取数据](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)（如有必要）   。

    ![移动报表瀑布图图标的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-waterfall-chart-icon.png)
    
2.  选择“数据”选项卡，然后在“数据属性”窗格中，为“类别坐标”选择一个数据中的类别字段，为“主系列”选择一个数值字段     ： 

    ![移动报表瀑布数据的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-waterfall-data.png)
    
3. 选择“布局”选项卡以查看预览瀑布图  。

   ![移动报表瀑布图的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-waterfall-chart.png)
   
   亏损月（如 2 月、6 月和 7 月）呈现红色。 
   盈利月（如 9 月、10 月和 11 月）呈现绿色。 

## <a name="see-also"></a>另请参阅 
* [Reporting Services 移动报表中的地图](../../reporting-services/mobile-reports/maps-in-reporting-services-mobile-reports.md)
* [Reporting Services 移动报表中的导航器](../../reporting-services/mobile-reports/add-navigators-to-reporting-services-mobile-reports.md)
* [Reporting Services 移动报表中的数据网格](../../reporting-services/mobile-reports/add-data-grids-to-mobile-reports-reporting-services.md)
* [Reporting Services 移动报表中的仪表](../../reporting-services/mobile-reports/add-gauges-to-mobile-reports-reporting-services.md)
  

