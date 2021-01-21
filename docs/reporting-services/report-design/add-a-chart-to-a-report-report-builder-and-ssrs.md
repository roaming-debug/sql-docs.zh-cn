---
title: 向报表添加图表（报表生成器）|Microsoft Docs
description: 了解在报表生成器中以可视化形式汇总数据时如何向分页报表添加图表。
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: a6b595dc-f775-4a53-8554-74a0bf9335ec
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 075dac12d1a4b5e393ead71da7bbf297fe40ae69
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596771"
---
# <a name="add-a-chart-to-a-report-report-builder-and-ssrs"></a>向报表添加图表（报表生成器和 SSRS）
  如果要在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 分页报表中以可视化格式汇总数据，请使用图表数据区域。 为您要呈现的数据类型选择一种适当的图表类型非常重要。 这将影响数据以图表形式呈现时对数据进行解释的好坏程度。 有关详细信息，请参阅 [图表（报表生成器和 SSRS）](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)。  
  
 将图表数据区域添加到报表中的最简单方式是运行新建图表向导。 该向导提供柱形图、折线图、饼图、条形图和面积图。 对于这些图表类型和其他图表类型，还可以手动添加图表。  
  
 将图表数据区域添加到设计图面后，可以将数值数据和非数值数据的报表数据集字段拖到图表的“图表数据”窗格中。 单击图表以显示“图表数据”窗格及其三个区域：“序列组”、“类别组”和“值”。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-add-a-chart-to-a-report-by-using-the-chart-wizard"></a>通过使用图表向导将图表添加到报表  
  
1.  > [!NOTE]  
    >  该图表向导仅在报表生成器中提供。  
  
     在 **“插入”** 选项卡上，单击 **“图表”** ，然后单击 **“图表向导”** 。  
  
2.  执行 **“新建图表”** 向导中的步骤。  
  
3.  在 **“主文件夹”** 选项卡上，单击 **“运行”** ，以查看所呈现的报表。  
  
4.  在 **“运行”** 选项卡上，单击 **“设计”** 以继续处理报表。  
  
## <a name="to-add-a-chart-to-a-report"></a>向报表添加图表  
  
1.  创建一个报表，并定义一个数据集。 有关详细信息，请参阅[报表数据集 (SSRS)](../../reporting-services/report-data/report-datasets-ssrs.md)。  
  
2.  在 **“插入”** 选项卡上，单击 **“图表”** ，然后单击 **“插入图表”** 。  
  
3.  在设计图面上，单击希望图表左上角所处的位置，并拖动至希望图表右下角所处的位置。  
  
     将出现 **“选择图表类型”** 对话框。  
  
4.  选择要添加的图表类型。 [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
5.  单击图表以显示 **“图表数据”** 窗格。  
  
6.  将一个或多个字段添加到 **“值”** 区域。 此信息将绘制在值轴上。  
  
7.  向 **“类别组”** 区域添加分组字段。 将此字段添加到 **“类别组”** 区域时，将自动创建一个分组字段。 每个组都表示序列中的一个数据点。  
  
8.  若要按类别汇总数据，请右键单击数据字段，然后单击“序列属性”。 在“类别”框中，从下拉列表中选择类别字段。 [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
9. 在 **“主文件夹”** 选项卡上，单击 **“运行”** ，以查看所呈现的报表。  
  
10. 在 **“运行”** 选项卡上，单击 **“设计”** 以继续处理报表。  
  
 在具有轴的图表（如条形图和柱形图）中，类别轴可能不会显示所有的类别标签。 有关如何更改轴标签的详细信息，请参阅[指定轴间隔（报表生成器和 SSRS）](../../reporting-services/report-design/specify-an-axis-interval-report-builder-and-ssrs.md)。  
  
## <a name="see-also"></a>另请参阅  
 [图表（报表生成器和 SSRS）](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [图表类型（报表生成器和 SSRS）](../../reporting-services/report-design/chart-types-report-builder-and-ssrs.md)   
 [图表中的空点和 Null 数据点（报表生成器和 SSRS）](../../reporting-services/report-design/empty-and-null-data-points-in-charts-report-builder-and-ssrs.md)   
 [教程：向报表添加条形图（报表生成器）](../tutorial-add-a-bar-chart-to-your-report-report-builder.md)   
 [教程：向报表添加条形图（报表设计器）](/previous-versions/sql/sql-server-2008-r2/cc281302(v=sql.105))   
 [教程：向报表添加饼图（报表生成器）](../tutorial-add-a-pie-chart-to-your-report-report-builder.md)   
 [教程：向报表添加饼图（报表设计器）](/previous-versions/sql/sql-server-2008-r2/cc281304(v=sql.105))  
  
