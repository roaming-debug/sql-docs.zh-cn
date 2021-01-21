---
title: 使用报表设计器设计报表 | Microsoft Docs
description: 了解如何使用 SQL Server Reporting Services 中的报表设计器来创建功能齐全的分页报表和报表解决方案。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- Report Designer [Reporting Services], report creation
ms.assetid: 3a26dccc-6ad6-48f5-a882-f96c6c0dd405
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 500ecc4e4b0dae80faa06f8d152b99423bb45251
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597175"
---
# <a name="design-reporting-services-paginated-reports-with-report-designer-ssrs"></a>使用报表设计器设计 Reporting Services 分页报表 (SSRS)

使用报表设计器可创建功能齐全的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 分页报表和报表解决方案。 报表设计器提供可在其中定义数据源、数据集和查询的图形界面，用于数据区域和字段的报表布局位置，以及交互功能（例如一起使用的参数和报表集）。  

报表设计器是  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]的一项功能，后者是一种用于创建商业智能解决方案的 Microsoft Visual Studio 环境。 SQL Server 不提供 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]。 下载 [SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)。 
  
## <a name="benefits-of-report-projects"></a>报表项目的优点  
报表项目充当报表定义和资源的容器。 使用项目可以：  
  
-   将报表和相关项组织到一个容器中。  
  
-   在本地测试包含报表和相关项的报表解决方案。  
  
-   一起部署相关项。 使用项目属性和配置管理以便部署到多个环境。  
  
-   为报表和相关项保留一组主副本。 部署后，已发布的报表可能会意外修改。  
  
 利用本主题中的信息可为 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 解决方案中的单个报表项目设计分页报表和相关项。 有关 SQL Server Data Tools 中的解决方案和多个项目的详细信息，请参阅 [SQL Server Data Tools 中的 Reporting Services](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md)。  

  
##  <a name="shared-data-sources"></a><a name="bkmk_SharedDataSources"></a> 共享数据源  
 使用 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 可为报表解决方案定义和部署共享数据源。 可通过使用 **OverwriteDataSources** 和 **TargetDataSourceFolder** 属性，脱离项目中的其他项而单独部署共享数据源。 有关详细信息，请参阅[设置部署属性 (Reporting Services)](../../reporting-services/tools/set-deployment-properties-reporting-services.md)。  
  
 在报表设计器中，您可以使用“报表数据”窗格和解决方案资源管理器来定义在报表中使用的数据源。 有关详细信息，请参阅 [Report Data Pane](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_ReportDataPane)。 不能使用 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 打开已发布到某一报表服务器或 SharePoint 站点、但不包括在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 解决方案中的数据源。 对于该功能，请使用[报表生成器创作环境 (SSRS)](../../reporting-services/tools/report-builder-authoring-environment-ssrs.md)。  
  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 是一个客户端工具。 您可以在您的计算机上在本地测试报表解决方案，将其部署到测试环境以便测试服务器解决方案，然后将其部署到生产环境。 在部署后，验证为报表服务器环境配置了数据源处理扩展插件和数据源凭据。 您可以使用配置管理器来帮助管理不同部署的属性。 有关详细信息，请参阅 [SQL Server Data Tools 中的 Reporting Services (SSDT)](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md)。  
  
 有关详细信息，请参阅[创建数据连接字符串 - 报表生成器和 SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)。  
   
##  <a name="shared-datasets"></a><a name="bkmk_SharedDatasets"></a> 共享数据集  
 使用 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 可为报表解决方案定义和部署共享数据集。 可通过使用 **OverwriteDatasets** 和 **TargetDatasetFolder** 属性，脱离项目中的其他项而单独部署共享数据集。 有关详细信息，请参阅[设置部署属性 (Reporting Services)](../../reporting-services/tools/set-deployment-properties-reporting-services.md)。  
  
 在报表设计器中，您可以使用“报表数据”窗格和解决方案资源管理器来定义在报表中使用的共享数据集。 有关详细信息，请参阅 [Report Data Pane](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_ReportDataPane)。 不能使用 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 从报表服务器或 SharePoint 站点直接打开已发布的数据集。 对于该功能，请在共享数据集模式下使用[报表生成器创作环境 (SSRS)](../../reporting-services/tools/report-builder-authoring-environment-ssrs.md)。  
  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 是一个客户端工具。 您可以使用查询设计器在本地以预览方式创建和测试您的查询结果。 在部署后，您可以独立于共享数据集所依赖的共享数据源和报表来单独管理共享数据集。 有关详细信息，请参阅[报表的嵌入数据集和共享数据集（报表生成器和 SSRS）](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)、[查询设计工具 (SSRS)](../../reporting-services/report-data/query-design-tools-ssrs.md) 和[共享数据集](../../reporting-services/report-data/manage-shared-datasets.md)。  
  
