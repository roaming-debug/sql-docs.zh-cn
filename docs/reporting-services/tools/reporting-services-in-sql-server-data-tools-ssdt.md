---
title: SQL Server Data Tools 中的 Reporting Services (SSDT) | Microsoft Docs
description: 了解如何使用 Microsoft Visual Studio 中的 SQL Server Data Tools 报表设计器创作环境为 Reporting Services 创建解决方案。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- Business Intelligence Development Studio, Reporting Services in
ms.assetid: 0903c7b2-ac59-45f1-b7d0-922ecd9d76f8
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9ed7c7d204fe315280e379206d56f122606d1f03
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597300"
---
# <a name="reporting-services-in-sql-server-data-tools-ssdt"></a>SQL Server Data Tools 中的 Reporting Services (SSDT)

  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 是用于创建商业智能解决方案的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 环境。 SSDT 是报表设计器创作环境，你可以在其中打开、修改、预览、保存和部署 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 分页报表定义、共享数据源、共享数据集和报表部件。 SQL Server 不提供 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]。 下载 [SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)。 
  
 本主题介绍用于 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]解决方案、项目、项目模板和配置，以及可在报表设计器中使用的视图、菜单、工具栏和快捷键。  
  
 若要开始设计报表，请参阅[使用报表设计器设计报表 (SSRS)](../../reporting-services/tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md)。  
  
##  <a name="solutions-and-projects"></a><a name="bkmk_SolutionsandProjects"></a> 解决方案和项目  
 报表项目的作用是充当报表定义和资源的容器。 在部署项目时，会将报表项目中的每个文件发布到报表服务器上。 在第一次创建项目时，还将创建一个解决方案作为该项目的容器。 您可以将多个项目添加到一个解决方案中。  
  
  
##  <a name="configurations"></a><a name="bkmk_Configurations"></a> 配置  
 若要创建多组项目属性以部署诸如企业测试和生产报表服务器等变体，请使用配置管理器。 有关详细信息，请参阅 [SQL Server Data Tools 中的部署和版本支持 (SSRS)](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)中。  
  
##  <a name="report-server-projects"></a><a name="bkmk_ReportServerProjects"></a> 报表服务器项目  
 安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]后，在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中可以使用下列项目模板：  
  
-   **报表服务器项目。** 选择“报表服务器项目”时，将打开报表设计器。 报表服务器项目是一个由 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 安装的商业智能项目模板，可在 **“新建项目”** 对话框中找到它。 有关详细信息，请参阅[向报表项目添加新报表或现有报表 (SSRS)](../../reporting-services/tools/add-a-new-or-existing-report-to-a-report-project-ssrs.md)。报表服务器项目属性适用于 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 项目中的所有报表和共享数据源。 这些属性包括报表服务器的 URL 以及报表和共享数据源的文件夹名称。 使用 **“项目属性页”** 对话框可查看当前属性值。 若要打开此对话框，请在“项目”  菜单上单击 “属性” 。  
  
-   **报表服务器项目向导。** 选择报表服务器向导项目时，将自动创建一个报表服务器项目并打开报表向导。 在该向导中，您可以根据每个页面上的说明来创建报表：创建到数据源的连接字符串，设置数据源凭据，设计查询，添加表或矩阵数据区域，指定报表数据和组，选取字体和颜色样式，将报表发布到报表服务器，在本地预览报表。 使用该向导创建报表之后，您可以通过使用报表服务器项目中的报表设计器来更改报表数据和报表设计器。  
  
 ![SSDT 中的“新建项目模板”](/analysis-services/analysis-services/media/ssdt-biprojects.png "SSDT 中的“新建项目模板”")  
  
  
##  <a name="report-designer-windows-and-panes"></a><a name="bkmk_ReportDesignerWindowsandPanes"></a> 报表设计器窗口和窗格  
 报表设计器支持两种视图：“设计”视图，可以定义报表数据和报表布局；“预览”视图，可以显示报表的呈现视图。 在每一种视图中都可以显示多个窗口，以帮助您设计或查看呈现的报表。  
  
