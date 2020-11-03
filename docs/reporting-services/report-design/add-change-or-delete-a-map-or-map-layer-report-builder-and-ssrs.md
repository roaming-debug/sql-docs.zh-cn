---
title: 添加、更改或删除地图或地图层（报表生成器）| Microsoft Docs
description: 了解如何在报表生成器中手动或通过地图层向导来添加、删除或更改地图选项。
ms.date: 05/24/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.maplayerproperties.general.f1
- "10526"
- sql13.rtp.rptdesigner.mappolygonlayerproperties.general.f1
- "10529"
- "10525"
- "10535"
- sql13.rtp.rptdesigner.mappointlayerproperties.general.f1
- sql13.rtp.rptdesigner.shared.layerfilters.f1
- sql13.rtp.rptdesigner.maplinelayerproperties.general.f1
- "10524"
- sql13.rtp.rptdesigner.maptilelayerproperties.general.f1
- "10532"
- sql13.rtp.rptdesigner.maplayerproperties.analyticaldata.f1
- "10528"
- "10527"
- sql13.rtp.rptdesigner.shared.layervisibility.f1
ms.assetid: 6e89815e-187e-45bf-bf63-3d5c4a246360
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e33750c6c62b1de3b889399fca46dbf5aa83b3fd
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907275"
---
# <a name="add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs"></a>添加、更改或删除地图或地图层（报表生成器和 SSRS）
  地图是层的集合。 当你向 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 分页报表添加一个地图时，就定义了第一个层。 可以使用地图层向导创建其他层。  
  
 若要添加、删除或更改层的选项，最简单的方法是使用地图层向导。 还可以从“地图”窗格中手动更改选项。 若要显示 **“地图”** 窗格，请单击报表设计图面中的地图。 下图显示该窗格的各个部分：  
  
 ![“地图层”部分的屏幕截图，其中指出了“层工具栏”、“层可见性”、“层名称”、“空间数据源的类型”、“层类型”、“调整缩放级别”和“调整视图中心”选项。](../../reporting-services/report-design/media/rsmaplayerzone.gif "rsMapLayerZone")  
  
 按照地图层在“地图”窗格中的显示顺序从下到上绘制地图层。 在上图中，首先绘制图块层，最后绘制多边形层。 后来绘制的层可能隐藏先前绘制的层上的地图元素。 可以使用“地图”窗格工具栏上的箭头键来更改层的顺序。 若要显示或隐藏层，请切换可见性图标。 可以在 **“层数据”** 属性对话框的 **“可见性”** 页上更改层的透明度。  
  
 下表列出了 **“地图”** 窗格的工具栏图标：  
  
