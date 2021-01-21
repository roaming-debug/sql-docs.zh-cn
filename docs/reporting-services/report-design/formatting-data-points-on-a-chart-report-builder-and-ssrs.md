---
title: 设置图表上数据点的格式（报表生成器）| Microsoft Docs
description: 了解报表生成器中报表的图表上不同类型的数据点格式。
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- "10248"
- sql13.rtp.rptdesigner.serieslabelproperties.general.f1
ms.assetid: 08ec3818-f63a-4e89-b52c-750e47f48b85
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0d9f3036e7d8c580e770ded7bbbec6e5f29b2750
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596910"
---
# <a name="formatting-data-points-on-a-chart-report-builder-and-ssrs"></a>设置图表上数据点的格式（报表生成器和 SSRS）
在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 分页报表中，数据点是图表上的最小单个实体。 在非形状图上，根据数据点的图表类型来表示数据点。 例如，线条序列由一个或多个连接数据点组成。 在形状图上，数据点是通过构成整个图表的单个切片和段来表示的。 例如，饼图上的每个块都是一个数据点。 有关详细信息，请参阅 [图表类型（报表生成器和 SSRS）](../../reporting-services/report-design/chart-types-report-builder-and-ssrs.md)。  
  
 序列由一个或多个数据点构成。 默认情况下，所有格式设置选项应用于序列中的所有数据点。 如果要指定单个数据点的属性，则可以在序列上指定在运行时基于数据集来设置单个数据点的格式的字段或表达式。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="adding-tooltips-and-drillthrough-actions-to-data-points"></a>向数据点添加工具提示和钻取操作  
 通过在序列上设置 **“工具提示”** 属性的值，可以向每个数据点添加工具提示。 通过显示工具提示，您可以向用户授予查看与数据点有关的信息的功能，例如，组名、数据点的值和数据点相对于序列总计的百分比。 有关详细信息，请参阅 [在序列上显示工具提示（报表生成器和 SSRS）](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md)。  
  
 还可以为序列上的数据点指定钻取操作，以便显示其他报表或 URL。 可以传递参数来显示与已单击数据点相关的信息。 有关详细信息，请参阅[在报表中添加钻取操作（报表生成器和 SSRS）](../../reporting-services/report-design/add-a-drillthrough-action-on-a-report-report-builder-and-ssrs.md)。  
  
## <a name="highlighting-individual-data-points-in-a-series"></a>突出显示序列中的单个数据点  
 在任何非形状图上，通过指定“颜色”属性的表达式，可以突出显示单个数据点。 例如，若要用不同于其他数据点的颜色来突出显示序列中命名为 `MyField` 的最高数据点值，表达式则类似于以下内容：  
  
 `=Iif(Fields!MyField.Value >= Max(Fields!MyField.Value, "MyDataSet"), "Red", "Green")`  
  
 在本示例中， `MyField` 的最高值的颜色将为 Red，而所有其他数据点的颜色将为 Green。 当在序列上使用 **“填充”** 属性来指定该序列的颜色时，图表将覆盖在调色板中指定的颜色。 有关详细信息，请参阅 [设置图表上序列颜色的格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-series-colors-on-a-chart-report-builder-and-ssrs.md)。  
  
## <a name="positioning-data-point-labels-on-a-chart"></a>在图表上定位数据点标签  
 对于所有图表类型，右键单击图表并选择 **“显示数据标签”** 后可以显示数据点标签。 数据点标签的位置是根据图表类型指定的：  
  
-   在条形图中，可以使用 **BarLabelStyle** 自定义属性重新定位数据点标签。 有四个可能位置：Outside、Left、Center 和 Right。 当条形标签样式设置为“外部”时，只要图表区放得下，标签就将定位到图条的外部。 如果标签在图表区内图条以外的区域放不下，则标签将定位到图条内。  
  
-   在饼图中，可以使用 **PieLabelStyle** 自定义属性重新定位数据点标签。 围绕饼图定位数据点标签时应考虑诸多注意事项，包括饼图的大小、饼图及其相应图例之间的可用空间和标签大小。 有关详细信息，请参阅 [在饼图外显示数据点标签（报表生成器和 SSRS）](../../reporting-services/report-design/display-data-point-labels-outside-a-pie-chart-report-builder-and-ssrs.md)。  
  
-   在棱锥图或漏斗图中，可以使用 **FunnelLabelStyle** 和 **PyramidLabelStyle** 自定义属性重新定位数据点标签。 在选择棱锥图或漏斗图类型后，可以在 **“属性”** 窗格中设置这些属性。  
  
-   在堆积图中，数据点标签始终位于序列内，并忽略序列标签上的 **“位置”** 属性。  
  
-   在所有其他图表类型中，可以使用序列标签上的 **“位置”** 属性来重新定位数据点标签。 默认情况下，图表自动计算数据点标签的位置，以便避免标签冲突。 设置 **“位置”** 的值之后，所有数据点标签将按照相同方式定位，这可能导致标签重叠。 仅当具有较少的数据点时，请考虑使用该方法。  
  
 有关详细信息，请参阅[图表中的位置标签（报表生成器和 SSRS）](../../reporting-services/report-design/position-labels-in-a-chart-report-builder-and-ssrs.md)。  
  