###  <a name="report-data-pane"></a><a name="bkmk_ReportDataPane"></a> “报表数据”窗格  
 “报表数据”窗格显示内置字段、数据源、数据集、字段集合、报表参数和图像。  
  
 使用“报表数据”窗格可以查看：  
  
-   **内置字段** 预定义的报表信息，如报表名称或处理报表的时间。  
  
-   **数据源** 数据源表示数据来源的名称或与数据来源的连接。  
  
-   **数据集** 每个数据集都包括一个查询，该查询指定要从数据源中检索的数据。 展开数据集可查看由数据集查询指定的字段集合。  
  
     在针对多维数据集的某些查询设计器中，您可以在“筛选器”窗格中指定筛选器，并且指示是否创建报表参数。 如果指定报表参数选项，则将自动创建一个特殊的数据集，以便填充该参数的有效值列表。  默认情况下，该数据集不显示在“报表数据”窗格中。 有关详细信息，请参阅[为多维数据的参数值显示隐藏的数据集（报表生成器和 SSRS）](../../reporting-services/report-data/show-hidden-datasets-for-parameter-values-multidimensional-data.md)。  
  
-   **报表参数** 报表参数的列表。 参数可手动创建，也可以在数据集查询包括查询参数时自动创建。  
  
-   **图像** 可作为报表中的图像报表项包括的图像的列表。  
  
 “报表数据”窗格中的数据源和数据集表示报表定义中的元素。 “报表数据”窗格是多个报表创作环境支持的功能。 在报表生成器中，它是唯一可用于管理数据源和数据集的窗格。 在报表设计器中，“报表数据”窗格使用解决方案资源管理器，该解决方案资源管理器将共享数据源和共享数据集作为文件列出。 “报表数据”窗格中的共享数据源和共享数据集必须指向解决方案资源管理器中的相应共享数据源和共享数据集。 然后，“报表数据”窗格元素将包含对解决方案资源管理器中的数据文件的引用。 项目属性确定共享数据源和共享数据集是否部署到报表服务器或 SharePoint 站点。 有关详细信息，请参阅[转换数据源（报表生成器和 SSRS）](../../reporting-services/report-data/convert-data-sources-report-builder-and-ssrs.md)。  
  
> [!NOTE]  
>  如果看不到“报表数据”窗格，请单击设计区域，然后在“视图”  菜单上单击“报表数据” 。 如果“报表数据”窗格是浮动的，您可以对它进行定位。 有关详细信息，请参阅[在报表设计器中停靠“报表数据”窗格 (SSRS)](../../reporting-services/tools/dock-the-report-data-pane-in-report-designer-ssrs.md)。  
  
  
###  <a name="grouping-pane"></a><a name="bkmk_GroupingPane"></a> “分组”窗格  
 使用“分组”窗格可为 Tablix 数据区域定义组。 您可以为表定义行组和详细信息组，为矩阵定义行组和列组。 不能使用“分组”窗格为图表或其他数据区域定义组。 有关详细信息，请参阅[了解组（报表生成器和 SSRS）](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md)。  
  
 “分组”窗格具有两种模式：  
  
-   **默认。** 使用 **“默认”** 模式可以层次结构格式显示所有行和列组，该层次结构格式显示父组、子组、相邻组以及详细信息组之间的关系。 子组显示在相对于其父组的下一个缩进级别的下面。 相邻组显示在与它的对等组或同级组相同的缩进级别中。  
  
     使用默认模式可添加、编辑或删除组。 对于基于单个数据集字段的组，将该字段拖至“行组”或“列组”窗格中。 可以将组插入到现有组的上面或下面。 若要添加一个相邻组，请右键单击同级组并使用快捷菜单。 若要显示属于某一组的 Tablix 单元，请在“分组”窗格中选择该组。  
  
-   **高级。** 使用 **“高级”** 模式可显示所选 Tablix 数据区域的静态和动态行和列组成员。  必须使用组成员来设置控制与组或组成员关联的行和列可见性的属性，或者来设置呈现程序用于尝试使组保持在单个页面上的规则。 组成员将作为行组和列组区域中的单元显示在设计图面上。  
  
> [!NOTE]  
>  若要在“默认”和“高级”模式之间切换，请右键单击“列组”图标右侧的向下箭头  。  
  
 有关详细信息，请参阅 [Grouping Pane](../../reporting-services/tools/grouping-pane.md)。  
  
  
