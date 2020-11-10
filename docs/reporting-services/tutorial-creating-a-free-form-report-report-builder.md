---
title: 教程：创建自由格式的报表（报表生成器） | Microsoft Docs
description: 了解如何创建充当新闻稿的分页报表，其中每一页都会显示静态文本、汇总视觉对象和详细的示例销售数据。
ms.date: 09/02/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 87288b59-faf2-4b1d-a8e4-a7582baedf2f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a3a4bada590dc9bea822aadffd102b49e75d10cf
ms.sourcegitcommit: 9e2c682929ee64c051dc62f8917d147861f7c635
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043828"
---
# <a name="tutorial-creating-a-free-form-report-report-builder"></a>教程：创建自由格式的报表（报表生成器）
在本教程中，将创建充当新闻稿的分页报表。 每一页都会显示静态文本、汇总视觉对象和详细的示例销售数据。

![报表生成器自由格式报表的屏幕截图。](../reporting-services/media/report-builder-free-form-report-complete.png)

该报表按地区对信息进行分组，并且显示该地区的销售经理的姓名以及详细和汇总销售信息。 从使用列表数据区域作为自由格式报表的基础开始，然后添加具有图像的装饰性面板、插入数据的静态文本、用于显示详细信息的表以及可选的用于显示汇总信息的饼图和柱形图。  
  
完成本教程的估计时间：20 分钟。  
  
