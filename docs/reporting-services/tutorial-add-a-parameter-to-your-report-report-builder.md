---
title: 教程：向报表添加参数（报表生成器）| Microsoft Docs
description: 了解如何将参数添加到 Reporting Services 分页报表，以使报表读者可以筛选报表数据的一个或多个值。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: eab34ec4-b3ad-4a76-95cc-07b2f75ee6d7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8e4d7569f8624843f0cf05b4c873f0be7b26df3c
ms.sourcegitcommit: 9e2c682929ee64c051dc62f8917d147861f7c635
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043784"
---
# <a name="tutorial-add-a-parameter-to-your-report-report-builder"></a>教程：向报表添加参数（报表生成器）
在本教程中，将参数添加到 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 分页报表，使报表读者可以筛选报表数据的一个或多个值。 
  
![屏幕截图显示可以向分页报表添加参数。](../reporting-services/media/report-builder-parameter-tutorial.png)

报表参数是针对您在数据集查询中包含的每个查询参数自动创建的。 参数的数据类型确定了参数在报表视图工具栏上显示的方式。 
   
> [!NOTE]  
> 在本教程中，将向导的多个步骤合并为一个过程。 有关如何浏览到报表服务器、选择数据源和创建数据集的分步说明，请参阅本系列中的第一个教程：[教程：创建基本表报表（报表生成器）](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md)。  
  
完成本教程的估计时间：25 分钟。  
  
