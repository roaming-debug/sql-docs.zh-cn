---
title: 教程：表达式简介 | Microsoft Docs
description: 了解如何使用包含常用函数和运算符的表达式创建功能强大且灵活的 Reporting Services 分页报表。
ms.date: 09/16/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 2d05ef4c-5f91-48b2-8795-f0a201a0b3cc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: db1efab37a24e49dcf9eac774809ec788831b037
ms.sourcegitcommit: 9e2c682929ee64c051dc62f8917d147861f7c635
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043819"
---
# <a name="tutorial-introducing-expressions"></a>教程：表达式简介
该 [!INCLUDE[ssRBnoversion_md](../includes/ssrbnoversion.md)] 教程介绍了如何使用包含常用函数和运算符的表达式创建功能强大且灵活的 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 分页报表。 

将编写表达式，这些表达式用于连接名称值、在单独的数据集中查找值和基于字段值显示不同的颜色等。  
  
报表是一个镶边报表，采用白色和彩色交替显示行。 报表包括用于选择非白色行的颜色的参数。  
  
此图显示与将创建的报表类似的报表。  
  
![使用表达式的报表生成器报表的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-in-browser.png) 
  
完成本教程的估计时间：30 分钟。  
  
## <a name="requirements"></a>要求  
有关要求的信息，请参阅[教程先决条件（报表生成器）](../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
## <a name="1-create-a-table-report-and-dataset-from-the-table-or-matrix-wizard"></a><a name="Setup"></a>1.使用表向导或矩阵向导创建表报表和数据集  
在本部分中，将创建表报表、数据源和数据集。 在设计表的布局时，将只包含若干字段。 完成向导后，您将手动添加列。 使用该向导可以方便地对表进行布局。  
  
> [!NOTE]  
> 在本教程中，由于查询包含了数据值，因此它不需要外部数据源。 这样，查询就会非常长。 在业务环境中，查询不会包含数据。 本教程中的查询仅供学习使用。  
  
### <a name="to-create-a-table-report"></a>创建表报表  
  
1.  通过计算机、[Web 门户或 SharePoint 集成模式](../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 。  
  
    将打开“新建报表或数据集”对话框。  
  
    如果未出现“新建报表或数据集”对话框，请通过“文件”菜单转至“新建”  。  
  
2.  在左窗格中，确认已选中 **“新建报表”** 。  
  
3.  在右窗格中，单击“表或矩阵向导”。  
  
4.  在“选择数据集”页上，单击“创建数据集” > “下一步”  。  
  
6.  在“选择数据源的连接”页上，选择类型为“SQL Server”的数据源。 从列表中选择一个数据源或浏览到报表服务器以选择一个数据源。  

    > [!NOTE]  
    > 只要具有足够的权限，则选择哪一个数据源并不重要。 您将不会从数据源中获取数据。 有关详细信息，请参阅[获取数据连接的备选方式（报表生成器）](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md)。  
  
7.  单击“下一步”。  
  
8.  在“设计查询”页上，单击“编辑为文本” 。  
  
9. 将以下查询粘贴到查询窗格中：  
  
    ```  
    SELECT 'Lauren' AS FirstName,'Johnson' AS LastName, 'American Samoa' AS StateProvince, 1 AS CountryRegionID,'Female' AS Gender, CAST(9996.60 AS money) AS YTDPurchase, CAST('2015-6-10' AS date) AS LastPurchase  
    UNION SELECT'Warren' AS FirstName, 'Pal' AS LastName, 'New South Wales' AS StateProvince, 2 AS CountryRegionID, 'Male' AS Gender, CAST(5747.25 AS money) AS YTDPurchase, CAST('2015-7-3' AS date) AS LastPurchase  
    UNION SELECT 'Fernando' AS FirstName, 'Ross' AS LastName, 'Alberta' AS StateProvince, 3 AS CountryRegionID, 'Male' AS Gender, CAST(9248.15 AS money) AS YTDPurchase, CAST('2015-10-17' AS date) AS LastPurchase  
    UNION SELECT 'Rob' AS FirstName, 'Caron' AS LastName, 'Northwest Territories' AS StateProvince, 3 AS CountryRegionID, 'Male' AS Gender, CAST(742.50 AS money) AS YTDPurchase, CAST('2015-4-29' AS date) AS LastPurchase  
    UNION SELECT 'James' AS FirstName, 'Bailey' AS LastName, 'British Columbia' AS StateProvince, 3 AS CountryRegionID, 'Male' AS Gender, CAST(1147.50 AS money) AS YTDPurchase, CAST('2015-6-15' AS date) AS LastPurchase  
    UNION SELECT  'Bridget' AS FirstName, 'She' AS LastName, 'Hamburg' AS StateProvince, 4 AS CountryRegionID, 'Female' AS Gender, CAST(7497.30 AS money) AS YTDPurchase, CAST('2015-5-10' AS date) AS LastPurchase  
    UNION SELECT 'Alexander' AS FirstName, 'Martin' AS LastName, 'Saxony' AS StateProvince, 4 AS CountryRegionID, 'Male' AS Gender, CAST(2997.60 AS money) AS YTDPurchase, CAST('2015-11-19' AS date) AS LastPurchase  
    UNION SELECT 'Yolanda' AS FirstName, 'Sharma' AS LastName ,'Micronesia' AS StateProvince, 5 AS CountryRegionID, 'Female' AS Gender, CAST(3247.95 AS money) AS YTDPurchase, CAST('2015-8-23' AS date) AS LastPurchase  
    UNION SELECT 'Marc' AS FirstName, 'Zimmerman' AS LastName, 'Moselle' AS StateProvince, 6 AS CountryRegionID, 'Male' AS Gender, CAST(1200.00 AS money) AS YTDPurchase, CAST('2015-11-16' AS date) AS LastPurchase  
    UNION SELECT 'Katherine' AS FirstName, 'Abel' AS LastName, 'Moselle' AS StateProvince, 6 AS CountryRegionID, 'Female' AS Gender, CAST(2025.00 AS money) AS YTDPurchase, CAST('2015-12-1' AS date) AS LastPurchase  
    UNION SELECT 'Nicolas' as FirstName, 'Anand' AS LastName, 'Seine (Paris)' AS StateProvince, 6 AS CountryRegionID, 'Male' AS Gender, CAST(1425.00 AS money) AS YTDPurchase, CAST('2015-12-11' AS date) AS LastPurchase  
    UNION SELECT 'James' AS FirstName, 'Peters' AS LastName, 'England' AS StateProvince, 12 AS CountryRegionID, 'Male' AS Gender, CAST(887.50 AS money) AS YTDPurchase, CAST('2015-8-15' AS date) AS LastPurchase  
    UNION SELECT 'Alison' AS FirstName, 'Nath' AS LastName, 'Alaska' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(607.50 AS money) AS YTDPurchase, CAST('2015-10-13' AS date) AS LastPurchase  
    UNION SELECT 'Grace' AS FirstName, 'Patterson' AS LastName, 'Kansas' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(1215.00 AS money) AS YTDPurchase, CAST('2015-10-18' AS date) AS LastPurchase  
    UNION SELECT 'Bobby' AS FirstName, 'Sanchez' AS LastName, 'North Dakota' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(6191.00 AS money) AS YTDPurchase, CAST('2015-9-17' AS date) AS LastPurchase  
    UNION SELECT 'Charles' AS FirstName, 'Reed' AS LastName, 'Nebraska' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(8772.00 AS money) AS YTDPurchase, CAST('2015-8-27' AS date) AS LastPurchase  
    UNION SELECT 'Orlando' AS FirstName, 'Romeo' AS LastName, 'Texas' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(8578.00 AS money) AS YTDPurchase, CAST('2015-7-29' AS date) AS LastPurchase  
    UNION SELECT 'Cynthia' AS FirstName, 'Randall' AS LastName, 'Utah' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(7218.10 AS money) AS YTDPurchase, CAST('2015-1-11' AS date) AS LastPurchase  
    UNION SELECT 'Rebecca' AS FirstName, 'Roberts' AS LastName, 'Washington' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(8357.80 AS money) AS YTDPurchase, CAST('2015-10-28' AS date) AS LastPurchase  
    UNION SELECT 'Cristian' AS FirstName, 'Petulescu' AS LastName, 'Wisconsin' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(3470.00 AS money) AS YTDPurchase, CAST('2015-11-30' AS date) AS LastPurchase  
    UNION SELECT 'Cynthia' AS FirstName, 'Randall' AS LastName, 'Utah' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(7218.10 AS money) AS YTDPurchase, CAST('2015-1-11' AS date) AS LastPurchase  
    UNION SELECT 'Rebecca' AS FirstName, 'Roberts' AS LastName, 'Washington' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(8357.80 AS money) AS YTDPurchase, CAST('2015-10-28' AS date) AS LastPurchase  
    UNION SELECT 'Cristian' AS FirstName, 'Petulescu' AS LastName, 'Wisconsin' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(3470.00 AS money) AS YTDPurchase, CAST('2015-11-30' AS date) AS LastPurchase  
    ```  

  
10. 在查询设计器工具栏中，单击“运行”( **!** )。 结果集在以下列中显示 23 行数据：“FirstName”、“LastName”、“StateProvince”、“CountryRegionID”、“Gender”、“YTDPurchase”和“LastPurchase”。  

    ![“新建表”或“矩阵”向导的“设计查询步骤”的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-query-as-text.png)
  
11. 单击“下一步”。  
  
12. 在“排列字段”页上，将以下字段按指定顺序从“可用字段”列表拖到“值”列表  。  
  
    -   StateProvince   
    -   CountryRegionID  
    -   LastPurchase  
    -   YTDPurchase  
  
    因为 CountryRegionID 和 YTDPurchase 包含数值数据，所以，SUM 聚合将默认应用于这两个字段，但你不希望对其进行求和。  
   
13. 在“值”列表中，右键单击“CountryRegionID”，然后取消选中“Sum”复选框  。  
  
    Sum 不再应用于 CountryRegionID。  
  
14. 在“值”列表中，右键单击“YTDPurchase”，然后单击“Sum”选项  。  
  
    Sum 不再应用于 YTDPurchase。  
    
    ![值列表的屏幕截图，其中显示了已准备好清除的 Sum 选项。](../reporting-services/media/report-builder-expression-not-sum.png)
  
15. 单击“下一步”。  
  
16. 在“选择布局”页上，保留所有默认设置，然后单击“下一步” 。  

    ![“新建表”或“矩阵”向导的“选择布局”步骤的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-choose-layout.png)
  
17. 单击“完成”。  
  
## <a name="2-update-default-names-of-the-data-source-and-dataset"></a><a name="UpdateNames"></a>2.更新数据源和数据集的默认名称  
  
### <a name="to-update-the-default-name-of-the-data-source"></a>更新数据源的默认名称  
  
1.  在“报表数据”窗格中，展开“数据源”文件夹。  
  
2.  右键单击“DataSource1”，然后单击“数据源属性”。  
  
3.  在“名称”框中，键入“ExpressionsDataSource”   
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-update-the-default-name-of-the-dataset"></a>更新数据集的默认名称  
  
1.  在“报表数据”窗格中，展开“数据集”文件夹。  
  
2.  右键单击“DataSet1”，然后单击“数据集属性”。  

    ![屏幕截图：如何访问报表生成器中的“数据集属性”。](../reporting-services/media/report-builder-expression-tutorial-rename-dataset.png)
  
3.  在“名称”框中，键入 **Expressions**  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="3-display-first-initial-and-last-name"></a><a name="Concatenate"></a>3.显示名字首字母和姓氏  
在本部分，在表达式中使用 **Left** 函数和 **Concatenate** ( **&** ) 运算符（其计算结果为包括首字母和姓氏的名称）。 你可以逐步生成表达式，也可以跳过一些步骤，将表达式从教程复制/粘贴到“表达式”对话框中。   
  
1.  右键单击“StateProvince”列，指向“插入列”，然后单击“左”。  
  
    一个新列添加到“StateProvince”列的左侧。 
    
    ![显示如何将左侧列插入报表的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-insert-column.png) 
  
2.  单击新列的标题，然后键入 **Name** 。  
  
3.  右键单击“Name”列的数据单元，然后单击“表达式”。  

    ![显示如何将表达式插入报表的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-insert-expression.png)
  
4.  在“表达式”对话框中，展开“常见函数”，然后单击“文本”。  
  
5.  在“项”列表中，双击“左” 。  
  
    将 **Left** 函数添加到表达式中。  
    
    ![显示如何将 Left 函数添加到表达式的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-left-function.png)
  
6.  在“类别”列表中，单击“字段(表达式)”。  
  
7.  在“值”列表中，双击 **FirstName** 。  
  
8.  键入 **, 1)**  
  
    此表达式将从 **FirstName** 值提取一个字符，从左侧开始计数。  
  
9. 键入“&". "&”  

    这将在表达式后添加一个句点和一个空格。
  
10. 在“值”列表中，双击 **LastName** 。  
  
    完成的表达式： `=Left(Fields!FirstName.Value, 1) &". "& Fields!LastName.Value`  
    
    ![显示如何将 LastName 值添加到表达式的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-complete-name-expression.png)
  
11. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
12. 单击 **“运行”** 以预览报表。  

## <a name="optional-format-the-date-and-currency-columns-and-header-row"></a><a name="DateFormat"></a>（可选）设置日期列、货币列和标题行的格式  
在此部分中，设置“Last Purchase”列和“YTDPurchase”列的格式，此两列分别包含日期和货币。 同样设置标题行的格式。  
  
### <a name="to-format-the-date-column"></a>设置日期列的格式  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  选择“Last Purchase”列中的数据单元格，然后在“开始”选项卡上，转至“数字”部分，选择“日期”   。  

    ![显示如何将“Last Purchase”列设置为日期的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-date-format.png)
  
3.  另外，请在“数字”部分中，单击“占位符样式”旁边的箭头，然后选择“示例值”  。 

    ![显示报表生成器中的“示例值”选项的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-sample-values.png)

    现可看到所选格式设置的示例。 
  
### <a name="to-format-the-currency-column"></a>设置货币列的格式

- 选择“YTDPurchase”列中的数据单元格，然后在“数字”部分，选择“货币符号”  。
 
### <a name="to-format-the-column-headers"></a>设置列标题的格式

1. 选择列标题所在行。

2. 在“开始”选项卡上，转至“段落”部分，选择“左”  。 

    ![显示如何在报表生成器中设置标题格式的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-format-headings.png)

3. 单击 **“运行”** 以预览报表。 

以下是目前为止的报表，具有设置了格式的日期、货币和列标题。

![屏幕截图：已设置格式的报表的预览。](../reporting-services/media/report-builder-expression-tutorial-preview-formatted.png)

  
## <a name="4-use-color-to-display-gender"></a><a name="Gender"></a>4.使用颜色显示性别  
在本部分中，将添加颜色以显示人员性别。 将添加一个新列以显示这些颜色，然后基于“性别”字段的值确定在列中显示的颜色。  
  
将该报表制作为镶边报表时，若要保留在该表单元格中使用的颜色，添加一个矩形，然后向该矩形添加背景色。  
    
 
### <a name="to-add-an-mf-column"></a>添加 M/F 列  
  
1.  右键单击“Name”列，指向“插入列”，然后单击“左”  。  
  
    一个新列添加到“Name”列的左侧。  
  
2.  单击新列的标题，然后键入 **M/F** 。  
  
### <a name="to-add-a-rectangle"></a>添加矩形  
  
1.   在“插入”选项卡上，单击“矩形”，然后单击“M/F”列的数据单元  。  
  
     将在该单元中添加一个矩形。  
     
     ![显示如何插入矩形的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-insert-rectangle.png)
  
2. 拖动“M/F”和“Name”列之间的分隔符，使“M/F”列变窄  。

    ![显示如何使列变窄的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-narrow-column.png)
  
### <a name="to-use-color-to-show-gender"></a>使用颜色显示性别  
  
1.  右键单击“M/F”列的数据单元中的矩形，然后单击“矩形属性” 。  
  
2.  在“矩形属性”对话框中，转至“填充”选项卡，单击“填充颜色”旁边的表达式“fx”按钮   。  
  
3.  在“表达式”对话框中，展开“常见函数”，然后单击“程序流”。  
  
4.  在“项”列表中，双击“Switch” 。  
  
5.  在“类别”列表中，单击“字段(表达式)”。  
  
6.  在“值”列表中，双击 **Gender** 。  
  
7.  键入 **="Male",** （包括逗号）。

8. 在“类别”列表中，单击“常量”，然后在“值”框中，单击“青蓝色”   。

    ![显示如何使用颜色显示性别的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-color-expression-cornflower-blue.png)

9. 在其后键入一个逗号。 
  
5.  在“类别”列表中，单击“字段(表达式)”，然后在“值”列表中，再次双击“Gender”   。  
  
7.  键入 **="Female",** （包括逗号）。 

8. 在“类别”列表中，单击“常量”，然后在“值”框中，单击“番茄色”   。

13. 在其后键入一个右括号 **)** 。 
  
    完成的表达式： `=Switch(Fields!Gender.Value ="Male", "CornflowerBlue",Fields!Gender.Value ="Female","Tomato")`  
    
    ![显示“表达式”对话框中完整表达式的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-color-expression-complete.png)
  