## <a name="adding-keywords-for-data-point-labels-tooltips-and-legend-text"></a>为数据点标签、工具提示和图例文本添加关键字  
 可以使用区分大小写的特定于图表的关键字来表示图表中的存在项。 这些关键字仅适用于工具提示、自定义图例文本和数据点标签属性。 在多数情况中，图表关键字具有等效的简单表达式，但关键字更快且更容易键入。 下面列出了图表关键字。  
  
|图表关键字|说明|适用的图表类型|等效的简单表达式的示例|  
|-------------------|-----------------|------------------------------|------------------------------------------------|  
|#VALY|数据点的 Y 值。|All|`=Fields!MyDataField.Value`|  
|#VALY2|数据点的 Y 值 #2。|范围图、气泡图|无|  
|#VALY3|数据点的 Y 值 #3。|股价图、K 线图|无|  
|#VALY4|数据点的 Y 值 #4。|股价图、K 线图|无|  
|#SERIESNAME|序列名称。|All|无|  
|#LABEL|数据点标签。|All|无|  
|#AXISLABEL|轴数据点标签。|形状|`=Fields!MyDataField.Value`|  
|#INDEX|数据点索引。|All|无|  
|#PERCENT|数据点 Y 值的百分比。|All|`=FormatPercent(Fields!MyDataField.Value/Sum(Fields!MyDataField.Value, "MyDataSet"),2)`|  
|#TOTAL|序列中所有 Y 值的总计。|All|`=Sum(Fields!MyDataField.Value)`|  
|#LEGENDTEXT|对应于图例项文本的文本。|All|无|  
|#AVG|序列中所有 Y 值的平均值。|All|`=Avg(Fields!MyDataField.Value)`|  
|#MIN|序列中所有 Y 值的最小值。|All|`=Min(Fields!MyDataField.Value)`|  
|#MAX|序列中所有 Y 值的最大值。|All|`=Max(Fields!MyDataField.Value)`|  
|#FIRST|序列中所有 Y 值第一个值。|All|`=First(Fields!MyDataField.Value)`|  
  
 若要设置关键字格式，请将 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 格式字符串用括号括起来。 例如，若要在工具提示中将数据点的值指定为具有两个小数位数的数字，请将格式字符串“N2”放置到大括号中，例如序列上 **“工具提示”** 属性的“#VALY{N2}”。 有关 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 格式字符串的详细信息，请参阅 MSDN 上的 [Formatting Types](/previous-versions/) （为类型设置格式）。 有关在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中设置数字格式的详细信息，请参阅[设置数字和日期格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-numbers-and-dates-report-builder-and-ssrs.md)。  
  
 有关将关键字添加到图表的详细信息，请参阅[在序列上显示工具提示（报表生成器和 SSRS）](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md)、[更改图例项的文本（报表生成器和 SSRS）](../../reporting-services/report-design/chart-legend-change-item-text-report-builder.md)。  
  
## <a name="increasing-readability-in-a-chart-with-multiple-data-points"></a>提高具有多个数据点的图表的可读性  
 如果图表中具有多个序列，则可能降低图表数据点的可读性。 向图表添加多个序列时，请考虑使用区分如何有效读取和了解图表中的每个序列的方法。 有关详细信息，请参阅 [图表中的多个序列（报表生成器和 SSRS）](../../reporting-services/report-design/multiple-series-on-a-chart-report-builder-and-ssrs.md)。  
  
 为方便起见，当使用形状图时，请考虑仅添加一个数据字段和一个类别字段。 有关详细信息，请参阅[形状图（报表生成器和 SSRS）](../../reporting-services/report-design/shape-charts-report-builder-and-ssrs.md)。 如果图表需要多个数据字段和类别字段，请考虑更改图表类型。 您可以右键单击该序列，然后选择 **“更改图表类型”** 。  
  
## <a name="inserting-data-point-markers"></a>插入数据点标记  
 数据点标记是用于突出序列中的每个数据点的可视指示器。 在散点图中，该标记用于确定每个数据点的形状和大小。 标记的大小是根据图表类型指定的。 可以更改标记的大小、颜色或样式。 标记不适用于范围和形状图表类型，或任何堆积子类型。  
  
## <a name="in-this-section"></a>本节内容  
 [在序列上显示工具提示（报表生成器和 SSRS）](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md)  
  
 [在饼图外显示数据点标签（报表生成器和 SSRS）](../../reporting-services/report-design/display-data-point-labels-outside-a-pie-chart-report-builder-and-ssrs.md)  
  
 [在饼图上显示百分比值（报表生成器和 SSRS）](../../reporting-services/report-design/display-percentage-values-on-a-pie-chart-report-builder-and-ssrs.md)  
  
## <a name="see-also"></a>另请参阅  
 [设置图表格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [设置图表上轴标签的格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md)   
 [图表（报表生成器和 SSRS）](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [将轴标签的格式设置为日期或货币（报表生成器和 SSRS）](../../reporting-services/report-design/format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs.md)   
 [教程：向报表添加饼图（报表生成器）](../../reporting-services/tutorial-add-a-pie-chart-to-your-report-report-builder.md)   
 [表达式示例（报表生成器和 SSRS）](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
