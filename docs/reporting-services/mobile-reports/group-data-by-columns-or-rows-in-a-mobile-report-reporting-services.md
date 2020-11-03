---
title: 在移动报表中按列或行对数据进行分组 | Reporting Service | Microsoft Docs
description: 在移动报表发布服务器中，可以按多种类型的图表中的列或行来组织数据。 本文介绍按列或按行构造的数据。
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: b9ebd36c-a337-47ae-83e5-6c2f2144eb52
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d775b0346ce2838abeec4bebce55762afd3b0adc
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907325"
---
# <a name="group-data-by-columns-or-rows-in-a-mobile-report--reporting-services"></a>在移动报表中按列或行对数据进行分组 | Reporting Service
在 [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)]中的许多类型的图表中，可以按列或按行来组织数据。 按照此分步说明执行操作。

在时间图、总计图、饼图和漏斗图，可以按行或按列来组织数据。 
* 如果表包含几列要进行比较的数据，则按列进行组织会很有用。 
* 如果表中的一列包含不同类别的名称，则按行进行组织会效果更好。 

以下步骤在 [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] 中使用具有模拟数据的比较总计表来演示在图表中按行或按列来构建数据结构之间的差异。  

1. 将“比较总计图表”从“布局”选项卡拖动到设计图面并扩大它   。

2. 选择“数据”选项卡  。你会看到 SimulatedTable 表包含一系列的列，即“Metric1”到“Metric5”，以及“Comparison1”到“Comparison5”     。 

   ![移动报表数据组列的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-data-group-column.png)

3. 在“数据属性”窗格中，“主系列”是“SimulatedTable”    。 选择“主系列”旁的框中的箭头，会看到“Metric1”到“Metric5”处于选中状态    。

   ![主系列旁选项的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-properties-columns.png)

   “比较系列”也是同样情况   -- **Comparison1** 到 **Comparison5** 处于选择状态。
   
4. 选择“预览”  。

   ![比较总计图表预览视图的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-chart-by-columns.png)

   图表中的每条都表示表中的一列。 较粗条是“度量”列，较细条是“比较”列。

5. 选择左上角的向后箭头以离开预览模式。

6. 在“布局”选项卡上的“视觉属性”窗格中，将“数据结构”从“按列”更改为“按行”      。  

7. 选择“数据”选项卡  。现在 SimulatedTable 表具有一个“类别”列以及“指标”和“比较”列（类别 A 到 E）    。 

   ![移动报表数据组行的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-data-group-rows.png)

8.  在“数据属性”窗格中，现在有一个“类别列”框，其中列出了 SimulatedTable 中的“类别”列  。 在“主系列”中，可以选取要用于值的列。 默认情况下， [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] 会为“主系列”选择 Metric5 到 Metric1，为“比较系列”选择 Comparison1 到 Comparison5。 

    ![比较系列旁选项的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-properties-rows.png)

9. 选择“预览”  。

   ![更新后比较总计图表预览视图的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-chart-by-rows.png)

   现在图表中的每条都表示“类别”列中每种类别的值。

### <a name="see-also"></a>另请参阅
* [Reporting Services 移动报表中的可视化效果](../../reporting-services/mobile-reports/add-visualizations-to-reporting-services-mobile-reports.md)
