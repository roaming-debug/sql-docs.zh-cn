---
title: 教程：向报表添加条形图（报表生成器）| Microsoft Docs
description: 了解如何使用报表生成器中的向导在 Reporting Services 分页报表中创建条形图。
ms.date: 06/15/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 6956ebd6-0217-4087-a4fa-5cc1c3804691
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9b18950488cb0fed336feb3df61e5a4e66b56603
ms.sourcegitcommit: 36fe62a3ccf34979bfde3e192cfa778505add465
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94521117"
---
# <a name="tutorial-add-a-bar-chart-to-your-report-report-builder"></a>教程：向报表添加条形图（报表生成器）
本教程将使用 [!INCLUDE[ssRBnoversion_md](../includes/ssrbnoversion.md)] 中的向导，在 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 分页报表中创建条形图。 然后添加筛选器，并优化图表。 

条形图以水平方式显示类别数据。 这有助于：  
  
-   提高长类别名称的可读性。  
-   提高绘制为值的时间的可理解性。   
-   比较多个序列的相对值。  
  
下图显示了将创建的条形图，条形图从 2015 年最低销售额开始排列，显示了 2014 和 2015 年度前五位销售人员的销售情况。  
  
![报表生成器条形图的屏幕截图。](../reporting-services/media/report-builder-bar-chart.png) 
  
 
> [!NOTE]  
> 在本教程中，将向导的多个步骤合并为一个过程。 有关如何浏览到报表服务器、创建数据集和选择数据源的分步说明，请参阅这一系列教程中的第一个教程：[教程：创建基本表报表（报表生成器）](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md)。  
  
完成本教程的估计时间：15 分钟。  
  