12. 单击“确定”，然后再次单击“确定”关闭“矩形属性”对话框  。  
  
14. 单击 **“运行”** 以预览报表。  

    ![显示带有 M/F 列的预览的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-preview-m-f-column.png)

### <a name="to-format-the-color-rectangles"></a>设置彩色矩形的格式

1. 单击 **“设计”** 返回设计视图。  

16. 在“M/F”列选择该矩形。 在“边框”部分的“属性”窗格中，设置以下属性：

    - BorderColor = 白色
    - BorderStyle = 实线
    - BorderWidth = 5pt
    
    ![显示如何设置 M/F 列中的彩色矩形格式的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-format-m-f-column.png)

18. 单击“运行”再次预览报表。 此次，颜色块周围具有空白区域。

    ![屏幕截图：在 M/F 列中设置了矩形格式的预览。](../reporting-services/media/report-builder-expression-tutorial-preview-formatted-m-f-column.png)  
  
## <a name="5-look-up-the-countryregion-name"></a><a name="Lookup"></a>5.查找 CountryRegion 名称  
在此部分，创建 CountryRegion 数据集，使用 **Lookup** 函数显示国家/地区的名称，而非国家/地区的标识符。  
  
### <a name="to-create-the-countryregion-dataset"></a>创建 CountryRegion 数据集  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  在“报表数据”窗格中，单击“新建”，然后单击“数据集”。  
  
