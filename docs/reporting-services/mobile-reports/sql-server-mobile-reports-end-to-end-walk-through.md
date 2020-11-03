---
title: SQL Server 移动报表：端到端演练
description: 了解如何在 SQL Server 移动报表发布服务器上创建移动报表、在 Reporting Services web 门户上保存报表以及在 Power BI 移动应用中查看报表。
ms.date: 12/07/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.custom: seodec18
ms.topic: conceptual
ms.assetid: e198575e-b154-4342-b944-2bf19ec49bfd
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4013c119093adda0fbb721c376eef502a7b05a38
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907245"
---
# <a name="sql-server-mobile-reports-end-to-end-walk-through"></a>SQL Server 移动报表：端到端演练
在 [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-long.md)] Web 门户上使用 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 创建适用于任何屏幕大小的移动报表，并在 Power BI 移动应用中查看它们。

在网格行和列可调整且移动报表元素灵活的设计图面上创建移动报表。 连接到各种本地数据源，或上传 Excel 工作簿以创建移动报表。 然后将报表保存到 [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] Web 门户，并在浏览器或 Power BI 移动应用中查看它们。  
  
本文介绍：   
  
- 在 [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] Web 门户上创建共享数据源和数据集，使用 AdventureWorks 数据库作为示例数据源。  
- 在 [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)]中创建 Reporting Services 移动报表  
- 将移动报表发布到 [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] Web 门户。  
- 在 Power BI 移动应用中查看移动报表。  
  
## <a name="before-we-start"></a>准备工作  
若要继续，需要准备这些产品：  
  
* 要创建数据源和 KPI，以及发布数据集和移动报表，需要访问 [Reporting Services 本机模式报表服务器](../install-windows/install-reporting-services-native-mode-report-server.md)。  
* 要创建共享数据集，请[安装报表生成器](../install-windows/install-report-builder.md)。  
* 要创建移动报表，需 [安装 SQL Server 移动报表发布服务器](https://go.microsoft.com/fwlink/?LinkId=717766)。  
* [AdventureWorks sample databases](https://github.com/Microsoft/sql-server-samples/releases)（AdventureWorks 示例数据库）。  
*  或：World Wide Importers 示例数据库，可从 [Microsoft SQL Server 示例](../../sample/microsoft-sql-server-samples.md)页面获得。
* 查看结果： 
  *   [注册 Power BI 服务](https://go.microsoft.com/fwlink/?LinkID=513879) 并
  *  [下载 Power BI 移动应用](https://docs.microsoft.com/power-bi/consumer/mobile/mobile-apps-for-mobile-devices) 到你的移动设备：iOS、Android 手机或 Windows 10 设备。  

  
## <a name="create-a-shared-data-source"></a>创建共享数据源  
  
可以从 Reporting Services 支持的任何数据源为移动报表创建共享数据源。 请参阅[支持的数据源列表](../report-data/data-sources-supported-by-reporting-services-ssrs.md)。  
  
1. 在 [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] Web 门户中，单击“新建” > “数据源”。  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)  
3. 输入数据源信息，然后单击“确定”  。  
  
    默认情况下，数据源不会显示在门户中。    
   
5. 若要查看数据源，请单击“显示” > “数据源”。  
  
   ![PBI_SSMRP_DisplayDataSources](../../reporting-services/mobile-reports/media/pbi-ssmrp-displaydatasources.png)  
   
6. 现在，你便可以在 [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] 门户中查看数据源了。  
  
   ![PBI_SSMRP_PortlDataSource](../../reporting-services/mobile-reports/media/pbi-ssmrp-portldatasource.png)  
  
阅读以深入了解 [Reporting Services 中的共享数据源](../report-data/create-modify-and-delete-shared-data-sources-ssrs.md)。  
   
## <a name=""></a><a name="shared-dataset">创建共享数据集</a>  
  
使用现有的 [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] 客户端工具（例如 [!INCLUDE[ssBIDevStudioFull_md](../../includes/ssbidevstudiofull-md.md)]中的报表设计器）创建共享数据集。  本演练使用 [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)]。 [安装报表生成器](../install-windows/install-report-builder.md)，或从 Web 门户启动它。 需要创建三个数据集，一个用于 KPI 值，一个用于 KPI 趋势，包含更多字段的那一个用于 Reporting Services 移动报表。     
  
1. 在 [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] Web 门户中，单击“新建” > “分页报表”以启动 [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)]。  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)   
2. 单击“新数据集”  。  
  
   ![PBI_SSMRP_RBNewDataset](../../reporting-services/mobile-reports/media/pbi-ssmrp-rbnewdataset.png)  
   
