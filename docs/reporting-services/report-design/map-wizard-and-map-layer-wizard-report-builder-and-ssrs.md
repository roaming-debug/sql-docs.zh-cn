---
title: 地图向导和地图层向导（报表生成器）| Microsoft Docs
description: 了解如何使用报表生成器中的地图向导或地图层向导自动创建地图、添加地图层或更改地图层选项。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: reference
f1_keywords:
- sql13.rtp.rptdesigner.mapandlayerwizard.f1
- "10542"
- MICROSOFT.REPORTDESIGNER.MAPLAYER.NAME
ms.assetid: 48cbe18b-1290-4107-8a1c-ec6acd71f73b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b307966577cfd30197963263597aee7a9d06e018
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597331"
---
# <a name="map-wizard-and-map-layer-wizard-report-builder-and-ssrs"></a>地图向导和地图层向导（报表生成器和 SSRS）
 在 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 分页报表中，地图向导和地图层向导可以自动执行创建地图、添加地图层或更改现有层上的地图层选项的任务。  
  
 向报表添加地图或向地图添加地图层之前，请收集以下信息：  
  
-   **空间数据源。** 提供空间数据的源的位置或连接，例如，包含空间数据的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例和数据库的名称，或是 Environmental Systems Research Institute, Inc.(ESRI) 形状文件的名称。  
  
-   **Spatial data.** 来自空间数据源，是包含用于指定位置的一系列坐标的字段。  
  
-   **分析数据。** 用于改变地图显示的分析数据，例如，年度商店销售额。  
  
-   **匹配字段。** 定义空间数据和分析数据之间关系的匹配字段，例如，唯一标识每个城市的地区和城市的名称。  
  
 以下部分提供了有关在地图向导和地图层向导中可以指定的选项的信息。  
  
-   首次打开报表生成器时，请单击设计图面中心区域上的 **“地图”** 向导图标。  
  
-   在 **“插入”** 选项卡上，单击 **“地图”** ，然后单击 **“地图向导”** 。  
  
 若要打开地图层向导，请执行以下操作之一：  
  
-   单击地图以显示“地图”窗格和工具栏，然后单击 **“新建层向导”** 按钮。  
  
 单击向导页标题以获得相应的帮助内容。 您看到的页面取决于您选择的地图类型、空间数据源和分析数据源。  
  