3.  在“数据集”属性中，单击“使用在我的报表中嵌入的数据集”。  
  
4.  在“数据源”列表中，选择“ExpressionsDataSource”。  
  
5.  在“名称”框中，键入“CountryRegion”   
  
6.  确保已选择“文本”查询类型，然后单击“查询设计器” 。  
  
7.  单击 **“编辑为文本”** 。  
  
8.  复制并将以下查询粘贴到查询窗格中：  
  
    ```  
    SELECT 1 AS ID, 'American Samoa' AS CountryRegion  
    UNION SELECT 2 AS CountryRegionID, 'Australia' AS CountryRegion  
    UNION SELECT 3 AS ID, 'Canada' AS CountryRegion  
    UNION SELECT 4 AS ID, 'Germany' AS CountryRegion  
    UNION SELECT 5 AS ID, 'Micronesia' AS CountryRegion  
    UNION SELECT 6 AS ID, 'France' AS CountryRegion  
    UNION SELECT 7 AS ID, 'United States' AS CountryRegion  
    UNION SELECT 8 AS ID, 'Brazil' AS CountryRegion  
    UNION SELECT 9 AS ID, 'Mexico' AS CountryRegion  
    UNION SELECT 10 AS ID, 'Japan' AS CountryRegion  
    UNION SELECT 10 AS ID, 'Australia' AS CountryRegion  
    UNION SELECT 12 AS ID, 'United Kingdom' AS CountryRegion  
    ```  
  
