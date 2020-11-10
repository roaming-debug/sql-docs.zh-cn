---
title: 教程：向报表添加 KPI（报表生成器）| Microsoft Docs
description: 了解如何向报表生成器中的 Reporting Services 分页报表添加关键绩效指标 (KPI)。
ms.date: 06/15/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 1bf77859-0b33-4f40-abaf-ebeeb6ebb1f8
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f4802ee3beb72e34ed2b85e8857ac69c61557018
ms.sourcegitcommit: 9e2c682929ee64c051dc62f8917d147861f7c635
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043709"
---
# <a name="tutorial-adding-a-kpi-to-your-report-report-builder"></a>教程：向报表添加 KPI（报表生成器）
在 [!INCLUDE[ssRBnoversion_md](../includes/ssrbnoversion.md)] 教程中，向 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 分页报表添加关键绩效指标 (KPI)。  

KPI 是对业务重要性的可测量值。 在本教程中，按产品子类别进行的销售汇总为 KPI。 可以使用颜色、仪表和指示器来显示 KPI 的当前状态。
  
下图与将要创建的报表类似。  
  
![报表生成器 KPI 报表的屏幕截图。](../reporting-services/media/report-builder-kpi-report.png)
    
> [!NOTE]  
> 在本教程中，将向导的多个步骤合并为两个过程：一个用于创建数据集，一个用于创建表。 有关如何浏览到报表服务器、选择数据源、创建数据集和运行向导的分步说明，请参阅本系列的第一个教程：[教程：创建基本表报表（报表生成器）](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md)。  
  
完成本教程的估计时间：15 分钟。  
  
