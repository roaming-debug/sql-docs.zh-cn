---
title: 教程：向报表添加饼图（报表生成器）| Microsoft Docs
description: 了解如何在 Reporting Services 分页报表中创建饼图、添加百分比并将若干小切片合并为一个切片。
ms.date: 06/15/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: eaadf7bf-c312-428a-b214-0a1fbf959c3f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a924e8e6ed7a19904ddc9c8cded38683b849edeb
ms.sourcegitcommit: 9e2c682929ee64c051dc62f8917d147861f7c635
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043744"
---
# <a name="tutorial-add-a-pie-chart-to-your-report-report-builder"></a>教程：向报表添加饼图（报表生成器）
在本教程中，将在 Reporting Services 分页报表中创建饼图。 添加百分比，并将小切片合并为一个切片。

饼图和圆环图将数据显示为整体的一定比例。 它们没有轴。 在饼图上添加某数值字段后，该图表将计算每个值相对总计的百分比。  

此图显示了将创建的饼图。 
 
![报表生成器饼图的屏幕截图。](../reporting-services/media/report-builder-pie-chart-final.png)
  
如果饼图上有太多数据点，这些数据点就可能挤在一起，这会降低图表的可读性。 在这种情况下，请考虑将多个小切片组合成一个更大的切片。 将数据聚合到少量数据点之后，饼图更具可读性。  
 
> [!NOTE]  
> 在本教程中，将向导的多个步骤合并为两个过程。 有关如何浏览到报表服务器、添加数据源和添加数据集的分步说明，请参阅这一系列教程中的第一个教程：[教程：创建基本表报表（报表生成器）](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md)。  
  
完成本教程的估计时间：10 分钟  
  