9. 单击 **“运行”** ( **!** ) 以运行查询。  
  
    查询结果是国家/地区标识符和名称。  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
11. 再次单击“确定”以关闭“数据集属性”对话框。  

     现在，“报表数据”列有了第二个数据集。
  
### <a name="to-look-up-values-in-the-countryregion-dataset"></a>查找 CountryRegion 数据集中的值  
  
1.  单击“国家/地区 ID”列标题，并删除文本“ID”，以显示“国家/地区”。  
  
2.  右键单击“Country Region”列的数据单元，然后单击“表达式”。  
  
3.  删除表达式，开头的等号 (=) 除外。  
  
    剩余表达式为： `=`  
  
4.  在“表达式”对话框中，展开“常见函数”，然后单击“杂项”，在“项”列表中，双击“查找”。  
  
6.  在“类别”列表中，单击“字段(表达式)”，然后在“值”列表中，再次双击“CountryRegionID”   。  
  
8.  立即将光标放在 `CountryRegionID.Value`后，然后键入 **,Fields!ID.value, Fields!CountryRegion.value, "CountryRegion")**  
  
    完成的表达式： `=Lookup(Fields!CountryRegionID.Value,Fields!ID.value, Fields!CountryRegion.value, "CountryRegion")`  
  
    **Lookup** 函数的语法指定对表达式数据集的 CountryRegionID 和 CountryRegion 数据集中 ID 进行查找，返回 CountryRegion 数据集中的 CountryRegion 值。  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
