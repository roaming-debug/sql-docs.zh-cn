---
title: 教程：地图报表（报表生成器）| Microsoft Docs
description: 了解可用于在 Reporting Services 分页报表中在地理背景上显示数据的地图功能。
ms.date: 08/31/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 8d831356-7efa-40cc-ae95-383b3eecf833
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 14efcfa864cde1fd932c92c79d802efcdd2118c4
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243673"
---
# <a name="tutorial-map-report-report-builder"></a>教程：地图报表（报表生成器）
在本 [!INCLUDE[ssRBnoversion_md](../includes/ssrbnoversion.md)] 教程中，将了解可用于在 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 分页报表中针对地理背景显示数据的地图功能。 
  
地图以空间数据为基础，这些数据通常包含点、线条和多边形。 例如，多边形可以表示国家/地区轮廓，线条可以表示道路，而点则可以表示市县所在位置。 各种类型的空间数据作为一组地图元素显示在单独的地图层上。  
  
若要改变地图元素的外观，可以指定一个字段，通过字段值将地图元素与数据集中分析数据相匹配。 还可以定义相关规则，依据数据范围改变颜色、大小、或其他属性。  

![显示报表生成器地图的屏幕截图。](../reporting-services/media/report-builder-map-final-map-only.png)
  
在本教程中，将生成一个地图报表，该报表显示了纽约州各县内的商店位置。  
   
> [!NOTE]  
> 在本教程中，将向导的多个步骤合并为两个过程：一个用于创建数据集，一个用于创建表。 有关如何浏览到报表服务器、选择数据源、创建数据集和运行向导的分步说明，请参阅本系列的第一个教程：[教程：创建基本表报表（报表生成器）](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md)。  
  
完成本教程的估计时间：30 分钟。  
  
## <a name="requirements"></a>要求  
对于本教程，报表服务器必须配置为支持将 Bing 地图作为背景。 有关详细信息，请参阅 [计划地图报表支持](./report-design/plan-a-map-report-report-builder-and-ssrs.md)。 