3. 单击“浏览其他数据源”  。  
   
4. 在“名称”字段中，输入保存数据源的服务器名称，格式如下：   
   
   名称： https://localhost/ReportServer   
   项的类型：Data Sources (*.rsds)  
   
5. 单击“打开”  ，并导航到在该服务器上创建的数据源。  
   
6. 选择数据源，并再次单击“打开”  。    
  
7. 在 [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)]中设计你的数据集。  
  
   ![PBI_SSMRP_RB_QueryDesignr600](../../reporting-services/mobile-reports/media/pbi-ssmrp-rb-querydesignr600.png)  
   
8. 完成后，将数据集保存到 [!INCLUDE[PRODUCT_NAME](../../includes/ssrs.md)] 报表服务器。    
   
现在，你便可以基于该数据集创建 KPI 和移动报表了。  可以针对相同的数据源创建多个数据集。 可以针对这些共享数据集创建多个 KPI 和移动报表。   
  
## <a name=""></a><a name="create-KPI">创建 KPI</a>  
可在 [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] Web 门户中立即创建 KPI。    
  
1. 在 Web 门户右上角，单击“新建” > “新建 KPI”。   
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)  
      
   在 KPI 创建屏幕中，可以手动输入值或使用共享数据集。    
2. 将“值”  从“手动设置”  更改到“数据集字段”  。  
   
   ![PBI_SSMRP_KPI_DatasetField](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpi-datasetfield.png)  
   
3. 单击“选取数据集字段”  框中的省略号 ( **...** )，然后从上一步中选择数据集。  
   
   ![PBI_SSMRP_KPIPickDataset](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipickdataset.png)  
   
4. 在数据集中选择字段。    
   
   ![PBI_SSMRP_KPIPickField](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipickfield.png)  
     
5. 选择你想要的聚合。 KPI 只能显示一个数字，因此将聚合该字段以显示该数字。

   ![“从 AWSalesYTD 选择字段”部分的屏幕截图，其中显示了“平均聚合”部分。](../../reporting-services/mobile-reports/media/reporting-services-kpi-pick-aggregation.png)

6. 单击“确定”。 

7. 在“趋势集”  框中，单击“数据集趋势”  。  
  
6. 在“选取数据集趋势”  框中，单击省略号 ( **...** )  
   
7. 选择一个字段，并单击“确定”  。  

   ![PBI_SSMRP_KPIPickTrend](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipicktrend.png)  
  
8. 为你的 KPI 命名并选取可视化类型，然后单击“创建”  。   
  
   此 KPI 将显示在 [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] Web 门户中。  
   
    ![PBI_SSMRP_NewKPI](../../reporting-services/mobile-reports/media/pbi-ssmrp-newkpi.png)  
    
## <a name=""></a><a name="create-mobile-report">创建 Reporting Services 移动报表</a>  
   
要创建 Reporting Services 移动报表，请 [安装 SQL Server 移动报表发布服务器](https://go.microsoft.com/fwlink/?LinkId=717766)，或从 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] Web 门户启动它。 

在首次打开 [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)]时，你将看到一块空白画布，可以在此创建移动报表。 可以从创建视觉对象开始，也可以从数据开始。 如果首先创建视觉对象， [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] 会自动生成绑定到报表的模拟数据，并在更改视觉选择时进行动态更改。 请自己尝试一下。   
  
## <a name="start-with-the-visuals"></a>从视觉对象开始  
  
1. 在 [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] Web 门户中，单击“新建” > “移动报表”以启动 [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)]。  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)

   [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] 将打开母版布局网格。  
  
2. 在“布局”  选项卡上，向下滚动到“图表”部分。  
  
   ![PBI_SSMRP_LayoutTabCharts2](../../reporting-services/mobile-reports/media/pbi-ssmrp-layouttabcharts2.png)  
  