###  <a name="toolbox"></a><a name="bkmk_Toolbox"></a> 工具箱  
 工具箱包含您可以拖到设计图面的报表项。 数据区域是您用于在报表上组织数据的报表项。 表、矩阵、列表、图表、仪表、数据栏、迷你图和指示器都是数据区域。 其他报表项还包括地图、文本框、矩形、线条、图像和子报表。 如果您的系统管理员安装并注册了这些自定义报表项，则它们也会显示在此列表上。  
  
###  <a name="properties-pane"></a><a name="bkmk_PropertiesPane"></a> “属性”窗格  
 “属性”窗格是一个标准的 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 窗口，可显示设计图面上当前选定报表项的属性名称和值。 在大多数情况下，属性名称与报表定义语言 (RDL) 文件中的元素和特性相对应。 可使用“属性”对话框为选定项设置最常用的属性。 若要打开相应的对话框，请单击“属性”窗格工具栏上的 **“属性页”** 按钮。 高级用户可以在“属性”窗格中直接设置属性值。  
  
 使用“属性”窗格，可以：  
  
-   为设计图面上的当前选定的项设置属性。 某些属性提供了值下拉列表。 您也可以直接在单元格中键入值。 某些属性包含值集合，该集合用值“(集合)”表示。 大部分属性都可以接受表达式；复杂的表达式用值 \<Expression> 表示。 单击“\<Expression>”可打开“表达式”对话框 。 有关详细信息，请参阅 [Expression Dialog Box](/previous-versions/sql/)。  
  
-   使用“属性”窗格工具栏按钮可将网格从类别视图更改为字母顺序视图。 在类别视图中，您可能需要展开类别才能看到它下面的所有属性。 若要打开某项的“属性”对话框，请单击工具栏上的“属性页”按钮，或者右键单击该项并单击“属性” 。  
  
-   为“分组”窗格中的当前所选组成员设置属性。 组成员属性可帮助控制对于每个组实例，静态组头和组尾行是否重复出现。 有关详细信息，请参阅[与组一起显示组头和组尾（报表生成器和 SSRS）](../../reporting-services/report-design/display-headers-and-footers-with-a-group-report-builder-and-ssrs.md)。  
  
 若要显示“属性”窗格，请从 **“视图”** 菜单中单击 **“属性窗口”** 。 您可以取消停靠此窗格，并将它移到 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]窗口的其他区域，或者将它显示为设计图面上的选项卡式视图。  
  
  
###  <a name="solution-explorer"></a><a name="bkmk_SolutionExplorer"></a> 解决方案资源管理器  
 解决方案资源管理器是一个标准的 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 组件，可显示项目中的所有项。 对于报表服务器项目，这包括文件夹以便组织共享数据源、共享数据集、报表和资源。 当您打开解决方案文件时，文件夹项自动按字母顺序出现。 若要在“属性”窗格中查看项属性，请选择该项。  
  
###  <a name="output"></a><a name="bkmk_Output"></a>输出  
 “输出”窗口显示预览报表时出现的处理错误，以及部署报表或共享数据源时出现的发布错误。  
  
 使用“输出”和“文档大纲”窗口有助于调试表达式中的错误。  
  
  
###  <a name="document-outline"></a><a name="bkmk_DocumentOutline"></a> 文档大纲  
 “文档大纲”窗口显示报表定义中所有报表项的层次结构列表。 若要打开“文档大纲”窗格，请在 **“视图”** 菜单中指向 **“其他窗口”** ，然后单击 **“文档窗口”** 。  
  
 使用“文档大纲”窗格有助于按名称标识文本框和其他报表项。 当您在文档大纲中选择某一项时，在设计图面上也将选择该项。  
  
###  <a name="task-list"></a><a name="bkmk_TaskList"></a> 任务列表  
 当从其他应用程序（如 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access）导入报表时，“任务列表”窗口会显示不受支持功能的生成错误。  
  
  