|符号|说明|何时使用|  
|------------|-----------------|-----------------|  
|:::image type="icon" source="../../reporting-services/media/rs-iconmaplayerwizard.gif":::|地图层向导|若要使用向导添加层，请单击 **“新建层向导”** 。|  
|:::image type="icon" source="../../reporting-services/media/rs-iconmapaddlayer.gif":::|添加层|若要手动添加层，请单击 **“添加层”** ，然后单击要添加的地图层类型。|  
|:::image type="icon" source="../../reporting-services/report-design/media/rs-iconmappolygonlayer.gif":::|多边形层|添加显示基于一组多边形坐标的区域或形状的地图层。|  
|:::image type="icon" source="../../reporting-services/report-design/media/rs-iconmaplinelayer.gif":::|线条层|添加显示基于一组线条坐标的路径或路线的地图层。|  
|:::image type="icon" source="../../reporting-services/report-design/media/rs-iconmappointlayer.gif":::|点层|添加显示基于一组点坐标的位置的地图层。|  
|:::image type="icon" source="../../reporting-services/report-design/media/rs-iconmaptilelayer.gif":::|图块层|添加显示 Bing 地图图块的一个地图层，这些图块对应于由视区定义的当前地图视图区域。|  
  
 “地图”窗格的底部是“地图”视图区域。 若要更改地图的中心或缩放选项，请使用箭头键来调整视图中心和使用滑块来调整缩放级别。  
  
 有关层的详细信息，请参阅 [地图（报表生成器和 SSRS）](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="to-add-a-layer-from-the-map-layer-wizard"></a><a name="AddLayer"></a> 从地图层向导添加层  
  
-   从“功能区”的 **“插入”** 菜单上，单击 **“地图”** ，然后单击 **“地图” Wizard.** 。通过该向导可以向现有地图添加层。 地图向导和地图层向导的大多数向导页是相同的。  
  
     有关详细信息，请参阅 [地图向导和地图层向导（报表生成器和 SSRS）](../../reporting-services/report-design/map-wizard-and-map-layer-wizard-report-builder-and-ssrs.md)。  
  
##  <a name="to-change-options-for-a-layer-by-using-the-map-layer-wizard"></a><a name="ChangeLayer"></a> 使用地图层向导更改层的选项  
  
-   运行地图层向导。 此向导允许您更改使用地图层向导创建的层的选项。 在“地图”窗格中，右键单击该层，然后在工具栏上单击层向导按钮 (![rs_IconMapLayerWizard](../../reporting-services/media/rs-iconmaplayerwizard.gif "rs_IconMapLayerWizard"))。  
  
     有关详细信息，请参阅 [地图向导和地图层向导（报表生成器和 SSRS）](../../reporting-services/report-design/map-wizard-and-map-layer-wizard-report-builder-and-ssrs.md)。  
  
##  <a name="to-add-a-point-line-or-polygon-layer-from-the-map-pane-toolbar"></a><a name="AddVectorLayer"></a> 从“地图”窗格工具栏添加点、线条或多边形层  
  
1.  单击地图直到显示“地图”窗格。  
  
2.  依次单击工具栏中的“添加层”  按钮，以及下拉列表中的要添加的层类型：“点”  、“线”  或“多边形”  。  
  
    > [!NOTE]  
    >  尽管可以手动添加并配置地图层，我们仍建议您使用地图层向导来添加新层。 若要在“地图”窗格工具栏中启动向导，请单击层向导按钮 (![rs_IconMapLayerWizard](../../reporting-services/media/rs-iconmaplayerwizard.gif "rs_IconMapLayerWizard"))。  
  
3.  右键单击该层，然后单击“层数据”  。  
  
4.  在 **“使用的空间数据来自”** 中，选择空间数据的源。 选项根据您的选择内容而有所不同。  
  
     如果要将此层上的报表的分析数据可视化，请执行以下操作：  
  
    1.  单击 **“分析数据”** 。  
  
    2.  在 **“分析数据集”** 中，单击包含分析数据和匹配字段（用于建立分析数据和空间数据之间的关系）的数据集的名称。  
  
    3.  单击“添加”  。  
  
    4.  键入空间数据集中匹配字段的名称。  
  
    5.  键入分析数据集中匹配字段的名称。  
  
     有关链接空间数据和分析数据的详细信息，请参阅[自定义地图或地图层的数据和显示（报表生成器和 SSRS）](../../reporting-services/report-design/customize-the-data-and-display-of-a-map-or-map-layer-report-builder-and-ssrs.md)。  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="to-filter-analytical-data-for-the-layer"></a><a name="FilterAnalyticalData"></a> 为层筛选分析数据  
  
1.  单击地图直到显示“地图”窗格。  
  
2.  在“地图”窗格中右键单击层，然后单击“层数据”  。  
  
3.  单击 **“筛选器”** 。  
  
4.  定义筛选器公式以便限制用于地图显示的分析数据。 有关详细信息，请参阅[筛选器公式示例（报表生成器和 SSRS）](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md)。  
  
##  <a name="to-control-point-properties-for-a-point-layer-or-for-polygon-center-points"></a><a name="PointProperties"></a> 为点层或多边形中心点控制点属性  
  
1.  选择 **“地图点属性”** 对话框中的 **“常规”** 可以更改以下地图元素的标签、工具提示和标记类型选项：  
  
    -   点层上的所有动态或嵌入的点。 点的颜色规则、大小规则和标记类型规则覆盖这些选项。 若要覆盖特定嵌入点的选项，请使用 [Map Embedded Point Properties Dialog Box, Marker](./maps-report-builder-and-ssrs.md) 页。  
  
    -   多边形层上的所有动态或嵌入的多边形的中心点。 中心点的颜色规则、大小规则和标记类型规则覆盖这些选项。 若要覆盖特定中心点的选项，请使用 [“地图嵌入点属性”对话框，标记](./maps-report-builder-and-ssrs.md) 页。  
  
##  <a name="to-specify-embedded-data-as-a-source-of-spatial-data"></a><a name="Embedded"></a> 指定嵌入数据作为空间数据的源  
  
1.  单击地图直到显示“地图”窗格。  
  
2.  右键单击该层，然后单击“层数据”  。  
  
3.  在 **“使用的空间数据来自”** 中，选择 **“报表中嵌入的数据”** 。  
  
4.  若要从现有报表加载地图元素或基于 ESRI 文件创建地图元素，请单击 **“浏览”** ，指向该文件，然后单击 **“打开”** 。 这些地图元素嵌入在此报表定义中。 您指向的空间数据必须与该层类型匹配。 例如，对于点层，必须指向指定一组点坐标的空间数据。  
  
5.  在 **“空间字段”** 中，指定包含空间数据的字段的名称。 您可能需要从空间数据的源确定此名称。  
  
    > [!NOTE]  
    >  如果不知道该字段的名称且已找到 ESRI 形状文件，请使用“链接到 ESRI 形状文件”  选项来替代此选项。  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="to-specify-an-esri-shapefile-as-a-source-of-spatial-data"></a><a name="ESRI"></a> 指定 ESRI 形状文件作为空间数据的源  
  
1.  单击地图直到显示“地图”窗格。  
  
2.  右键单击该层，然后单击“层数据”  。  
  
3.  在 **“使用的空间数据来自”** 中，选择 **“链接到 ESRI 形状文件”** 。  
  
4.  在 **“文件名”** 中，键入 ESRI 形状文件的位置，或单击 **“浏览”** 以选择 ESRI 形状文件。  
  
    > [!NOTE]  
    >  如果该形状文件位于本地计算机上，则将空间数据嵌入报表定义中。 若要在处理报表时动态检索数据，必须将 ESRI .shp 文件及其 .dbf 支持文件上载到报表服务器。 有关详细信息，请参阅[上传文件或报表](../reports/upload-a-file-or-report-report-manager.md)。  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="to-specify-a-report-dataset-field-as-a-source-of-spatial-data"></a><a name="DatasetField"></a> 指定报表数据集字段作为空间数据的源  
  
1.  单击地图直到显示“地图”窗格。  
  
2.  右键单击该层，然后单击“层数据”  。  
  
3.  在 **“使用的空间数据来自”** 中，选择 **“数据集中的空间字段”** 。  
  
4.  在 **“数据集名称”** 中，单击报表中包含您所需的空间数据的数据集的名称。  
  
5.  在 **“空间字段名称”** 中，单击数据集中包含空间数据的字段的名称。  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="to-add-a-tile-layer"></a><a name="TileLayer"></a> 添加图块层  
  
1.  单击地图直到显示“地图”窗格。  
  
2.  在工具栏上单击“添加层”  按钮，然后从下拉列表中单击“图块层”  。  
  
    > [!NOTE]  
    >  有关在报表中使用 Bing 地图图块的详细信息，请参阅 [其他使用条款](https://go.microsoft.com/fwlink/?LinkId=151371)。  
  
3.  右键单击“地图”窗格中的该图块层，然后单击“图块属性”  。  
  
4.  在 **“图块选项”** 中，选择一种图块样式。 如果 Bing 地图图块可用，则使用您选择的样式更新设计图面上的该层。  
  
    > [!NOTE]  
    >  当您在地图向导或地图层向导中添加多边形、线条或点层时，也可以添加图块层。 在 **“选择空间数据和地图视图选项”** 页上，选择选项 **“为该地图视图添加 Bing 地图背景”** 。  
  
##  <a name="to-change-the-drawing-order-of-a-layer"></a><a name="DrawingOrder"></a> 更改层的绘制顺序  
  
1.  单击地图直到显示“地图”窗格。  
  
2.  单击“地图”窗格中的层以选中它。  
  
3.  在“地图”窗格工具栏上，单击向上键或向下键来更改每个层的绘制顺序。  
  
##  <a name="to-change-the-transparency-of-a-polygon-line-or-point-layer"></a><a name="Transparency"></a> 更改多边形、线条或点层的透明度  
  
1.  单击地图直到显示“地图”窗格。  
  
2.  右键单击该层，然后单击“层数据”  。  
  
3.  单击 **“可见性”** 。  
  
4.  在 **“透明度选项”** 中，键入表示百分比透明度的值，例如 **40** 。 零 (0) % 透明度表示该层不透明。 100% 透明度意味着您在报表中将看不到该层。  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="to-change-the-transparency-of-a-tile-layer"></a><a name="TileTransparency"></a> 更改图块层的透明度  
  
1.  单击地图直到显示“地图”窗格。  
  
2.  右键单击该层，然后单击“图块属性”  。  
  
3.  单击 **“可见性”** 。  
  
4.  在 **“透明度选项”** 中，键入表示百分比透明度的值，例如 **40** 。  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="to-specify-a-secure-connection-for-a-tile-layer"></a><a name="Secure"></a> 为图块层指定安全连接  
  
1.  单击地图直到显示“地图”窗格。  
  
2.  在“地图”窗格中，单击该图块层以选中它。 “属性”窗格显示该图块层的属性。  
  
3.  在“属性”窗格中，将 UseSecureConnection 设置为 **True** 。  
  
 必应地图 Web 服务的连接将使用 HTTP SSL 服务来检索此层的必应地图图块。 传输层安全性 (TLS) 旧称为“安全套接字层 (SSL)”。
  
##  <a name="to-specify-the-language-for-tile-labels"></a><a name="Language"></a> 为图块标签指定语言  
  
1.  默认情况下，对于显示标签的图块样式，从报表生成器的默认区域设置确定语言。 您可以按如下方式自定义图块标签的语言设置：  
  
    -   在视区外单击地图以选中它。 在“属性”窗格中，对于 TileLanguage 属性，从下拉列表中选择一个区域性值。  
  
    -   单击报表背景以选择该报表。 在“属性”窗格中，对于 Language 属性，从下拉列表中选择一个区域性值。  
  
     设置图块标签语言的优先次序为：报表属性 Language、“报表生成器”的默认区域设置、地图属性 TileLanguage。  
  
##  <a name="to-conditionally-hide-a-layer-based-on-viewport-zoom-level"></a><a name="ConditionalHide"></a> 基于视区缩放级别有条件地隐藏层  
  
1.  设置 **“可见性”** 选项以控制地图层的显示方式。  
  
    -   在“地图层”窗格中，右键单击某个层以便选择该层，然后在“地图层”工具栏上单击“属性”，以便打开“地图层属性”  。  
  
    -   单击 **“可见性”** 。  
  
    -   在层可见性中，选择 **“基于缩放值显示或隐藏”** 。  
  
    -   输入显示层时的最小和最大缩放值。  
  
    -   可选。 输入透明度值。  
  
     还可以有条件隐藏层。 有关详细信息，请参阅[隐藏项（报表生成器和 SSRS）](../../reporting-services/report-builder/hide-an-item-report-builder-and-ssrs.md)。  
  
## <a name="see-also"></a>另请参阅  
 [地图（报表生成器和 SSRS）](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)   
 [报表故障排除：地图报表（报表生成器和 SSRS）](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md)  
  