11. 单击 **“运行”** 以预览报表。  
  
## <a name="6-count-days-since-last-purchase"></a><a name="Count"></a>6.计算自上次采购后的天数  
在本部分中，添加一列，然后使用 **Now** 函数或 `ExecutionTime` 内置全局变量计算客户自上次采购至今的天数。  
  
### <a name="to-add-the-days-ago-column"></a>添加 Days Ago 列  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  右键单击“Last Purchase”列，指向“插入列”，然后单击“右”  。  
  
    一个新列将添加到“Last Purchase”列的右侧。  
  
3.  在列标题中，键入“Days Ago”  
  
4.  右键单击“Days Ago”列的数据单元格，然后单击“表达式” 。  
  
5.  在“表达式”对话框中，展开“常见函数”，然后单击“日期和时间”  。  
  
6.  在“项”列表中，双击 **DateDiff** 。  
  
7.  紧随 `DateDiff(`，键入 **"d",** （包括引号 "" 和逗号）。 
  
9. 在“类别”列表中，单击“字段(表达式)”，然后在“值”列表中，再次双击“LastPurchase”   。  
  
11. 紧随 `Fields!LastPurchase.Value`，键入 **,** （逗号）。 
  
13. 在“类别”列表中，再次单击“日期和时间”，然后在“项”列表中，双击“现在”   。  
  
    > [!WARNING]  
    > 在生产报表中，不应在报表呈现过程中会多次计算的表达式中（例如，在报表的详细信息行中）使用 **Now** 函数。 **Now** 的值随行变化，不同的值会影响表达式的计算结果，这将导致结果略微不一致。 相反，使用 `ExecutionTime` 提供的 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 全局变量。  
  
