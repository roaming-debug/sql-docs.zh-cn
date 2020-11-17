---
title: 教程：生成基本表报表（报表生成器）| Microsoft Docs
description: 了解如何使用向导根据报表生成器中的示例销售数据创建基本的表报表。
ms.date: 06/23/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: d9e30521-f8ae-4c45-89c3-d40727f622f7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bc7e78fb2b4101df84c2d54162d62ec8b95f6aab
ms.sourcegitcommit: 863420525a1f5d5b56b311b84a6fb14e79404860
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94418004"
---
# <a name="tutorial-creating-a-basic-table-report-report-builder"></a>教程：生成基本表报表（报表生成器）
本教程教您如何基于示例销售数据创建基本表格报表。 下图显示了将创建的报表。  
  
![SSRS_Tutorial_Basic_Table_Report](../reporting-services/media/ssrs-tutorial-basic-table-report.png)  
  

学完本教程的估计时间：20 分钟。  
  
## <a name="requirements"></a>要求  
有关要求的详细信息，请参阅[教程先决条件（报表生成器）](../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
## <a name="1-create-a-report-using-a-wizard"></a><a name="CreateTable"></a>1.使用向导创建报表  
使用表或矩阵向导创建表报表。 有两类模式：报表设计模式和共享数据集设计模式。 在报表设计模式中，您可以在“报表数据”窗格中指定数据，在设计图面上指定报表布局。 在共享数据集设计模式中，可以创建与他人共享的数据集查询。 在本教程中，您将使用报表设计模式。  
  
### <a name="to-create-a-report"></a>创建报表  
  
1.  通过计算机、[Web 门户或 SharePoint 集成模式](../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 。  
  
    将打开“新建报表或数据集”对话框。  
  
    如果未出现“新建报表或数据集”对话框，请通过“文件”菜单转至“新建”  。  
  
2.  在左窗格中，确认已选中 **“新建报表”** 。  
  
3.  在右窗格中，选择“表或矩阵向导”。  
  
## <a name="1a-specify-a-data-connection-in-the-table-wizard"></a><a name="DataConnection"></a>1a. 在表向导中指定数据连接  
数据连接包含要连接到外部数据源（如 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库）的信息。 通常会从数据源所有者处获取连接信息以及要使用的凭据类型。 若要指定数据连接，可以从报表服务器使用共享数据源或创建仅在此报表中使用的嵌入数据源。  
  
在本教程中，您将使用嵌入数据源。 若要了解有关使用共享数据源的详细信息，请参阅[获取数据连接的备选方式（报表生成器）](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md)。  
  
### <a name="to-create-an-embedded-data-source"></a>创建嵌入数据源  
  
1.  在“选择数据集”页上，选择“创建数据集”，然后单击“下一步”  。 将打开“选择数据源的连接”页面。  
  
2.  单击 **“新建”** 。 此时将打开 **“数据源属性”** 对话框。  
  
3.  在“名称”中，为数据源键入名称 Product_Sales 。  
  
4.  在“选择连接类型”中，确认已选择“Microsoft SQL Server” 。  
  
5.  在“连接字符串”中，键入以下文本，其中 \<servername> 为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的实例的名称：  
  
    ```  
    Data Source=<servername>  
    ```  
  
    由于您将使用的查询会包含数据而不是从数据库检索数据，因此连接字符串不包含数据库名称。 有关详细信息，请参阅[教程先决条件&#40;报表生成器&#41;](../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
6.  单击“凭据”选项卡。输入访问外部数据源所需的凭据。  
  
7. 再次单击“常规”选项卡。 若要验证是否能连接到数据源，请单击“测试连接”。  
  
    将显示消息“已成功地创建连接”。  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    这将返回“选择数据源的连接”页，且在此页中选择了新数据源。  
  
9. 单击“下一步”。  
  
## <a name="1b-create-a-query-in-the-table-wizard"></a><a name="Query"></a>1b. 在表向导中创建查询  
在报表中，可以使用具有预定义查询的共享数据集，也可以创建仅在这一报表中使用的嵌入数据集。 在本教程中，将创建一个嵌入数据集。  
  
> [!NOTE]  
> 在本教程中，由于查询包含了数据值，因此它不需要外部数据源。 这样，查询就会非常长。 在业务环境中，查询不会包含数据。 本教程中的查询仅供学习使用。  
  
### <a name="to-create-a-query"></a>创建查询  
  
1.  在“设计查询”页中，关系查询设计器处于打开状态。 在本教程中，您将使用基于文本的查询设计器。  
  
    单击“编辑为文本”。 基于文本的查询设计器将显示查询窗格和结果窗格。  
  
2.  将以下 [!INCLUDE[tsql](../includes/tsql-md.md)] 查询粘贴到空白上框中。  
  
    ```  
    SELECT CAST('2009-01-05' AS date) as SalesDate, 'Accessories' as Subcategory,   
       'Carrying Case' as Product, CAST(9924.60 AS money) AS Sales, 68 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Tripod' as Product, CAST(1350.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2009-01-11' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Lens Adapter' as Product, CAST(1147.50 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Mini Battery Charger' as Product, CAST(1056.00 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate,  'Accessories' as Subcategory,  
       'Telephoto Conversion Lens' as Product, CAST(1380.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate,'Accessories' as Subcategory,    
       'USB Cable' as Product, CAST(780.00 AS money) AS Sales, 26 as Quantity  
    UNION SELECT CAST('2009-01-08' AS date) as SalesDate, 'Accessories' as Subcategory,   
       'Budget Movie-Maker' as Product, CAST(3798.00 AS money) AS Sales, 9 as Quantity  
    UNION SELECT CAST('2009-01-09' AS date) as SalesDate, 'Camcorders' as Subcategory,   
       'Business Videographer' as Product, CAST(10400.00 AS money) AS Sales, 13 as Quantity  
    UNION SELECT CAST('2009-01-10' AS date) as SalesDate, 'Camcorders' as Subcategory,   
       'Social Videographer' as Product, CAST(3000.00 AS money) AS Sales, 60 as Quantity  
    UNION SELECT CAST('2009-01-11' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Advanced Digital' as Product, CAST(7234.50 AS money) AS Sales, 39 as Quantity  
    UNION SELECT CAST('2009-01-07' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Compact Digital' as Product, CAST(10836.00 AS money) AS Sales, 84 as Quantity  
    UNION SELECT CAST('2009-01-08' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Consumer Digital' as Product, CAST(2550.00 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Digital' as Subcategory,   
       'Slim Digital' as Product, CAST(8357.80 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2009-01-09' AS date) as SalesDate, 'Digital SLR' as Subcategory,   
       'SLR Camera 35mm' as Product, CAST(18530.00 AS money) AS Sales, 34 as Quantity  
    UNION SELECT CAST('2009-01-07' AS date) as SalesDate, 'Digital SLR' as Subcategory,   
       'SLR Camera' as Product, CAST(26576.00 AS money) AS Sales, 88 as Quantity  
  
    ```  
  
3.  在查询设计器工具栏中，单击“运行”( **!** )。  
  
    该查询运行并显示 SalesDate、Subcategory、Product、Sales 和 Quantity 字段的结果集。  
  
    在结果集中，列标题基于查询中的名称。 在数据集中，列标题会成为字段名称并保存在报表中。 完成向导后，可以使用“报表数据”窗格查看数据集字段集合。  
  
4.  单击“下一步”。  
  
## <a name="1c-organize-data-into-groups-in-the-table-wizard"></a><a name="Groups"></a>1c. 在表向导中将数据组织到组中  
在选择要进行分组的字段时，可以设计一个表格，其中的行和列显示了详细数据和聚合数据。  
  
### <a name="to-organize-data-into-groups"></a>将数据组织到组中  
  
1.  在“排列字段”页上，将 Product 拖到“值”中 。  
  
2.  将 Quantity 拖到“值”中并将其置于 Product 下方。  
  
    Quantity 由 Sum 函数（即数值字段的默认聚合函数）自动聚合。 值为 [Sum(Quantity)]。  
  
    选择 [Sum(Quantity)] 旁边的箭头，查看其他可用的聚合函数。 不要更改聚合函数。  
  
3.  将 Sales 拖到“值”中并将其置于 [Sum(Quantity)] 下方。  
  
    Sales 由 Sum 函数聚合。 值为 [Sum(Sales)]。  
  
    步骤 1、2 和 3 指定要在表中显示的数据。  
  
4.  将 SalesDate 拖到“行组”中。  
  
5.  将 Subcategory 拖到“行组”中并将其置于 SalesDate 下方。  
  
    步骤 4 和 5 首先按日期组织字段的值，然后按照该日期的产品子类别组织字段的值。  
  
6.  单击“下一步”。  
  
## <a name="1d-add-subtotal-and-total-rows-in-the-table-wizard"></a><a name="Subtotals"></a>1d. 在表向导中添加小计行和合计行  
创建组后，可以添加用于显示字段的聚合值的行并设置其格式。 可以选择是显示所有数据还是允许用户以交互方式展开和折叠已分组数据。  
  
### <a name="to-add-subtotals-and-totals"></a>添加小计和总计  
  
1.  在“选择布局”页的“选项”下，确认已选择“显示小计和总计”  。  
  
2.  验证是否选择了“分块式，小计下方显示”。  
  
    向导的“预览”窗格将显示包含有五行的表。 当您运行报表时，每行将按以下方式显示：  
  
    1.  第一行将对表重复一次以显示列标题。  
  
    2.  第二行将对销售订单中的每个行项重复一次并显示产品名称、订单数量和行总计。  
  
    3.  第三行将对每个销售订单类别重复一次以显示每个类别的小计。  
  
    4.  第四行将对每个订单日期重复一次以显示每天的小计。  
  
    5.  第五行将对表重复一次以显示总计。  
  
3.  清除“展开/折叠组”选项。 在本教程中，创建的报表不会使用明细功能（用户可通过此功能来展开父组层次结构）来显示子组行和详细信息行。  
  
4.  单击“下一步”预览表，然后单击“完成” 。  
  
表将添加到设计图面中。 该表有 5 列、5 行。 “行组”窗格显示三个行组：“SalesDate”、“Subcategory”和“Details”。 详细信息数据是由数据集查询检索的所有数据。  
  
## <a name="2-format-data-as-currency"></a><a name="FormatCurrency"></a>2.将数据格式设置为货币  
默认情况下，Sales 字段的汇总数据将显示总数。 请设置其格式，以使其显示货币形式的数字。   
  
### <a name="to-format-a-currency-field"></a>设置货币字段格式  
  
1.  若要在“设计”视图中将格式化文本框和占位符文本视为示例值，请在“开始”选项卡上的“数字”组中，单击“占位符样式”图标旁边的箭头，然后选择“示例值”。  
  
2.   单击第二行（位于列标题行下）Sales 列的单元，然后向下拖动以选定包含 `[Sum(Sales)]`的所有单元。  
  
3.  在“开始”选项卡上的“数字”组中，单击“货币”按钮  。 单元会更改为显示已设置好格式的货币。  
  
    如果区域设置为“英语(美国)”，则默认示例文本为 [ **$12,345.00**]。 如果未看到示例货币值，请在“开始”选项卡上的“数字”组中，单击“占位符样式”图标旁边的箭头，然后选择“示例值”。  
  
4.  单击 **“运行”** 以预览报表。  
  
Sales 的汇总值会以货币形式显示。  
  
## <a name="3-format-data-as-date"></a><a name="FormatDate"></a>3.将数据格式设置为日期格式  
默认情况下，SalesDate 字段会同时显示日期和时间。 您可以设置其格式，使其只显示日期。  
  
### <a name="to-format-a-date-field-as-the-default-format"></a>将日期字段设置为默认格式  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  单击包含 `[SalesDate]`的单元格。  
  
3.  在功能区的“开始”选项卡上的“数字”组中，单击箭头并选择“日期”  。  
  
    单元格会显示示例日期 **[2000/1/31]** 。 如果未看到示例日期，请在“开始”选项卡上的“数字”组中，单击“占位符样式”图标旁边的箭头，然后选择“示例值”。  
  
4.  单击 **“运行”** 以预览报表。  
  
SalesDate 值将以默认日期格式显示。  
  
### <a name="to-change-the-date-format-to-a-custom-format"></a>将日期格式更改为自定义格式  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  选择包含 `[SalesDate]`的单元格。  
  
3.  在“开始”选项卡上的“数字”组中，单击右下角的箭头，打开对话框 。  
  
    将打开“文本框属性”对话框。  
  
4.  在“类别”窗格中，确认已选中“日期”。  
  
5.  在“类型”窗格中，选择“2000 年 1 月 31 日” 。  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    单元会显示示例日期 **[2000 年 1 月 31 日]** 。  
  
7.  单击 **“运行”** 以预览报表。  
  
SalesDate 值将显示月份名而非月份数字。  
  
## <a name="4-change-column-widths"></a><a name="Width"></a>4.更改列宽  
默认情况下，表中的每个单元格都包含一个文本框。 在呈现页面时，文本框将垂直扩展以容纳文本。 在呈现的报表中，每个行将扩展到行中呈现的最高文本框的高度。 设计图面上的行的高度不会影响已呈现报表中的行的高度。  
  
若要减少每个行占用的垂直空间量，请扩展列宽以容纳单个行的列中的文本框的预计内容。  
  
### <a name="to-change-the-width-of-table-columns"></a>更改表的列宽  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  单击表，以便在此表的上方和旁边显示列控点和行控点。  
  
    沿此表的上方和一侧显示的灰色条状物就是列控点和行控点。  
  
3.  指向列控点之间的行，使光标变为双箭头。 拖动列，调整到所需列宽。 例如，展开 Product 列，以便产品名称显示在一行中。  
  
4.  单击 **“运行”** 以预览报表。  
  
## <a name="5-add-a-report-title"></a><a name="Title"></a>5.添加报表标题  
报表标题将出现在报表的顶部。 可以将报表标题置于报表表头中或置于表体顶部的文本框中（如果报表未使用表头）。 在本教程中，您将使用自动放置在表体顶部的文本框。  
  
通过将不同的字体样式、大小和颜色应用于文本的短语和单个字符，可以进一步增强文本。 有关详细信息，请参阅[设置文本框中文本的格式（报表生成器和 SSRS）](../reporting-services/report-design/format-text-in-a-text-box-report-builder-and-ssrs.md)。  
  
### <a name="to-add-a-report-title"></a>添加报表标题  
  
1.  在设计图面上，单击“单击以添加标题”。  
  
2.  键入 **Product Sales**，然后在文本框外部单击。  
  
3.  右键单击包含 Product Sales 的文本框，然后单击“文本框属性” 。  
  
4.  在“文本框属性”对话框中，单击“字体”。  
  
5.  在“大小”列表中，选择“18pt”。  
  
6.  在“颜色”列表中，选择“青蓝色” 。  
  
7.  选择“粗体”。  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="6-save-the-report"></a><a name="Save"></a>6.保存报表  
将报表保存到报表服务器或计算机上。 如果不将报表保存到报表服务器上，则许多 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 功能（如报表部件和子报表）将不可用。  
  
### <a name="to-save-the-report-on-a-report-server"></a>将报表保存到报表服务器  
  
1.  单击“文件” > “另存为” 。  
  
2.  单击 **“最近使用的站点和服务器”** 。  
  
3.  选择或键入您拥有保存报表权限的报表服务器的名称。  
  
    此时将显示“正在连接到报表服务器”消息。 当连接完成时，您将看到报表服务器管理员指定为报表默认位置的报表文件夹的内容。  
  
4.  在“名称”中，将“无标题”替换为“Product_Sales”  。  
  
5.  单击“ **保存**”。  
  
报表即已保存至报表服务器。 您连接的报表服务器的名称将显示在窗口底部的状态栏中。  
  
### <a name="to-save-the-report-on-your-computer"></a>将报表保存到计算机上  
  
1.  单击“文件” > “另存为” 。  
  
2.  依次单击“桌面”、“我的文档”或“我的电脑”，并浏览到要保存该报表的文件夹  。  
  
3.  在“名称”中，将“无标题”替换为“Product Sales”  。  
  
4.  单击“ **保存**”。  
  
## <a name="7-export-the-report"></a><a name="Export"></a>7.导出报表  
可以将报表导出为不同的格式，如 Microsoft Excel 和逗号分隔值 (CSV) 文件。 有关详细信息，请参阅 [导出报表（报表生成器和 SSRS）](../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md)中用于控制分页的规则。  
  
在本教程中，您将报表导出为 Excel 格式，并设置报表的属性以便为工作簿选项卡提供自定义名称。  
  
### <a name="to-specify-the-workbook-tab-name"></a>指定工作簿选项卡名称  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  单击设计图面上报表外的任何位置。  
  
3.  在“属性”窗格中，找到“InitialPageName”属性并键入 **Product Sales Excel**。  
  
    > [!NOTE]  
    > 如果“属性”窗格不可见，请在 **“视图”** 选项卡上选择 **“属性”** 。  
    > 如果未在属性窗格中看到属性，请尝试选择窗格顶部的“按字母顺序”按钮，将所有属性按字母顺序排序。   
  
### <a name="to-export-a-report-to-excel"></a>将报表导出为 Excel 格式  
  
1.  单击 **“运行”** 以预览报表。  
  
2.  在功能区上，单击“导出” > “Excel” 。  
  
    此时将打开。  
  
3.  在“另存为”对话框中，浏览到要保存该文件的位置。  
  
4.  在“文件名”文本框中，键入“Product_Sales_Excel” 。  
  
5.  确认文件类型为 Excel (\*.xlsx)。  
  
6.  单击“ **保存**”。  
  
### <a name="to-view-the-report-in-excel"></a>在 Excel 中查看报表  
  
1.  打开保存该工作簿的文件夹，并双击 **Product_Sales_Excel.xlsx**。  
  
2.  验证工作簿选项卡的名称是否为 **Product Sales Excel**。  
  
## <a name="next-steps"></a>后续步骤  
到此为止，我们结束了有关如何创建基本表格报表的演练。 有关表的详细信息，请参阅[表、矩阵和列表（报表生成器和 SSRS）](../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)。  
  
## <a name="see-also"></a>另请参阅  
[报表生成器教程](../reporting-services/report-builder-tutorials.md)  
[SQL Server 中的报表生成器](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  

