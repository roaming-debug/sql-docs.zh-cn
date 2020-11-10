---
title: 教程：向报表添加柱形图（报表生成器）| Microsoft Docs
description: 了解如何创建 Reporting Services 分页报表，该报表包含一个柱形图，其中将序列显示为一组按类别分组的垂直条。
ms.date: 09/02/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 63480059-b7b9-44b5-9d7f-91780db708b6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 23796da12c2b987dfad6f023d40e9975cb301e28
ms.sourcegitcommit: 9e2c682929ee64c051dc62f8917d147861f7c635
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043773"
---
# <a name="tutorial-add-a-column-chart-to-your-report-report-builder"></a>教程：向报表添加柱形图（报表生成器）
本教程中将创建 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 分页报表，该报表包含一个柱形图，将序列显示为一组按类别分组的垂直条。 

柱形图对以下方面很有用：  
  
-   显示一段时间内数据的更改。  
-   比较多个序列的相对值。  
-   显示移动平均线，以显示趋势。  
  
下图显示了将创建的带有移动平均线的柱形图。  
  
![显示报表生成器柱形图的屏幕截图。](../reporting-services/media/report-builder-column-chart-tutorial.png)    
> [!NOTE]  
> 在本教程中，将向导的多个步骤合并为一个过程。 有关如何浏览到报表服务器、选择数据源和创建数据集的分步说明，请参阅本系列中的第一个教程：[教程：创建基本表报表（报表生成器）](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md)。  
  
完成本教程的估计时间：15 分钟。  
  