2. 将“树形图”  拖动到网格，然后拖动右下角，使其宽为三列，高为三行。  
  
   ![PBI_SSMRP_TreeMap](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemap.png)  
  
3. 你可以在底部看到它的视觉对象属性。 可能需要向侧面滚动才能看到全部。   
  
   ![PBI_SSMRP_TreeMapVisProps](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemapvisprops.png)  
  
4. 选择树形图视觉对象后，选择左上角的“数据”  选项卡。   
  
   现在将显示 [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] 生成的模拟字段和值，你可以查看树形图中显示的大小和颜色。  
  
   ![PBI_SSMRP_TreeMapDataProps](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemapdataprops.png)  
  
6. 单击“布局”选项卡。   
  
7. 单击树形图右上角的“选项”嵌齿 ![PBI_SSMRP_Cog](../../reporting-services/mobile-reports/media/pbi-ssmrp-cog.png) 可以看到其包含的菜单。   
  
   ![PBI_SSMRP_OptionsWheel](../../reporting-services/mobile-reports/media/pbi-ssmrp-optionswheel.png)  
  
8. 单击滚轮中间的箭头可将其关闭。  
  
## <a name="add-your-own-data"></a>添加自己的数据  
  
1. 切换到“数据”  选项卡。    
   
2. 要添加自己的数据，请单击右上角的“添加数据”  ，然后导航到你的数据。    
  
3. 可以使用本地 Excel 工作簿中的数据，但本示例使用的是 [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] Web 门户上的共享数据集。 你会看到“已添加服务器”消息。  
  
4. 选择该服务器，然后选择你所创建的数据集。  
   
3. 返回到“数据”  选项卡，在“数据属性”  窗格中将“大小表示”  、“颜色表示”  和其他属性更改为自己数据中的字段。 
   
   *  “大小表示”  ,  和“自定义中间值”  必须是具有数值的字段。 
   *  **分组依据** 是一种类别，因此它是文本字段。
   
   ![“数据”属性部分的屏幕截图。](../../reporting-services/mobile-reports/media/ssrs-mobile-report-data-properties.png)
   
6. 选择“预览”  可查看使用你的数据更新的树形图。  

## <a name="add-a-gauge-to-your-mobile-report"></a>向移动报表添加仪表

添加一个仪表，使用相同数据集查看与去年销售额相比，今年年初至今的销售额是多少。

1. 在“布局”选项卡上，将半同心圆拖到画布上。 把它放在树形图下面，拖动右下角，使它为三个正方形宽，两个正方形高。 

2. 同样，从模拟数据开始。 

   请注意，在“视觉对象属性”  中，默认情况下 **值越高越好** ，且“增量标签”  是 **目标百分比** 。 它具有默认的“数据区域停止点”  ，可以对其进行更改，但现在不需要。

   ![移动报表环形图视觉对象属性“设置范围”部分的屏幕截图。](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-visual-properties.png)
   
3. 在“数据”  选项卡上，选择包含数据的表，然后选择“主值”  字段以及要在“比较值”  中比较的字段。

4. 可以选择不同的聚合，为“主值”  和“比较值”  各提供一个数字。 默认情况下，该值是一个总和。

   ![“选项”或“比较值”的屏幕截图。](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-sum.png)

5. 选择“预览”  可查看其外观。 

   ![移动报表环形图预览视图的屏幕截图。](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-preview.png)

## <a name="add-a-selection-list-as-a-filter"></a>添加选择列表作为过滤器

选择列表的行为与 Power BI 和 Excel 中的切片器类似。 我们可以添加一个选择列表来筛选移动报表中的其他视觉对象。

1. 在“布局”  选项卡上，将选择列表拖动到树形图的右侧，然后拖动右下角以使其宽度为两个正方形，并与画布一样高（五个正方形高）。 

   ![移动报表选择列表的屏幕截图。](../../reporting-services/mobile-reports/media/ssrs-mobile-report-selection-list.png)

2. 在“数据”  选项卡上的“数据属性”  中，将“密钥”  和“标签”  设置为数据中要筛选的字段。

   ![移动报表选择列表“数据属性”部分的屏幕截图。](../../reporting-services/mobile-reports/media/ssrs-mobile-report-selection-list-data-properties.png)
   
