---
title: 图表中的空和 Null 数据点（报表生成器）| Microsoft Docs
description: 了解报表生成器中图表处理的空值、占位符的用法，以及空数据点和 null 数据点的显示方式。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: faddd29d-4cc1-4c2c-8e29-d3d9918fe22a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4ed8a71adea4b57d96137cc94168ca1810956bdd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073781"
---
# <a name="empty-and-null-data-points-in-charts-report-builder-and-ssrs"></a>图表中的空和 Null 数据点（报表生成器和 SSRS）

  如果在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 分页报表的图表中显示具有空值或 Null 值的字段，则图表外观可能与预期不符。 图表处理空值的方式会根据指定图表类型而改变：  
  
-   如果图表类型是线性图表类型（条形图、柱形图、散点图、折线图、面积图和范围图），则空值将在该图表中显示为空格或“空白”。 若要指示空点，则必须添加空点占位符。 有关详细信息，请参阅[向图表添加空点（报表生成器和 SSRS）](../../reporting-services/report-design/add-empty-points-to-a-chart-report-builder-and-ssrs.md)。  
  
-   如果图表类型是连续线性图表类型（面积图、条形图、柱形图、折线图和散点图），则空数据点将添加到该图表中以维护序列的连续性。  
  
-   如果图表类型是非线性图表类型（极坐标图、饼图、圆环图、漏斗图或棱锥图），则空值将不显示在该图表中。  
  
-   在形状图表类型中，会省略 Null 值。  
  
 具有空数据点的图表的示例可用于示例报表。 有关下载此示例报表和其他内容的详细信息，请参阅 [报表生成器和报表设计器示例报表](https://go.microsoft.com/fwlink/?LinkId=198283)。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="removing-empty-or-null-values"></a>删除空值或 Null 值  
 若要避免重要数据变得模糊，请考虑从数据集中删除空值。 若要筛选 Null 值，可以在查询中使用 NOT IS NULL 子句。 或者，也可以添加指定仅显示非 0 值的筛选表达式。 有关详细信息，请参阅 [添加数据集筛选器、数据区域筛选器和组筛选器（报表生成器和 SSRS）](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)。  
  
## <a name="fields-with-no-values-in-a-chart"></a>在图表中不含值的字段  
 如果在返回的结果集中某个字段不包含任何值，则图表会显示一个没有任何数据点的空图表，但是序列名称（通常是字段名称）会添加为图例项。  
  
 此行为不同于当报表已参数化并且所选值返回空结果集时，返回的数据集可能包含 0 行数据的情况。 如果数据集查询返回 0 行数据，在运行时系统会显示一条消息，指示没有可显示的数据。 通过在“属性”窗格中修改该报表的 NoDataMessage 标题可以自定义此消息。 有关详细信息，请参阅 [报表的嵌入数据集和共享数据集（报表生成器和 SSRS）](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)。  

## <a name="next-steps"></a>后续步骤

[图表](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
[设置图表的格式](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
[向报表添加图表](../../reporting-services/report-design/add-a-chart-to-a-report-report-builder-and-ssrs.md)   
[图表疑难解答](../../reporting-services/report-design/troubleshoot-charts-report-builder-and-ssrs.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
