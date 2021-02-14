---
title: 教程：脱机生成快速图表报表（报表生成器）| Microsoft Docs
description: 在本教程中，将在报表生成器中的 Reporting Services 分页报表中创建饼图。 然后添加百分比并对饼图进行修改。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
helpviewer_keywords:
- reports, creating
- tutorials, getting started
- creating reports
ms.assetid: 6b1db67a-cf75-494c-b70c-09f1e6a8d414
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2f7fb9781583a56a2765f066462e62717c1d32d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070546"
---
# <a name="tutorial-create-a-quick-chart-report-offline-report-builder"></a>教程：脱机生成快速图表报表（报表生成器）

  在本教程中，可使用向导在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 分页报表中创建饼图。 然后添加百分比并对饼图进行少量修改。 
  
您可以通过两种不同的方式学习本教程。 两种方法具有相同的结果，都将得到与下图类似的饼图：  
  
 ![报表生成器快速饼图](../../reporting-services/report-builder/media/report-builder-quick-pie-chart.png "报表生成器快速饼图")  
  
## <a name="prerequisites"></a>先决条件  
 无论你是使用 XML 数据还是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询，都需要有权访问报表生成器。 你可以从本机模式或 SharePoint 集成模式中的 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 报表服务器上启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ，也可以从 Microsoft 下载中心下载 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 。 有关详细信息，请参阅 [Install Report Builder](../../reporting-services/install-windows/install-report-builder.md)。  
  
##  <a name="two-ways-to-do-this-tutorial"></a><a name="TwoWays"></a> 用于完成本教程的两种方法  
  