## <a name="create-a-mobile-report-for-phones"></a>为手机创建移动报表  
  
在母版布局上创建视觉对象后，接下来可以创建一个移动报表，并专门针对你的手机用户布局进行优化。    
  
1. 单击右上角的画布图标，再单击“手机”  。  
  
2. 在“控制实例”  下的布局选项卡上，你会看到已创建的两个图表。   
  
3. 将树形图拖动到手机画布，使其为四列宽，三行高。  
  

## <a name="save-your-mobile-report"></a>保存移动报表  
可将报表保存到本地，或保存到 [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] Web 门户。 如果在本地保存， [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] 会使用缓存数据保存它，因此你可以打开它继续进行操作。 但无法在移动设备上查看它。   
  
1. 单击左上角的保存图标。   
   
2. 要与他人共享并在移动设备上查看，请单击“保存到服务器”  。  
  
3. 在服务器上，浏览到要保存移动报表的文件夹。  
  
4. 单击“选择文件夹” > “保存”。  
  
   你会收到一条确认报表已保存的消息。  
    
   保存后，你可以返回到门户并查看移动报表的缩略图。   
    
5. 点击缩略图可在 Web 门户中查看报表。  
  
## <a name="view-your-report-on-the-web-portal"></a>在 Web 门户中查看报表

  
## <a name="view-your-report-on-a-mobile-device"></a>在移动设备上查看报表   
  
若要查看 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 报表，首先要：

*  [注册 Power BI 服务](https://go.microsoft.com/fwlink/?LinkID=513879)（如果还没有一个帐户）。
*  [下载 Power BI 移动应用](https://powerbi.microsoft.com/documentation/powerbi-power-bi-apps-for-mobile-devices/) 。  

### <a name="view-your-mobile-report"></a>查看移动报表
  
1.  打开并登录到移动设备上的 Power BI 应用。  
    
2.  要查看 Reporting Services 移动报表和 KPI，请点击“Reporting Services”  。  
![PBI_iPad_GetStartedSm](../../reporting-services/mobile-reports/media/pbi-ipad-getstartedsm.png)  
  
3. 点击左上角的选择图标 ![PBI_iPad_OptionsIcon](../../reporting-services/mobile-reports/media/pbi-ipad-optionsicon.png) ，然后点击“连接到服务器”  。  
  
   ![PBI_iPad_SSMRP_ConnectCrop](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-connectcrop.png)  
  
4. 为服务器命名，并填写服务器地址和你的电子邮件地址及密码，格式如下：  
  
   ![PBI_iPad_SSMRP_ConnectContoso](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-connectcontoso.png)   
  
5.  现在你便可在左侧导航栏中看到该服务器。  
  
    ![PBI_iPad_SSMRP_LeftNavBiggr](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-leftnavbiggr.png)  
      
> [!TIP]
> 点击选项图标 ![PBI_iPad_OptionsIcon](../../reporting-services/mobile-reports/media/pbi-ipad-optionsicon.png) 可随时在 Reporting Services Web 门户中的 Reporting Services 移动报表和 Power BI 服务中的仪表板之间移动。  

  
## <a name="view-kpis-and-mobile-reports-in-the-power-bi-app"></a>在 Power BI 应用中查看 KPI 和移动报表  
  
点击“KPI”  或“移动报表”  选项卡。   
  
![PBI_iPad_SSMRP_Portal](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-portal.png)  
  
- 点击一个“KPI”在“焦点模式”下查看。  
  
    ![PBI_iPad_SSMRP_Tile](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-tile.png)  
  
- 点击移动报表以在 Power BI 应用中打开并与其进行交互。  
  
KPI 和移动报表显示在 Reporting Services Web 门户中它们所在的相同文件夹中。   
  
## <a name="see-also"></a>另请参阅  
 
-  查看 iOS 和 Android 版 [Power BI 移动应用中的本地报表服务器移动报表和 KPI](https://docs.microsoft.com/power-bi/consumer/mobile/mobile-app-ssrs-kpis-mobile-on-premises-reports)
-  查看[适用于 Windows 10 设备的 Power BI 移动应用中的本地报表服务器移动报表和 KPI](https://powerbi.microsoft.com/documentation/powerbi-mobile-win10-kpis-mobile-reports/)    
  
   