##  <a name="report-designer-design-view"></a><a name="bkmk_ReportDesignerDesignView"></a> 报表设计器设计视图  
 默认情况下，在创建报表服务器项目时，将在“设计”视图中打开报表设计器并显示设计图面。 默认情况下，设计图面显示包括表体和报表背景。  
  
 背景上的快捷菜单提供了可用于添加页眉和页脚的选项，并且可从“视图”菜单显示标尺和“分组”窗格。  
  
 使用缩放控件增大或减小报表的放大倍数。  
  
 若要设计报表，请从工具箱将报表项拖动到报表设计图面，然后配置它们的属性并更改它们在报表上的排列方式。  
  
  
##  <a name="report-designer-preview"></a><a name="bkmk_ReportDesignerPreview"></a> 报表设计器预览  
 使用“预览”可在报表查看器中运行报表并查看呈现的报表。 在本机预览缓存报表数据。 您还可以将配置属性设置为使用浏览器在调试视图中运行报表。  
  
 预览报表时，报表设计器将连接到报表数据源，运行数据集查询，在本地计算机上缓存数据，处理报表以组合数据和布局，然后呈现报表。 您可以在“预览”选项卡中查看报表，也可以将项目属性设置为在调试模式中查看报表并随后直接在浏览器中查看它。  
  
-   **预览参数化报表。** 预览报表时，如果所有报表参数均有有效默认值，则将自动处理报表。 如果一个或多个报表参数没有有效默认值，则您必须为每一个未赋值的参数选择一个值，然后单击报表工具栏上的 **“查看报表”**。  
  
-   **了解本地数据缓存** 预览报表时，报表处理器将使用当前参数默认值运行报表中数据集的所有查询，并将结果保存为本地数据缓存 (.rdl.data) 文件。 您可以继续设计报表，如果不更改报表数据集查询或报表参数，则不会再次产生检索此数据的开销。  
  
-   **使用配置管理器和调试预览报表。** 在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，项目属性定义您要如何部署和调试报表。 这些属性适用于项目中的所有报表和共享数据源。 若要设置项目属性，请从 **“项目”** 菜单中单击 **“属性”** 。 使用这些设置可测试您的报表并将它们发布到报表服务器。  
  
-   **监视“输出”窗格中是否存在错误消息。** 预览报表时，如果报表处理器检测到问题，则会将错误消息写入到“输出”窗格中。  
  
  
##  <a name="report-designer-menus"></a><a name="bkmk_ReportDesignerMenus"></a> 报表设计器菜单  
 当报表设计器项目在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中处于活动状态时，以下工具栏将添加到主工具栏中。 报表设计器菜单仅在“设计”视图中可见。  
  
###  <a name="format-menu"></a><a name="FormatMenu"></a> “格式”菜单  
 当在设计图面上选定项时， **“格式”** 菜单包含以下选项：  
  
-   **前景色** 选择一种文本颜色。 默认文本颜色为黑色。  
  
-   **背景色** 为文本框和数据区域选择背景色。  
  
-   **字体** 指定文本是否加粗、倾斜或带下划线。  
  
-   **两端对齐** 指定文本是右对齐、居中还是左对齐。  
  
-   **对齐方式** 指定所选对象相对于报表内其他对象的对齐方式。  
  
-   **使大小相同** 调整报表内所选对象的大小。  
  
-   **水平间距** 调整报表内所选对象之间的水平间距。  
  
-   **垂直间距** 调整报表内所选对象之间的垂直间距。  
  
-   **在窗体中居中** 使所选对象相对于报表设计器窗口垂直和水平居中。  
  
-   **排序** 将所选对象移到背景或前景中。  
  
###  <a name="report-menu"></a><a name="ReportMenu"></a> “报表”菜单  
 当报表设计图面具有焦点时， **“报表”** 菜单包含下列选项：  
  
-   **报表属性** 选择可打开 **“报表属性”** 对话框。 在此对话框中，您可以分配常规报表属性（如作者姓名和网格间距）以及指定报表布局的属性（如列数和页面大小）。 还可以包括自定义代码、对程序集和类的引用，以及数据输出元素、数据转换和数据架构的名称。  
  
-   **视图**：在以下两个“报表设计器”选项卡之间切换：“设计”和“预览”。  
  
-   **页眉** 添加或删除报表的页眉。 删除页眉时，页眉中的所有项都将被删除。  
  
