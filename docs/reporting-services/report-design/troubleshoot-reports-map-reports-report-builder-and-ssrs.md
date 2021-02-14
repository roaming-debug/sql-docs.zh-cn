---
title: 报表故障排除：映射报表（报表生成器）| Microsoft Docs
description: 探索在报表生成器中进行自定义、添加层或发布带有地图的报表时可能导致的情况的故障排除建议。
ms.date: 07/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: a690aec2-056b-40bc-8cab-c694bd2d6d62
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a4580559356869ffb52d75241219e42c8511ba3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081158"
---
# <a name="troubleshoot-reports-map-reports-report-builder-and-ssrs"></a>报表故障排除：地图报表（报表生成器和 SSRS）
  当你向报表中添加地图或地图层、自定义报表中的现有地图或地图层、预览报表中的地图或发布具有地图的报表时， [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 分页报表中的地图可能会出现问题。 使用本主题可以帮助解决这些问题。  
    
   ## <a name="need-more-help"></a>需要更多帮助？  
   
  请尝试：  
 * Stack Overflow 上的 [SQL Server Reporting Services](https://stackoverflow.com/questions/tagged/reporting-services)  
 * 在 [Microsoft SQL Server UserVoice](https://feedback.azure.com/forums/908035-sql-server) 上提出问题或建议。  

  
##  <a name="report-definition-size-issues"></a><a name="Embedded"></a> 报表定义大小问题  
 使用这一部分可以帮助解决与报表定义大小相关的问题。  
  
## <a name="how-do-i-reduce-the-report-definition-size"></a>如何减少报表定义大小？  
 地图层包含从空间数据创建的地图元素。 在某些情况下，地图元素嵌入在报表定义中。 这以如下方式发生：  
  
-   如果空间数据的源来自地图库中的地图，或者来自本地计算机上的 ESRI 形状文件，则将在报表定义中自动嵌入地图元素。  
  
     如果您将报表发布到报表服务器，并且对于本地文件具有空间数据源引用，则无法在报表处理期间检索空间数据。 为避免发生此问题，应将地图数据嵌入在报表定义中。  
  
-   在“地图”向导或“层”向导中，如果您选择嵌入空间数据的选项，则基于空间数据的地图元素将嵌入到报表定义的地图层中。  
  
-   在“地图”窗格中，如果右键单击该层，然后单击 **“嵌入空间数据”** 选项之一，则基于空间数据的地图元素将嵌入到报表定义内的地图层中。  
  
-   若要从报表定义中删除基于 ESRI 形状文件的嵌入数据，必须执行以下操作：  
  
1.  将 ESRI .shp 和 .dbf 文件上载或发布到报表服务器。  
  
2.  在报表中，在“设计”视图的“地图”窗格中，选择具有嵌入数据的层，然后打开 **“层数据”** 属性。 在 **“使用的空间数据来自”** 中，选择 **“链接到 ESRI 形状文件”**，然后浏览到报表服务器上包含 ESRI 形状文件的文件夹，选择该文件夹，然后单击“确定”。  
  
3.  保存报表。 您更改的层的嵌入数据已从报表定义中删除。  
  
 来自地图库中某个报表的地图元素将始终嵌入在某个地图层中。  
  
##  <a name="spatial-data-issues"></a><a name="Spatial"></a> 空间数据问题  
 使用这一部分可以帮助解决与空间数据相关的问题。  
  
## <a name="on-the-design-surface-i-see-sample-spatial-data"></a>在设计图面上，我看到了示例空间数据  
 在设计时，出于以下原因，设计图面可能显示有关示例空间数据的消息：  
  
-   空间数据来自于 ESRI .shp 文件，但对应的 .dbf 文件不可用。 ESRI 形状文件通常同时包含一个 .shp 文件（具有空间数据）和一个支持文件 .dbf。 验证 .dbf 文件与 .shp 文件位于同一个目录下。  
  
-   空间数据来自数据集，但此查询的数据连接无效或当前凭据无效。  
  
-   地图层包含一个属性以及一个表达式。 在报表运行之前，不计算表达式的值。 若要查看地图，必须预览报表。  
  
-   空间数据来自具有特定作用域的数据集。 例如，当地图嵌套在 Tablix 数据区域中或地图将相同数据集作用域用于分析数据和空间数据时，将不计算数据作用域，直到报表运行。  
  
## <a name="when-i-set-an-offset-for-an-individual-map-element-a-cluster-of-map-elements-move"></a>当我为单独的地图元素设置偏移量时，一系列地图元素移动  
 空间数据定义显示在每个地图层上的地图元素。 地图元素可以基于空间数据，该空间数据可以为单个点、一组点、单一线条、一组线条、单个多边形或一组多边形。 每个地图元素为一个单元。 如果地图元素包含多个点，在移动该元素时，将移动该地图元素的所有点。  
  
 每个地图元素的数据都由来自外部源的空间数据的格式确定。 例如，当查询指定来自 SQL Server 数据库的空间数据时，结果集中的每行可能包含多个点集、行集或多边形坐标集。 由单一行在结果集中定义的所有地图元素都被视为单位。 如果要改变特定坐标集的显示，必须执行以下操作之一：  
  
-   更改查询以便在结果集中将坐标集作为单独的行返回。  
  
-   选择要改变的地图元素，并通过为对应的层类型覆盖默认的显示属性来设置对应的嵌入点、行或多边形属性。  
  
## <a name="my-layer-that-uses-spatial-data-from-an-esri-shapefile-always-has-embedded-data"></a>使用来自 ESRI 形状文件的空间数据的层始终具有嵌入的数据。  
 若要确保具有地图的报表可以在报表服务器上运行，ESRI 形状文件必须可用作报表服务器上的资源。 如果您将层添加到地图并指定一个位于本地文件系统中的形状文件，则空间数据将自动嵌入到报表中。  
  
 若要将嵌入数据替换为指向 ESRI 形状文件的链接，必须将 .shp 文件及其匹配的 .dbf 文件上载到报表服务器，然后更改该层的空间数据的源。  
  
## <a name="i-renamed-a-data-source-or-dataset-to-a-friendly-name-and-now-no-data-appears-in-my-map"></a>我将数据源或数据集重命名为友好名称后，现在地图中不显示数据。  
 当您手动更改任何报表项的名称时，不会自动更新报表定义。  
  
 当您更改数据集的名称时，必须手动更新引用该数据集的任何数据区域或地图层。 若要将 Tablix、图表或仪表绑定到数据集，请在设计图面上选择该项，打开数据区域属性，然后选择适当数据集的名称。 若要将地图层绑定到数据集，请选择该层，打开层属性，然后选择适当数据集的名称。  
  
## <a name="my-spatial-data-contains-nulls-and-empty-strings"></a>空间数据包含空值或空字符串。  
 在地图报表项的空间数据中，空值设置为零 (0)，而空字符串设置为空白 ("")。  
  
 对于来自 SQL Server 数据库的空间数据，若要更改其行为，必须更改返回空间数据的查询。  
  
## <a name="my-map-exceeds-the-maximum-number-of-spatial-elements"></a>地图超过了最大空间元素数  
 默认情况下，地图可以具有 20,000 个地图元素或 1,000,000 个点。 如果地图超出了这些限制，则可以使用以下方法之一：  
  
-   删除层。  
  
-   降低地图分辨率。  
  
-   减小地图视区坐标以查看较小的区域。  
  
-   如果空间数据来自报表数据集，则设置筛选器以限制数据集中的数据。 必须在不属于空间数据类型的字段上设置此筛选器。  
  
-   如果空间数据来自 SQL Server 数据库，则更改此查询以使用空间函数将数据限制到较小的区域。  
  
##  <a name="viewport-center-and-view-issues"></a><a name="Viewport"></a> 视区居中和视图问题  
 使用这一部分可以帮助解决与视区选项相关的问题。  
  
## <a name="i-cannot-set-the-center-and-view-on-an-embedded-map-element"></a>无法对嵌入的地图元素设置居中和视图。  
 若要使视区在特定地图元素上居中，必须已将层上的空间数据与分析数据关联。  
  
## <a name="i-set-the-map-center-and-view-in-my-report-when-i-reopen-the-report-why-isnt-the-map-view-the-same"></a>我在报表中对地图设置了居中和视图。 当我重新打开报表时，为何地图视图并不相同？  
 如果当您打开报表时，读取空间数据所需的用户凭据对于此报表不可用，将使用占位符空间数据。 根据您为地图视区设置的居中和缩放选项，地图视图可能在不同的层上居中。  
  
 若要重新加载空间数据并使用保存在报表中的地图视图中心，请右键单击地图视区，然后单击 **“重新加载”** 。 在为空间数据源输入凭据之后，该层将加载空间数据，并将还原地图视图。  
  
## <a name="the-center-and-view-for-a-map-layer-option-does-not-work"></a>用于地图层的居中和视图选项不起作用。  
 如果视区设置为对于特定层的空间数据居中，并且视图中心并不显示为该层的中心，则可能是空间数据中包含的小岛或区域过小，而导致在视区中无法看到。 例如，某个国家/地区的空间数据可能包含小岛或其他小地域作为其领土的一部分。 视区使用所有空间数据来计算层的中心。  
  
 若要覆盖层的计算，可以执行以下操作之一：  
  
-   为视区指定自定义中心。  
  
-   更改视区的缩放级别，以消除您不想包含的位置。  
  
-   在报表中嵌入空间数据，并删除您不想包含的位置。  
  
##  <a name="layer-issues"></a><a name="Layers"></a> 层问题  
 使用这一部分可以帮助解决与层选项相关的问题。  
  
## <a name="i-do-not-see-one-or-more-layers-in-my-map"></a>我看不到地图中的一层或多层。  
 您是否能看到报表中的某个地图层取决于以下因素：空间数据的可用性、空间数据与分析数据之间的关系、空间数据的类型和对应的层类型、层上的可见性和透明度选项以及层绘制顺序。 如果您看不到某层的数据，则检查以下选项：  
  
-   **层类型和空间数据类型。** 层类型只显示与该层类型匹配的空间数据。 例如，如果层类型为“点”，但空间数据为“线条”，则不显示任何数据。  
  
-   **匹配字段值。** 您指定要将分析数据与空间数据相关的字段中的值必须唯一标识每个地图元素。 这些字段必须具有相同的数据类型。 字段中的值必须完全相同。 有关详细信息，请参阅 [图例、色阶和距离刻度问题](#Legend)。  
  
-   **层顺序。** “地图”窗格中各层的顺序是在报表呈现器中绘制层时所采用的顺序。 首先绘制的层的空间数据可能会被后面绘制的层的空间数据覆盖。 将首先绘制出现在列表顶部的层。 当您更改层在列表中的顺序时，您是在更改层的绘制顺序。  
  
-   **透明度。** 可以单独为每个地图层指定透明度。 默认的透明度值根据您添加层的方式不同而异。 透明度为 0% 表明该层是不透明的，而没有其他层数据将透过这种方式显示。 若要允许其他数据透过现有层进行显示，请将该值调整为更高的百分比，以提供您所需的效果。  
  
-   **可见性。** 层的可见性为“可见”、“隐藏”或“ZoomBased”，具体根据地图视区的缩放级别而定。   还可以指定最大和最小缩放级别范围。 可见性可以基于一个表达式，该表达式计算为上述三个值之一。  
  
    > [!TIP]  
    >  可以针对“地图”窗格中的每层切换可见性。 当您设计每层时，请关闭所有其他层，以确定问题是针对某个单独层，还是针对层间的透明度问题。  
  
## <a name="i-set-a-filter-on-the-map-layer-and-it-has-no-effect"></a>我对地图层设置了一个筛选器，但它不起作用。  
 若要对某层筛选数据，必须在筛选器表达式中指定数据类型。 请验证您已指定正确的基础数据类型，以便筛选器公式正确地计算指定的条件。 有关详细信息，请参阅[筛选器公式示例（报表生成器和 SSRS）](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md)。  
  
##  <a name="legend-color-scale-and-rule-issues"></a><a name="Legend"></a> 图例、色阶和规则问题  
 使用这一部分可以帮助解决与规则、图例和色阶选项相关的问题。  
  
## <a name="how-do-i-control-the-values-in-the-map-legend"></a>如何控制地图图例中的值？  
 将根据您为每个地图层指定的地图元素类型规则以及您为图例指定的分布规则，自动确定图例值。  
  
 默认情况下，由所有规则生成的所有项显示在第一个图例中。 每层的所有多边形、线条和点规则的值均影响合并的图例范围。 若要在不同的图例中显示项，首先必须创建多个图例，然后对于每个规则，指定在哪个图例中显示相关的项。  
  
 若要将规则与特定图例关联，请打开规则属性，并在“图例”页上选择要使用的图例的名称。 若要从图例中删除项，在图例选项中，为图例的名称选择空行。 如果您对报表中的图例元素进行重命名，则必须手动将每层与适当的图例项关联。  
  
 若要控制每个图例的标题和内容，请将图例属性用于规则。 您可以指定要创建的划分数、更改向每个划分赋值的计算、设置最小和最大范围值以及更改图例文本的格式。  
  
 有关详细信息，请参阅 [更改地图图例、色阶和关联的规则（报表生成器和 SSRS）](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md)。  
  
## <a name="the-rules-that-i-set-do-not-give-the-results-that-i-expect"></a>我设置的规则没有带来我预期的结果。  
 规则应用于与层上的地图元素关联的分析数据。 使用以下列表帮助确定与所有颜色规则、大小规则、宽度规则以及标记类型规则相关的问题：  
  
-   对每个地图元素（多边形、线条、点）应用样式的从低到高的顺序为：层属性、层上所有地图元素的地图元素属性、您指定的规则、然后是您为其选择覆盖选项的嵌入地图元素、您指定的值。 在为嵌入元素选择覆盖选项之后，规则不再适用，即使您后来将值改回为其原始设置。  
  
-   匹配字段问题。 通过匹配字段，可以在地图元素与分析数据之间实现数据绑定。 与匹配字段对应的空间数据字段和分析数据字段必须具有相同的数据类型和相同的格式。 如果匹配字段与空间数据以及分析数据并不完全匹配，则规则不起任何作用。 例如，如果空间数据的匹配字段与分析数据的匹配字段相比，前者具有多余空格或多余标点符号，则不会发生匹配。  
  
-   有关详细信息，请参阅 [按规则和分析数据更改多边形、线条和点的显示方式（报表生成器和 SSRS）](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md)。  
  
## <a name="what-is-the-value-nan-on-the-color-scale"></a>对于色阶，值 NaN 是什么含义？  
 **NaN** 表示“非数字”。 色阶值应为数字值。 对于与色阶关联的规则，检查分布设置和图例文本值。 如果您创建了自定义分布范围，请验证您对第一个范围指定了下限，而对最后范围指定了上限。  
  
## <a name="my-color-scale-does-not-appear-when-i-run-the-report"></a>当我运行报表时，不显示色阶。  
 当地图层为整个层的多边形、线条或点或嵌入地图元素指定颜色规则时，色阶向用户显示信息。 如果没有地图元素指定颜色规则，或者如果地图规则是使用图例而非颜色地图指定的，则呈现的报表中不显示颜色地图。  
  
 若要显示色阶，请为层或嵌入地图元素指定颜色规则。 有关详细信息，请参阅 [更改地图图例、色阶和关联的规则（报表生成器和 SSRS）](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md)。  
  
##  <a name="tile-issues"></a><a name="Tile"></a> 图块问题  
 使用这一部分可以帮助解决与图块背景选项相关的问题。  
  
## <a name="i-cannot-see-the-bing-maps-tile-background"></a>我看不到 Bing 地图图块背景。  
 下面的设置影响 Bing 地图图块背景是显示本地预览中，还是显示在从报表服务器运行的报表上。  
  
-   地图图块层必须存在。 在“地图”向导或“层”向导中，选择 **“为该地图视图添加 Bing 地图背景”** 。 这将为当前地图视区视图中心和缩放级别添加一个图块层。 还可以从“地图”窗格工具栏中添加图块层。  
  
-   视区的地图坐标系必须为 **“地理”** ，而非 **“平面数据”** 。  
  
-   地图投影必须为 **Mercator**。  
  
-   对于本地预览，您必须能够访问 Internet。 对于从报表服务器运行的报表，必须将报表服务器配置为支持图块背景。 有关详细信息，请参阅[计划地图报表](plan-a-map-report-report-builder-and-ssrs.md)。  
  
 有关添加图块层的详细信息，请参阅[添加、更改或删除地图或地图层（报表生成器和 SSRS）](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md)。  
  
## <a name="how-do-i-control-the-text-on-a-tile-layer"></a>如何控制图块层上的文本？  
 **“道路”** 和 **“混合”** 视图均包含文本。 文本是来自 Bing 地图 Web 服务的图块的组成部分。  
  
 若要包含没有文本的图块层，请选择 **“空中”** 视图。  
  
##  <a name="tooltip-and-label-issues"></a><a name="Tooltip"></a> 工具提示和标签问题  
 使用这一部分可以帮助解决与标签或工具提示选项相关的问题。  
  
## <a name="i-get-an-expression-error-about-dataset-scope-when-i-set-a-label-or-tooltip-to-an-expression"></a>当我将标签或工具提示设置为表达式时，获得一个有关数据集作用域的表达式错误。  
 当您的空间数据来自地图库或 ESRI 形状文件时，关联的数据不是报表数据集的组成部分。 您不能将表达式语法用于数据集字段引用来为标签或工具提示指定此数据。  
  
 若要指定与并非报表数据集组成部分的空间数据相关的数据，必须使用符号 # 且后跟一个指定数据名称的标签。  
  
## <a name="see-also"></a>另请参阅  
 [地图（报表生成器和 SSRS）](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)   
 [报表生成器故障排除](/previous-versions/sql/2014/reporting-services/troubleshoot-report-builder?view=sql-server-2014&preserve-view=true)  
  
  