15. 删除 `Now(`后的左括号，然后键入右括号 **)**  
  
    完成的表达式： `=DateDiff("d", Fields!LastPurchase.Value, Now)`  
    
    ![显示自上次采购以来日期的完整表达式的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-date-since-last-purchase.png)
  
17. [!INCLUDE[clickOK](../includes/clickok-md.md)]  

11. 单击 **“运行”** 以预览报表。  
  
## <a name="7-use-an-indicator-to-show-sales-comparison"></a><a name="Indicator"></a>7.使用指示器显示销售情况对比  
在本部分中，添加一个新列，使用指示器显示某个人的年初至今 (YTD) 采购量是高于还是低于平均 YTD 采购量。 **Round** 函数从值中去掉小数部分。  
  
需通过几个步骤配置指示器及其状态。 如果需要，可以跳过“配置指示器”过程，将已完成的表达式从本教程复制/粘贴到“表达式”对话框。  
  
### <a name="to-add-the--or---avg-sales-column"></a>添加 + or - AVG Sales 列  
  
1.  右键单击“YTD Purchase”列，指向“插入列”，然后单击“右”  。  
  
    将一个新列添加到“YTD Purchase”列的右侧。  
  
2.  单击列标题，然后键入 **+ or - AVG Sales**  
  
### <a name="to-add-an-indicator"></a>添加指示器  
  
1.  在“插入”选项卡上，单击“指示器”，然后单击“+ or - AVG Sales”列中的数据单元格  。  
  
    “选择指示器类型”对话框即会打开。  
  
2.  在图标集的“方向”组中，单击由三个灰色箭头构成的图标集。  

    ![显示如何添加指示器的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-select-indicator.png)
  
3.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-configure-the-indicator"></a>配置指示器  
  
1.  右键单击指示器，单击“指示器属性”，然后单击“值和状态” 。  
  
2.  单击“值”文本框旁边的表达式“fx”按钮 。  
  
3.  在“表达式”对话框中，展开“常见函数”，然后单击 **Math** 。  
  
4.  在“项”列表中，双击 **Round** 。  
  
5.  在“类别”列表中，单击“字段(表达式)”，然后在“值”列表中，再次双击“YTDPurchase”   。  
  
7.  紧随 `Fields!YTDPurchase.Value`，键入  **-** （减号）。 
  
9. 再次展开“常见函数”，单击“聚合”，然后在“项”列表中，双击“Avg”   。  
  
11. 在“类别”列表中，单击“字段(表达式)”，然后在“值”列表中，再次双击“YTDPurchase”   。  
  