##  <a name="paginated-reports"></a><a name="bkmk_Reports"></a> 分页报表  
分页报表是报表项目中存储的文件。 报表可用作独立报表、子报表或者从主报表进行的钻取操作的目标。 可通过使用 **TargetReportFolder** 和其他属性，脱离项目中的其他项而单独部署报表。 有关详细信息，请参阅[设置部署属性 (Reporting Services)](../../reporting-services/tools/set-deployment-properties-reporting-services.md)。  
  
> [!NOTE]  
>  如果您在 SharePoint 模式下发布到某一报表服务器，则某些报表解决方案功能将无法在报表设计器项目中进行测试。 对报表、子报表和钻取报表的引用必须使用全限定的 URL，此类 URL 只能在您部署报表项目后进行测试。 有关详细信息，请参阅 [用于 SharePoint 模式下在报表服务器上已发布的报表项的 URL 示例 (SSRS)](../../reporting-services/tools/url-examples-for-items-on-a-report-server-sharepoint-mode.md)。  
  
 可以按下列方式向项目中添加报表：  
  
-   **添加新的报表项目。** 默认情况下，一个空报表将在报表设计器中打开。 有关详细信息，请参阅[向报表项目中添加新报表或现有报表 (SSRS)](../../reporting-services/tools/add-a-new-or-existing-report-to-a-report-project-ssrs.md)。  
  
-   **添加报表向导项目。** 按照向导中的说明逐步创建报表。 报表向导将数据定义和报表设计简化成一系列完成报表的步骤。 您可以添加样式以便为自己的组织自定义向导。 有关详细信息，请参阅[向报表项目中添加新报表或现有报表 (SSRS)](../../reporting-services/tools/add-a-new-or-existing-report-to-a-report-project-ssrs.md)。  
  
-   **添加报表类型的新项。** 一个空报表将在报表设计器中打开。  
  
-   **添加现有项。** 一个现有报表定义 (.rdl) 将在报表设计器中打开。 从早期版本的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 打开某一报表或项目可能会将该项目自动升级到当前版本，或者将该报表升级到当前架构。 有关更多信息，请参见 [Upgrade Reports](../../reporting-services/install-windows/upgrade-reports.md)。  
  
-   导入 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access 报表。 从 Access 数据库（.mdb、.accdb）或项目 (.adp) 文件中导入所有报表。 报表设计器将数据库或项目文件中的所有报表都转换为 RDL 格式，然后将其保存在报表项目中。 并非 Access 报表的所有功能都转移到报表定义 (.rdl) 文件中。 有关详细信息，请参阅[从 Microsoft Access 导入报表 (Reporting Services)](../reports/reporting-services-reports-ssrs.md) 和[支持的 Access 报表功能 (SSRS)](../reports/reporting-services-reports-ssrs.md)。  
  
    > [!NOTE]  
    >  若要使用导入功能，必须将 Access 2002 或更高版本与报表设计器安装在同一台计算机上。 在导入 Access 报表时，必须能够使用报表的数据源。  
  
-   **直接使用 RDL。** 在报表设计器中编写报表时，报表将以 XML 格式另存为报表定义语言 (RDL) 文件。 您可以在报表设计器、文本编辑器或任何可以编辑 XML 的工具中编辑此文件。  
  
     当您在报表设计器中编辑报表定义源时，您使用的是从其安装了开发工具的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本的当前 RDL 架构。 在生成项目时，架构版本可能会根据您的部署属性而发生变化。 有关详细信息，请参阅 [SQL Server Data Tools 中的部署和版本支持 (SSRS)](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)中。  
  
     直接编辑 RDL 可能导致报表无法发布到报表服务器，或者无法运行。 对于任何 XML 文件，请确保对元素内使用的 XML 特定字符进行了正确编码。 发布报表时，报表服务器会使用该架构来验证 RDL 文件中包含的 XML。  
  
     若要包括不属于 RDL 架构的元素，请将其放置于自定义元素中。 自定义元素可由自定义呈现扩展插件读取，但将被 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]随附的呈现扩展插件忽略。 例如，您可以使用自定义元素存储报表中的注释。  
  
     有关详细信息，请参阅[报表定义语言 (SSRS)](../../reporting-services/reports/report-definition-language-ssrs.md)。  
  
##  <a name="report-parts"></a><a name="bkmk_ReportParts"></a> 报表部件  
 在报表设计器中，在你创建了表、图表和项目中的其他分页报表项后，可以将它们作为“报表部件”发布到报表服务器或与报表服务器相集成的 SharePoint 站点中，以便你和他人可以在其他报表中重复使用它们。 有关详细详细信息，请参阅[报表设计器中的报表部件 (SSRS)](../../reporting-services/report-design/report-parts-in-report-designer-ssrs.md)。  
  
 可通过使用 **TargetReportPartFolder** 和其他属性，脱离项目中的其他项而单独部署报表部件。 有关详细信息，请参阅[设置部署属性 (Reporting Services)](../../reporting-services/tools/set-deployment-properties-reporting-services.md)。  
  