## <a name="requirements"></a>要求  
有关要求的信息，请参阅[教程先决条件（报表生成器）](../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
## <a name="1-create-a-matrix-report-and-dataset-in-the-table-or-matrix-wizard"></a><a name="Setup"></a>1.在表或矩阵向导中创建矩阵报表和数据集  
创建矩阵报表、数据源和数据集。  
  
> [!NOTE]  
> 在本教程中，由于查询包含了数据值，因此它不需要外部数据源。 这样，查询就会非常长。 在业务环境中，查询不会包含数据。 本教程中的查询仅供学习使用。  
  
### <a name="to-create-a-new-matrix-report"></a>创建新的矩阵报表  
  
1.  通过计算机、[Web 门户或 SharePoint 集成模式](../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 。  
  
    将打开“新建报表或数据集”对话框。  
  
    如果未出现“新建报表或数据集”对话框，请通过“文件”菜单转至“新建”  。  
  
2.  在左窗格中，确保已选中“新建报表”。  
  
3.  在右窗格中，单击“表或矩阵向导”。  
  
4.  在“选择数据集”页上，单击“创建数据集” > “下一步”  。  
  
7.  在“选择数据源的连接”页上，从列表中选择一个数据源或浏览到报表服务器进行选择。 选择任何类型为 **SQL Server** 的数据源。  
      
8.  单击“下一步”。  

    可能需要输入凭据。    
     
9. 在“设计查询”页上，单击“编辑为文本” 。  
  
10. 将以下查询粘贴到顶部的空窗格中：  
  
    ```  
    ;WITH CTE (StoreID, Subcategory, Quantity)   
    AS (  
    SELECT 200 AS StoreID, 'Digital SLR Cameras' AS Subcategory, 2002 AS Quantity  
    UNION SELECT  200 AS StoreID, 'Camcorders' AS Subcategory, 1954 AS Quantity  
    UNION SELECT  200 AS StoreID, 'Accessories' AS Subcategory, 1895 AS Quantity  
    UNION SELECT  199 AS StoreID, 'Digital Cameras' AS Subcategory, 1849 AS Quantity  
    UNION SELECT  306 AS StoreID, 'Digital SLR Cameras' AS Subcategory, 1579 AS Quantity  
    UNION SELECT  306 AS StoreID, 'Camcorders' AS Subcategory, 1561 AS Quantity  
    UNION SELECT  306 AS StoreID, 'Digital Cameras' AS Subcategory, 1553 AS Quantity  
    UNION SELECT  306 AS StoreID, 'Accessories' AS Subcategory, 1534 AS Quantity  
    UNION SELECT 307 AS StoreID, 'Accessories' AS Subcategory, 1755 AS Quantity  
    UNION SELECT 307 AS StoreID, 'Camcorders' AS Subcategory, 1631 AS Quantity  
    UNION SELECT 307 AS StoreID, 'Digital SLR Cameras' AS Subcategory, 1772 AS Quantity)  
    SELECT StoreID, Subcategory, Quantity  
    FROM CTE  
    ```  
  
    此查询在一个公用表表达式中组合了若干 [!INCLUDE[tsql_md](../includes/tsql-md.md)] SELECT 语句的结果，以指定基于来自 Contoso 示例数据库的简化相机销售数据的值。 子类别有数码相机、数码单反 (SLR) 相机、摄像机和附件。  
  
11. 在查询设计器工具栏中，单击“运行(!)”来查看数据 。   
  
    结果集由 11 行数据组成，这些数据显示四个商店的每个子类别销售的物品数量，包含以下列：StoreID、Subcategory、Quantity。商店名称不是结果集的一部分。 接下来，您将在本教程中从单独的数据集查找与商店标识符对应的商店名称。  
  
    此查询不包含查询参数。 稍后，您将在本教程中添加查询参数。   
  
12. 单击“下一步”。  
  
## <a name="2-organize-data-and-choose-layout-in-the-wizard"></a><a name="CompleteWizard"></a>2.在向导中组织数据并选择布局  
该向导提供用于显示数据的起始设计。 此向导中的预览窗格可帮助您在完成表或矩阵设计之前将对数据进行分组的结果可视化。  
  
### <a name="to-organize-data-into-groups"></a>将数据组织到组中  
  
1.  在“排列字段”页上，将 Subcategory 拖到“行组”中 。  
  
2.  将 StoreID 拖到“列组”中。  
  
3.  将 Quantity 拖到“值”中。  
  
    已将销售量值组织到了按子类别分组的行中，一家商店一列。  
  
4.  单击“下一步”。  
  
5.  在“选择布局”页的“选项”下，确保已选择“显示小计和总计”  。  
  
    当您运行报表时，最后一列将显示所有商店的每个子类别的总数量，而最后一行将显示每个商店的所有子类别的总数量。  
  
6.  单击“下一步”。  
  
8.  单击“完成”。  
  
    矩阵将添加到设计图面中。 矩阵将显示 3 列和 3 行。 第一行中的单元格内容是 Subcategory、[StoreID] 和“总计”。 第二行中的单元格内容包含的表达式表示子类别、每个商店销售的物品数量以及所有商店每个子类别的总数量。 最后一行中的单元格显示每个商店的总计。  
      
    ![ssRB_ParamTut_Design](../reporting-services/media/ssrb-paramtut-design.png)  
  
9. 在矩阵中单击，将光标悬停在第一个列的边缘并抓住句柄，然后扩展列宽。  
  
   ![ssRB_ParamTut_Drag](../reporting-services/media/ssrb-paramtut-drag.png)  
  
10. 单击 **“运行”** 以预览报表。  
  
报表将在报表服务器上运行并显示标题以及进行报表处理的时间。  

![ssRB_ParamTut__Preview1](../reporting-services/media/ssrb-paramtut-preview1.png)
  
到目前为止，列标题显示的是商店标识符而不是商店名称。 接下来，您将添加表达式以便在包含商店标识符/商店名称对的数据集中查找商店名称。  
  
## <a name="3-add-a-query-parameter-to-create-a-report-parameter"></a><a name="Query"></a>3.添加查询参数以创建报表参数  
当向查询添加查询参数时，报表生成器将自动创建单值报表参数，其中名称、提示符和数据类型使用默认属性。  
  
### <a name="to-add-a-query-parameter"></a>添加查询参数  
  
1.  单击“设计”切换回设计视图。  
  
2.  在“报表数据”窗格中，展开“数据集”文件夹，右键单击“DataSet1”，然后单击“查询”  。  
  
3.  将以下 [!INCLUDE[tsql](../includes/tsql-md.md)] WHERE 子句添加为查询的最后一行：  
  
    ```  
    WHERE StoreID = (@StoreID)  
    ```  
  
    WHERE 子句将检索的数据限制为由查询参数“\@StoreID”指定的商店标识符。  
  
4.  在查询设计器工具栏中，单击“运行”( **!** )。 此时将打开“定义查询参数”对话框，提示为查询参数“\@StoreID”输入值。  
  
5.  在“参数值”中，键入 **200** 。  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    结果集显示了标识符为 **200** 的商店销售的附件、摄像机和数码 SLR 相机数量。  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
8.  在“报表数据”窗格中，展开“参数”文件夹。  
  
请注意，现在有一个名为“\@StoreID”的报表参数，一个可在其中排放报表参数的“参数”窗格。   
  
![ssRB_ParamPane](../reporting-services/media/ssrb-parampane.png)  
  
看不到参数窗格？ 在“视图”菜单上，选择“参数”。  
  
## <a name="4-change-default-data-type-and-other-properties-for-a-report-parameter"></a><a name="ChangeDefaultProperties"></a>4.更改报表参数的默认数据类型和其他属性  
创建报表参数之后，可以调整属性的默认值。  
  
### <a name="to-change-the-default-data-type-for-a-report-parameter"></a>更改报表参数的默认数据类型  
  
默认情况下，创建的参数数据类型为 **Text** 。 由于商店标识符是一个整数，因此可将数据类型更改为 Integer。  
  
1.  在“报表数据”窗格的“参数”节点中，右键单击“\@StoreID”，然后单击“参数属性”。  
  
2.  在“提示符”下，键入 **Store identifier?** 当您运行报表时，此文本出现在报表查看器工具栏上。  
  
3.  在“数据类型”的下拉列表中，选择“整数” 。  
  
4.  接受对话框中的其他默认值。  
  
5.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
6.  单击 **“运行”** 以预览报表。 报表查看器将显示“\@StoreID”的提示“Store Identifier?”。  
  
7.  在报表查看器工具栏上，在 Store ID 的旁边键入 **200** ，然后单击“查看报表”。  
  
![SSRB_ParamTutStoreID](../reporting-services/media/ssrb-paramtutstoreid.png)  
  
## <a name="4a-add-a-dataset-to-provide-available-values-and-display-names"></a><a name="AddDataset"></a>4a. 添加数据集以提供可用值和显示名称  
若要确保报表读者仅对参数键入有效值，可以创建一个从中选择值的下拉列表。 值可以来自于您指定的数据集或列表。 可用值必须由具有查询的数据集提供，该查询不包含对参数的引用。  
  
### <a name="to-create-a-dataset-for-valid-values-for-a-parameter"></a>为参数创建有效值数据集  
  
1.  单击“设计”切换到“设计”视图。  
  
2.  在“报表数据”窗格中，右键单击“数据集”文件夹，然后单击“添加数据集” 。  
  
3.  在“名称”中，键入“Stores” 。  
  
4.  选择“使用在我的报表中嵌入的数据集” 。  
  
5.  在“数据源”的下拉列表中，选择在第一步所用的数据源。  
  
6.  在“查询类型”中，确保已选中“文本” 。  
  
7.  在“查询”中，粘贴以下文本：  
  
    ```  
    SELECT 200 AS StoreID, 'Contoso Catalog Store' as StoreName  
    UNION SELECT 199 AS StoreID, 'Contoso North America Online Store' as StoreName  
    UNION SELECT 307 AS StoreID, 'Contoso Asia Online Store' as StoreName  
    UNION SELECT 306 AS StoreID, 'Contoso Europe Online Store' as StoreName  
    ```  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    “报表数据”窗格在 **Stores** 数据集节点下显示字段 StoreID 和 StoreName。  
  
## <a name="4b-specify-available-values-to-show-in-a-list"></a><a name="AvailableValues"></a>4b. 指定要显示在列表中的可用值 
创建数据集以提供可用值后，更改报表属性，以指定将哪个数据集和哪个字段用于填充报表查看器工具栏上的有效值下拉列表。  
  
### <a name="to-provide-available-values-for-a-parameter-from-a-dataset"></a>为参数提供数据集中的可用值  
  
1.  在“报表数据”窗格中，右键单击参数“\@StoreID”，然后单击“参数属性”。  
  
2.  单击“可用值”，然后单击“从查询中获取值”。  
  
3.  在“数据集”的下拉列表中，单击“Stores” 。  
  
4.  在“值字段”的下拉列表中，单击“StoreID”。  
  
5.  在“标签字段”的下拉列表中，单击“StoreName”。 标签字段指定值的显示名称。  
  
6.  单击 **“常规”** 。  
  
7.  在“提示符”中，将“Store Identifer?”更改为“Store name?”    
  
    报表读者现在将从商店名称列表而非商店标识符列表中进行选择。 请注意，参数数据类型保持为 **Integer** ，因为参数是基于商店标识符而不是商店名称。  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
9. 预览报表。  
  
    在报表查看器工具栏中，参数文本框现在为显示“选择一个值”的下拉列表。  
  
10. 从下拉列表中，选择“Contoso Catalog Store”，然后单击“查看报表”。  
  
报表显示了标识符为 **200** 的商店销售的附件、摄像机和数码 SLR 相机数量。  
  
## <a name="4c-specify-a-default-value"></a><a name="DefaultValues"></a>4c. 指定默认值 
可以为每个参数指定默认值，以便自动运行报表。  
  
### <a name="to-specify-a-default-value-from-a-dataset"></a>从数据集中指定默认值  
  
1.  切换到“设计”视图。  
  
2.  在“报表数据”窗格中，右键单击“\@StoreID”，然后单击“参数属性”。  
  
3.  单击“默认值”，然后单击“从查询中获取值”。  
  
4.  在“数据集”的下拉列表中，单击“Stores” 。  
  
5.  在“值字段”的下拉列表中，单击“StoreID”。  
  
6.  [!INCLUDE[clickOK_md](../includes/clickok-md.md)]  
  
7.  预览报表。  
  
对于 \@StoreID，报表查看器显示值“Contoso North America Online Store”，因为它是数据集“Stores”的结果集的第一个值。 报表显示了标识符为 **199** 的商店销售的数码相机数量。  
  
### <a name="to-specify-a-custom-default-value"></a>指定自定义默认值  
  
1.  切换到“设计”视图。  
  
2.  在“报表数据”窗格中，右键单击“\@StoreID”，然后单击“参数属性”。  
  
3.  单击“默认值” > “指定值” > “添加”。 此时将添加一个新的值行。  
  
4.  在“值”中，键入“200” 。  
  
5.  [!INCLUDE[clickOK_md](../includes/clickok-md.md)] 
  
6.  预览报表。  
  
对于 \@StoreID，报表查看器显示“Contoso Catalog Store”，因为它是标识符为 200 的商店的显示名称。 报表显示了标识符为 **200** 的商店销售的附件、摄像机和数码 SLR 相机数量。  
  
## <a name="4d-look-up-a-namevalue-pair"></a><a name="NameValue"></a>4d. 查找名称/值对  
数据集可以同时包含标识符和对应的名称字段。 若只有一个标识符，则可以在创建的包含名称/值对的数据集中查找对应的名称。  
  
### <a name="to-look-up-a-value-from-a-dataset"></a>从数据集查找值  
  
1.  切换到“设计”视图。  
  
2.  在设计图面上，在矩阵的第一行的列标题中，右键单击 `[StoreID]`，然后单击“表达式”。  
  
3.  在“表达式”窗格中，删除除开头的 **等号** (=) 之外的所有文本。  
  
4.  在“类别”中，展开“常见函数”，然后单击“杂项”  。 “项”窗格将显示一组函数。  
  
5.  在“项”中，双击“查找”。 表达式窗格将显示 `=Lookup(`。 “示例”窗格将显示一个 Lookup 语法示例。  
  
6.  键入以下表达式： 

    ```  
    =Lookup(Fields!StoreID.Value,Fields!StoreID.Value,Fields!StoreName.Value,"Stores")      
    ```  
  
    Lookup 函数将采用 StoreID 的值，在“Stores”数据集中查找该值，并返回 StoreName 值。  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    店铺列标题包含复杂表达式的显示文本：Expr。  
  
8.  预览报表。  
  
每列顶部的列标题显示商店名称而不是商店标识符。  
  
## <a name="5-display-the-selected-parameter-value-in-the-report"></a><a name="Expression"></a>5.在报表中显示所选参数值  
当报表读者对报表有疑问时，此操作可帮助用户获知他们选择的参数值。 可以在报表中保留用户为每个参数的选定的值。 实现这一点的一种方式是在页脚的文本框中显示参数。  
  
### <a name="to-display-the-selected-parameter-value-and-label-on-a-page-footer"></a>在页脚中显示所选参数值和标签  
  
1.  切换到“设计”视图。  
  
2.  右键单击“页脚” > 单击“插入” > “文本框” 。 拖动带时间戳的文本框旁边的文本框。 抓住该文本框的侧手柄并扩展宽度。  
  
3.  从“报表数据”窗格中，将参数“\@StoreID”拖到文本框。 文本框显示 `[@StoreID]`。  
  
4.  若要显示参数标签，请在文本框中单击，直到插入游标出现在现有表达式之后，键入一个空格，然后从“报表数据”窗格将参数的另一个副本拖到文本框。 文本框显示 `[@StoreID] [@StoreID]`。  
  
5.  右键单击第一个 `[@StoreID]`，然后单击“表达式”。 此时将打开 **“表达式”** 对话框。 将文本 `Value` 替换为 `Label`。  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    文本显示： `[@StoreID.Label] [@StoreID]`。  
  
7.  预览报表。  
  
## <a name="6-use-the-report-parameter-in-a-filter"></a><a name="Filter"></a>6.在筛选器中使用报表参数  
筛选器可帮助控制在从外部数据源检索到数据后在报表中使用哪些数据。 为了让报表读者能够控制他们要看到的数据，可以在矩阵的筛选器中包含报表参数。  
  
### <a name="to-specify-a-parameter-in-a-matrix-filter"></a>在矩阵筛选器中指定参数  
  
1.  切换到“设计”视图。  
  
2.  右键单击矩阵中的某个行或列标题句柄，然后单击“Tablix 属性”。  
  
3.  单击“筛选器”，然后单击“添加” 。 此时将显示一个新的筛选器行。  
  
4.  在“表达式”的下拉列表中，选择数据集字段 StoreID。 数据类型显示 **Integer** 。 当表达式值为数据集字段时，将自动设置数据类型。  
  
5.  在“运算符”中，确认选择了“等于号 (=)” 。  
  
6.  在“值”中，键入 `[@StoreID]`。 

    `[@StoreID]` 是一个简单表达式语法，它表示 `=Parameters!StoreID.Value`。  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
8.  预览报表。  
  
    矩阵仅显示“Contoso Catalog Store”的数据。  
  
9. 在报表查看器工具栏上，对于“Store name?”，选择“Contoso Asia Online Store”，然后单击“查看报表”  。  
  
矩阵将显示与所选商店对应的数据。  
  
## <a name="7-change-the-report-parameter-to-accept-multiple-values"></a><a name="Multivalued"></a>7.更改报表参数以接受多个值  
若要将一个参数从单值参数更改为多值参数，则必须更改查询和包含对该参数的引用的所有表达式（包括筛选器）。 多值参数是一个值数组。 在数据集查询中，查询语法必须一个值是否包含在一组值中。 在报表表达式中，表达式语法必须访问一个值数组而不是单个值。  
  
### <a name="to-change-a-parameter-from-single-to-multivalued"></a>将一个参数从单值参数更改为多值参数  
  
1.  切换到“设计”视图。  
  
2.  在“报表数据”窗格中，右键单击“\@StoreID”，然后单击“参数属性”。  
  
3.  选择“允许多个值”。  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
5.  在“报表数据”窗格中，展开“数据集”文件夹，右键单击“DataSet1”，然后单击“查询”  。  
  
6.  在查询最后一行的 [!INCLUDE[tsql](../includes/tsql-md.md)] WHERE 子句中，将“等于号 (=)”更改为 IN  ：  
  
    ```  
    WHERE StoreID IN (@StoreID)  
    ```  
  
    **IN** 运算符测试一个值是否包含在一组值中。  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
8.  右键单击矩阵中的某个行或列标题句柄，然后单击“Tablix 属性”。  
  
9. 单击 **“筛选器”** 。  
  
10. 在“运算符”中，选择“In” 。  
  
11. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
12. 从页脚中用于显示参数的文本框中删除所有文本。  
  
13. 右键单击该文本框，然后单击“表达式”。 键入以下表达式： `=Join(Parameters!StoreID.Label, ", ")`  
  
    此表达式将连接用户所选的所有商店名称，用逗号或空格隔开。  
  
14. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
15. 在刚创建的表达式的前面的文本框中单击，然后键入以下内容： 

    **Parameter Values Selected:** 
  
16. 预览报表。  
  
17. 单击 Store Name? 旁边的下拉列表  
  
    每个复选框旁边都会显示一个有效值。  
  
18. 单击“全选”，再单击“查看报表” 。  
  
    报表将显示所有商店销售的所有子类别的数量。  
  
19. 从下拉列表中，单击“全选”清除列表，再单击“Contoso Catalog Store”和“Contoso Asia Online Store”，然后单击“查看报表”。  

    ![屏幕截图显示可选择多个参数添加到报表。](../reporting-services/media/report-builder-parameter-multiselect.png)
  
 
## <a name="8-add-a-boolean-parameter-for-conditional-visibility"></a><a name="Boolean"></a>8.为条件可见性添加布尔参数  
  
### <a name="to-add-a-boolean-parameter"></a>添加布尔参数  
  
1.  在设计图面上的“报表数据”窗格中，右键单击“参数”，再单击“添加参数” 。  
  
2.  在“名称”中，键入“ShowSelections”。  
  
3.  在“提示符”，键入“Show selections?”  
  
4.  在“数据类型”中，单击“布尔” 。  
  
5.  单击 **“默认值”** 。  
  
6.  单击“指定值”，然后单击“添加” 。  
  
7.  在“值”中，键入“False” 。  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-set-visibility-based-on-a-boolean-parameter"></a>基于布尔参数设置可见性  
  
1.  在设计图面上，右键单击页脚中用于显示参数值的文本框，然后单击“文本框属性”。  
  
2.  单击 **“可见性”** 。  
  
3.  选择选项“基于表达式显示或隐藏”，然后单击表达式按钮“Fx”。  
  
4.  键入以下表达式： `=Not Parameters!ShowSelections.Value`  
  
    文本框的“可见性”选项由属性 Hidden 控制。 应用 **Not** 运算符，以便在选择参数时，Hidden 属性为 false 并显示文本框。  
  
5.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
7.  预览报表。  
  
    在页脚中显示参数选项的文本框未出现。  
  
8.  在报表查看器工具栏上，单击“Show selections”旁边的“True” > “查看报表”  。  
  
    页脚中的文本框将出现，显示所选的所有商店名称。  
  
## <a name="9-add-a-report-title"></a><a name="Title"></a>9.添加报表标题  
  
### <a name="to-add-a-report-title"></a>添加报表标题  

1.  切换到“设计”视图。  
   
1.  在设计图面上，单击“单击以添加标题”。  
  
2.  键入 Parameterized Product Sales，然后在文本框外部单击。  
  
## <a name="10-save-the-report"></a><a name="Save"></a>10.保存报表  
  
### <a name="to-save-the-report-on-a-report-server"></a>将报表保存到报表服务器  
  
1.  从 **“报表生成器”** 按钮，单击 **“另存为”** 。  
  
2.  单击 **“最近使用的站点和服务器”** 。  
  
3.  选择或键入您拥有保存报表权限的报表服务器的名称。  
  
    此时将显示“正在连接到报表服务器”消息。 当连接完成时，您将看到报表服务器管理员指定为报表默认位置的报表文件夹的内容。  
  
4.  在“名称”中，用 Parameterized Sales Report 替换默认名称。  
  
5.  单击“ **保存** ”。  
  
报表即已保存至报表服务器。 您连接的报表服务器将显示在窗口底部的状态栏中。  
  
## <a name="next-steps"></a>后续步骤  
到此为止，我们结束了有关如何向报表添加参数的演练。 要了解有关参数的详细信息，请参阅[报表参数（报表生成器和报表设计器）](../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)。  
  
## <a name="see-also"></a>另请参阅  
* [报表生成器教程](../reporting-services/report-builder-tutorials.md)
* [SQL Server 中的报表生成器](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
*  [Lookup 函数](../reporting-services/report-design/report-builder-functions-lookup-function.md)   
