---
title: 教程：创建矩阵报表（报表生成器）| Microsoft Docs
description: 了解如何使用嵌套行组和列组中的示例销售数据的矩阵创建 Reporting Services 分页报表。
ms.date: 06/23/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 9ee19c2e-2a8c-4bb0-9274-04a5812c2e96
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a0b49b2cf9ac1e131e74ba39c1430492823e39e5
ms.sourcegitcommit: 9e2c682929ee64c051dc62f8917d147861f7c635
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043788"
---
# <a name="tutorial-creating-a-matrix-report-report-builder"></a>教程：生成矩阵报表（报表生成器）
本教程教你如何使用嵌套行组和列组中的示例销售数据的矩阵组件创建 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 分页报表。 

你还可以创建相邻列组、设置列的格式和旋转文本。 下图显示与您将创建的报表类似的报表。  
  
![报表生成器矩阵报表的屏幕截图。](../reporting-services/media/report-builder-matrix-tutorial.png)
   
完成本教程的估计时间：20 分钟。  
  
## <a name="requirements"></a>要求  
有关要求的信息，请参阅 [教程先决条件](../reporting-services/prerequisites-for-tutorials-report-builder.md)。 
  
## <a name="1-create-a-matrix-report-and-dataset-from-the-new-table-or-matrix-wizard"></a><a name="CreateMatrix"></a>1.使用新的表或矩阵向导创建矩阵报表和数据集  
在本部分中，选择共享数据源，创建嵌入数据集，并在矩阵报表中显示数据。  
  
> [!NOTE]  
> 在本教程中，由于查询已经包含了数据值，因此它不需要外部数据源。 这样，查询就会非常长。 在业务环境中，查询不会包含数据。 本教程中的查询仅供学习使用。  
  
### <a name="to-create-a-matrix"></a>创建矩阵  
  