-   [使用 XML 数据创建饼图](#CreatePieChartXML)  
  
-   [使用包含数据的 Transact-SQL 查询创建饼图](#CreatePieQueryData)  
  
### <a name="using-xml-data-for-this-tutorial"></a>使用本教程的 XML 数据  
 通过复制本主题中的 XML 数据并粘贴到向导中，可以使用本主题中的 XML 数据。 你无需连接到处于本机模式或 SharePoint 集成模式下的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表服务器，也无需访问 SQL Server 的实例。  
  
 [使用 XML 数据创建饼图](#CreatePieChartXML)  
  
### <a name="using-a-tsql-query-that-contains-data-for-this-tutorial"></a>使用包含此教程数据的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询  
 可以复制本主题中包含数据的查询，并将其粘贴到向导中。 你需要 SQL Server 实例的名称以及足以对任何数据库进行只读访问的凭据。 本教程中的数据集查询使用文本数据，但查询必须由 SQL Server 实例来处理才能返回报表数据集所需的元数据。  
  
 使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询的优势在于，其他所有 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 教程均使用相同的方法，这样，在你学习其他教程时，你已经提前知道了该做什么。  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询要求满足一些其他前提条件。 有关详细信息，请参阅[教程先决条件&#40;报表生成器&#41;](../../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
 [使用包含数据的 Transact-SQL 查询创建饼图](#CreatePieQueryData)  
  
##  <a name="creating-the-pie-chart-with-xml-data"></a><a name="CreatePieChartXML"></a> 使用 XML 数据创建饼图  
  
1.  从[Web 门户或从 SharePoint 集成模式下的报表服务器，或从计算机](../../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 。  
  
     此时将显示 **“入门”** 对话框。  
  
     ![报表生成器入门](../../reporting-services/media/rb-getstarted.png "报表生成器入门")  
  
     如果没有出现“入门”对话框，请单击“文件” >“新建”    。 “新建报表或数据集”  对话框的内容与“入门”  对话框的内容大致相同。  
  
2.  在左窗格中，确认已选中 **“新建报表”** 。  
  
3.  在右窗格中，单击 **“图表向导”** ，然后单击 **“创建”** 。  
  
4.  在 **“选择数据集”** 页中，单击 **“创建数据集”** ，然后单击 **“下一步”** 。  
  
5.  在 **“选择数据源的连接”** 页中，单击 **“新建”** 。  
  
     此时将打开 **“数据源属性”** 对话框。  
  
6.  可以将数据源命名为任何名称。 在 **“名称”** 框中，键入 **MyPieChart**。  
  
7.  在“选择连接类型”  框中，单击“XML”  。  
  
8.  单击“凭据”选项卡，选择“使用当前 Windows 用户。   可能需要 Kerberos 委托”，然后单击“确定”  。  
  
9. 在 **“选择数据源的连接”** 页中，单击 **MyPieChart**，然后单击 **“下一步”** 。  
  
10. 复制以下文本，并将它粘贴到“设计查询”  页中心的大框中。  
  
    ```  
    <Query>  
    <ElementPath>Root /S  {@Sales (Integer)} /C {@FullName} </ElementPath>  
    <XmlData>  
    <Root>  
    <S Sales="150">  
      <C FullName="Jae Pak" />  
    </S>  
    <S Sales="350">  
      <C FullName="Jillian  Carson" />  
    </S>  
    <S Sales="250">  
      <C FullName="Linda C Mitchell" />  
    </S>  
    <S Sales="500">  
      <C FullName="Michael Blythe" />  
    </S>  
    <S Sales="450">  
      <C FullName="Ranjit Varkey" />  
    </S>  
    </Root>  
    </XmlData>  
    </Query>  
    ```  
  
11. （可选）单击“运行”按钮 (!)，查看要用于图表的数据   。  
  
     ![报表生成器设计查询](../../reporting-services/report-builder/media/rb-designquery.png "报表生成器设计查询")  
  
12. 单击“下一步”。   
  
13. 在 **“选择图表类型”** 页中，单击 **“饼图”** ，然后单击 **“下一步”** 。  
  
14. 在“排列图表字段”页中，在“可用字段”框中双击“Sales”字段    。  
  
     注意，它将自动移动到 **“值”** 框，因为它是数字值。  
  
     ![报表生成器向导排列字段](../../reporting-services/report-builder/media/rb-wizarrangefields.png "报表生成器向导排列字段")  
  
15. 将“FullName”字段从“可用字段”框拖到“类别”框（或双击它，它将转到“类别”框），然后单击“下一步”      。  
  
     “预览”页上将显示带表述性数据的新饼图。 图例读取 Full Name 1、Full Name 2 等，而不是销售人员的名字，并且饼图切片的大小不准确。 这只用于展示报表的外观。  
  
     ![报表生成器新的图表预览](../../reporting-services/report-builder/media/rb-newchartpreview.png "报表生成器新的图表预览")  
  
16. 单击“完成”  。  
  
     现在你将在设计视图中看到新的饼图，以及表述性数据。  
  
     ![设计视图中的报表生成器新饼图](../../reporting-services/report-builder/media/rb-newpiedesign.png "设计视图中的报表生成器新饼图")  
  
17. 若要看见实际的饼图，请在功能区的 **“主文件夹”** 选项卡上单击 **“运行”** 。  
  
     ![报表生成器新图表运行](../../reporting-services/report-builder/media/rb-newchartrun.png "报表生成器新图表运行")  
  
18. 若要继续修改饼图，请转到本文的 [运行向导之后](#AfterWizard) 一节。  
  
##  <a name="creating-the-pie-chart-with-a-tsql-query"></a><a name="CreatePieQueryData"></a> 使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询创建饼图  
  
1.  从[Web 门户、从 SharePoint 集成模式下的报表服务器，或从计算机](../../reporting-services/report-builder/start-report-builder.md) 启动报表生成器 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] web portal, 启动报表生成器 report server in SharePoint integrated mode, or from your computer.  
  
     此时将显示 **“入门”** 对话框。  
  
    > [!NOTE]  
    >  如果没有出现“入门”对话框，请单击“文件” >“新建”    。 “新建报表或数据集”  对话框的内容与“入门”  对话框的内容大致相同。  
  
2.  在左窗格中，确认已选中 **“新建报表”** 。  
  
3.  在右窗格中，单击 **“图表向导”** ，然后单击 **“创建”** 。  
  
4.  在 **“选择数据集”** 页中，单击 **“创建数据集”** ，然后单击 **“下一步”** 。  
  
5.  在 **“选择数据源的连接”** 页中，选择现有数据源或浏览到报表服务器并选择一个数据源，然后单击 **“下一步”** 。 您可能需要输入用户名和密码。  
  
    > [!NOTE]  
    >  只要您具有足够的权限，则选择哪一个数据源并不重要。 您将不会从数据源中获取数据。 有关详细信息，请参阅[教程先决条件&#40;报表生成器&#41;](../../reporting-services/prerequisites-for-tutorials-report-builder.md)。  
  
6.  在 **“设计查询”** 页中，单击 **“编辑为文本”** 。  
  
7.  将以下查询粘贴到查询窗格中：  
  
    ```  
    SELECT 150 AS Sales, 'Jae Pak' AS FullName   
    UNION SELECT 350 AS Sales, 'Jillian Carson' AS FullName   
    UNION SELECT 250 AS Sales, 'Linda C Mitchell' AS FullName   
    UNION SELECT 500 AS Sales, 'Michael Blythe' AS FullName   
    UNION SELECT 450 AS Sales, 'Ranjit Varkey' AS FullName   
    ```  
  
8.  （可选）单击“运行”按钮 ( **!** )，查看要用于图表的数据。  
  
9. 单击“下一步”。   
  
10. 在 **“选择图表类型”** 页中，单击 **“饼图”** ，然后单击 **“下一步”** 。  
  
11. 在“排列图表字段”页中，在“可用字段”框中双击“Sales”字段    。  
  
     注意，它将自动移动到 **“值”** 框，因为它是数字值。  
  
12. 将“FullName”字段从“可用字段”框拖到“类别”框（或双击它，它将转到“类别”框），然后单击“下一步”      。  
  
13. 单击“完成”  。  
  
     现在，将在设计图面上看到新饼图报表。 看到的内容很有代表性。 图例读取 Full Name 1、Full Name 2 等，而不是销售人员的名字，并且饼图切片的大小不准确。 这只用于展示报表的外观。  
  
15. 若要看见实际的饼图，请在功能区的 **“主文件夹”** 选项卡上单击 **“运行”** 。  
 
##  <a name="after-you-run-the-wizard"></a><a name="AfterWizard"></a> 运行向导之后  
 现在您便拥有了饼图报表，从而可以对其进行操作。 在功能区的 **“运行”** 选项卡上，单击 **“设计”** ，这样可以继续修改它。  
  
## <a name="make-the-chart-bigger"></a>放大图表  
 您可能希望放大饼图。 
 
*  单击图表（但不要单击图表中的任何元素）以选择它，并拖动右下角以调整它的大小。  

注意在你拖动时设计图面会变大。
  
## <a name="add-a-report-title"></a>添加报表标题  
1. 选择图表顶部的词语 **“图表标题”** ，然后键入标题，例如 **Sales Pie Chart**。  
2. 选择标题后，在“属性”窗格中，将“颜色”更改为“黑色”并将“字号”更改为“12pt”     。
  
## <a name="add-percentages"></a>添加百分比  
 
1.  右键单击饼图，并选择“显示数据标签”  。 数据标签将显示在饼图上的每个切片上。  
  
2.  右键单击标签，并选择“序列标签属性”  。 此时将显示 **“序列标签属性”** 对话框。  
  
3.  在“标签数据”框中键入“#PERCENT{P0}”   。  
  
     **{P0}** 提供没有小数位的百分比。 如果只是键入“#PERCENT”，则数字将有两位小数  。 “#PERCENT”是执行计算或函数的关键字，还有很多这样的关键字  。  
     
4. 单击“ **是** ”确认你想要将 **UseValueAsLabel** 设置为 **False**。

5. 在“ **字体** ”选项卡上，选择“ **粗体** ”并将“ **颜色** ”更改为“ **白色**”。

6. 单击“确定”。      
  
 有关自定义饼图标签和图例的详细信息，请参阅[在饼图上显示百分比值&#40;报表生成器和 SSRS&#41;](../../reporting-services/report-design/display-percentage-values-on-a-pie-chart-report-builder-and-ssrs.md) 和 [更改图例项文本&#40;报表生成器和 SSRS&#41;](../../reporting-services/report-design/chart-legend-change-item-text-report-builder.md)。  
  
##  <a name="whats-next"></a><a name="WhatsNext"></a> 下一步是什么？  
 现在已在 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]中创建了第一个报表，可准备尝试其他教程，也可以开始利用自己的数据创建报表。 若要运行 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]，需要具有通过“连接字符串”（它使你实际连接到数据源）访问数据源（如数据库）的权限  。 系统管理员拥有此信息，并且可以为您设置相应的权限。  
  
 若要完成其他教程，你需要 SQL Server 实例的名称以及足以对任何数据库进行只读访问的凭据。 系统管理员也可以为您设置该权限。  
  
 最后，若要将报表保存到报表服务器或与报表服务器集成的 SharePoint 站点，需要具有 URL 和相应权限。 可以直接从您的计算机运行您创建的任何报表，但如果从报表服务器或 SharePoint 站点运行报表，则报表会有更多功能。 您需要有一定权限才能运行您的报表或报表服务器或 SharePoint 站点上发布的其他报表。 请与系统管理员联系以获取访问权限。  
  
 在入门之前，可能有必要了解一些概念和术语。 请参阅 [Reporting Services 概念 (SSRS)](../reporting-services-concepts-ssrs.md)。 而且，在创建第一个报表之前，应当花一些时间进行规划。 这将需要较长时间。 请参阅[规划报表&#40;报表生成器&#41;](../../reporting-services/report-design/planning-a-report-report-builder.md)。  

## <a name="next-steps"></a>后续步骤

[报表生成器教程](../../reporting-services/report-builder-tutorials.md)   
[SQL Server 中的报表生成器](../../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