## <a name="requirements"></a>要求  
有关要求的详细信息，请参阅[教程先决条件（报表生成器）](../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
## <a name="1-create-a-chart-report-from-the-chart-wizard"></a><a name="Chart"></a>1.使用图表向导创建图表报表  
从中创建嵌入数据集，选择共享数据源，并使用“图表向导”创建条形图。  
  
> [!NOTE]  
> 在本教程中，由于查询包含了数据值，因此它不需要外部数据源。 这样，查询就会非常长。 在业务环境中，查询不会包含数据。 本教程中的查询仅供学习使用。  
  
1.  从[Web 门户、从 SharePoint 集成模式下的报表服务器，或从计算机](../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] web portal, 启动报表生成器 report server in SharePoint integrated mode, or from your computer.  
  
     此时将显示 **“入门”** 对话框。  
  
     ![报表生成器入门](../reporting-services/media/rb-getstarted.png "报表生成器入门")  
  
     如果看不到“入门”对话框中，单击“文件” >“新建”。 “新建报表或数据集”  对话框的内容与“入门”  对话框的内容大致相同。 
      
2.  在左窗格中，确认已选中 **“新建报表”** 。  
  
3.  在右窗格中，单击“图表向导”。  
  
4.  在“选择数据集”页上，单击“创建数据集”，然后单击“下一步”  。  
  
5.  在“选择数据源的连接”页中，选择现有数据源或浏览到报表服务器并选择一个数据源，然后单击“下一步”。 您可能需要输入用户名和密码。  
  
    > [!NOTE]  
    > 只要您具有足够的权限，则选择哪一个数据源并不重要。 您将不会从数据源中获取数据。 有关详细信息，请参阅[获取数据连接的备选方式（报表生成器）](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md)。  
  
6.  在“设计查询”页上，单击“编辑为文本” 。  
  
7.  将以下查询粘贴到查询窗格中：  
  
    ```  
    SELECT 'Luis' as FirstName, 'Alverca' as LastName, CAST(170000.00 AS money) AS SalesYear2015, CAST(150000. AS money) AS SalesYear2014  
    UNION SELECT 'Jeffrey' as FirstName, 'Zeng' as LastName, CAST(210000. AS money) AS SalesYear2015, CAST(190000. AS money) AS SalesYear2014  
    UNION SELECT 'Houman' as FirstName, 'Pournasseh' as LastName, CAST(150000. AS money) AS SalesYear2015, CAST(180000. AS money) AS SalesYear2014  
    UNION SELECT 'Robin' as FirstName, 'Wood' as LastName, CAST(75000. AS money) AS SalesYear2015, CAST(175000. AS money) AS SalesYear2014  
    UNION SELECT 'Daniela' as FirstName, 'Guaita' as LastName,  CAST(170000. AS money) AS SalesYear2015, CAST(175000. AS money) AS SalesYear2014  
    UNION SELECT 'John' as FirstName, 'Yokim' as LastName, CAST(160000. AS money) AS SalesYear2015, CAST(195000. AS money) AS SalesYear2014  
    UNION SELECT 'Delphine' as FirstName, 'Ribaute' as LastName, CAST(180000. AS money) AS SalesYear2015, CAST(205000. AS money) AS SalesYear2014  
    UNION SELECT 'Robert' as FirstName, 'Hernady' as LastName, CAST(140000. AS money) AS SalesYear2015, CAST(180000. AS money) AS SalesYear2014  
    UNION SELECT 'Tanja' as FirstName, 'Plate' as LastName, CAST(150000. AS money) AS SalesYear2015, CAST(160000. AS money) AS SalesYear2014  
    UNION SELECT 'David' as FirstName, 'Bradley' as LastName, CAST(210000. AS money) AS SalesYear2015, CAST(180000. AS money) AS SalesYear2014  
    UNION SELECT 'Michal' as FirstName, 'Jaworski' as LastName, CAST(175000. AS money) AS SalesYear2015, CAST(220000. AS money) AS SalesYear2014  
    UNION SELECT 'Chris' as FirstName, 'Ashton' as LastName, CAST(195000. AS money) AS SalesYear2015, CAST(205000. AS money) AS SalesYear2014  
    UNION SELECT 'Pongsiri' as FirstName, 'Hirunyanitiwatna' as LastName, CAST(175000. AS money) AS SalesYear2015, CAST(215000. AS money) AS SalesYear2014  
    UNION SELECT 'Brian' as FirstName, 'Burke' as LastName, CAST(187000. AS money) AS SalesYear2015, CAST(207000. AS money) AS SalesYear2014  
    ```  
  
8.  （可选）单击“运行”按钮 ( **!** )，查看要用于图表的数据。  
  
9. 单击“下一步”。  
  
## <a name="2-create-a-bar-chart"></a><a name="ChartType"></a>2.创建条形图  
 
1.  在“选择图表类型”页上，柱形图为默认图表类型。  
  
2.  单击“条形图”，然后单击“下一步” 。  
  
    “排列图表字段”页上的“可用字段”窗格中有 4 个字段：“FirstName”、“LastName”、“SalesYear2015”和“SalesYear2014”。  
  
3.  将 LastName 拖动到“类别”窗格。  
  
4.  将 SalesYear2015 拖动到“值”窗格。 SalesYear2015 表示每个销售人员在 2015 年的销售总额。 “值”窗格显示 `[Sum(SalesYear2015)]` ，因为该图表显示的是每个产品的销售总额。  
  
5.  将 SalesYear2014 拖动到 SalesYear2015 下的“值”窗格。 SalesYear2014 表示每个销售人员在 2014 年的销售总额。  
  
6.  单击“下一步”。  
  
7.  单击“完成”。  
  
    图表将添加到设计图面中。 请注意，新的条形图只显示表述性数据。 图例读取 Last Name A、Last Name B 等，而不是人员的姓名，这只用于展示报表的外观。 
  
9. 单击图表以显示图表控点。 拖动该图表的右下角以扩大该图表。 注意在你拖动时设计图面会变大。 
  
10. 单击 **“运行”** 以预览报表。  
  
条形图将显示每个销售人员在 2014 和 2015 年度的销售情况。 条形图的长度对应于总销售额。  
  
## <a name="3-display-all-the-names-on-the-vertical-axis"></a><a name="AllValues"></a>3.在垂直轴上显示所有名称  
默认情况下，垂直轴上只显示某些值。 您可以更改图表以显示所有类别。  
  
1.  切换到报表设计视图。  
  
2.  右键单击垂直轴，然后单击“垂直轴属性”。  
  
3.  在“轴范围和间隔”下的“间隔”框中，键入 1  。  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
5.  单击 **“运行”** 以预览报表。  
  
> [!NOTE]  
> 如果无法阅读垂直轴上的销售人员姓名，则可以增加图表的高度，或更改轴标签的格式选项。  
  
### <a name="display-last-name-and-first-name-on-vertical-axis"></a><a name="CategoryExpression"></a>在垂直轴上显示姓氏和名字  
可以更改类别表达式以将每个销售人员的姓氏包含在名字之后。  
  
1.  切换到报表设计视图。  
  
2.  双击图表以显示“图表数据”窗格。  
  
3.  在“类别组”区域中右键单击 [LastName]，然后单击“类别组属性” 。  
  
4.  在“标签”中，单击表达式 (Fx) 按钮。  
  
5.  键入以下表达式： `=Fields!LastName.Value & ", " & Fields!FirstName.Value`  
  
    此表达式将连接姓氏、逗号和名字。  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
8.  单击 **“运行”** 以预览报表。  
  
如果在运行报表时没有显示名字，则您可以手动刷新数据。 在仍处于预览模式时，在“导航”组的“运行”选项卡上，单击“刷新”  。  
  
> [!NOTE]  
> 如果无法阅读垂直轴上的销售人员姓名，则可以增加图表的高度，或更改轴标签的格式选项。  
  
## <a name="4-change-the-sort-order-on-the-vertical-axis"></a><a name="Sort"></a>4.更改垂直轴上的排序顺序  
当您对图表中的数据进行排序时，您是在更改类别轴上的值的顺序。  
  
1.  切换到报表设计视图。  
  
2.  双击图表以显示“图表数据”窗格。  
  
3.  在“类别组”区域中右键单击 [LastName]，然后单击“类别组属性” 。  
  
4.  单击 **“排序”** 。 “更改排序选项”页将显示排序表达式的列表。 默认情况下，此列表具有一个与原始类别组表达式相同的排序表达式。  
  
5.  在“排序方式”中单击“[SalesYear2015]” 。  
  
6.  在“顺序”列表中，选择“A 到 Z”，按 2015 年销售额从大到小的顺序显示姓名 。
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
10. 单击 **“运行”** 以预览报表。  
  
水平轴上的姓名按 2015 年销售额从大到小的顺序排列， **Zeng** 位于顶部。  
  
## <a name="5-move-the-legend"></a><a name="Legend"></a>5.移动图例  
为了提高图表值的可读性，可能需要移动图表图例。 例如，在水平显示图条的条形图中，可以更改图例的位置，将其放置在图表区的上方或下方。 这可为图条提供更大的水平空间。  
  
#### <a name="to-display-the-legend-below-the-chart-area-of-a-bar-chart"></a>在条形图的图表区下方显示图例  
  
1.  切换到报表设计视图。  
  
2.  右键单击图表上的图例。  
  
3.  选择“图例属性”。  
  
4.  对于“图例位置”，请选择其他位置。 例如，将图例位置设置为底部中间。  
  
    如果将图例置于图表的顶部或底部，则图例的布局将会从垂直改为水平。 可以从“布局”下拉列表中选择不同的布局。  
  
5.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
6.  单击 **“运行”** 以预览报表。  
  
## <a name="6-title-the-chart"></a><a name="ChartTitle"></a>6.设置图表的标题  
  
1.  切换到报表设计视图。  
  
2.  选择图表顶部的“图表标题”字词，再键入：“2014 年和 2015 年销售额”。  
  
3.  选择标题后，在“属性”窗格中，将“颜色”设置为“黑色”并将“字体”设置为“12磅”   。 
  
4.  单击 **“运行”** 以预览报表。  
  
## <a name="7-format-and-label-the-horizontal-axis"></a><a name="Horizontal"></a>7.设置水平轴的格式和标签  
默认情况下，水平轴采用常用格式显示值，将自动调整为适合图表的大小。 可以将其更改为货币格式。  
   
1.  切换到报表设计视图。  
  
2.  沿图表底部单击水平轴，以选择它。  
  
3.  在“主文件夹”选项卡上，依次单击“数字”组、“货币”  。 水平轴标签将更改为货币。  
  
3.  （可选）删除小数位数。 在“货币”按钮附近，单击两次“减少小数位数”按钮 。  
  
4.  右键单击水平轴，然后单击“水平轴属性”。  
  
5.  在“数字”选项卡上，选择“以千为单位显示值” 。  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  

8.  右键单击水平轴，然后选择“显示轴标题”。
  
7.  在“轴标题”框中，键入 Sales in thousands，然后按 Enter 。  

    >**注意：** 键入时，“轴标题”框显示在垂直轴上。 但是，按 Enter 后，它将转到水平轴。
  
9. 单击 **“运行”** 以预览报表。  
  
报表将在水平轴上以千为单位将销售额显示为货币，且没有小数位数。  
  
## <a name="8-add-a-filter-to-display-the-top-five-values"></a><a name="Filter"></a>8.添加筛选器以显示前五个值  
可以向图表添加筛选器，以指定数据集中哪些数据要包含于图表中或排除在图表外。   
  
1.  切换到报表设计视图。  
  
2.  双击图表以显示“图表数据”窗格。  
  
3.  在“类别组”区域中，右键单击 [LastName] 字段，然后单击“类别组属性” 。  
  
4.  单击 **“筛选器”** 。 “更改筛选器”页可以显示筛选器表达式的列表。 默认情况下，此列表为空。  
  
5.  单击“添加”。 此时将显示一个新的空白筛选器。  
  
6.  在“表达式”中，键入 [Sum(SalesYear2015)] 。 此时将创建基础表达式 `=Sum(Fields!SalesYear2015.Value)`，可以查看是否单击了“fx”按钮。  
  
7.  验证确保数据类型是“文本”。  
  
8.  在“运算符”中，从下拉列表中选择“前 N 个” 。  
  
9. 在“值”中，键入以下表达式：=5   
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
11. 单击 **“运行”** 以预览报表。  
  
如果在运行报表时没有对结果进行筛选，则可以手动刷新数据。 在“导航”组的“运行”选项卡上，单击“刷新”  。  
  
图表将显示 2015 年销售数据中前五位销售人员的姓名。  
  
## <a name="9-add-a-report-title"></a><a name="Title"></a>9.添加报表标题  
  
1.  在设计图面上，单击“单击以添加标题”。  
  
2.  键入 **Sales Bar Chart**，再按 Enter，然后键入 **Top Five Sellers for 2015**，如下所示：  
  
    **Sales Bar Chart**  
  
    **Top Five Sellers for 2015**  
  
3.  选择 **Sales Bar Chart**，并单击“加粗”按钮。  
  
4.  选择 Top Five Sellers for 2015，并在“主文件夹”选项卡上的“字体”部分中，将字号设置为“10”   。  
  
5.  （可选）可能需要调高“标题”文本框高度，并将条形图的顶部向下拉，以容纳两行文本。  
  
    此标题将显示在报表的顶部。 未定义页标头时，报表体顶部的项就等同于报表标头。  
  
6.  单击 **“运行”** 以预览报表。  
  
## <a name="10-save-the-report"></a><a name="Save"></a>10.保存报表  
  
1.  切换到报表设计视图。  
  
2.  单击“文件” > “另存为” 。  
  
3.  在“名称”中，键入 Sales Bar Chart 。  

    可将其保存到计算机或报表服务器中。
  
4.  单击“ **保存**”。   
  
## <a name="next-steps"></a>后续步骤  
您已成功完成“向报表添加条形图”教程的学习。 若要了解有关图表的详细信息，请参阅 [图表](../reporting-services/report-design/charts-report-builder-and-ssrs.md) 和 [条形图](../reporting-services/report-design/bar-charts-report-builder-and-ssrs.md)。  
  
## <a name="see-also"></a>另请参阅  
[报表生成器教程](../reporting-services/report-builder-tutorials.md)  
[SQL Server 中的报表生成器](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  