## <a name="requirements"></a>要求  
有关要求的详细信息，请参阅[教程先决条件（报表生成器）](../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
## <a name="1-create-a-blank-report-data-source-and-dataset"></a><a name="BlankReport"></a>1.创建空白报表、数据源和数据集  
  
> [!NOTE]  
> 在本教程中，由于查询包含了数据值，因此它不需要外部数据源。 这样，查询就会非常长。 在业务环境中，查询不会包含数据。 本教程中的查询仅供学习使用。  
  
### <a name="to-create-a-blank-report"></a>创建空白报表  
  
1.  通过计算机、[Web 门户或 SharePoint 集成模式](../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 。  
  
    将打开“新建报表或数据集”对话框。  
  
    如果未出现“新建报表或数据集”对话框，请通过“文件”菜单转至“新建”  。  
  
2.  在左窗格中，确保已选中“新建报表”。 
 
3.  在右窗格中，单击 **“空白报表”** 。  
  
### <a name="to-create-a-new-data-source"></a>创建新数据源  
  
1.  在“报表数据”窗格中，单击“新建” > “数据源” 。  
  
2.  在“名称”框中键入：ListDataSource  
  
3.  单击 **“使用我的报表中嵌入的连接”** 。  
  
4.  验证连接类型是否为 Microsoft SQL Server，然后在“连接字符串”框中键入：数据源 = \<servername>  
  
    \<servername>（例如，Report001）指定安装了 SQL Server 数据库引擎的实例的计算机。 因为此报表的数据不是从 SQL Server 数据库中提取的，所以不需要包括数据库的名称。 指定服务器上的默认数据库仅用于对查询进行分析。  
  
5.  单击 **“凭据”** ，然后输入连接至 SQL Server 数据库引擎实例所需的凭据。  
  
6.  单击“确定”。  
  
### <a name="to-create-a-new-dataset"></a>新建数据集  
  
1.  在“报表数据”窗格中，单击“新建” > “数据集” 。  
  
2.  在“名称”框中键入：ListDataset。  
  
3.  单击“使用在我的报表中嵌入的数据集” ，然后验证数据源是否为 **ListDataSource** 。  
  
4.  确保已选择 **“文本”** 查询类型，然后单击 **“查询设计器”** 。  
  
5.  单击 **“编辑为文本”** 。  
  
6.  复制并将以下查询粘贴到查询窗格中：  
  
    ```  
    SELECT CAST('2009-01-05' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Accessories' as Subcategory,'Carrying Case' as Product, CAST(16996.60 AS money) AS Sales, 68 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Accessories' as Subcategory, 'Carrying Case' as Product, CAST(13747.25 AS money) AS Sales, 55 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Accessories' as Subcategory,'Carrying Case' as Product, CAST(9248.15 AS money) As Sales, 37 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(1350.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(1800.00 AS money) AS Sales, 24 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(1125.00 AS money) AS Sales, 15 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Accessories' as Subcategory,'Lens Adapter' as Product, CAST(1147.50 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Accessories' as Subcategory,  'Lens Adapter' as Product, CAST(742.50 AS money) AS Sales, 11 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Accessories' as Subcategory,'Lens Adapter' as Product, CAST(1417.50 AS money) AS Sales, 21 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Accessories' as Subcategory, 'Carrying Case' as Product, CAST(13497.30 AS money) AS Sales, 54 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Accessories' as Subcategory, 'Carrying Case' as Product, CAST(11997.60 AS money) AS Sales, 48 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Accessories' as Subcategory, 'Carrying Case' as Product, CAST(10247.95 AS money) As Sales, 41 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Accessories' as Subcategory, 'Tripod' as Product, CAST(1200.00 AS money) AS Sales, 16 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(2025.00 AS money) AS Sales, 27 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(1425.00 AS money) AS Sales, 19 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Accessories' as Subcategory,'Lens Adapter' as Product, CAST(887.50 AS money) AS Sales, 13 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Accessories' as Subcategory, 'Lens Adapter' as Product, CAST(607.50 AS money) AS Sales, 9 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Accessories' as Subcategory,'Lens Adapter' as Product, CAST(1215.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate,  'Lauren Johnson' as FullName,'Central' as Territory, 'Digital' as Subcategory,'Compact Digital' as Product, CAST(10191.00 AS money) AS Sales, 79 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate,  'Warren Pal' as FullName,'North' as Territory, 'Digital' as Subcategory, 'Compact Digital' as Product, CAST(8772.00 AS money) AS Sales, 68 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate,  'Fernando Ross' as FullName,'South' as Territory, 'Digital' as Subcategory, 'Compact Digital' as Product, CAST(10578.00 AS money) AS Sales, 82 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory,'Digital' as Subcategory, 'Slim Digital' as Product, CAST(7218.10 AS money) AS Sales, 38 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory,'Digital' as Subcategory, 'Slim Digital' as Product, CAST(8357.80 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory,'Digital' as Subcategory,'Slim Digital' as Product, CAST(9307.55 AS money) AS Sales, 49 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate,  'Lauren Johnson' as FullName,'Central' as Territory, 'Digital' as Subcategory,'Compact Digital' as Product, CAST(3870.00 AS money) AS Sales, 30 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate,  'Warren Pal' as FullName,'North' as Territory, 'Digital' as Subcategory,'Compact Digital' as Product, CAST(5805.00 AS money) AS Sales, 45 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate,  'Fernando Ross' as FullName,'South' as Territory, 'Digital' as Subcategory, 'Compact Digital' as Product, CAST(8643.00 AS money) AS Sales, 67 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Digital' as Subcategory, 'Slim Digital' as Product, CAST(9877.40 AS money) AS Sales, 52 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Digital' as Subcategory, 'Slim Digital' as Product, CAST(12536.70 AS money) AS Sales, 66 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Digital' as Subcategory, 'Slim Digital' as Product, CAST(6648.25 AS money) AS Sales, 35 as Quantity  
    ```  
  
7.  单击“运行”图标 (!) 以运行查询。  
  
    查询结果将是可用于在您的报表中显示的数据。  
  
    ![查询设计器的屏幕截图，其中显示了可在报表生成器自由格式报表中显示的数据。](../reporting-services/media/report-builder-free-form-tutorial-data.png) 
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="2-add-and-configure-a-list"></a><a name="List"></a>2.添加并配置列表  
在 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 中，列表数据区域是创建自由格式报表的理想工具。 它以及表和矩阵均基于 *tablix* 数据区域。 有关详细信息，请参阅 [创建带列表的发票和表单](../reporting-services/report-design/create-invoices-and-forms-with-lists-report-builder-and-ssrs.md)。  
  
将使用一个列表在格式类似新闻稿的报表中显示销售区域的销售信息。 这些信息按地区分组。 您将添加一个按地区对数据进行分组的新行组，然后删除内置的“详细信息”行组。  
  
### <a name="to-add-a-list"></a>添加列表  
  
1.  在“插入”选项卡 >“数据区域” > “列表”上  。 

2. 在表体（标题和页脚区域之间）内单击，然后拖动生成列表框。 将列表框设为 7 英寸高，6.25 英寸宽。 若要获取确切大小，请在“位置”下的“属性”窗格中，键入“宽度”和“高度”属性的值   。
  
    > [!NOTE]  
    > 此报表使用纸张大小 Letter (8.5 X11) 以及 1 英寸的边距。 高于 9 英寸或宽于 6.5 英寸的列表框可能会生成空白页。  
  
2.  单击列表框内部，然后右键单击列表顶部栏，单击“Tablix 属性”。  
  
    ![显示报表生成器自由格式报表的“Tablix 属性”选项的屏幕截图。](../reporting-services/media/report-builder-free-form-tablix-properties.png) 
  
3.  在 **“数据集名称”** 下拉列表中，选择 **ListDataset** 。  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
5.  在列表内右键单击，然后单击 **“矩形属性”** 。  
  
6.  在“常规”选项卡上，选中“在组件后面添加分页符”复选框 。  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-add-a-new-row-group-and-to-delete-the-details-group"></a>添加新行组和删除“详细信息”组  
  
1.  在“行组”窗格中，右键单击“详细信息”组，指向 **“添加组”** ，然后单击 **“父组”** 。  
  
    ![显示如何将父组添加到报表生成器自由格式报表的屏幕截图。](../reporting-services/media/report-builder-free-form-add-parent-group.png)  
  
2.  在“分组依据”列表中，选择 `[Territory].`  
  
3.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    一个包含单元“ `[Territory]` ”的列会添加到列表中。
  
4.  右键单击列表中的 Territory 列，然后单击 **“删除列”** 。  
  
    ![显示如何在报表生成器自由格式报表中删除列的屏幕截图。](../reporting-services/media/report-builder-free-form-delete-columns.png)
  
5.  选择“仅删除列”。  
  
6.  在“行组”窗格中，右键单击“详细信息”组，然后单击“删除组” 。  
   
7.  选择“仅删除组”。  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="3-add-graphic-elements"></a><a name="Graphics"></a>3.添加图形元素  
列表数据区域的好处之一是，可以将矩形和文本框之类的报表项添加到任何地方，而不会被限制为表格布局。 您将通过添加图形（用颜色填充的矩形）增强报表的外观。  
  
### <a name="to-add-graphic-elements-to-the-report"></a>向报表添加图形元素  
  
1.  在“插入”选项卡上，选择“矩形” 。 

2. 单击列表的左上角，然后通过拖动将矩形设为 7 英寸高，3.5 英寸宽。 同样，若要获取精确大小，请在“位置”下的“属性”窗格中，键入“宽度”和“高度”的值   。
  
2.  右键单击该矩形，然后单击“矩形属性”。  
  
3.  单击 **“填充”** 选项卡。  
  
4.  在“填充颜色”中，选择“浅灰色” 。  
   
5.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
6.  单击 **“运行”** 以预览报表。  
  
报表的左侧现在有一个垂直图形，该图形由一个浅灰色的矩形构成，如下图中所示。  
  
![报表生成器的屏幕截图，其中显示由浅灰色矩形构成的垂直图形。](../reporting-services/media/report-builder-free-form-gray-rectangle.png)
 
## <a name="4-add-free-form-text"></a><a name="Text"></a>4.添加自由格式的文本  
可以添加文本框，用于显示在每个报表页上都重复的静态文本，以及数据字段。  
  
### <a name="to-add-text-to-the-report"></a>向报表添加文本  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  在“插入”选项卡 >“文本框”上 。 在列表左上角的之前添加的矩形内部单击，通过拖动将文本框设为约 3.45 英寸宽，5 英寸高。  
  
3.  将光标置于文本框中，然后键入：Newsletter for。 在“for”一词后包括一个空格，以将文本与将要在下一步中添加的字段分隔开来。   
  
    ![添加新闻稿标题文本](../reporting-services/media/tutorial-newsletterfor.png "添加新闻稿标题文本")  
  
4.  将“ `[Territory]` ”字段从“报表数据”窗格的“ListDataSet”拖到文本框中，将它放在“Newsletter for ”后面。  
  
    ![显示如何将“Territory”字段拖到报表生成器自由格式报表中的屏幕截图。](../reporting-services/media/report-builder-free-form-territory-field.png)
  
5.  选择文本和“ `[Territory]` ”字段。  
  
6.  在“主文件夹”选项卡的“字体”上，选择 ： 
  
    *  “Segoe Semibold”。
    *  “20 磅”。
    *  “番茄色”。  
  
9. 将光标置于步骤 3 中键入的文本下方，然后键入 **Hello** 并在该词后面加一个空格，以将文本与将要在下一步中添加的字段分隔开来。  
 
10. 将“ `[FullName]` ”字段从“报表数据”窗格的“ListDataSet”拖到文本框中，将它放在“Hello ”后面，然后键入一个逗号 (,)。  
   
11. 选择在之前步骤中添加的文本。
  
12. 在“主文件夹”选项卡的“字体”上，选择 ： 
  
    *  “Segoe Semibold”。
    *  “16 磅”。
    *  “黑色”。  
   
15. 将光标置于在步骤 9 到 13 中添加的文本下方，然后复制并粘贴以下无意义文本：  
  
    ```  
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin sed dolor in ipsum pulvinar egestas. Sed sed lacus at leo ornare ultricies. Vivamus velit risus, euismod nec sodales gravida, gravida in dui. Etiam ullamcorper elit vitae justo fermentum ut ullamcorper augue sodales. 
    Ut placerat, nisl quis feugiat adipiscing, nibh est aliquet est, mollis faucibus mauris lectus quis arcu. In mollis tincidunt lacinia. In vitae erat ut lorem tincidunt luctus. Curabitur et magna nunc, sit amet adipiscing nisi. Nulla rhoncus elementum orci nec tincidunt. 
    Aliquam imperdiet cursus erat vel tincidunt. Donec et neque ac urna rutrum sodales. In id purus et nisl dignissim dapibus. Sed rhoncus metus at felis feugiat eu tempor dolor vehicula. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam faucibus consectetur diam eu pellentesque.   
 
    ```  
  
16. 选择刚才添加的文本。  
  
17.  在“主文件夹”选项卡的“字体”上，选择 ： 
  
      *  “Segoe UI”。
      *  “10 磅”。
      *  “黑色”。  
 
20. 将光标置于文本框内无意义文本的下方，然后键入 **Congratulations on your total sales of** 并在文本后面加一个空格，以将文本与将要在下一步中添加的字段分隔开来。 
  
21. 将“Sales”字段拖到文本框中，放在之前步骤中键入的文本后面，然后键入一个感叹号 (!)。  

25. 选择刚才添加的文本和字段。  
  
17.  在“主文件夹”选项卡的“字体”上，选择 ： 
  
      *  “Segoe Semibold”。
      *  “16 磅”。
      *  “黑色”。  
  
22. 仅选择“`[Sales]`”字段，右键单击该字段，然后单击“表达式”。  
  
23. 在“表达式”框中，更改表达式以便包括 Sum 函数，如下所示：  
  
    ```  
    =Sum(Fields!Sales.value)  
    ```  
  
24. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    ![显示报表生成器自由格式报表中的文本框的屏幕截图。](../reporting-services/media/report-builder-free-form-text-box.png)
 
29. 仍然选择“`[Sum(Sales)]`”，在“主文件夹”选项卡上，依次单击“数字”组和“货币”  。  
  
30. 右键单击包含“单击以添加标题”文本的文本框，然后单击 **“删除”** 。  
  
31. 选择列表框。 选择两个双头箭头，将它移到页面顶部。  

    ![显示如何在报表生成器自由格式报表中拖动列表的屏幕截图。](../reporting-services/media/report-builder-drag-list.png)
  
32. 单击 **“运行”** 以预览报表。  
  
报表将显示静态文本，并且每个报表页都包括属于某个地区的数据。 销售额的格式设为货币。  
  
![显示报表生成器自由格式报表中新闻稿页面预览的屏幕截图。](../reporting-services/media/report-builder-newsletter-page-preview.png)
  
## <a name="5-add-a-table-to-show-sales-details"></a><a name="Table"></a>5.添加一个表以显示销售详细信息  
使用新建表和矩阵向导可以将表添加到自由格式的报表。 完成向导后，您将手动添加一个合计行。  
  
### <a name="to-add-a-table"></a>添加表  
  
1.  在“插入”选项卡上，依次单击“数据区域”区域 >“表” > “表向导”   。  
  
2.  在“选择数据集”页上，单击“ListDataset” > “下一步”  。  
  
4.  在“排列字段”页上，将“Product”字段从“可用字段”拖到“值”中  。  
  
5.  对“SalesDate”、“Quantity”和“Sales”重复步骤 3。 将 SalesDate 放置于 Product 之下，将 Quantity 放置于 SalesDate 之下，并且将 Sales 放置于 SalesDate 之下。  
  
6.  单击“下一步”。  
  
7.  在 **“选择布局”** 页上，查看表的布局。  
  
    这个表很简单：只有五列，没有行或列组。 因为它不具有组，所以与组相关的布局选项不可用。 您将手动更新该表以便包括本教程后面部分中的总计。  
  
8.  单击“下一步”。  
  
9. 单击“完成”。  
  
11. 将该表拖到您在第 4 课中添加的文本框之下。  
  
    > [!NOTE]  
    > 确保该表位于列表框和灰色矩形内部。  
  
12. 选中该表，在“行组”窗格中右键单击“详细信息” > “添加总计” > “之后”   。  
  
    ![显示如何将总计添加到报表生成器自由格式报表的屏幕截图。](../reporting-services/media/report-builder-free-form-table-totals.png)
  
13. 选择“Product”列中的单元，然后键入 **Total** 。

    ![“Product”列中的单元格的屏幕截图，其中已键入“Total”。](../reporting-services/media/report-builder-free-form-type-total.png)

12. 选择“[SalesDate]”字段。 在“主文件夹”选项卡上，单击“数字”，将“默认值”更改为“Date”   。

13. 选择“[Sum(Sales)]”字段。 在“主文件夹”选项卡上，单击“数字”，将“默认值”更改为“Currency”   。

单击 **“运行”** 以预览报表。  
  
该报表将显示具有销售详细信息和总计的一个表。  
  
![显示报表生成器自由格式报表中该表预览的屏幕截图。](../reporting-services/media/report-builder-free-form-with-table.png)
   
## <a name="6-save-the-report"></a><a name="Save"></a>6.保存报表  
您可以将报表保存到报表服务器、SharePoint 库或本地计算机。  
  
在本教程中，将报表保存到报表服务器。 如果您没有对报表服务器的访问权限，则可以保存到您的计算机。  
  
### <a name="to-save-the-report-on-a-report-server"></a>将报表保存到报表服务器  
  
1.  从 **“报表生成器”** 按钮，单击 **“另存为”** 。  
  
2.  单击 **“最近使用的站点和服务器”** 。  
  
3.  选择或键入您拥有保存报表权限的报表服务器的名称。  
  
    此时将显示“正在连接到报表服务器”消息。 当连接完成时，您将看到报表服务器管理员指定为报表默认位置的报表文件夹的内容。  
  
4.  在 **“名称”** 中，用 **SalesInformationByTerritory** 替换默认名称。  
  
5.  单击“ **保存** ”。  
  
报表即已保存至报表服务器。 您连接的报表服务器的名称将显示在窗口底部的状态栏中。  
  
### <a name="to-save-the-report-on-your-computer"></a>将报表保存到计算机上  
  
1.  从 **“报表生成器”** 按钮，单击 **“另存为”** 。  
  
2.  单击 **“桌面”** 、 **“我的文档”** 或 **“我的电脑”** ，然后浏览到要将报表保存到的文件夹。  
  
3.  在 **“名称”** 中，用 **SalesInformationByTerritory** 替换默认名称。  
  
4.  单击“ **保存** ”。  
  
## <a name="7-optional-add-a-line-to-separate-areas-of-the-report"></a><a name="Line"></a>7.（可选）添加线条以便分隔报表区域  
添加线条可以分隔报表的可编辑区域和详细信息区域。  
  
### <a name="to-add-a-line"></a>添加线条  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  在“插入”选项卡上，单击“报表项” > “线条”  。  
  
3.  在第 4 课中添加的文本框下方画一条线。  
  
4.  单击这条线，然后在“主文件夹”选项卡上单击“边框”，选择以下各项 ：
     * “宽度”：选择“3 磅” 。
     * “颜色”选择“番茄色”。  
  
## <a name="8-optional-add-summary-data-visualizations"></a><a name="Visualization"></a>8.（可选）添加汇总数据可视化  
矩形有助于控制报表的呈现方式。 将饼图和柱形图放置于矩形内，可以确保报表以您所需的方式呈现。  
  
### <a name="to-add-a-rectangle"></a>添加矩形  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  在“插入”选项卡上，单击“报表项” >  “矩形”  。 将列表框内的矩形拖到表的右侧，将矩形设为约 2.25 英寸宽，7.9 英寸高。  
  
3.  选中新矩形，在“属性”窗格中，将 **边框颜色设为浅灰色** 、 **边框样式设为纯色** 以及 **边框宽度设为 2 磅** 。 

4. 将矩形的顶部和表的顶部对齐。  
  
## <a name="to-add-a-pie-chart"></a>添加饼图  
  
1.  在“插入”选项卡上，单击“数据可视化” > “图表” > “图表向导”   。  
  
2.  在“选择数据集”页上，单击“ListDataset” > “下一步”  。  
  
3.  单击“饼图” > “下一步” 。  
  
4.  在“排列图表字段”页上，将“Product”拖到“类别”中。  
  
5.  将“Quantity”拖到“值”中，然后单击“下一步” 。  
  
6.  单击“完成”。  
  
8.  调整报表左上角中出现的图表的大小，将其设为约 2.25 英寸宽，3.6 英寸高。  
  
9. 将图表拖至该矩形内。  
   
10. 选择图表标题，然后键入：Product Quantities Sold。  
  
12. 在“开始”选项卡的“字体”上，对标题进行以下设置 ：
    * 字体为 Segoe UI Semibold。
    * 大小为 12 pt。
    * 颜色为黑色。  

13. 右键单击图例，然后单击“图例属性”。

14. 在“常规”选项卡的“图例位置”下，选择底部的中心点 。 
  
15. [!INCLUDE[clickOK](../includes/clickok-md.md)]  

16. 根据需要通过拖动让图表区域更高。

     ![显示要添加到报表生成器自由格式报表的饼图的屏幕截图。](../reporting-services/media/report-builder-free-form-pie.png)
  
## <a name="to-add-a-column-chart"></a>添加柱形图  
  
1.  在“插入”选项卡上，单击“数据可视化” > “图表” > “图表向导”   。  
  
2.  在“选择数据集”页上，单击“ListDataset”，然后单击“下一步”  。  
  
3.  单击“柱形图”，然后单击“下一步” 。  
  
4.  在“排列图表字段”页上，将“Product”字段拖到“类别”窗格中 。  
  
5.  将“Sales”拖到“值”中，然后单击“下一步” 。  
  
    “值”显示在垂直轴上。  
  
6.  单击“完成”。  
  
    一个柱形图将添加到报表的左上角。  
  
8.  将图表大小调整为约 2.25 英寸宽，4 英寸高。  
  
9. 将图表拖至该矩形内的饼图之下。  
   
10. 选择图表标题，然后键入：Product Sales。  
  
12. 在“开始”选项卡的“字体”上，对标题进行以下设置 ：
    * 字体为 Segoe UI Semibold。
    * 大小为 12 pt。
    * 颜色为黑色。  
  
15. 右键单击图例，然后单击 **“删除图例”** 。  
  
    > [!NOTE]  
    > 删除图例可在图表较小时提高图表的可读性。  
  
    ![显示要添加到报表生成器自由格式报表的柱形图的屏幕截图。](../reporting-services/media/report-builder-free-form-column.png)

12. 选择图表轴，然后在“开始”选项卡上单击“数字” > “货币”  。

13. 选择“减少小数位数”两次，以便数字仅显示美元，而不显示美分。      
### <a name="to-verify-the-charts-are-inside-the-rectangle"></a>确认图表位于矩形内  

可以使用矩形作为报表页上其他项的容器。 阅读有关 [矩形作为容器](../reporting-services/report-design/rectangles-and-lines-report-builder-and-ssrs.md)的详细信息。
  
1.  选择之前在本课中创建的并将图表添加到其中的矩形。  
  
    在“属性”窗格中， **Name** 属性显示矩形的名称。  
  
    ![显示如何在报表生成器自由格式报表中查看“Name”属性的屏幕截图。](../reporting-services/media/report-builder-free-form-rectangle-name.png) 
  
2.  单击饼图。  
  
3.  在 **“属性”** 窗格中，确认 **Parent** 属性包含矩形的名称。  
  
     ![显示如何在报表生成器自由格式报表中查看“Parent”属性的屏幕截图。](../reporting-services/media/report-builder-free-form-pie-parent.png) 
  
4.  单击柱形图，重复步骤 3。  
  
    > [!NOTE]  
    > 如果这些图表不位于矩形内，则呈现的报表不会一起显示这些图表。  
  
### <a name="to-make-the-charts-the-same-size"></a>使图表具有相同大小  
  
1.  选择饼图，按下 Ctrl 键，然后选择柱形图。  
  
2.  选中两个图表，然后右键单击“布局” > “使宽度相同” 。  
  
    > [!NOTE]  
    > 你首先单击的项决定所有选定项的宽度。  
  
3.  单击 **“运行”** 以预览报表。  
  
报表现在将在饼图和柱形图中显示汇总销售数据。  
  

  
## <a name="next-steps"></a>后续步骤  
有关如何创建自由格式报表的教程到此结束。  
  
有关列表的详细信息，请参阅： 
* [表、矩阵和列表（报表生成器和 SSRS）](../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md) 
* [创建带列表的发票和表单](../reporting-services/report-design/create-invoices-and-forms-with-lists-report-builder-and-ssrs.md)
* [Tablix 数据区域单元、行和列（报表生成器和 SSRS）](../reporting-services/report-design/tablix-data-region-cells-rows-and-columns-report-builder-and-ssrs.md)。  
  
若要详细了解查询设计器，请参阅[查询设计器工具 (SSRS)](report-data/query-design-tools-ssrs.md) 和[基于文本的查询设计器用户界面（报表生成器）](../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md)。  
  
## <a name="see-also"></a>另请参阅  
[报表生成器教程](../reporting-services/report-builder-tutorials.md) 
  