1.  [选择空间数据的源](#SpatialDataSource)。 空间数据可来自地图库、Environmental Systems Research Institute, Inc. (ESRI) 形状文件，或来自 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 关系数据库中的空间数据。  
  
    -   [什么是空间数据？](#SpatialData)  
  
    -   [什么是地图库？](#MapGallery)  
  
    -   [什么是 ESRI 形状文件？](#Shapefile)  
  
    -   [在何处可以获取 ESRI 形状文件？](#GetShapefiles)  
  
    -   [什么是 SQL Server 空间查询？](#SqlServerSpatial)  
  
2.  [选择空间数据和地图视图选项](#MapView)。 设置地图视图、地图分辨率，指定是否在报表中嵌入空间数据，并指定是否包括 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Bing 地图图块的图块背景。  
  
    -   [什么是地图视图或视区？](#Viewport)  
  
    -   [什么是地图分辨率或优化？](#Resolution)  
  
    -   [嵌入空间数据有什么作用？](#Embed)  
  
    -   [什么是 Bing 地图图块背景？](#Tiles)  
  
3.  [选择地图可视化](#Visualization)。 选择要创建的地图的类型。  
  
    -   [基本图、气泡图和分析图之间有什么区别？](#MapType)  
  
    -   选择地图可视化：多边形  
  
    -   选择地图可视化：线条  
  
    -   选择地图可视化：点  
  
4.  选择数据源连接 选择地图可视化：点： 选择或创建一个与包含要在地图上显示的分析数据的外部数据源的数据源连接。  
  
5.  设计查询。 生成指定分析数据的查询。  
  
6.  [选择分析数据集](#AnalyticalData)。 指定分析数据的数据源。  
  
    -   [空间数据和分析数据之间有什么区别？](#Diff)  
  
7.  [为空间数据和分析数据指定匹配字段](#SpecifyMatchFields)。 在空间数据和分析数据之间建立关系，以便地图元素的外观可随数据变化。  
  
    -   [什么是匹配字段？](#MatchFields)  
  
8.  [选择颜色主题和数据可视化](#ThemeandVisualization)。 若要指定如何在地图背景上实现数据的可视化，请指定地图主题、要实现可视化的字段，以及要改变的内容：颜色、大小和/或标记类型。  
  
    -   [主题有什么作用？](#Theme)  
  
    -   [地图预览中的图例和比例尺有什么作用？](#Legends)  
  
    -   [什么是规则？](#Rules)  
  
 添加地图或地图层并预览报表之后，您可以更改在向导中设置的地图和地图层选项。 有关详细信息，请参阅[自定义地图或地图层的数据和显示（报表生成器和 SSRS）](../../reporting-services/report-design/customize-the-data-and-display-of-a-map-or-map-layer-report-builder-and-ssrs.md)。  
  
 有关地图的详细信息，请参阅 [地图（报表生成器和 SSRS）](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)。 有关向报表添加地图的分步说明，请参阅[教程：地图报表（报表生成器）](../../reporting-services/tutorial-map-report-report-builder.md)。  
  
##  <a name="choose-a-source-of-spatial-data"></a><a name="SpatialDataSource"></a> 选择空间数据的源  
 在此页上，指定空间数据源和要包括的空间数据。 空间数据可来自地图库或 ESRI 形状文件，也可来自数据集查询（此类查询指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或更高版本数据库中的 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 空间数据）。  
  
 可以为每层使用相同或不同的空间数据源，但必须在每次添加层时指定源。 当空间数据来自地图库或 ESRI 形状文件时，空间数据源不是单独的报表项。 它不显示在“报表数据”窗格中。  
  
###  <a name="what-is-spatial-data"></a><a name="SpatialData"></a> 什么是空间数据？  
 空间数据包含定义地理或几何元素的坐标。 在地图中，空间数据定义“地图元素” ：定义区域或形状的多边形、定义路线或路径的线条，以及定义标记或图钉的点。 空间数据在数据源中以二进制格式存储，并指定为坐标集。 例如，点是 X 和 Y 坐标 (X Y)，线条是两组坐标 ((X1 Y1), (X2 Y2))，多边形是四组或更多坐标，其中第一组和最后一组坐标是相同的 ((X1 Y1), (X2 Y2), (X3 Y3), (X1 Y1))。  
  
 有关详细信息，请参阅所用空间数据类型的文档。  
  
###  <a name="what-is-the-map-gallery"></a><a name="MapGallery"></a> What is the map gallery?  
 地图库所包含的地图来自面向报表创作环境的地图库文件夹中的报表。 库中的地图提供支持向报表快速添加地图。 库中预先定义的地图是由地图提供商提供的。  
  
> [!NOTE]  
>  这一 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 地图功能使用的数据来自经美国人口免费获得 ([https://www.census.gov/](https://www.census.gov/))。 TIGER/Line 形状文件是从 Census MAF/TIGER 数据库中精选的地理和制图信息的摘录。 TIGER/Line 形状文件可以从美国人口普查局免费获得。 若要获取关于 TIGER/Line 形状文件的详细信息，请转到 [TIGER/Line 形状文件和 TIGER/Line 文件技术文档](https://www.census.gov/programs-surveys/geography/technical-documentation/complete-technical-documentation/tiger-geo-line.html)。 TIGER/Line 形状文件中的边界信息仅用于统计数据收集和制表目的；其中用于统计目的的描述和名称不构成对于司法机构、所有权或享有权利的界定，它们不是法律上关于领土的说明。 Census TIGER 和 TIGER/Line 是美国人口普查局的注册商标。  
  
 若要扩展地图库，可以在地图库目录中添加或删除报表，并添加文件夹对地图进行组织。 有关详细信息，请参阅[地图（报表生成器和 SSRS）](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)。  
  
###  <a name="what-is-an-esri-shapefile"></a><a name="Shapefile"></a> What is an ESRI shapefile?  
 ESRI 形状文件是一个文件集，其中的数据符合 Environmental Systems Research Institute, Inc. (ESRI) 形状文件空间数据格式。 该文件集通常包含 \<filename>.shp 文件（包含空间数据）和一个支持文件 \<filename>.dbf 。  
  
 当您将形状文件指定为空间数据源而该文件位于您的本地计算机中时，空间数据将自动嵌入到报表中。 若要动态使用 ESRI 文件中的空间数据，必须执行以下操作：  
  
 在报表生成器中，将 .shp 文件及 .dbf 文件同时上载到报表服务器上的同一文件夹，然后链接到作为空间数据源的 .shp 文件。  
  
 在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中的报表设计器中，将 .shp 文件及 .dbf 文件同时添加到报表项目中，然后将该 .shp 文件的名称指定为空间数据源。  
  
###  <a name="where-can-i-get-esri-shapefiles"></a><a name="GetShapefiles"></a> 在何处可以获取 ESRI 形状文件？  
 在网络上提供 ESRI 形状文件。 有关详细信息，请参阅 [Finding ESRI Shapefiles for a Map](https://go.microsoft.com/fwlink/?linkid=178814)（查找用于地图的 ESRI 形状文件）。  
  
###  <a name="what-is-a-sql-server-spatial-query"></a><a name="SqlServerSpatial"></a> 什么是 SQL Server 空间查询？  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 空间查询是一种数据集查询，该查询指定来自 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 关系数据库的 SQLGeometry 或 SQLGeography 数据类型的数据。  
  
> [!NOTE]  
>  在向导中定义数据源时，您将在“设计查询”页中看到不同的查询设计器，具体取决于您所连接的数据源的类型。 有关详细信息，请参阅[查询设计工具 (SSRS)](../report-data/query-design-tools-ssrs.md)。  
  
 在查询设计器中运行该查询时，结果集显示一列，其中的空间数据显示为文本。 例如，一行可能包含作为一个点的空间数据，而下一行可能包含定义一组点的空间数据。 每行都成为一个地图元素。 您可以改变每个图元素的显示，将其作为不可分的单元。  
  
 有关详细信息，请参阅 [空间数据类型](../../relational-databases/spatial/spatial-data-types-overview.md)。  
  
##  <a name="choose-spatial-data-and-map-view-options"></a><a name="MapView"></a> 选择空间数据和地图视图选项  
 在此页上可以设置以下选项：  
  
-   为在上一向导页中选定的空间数据设置视图中心和缩放级别。 您设置的视图适用于整个地图。  
  
-   设置地图分辨率。  
  
-   指定是否在报表中嵌入空间数据。  
  
-   对于嵌入数据，指定是包括所有数据还是仅包括当前视图中的数据。  
  
-   指定是否包括 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Bing 地图图块背景。  
  
###  <a name="what-is-the-map-view-or-viewport"></a><a name="Viewport"></a> 什么是地图视图或视区？  
 地图视区定义要为报表中的所有层显示的地图区域。  
  
 默认情况下，色阶和距离刻度显示在视区内，地图图例显示在视区外。 可以在完成向导操作后更改这些视区选项。  
  
###  <a name="what-is-map-resolution-and-optimization"></a><a name="Resolution"></a> 什么是地图分辨率和优化？  
 当您更改表示线条或多边形的空间数据的分辨率时，您是在指定要绘制的地图的详细程度。 例如，对于区域的空中视图，您是需要将粒度降到地球上百米的表面积，还是一英里的分辨率就足够了？  
  
 如果报表中嵌入了空间数据，较高的分辨率将增加按该分辨率绘制详细信息所需的元素数。 如果报表中未嵌入空间数据，较高的分辨率将在您每次查看报表时增加报表处理器按该分辨率计算地图中线条所需的时间。  
  
 降低分辨率后，报表处理器将应用一种算法来减少绘制地图元素所需的点数。 分辨率越低，显示地图元素所需的数据就越少，这样可以提高显示性能。  
  
 调整滑块时，向导窗格中的预览数据随之更新，为您指示调整的影响。 将地图添加到报表后，可以通过更改地图视区选项来调整此值。  
  
###  <a name="what-does-embedding-spatial-data-do"></a><a name="Embed"></a> 嵌入空间数据有什么作用？  
 在报表中嵌入地图元素或 Bing 地图图块时，空间数据存储在报表定义中。  
  
 带地图的报表可以使用在处理报表时动态检索到的空间数据或 Bing 地图图块，或是在设计时创建然后嵌入报表定义的空间数据或 Bing 地图图块。 嵌入的地图元素可能会显著增加报表定义的大小，但会减少在报表中查看地图所需的时间。 动态地图元素减少报表定义大小，但会增加处理和查看地图所需的时间。  
  
 优秀的报表设计要求您在静态和动态地图数据之间权衡利弊，找出适合您所在环境的平衡点。 总之，更多数据意味着报表定义和编译的报表要求在报表服务器上占有更大的存储空间，以及较长的处理时间。 剪裁空间数据并限制其他报表数据，以便仅包括报表所需信息，这始终不失为一种好的做法。  
  
###  <a name="what-is-a-bing-map-tile-background"></a><a name="Tiles"></a> 什么是 Bing 地图图块背景？  
 若要向地图中添加地理图像背景，请选择 Bing 地图图块背景选项。 报表处理器会为在此向导页上指定的地图区域和分辨率从 Bing 地图 Web 服务下载图块。 可以指定下列图块类型之一：  
  
-   **道路：** 显示背景为白色的道路地图样式。  
  
-   **空中：** 仅显示鸟瞰图。 这种模式下不显示任何文本。  
  
-   **混合。** 显示组合起来的 **“道路”** 和 **“空中”** 视图。  
  
 有关图块的详细信息，请参阅 [Bing 地图图块系统](/bingmaps/articles/bing-maps-tile-system)。 有关在报表中使用 Bing 地图图块的详细信息，请参阅 [其他使用条款](https://go.microsoft.com/fwlink/?LinkId=151371)。  
  
 若要在设计视图中查看图块背景，您必须能访问 Internet。 若要通过报表服务器上的报表以预览模式查看图块背景，报表服务器必须配置为支持 Bing 地图图块。 有关详细信息，请参阅[排除报表故障：地图报表（报表生成器和 SSRS）](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md)和[计划地图报表](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md)。  
  
 有关自定义图块层的其他方式的详细信息，请参阅[添加、更改或删除地图或地图层（报表生成器和 SSRS）](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md)。  
  
##  <a name="choose-map-visualization"></a><a name="Visualization"></a> 选择地图可视化  
 在此页上，选择要添加到报表的地图或地图层的类型。 首次运行该向导时，将向报表添加地图以及第一个地图层。 一个地图可以包含多个地图层。 每个地图层都显示一种特定类型的空间数据：多边形、线条或点。  
  
 您选择的地图类型取决于地图的用途和可用的数据。  
  
###  <a name="what-is-the-difference-among-a-basic-map-a-bubble-map-and-an-analytical-map"></a><a name="MapType"></a> 基本图、气泡图和分析图之间有什么区别？  
 **“基本图”** 只显示位置。 您可以通过阴影来改变地图上区域的颜色，但颜色不代表分析数据值。  
  
 **“气泡图”** 将单个分析数据聚合的相关值表示为气泡大小，例如，商店销售额。 可以为多边形或点创建气泡图。 对于多边形，设置多边形中心点属性；对于点，设置标记属性。  
  
 **“分析图”** 表示每个地图元素的一个或多个分析数据聚合的相关值。 例如，将商店销售额表示为标记大小，将产品类别的利润范围表示为标记颜色，将畅销产品表示为标记类型。  
  
 有关详细信息，请参阅 [规划地图报表（报表生成器和 SSRS）](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md)。  
  
##  <a name="choose-the-analytical-dataset"></a><a name="AnalyticalData"></a> 选择分析数据集  
 在此页上，指定要在此地图层上显示的分析数据的来源。  
  
 若要在地图背景上显示报表数据或任何分析数据，必须指定数据来源以及如何将这些数据关联到空间数据。 数据可以来自现有报表数据集，或来自针对其建立查询的新数据集。 现有分析数据可以包括在包含空间数据的 ESRI 形状文件中。  
  
###  <a name="what-is-the-difference-between-spatial-data-and-analytical-data"></a><a name="Diff"></a> 空间数据和分析数据之间有什么区别？  
 空间数据包含指定点、线条和多边形的一系列坐标。 地图元素基于空间数据。  
  
 分析数据是您要用来改变地图外观的数值或类别数据。 分析数据可以来自报表数据集，也可能随地图库或 ESRI 形状文件中的某个地图的空间数据附带。  
  
##  <a name="specify-the-match-fields"></a><a name="SpecifyMatchFields"></a> 指定匹配字段  
 在此页上，建立空间数据与分析数据之间的关系。  
  
###  <a name="what-are-match-fields"></a><a name="MatchFields"></a> 什么是匹配字段？  
 匹配字段支持报表处理器在分析数据与空间数据之间建立关系。 匹配字段指定分析数据内的唯一值。 例如，商店名称在数据中可能不唯一，所以可以同时指定城市和商店的名称。  
  
##  <a name="choose-color-theme-and-data-visualization"></a><a name="ThemeandVisualization"></a> 选择颜色主题和数据可视化  
 在此页上，指定如何在地图背景上使数据可视、指定地图主题、要可视化的字段，以及要改变的内容：颜色、大小和/或标记类型。  
  
###  <a name="what-does-the-theme-do"></a><a name="Theme"></a> 主题有什么作用？  
 所选主题设置颜色、边框和字体的默认值。 可以在完成向导操作后更改这些选项。  
  
###  <a name="what-are-the-legends-and-scales-in-map-preview-for"></a><a name="Legends"></a> 地图预览中的图例和比例尺有什么作用？  
 图例可帮助用户解释地图上显示的数据。 地图中提供了色阶、距离刻度和图例。  
  
-   **颜色范围。** 颜色范围显示一个带刻度的颜色条，刻度用于提供由报表处理器根据你为层指定的规则所确定的数据间隔指南。  
  
-   **距离刻度：** 距离刻度用于提供地图上的距离单位指南。 距离单位将根据地图投影和缩放级别自动确定。  
  
-   **图例。** 图例用于提供帮助解释地图上的颜色、大小和标记类型含义的指南。 默认情况下，所有层的所有规则都会在第一个图例中显示数据间隔。 将地图添加到报表后，可以自定义此图例以及添加图例。  
  
###  <a name="what-are-rules"></a><a name="Rules"></a> 什么是规则？  
 规则是报表处理器将分析数据划分为范围所使用的计算。 可以为每一层指定不同的规则。 可以指定的规则类型取决于层上空间数据的类型：  
  
-   **多边形：** 可以指定颜色规则。  
  
-   **多边形中心点：** 可以指定颜色、大小和标记类型规则。  
  
-   **线条：** 可以指定颜色和宽度规则。  
  
-   **点：** 可以指定颜色、大小和标记类型规则。  
  
 报表处理器应用您设置的规则，并自动确定要显示在图例中的项列表。 默认情况下，所有层的所有规则的结果都会显示在第一个图例中。 您可以在完成向导操作后对此进行调整。 有关详细信息，请参阅 [按规则和分析数据更改多边形、线条和点的显示方式（报表生成器和 SSRS）](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md)。  
  
## <a name="see-also"></a>另请参阅  
 [报表故障排除：映射报表（报表生成器和 SSRS）](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md)   
 [规划地图报表（报表生成器和 SSRS）](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md)   
 [地图（报表生成器和 SSRS）](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)  
  