## <a name="requirements"></a>要求  
有关要求的信息，请参阅[教程先决条件（报表生成器）](../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
## <a name="1-create-a-chart-report-from-the-chart-wizard"></a><a name="Chart"></a>1.使用图表向导创建图表报表  
在本部分中，使用图表向导创建嵌入数据集、选择共享数据源并创建柱形图。  
  
> [!NOTE]  
> 在本教程中，由于查询包含了数据值，因此它不需要外部数据源。 这样，查询就会非常长。 在业务环境中，查询不会包含数据。 本教程中的查询仅供学习使用。  
  
### <a name="to-create-a-chart-report"></a>创建图表报表  
  
1.  通过计算机、[Web 门户或 SharePoint 集成模式](../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 。  
  
    将打开“新建报表或数据集”对话框。  
  
    如果未出现“新建报表或数据集”对话框，请通过“文件”菜单转至“新建”  。  
  
2.  在左窗格中，确认已选中 **“新建报表”** 。  
  
3.  在右窗格中，单击“图表向导”。  
  
4.  在“选择数据集”页上，单击“创建数据集”，然后单击“下一步”  。  
  
5.  在“选择数据源的连接”页中，选择现有数据源或浏览到报表服务器并选择一个数据源，然后单击“下一步”。 您可能需要输入用户名和密码。  
  
    > [!NOTE]  
    > 只要您具有足够的权限，则选择哪一个数据源并不重要。 您将不会从数据源中获取数据。 有关详细信息，请参阅[获取数据连接的备选方式（报表生成器）](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md)。  
  
6.  在“设计查询”页上，单击“编辑为文本” 。  
  
7.  将以下查询粘贴到查询窗格中：  
  
    ```  
    SELECT CAST('2015-01-01' AS date) AS SalesDate, CAST(54995.21 AS money) AS Sales  
    UNION SELECT CAST('2015-01-05' AS date) AS SalesDate, CAST(64499.04 AS money) AS Sales  
    UNION SELECT CAST('2015-02-11' AS date) AS SalesDate, CAST(37821.79 AS money) AS Sales  
    UNION SELECT CAST('2015-03-18' AS date) AS SalesDate, CAST(53633.08 AS money) AS Sales  
    UNION SELECT CAST('2015-04-23' AS date) AS SalesDate, CAST(24019.3 AS money) AS Sales  
    UNION SELECT CAST('2015-05-01' AS date) AS SalesDate, CAST(93245.5 AS money) AS Sales  
    UNION SELECT CAST('2015-06-06' AS date) AS SalesDate, CAST(55288.0 AS money) AS Sales  
    UNION SELECT CAST('2015-06-16' AS date) AS SalesDate, CAST(68733.5 AS money) AS Sales  
    UNION SELECT CAST('2015-07-16' AS date) AS SalesDate, CAST(24750.85 AS money) AS Sales  
    UNION SELECT CAST('2015-08-23' AS date) AS SalesDate, CAST(43452.3 AS money) AS Sales  
    UNION SELECT CAST('2015-09-24' AS date) AS SalesDate, CAST(58656. AS money) AS Sales  
    UNION SELECT CAST('2015-10-15' AS date) AS SalesDate, CAST(44583. AS money) AS Sales  
    UNION SELECT CAST('2015-11-21' AS date) AS SalesDate, CAST(81568. AS money) AS Sales  
    UNION SELECT CAST('2015-12-15' AS date) AS SalesDate, CAST(45973. AS money) AS Sales  
    UNION SELECT CAST('2015-12-26' AS date) AS SalesDate, CAST(96357. AS money) AS Sales  
    UNION SELECT CAST('2015-12-31' AS date) AS SalesDate, CAST(81946. AS money) AS Sales  
    ```  
  
8.  （可选）单击“运行”按钮 ( **!** )，查看要用于图表的数据。  
  
9. 单击“下一步”。  
  
## <a name="2-choose-the-chart-type"></a><a name="ChartType"></a>2.选择图表类型  
可从几种预定义的图表类型中进行选择，完成向导后可修改图表。  
  
### <a name="to-add-a-column-chart"></a>添加柱形图  
  
1.  在“选择图表类型”页上，柱形图为默认图表类型。 单击“下一步”。  
  
2.  在“排列图表字段”页上，将 SalesDate 字段拖到“类别”中 。 “类别”显示在水平轴上。  
  
3.  将 Sales 字段拖到“值”中。 “值”框显示 Sum(Sales)，因为销售总计值之和是对每个日期的合计。 “值”显示在垂直轴上。  
  
4.  单击“下一步”。  
 
6.  单击“完成”。  
  
    图表将添加到设计图面中。 请注意，新的柱形图只显示代表性数据。 图例标有 Sales Date A、Sales Date B 等，只为大概给出报表的外形。 
    
    ![屏幕截图：设计视图下的报表生成器柱形图。](../reporting-services/media/report-builder-column-chart-1-design-view.png)
  
7.  单击图表以显示图表控点。 拖动该图表的右下角以扩大该图表。 请注意，报表设计图面的大小将增大以容纳图表。  
  
8.  单击 **“运行”** 以预览报表。  

    ![显示报表生成器柱形图预览的屏幕截图。](../reporting-services/media/report-builder-column-chart-1-preview.png)

请注意，图表不会在水平轴上显示每个类别的标签。 默认情况下，仅包括适合放在轴旁边的标签。 
  
## <a name="3-format-a-date-on-the-horizontal-axis"></a><a name="Horizontal"></a>3.设置水平轴上的日期格式  
默认情况下，水平轴采用常用格式显示值，将自动调整为适合图表的大小。  
  
1.  切换到报表设计视图。  
  
2.  右键单击水平轴，然后单击“水平轴属性”。  
  
3.  在“数字”选项卡的“类别”中选择“日期”  。  
  
5.  在“类型”框中，选择“2000 年 1 月 31 日” 。  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
7.  在“主文件夹”选项卡上，单击“运行”以预览报表。  
  
日期会以您选择的日期格式显示。 图表仍不会在水平轴上显示每个类别的标签。 

![屏幕截图：显示日期格式的报表生成器柱形图。](../reporting-services/media/report-builder-column-chart-2-preview.png)
  
通过旋转标签和指定间隔，可以自定义标签显示方式。  
  
## <a name="4-rotate-the-axis-labels-on-the-horizontal-axis"></a>4.在水平轴上旋转轴标签  
  
1.  切换到报表设计视图。  
  
2.  右键单击水平轴标题，然后单击“显示轴标题”以删除标题。 因为水平轴显示日期，所以不需要标题。  
  
3.  右键单击水平轴，然后单击“水平轴属性”。  
  
5.  在“标签”选项卡的“更改轴标签自动调整选项”下，选择“禁用自动调整”  。  
  
7.  在“标签旋转角度”中，选择 -90 。  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    水平轴的示例文本将旋转 90 度。  
    
    ![屏幕截图：设计视图中 x 轴文本已旋转的报表生成器柱形图。](../reporting-services/media/report-builder-column-chart-rotate-x-axis.png)
  
9. 单击 **“运行”** 以预览报表。  
  
在图表中，会标签旋转。  

![屏幕截图：x 轴文本已旋转的报表生成器柱形图的预览。](../reporting-services/media/report-builder-column-chart-rotate-x-axis-preview.png)
  
## <a name="5-move-the-legend"></a><a name="Legend"></a>5.移动图例  
系统会根据类别和序列数据自动创建图例。 可在柱形图的图表区域的下方移动图例。  
  
1.  切换到报表设计视图。  
  
2.  右键单击图表上的图例，然后单击“图例属性”。  
  
3.  在“布局和位置”下，选择其他位置。 例如，选择底部中间选项。  
  
    如果将图例置于图表的顶部或底部，则图例的布局将会从垂直改为水平。 可以在“布局”框中选择不同的布局。  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
5.  （可选）因为本教程中只有一个类别，所以图表不需要图例。 若要删除图例，右键单击“图例”，然后单击“删除图例”。  
  
6.  单击 **“运行”** 以预览报表。  
  
## <a name="6-title-the-chart"></a><a name="ChartTitle"></a>6.设置图表的标题  
    
1.  切换到报表设计视图。  
  
2.  选择图表顶部的词语“图表标题”，然后键入 Store Sales Order Totals 。  
  
3.  单击 **“运行”** 以预览报表。  
  
## <a name="7-format-and-label-the-vertical-axis"></a><a name="Vertical"></a>7.设置垂直轴的格式和标签  
默认情况下，垂直轴采用常用格式显示值，将自动调整为适合图表的大小。   
  
1.  切换到报表设计视图。  
  
2. 单击图表左侧垂直轴上的标签以选择它们。  
  
3.  在“主文件夹”选项卡上，单击“数字”组，然后单击“货币”按钮  。 轴标签将更改以显示货币格式。  
  
4.  单击两次“减少小数位数”按钮，以显示舍入为最接近的美元金额。  
  
5.  右键单击垂直轴，然后单击“垂直轴属性”。  
  
6.  在“数字”选项卡中，注意已在“类别”框中选择了“货币”，并且“小数位数”已经为 0（零）    。  
  
7.  检查“值的显示位置”。 已选中“千位”。  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
9. 右键单击垂直轴，然后单击“显示轴标题”。 

10. 右键单击垂直轴标题，然后单击“轴标题属性”。  
  
10. 将“标题文本”字段中的文本替换为 Sales Total (in Thousands) 。 还可以指定与如何设置标题格式相关的多种选项。  
  
11. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
12. 单击 **“运行”** 以预览报表。  

    ![屏幕截图：报表生成器柱形图格式预览，其中已正确标注 y 轴。](../reporting-services/media/report-builder-column-chart-format-y-axis.png)
    
## <a name="8-show-all-the-labels-on-the-horizontal-x-axis"></a>8.在水平 (x) 轴上显示所有标签

你会注意到 x 轴上仅显示了部分标签。 在本部分中，可在“属性”窗格设置属性以显示所有标签。

1.  切换到报表设计视图。  
  
2.  单击图表，然后选择水平轴标签。

3. 在“属性”窗格中，将 LabelInterval 设置为 1。

    ![报表生成器柱形图的屏幕截图，其中显示了如何设置标签间隔。](../reporting-services/media/report-builder-column-chart-set-label-interval.png)

    该图表同设计视图中一样。 
    
5.  单击 **“运行”** 以预览报表。

    ![屏幕截图：标签间隔设置为 1 的报表生成器柱形图的预览。](../reporting-services/media/report-builder-column-chart-label-interval-one-preview.png)
    
    现在，图表显示其所有标签。
  
## <a name="9-add-a-moving-average-with-a-calculated-series"></a><a name="Average"></a>9.添加带已计算序列的移动平均线  

移动平均值是序列中数据的平均值，它是根据一段时间内的数据计算的。 移动平均线可以确定趋势。
  
1.  切换到报表设计视图。  
  
2.  双击图表以显示“图表数据”窗格。  
  
3.  右键单击“值”区域中的“[Sum(Sales)]”字段，然后单击“添加计算序列”  。  

     ![显示如何将计算序列添加到报表生成器柱形图的屏幕截图。](../reporting-services/media/report-builder-column-chart-add-calculated-series.png)
  
4.  在“公式”中，验证是否已选中“移动平均线” 。  
  
5.  在“设置公式参数”中，针对“期间”，选择“4”  。  
  
6.  在“边框”选项卡的“线条宽度”中，选择“3磅”  。  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
9. 单击 **“运行”** 以预览报表。  
  
图表将显示一条线条，它按日期显示销售总计的移动平均线，每隔四天计算一次平均值。 阅读更多有关 [向图表添加移动平均线](../reporting-services/report-design/add-a-moving-average-to-a-chart-report-builder-and-ssrs.md)的信息。 

![描述移动平均线的报表生成器柱形图的屏幕截图。](../reporting-services/media/report-builder-column-chart-moving-average.png)
  
## <a name="10-add-a-report-title"></a><a name="Title"></a>10.添加报表标题  
  
1.  切换到报表设计视图。  
  
2.  在设计图面上，单击“单击以添加标题”。  
  
3.  键入 **Sales Chart** ，再按 Enter，然后键入 **January to December 2015** ，内容如下所示：  
  
    **Sales Chart**  
  
    **January to December 2015**  
  
4.  选择“Sales Chart”，然后转至“主文件夹”选项卡 > “字体”部分 > “粗体”。  
  
5.  选择“January to December 2015”，并在“主文件夹”选项卡上，单击“字体”部分，将字号设置为 10   。  
  
6.  （可选）可能需要使“标题”文本框更高一些，以容纳两行文本。 单击下边缘中间，显示双向箭头时下拉。 并且可能需要拖动图表的顶部，使标题不重叠。  
  
    此标题将出现在报表的顶部。 未定义页标头时，报表体顶部的项就等同于报表标头。  
  
7.  单击 **“运行”** 以预览报表。  
  
## <a name="11-save-the-report"></a><a name="Save"></a>11.保存报表  
  
### <a name="to-save-the-report"></a>保存报表  
  
1.  切换到报表设计视图。  
  
2.  从“报表生成器”按钮，单击 **“另存为”** 。  

    可将其保存到计算机或报表服务器中。
  
3.  在“名称”中，键入 Sales Order Column Chart 。  
  
4.  单击“ **保存** ”。  
  
## <a name="next-steps"></a>后续步骤  
您已成功完成“向报表添加柱形图”教程。 若要了解有关图表的详细信息，请参阅[图表（报表生成器和 SSRS）](../reporting-services/report-design/charts-report-builder-and-ssrs.md)和[迷你图和数据条（报表生成器和 SSRS）](../reporting-services/report-design/sparklines-and-data-bars-report-builder-and-ssrs.md)。  
  
## <a name="see-also"></a>另请参阅  
-    [报表生成器教程](../reporting-services/report-builder-tutorials.md) 
-    [SQL Server 中的报表生成器](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  