13. 紧随 `Fields!YTDPurchase.Value`，键入 **, "Expressions"))**  
  
    完成的表达式： `=Round(Fields!YTDPurchase.Value - Avg(Fields!YTDPurchase.Value, "Expressions"))`  
  
15. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
16. 在“状态度量单位”框中，选择“数字”。  
  
17. 在具有向下箭头的行中，单击“起始”值的文本框右侧的 **fx** 按钮。  

    ![显示如何在“开始”文本框旁选择“fx”按钮的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-indicator-start.png)
  
18. 在“表达式”对话框中，展开“常见函数”，然后单击 **Math** 。  
  
19. 在“项”列表中，双击 **Round** 。  
  
20. 在“类别”列表中，单击“字段(表达式)”，然后在“值”列表中，再次双击“YTDPurchase”   。  
  
22. 紧随 `Fields!YTDPurchase.Value`，键入  **-** （减号）。 
  
24. 再次展开“常见函数”，单击“聚合”，然后在“项”列表中，双击“Avg”   。  
  
26. 在“类别”列表中，单击“字段(表达式)”，然后在“值”列表中，再次双击“YTDPurchase”   。  
  
28. 紧随 `Fields!YTDPurchase.Value`，键入 **, "Expressions")) < 0**  
  
    完成的表达式： `=Round(Fields!YTDPurchase.Value - Avg(Fields!YTDPurchase.Value, "Expressions")) < 0`  
  
30. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
31. 在“结束”值的文本框中，键入 **0**  
  
32. 单击具有水平方向箭头的行，然后单击“删除”。  

    ![显示如何删除指示器的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-delete-indicator-state.png)
    
    现在仅有两个箭头，向上或向下。
  
33. 在具有向上箭头的行的“起始”框中，键入 **0**  
  
34. 单击“结束”值的文本框右侧的 **fx** 按钮。  
  
35. 在“表达式”对话框中，删除“100”并创建表达式：`=Round(Fields!YTDPurchase.Value - Avg(Fields!YTDPurchase.Value, "Expressions")) >0`   
  
36. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
37. 再次单击“确定”以关闭“指示器属性”对话框。  
  
38. 单击 **“运行”** 以预览报表。  

    ![屏幕截图：“+ or - AVG Sales”列的预览，其中包含所有新指示器。](../reporting-services/media/report-builder-expression-tutorial-preview-indicator.png)
  
## <a name="8-make-a-banded-report"></a><a name="GreenBar"></a>8.制作镶边报表  
创建参数以便报表读者可指定要应用于报表的交替行的颜色，使报表成为镶边报表。  
  
### <a name="to-add-a-parameter"></a>添加参数  
  
1.  单击 **“设计”** 返回设计视图。  
  
2.  在“报表数据”窗格中，右键单击“参数”，然后单击“添加参数”。  

    ![显示如何添加参数的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-add-parameter.png)
  
    此时将打开 **“报表参数属性”** 对话框。  
  
3.  在“提示符”下，键入“选择颜色”   
  
4.  在“名称”中，键入 **RowColor**  
  
5.  在“可用值”选项卡上，单击“指定值” 。  
  
7.  单击“添加”。  
  
8.  在“标签”框中，键入“黄色”   
  
9. 在“值”框中，键入 **Yellow**  
  
10. 单击“添加”。  
  
11. 在“标签”框中，键入“绿色”  
  
12. 在“值”框中，键入“淡绿色”  
  
13. 单击“添加”。  
  
14. 在“标签”框中，键入“蓝色”  
  
15. 在“值”框中，键入“浅蓝色”  
  
16. 单击“添加”。  
  
17. 在“标签”框中，键入“粉色”  
  
18. 在“值”框中，键入 **Pink**  

    ![“报表参数属性”对话框的屏幕截图，其中显示“选择此参数的可用值”步骤。](../reporting-services/media/report-builder-expression-tutorial-parameter-available.png)
  
19. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-apply-alternating-colors-to-detail-rows"></a>将交替颜色应用于详细信息行  
  
1.   选择数据行中的所有单元格， **M/F** 列中具有自己的背景色的单元格除外。  

     ![显示数据行中已选择的单元格的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-select-banded.png)
  
4.  在“属性”窗格中，单击 **BackgroundColor** 。 

     如果未看到“属性”窗格，则在“视图”选项卡上，选中“属性”框 。  
  
    如果在“属性”窗格中按类别列出属性，会在“杂项”类别中发现“BackgroundColor” 。  
  