有关其他要求的信息，请参阅[教程先决条件（报表生成器）](../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
## <a name="1-create-a-map-with-a-polygon-layer-from-the-map-wizard"></a><a name="Map"></a>1.通过地图向导使用多边形层创建地图  
在本部分中，从地图库向报表中添加地图。 该地图具有一个层，此层显示了纽约州中的各个县。 各县的形状为根据地图库中的地图内嵌入的空间数据得出的多边形。  
  
### <a name="to-add-a-map-with-the-map-wizard-in-a-new-report"></a>使用地图向导在新报表中添加地图  
  
1.  通过计算机、[Web 门户或 SharePoint 集成模式](../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 。  
  
    将打开“新建报表或数据集”对话框。  
  
    如果未出现“新建报表或数据集”对话框，请通过“文件”菜单转至“新建”  。  
  
2.  在左窗格中，确认已选中 **“新建报表”** 。  
  
3.  在右窗格中，单击“地图向导”。  
  
4.  在“选择空间数据的来源”页上，确认已选中“地图库” 。  
  
6.  在“地图库”框中，展开“美国”下的“州/按县”，然后单击“纽约”  。  
  
    此时，“地图预览”窗格将显示纽约的县地图。  
    
    ![屏幕截图：“新建地图”向导的“选择空间数据的来源”步骤，其中已在“地图库”部分中调出“纽约”。](../reporting-services/media/report-builder-map-ny-counties.png)
  
7.  单击“下一步”。  
  
8.  在“选择空间数据和地图视图选项”页上，接受默认值，然后按“下一步” 。 
 
    默认情况下，来自地图库的地图元素会自动嵌入到报表定义中。  
  
9. 在“选择地图可视化”页上，确认已选中“基本地图”，然后单击“下一步”  。  
  
11. 在“选择颜色主题和数据可视化”页上，选择“显示标签”选项 。  
  
12. 如果已选择，则清除“单色图”选项。  
  
13. 从“数据字段”下拉列表中，单击“#COUNTYNAME” 。 向导中的“地图预览”窗格显示以下各项：  
  
    -   一个标题，其文本为“地图标题”。  
  
    -   一个地图，显示纽约的各个县，其中每个县都用一种不同的颜色表示，且县名称出现在县区域上方适合的位置。  
  
    -   一个图例，包含标题和项 1 至 5 的列表。  
  
    -   一个色阶，包含值 0 到 160 但没有颜色。  
  
    -   一个距离宽度，显示公里数 (km) 和英里数 (mi)。  
    
    ![新建地图向导的“选择颜色主题和数据可视化”步骤的屏幕截图。](../reporting-services/media/report-builder-map-choose-color-theme.png)
  
14. 单击“完成”。  
  
    此时，将向设计图面添加一个地图。  
  
13. 选择“地图标题”文本并键入“商店销售额”，然后按 Enter。  

15. 双击地图，显示“地图层”窗格。 “地图层”窗格显示一个层类型为“嵌入”的多边形层 PolygonLayer1 。 每个县都是该层上的一个嵌入地图元素。  
  
    > [!NOTE]  
    > 如果没有看到“地图层”窗格，则它可能在当前视图之外显示。 请使用位于“设计”视图窗口底部的滚动条来更改视图。 或者，在“视图”选项卡中，清除“报表数据”选项，提供更多的设计图面区域 。   

15. 选择“PolygonLayer1”旁边的箭头，然后单击“多边形属性”。

16. 在“字体”选项卡上，将颜色更改为“暗灰色” 。

17. 在“开始”选项卡上，单击“运行”预览报表 。  
  
    ![显示新创建的报表生成器地图预览的屏幕截图。](../reporting-services/media/report-builder-map-first-preview.png)
  
呈现的报表显示地图标题、地图以及距离刻度。 各县位于地图多边形层上。 各个县均为多边形，以调色板中的颜色区分，但颜色并不与任何数据关联。 距离刻度同时用公里和英里显示距离。  
  
并不显示地图图例和色阶，因为没有任何与各县关联的分析数据。 稍后，您将在本教程中添加分析数据。  
  
## <a name="2-add-a-map-point-layer-to-display-store-locations"></a><a name="PointLayer"></a>2.添加地图点层以显示商店位置  
在本部分中，将使用地图层向导添加一个点层，用于显示商店的位置。  
  
> [!NOTE]  
> 在本教程中，由于查询包含了数据值，因此它不需要外部数据源。 这样，查询就会非常长。 在业务环境中，查询不会包含数据。 本教程中的查询仅供学习使用。  
  
### <a name="to-add-a-point-layer-based-on-a-sql-server-spatial-query"></a>基于 SQL Server 空间查询添加点层  
  
1.  在“运行”选项卡上，单击“设计”切换回“设计”视图 。  
  
2.  双击地图以显示“地图层”窗格。 在工具栏上，单击“新建层向导”按钮 ![rs_IconMapLayerWizard](../reporting-services/media/rs-iconmaplayerwizard.gif "rs_IconMapLayerWizard")。 

    ![显示地图层窗格的屏幕截图，其中已调出“新建层向导”图标。](../reporting-services/media/report-builder-map-new-layer-wizard-icon.png) 
  
3.  在“选择空间数据的来源”页上，选择“SQL Server 空间查询”，然后单击“下一步”  。  
  
4.  在“选择具有 SQL Server 空间数据的数据集”页上，单击“添加具有 SQL Server 空间数据的新数据集” > “下一步”  。  
  
5.  在“选择与 SQL Server 空间数据源的连接”页上，选择现有数据源，或浏览到报表服务器并选择数据源。  

    > [!NOTE]  
    > 只要您具有足够的权限，则选择哪一个数据源并不重要。 您将不会从数据源中获取数据。 有关详细信息，请参阅[获取数据连接的备选方式（报表生成器）](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md)。  
  
6.  单击“下一步”。  
  
7.  在 **“设计查询”** 页中，单击 **“编辑为文本”** 。  
  
8.  复制以下文本并粘贴到查询窗格中：  
  
    ```  
    Select 114 as StoreKey, 'Contoso Albany Store' as StoreName, 1125 as SellingArea, 'Albany' as City, 'Albany' as County,   
     CAST(1000000 as money) as Sales, CAST('POINT(-73.7472924218681 42.6564617079878)' as geography) AS SpatialLocation  
    UNION ALL SELECT 115 AS StoreKey, 'Contoso New York No.1 Store' AS  StoreName, 500 as SellingArea, 'New York' AS City, 'New York City' as County,  
     CAST('2000000' as money) as Sales, CAST('POINT(-73.9922069374483 40.7549638237402)' as geography) AS SpatialLocation  
    UNION ALL Select 116 as StoreKey, 'Contoso Rochester No.1 Store' as StoreName, 462 as SellingArea, 'Rochester' as City,  'Monroe' as County,    
     CAST(3000000 as money) as Sales, CAST('POINT(-77.624041566786 43.1547066024338)' as geography)  AS SpatialLocation  
    UNION ALL Select 117 as StoreKey, 'Contoso New York No.2 Store' as StoreName, 700 as SellingArea, 'New York' as City,'New York City' as County,    
      CAST(4000000 as money) as Sales, CAST('POINT(-73.9712488 40.7830603)' as geography) AS SpatialLocation  
    UNION ALL Select 118 as StoreKey, 'Contoso Syracuse Store' as StoreName, 680 as SellingArea, 'Syracuse' as City, 'Onondaga' as County,  
     CAST(5000000 as money) as Sales, CAST('POINT(-76.1349120532546 43.0610223535974)' as geography) AS SpatialLocation  
    UNION ALL Select 120 as StoreKey, 'Contoso Plattsburgh Store' as StoreName, 560 as SellingArea, 'Plattsburgh' as City,  'Clinton' as County,  
     CAST(6000000 as money) as Sales, CAST('POINT(-73.4728622833178 44.7028831413324)' as geography) AS SpatialLocation  
    UNION ALL Select 121 as StoreKey, 'Contoso Brooklyn Store' as StoreName, 1125 as SellingArea, 'Brooklyn' as City, 'New York City' as County,  
     CAST(7000000 as money) as Sales, CAST('POINT (-73.9638533447143 40.6785123489351)' as geography) AS SpatialLocation  
    UNION ALL Select 122 as StoreKey, 'Contoso Oswego Store' as StoreName, 500 as SellingArea, 'Oswego' as City, 'Oswego' as County,    
     CAST(8000000 as money) as Sales, CAST('POINT(-76.4602850815536 43.4353224527794)' as geography) AS SpatialLocation  
    UNION ALL Select 123 as StoreKey, 'Contoso Ithaca Store' as StoreName, 460 as SellingArea, 'Ithaca' as City, 'Tompkins' as County,  
     CAST(9000000 as money) as Sales, CAST('POINT(-76.5001866085881 42.4310489934743)' as geography) AS SpatialLocation  
    UNION ALL Select 124 as StoreKey, 'Contoso Buffalo Store' as StoreName, 700 as SellingArea, 'Buffalo' as City, 'Erie' as County,    
     CAST(100000 as money) as Sales, CAST('POINT(-78.8784 42.8864)' as geography) AS SpatialLocation  
    UNION ALL Select 125 as StoreKey, 'Contoso Queens Store' as StoreName, 700 as SellingArea,'Queens' as City, 'New York City' as County,  
     CAST(500000 as money) as Sales, CAST('POINT(-73.7930979029883 40.7152781765927)' as geography) AS SpatialLocation  
    UNION ALL Select 126 as StoreKey, 'Contoso Elmira Store' as StoreName, 680 as SellingArea, 'Elmira' as City, 'Chemung' as County,  
     CAST(800000 as money) as Sales, CAST('POINT(-76.7397414783301 42.0736492742663)' as geography) AS SpatialLocation  
    UNION ALL Select 127 as StoreKey, 'Contoso Poestenkill Store' as StoreName, 455 as SellingArea, 'Poestenkill' as City, 'Rensselaer' as County,    
    CAST(1500000 as money) as Sales, CAST('POINT(-73.5626737425063 42.6940551238618)' as geography) AS SpatialLocation  
    ```  
  
9. 在查询设计器工具栏中，单击“运行”( **!** )。  
  
    结果集包含七列，这些列表示纽约州的一组销售消费品的商店。 以下是列表，并对意思可能不明显的列进行说明： 
    *   **StoreKey** ：店铺标识符。  
    *   **StoreName** 。
    *   **SellingArea** ：产品展示区域，面积介于 455 平方英尺和 1,125 平方英尺之间。
    *   **City** 。
    *   **County** 。
    *   **销售** ：总销售额。 
    *   **SpatialLocation** ：以经度和纬度表示的位置。 

    ![“新建地图层”向导的“设计查询”步骤的屏幕截图。](../reporting-services/media/report-builder-map-design-query.png) 
  
10. 单击“下一步”。  
  
    此时，将会为您创建一个名为 DataSet1 的报表数据集。 在完成向导后，可以在“报表数据”窗格中看到它的字段集合。  
  
11. 在“选择空间数据和地图视图选项”页上，确认“空间字段”为“SpatialLocation”，且“层类型”为“点”。 接受本页上的其他默认值。  
  
    地图视图显示圆圈，这些圆圈标记每个商店的位置。  
  
12. 单击“下一步”。  
  
13. 在“选择地图可视化”页上，单击“气泡图”地图类型，该地图类型根据数据显示不同大小的标记。 单击“下一步”。  
  
14. 在“选择分析数据集”页上，单击“DataSet1”，然后单击“下一步” 。 此数据集同时包含分析数据和空间数据，它将显示在新的点层上。   
  
16. 在“选择颜色主题和数据可视化”页上，选择“使用气泡大小实现数据的可视化效果” 。  
  
17. 在“数据字段”中选择 `[Sum(SellingArea)]`，根据商店为展示产品而保留的区域的大小来改变气泡大小。  
  
18. 选择“显示标签”，然后在“数据字段”中选择 `[City]` 。

18. 单击“完成”。  
  
    将向报表添加该地图层。 图例根据“SellingArea”值显示气泡大小。  
  
 19. 双击地图以显示“地图层”窗格。 “地图层”窗格显示新层“PointLayer1”，以及空间数据源类型“DataRegion” 。  
  
19. 添加图例标题。 在图例中，选择文本“标题”，键入“显示区域（平方英尺）”，然后按 Enter 。  
  
21. 在“地图层”窗格中，单击“PointLayer1”旁边的箭头，然后单击“点属性” 。  

    ![显示“点属性”选项的“地图层”窗格的屏幕截图。](../reporting-services/media/report-builder-map-point-properties.png)
  
22. 在“字体”选项卡上，将样式设为“加粗”，将大小设为“10 磅”  。

    ![显示“地图点属性”对话框的“更改字体、大小和颜色选项”部分的屏幕截图。](../reporting-services/media/report-builder-map-point-properties-font.png)
  
23. 在“常规”选项卡上，将“位置”选择为“底部”  。

24. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
24. 单击 **“运行”** 以预览报表。  

    ![显示报表生成器地图预览的屏幕截图，其中城市名称显示为粗体 10 磅格式。](../reporting-services/media/report-builder-map-city-names.png)
  
    该地图显示纽约州的商店的位置。 每个商店的标记大小基于显示区域。 系统会自动为您计算五个范围的显示区域。


  
## <a name="3-add-a-map-line-layer-to-display-a-route"></a><a name="LineLayer"></a>3.添加地图线条层以显示路线  
使用地图层向导添加一个显示两个商店间路线的地图层。 本教程中，通过三个商店位置创建路径。 在业务应用程序中，路径可能是两个商店间的最佳路线。  
  
### <a name="to-add-a-line-layer-to-map"></a>向地图添加线条层  
  
1.  切换到“设计”视图。  
  
2.  双击地图以显示“地图层”窗格。 在工具栏上，单击“新建层向导”按钮 ![rs_IconMapLayerWizard](../reporting-services/media/rs-iconmaplayerwizard.gif "rs_IconMapLayerWizard")。  
  
3.  在“选择空间数据的来源”页上，选择“SQL Server 空间查询”，然后单击“下一步”。  
  
4.  在“选择具有 SQL Server 空间数据的数据集”页上，单击“添加具有 SQL Server 空间数据的新数据集”，然后单击“下一步”。  
  
5.  在“选择与 SQL Server 空间数据源的连接”中，选择在第一步中使用的数据源。  
  
6.  单击“下一步”。  
  
7.  在 **“设计查询”** 页中，单击 **“编辑为文本”** 。 查询设计器切换到基于文本的模式。  
  
8.  将以下文本粘贴到查询窗格中：  
  
    ```  
    SELECT N'Path' AS Name, CAST('LINESTRING(  
       -76.5001866085881 42.4310489934743,  
       -76.4602850815536 43.4353224527794,  
       -73.4728622833178 44.7028831413324)' AS geography) as Route  
    ```  
  
9. 单击“下一步”。  
  
    此时，地图上将显示一条连接三个商店的路径。  
  
10. 在“选择空间数据和地图视图选项”页上，确认“空间字段”为“路线”，且“层类型”为“线条”。 接受其他默认值。  
  
    地图视图显示一条从位于纽约州北部的商店到位于纽约州南部商店的路径。  
  
11. 单击“下一步”。  
  
12. 在“选择地图可视化”页上，单击“基本线条图”，然后单击“下一步”  。  
  
13. 在“选择颜色主题和数据可视化”上，选择“单色图”选项 。 该路径基于所选主题显示为某种颜色。  
  
14. 单击“完成”。  

    ![显示带有线条层的报表生成器地图的屏幕截图。](../reporting-services/media/report-builder-map-line.png)
  
     地图显示一个空间数据源类型为“DataRegion”的新线条层。 在本例中，空间数据来自数据源，但没有分析数据与此线条关联。  

## <a name="adjust-the-zoom"></a>调整缩放比例
1. 如果看不到整个纽约州，可以调整缩放比例。 选中地图，在“属性窗格”中可以看到“MapViewport”属性。 

15. 展开“视图”部分，然后展开“视图”，此时可以看到“缩放”属性  。 将它设置为“125”。 

    ![显示如何调整报表生成器地图缩放效果的屏幕截图。](../reporting-services/media/report-builder-map-zoom.png)

      这是缩放百分比。 缩放百分比为 125% 时，应该可以看到整个州。
  
## <a name="4-add-a-bing-maps-tile-background"></a><a name="TileLayer"></a>4.添加 Bing 地图图块背景  
在本部分中，将添加一个地图层，用于显示 Bing 地图图块背景。  
  
1.  切换到“设计”视图。  
  
2.  双击地图以显示“地图层”窗格。 在工具栏上，单击“添加层”![rs_IconMapAddLayer](../reporting-services/media/rs-iconmapaddlayer.gif "rs_IconMapAddLayer")。  
  
3.  从下拉列表中，单击“图块层”。  
  
    “地图层”窗格中的最后一层为“TileLayer1”。 默认情况下，图块层显示道路图样式。  
  
    > [!NOTE]  
    > 在本向导中，还可以在“选择空间数据和地图视图选项”页上添加图块层。 若要执行此操作，请选择“为该地图视图添加必应地图背景”。 在呈现的报表中，图块背景为当前地图视区中心和缩放级别显示 Bing 地图图块。  
  
4.  单击“TileLayer1”旁边的箭头，然后单击“图块属性”。  
  
5.  在“常规”选项卡的“类型”下，选择“空中”  。 空中视图不包含文本。  

    ![屏幕截图：添加到报表生成器地图的必应地图空中图块层。](../reporting-services/media/report-builder-map-bing-aerial.png)
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="5-make-a-layer-transparent"></a><a name="Transparent"></a>5.将层设置为透明  
在本部分中，要让某一层上的项透过另一层显示出来，可以调整层的顺序以及透明度，从而获得想要的效果。 从创建的第一层（即“PolygonLayer1”）开始。 
  
1.  双击地图以显示“地图层”窗格。  
  
3.  单击“PolygonLayer1”旁边的箭头，然后单击“层数据”。 将打开“地图多边形层属性”对话框。  
  
4.  在“可见性”选项卡的“透明度(百分比)”下，键入“30”  。  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
     设计图面将县显示为半透明。  

    ![显示如何设置报表生成器地图层的透明度的屏幕截图。](../reporting-services/media/report-builder-map-transparency.png)
  
## <a name="6-vary-county-color-based-on-sales"></a><a name="Vary"></a>6.根据销售改变县颜色  
多边形层上的每个县都有一种不同的颜色，因为报表处理器会根据您在地图向导的最后一页选择的主题，自动从调色板中分配一个颜色值。  
  
在本部分中，指定颜色规则，以便将特定的颜色与每个县的商店销售额范围关联起来。 颜色红-黄-绿指示相应的高-中-低销售额。 设置色阶的格式以显示货币。 在新的图例中显示年销售额范围。 对于不包含商店的县，不使用任何颜色，以指明没有关联的数据。  
  
### <a name="6a-build-a-relationship-between-spatial-and-analytical-data"></a><a name="Relationship"></a>6a. 在空间数据与分析数据之间建立关系  
若要基于分析数据改变县形状中的颜色，首先需要将分析数据与空间数据关联起来。 在本教程中，您将使用要匹配的县名称。 
  
1.  切换到“设计”视图。  
  
2.  双击地图以显示“地图层”窗格。  
  
3.  单击“PolygonLayer1”旁边的箭头，然后单击“层数据”。 将打开“地图多边形层属性”对话框。  
  
4.  在“分析数据”选项卡的“分析数据集”下，选择“DataSet1” 。 此数据集是为县创建空间数据查询时由向导创建的。  
  
6.  在“要匹配的字段”下，单击“添加” 。 将添加一个新行。  
  
7.  在“来自空间数据集”下，单击“COUNTYNAME”。  
  
8.  在“来自分析数据集”下，单击“[County]”。  

    ![显示“地图多边形层属性”对话框中的“分析数据”选项卡的屏幕截图。](../reporting-services/media/report-builder-map-county-colors.png)
  
9. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
10. 预览报表。  

    ![报表生成器地图预览的屏幕截图，其中突出显示了特定县。](../reporting-services/media/report-builder-map-county-highlight.png)
  
通过从空间数据源和分析数据集中指定一个匹配字段，报表处理器可以基于地图元素对分析数据进行分组。 针对您指定的值，数据绑定的地图元素具有成功的匹配项。  
  
对于包含商店的每个县，其颜色取决于您在向导中选择的样式的调色板。 其他县为灰色。  
  
### <a name="6b-specify-color-rules-for-polygons"></a><a name="ColorRules"></a>6b. 为多边形指定颜色规则  
若要创建根据商店销售额改变每个县颜色的规则，必须指定范围值、要显示的范围的划分数以及要使用的颜色。  
  
#### <a name="to-specify-color-rules-for-all-polygons-that-have-associated-data"></a>为具有关联数据的所有多边形指定颜色规则  
  
1.  切换到“设计”视图。  
  
2.  单击“PolygonLayer1”旁边的箭头，然后单击“多边形颜色规则”。 将打开“地图颜色规则属性”对话框。 请注意，已选择“使用调色板实现数据的可视化效果”颜色规则选项。 此选项已由向导进行设置。  
  
3.  选择“使用颜色范围实现数据的可视化效果”。 调色板选项被开始颜色、中间颜色和结束颜色选项取代。  
  
4.  为每个县的销售额定义范围值。 在“数据字段”中，从下拉列表中选择“`[Sum(Sales)]`”。  
  
5.  若要更改格式以便以千为单位显示货币，请将表达式更改为以下形式： `=Sum(Fields!Sales.Value)/1000`  
  
6.  将“开始颜色”更改为“红色”。  
  
7.  将“结束颜色”更改为“绿色” 。  
  
    “红色”表示低销售值，“黄色”表示中等销售值，而“绿色”表示高销售值  。 报表处理器将基于这些值以及在“分布”页上选择的选项来计算颜色范围。  
    
    ![显示“地图颜色规则属性”对话框的“更改多边形颜色规则”部分的屏幕截图。](../reporting-services/media/report-builder-map-county-color-rules.png)
  
8.  单击 **“分布”** 。  
  
9. 确认分布类型为“最佳”。 对于步骤 5 中的表达式，最佳分布将值划分到各个子范围，这些子范围在每个范围中的项数与每个范围的跨度之间实现平衡。  
  
10. 对于本页上的其他选项接受默认值。 如果您选择最佳分布类型，则在运行报表时将计算子范围数。  
  
11. 单击 **“图例”** 。  
  
12. 在“色阶选项”中，确认已选中“在色阶中显示” 。  
  
13. 在“在此图例中显示”中，从下拉列表中选择空行。 现在，您只将颜色范围显示在色阶中。  
  
14. [!INCLUDE[clickOK](../includes/clickok-md.md)]  

15. 预览报表。

    ![屏幕截图：应用了特定颜色规则的报表生成器地图县的预览。](../reporting-services/media/report-builder-map-county-color-rule-preview.png)
  
    色阶显示四种颜色：红色、橙色、黄色和绿色。 每个颜色表示一个销售额范围，此范围是以县为单位根据销售额自动计算得出的。  
  
### <a name="6c-format-the-data-in-the-color-scale-as-currency"></a><a name="ColorScale"></a>6c. 将色阶中的数据的格式设置为“货币”  
默认情况下，数据具有常规格式。 在本部分中，将应用自定义格式。  
  
1. 切换到“设计”视图。  

2. 选择色阶。 在“开始”选项卡上，单击“数字”部分，然后单击“货币”  。  
  
4.  继续在“数字”部分中单击两次“减少小数位数”按钮 。  
  
    色阶对于每个范围用货币格式显示年销售额。  
  
### <a name="6d-add-a-legend-title"></a><a name="NewLegend"></a>6d. 添加图例标题   
  
1.  仍然选中“色阶”，在“属性”窗格中可以看到“MapColorScale”的属性。 
  
2. 展开“标题”部分，然后在“标题”属性中键入 **Sales (Thousands)** 。

3. 将“TextColor”属性更改为“白色”。  

    ![显示如何配置图例标题的屏幕截图。](../reporting-services/media/report-builder-map-color-scale-title.png)
  
8.  预览报表。  
  
具有关联的商店和销售额的县根据颜色规则进行显示。 没有销售额的县没有颜色。  
  
### <a name="6f-change-color-for-counties-with-no-data"></a><a name="NoData"></a>6f. 更改没有数据的县的颜色  
可以为层上所有地图元素设置默认显示选项。 颜色规则优先于这些显示选项。  
  
#### <a name="to-set-the-display-properties-for-all-elements-on-a-layer"></a>为层上的所有元素设置显示属性  
  
1.  切换到“设计”视图。  
  
2.  双击地图以显示“地图层”窗格。  
  
3.  单击“PolygonLayer1”上的向下箭头，然后单击“多边形属性”。 

     ![显示如何选择“多边形属性”选项的屏幕截图。](../reporting-services/media/report-builder-map-polygon-layer-properties.png)

     将打开“地图多边形属性”对话框。 在应用基于规则的显示选项之前，此对话框中设置的显示选项将应用于层上的所有多边形。  
  
4.  在“填充”选项卡上，确认填充样式为“纯色” 。 。渐变样式和图案样式应用于所有颜色。  
  
6.  在“颜色”中，选择“浅钢蓝色” 。  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
8.  预览报表。  
  
不具有关联数据的县显示为灰蓝色。 只有那些具有关联的分析数据的县才会根据指定的颜色规则具有从“红色”到“绿色”的颜色。  
  
## <a name="7-add-a-custom-point"></a><a name="CustomPoint"></a>7.添加自定义点  
为了表示尚未建立的新商店，将在本部分中指定一个点并使用“星形”标记类型。  
  
1.  切换到“设计”视图。  
  
2.  双击地图以显示“地图层”窗格。 在工具栏上，单击“添加层”![rs_IconMapAddLayer](../reporting-services/media/rs-iconmapaddlayer.gif "rs_IconMapAddLayer")，然后单击“点层”。  
  
    将向地图添加一个新点层。 默认情况下，该点层的空间数据类型为“嵌入”。  
  
3.  单击“PointLayer2”上的箭头，然后单击“添加点”。  
  
4.  将指针移到地图视区上方。 光标将变为十字准线。  
  
5.  单击地图上您要添加点的位置。 在本教程中，单击奥奈达县中的某个位置。 用圆圈标记的点将添加到层中单击过的位置。 默认情况下，该点处于选中状态。  

    ![显示如何将自定义点添加到报表生成器地图的屏幕截图。](../reporting-services/media/report-builder-map-custom-point.png)
  
6.  右键单击添加的点，然后单击“嵌入的点属性”。  
  
7.  选择“覆盖此层的点选项”。 其他页将显示在对话框中。 您在此处设置的值优先于层或颜色规则的显示选项。  

    ![显示“地图嵌入的点属性”对话框的“更改嵌入点”选项部分的屏幕截图。](../reporting-services/media/report-builder-map-custom-point-general.png)
  
8.  在“标记”选项卡上，将“标记类型”选择为“星形”  。  

10. 将“标记大小”更改为“18 磅” 。
  
3.  在“标签”选项卡的“标签文本”中，键入“新商店”  。  
  
5.  在“位置”中，单击“顶部” 。  

13. 在“字体”选项卡上，将字号设为“10 磅”和“加粗”  。

    ![显示“嵌入的地图点属性”对话框的“更改字体、大小和颜色选项”部分的屏幕截图。](../reporting-services/media/report-builder-map-custom-point-font.png)
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
7.  预览报表。  
  
标签显示在商店位置上方。  

![显示报表生成器地图预览的屏幕截图，其中已调出自定义点新商店。](../reporting-services/media/report-builder-map-custom-point-new-store.png)
  
## <a name="8-center-and-resize-the-map"></a><a name="CenterView"></a>8.中心和调整地图大小   
在本部分中，将学习如何更改地图中心，以及更改缩放级别的另一种方法。  
 
1.  切换到“设计”视图。  

1.  选择地图，然后右键单击，单击“视区属性”。  
  
2.  在“中心和缩放”选项卡上，确保已选择“设置视图中心和缩放级别” 。  

4. 将“缩放级别(百分比)”设置为“125” 。
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
5.  单击地图，将它拖到所需的中心位置。  
  
6.  还可使用鼠标滚轮更改缩放级别。  
  
7.  预览报表。  
  
在“设计”视图中，显示图面上的地图以及视图基于示例数据。 在呈现的报表中，地图视图位于您指定的视图的中心。  
  
## <a name="9-add-a-report-title"></a><a name="Title"></a>9.添加报表标题  
  
1.  切换到“设计”视图。
  
1.  在设计图面上，单击“单击以添加标题”。  
  
2.  键入 **Sales in New York Stores** ，然后在文本框外部单击。  
  
此标题将显示在报表的顶部。 当未定义页眉时，表体顶部的项等同于报表表头。  
  
## <a name="10-save-the-report"></a><a name="Save"></a>10.保存报表  
  
1.  在“设计”视图或“预览”中，在“文件”菜单上单击“另存为” 。
 
3.  在“名称”中，键入“纽约的商店销售额” 。  

3. 将其保存到本地计算机或 [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] 服务器。
  
4. 单击“ **保存** ”。 

如果将其保存到报表服务器，则可在其中进行查看。

![屏幕截图：门户中显示已完成的报表生成器地图。](../reporting-services/media/report-builder-map-in-portal.png) 
  
## <a name="next-steps"></a>后续步骤  
到此为止，我们结束了有关如何向报表添加地图的演练。  
  
有关详细信息，请参阅[地图（报表生成器和 SSRS）](../reporting-services/report-design/maps-report-builder-and-ssrs.md)。  
  
## <a name="see-also"></a>另请参阅  
[报表生成器教程](../reporting-services/report-builder-tutorials.md)  
[SQL Server 中的报表生成器](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
[地图向导和地图层向导（报表生成器和 SSRS）](../reporting-services/report-design/map-wizard-and-map-layer-wizard-report-builder-and-ssrs.md)  
[按规则和分析数据更改多边形、线条和点的显示方式（报表生成器和 SSRS）](../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md)  