1.  通过计算机、[Web 门户或 SharePoint 集成模式](../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 。  
  
    将打开“新建报表或数据集”对话框。  
  
    如果未出现“新建报表或数据集”对话框，请通过“文件”菜单转至“新建”  。  
  
2.  在左窗格中，确认已选中 **“新建报表”** 。  
  
3.  在右窗格中，单击“表或矩阵向导”。  
  
4.  在“选择数据集”页上，单击“创建数据集” 。  
  
5.  单击“下一步”。  
  
6.  在“选择数据源的连接”页上，选择现有数据源或浏览到报表服务器并选择一个数据源。 如果没有可用数据源，或您无权访问报表服务器，您可以改用嵌入数据源。 若要了解如何创建嵌入数据源，请参阅[教程：生成基本表报表（报表生成器）](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md)。  
  
7.  单击“下一步”。  
  
8.  在“设计查询”页上，单击“编辑为文本” 。  
  
9. 复制并将以下查询粘贴到查询窗格中：  
  
    ```  
    SELECT CAST('2015-01-05' AS date) as SalesDate, 'Central' as Territory, 'Accessories' as Subcategory,'Carrying Case' as Product, CAST(16996.60 AS money) AS Sales, 68 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'North' as Territory, 'Accessories' as Subcategory, 'Carrying Case' as Product, CAST(13747.25 AS money) AS Sales, 55 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'South' as Territory, 'Accessories' as Subcategory,'Carrying Case' as Product, CAST(9248.15 AS money) As Sales, 37 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Central' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(1350.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'North' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(1800.00 AS money) AS Sales, 24 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'South' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(1125.00 AS money) AS Sales, 15 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Central' as Territory, 'Accessories' as Subcategory,'Lens Adapter' as Product, CAST(1147.50 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'North' as Territory, 'Accessories' as Subcategory,  'Lens Adapter' as Product, CAST(742.50 AS money) AS Sales, 11 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'South' as Territory, 'Accessories' as Subcategory,'Lens Adapter' as Product, CAST(1417.50 AS money) AS Sales, 21 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Central' as Territory, 'Accessories' as Subcategory, 'Carrying Case' as Product, CAST(13497.30 AS money) AS Sales, 54 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'North' as Territory, 'Accessories' as Subcategory, 'Carrying Case' as Product, CAST(11997.60 AS money) AS Sales, 48 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'South' as Territory, 'Accessories' as Subcategory, 'Carrying Case' as Product, CAST(10247.95 AS money) As Sales, 41 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Central' as Territory, 'Accessories' as Subcategory, 'Tripod' as Product, CAST(1200.00 AS money) AS Sales, 16 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'North' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(2025.00 AS money) AS Sales, 27 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'South' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(1425.00 AS money) AS Sales, 19 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Central' as Territory, 'Accessories' as Subcategory,'Lens Adapter' as Product, CAST(887.50 AS money) AS Sales, 13 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'North' as Territory, 'Accessories' as Subcategory, 'Lens Adapter' as Product, CAST(607.50 AS money) AS Sales, 9 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'South' as Territory, 'Accessories' as Subcategory,'Lens Adapter' as Product, CAST(1215.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate,  'Central' as Territory, 'Digital' as Subcategory,'Compact Digital' as Product, CAST(10191.00 AS money) AS Sales, 79 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate,  'North' as Territory, 'Digital' as Subcategory, 'Compact Digital' as Product, CAST(8772.00 AS money) AS Sales, 68 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate,  'South' as Territory, 'Digital' as Subcategory, 'Compact Digital' as Product, CAST(10578.00 AS money) AS Sales, 82 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Central' as Territory,'Digital' as Subcategory, 'Slim Digital' as Product, CAST(7218.10 AS money) AS Sales, 38 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'North' as Territory,'Digital' as Subcategory, 'Slim Digital' as Product, CAST(8357.80 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'South' as Territory,'Digital' as Subcategory,'Slim Digital' as Product, CAST(9307.55 AS money) AS Sales, 49 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,  'Central' as Territory, 'Digital' as Subcategory,'Compact Digital' as Product, CAST(3870.00 AS money) AS Sales, 30 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,  'North' as Territory, 'Digital' as Subcategory,'Compact Digital' as Product, CAST(5805.00 AS money) AS Sales, 45 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,  'South' as Territory, 'Digital' as Subcategory, 'Compact Digital' as Product, CAST(8643.00 AS money) AS Sales, 67 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Central' as Territory, 'Digital' as Subcategory, 'Slim Digital' as Product, CAST(9877.40 AS money) AS Sales, 52 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'North' as Territory, 'Digital' as Subcategory, 'Slim Digital' as Product, CAST(12536.70 AS money) AS Sales, 66 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'South' as Territory, 'Digital' as Subcategory, 'Slim Digital' as Product, CAST(6648.25 AS money) AS Sales, 35 as Quantity  
    ```  
  
10. （可选）单击运行图标 (!) 以运行查询并查看数据。

11. 单击“下一步”。  
  
## <a name="2-organize-data-and-choose-layout-from-the-new-table-or-matrix-wizard"></a><a name="Groups"></a>2.使用新的表或矩阵向导组织数据并选择布局  
使用此向导可提供用于显示数据的起始设计。 此向导中的预览窗格可帮助您在完成矩阵设计之前展现对数据进行分组的结果。  
  
1.  在“排列字段”页上，将 Territory 从“可用字段”拖到“行组”中。  
  
2.  将 SalesDate 拖到“行组”中并将其放在 Territory 下面。  
  
    “行组”中字段的列出顺序定义组层次结构。 步骤 1 和 2 首先按地区组织字段的值，然后按照销售日期组织字段的值。  
  
3.  将 Subcategory 拖到“列组”中。  
  
4.  将 Product 拖到“列组”中，然后将其放在 Subcategory 下面。  
  
    “列组”中字段的列出顺序定义了组层次结构。 步骤 3 和 4 首先按子类别组织字段的值，然后按照产品组织字段的值。  
  
5.  将 Sales 拖到“值”中。  
  
    将使用汇总数值字段的默认函数 Sum 函数对 Sales 进行汇总。  
  
6.  将 Quantity 拖到“值”中。  
  
    Quantity 使用 Sum 函数进行汇总。  
  
    步骤 5 和 6 指定要在矩阵数据单元中显示的数据。
    
    ![显示报表生成器排列字段报表向导的屏幕截图。](../reporting-services/media/report-builder-arrange-fields-report-wizard.png)  
  
7.  单击“下一步”。  
  
8.  在“选择布局”页的“选项”下，确认已选择“显示小计和总计” 。  
  
9. 验证是否选择了“分块式，小计下方显示”。  
  
10. 确认选择了“展开/折叠组”选项。  
  
11. 单击“下一步”。  
  
13. 单击“完成”。  
  
    矩阵将添加到设计图面中。 “行组”窗格显示两个行组：“Territory”和“SalesDate”。 “列组”窗格显示两个列组：“Subcategory”和“Product”。 详细信息数据是由数据集查询检索的所有数据。  
    
    ![显示报表生成器“行组”和“列组”的屏幕截图。](../reporting-services/media/report-builder-row-and-column-groups.png)
  
14. 单击 **“运行”** 以预览报表。  
  
    对于在特定日期销售的每个产品，该矩阵显示产品所属于的子类别以及销售地区。  

14. 展开子类别。 可以看到报表立刻变得很宽。

![显示展开的报表生成器矩阵报表的屏幕截图。](../reporting-services/media/report-builder-expand-matrix.png)
  
## <a name="3-format-data"></a><a name="FormatData"></a>3.设置数据格式  
默认情况下，Sales 字段的汇总数据显示一般数字，而 SalesDate 字段则显示日期和时间信息。 在本节中，设置 Sales 字段格式以便将数字显示为货币，并且设置 SalesDate 字段格式以便只显示日期。 切换“占位符样式”，将格式化的文本框和占位符文本显示为示例值。  
  
### <a name="to-format-fields"></a>设置字段格式  
  
1.  单击 **“设计”** 切换到设计视图。  
  
2.  按 Ctrl 键，然后选择包含 `[Sum(Sales)]`的九个单元。  
  
3.  在“主文件夹”选项卡中，单击“数字” > “货币”  。 单元会更改为显示已设置好格式的货币。  
  
    如果区域设置为“英语(美国)”，则默认示例文本为 [ **$12,345.00** ]。 如果看不到示例货币值，请在“数字”组中单击“占位符样式” > “示例值”  。  
    
    ![报表生成器“示例值”选项的屏幕截图。](../reporting-services/media/report-builder-placeholder-value.png)
  
4.  单击包含 `[SalesDate]`的单元格。  
  
5.  在“数字”组中转至“日期” 。  
  
    单元格会显示示例日期 **[2000/1/31]** 。 如果看不到示例日期，请单击“数字”组中的“占位符样式”，然后单击“示例值”。  
  
6.  单击 **“运行”** 以预览报表。  
  
日期值仅显示日期，销售值显示为货币。  
  
## <a name="4-add-adjacent-column-group"></a><a name="AdjacentGroup"></a>4.添加相邻列组  
可以在父子关系中嵌套行组和列组，或者在同级关系中保持它们相邻。  
  
在本节中，添加与 Subcategory 列组相邻的列组，复制单元以便填充这个新的列组，然后使用表达式创建该列组标题的值。  
  
### <a name="to-add-an-adjacent-column-group"></a>添加相邻列组  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  右键单击包含 `[Subcategory]` 的单元，指向“添加组”，然后单击“右侧相邻”。  
  
    此时将打开 **“Tablix 组”** 对话框。  
  
3.  在“分组依据”列表中选择 SalesDate，然后单击“确定”。  
  
    一个新的列组被添加到 Subcategory 列组的右侧。  
  
4.  右键单击新列组中包含 `[SalesDate],` 的单元，然后单击“表达式”。  
  
5.  将以下表达式复制到“表达式”框中。  
  
    ```  
    =WeekdayName(DatePart("w",Fields!SalesDate.Value))  
    ```  
  
    该表达式从销售日期中提取工作日名称。 有关详细信息，请参阅[表达式（报表生成器和 SSRS）](../reporting-services/report-design/expressions-report-builder-and-ssrs.md)。  
  
6.  右键单击 Subcategory 列组中包含 Total 的单元，然后单击“复制”。  
  
7.  右键单击包含在步骤 5 中创建的表达式的单元紧下方的单元，然后单击“粘贴”。  
  
8.  按 Ctrl 键。  
  
9. 在 Subcategory 组中，单击 Sales 列标题以及标题下的三个单元，右键单击，然后单击“复制”。  
  
10. 将这四个单元粘贴到新列组中的四个空单元中。  
  
11. 单击 **“运行”** 以预览报表。  
  
报表包括名为 Monday 和 Tuesday 的列。 数据集仅包含针对这两天的数据。  

![显示在报表生成器矩阵报表中显示的工作日的屏幕截图。](../reporting-services/media/report-builder-matrix-weekdays.png)
  
> [!NOTE]  
> 如果数据包括了其他天，则报表也将包括这些天的相应列。 每个列都具有列标题 **Sales** 以及按地区统计的销售总额。  
  
## <a name="5-change-column-widths"></a><a name="Width"></a>5.更改列宽  
包括矩阵的报表填充以水平方式展开，并且在运行时以垂直方式展开。 如果您计划将数据导出到用于打印报表的格式（例如 Microsoft Word 或 Adobe PDF），则控制水平展开将特别重要。 如果报表跨多页水平展开，则打印报表将很难理解。 为了尽量缩小水平展开，您可以将列的大小调整为宽度仅供无需换行就可以显示数据。 您还可以重命名列，以便其标题适合显示数据所需的宽度。  
  
### <a name="to-rename-and-resize-the-columns"></a>重命名列和调整列的大小  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  选择距左侧最远的 Quantity 列中的文本，然后键入“QTY”。  
  
    列标题现在是 QTY。  
  
3.  对于名为 Quantity 的其他两列，重复执行步骤 2。
  
4.  单击矩阵，以便在此矩阵的上方和旁边显示列控点和行控点。  
  
    沿此表的上方和一侧显示的灰色条状物就是列控点和行控点。  
    
    ![报表生成器矩阵报表中的列图柄和行图柄的屏幕截图。](../reporting-services/media/report-builder-column-handles.png)
  
5.  若要将 QTY 列向最左侧调整，请指向列控点之间的线条，使光标变为双箭头。 将列向左侧拖动，直到宽度为 1/2 英寸。  
  
    1/2 英寸的列宽足以显示数量了。  
  
6.  对于名为 QTY 的其他列，重复执行步骤 5。  
  
7.  单击 **“运行”** 以预览报表。  
  
包含数量的列现在变窄，并且命名为 QTY。  
  
## <a name="6-merge-matrix-cells"></a><a name="MergeCells"></a>6.合并矩阵单元  
角区域是矩阵的左上角。 根据矩阵中行组和列组的数目，角区域中单元的数目将有所不同。 在本教程中内置的矩阵在其角区域中具有四个单元。 单元按两行和两列排列，反映行和列组层次结构的深度。 这四个单元并不用于此报表，并且您要将它们合并为一个单元。  
  
### <a name="to-merge-matrix-cells"></a>合并矩阵单元  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  单击矩阵，以便在此矩阵的上方和旁边显示列控点和行控点。  
  
3.  按 Ctrl 键，选择四个角单元。  
  
4.  右键单击所选单元，然后单击“合并单元”。  
  
5.  右键单击新合并的单元，然后单击“文本框属性”。  
  
6.  在“边框”选项卡中，转至“预设” > “无”。
  
9. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
10. 单击 **“运行”** 以预览报表。  
  
不再显示矩阵左上角的单元格。 
  
## <a name="7-add-a-report-header-and-report-title"></a><a name="HeaderTitle"></a>7.添加报表表头和报表标题  
报表标题将出现在报表的顶部。 可以将报表标题置于报表表头中或置于表体顶部的文本框中（如果报表未使用表头）。 在本教程中，您将删除报表顶部的文本框，并向表头中添加主题。  
  
### <a name="to-add-a-report-header-and-report-title"></a>添加报表表头和报表标题  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  选择表体顶部包含“单击以添加标题”的文本框，然后按 Delete 键。  
  
3.  在“插入”选项卡中，转至“表头” > “添加表头”。  
  
    页眉将添加到表体的顶部。  
  
4.  在“插入”选项卡中，单击“文本框”，然后将某一文本框拖到报表表头内。 使文本框大约 6 英寸长、3/4 英寸高，并且将其放置于表头的左侧。  
  
5.  在文本框中，键入“Sales by Territory, Subcategory, and Day”。  
  
6.  选择键入的文本，在“主页”选项卡中，转至“字体”：
    * **大小为 24 pt**
    * **颜色为褐红色**
 
10. 单击 **“运行”** 以预览报表。  
  
报表在表头中包含一个报表标题。  
  
## <a name="8-save-the-report"></a><a name="Save"></a>8.保存报表  
您可以将报表保存到报表服务器、SharePoint 库或本地计算机。  
  
在本教程中，将报表保存到报表服务器。 如果您没有对报表服务器的访问权限，则可以保存到您的计算机。  
  
### <a name="to-save-the-report-on-a-report-server"></a>将报表保存到报表服务器  
  
1.  从 **“报表生成器”** 按钮，单击 **“另存为”** 。  
  
2.  单击 **“最近使用的站点和服务器”** 。  
  
3.  选择或键入您拥有保存报表权限的报表服务器的名称。  
  
    此时将显示“正在连接到报表服务器”消息。 当连接完成时，您将看到报表服务器管理员指定为默认报表位置的报表文件夹的内容。  
  
4.  在“名称”中，用“SalesByTerritorySubcategory”替换默认名称。  
  
5.  单击“ **保存** ”。  
  
报表即已保存至报表服务器。 您连接的报表服务器的名称将显示在窗口底部的状态栏中。  
  
#### <a name="to-save-the-report-on-your-computer"></a>将报表保存到计算机上  
  
1.  从 **“报表生成器”** 按钮，单击 **“另存为”** 。  
  
2.  单击 **“桌面”** 、 **“我的文档”** 或 **“我的电脑”** ，然后浏览到要将报表保存到的文件夹。  
  
3.  在“名称”中，用“SalesByTerritorySubcategory”替换默认名称。  
  
4.  单击“ **保存** ”。  
  
## <a name="9-optional-rotate-text-box-270-degrees"></a><a name="RotateTextBox"></a>9.（可选）将文本框旋转 270 度  
具有矩阵的报表在运行时可以垂直方式和水平方式展开。 通过垂直旋转文本框或者旋转 270 度，您可以节约水平空间。 呈现的报表然后将更窄，并且如果导出到 Microsoft Word 之类的格式，报表将更有可能适合打印页面。  
  
文本框还可以将文本显示为竖排（从上到下）。 有关详细信息，请参阅[文本框（报表生成器和 SSRS）](../reporting-services/report-design/text-boxes-report-builder-and-ssrs.md)。  
  
### <a name="to-rotate-text-box-270-degrees"></a>旋转文本框 270 度  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  选择包含 的单元格。 `[Territory].` 

    >**注意** ：选择单元格，而不是文本。 WritingMode 属性仅可用于该单元格。
    
     ![显示报表生成器矩阵报表中“Territory”单元格的屏幕截图。](../reporting-services/media/report-builder-select-territory-cell.png)
  
3.  在“属性”窗格中，找到 WritingMode 属性，并将其值从“默认”更改为“Rotate270”。  
  
    如果“属性”窗格未打开，请单击功能区的“视图”选项卡，然后选择“属性”。  
  
4.  请确认 CanGrow 属性设置为“True”。  
  
5.  在“开始”选项卡上的“段落”部分中，选择“垂直居中”和“水平居中”，以便从垂直和水平方向上都将文本定位于单元格的中心。  
 
6. 将 Territory 列的大小调整为 1/2 英寸宽，并且删除列标题。  
6.  单击 **“运行”** 以预览报表。  
  
地区名称垂直书写，从下到上。 Territory 行组的高度由地区名称的长度决定。  
  
## <a name="next-steps"></a>后续步骤  
有关如何创建矩阵报表的教程到此结束。 有关矩阵报表的详细信息，请参阅： 
-    [表、矩阵和列表](../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)
-    [创建矩阵](../reporting-services/report-design/create-a-matrix-report-builder-and-ssrs.md)
-    [Tablix 数据区域](../reporting-services/report-design/tablix-data-region-areas-report-builder-and-ssrs.md) 
-    [Tablix 数据区域单元、行和列](../reporting-services/report-design/tablix-data-region-cells-rows-and-columns-report-builder-and-ssrs.md)  
  
## <a name="see-also"></a>另请参阅  
[报表生成器教程](../reporting-services/report-builder-tutorials.md)  
[SQL Server 中的报表生成器](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  