## <a name="requirements"></a>要求  
有关要求的信息，请参阅[教程先决条件（报表生成器）](../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
## <a name="1-create-a-table-report-and-dataset-from-the-table-or-matrix-wizard"></a><a name="Table"></a>1.使用表向导或矩阵向导创建表报表和数据集  
在本部分中，选择共享数据源，创建嵌入数据集，并在表中显示数据。  
 
### <a name="to-create-a-table-with-an-embedded-dataset"></a>创建具有嵌入数据集的表  
  
1.  通过计算机、[Web 门户或 SharePoint 集成模式](../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 。  
  
    将打开“新建报表或数据集”对话框。  
  
    如果未出现“新建报表或数据集”对话框，请通过“文件”菜单转至“新建”  。  
  
2.  在左窗格中，确认已选中 **“新建报表”** 。  
  
3.  在右窗格中，单击“表或矩阵向导”。  
  
4.  在“选择数据集”页上，单击“创建数据集” 。  
  
5.  单击“下一步”。  
  
6.  在“选择数据源的连接”页上，选择现有数据源或浏览到报表服务器并选择一个数据源。 如果没有可用数据源，或无权访问报表服务器，可以改用嵌入数据源。 有关详细信息，请参阅[教程：生成基本表报表（报表生成器）](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md)。  
  
7.  单击“下一步”。  
  
8.  在“设计查询”页上，单击“编辑为文本” 。  
  
9. 复制并将以下查询粘贴到查询窗格中：  

    > [!NOTE]  
    > 在本教程中，由于查询包含了数据值，因此它不需要外部数据源。 这样，查询就会非常长。 在业务环境中，查询不会包含数据。 本教程中的查询仅供学习使用。  
  
    ```  
    SELECT CAST('2015-01-05' AS date) as SalesDate, 'Accessories' as Subcategory,   
       'Carrying Case' as Product, CAST(16996.60 AS money) AS Sales, 68 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Tripod' as Product, CAST(1350.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2015-01-11' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Lens Adapter' as Product, CAST(1147.50 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Mini Battery Charger' as Product, CAST(1056.00 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,  'Accessories' as Subcategory,  
       'Telephoto Conversion Lens' as Product, CAST(1380.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,'Accessories' as Subcategory,    
       'USB Cable' as Product, CAST(780.00 AS money) AS Sales, 26 as Quantity  
    UNION SELECT CAST('2015-01-08' AS date) as SalesDate, 'Accessories' as Subcategory,   
       'Budget Movie-Maker' as Product, CAST(3798.00 AS money) AS Sales, 9 as Quantity  
    UNION SELECT CAST('2015-01-09' AS date) as SalesDate, 'Camcorders' as Subcategory,   
       'Business Videographer' as Product, CAST(10400.00 AS money) AS Sales, 13 as Quantity  
    UNION SELECT CAST('2015-01-10' AS date) as SalesDate, 'Camcorders' as Subcategory,   
       'Social Videographer' as Product, CAST(3000.00 AS money) AS Sales, 60 as Quantity  
    UNION SELECT CAST('2015-01-11' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Advanced Digital' as Product, CAST(7234.50 AS money) AS Sales, 39 as Quantity  
    UNION SELECT CAST('2015-01-07' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Compact Digital' as Product, CAST(10836.00 AS money) AS Sales, 84 as Quantity  
    UNION SELECT CAST('2015-01-08' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Consumer Digital' as Product, CAST(2550.00 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Digital' as Subcategory,   
       'Slim Digital' as Product, CAST(8357.80 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2015-01-09' AS date) as SalesDate, 'Digital SLR' as Subcategory,   
       'SLR Camera 35mm' as Product, CAST(18530.00 AS money) AS Sales, 34 as Quantity  
    UNION SELECT CAST('2015-01-07' AS date) as SalesDate, 'Digital SLR' as Subcategory,   
       'SLR Camera' as Product, CAST(26576.00 AS money) AS Sales, 88 as Quantity  
    ```  
  
10. 在查询设计器工具栏中，单击“运行”( **!** )。

11. 单击“下一步”。  
  
## <a name="2-organize-data-and-choose-layout-in-the-wizard"></a><a name="CompleteWizard"></a>2.在向导中组织数据并选择布局  
表或矩阵向导提供一个初始设计以在其中显示数据。 此向导中的预览窗格可帮助您在完成表或矩阵设计之前将对数据进行分组的结果可视化。  
  
### <a name="to-organize-data-into-groups-and-choose-a-layout"></a>将数据组织到组中并选择布局 
  
1.  在“排列字段”页上，将 Product 拖到“值”中。  
  
2.  将 Quantity 拖到“值”中并将其置于 Product 下方。  
  
    将使用汇总数值字段的默认函数 Sum 函数对 Quantity 进行汇总。  
  
3.  将 Sales 拖到“值”中并将其放在 Quantity 下面。  
  
    步骤 1、2 和 3 指定要在表中显示的数据。  
  
4.  将 SalesDate 拖到“行组”中。  
  
5.  将 Subcategory 拖到“行组”中并将其置于 SalesDate 下方。  
  
    步骤 4 和 5 首先按日期组织字段的值，然后按照该日期的所有销售组织字段的值。  
  
6.  单击“下一步”。  
  
    当您运行报表时，表将显示每个日期、每个日期的所有订单以及每个订单的所有产品、数量和销售额总计。  
  
7.  在“选择布局”页的“选项”下，确认已选择“显示小计和总计” 。  
  
8.  验证是否选择了“分块式，小计下方显示”。  
  
9. 清除“展开/折叠组”选项。  
  
    在本教程中，创建的报表不会使用明细功能（用户可通过此功能来展开父组层次结构）来显示子组行和详细信息行。  
  
10. 单击“下一步”。  
  
11. 单击“完成”。  
  
      表将添加到设计图面中。 此表有五个列和五个行。 “行组”窗格显示三个行组：“SalesDate”、“Subcategory”和“Details”。 详细信息数据是由数据集查询检索的所有数据。 “列组”窗格为空。  
      
      ![显示行组的屏幕截图。](../reporting-services/media/report-builder-kpi-row-groups.png)
  
12. 单击 **“运行”** 以预览报表。  
  
对于在特定日期销售的每个产品，表显示产品名称、销售数量及销售额总计。 此数据首先按销售日期组织，然后按子类别组织。 

![显示基本报表生成器 KPI 表的屏幕截图。](../reporting-services/media/report-builder-kpi-basic-table.png)
    
### <a name="format-dates-and-currency"></a>设置日期和货币的格式
将列加宽，并设置日期和货币的格式。

1. 单击“设计”返回“设计”视图。

2. Product 名称可以使用更多空间。 若要将 Product 列加宽，请选择整个表，然后拖动 Product 列顶部列句柄的右边缘。

3. 按 Ctrl 键，然后选择包含 [Sum(Sales)] 的 4 个单元。

4. 在“主文件夹”选项卡中，单击“数字” > “货币”  。 单元会更改为显示已设置好格式的货币。

   如果区域设置为“英语(美国)”，则默认示例文本为 [$12,345.00]。 如果看不到示例货币值，请在“数字”组中单击“占位符样式” > “示例值”  。
    
    ![已选择报表生成器示例值选项的屏幕截图。](../reporting-services/media/report-builder-placeholder-value-button.png)

5. （可选）在“开始”选项卡上的“数字”组中，单击两次“减少小数位数”按钮，显示不带美分的美元数字  。

6. 单击包含 [SalesDate] 的单元格。

6. 在“数字”组中转至“日期” 。

   单元会显示示例日期 [1/31/2000]。 

12. 单击 **“运行”** 以预览报表。  
 
![显示格式化数字的报表生成器 KPI 报表的屏幕截图。](../reporting-services/media/report-builder-kpi-format-numbers.png)

## <a name="3-use-background-colors-to-display-a-kpi"></a><a name="BackgroundColors"></a>3.使用背景色显示 KPI  
可将背景色设置为运行报表时计算的表达式。  
  
### <a name="to-display-the-present-state-of-a-kpi-by-using-background-colors"></a>使用背景色显示 KPI 的当前状态  
  
1.  在表中，右键单击第二个 `[Sum(Sales)]` 单元（在显示子类别的销售额的小计行中），然后单击“文本框属性”。 

    请确保选择该单元，而非单元中的文本，以查看“文本框属性”。 
    
    ![报表生成器文本框属性选项的屏幕截图。](../reporting-services/media/report-builder-text-box-properties.png)
  
2.  在“填充”选项卡上，单击“填充颜色”选项旁边的“fx”按钮，然后在“为以下项设置表达式: BackgroundColor”字段中输入以下表达式：  
  
    `=IIF(Sum(Fields!Sales.Value) >= 5000 ,"Lime", IIF(Sum(Fields!Sales.Value) < 2500, "Red","Yellow"))`  
  
     `[Sum(Sales)]` 的聚合汇总大于或等于 5000 的每个单元，其背景色会变为“暗黄绿色”。 2500 和 5000 之间的 `[Sum(Sales)]` 值会变为“黄色”。 小于 2500 的值会变为“红色”。  
  
1.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
2.  单击 **“运行”** 以预览报表。  
  
在显示子类别销售额的小计行中，根据销售额总计的值，单元的背景色为红色、黄色或绿色。  

![报表生成器 KPI 报表的屏幕截图，其中某些单元格已着色。](../reporting-services/media/report-builder-kpi-colors.png)
  
## <a name="4-display-a-kpi-by-using-a-gauge"></a><a name="Gauge"></a>4.使用仪表显示 KPI  
仪表显示数据集中的单个值。 本教程使用水平线性仪表，因为即使是在该仪表较小或在表单元内使用的情况下，其形状和简便性也使其易于读取。 有关详细信息，请参阅 [仪表（报表生成器和 SSRS）](../reporting-services/report-design/gauges-report-builder-and-ssrs.md)。  
  
### <a name="to-display-the-present-state-of-a-kpi-using-a-gauge"></a>使用仪表显示 KPI 的当前状态  
  
1.  切换回“设计”视图。  
  
2.  在表中，右键单击“Sales”列的列句柄，选择“插入列” > “右侧插入” 。 此时将向表添加一个新列。  

    ![显示如何将列插入到报表生成器 KPI 报表的屏幕截图。](../reporting-services/media/report-builder-kpi-insert-column.png)
  
3.  在列标题中键入 **Linear KPI** 。  
  
4.  在“插入”选项卡中，转至“数据可视化” > “仪表”，然后单击表外的设计图面  。   
  
5.  在“选择仪表类型”对话框中，选择第一个线性仪表类型 -“水平” 。  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    此时将向设计图面添加一个仪表。  
  
7.  从“报表数据”窗格中的数据集将 `Sales` 字段拖至该仪表。 将打开 **“仪表数据”** 窗格。  
  
    将 `Sales` 字段拖放到仪表中时，该字段将出现在“值”列表中，并对其使用内置 Sum 函数进行聚合。  
   
    ![显示如何将“销售额”字段拖到报表生成器 KPI 报表中的屏幕截图。](../reporting-services/media/report-builder-kpi-drag-sales-field.png)
   
9. 在“仪表数据”窗格中，单击“LinearPointer1”旁边的箭头 > “指针属性”  。  
  
10. 在“线性指针属性”对话框中，依次单击“指针选项”选项卡、“指针类型”，请确保“栏”处于选中状态   。 
 
11. 单击“确定”。  
  
12. 右键单击仪表中的刻度，然后单击“刻度属性”。  
  
13. 在“线性刻度属性”对话框中，单击“常规”选项卡，将“最大值”设置为 25000  。  

    > [!NOTE]  
    > 可以使用一个表达式动态计算“最大值”选项的值来代替常数（如 25000）。 该表达式将使用聚合功能的聚合，类似于表达式 `=Max(Sum(Fields!Sales.value), "Tablix1")`。  

14. 在“标签”选项卡上，勾选“隐藏刻度标签” 。

15. 单击“确定”。
  
14. 将表内的仪表拖到 Linear KPI 列中的第二个空单元格，在显示 `Subcategory` 字段的销售额小计的行中，此字段旁可添加背景色公式。  
  
    > [!NOTE]  
    > 可能需要调整列大小，以使单元能够容纳水平线性仪表。 若要调整列大小，请选择此表，并拖动列句柄。 调整报表设计图面大小以适合表。  
  
15. 单击 **“运行”** 以预览报表。  
  
    仪表中绿条的水平长度将根据 KPI 的值变化。  
  
![显示添加到报表生成器 KPI 报表的线性 KPI 列的屏幕截图。](../reporting-services/media/report-builder-linear-kpi.png) 
  
## <a name="5-display-a-kpi-by-using-an-indicator"></a><a name="Indicator"></a>5.使用指示器显示 KPI  
指示器是以直观的形式传递数据值的小巧而简单的仪表。 由于指示器尺寸较小且具有简便性，其经常被用于表和矩阵中。 有关详细信息，请参阅[指示器（报表生成器和 SSRS）](../reporting-services/report-design/indicators-report-builder-and-ssrs.md)。  
  
### <a name="to-display-the-present-state-of-a-kpi-using-an-indicator"></a>使用指示器显示 KPI 的当前状态  
  
1.  切换到“设计”视图。  
  
2.  在表中，右键单击“Linear KPI”列的列句柄，此列在上一个过程中通过选择“插入列” > “右侧插入”添加 。 此时将向表添加一个新列。  
  
3.  在列标题中键入 **Stoplight KPI** 。  
  
4.  单击子类别小计的单元格（上一个过程中添加的线性仪表旁）。  
  
5.  在“插入”选项卡中，单击“数据可视化”，然后双击“指示器”  。  
  
6.  在“选择指示器类型”对话框的“形状”下，选择第一个形状类型 -“3 个交通灯(无边框)”  。  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    将该指示器添加到新的“Stoplight KPI”列中的单元格。  
  
8.  右键单击指示器，然后单击“指示器属性”。  
  
9. 在“值和状态”选项卡上的“值”框中，选择“[Sum(Sales)]”  。 请勿更改任何其他选项。  
  
    默认情况下，数据区域会发生数据同步，且将在“同步作用域”框中看到“Tablix1”值（报表中表数据区域的名称）。  
  
    在该报表中，还可以更改放在子类别小计单元中的指示器的作用域，以在 SalesDate 字段内同步。  
  
11. 单击“确定”。

11. 单击 **“运行”** 以预览报表。  

![显示添加到报表生成器 KPI 报表的“Stoplight KPI”列的屏幕截图。](../reporting-services/media/report-builder-kpi-stoplight.png)
  
## <a name="6-add-a-report-title"></a><a name="Title"></a>6.添加报表标题  
报表标题将出现在报表的顶部。 可以将报表标题置于报表表头中或置于表体顶部的文本框中（如果报表未使用表头）。 在本部分中，可使用自动放在表体顶部的文本框。  
  
通过将不同的字体样式、大小和颜色应用于文本的短语和单个字符，可以进一步增强文本。 有关详细信息，请参阅[设置文本框中文本的格式（报表生成器和 SSRS）](../reporting-services/report-design/format-text-in-a-text-box-report-builder-and-ssrs.md)。  
  
### <a name="to-add-a-report-title"></a>添加报表标题  
  
1.  在设计图面上，单击“单击以添加标题”。  
  
2.  键入 **Product Sales KPIs** ，然后在文本框外部单击。  
  
3.  也可以右键单击包含 Product Sales KPI 的文本框，单击“文本框属性”，然后单击“字体”选项卡，选择不同字形、大小和颜色。  
  
4.  单击 **“运行”** 以预览报表。  
  
## <a name="7-save-the-report"></a><a name="Save"></a>7.保存报表  
将报表保存到报表服务器或计算机上。 如果不将报表保存到报表服务器上，则许多 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 功能（如报表部件和子报表）将不可用。  
  
### <a name="to-save-the-report-on-a-report-server"></a>将报表保存到报表服务器  
  
1.  从 **“报表生成器”** 按钮，单击 **“另存为”** 。  
  
2.  单击 **“最近使用的站点和服务器”** 。  
  
3.  选择或键入您拥有保存报表权限的报表服务器的名称。  
  
    此时将显示“正在连接到报表服务器”消息。 当连接完成时，您将看到报表服务器管理员指定为报表默认位置的报表文件夹的内容。  
  
4.  在“名称”中，用 Product Sales KPI 替换默认名称 。  
  
5.  单击“ **保存** ”。  
  
报表即已保存至报表服务器。 您连接的报表服务器的名称将显示在窗口底部的状态栏中。  
  
### <a name="to-save-the-report-on-your-computer"></a>将报表保存到计算机上  
  
1.  从 **“报表生成器”** 按钮，单击 **“另存为”** 。  
  
2.  依次单击“桌面”、“我的文档”或“我的电脑”，并浏览到要保存该报表的文件夹  。  
  
> [!NOTE]  
> 如果无权访问报表服务器，请单击“桌面”、“我的文档”或“我的电脑”，将报表保存到计算机上。  
  
1.  在“名称”中，用 Product Sales KPI 替换默认名称 。  
  
2.  单击“ **保存** ”。  
  
## <a name="next-steps"></a>后续步骤  
您已成功完成“向报表添加 KPI”教程的学习。 有关详细信息，请参阅：
*  [仪表](../reporting-services/report-design/gauges-report-builder-and-ssrs.md)
* [指示器](../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
  
## <a name="see-also"></a>另请参阅  
* [报表生成器教程](../reporting-services/report-builder-tutorials.md)
* [SQL Server 中的报表生成器](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  