## <a name="requirements"></a>要求  
有关要求的信息，请参阅[教程先决条件（报表生成器）](../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
## <a name="1-create-a-pie-chart-from-the-chart-wizard"></a><a name="Chart"></a>1.使用图表向导创建饼图  
本部分将介绍如何使用“图表向导”创建嵌入数据集、选择共享数据源并创建饼图。  

  
1.  通过计算机、[Web 门户或 SharePoint 集成模式](../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 。  
  
    将打开“新建报表或数据集”对话框。  
  
    如果未出现“新建报表或数据集”对话框，请通过“文件”菜单转至“新建”  。  
  
2.  在左窗格中，确认已选中 **“新建报表”** 。  
  
3.  在右窗格中，单击“图表向导”。  
  
4.  在“选择数据集”页上，单击“创建数据集”，然后单击“下一步”  。  
  
5.  在“选择数据源的连接”页中，选择现有数据源或浏览到报表服务器并选择一个数据源，然后单击“下一步”。 您可能需要输入用户名和密码。  
  
    > [!NOTE]  
    > 只要您具有足够的权限，则选择哪一个数据源并不重要。 您将不会从数据源中获取数据。 有关详细信息，请参阅[获取数据连接的备选方式（报表生成器）](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md)。  
  
6.  在 **“设计查询”** 页中，单击 **“编辑为文本”** 。  
  
7.  将以下查询粘贴到查询窗格中：  

    > [!NOTE]  
    > 在本教程中，由于查询包含了数据值，因此它不需要外部数据源。 这样，查询就会很长。 在业务环境中，查询不会包含数据。 本教程中的查询仅供学习使用。  
  
    ```  
    SELECT 'Advanced Digital Camera' AS Product, CAST(254995.21 AS money) AS Sales  
    UNION SELECT 'Slim Digital Camera' AS Product, CAST(164499.04 AS money) AS Sales  
    UNION SELECT 'SLR Digital Camera' AS Product, CAST(782176.79 AS money) AS Sales  
    UNION SELECT 'Lens Adapter' AS Product, CAST(36333.08 AS money) AS Sales  
    UNION SELECT 'Macro Zoom Lens' AS Product, CAST(40199.3 AS money) AS Sales  
    UNION SELECT 'USB Cable' AS Product, CAST(53245.5 AS money) AS Sales  
    UNION SELECT 'Independent Filmmaker Camcorder' AS Product, CAST(452288.0 AS money) AS Sales  
    UNION SELECT 'Full Frame Digital Camera' AS Product, CAST(247250.85 AS money) AS Sales  
    ```  
  
8.  （可选）单击“运行”按钮 ( **!** )，查看要用于图表的数据。  
  
9. 单击“下一步”。  
  
## <a name="2-choose-the-chart-type"></a><a name="ChartType"></a>2.选择图表类型  
可以从各种预定义的图表类型中进行选择。  

  
1.  在“选择图表类型”页中，单击“饼图”，然后单击“下一步”。 将打开“排列图表字段”页。  
  
    在“排列图表字段”页上，将“Product”字段拖到“类别”窗格中。 类别定义了饼图上的切片数。 在本示例中，将有 8 个切片，每个产品对应一个切片。  
  
2.  将“Sales”字段拖到“值”窗格中。 Sales 表示子类别的销售量。 “值”窗格显示 `[Sum(Sales)]`，因为该图表显示的是每个产品的销售总额。  
  
3.  单击“下一步”查看预览。  
  
5.  单击“完成”。  
  
    图表将添加到设计图面中。 看不到饼图的实际值 -- 只能看到 Product 1、Product 2 等，以便了解该图表的显示外观。  
    
    ![设计视图下的报表生成器饼图的屏幕截图。](../reporting-services/media/report-builder-pie-chart-first-design.png)
  
6.  单击图表以显示图表控点。 拖动图表右下角将其放大。 请注意，为适应报表大小，报表设计图面也会变大。  
  
7.  单击 **“运行”** 以预览报表。  
  
报表会显示具有 8 个切片的饼图，每个产品对应一个切片。 现在可以看到实际产品和每个切片大小所表示的该产品的销售额。 其中有三个切片非常薄。  

![显示报表生成器饼图预览的屏幕截图。](../reporting-services/media/report-builder-pie-chart-first-preview.png)
  
## <a name="3-display-percentages-in-each-slice"></a><a name="Percentages"></a>3.在每个切片中显示百分比  
在饼图的每个切片上，可以显示此切片占整个饼图的百分比。  

  
1.  切换到报表设计视图。  
  
2.  右键单击饼图，然后单击“显示数据标签”。 数据标签会显示在图表上。  
  
3.  右键单击标签，然后单击“序列标签属性”。  
  
4.  在“标签数据”框中，选择“#PERCENT”。  
    
5.  （可选）若要指定标签显示的小数位数，在“标签数据”框的“#PERCENT”后面键入 **{Pn}** ，其中 n 为要显示的小数位数。 例如，若不显示小数位数，请键入 **#PERCENT{P0}** 。  

6.  若要将值显示为百分比，则 UseValueAsLabel 属性必须为 false。 如果系统提示设置此值，请在“确认操作”对话框中单击“是”。  
  
    > [!NOTE]  
    > 设置百分比格式时，“序列标签属性”对话框中的“数字格式”不起作用。 它将标签的格式设置为百分比，但不会计算每一切片占饼图的百分比。  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
7.  单击 **“运行”** 以预览报表。  
  
报表会显示每个饼图切片占总体的百分比。  

![显示报表生成器饼图预览的屏幕截图，其中每个切片中都显示了百分比。](../reporting-services/media/report-builder-pie-chart-preview-percents.png)
  
## <a name="4-combine-small-slices-into-one-slice"></a><a name="CombineSlices"></a>4.将多个小型切片合并为一个切片  
在饼图所包含的切片中，有三个切片非常小。 可以将多个小切片合并为一个表示所有三个小切片的大型“其他”切片。  

1.  切换到报表设计视图。  
  
2.  如果看不到“属性”窗格，请在“视图”选项卡的“显示/隐藏”组中选择“属性”。  
  
3.  在设计图面上，单击饼图的任一切片。 序列的属性将显示在“属性”窗格中。  
  
4.  在 **“常规”** 部分，展开 **CustomAttributes** 节点。  
  
5.  将“CollectedStyle”属性设置为“SingleSlice”。  

    ![显示如何在报表生成器饼图中设置单个切片的属性的屏幕截图。](../reporting-services/media/report-builder-pie-chart-single-slice-property.png)
 
6.  确保将“CollectedThreshold”属性设置为“5”。  
  
7.  确保将“CollectedThresholdUsePercent”属性设置为“True”。  
  
8.  在“主文件夹”选项卡上，单击“运行”以预览报表。  
  
现在，在图例中可看到类别“其他”。 新饼图切片将所有小于 5% 的切片组合成一个占整个饼图 6% 的切片。  

![显示报表生成器如何在与图表顶部成 90 度的位置开始绘制饼图的屏幕截图。](../reporting-services/media/report-builder-pie-chart-start-at-90.png)
 
## <a name="5-start-pie-chart-values-at-the-top"></a><a name="DrawingEffect"></a>5.从顶部开始绘制饼图值 

默认情况下，在饼图中，数据集中的第一个值从与饼顶部成 90 度的位置开始绘制。 可以在前几部分的饼图中看到。

在本部分中，将从顶部开始绘制第一个值。

1.  切换到报表设计视图。  

2. 选择饼图本身。

3. 在“属性”窗格中，“自定义属性”下，将“PieStartAngle”从“0”更改为“270”。

4. 单击 **“运行”** 以预览报表。

现在，饼图切片从顶部开始按字母顺序排列，“其他”切片位于末尾。

![显示如何从顶部开始绘制报表生成器饼图的屏幕截图。](../reporting-services/media/report-builder-pie-chart-start-at-top.png)
  
## <a name="6-add-a-report-title"></a><a name="Title"></a>6.添加报表标题  
  
由于饼图是报表中的唯一可视化效果，因此图表无需具有标题。 需要报表标题。
  
1.  在图表中，选择“图表标题”框，然后按 Delete。

2. 在设计图面上，单击“单击以添加标题”。  
  
2.  键入 **Camera and Camcorder Sales** ，按 Enter，然后键入 **As a Percentage of Total Sales** ，如下所示：  
  
    **Camera and Camcorder Sales**  
  
    **As a Percentage of Total Sales**  
  
3.  选择 **Camera and Camcorder Sales** ，然后在“开始”选项卡上的“字体”部分，单击“加粗”。  
  
4.  选择“As a Percentage of Total Sales”，然后在“开始”选项卡上的“字体”部分中，将字号设置为“10”。  
  
5.  （可选）您可能需要使“标题”文本框更高一些，以容纳两行文本。  
  
    此标题将显示在报表的顶部。 未定义页标头时，报表体顶部的项就等同于报表标头。  
  
6.  单击 **“运行”** 以预览报表。  
  
## <a name="7-save-the-report"></a><a name="Save"></a>7.保存报表  
  
### <a name="to-save-the-report"></a>保存报表  
  
1.  切换到报表设计视图。  
  
2.  在“文件”菜单上，单击“保存”。  
  
3.  在“名称”中，键入“Sales Pie Chart”。  
  
4.  单击“ **保存** ”。  
  
报表将保存在报表服务器上。  
  
## <a name="next-steps"></a>后续步骤  
这样，您就成功完成了“向报表添加饼图”教程的学习。 若要了解有关图表的详细信息，请参阅[图表（报表生成器和 SSRS）](../reporting-services/report-design/charts-report-builder-and-ssrs.md)和[迷你图和数据条（报表生成器和 SSRS）](../reporting-services/report-design/sparklines-and-data-bars-report-builder-and-ssrs.md)。  
  
## <a name="see-also"></a>另请参阅  
[报表生成器教程](../reporting-services/report-builder-tutorials.md)  
[SQL Server 中的报表生成器](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  