##  <a name="resources"></a><a name="bkmk_Resources"></a> Resources  
 您可以将与您的项目相关、但报表服务器尚未处理的文件添加到您的项目中。 例如，您可以为图片添加图像，或者为空间数据添加 ESRI 形状文件。 有关详细信息，请参阅[资源](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md#bkmk_Resources)。  
 
##  <a name="paginated-report-layout"></a><a name="bkmk_ReportLayout"></a> 分页报表布局  
 若要创建报表布局，请将报表项和数据区域从工具箱拖到设计图面上，然后对其进行排列。 将数据集字段拖至设计图面上的项，以便向报表添加数据。 若要在 Tablix 数据区域的组中组织数据，请将数据集字段拖至“分组”窗格。 因为报表创作工具是实质上是一种创建报表定义的方法，所以，用于报表设计的方法在报表生成器和报表设计器之间十分相似。  
   
##  <a name="preview-a-paginated-report"></a><a name="bkmk_Preview"></a> 预览分页报表  
 使用 **“预览”** 可以验证报表数据和布局设计。 在你预览报表时，报表处理器将对报表定义架构和表达式语法进行验证，然后在 [Output](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_Output) 窗口中列出问题。  
  
> [!NOTE]  
>  在预览报表时，报表数据将缓存到本地计算机上的文件中。 使用相同的查询、参数和凭据再次预览同一报表时，报表设计器将检索缓存副本，而不是重新运行查询。 数据文件将在报表定义文件所在的同一目录中另存为 \<reportname>.rdl.data。 关闭报表设计器时，不会删除该文件。  
  
 您可以通过以下方式预览报表：  
  
-   **“预览”视图。** 单击 **预览** 选项卡。报表将在本地运行，使用的报表处理功能和呈现功能与报表服务器所提供的功能相同。 所显示的报表是一个交互式的图像；您可以选择参数、单击链接、查看文档结构图以及展开和折叠报表的隐藏区域。 您还可以使用所安装的任意一种呈现格式将报表导出。  
  
-   **独立预览。** 在浏览器中运行本地报表。 通过使用调试配置，您还可以使用此模式调试您撰写的自定义程序集。 在调试模式下运行项目的方法有以下三种：  
  
    -   在 **“调试”** 菜单中，单击 **“启动调试”** 。  
  
    -   在 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 标准工具栏上，单击 **“启动”** 按钮。  
  
    -   按 F5。  
  
     如果使用生成报表但不部署该报表的项目配置，则将在单独的预览窗口中打开在当前配置的 **StartItem** 属性中指定的报表。  
  
    > [!NOTE]  
    >  若要使用调试模式，必须设置开始项。 在解决方案资源管理器中右键单击报表项目，再单击 **“属性”** ，然后在 **StartItem** 中选择要显示的报表的名称。  
  
     若要预览项目开始项之外的特定报表，请选择生成报表但不部署该报表的配置（例如，DebugLocal 配置），右键单击报表，再单击 **“运行”**。 必须选择不部署报表的配置；否则，报表将发布到报表服务器，而不是显示在本地预览窗口中。  
  
-   **打印预览。**  
  
     首次在预览模式下或在预览窗口中查看报表时，报表的视图类似于 HTML 呈现扩展插件所生成的报表。 虽然预览并非 HTML 格式，但报表的布局和分页与 HTML 格式的输出结果类似。  
  
     可通过切换到打印预览模式，查看报表的打印效果。 单击预览工具栏上的 **“打印预览”** 按钮。 所显示的报表就如同打印在纸张上一样。 此视图与图像呈现扩展插件和 PDF 呈现扩展插件所生成的输出类似。 虽然打印预览并非图像或 PDF 文件，但报表的布局和分页与这些格式的输出类似。 您可以选择报表图像的大小，例如，设置页面宽度。  
  
     打印预览可帮助您标识在打印报表时可能遇到的许多呈现问题。 常见的呈现问题包括：  
  
    -   由于报表太宽以致您为报表指定的纸张大小无法容纳而导致的多余空白页。  
  
    -   由于报表包含的矩阵动态扩展以致超出指定的纸张宽度而导致的多余空白页。  
  
    -   组之间的分页符不像您所希望的那样工作。  
  
    -   页眉和页脚不按预期显示。  
  
    -   报表布局需要修改，才能更好地以打印格式阅读。  
   
##  <a name="save-and-deploy-paginated-reports"></a><a name="bkmk_SaveandDeploy"></a> 保存和部署分页报表  
 在报表设计器中，您可以在本地保存报表和其他项目文件，或者将它们部署到报表服务器或 SharePoint 站点。 共享数据源、共享数据集、报表、报表资源和报表部件可以单独部署或一起部署，具体部署方式取决于您配置的项目部署属性。 有关详细信息，请参阅 [Configuration and Deployment Properties](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md#bkmk_ConfigurationandDeploymentProperties)。  
  
 在报表设计器中，必须了解如何使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中当前版本的 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]所支持的报表定义架构设计报表。 在您为特定的报表服务器或 SharePoint 站点设置项目部署属性，然后保存报表时，报表设计器将报表定义保存到架构中与目标报表服务器上的版本匹配的生成目录中。 若要创建可在下级报表服务器上发布的报表，报表设计器将删除在目标架构中不存在的报表项。 这种情况将自动发生，而没有提示。 在发生此情况时，原始报表定义将保留在项目文件夹中。 已部署的修改的报表定义位于生成文件夹中。  
  
> [!NOTE]  
>  对于调试表达式和部署错误，您必须在生成文件夹中查看报表定义。 不要使用 **“查看源”** 。 **“查看源”** 显示项目文件夹中的报表定义源。  
  
 有关详细信息，请参阅 [SQL Server Data Tools 中的部署和版本支持 (SSRS)](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)中。  
  
### <a name="save-a-report-locally"></a>本地保存报表  
 当您在报表设计器中处理报表或其他项目项时，文件将保存到您的本地计算机或者您有权访问的其他计算机上的共享区。  
  
 如果使用的是源代码管理软件，则可以在保存报表时在源代码管理服务器中检查您的报表。 有关详细信息，请参阅 [Source Control](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_SourceControl)。  
  
### <a name="deploy-or-publish-paginated-reports"></a>部署或发布分页报表  
 从 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]，您可以将报表或其他项目项部署到多个版本的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表服务器。 使用项目配置可以控制报表定义升级到与目标报表服务器兼容的架构版本。 项目配置控制的属性包括目标报表服务器、生成进程临时存储报表定义以便预览和部署的文件夹以及错误级别。 有关详细信息，请参阅[配置和部署属性](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md#bkmk_ConfigurationandDeploymentProperties)和[设置部署属性 (Reporting Services)](../../reporting-services/tools/set-deployment-properties-reporting-services.md)。  
  
### <a name="export-a-paginated-report-to-a-different-file-format"></a>将分页报表导出为不同的文件格式  
 报表可导出为各种格式，这些格式会影响某些报表布局和交互式功能的行使。 有关各种输出格式的设计注意事项的详细信息，请参阅[导出报表（报表生成器和 SSRS）](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md)。  
   
##  <a name="report-validation-and-error-levels"></a><a name="bkmk_ReportValidationandErrorLevels"></a> 报表验证和错误级别  
 在预览之前和在部署过程中对报表进行验证。 当生成报表时，可能发生许多生成问题。 例如，报表可能包含与项目配置指定的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 版本不兼容的字符串（如表达式或查询）。  
  
 使用 ErrorLevel 属性可以管理生成警告和错误。 ErrorLevel 属性可以包含值 0 到 4（包括这两者）。 此值确定将哪些生成问题报告为错误，以及将哪些生成问题报告为警告。 默认值为 2。 警告和错误将写入到 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)][输出](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_Output) 窗口中。  
  
 严重级别小于或等于 ErrorLevel 的值的问题将报告为错误；否则，将问题报告为警告。  
  
 下表列出了错误级别。  
  
|错误级别|描述|  
|-----------------|-----------------|  
|0|最严重且无法避免的生成错误，将阻止预览和部署报表。|  
|1|严重的生成错误，会彻底更改报表布局。|  
|2|不太严重的生成错误，会明显更改报表布局。|  
|3|很小的生成问题，以轻微的方式更改报表布局，您可能注意不到所发生的更改。|  
|4|仅用于发布警告。|  
  
 当你尝试预览或部署的报表包含 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]中新增的报表项时，可能会从报表中删除这些报表项。 默认情况下，此配置的 ErrorLevel 属性设置为 2，这样，当删除地图时，将导致报表生成过程失败。 然而，如果将 ErrorLevel 属性的值更改为 0 或 1，则将删除地图，发出警告，但生成过程继续进行。  

## <a name="next-steps"></a>后续步骤

[下载 Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)  
[SQL Server Data Tools 中的 Reporting Services](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md)   
[查询设计工具](../../reporting-services/report-data/query-design-tools-ssrs.md)   
[SQL Server Data Tools 中的部署和版本支持](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)