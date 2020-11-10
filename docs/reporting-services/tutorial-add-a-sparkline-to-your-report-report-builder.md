---
title: 教程：向报表添加迷你图（报表生成器）| Microsoft Docs
description: 了解如何使用报表生成器在 Reporting Services 分页报表中创建具有迷你图的基本表。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 18c90a36-48bf-4805-a960-2d1e8f00c2dc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a60dfa5cf08b11789fde744f41a2c139f922d286
ms.sourcegitcommit: 9e2c682929ee64c051dc62f8917d147861f7c635
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043797"
---
# <a name="tutorial-add-a-sparkline-to-your-report-report-builder"></a>教程：向报表添加迷你图（报表生成器）

在 [!INCLUDE[ssRBnoversion_md](../includes/ssrbnoversion.md)]的本教程中，使用 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 分页报表中的迷你图创建基本表。   
  
迷你图和数据条是较小的简单图表，可以在很小的空间中传递很多信息，常常出现在 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 报表的表格和矩阵中。 下图所示报表和你要创建的报表类似。  
  
![显示报表生成器迷你图的屏幕截图。](../reporting-services/media/report-builder-sparkline-final.png)  
     
完成本教程的估计时间：30 分钟。  
  
## <a name="requirements"></a>要求  
有关要求的详细信息，请参阅[教程先决条件（报表生成器）](../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
## <a name="1-create-a-report-with-a-table"></a><a name="CreateTable"></a>1.创建含有表的报表  
  
1.  通过计算机、[Web 门户或 SharePoint 集成模式](../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 。  
  
    将打开“新建报表或数据集”对话框。  
  
    如果未出现“新建报表或数据集”对话框，请通过“文件”菜单转至“新建”  。  
  
2.  在左窗格中，确认已选中 **“新建报表”** 。  
  
3.  在右窗格中，单击“表或矩阵向导”。  
  
4.  在“选择数据集”页上，单击“创建数据集” > “下一步”  。 将打开“选择数据源的连接”页面。  
  
    > [!NOTE]  
    > 本教程不需要具体数据；只需要与 SQL Server 数据库连接。 如果已经具有在“数据源连接”下列出的某一数据源连接，则可以选择该连接并且转到步骤 10。 有关详细信息，请参阅[获取数据连接的备选方式（报表生成器）](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md)。  
  
5.  单击 **“新建”** 。 此时将打开 **“数据源属性”** 对话框。  
  
6.  在“名称”中，键入数据源的名称“Product Sales” 。  
  
7.  在“选择连接类型”中，确认已选择“Microsoft SQL Server” 。  
  
8.  在“连接字符串”中，键入以下文本：  
  
    `Data Source\=<servername>`  
  
    表达式 `<servername>`（例如 Report001）指定安装了 SQL Server 数据库引擎实例的计算机。 因为报表数据不是从 SQL Server 数据库提取的，所以不需要包括数据库的名称。 指定服务器上的默认数据库用于对查询进行分析。  
  
9. 单击 **“凭据”** 。 输入访问外部数据源所需的凭据。  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    返回至“选择数据源的连接”页。  
  
11. 若要验证是否能连接到数据源，请单击“测试连接”。  
  
    将显示消息“已成功地创建连接”。  
  
12. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
13. 单击“下一步”。  
  
## <a name="2-create-a-query-and-table-layout-in-the-table-wizard"></a><a name="Query"></a>2.在表向导中创建查询和表布局  
在报表中，可以使用具有预定义查询的共享数据集，也可以创建仅在报表中使用的嵌入数据集。 在本教程中，将创建一个嵌入数据集。  
  
> [!NOTE]  
> 在本教程中，由于查询包含了数据值，因此它不需要外部数据源。 这样，查询就会非常长。 在业务环境中，查询不会包含数据。 本教程中的查询仅供学习使用。  
  
### <a name="to-create-a-query-and-table-layout-in-the-table-wizard"></a>在表向导中创建查询和表布局 
  
1.  在“设计查询”页中，关系查询设计器处于打开状态。 在本教程中，您将使用基于文本的查询设计器。  
  
2.  单击“编辑为文本”。 基于文本的查询设计器将显示查询窗格和结果窗格。  
  
3.  将以下 [!INCLUDE[tsql](../includes/tsql-md.md)] 查询粘贴到“查询”框中。  
  
    ```  
    SELECT CAST('2015-01-04' AS date) as SalesDate, 'Accessories' as Subcategory,   
       'Carrying Case' as Product, CAST(16996.60 AS money) AS Sales, 68 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Carrying Case' as Product, CAST(1350.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2015-01-10' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Carrying Case' as Product, CAST(1147.50 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2015-01-04' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Budget Movie-Maker' as Product, CAST(1056.00 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate,  'Accessories' as Subcategory,  
       'Slim Digital' as Product, CAST(1380.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate,'Accessories' as Subcategory,    
       'Budget Movie-Maker' as Product, CAST(780.00 AS money) AS Sales, 26 as Quantity  
    UNION SELECT CAST('2015-01-07' AS date) as SalesDate, 'Accessories' as Subcategory,   
       'Budget Movie-Maker' as Product, CAST(3798.00 AS money) AS Sales, 9 as Quantity  
    UNION SELECT CAST('2015-01-08' AS date) as SalesDate, 'Camcorders' as Subcategory,   
       'Budget Movie-Maker' as Product, CAST(10400.00 AS money) AS Sales, 13 as Quantity  
    UNION SELECT CAST('2015-01-09' AS date) as SalesDate, 'Camcorders' as Subcategory,   
       'Budget Movie-Maker' as Product, CAST(3000.00 AS money) AS Sales, 60 as Quantity  
    UNION SELECT CAST('2015-01-10' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Budget Movie-Maker' as Product, CAST(7234.50 AS money) AS Sales, 39 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Carrying Case' as Product, CAST(10836.00 AS money) AS Sales, 84 as Quantity  
    UNION SELECT CAST('2015-01-07' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Slim Digital' as Product, CAST(2550.00 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2015-01-04' AS date) as SalesDate, 'Digital' as Subcategory,   
       'Slim Digital' as Product, CAST(8357.80 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2015-01-08' AS date) as SalesDate, 'Digital SLR' as Subcategory,   
       'Slim Digital' as Product, CAST(18530.00 AS money) AS Sales, 34 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Digital SLR' as Subcategory,   
       'Slim Digital' as Product, CAST(26576.00 AS money) AS Sales, 88 as Quantity  
    ```  
  
4.  在查询设计器工具栏中，单击“运行”( **!** )。  
  
    该查询运行并显示 **SalesDate** 、 **Subcategory** 、 **Product** 、 **Sales** 和 **Quantity** 字段的结果集。  
  
5.  单击“下一步”。  
  
6.  在“排列字段”页上，将 Sales 拖到“值”中  。  
  
    **Sales** 由 Sum 函数聚合。 值为 [Sum(Sales)]。  
  
7.  将 Product 拖到“行组”中 。  
  
8.  将 SalesDate 拖到“列组”中 。  

    ![显示如何排列字段的屏幕截图。](../reporting-services/media/report-builder-sparkline-arrange-fields.png)
  
9. 单击“下一步”。  
  
10. 在“选择布局”页的“选项”下，确认已选择“显示小计和总计”  。  
  
    向导的“预览”窗格将显示包含有三行的表。 当您运行报表时，每行将按以下方式显示：  
  
    *  第一行将对表出现一次以显示列标题。  
  
    *  第二行将对每个产品重复一次并显示产品名称、每日总计和行总计。  
  
    *  第三行将对表出现一次以显示总计。  
    
    ![显示如何选择布局的屏幕截图。](../reporting-services/media/report-builder-sparkline-choose-layout.png)
  
11. 单击“下一步”。  
  
12. 单击“完成”。  
  
14. 表将添加到设计图面中。 此表有三列和三行。  
  
    在“分组”窗格中查找。 如果未显示“分组”窗格，请在“视图”菜单上，单击“分组” 。 “行组”窗格显示一个行组：“Product”。 “列组”窗格显示一个列组：“SalesDate”。 详细信息数据是由数据集查询检索的所有数据。  
    
    ![显示报表生成器分组窗格的屏幕截图。](../reporting-services/media/report-builder-sparkline-grouping-pane.png)
  
15. 单击 **“运行”** 以预览报表。  

### <a name="2a-format-data-as-currency"></a><a name="FormatCurrency"></a>2a. 将数据格式设置为货币  
默认情况下，“Sales”字段的汇总数据显示为常规数字。 请设置其格式，以使其显示货币形式的数字。 切换“占位符样式”，将格式化的文本框和占位符文本显示为示例值。  
  
1.  单击 **“设计”** 切换到设计视图。  
  
2.  单击“SalesDate”列的第二行（位于列标题行下方）中的单元格。 按住 Ctrl 键并选择包含 `[Sum(Sales)]`的所有单元格。 

    ![显示如何选择总销售额的屏幕截图。](../reporting-services/media/report-builder-select-sum-sales.png) 
  
3.  在“开始”选项卡上的“数字”组中，单击“货币”  。 单元会更改为显示已设置好格式的货币。  

    ![显示用占位符货币值替换总销售额的屏幕截图。](../reporting-services/media/report-builder-placeholder-currency.png)
  
    如果区域设置为“英语(美国)”，则默认示例文本为 [ **$12,345.00** ]。 如果看不到示例货币值，请在“数字”组中单击“占位符样式” > “示例值”  。  
    
    ![显示已选择“示例值”选项的屏幕截图。](../reporting-services/media/report-builder-placeholder-value-button.png)
   
### <a name="2b-optional-format-data-as-dates"></a><a name="FormatDates"></a>2b. （可选）将数据格式设置为日期格式  
默认情况下，“SalesDate”字段同时显示日期和时间信息。 您可以设置其格式，使其只显示日期。  
  
1.  单击包含 `[SalesDate]`的单元格。  
  
3.  在“开始”选项卡上，转至“数字”组，再转至“日期”  。  
  
    单元格会显示示例日期 **[2000/1/31]** 。
     
4.  单击 **“运行”** 以预览报表。  
  
**SalesDate** 值显示为默认日期格式， **Sales** 的汇总值显示为货币格式。   
  
## <a name="3-add-a-sparkline"></a><a name="Sparkline"></a>3.添加迷你图    
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  选择表中的“Total”列。  
  
3.  右键单击，指向“插入列”，然后单击“左” 。  

    ![显示如何插入左侧列的屏幕截图。](../reporting-services/media/report-builder-add-column-left.png)
  
4.  在新列中，右键单击 `[Product]` 行中的单元格，选择“插入” > “迷你图” 。  

    ![显示如何插入迷你图的屏幕截图。](../reporting-services/media/report-builder-insert-sparkline.png)
  
5.  在“选择迷你图类型”对话框中，确保已选择“列”行中的第一个迷你图，然后单击“确定”  。  
  
6.  单击要在“图表数据”窗格中显示的迷你图。  
  
7.  单击“值”框旁边的加号 (+)，然后单击“Sales”。 

    ![显示如何将值添加到报表生成器迷你图的屏幕截图。 ](../reporting-services/media/report-builder-sparkline-values.png) 
  
    “Sales”字段中的值现在是迷你图的值。  
  
8.  单击“类别组”框旁边的加号 (+)，然后单击“SalesDate”。  
  
9. 单击 **“运行”** 以预览报表。  
  
    请注意迷你图中的各条形并不相互对齐。 在第二行数据中只有四个条形，因此，这些条比第一行中的条宽，因为第一行中有六个条形。 您不能比较每天每个产品的值。 它们之间需要相互对齐。  
  
    还要注意的是，对于每一行，该行的最高的条形是行高。 这也会产生误导，因为每一行的最大值不相等：Budget Movie-Maker 的最大值是 $10,400，但 Slim Digital 的最大值是 $26,576 - 是前者的两倍还多。 并且，这两行的最大的条形大约为相同高度。 所有迷你图需要使用相同的比例。  
  
     ![显示未对齐的报表生成器迷你图的屏幕截图。](../reporting-services/media/report-builder-sparkline-misaligned.png)
  
## <a name="4-align-the-sparklines-vertically-and-horizontally"></a><a name="AlignSparklines"></a>4.沿水平方向和垂直方向对齐迷你图  
当所有迷你图不是使用相同的度量尺度时理解起来很困难。 每个迷你图的水平轴和垂直轴都需要与其他部分匹配。  
   
1.  单击 **“设计”** 返回设计视图。  
  
2.  右键单击迷你图，然后单击“垂直轴属性”。  
  
3.  选中“对齐轴”复选框。 Tablix1 是列表中唯一的选项。  
  
     该选项设置在各迷你图中条形相对于其他条形的高度。 
  
4.  单击“确定”。  
  
5.  右键单击迷你图，然后单击“水平轴属性”。  
  
6.  选中“对齐轴”复选框。 Tablix1 是列表中唯一的选项。 
  
    该选项设置在各迷你图中条形相对于其他条形的宽度。 如果某些迷你图中的条形比其他迷你图少，则这些迷你图将对于缺失的数据具有空白空间。  
  
7.  单击“确定”。  
  
8.  单击“运行”，再次预览报表。  
  
现在每个迷你图中的所有条形均与其他迷你图中的条形对齐，它们的高度也是相对的。  
  
![显示已对齐的报表生成器迷你图的屏幕截图。](../reporting-services/media/report-builder-sparkline-aligned.png)
  
## <a name="7-optional-change-column-widths"></a><a name="Width"></a>7.（可选）更改列宽  
默认情况下，表中的每个单元格都包含一个文本框。 在呈现页面时，文本框将垂直扩展以容纳文本。 在呈现的报表中，每个行将扩展到行中呈现的最高文本框的高度。 设计图面上的行的高度不会影响已呈现报表中的行的高度。  
  
若要减少每个行占用的垂直空间量，请扩展列宽以容纳单个行的列中的文本框的预计内容。  
  
### <a name="to-change-the-width-of-columns"></a>更改列宽  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  单击表，灰色条形显示在表的上方和旁边。 这些是列和行的控点
  
3.  指向列控点之间的行，使光标变为双箭头。 拖动“Product”列，以便产品名称显示在一行中。  
  
4.  单击“运行”以预览报表，并查看报表是否足够宽。  
  
## <a name="8-optional-add-a-report-title"></a><a name="Title"></a>8.（可选）添加报表标题  
报表标题将出现在报表的顶部。 可以将报表标题置于报表表头中或置于表体顶部的文本框中（如果报表未使用表头）。 在本教程中，您将使用自动放置在表体顶部的文本框。  
  
通过将不同的字体样式、大小和颜色应用于文本的短语和单个字符，可以进一步增强文本。 有关详细信息，请参阅[设置文本框中文本的格式（报表生成器和 SSRS）](../reporting-services/report-design/format-text-in-a-text-box-report-builder-and-ssrs.md)。  
  
### <a name="to-add-a-report-title"></a>添加报表标题  
  
1.  在设计图面上，单击“单击以添加标题”。  
  
2.  键入“Sales by Date”，然后在文本框外部单击。  
  
3.  选择包含“Product Sales”的文本框。  
  
4.  在“开始”选项卡上，单击“字体”组，转至“颜色”，然后选择“蓝绿色”  。  
  
7.  选择“粗体”。  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="9-save-the-report"></a><a name="Save"></a>9.保存报表  
将报表保存到报表服务器或计算机上。 如果不将报表保存到报表服务器上，则许多 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 功能（如报表部件和子报表）将不可用。  
  
### <a name="to-save-the-report-on-a-report-server"></a>将报表保存到报表服务器  
  
1.  从 **“报表生成器”** 按钮，单击 **“另存为”** 。  
  
2.  单击 **“最近使用的站点和服务器”** 。  
  
3.  选择或键入您拥有保存报表权限的报表服务器的名称。  
  
    此时将显示“正在连接到报表服务器”消息。 当连接完成时，您将看到报表服务器管理员指定为报表默认位置的报表文件夹的内容。  
  
4.  在“名称”中，用“Product Sales”替换默认名称 。  
  
5.  单击“ **保存** ”。  
  
报表即已保存至报表服务器。 您连接的报表服务器的名称将显示在窗口底部的状态栏中。  
  
### <a name="to-save-the-report-on-your-computer"></a>将报表保存到计算机上  
  
1.  从 **“报表生成器”** 按钮，单击 **“另存为”** 。  
  
2.  依次单击“桌面”、“我的文档”或“我的电脑”，并浏览到要保存该报表的文件夹  。  
  
3.  在“名称”中，用“Product Sales”替换默认名称 。  
  
4.  单击“ **保存** ”。  
  
## <a name="next-steps"></a>后续步骤  

用于创建具有迷你图的表报表的教程到此结束。 有关迷你图的详细信息，请参阅[迷你图和数据条](../reporting-services/report-design/sparklines-and-data-bars-report-builder-and-ssrs.md)。  
  
[报表生成器教程](../reporting-services/report-builder-tutorials.md) 
[SQL Server 中的报表生成器](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