5.  单击向下箭头，然后单击“表达式”。  

    ![“属性”框的屏幕截图，其中显示如何将表达式与 BackgroundColor 关联。](../reporting-services/media/report-builder-expression-tutorial-banded-color-property.png)
  
6.  在“表达式”对话框中，展开“常见函数”，然后单击“程序流”。  
  
7.  在“项”列表中，双击“IIf”。  
  
8.  在“常见函数”下，单击“杂项”，然后在“项”列表中，双击“RowNumber”   。  

9. 紧随 **RowNumber(** ，键入 **Nothing) MOD 2,**
  
8. 单击“参数”，在“值”列表中，双击“RowColor”。  
  
22. 紧随 `Parameters!RowColor.Value`，键入 , “White”)  
  
    完成的表达式： `=IIF(RowNumber(Nothing) MOD 2, Parameters!RowColor.Value, "White")`  
    
    ![显示完整的镶边彩色表达式的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-banded-color-expressn.png)
  
24. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="run-the-report"></a>运行报表  
  
1.  在“开始”选项卡上，单击“运行” 。  

    现在运行报表时，在为非白色镶边选择颜色之前无法看到报表。
  
3.  在“选择颜色”列表中，为报表中的非白色镶边选择颜色。  
    
    ![显示如何为非白色镶边选择颜色的屏幕截图。](../reporting-services/media/report-builder-expression-tutorial-select-color.png)
  
4.  单击 **“查看报表”** 。  
  
    该报表呈现，报表行交替呈现您所选的背景。 
    
    ![屏幕截图：预览呈现交替颜色的行。](../reporting-services/media/report-builder-expression-tutorial-preview-banded.png) 
  
## <a name="optional-add-a-report-title"></a><a name="Title"></a>（可选）添加报表标题  
为报表添加标题。  
  
### <a name="to-add-a-report-title"></a>添加报表标题  
  
1.  在设计图面上，单击“单击以添加标题”。  
  
2.  键入 **Sales Comparison Summary** ，然后选择文本。  
  
3.  在“开始”选项卡上的“字体”框中，进行如下设置 ：

    -  大小 = 18
    -  颜色 = 灰色
    -  加粗
  
4.  在“开始”选项卡上，单击“运行” 。  
  
3.  为报表中的非白色镶边选择颜色，然后单击“查看报表”。  
  
## <a name="optional-save-the-report"></a><a name="Save"></a>（可选）保存报表  
您可以将报表保存到报表服务器、SharePoint 库或本地计算机。 有关详细信息，请参阅[保存报表（报表生成器）](../reporting-services/report-builder/saving-reports-report-builder.md)。  
  
在本教程中，将报表保存到报表服务器。 如果您没有对报表服务器的访问权限，则可以保存到您的计算机。  
  
### <a name="to-save-the-report-to-a-report-server"></a>将报表保存到报表服务器  
  
1.  在“文件”菜单上，转至“另存为” 。  
  
2.  单击 **“最近使用的站点和服务器”** 。  
  
3.  选择或键入您拥有保存报表权限的报表服务器的名称。  
  
    此时将显示“正在连接到报表服务器”消息。 当连接完成时，您将看到报表服务器管理员指定为默认报表位置的报表文件夹的内容。  
  
4.  为报表提供名称，然后单击“保存”。  
  
报表即已保存至报表服务器。 您连接的报表服务器的名称将显示在窗口底部的状态栏中。

现在，报表读者可以在 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] Web 门户中查看报表。

![已完成的新报表的屏幕截图，其中每个表达式都可见。](../reporting-services/media/report-builder-expression-tutorial-final-in-browser.png)

   
## <a name="see-also"></a>另请参阅  
[表达式（报表生成器和 SSRS）](../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
[表达式示例（报表生成器和 SSRS）](../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)  
[指示器（报表生成器和 SSRS）](../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
[图像、文本框、矩形和线条（报表生成器和 SSRS）](../reporting-services/report-design/images-text-boxes-rectangles-and-lines-report-builder-and-ssrs.md)  
[表（报表生成器和 SSRS）](../reporting-services/report-design/tables-report-builder-and-ssrs.md)  
[报表数据集 (SSRS)](../reporting-services/report-data/report-datasets-ssrs.md)  
  
  
  