-   **页脚** 添加或删除报表的页脚。 删除页脚时，页脚中的所有项都将被删除。  
  
-   **分组窗格** 显示或隐藏“分组”窗格。  
  
###  <a name="view-menu"></a><a name="ViewMenu"></a> “视图”菜单  
 使用 **“视图”** 菜单可显示报表设计器窗口和工具栏  
  
-   **错误列表** 使用此选项可显示发布或预览报表时检测到的错误。  
  
-   **输出** 使用此选项可显示发布或处理报表时检测到的错误，或有关报表显示文本“错误号”时所出现表达式错误的详细信息。  
  
-   **属性窗口** 使用此选项可显示设计图面上当前选定报表项的属性值。 若要查看嵌套报表项的属性，则必须多次单击报表项，以循环通过该报表项及其嵌套成员的层次结构。 检查“属性”窗格顶部显示的项名称可查看显示的是哪个报表项的属性。  
  
-   **工具箱** 使用此选项可显示工具箱。  
  
-   **其他窗口** 使用此选项可显示下列窗格：  
  
    -   **文档大纲** 使用此选项可在报表中显示报表项及其文本框集合的层次结构视图。  
  
-   **工具栏** 使用此选项可显示支持报表设计器功能的工具栏，包括 **“报表边框”** 和 **“报表格式”** 。 有关详细信息，请参阅 [报表设计器工具栏](#bkmk_ReportDesignerToolbars)。  
  
-   **报表数据** 使用此选项可显示“报表数据”窗格，您可以在该窗格添加报表参数、数据源、数据集和图像。  
  
###  <a name="project-menu"></a><a name="ProjectMenu"></a> “项目”菜单  
 使用 **“项目”** 菜单可管理项目中的共享数据源和报表。 向项目添加项或从中移除项时，解决方案资源管理器中的项目项的层次结构显示会自动更新。  
  
-   **添加新项** 向项目添加新的共享数据源或新的报表。  
  
-   **添加现有项** 向项目添加现有共享数据源或现有报表。  
  
-   **导入报表** 从其他应用程序（例如 Microsoft Access）导入报表。  
  
-   **从项目中排除** 从项目中排除项。 此选项不会将项从您的文件系统中删除。  
  
-   **显示所有文件** 显示项目中的所有文件。  
  
-   **刷新项目工具箱项** 当您在项目中安装新的自定义报表项时刷新工具箱缓存。  
  
-   **属性** 为项目打开 **“属性页”** 对话框。 有关详细信息，请参阅 [“项目属性页”对话框](../../reporting-services/tools/project-property-pages-dialog-box.md)。  
  
  
##  <a name="report-designer-toolbars"></a><a name="bkmk_ReportDesignerToolbars"></a> 报表设计器工具栏  
 报表设计器提供了下列可在设计报表时使用的专用工具栏：  
  
-   **报表** 添加页眉或页脚，设置报表属性，切换标尺或“分组”窗格，或使用缩放更改报表视图。  
  
-   **报表边框** 为所有选定行以及所有选定报表项的边框设置颜色、样式和宽度。  
  
-   **报表格式** 设置选定报表项的格式。 对于文本框，可使用该工具栏更改下列类型的格式：字体属性和文本颜色、背景色以及文本对齐。  
  
-   **布局** 设置报表项的绘制顺序和数据区域内的合并单元。  
  
-   **标准** 打开或保存项目，显示窗口，以及选择“调试”配置。  
  
 使用 **“视图”** 菜单可控制是否显示这些工具栏。 如果其他 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 工具栏的功能不适用于报表设计器功能，则它们可能会被禁用。  
  

##  <a name="source-control"></a><a name="bkmk_SourceControl"></a> 源代码管理  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 可与源插件集成。使用“选项”对话框中的“项目和解决方案”页可指定插件和配置属性。  
  
##  <a name="custom-report-templates"></a><a name="bkmk_CustomReportTemplates"></a> 自定义报表模板  
 若要将自定义报表用作新报表的模板，只需将其复制到安装 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 的计算机上的 ReportProject 文件夹。 默认情况下，此文件夹位于以下位置：`<drive>:\Program Files\Microsoft Visual Studio 14.0\Common7\IDE\Private Assemblies\ProjectItems\ReportProject`。 向报表项目中添加新项时，自定义报表将显示在“模板”窗格中。  
  
 还可以向报表向导添加自定义样式。  
  
  
##  <a name="command-line-support-for-sql-server-data-tools"></a><a name="bkmk_CommandLineSupportForssdt"></a> 针对 SQL Server Data Tools 的命令行支持  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 基于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 和基础 devenv.exe 应用程序。 必须先为以下两项设置有效值，才能使用这些选项：  
  
-   项目的 OverwriteDataSources、TargetDataSourceFolder、TargetReportFolder 和 TargetServerURL 属性。  
  
-   至少一组配置属性，例如，Debug 或 Release。  
  
 有关详细信息，请参阅 [Publishing Data Sources and Reports](../../reporting-services/reports/publishing-data-sources-and-reports.md)。  
  
 对于报表服务器项目，您可以通过命令行指定以下选项：  
  
-   **/deploy** 使用配置文件中指定的项目属性部署报表。 例如，以下命令通过使用项目属性中指定的 Release 配置设置来部署解决方案文件 Reports.sln 指定的报表：  
  
    ```  
    devenv.exe "C:\Users\MyUser\Documents\Visual Studio 2015\Projects\Reports\Reports.sln" /deploy "Release"  
    ```  
  
-   **/build** 生成解决方案文件，但不部署它。 例如，以下命令通过使用项目属性中指定的 Debug 配置设置来生成解决方案文件 Reports.sln 指定的报表：  
  
    ```  
    devenv.exe "C:\Users\MyUser\Documents\Visual Studio 2015\Projects\Reports\Reports.sln" /build "Debug"  
    ```  
  
-   **/out** 将生成解决方案所产生的输出结果重定向到指定的文件。 例如，以下命令会将在上一示例中生成的输出结果重新定向到名为 mybuildlog.txt 的文件。  
  
    ```  
    devenv.exe "C:\Users\MyUser\Documents\Visual Studio 2015\Projects\Reports\Reports.sln" /build "Debug" /out mybuildlog.txt  
    ```  
  
##  <a name="keyboard-shortcuts-in-reporting-services"></a><a name="bkmk_KeyboardShortcuts"></a> Reporting Services 中的键盘快捷方式  
 使用键盘快捷键可以：  
  
-   控制 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中的窗口和模式：  
  
    |说明|键组合|  
    |-----------------|---------------------|  
    |生成选定的项目|Ctrl+Shift+B|  
    |显示“属性”窗口|F4|  
    |显示数据窗口|Ctrl+Alt+D|  
    |开始调试|F5|  
    |从一个已打开的窗口移到下一个窗口|F6|  
  
-   控制报表设计图面上的项：  
  
    |说明|键组合|  
    |-----------------|---------------------|  
    |将焦点从一个报表项移到下一个报表项|Tab|  
    |移动选定的报表项|箭头键|  
    |微移选定的报表项|Ctrl+箭头键|  
    |增大或减小所选报表项|Ctrl+Shift+箭头键|  
    |在文本框中，将光标移到可见显示文本的开头|Ctrl+Home|  
    |在文本框中，将光标移到可见显示文本的末尾|Ctrl+End|  
    |在文本框中，选择从当前光标位置到可见显示文本开头的文本|Shift+Home|  
    |在文本框中，选择从当前光标位置到可见显示文本末尾的文本|Shift+End|  
    |在文本框中，选择从当前光标位置到表达式开头的文本|Ctrl+Shift+Home|  
    |在文本框中，选择从当前光标位置到表达式末尾的文本|Ctrl+Shift+End|  
    |打开所选报表项的快捷菜单|Shift+F10+新型键盘上的属性键（右侧 Windows 键旁边的键，代替鼠标右键）|
  
## <a name="next-steps"></a>后续步骤

[下载 SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)
[解决方案资源管理器](../../ssms/solution/solution-explorer.md)   
[Reporting Services 报表](../../reporting-services/reports/reporting-services-reports-ssrs.md)   
[报表定义语言](../../reporting-services/reports/report-definition-language-ssrs.md)   
[SQL Server Data Tools 中的部署和版本支持](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)