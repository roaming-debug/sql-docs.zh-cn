---
title: 导出报表（报表生成器）| Microsoft Docs
description: 可以使用报表生成器、报表设计器或报表服务器将 Reporting Services 报表导出为其他文件格式，如 PowerPoint 或 PDF。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d82c6f45d6cd8e4e35735291433d3e054afe5481
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024103"
---
# <a name="export-reports-report-builder-and-ssrs"></a>导出报表（报表生成器和 SSRS）

  你可以将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表导出为其他文件格式（如 PowerPoint、图像、PDF、 [!INCLUDE[ofprword](../../includes/ofprword-md.md)]或 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] ），也可以通过生成 Atom 服务文档，列出可从报表获得的与 Atom 兼容的数据馈送来导出报表。 可以从报表生成器、报表设计器 ([!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]) 或报表服务器导出报表。  
  
 导出报表后，可以执行以下操作：  
  
-   **在其他应用程序中处理报表数据。** 例如，可以将报表导出为 Excel 格式，然后在 Excel 中继续处理相应数据。  
  
-   **以不同的格式打印报表。** 例如，可以将报表导出为 PDF 文件格式，并打印报表。  
  
-   **将报表的副本另存为其他文件类型。** 例如，可以将报表导出为 Word 格式并将其保存来创建报表的副本。  
  
-   **在应用程序中将报表数据用作数据馈送。** 例如，可以生成 Power Pivot、Power BI 可使用的与 Atom 兼容的数据馈送，然后在 Power Pivot 或 Power BI 中处理数据。 有关详细信息，请参阅[基于单个报表生成数据馈送](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md)  
  
-   在设置订阅或通过电子邮件传递报表时，或者希望保存报表服务器上可用的报表时，呈现报表服务器上的报表将非常有用。 有关详细信息，请参阅[订阅和传递 (Reporting Services)](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)。  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 提供多个呈现扩展插件，以支持将报表导出为常见文件格式。 呈现扩展插件支持带软分页的文件格式（如 Word 或 Excel）、带硬分页的文件格式（如 PDF 或 TIFF）或仅带数据的文件格式（如 CSV 或与 Atom 兼容的 XML）。  
  
 当以不同的格式导出报表时，报表分页可能会受到影响。 预览报表时，您查看的是遵循软分页规则的 HTML 呈现扩展插件呈现的报表效果。 将报表导出为其他文件格式（如 Adobe Acrobat (PDF)）时，分页是根据物理页大小进行的，这遵循的是硬分页规则。 还可以用您添加到报表的逻辑分页符来分隔各页，但页的实际长度会因您使用的呈现器类型而异。 若要更改报表的分页情况，必须了解您选择的呈现扩展插件的分页行为。 您可能需要针对此呈现扩展插件调整报表布局的设计。 有关详细信息，请参阅[页面布局和呈现方式](../../reporting-services/report-design/page-layout-and-rendering-report-builder-and-ssrs.md)。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]

## <a name="to-export-a-report-from-report-builder"></a><a name="bkmk_export_from_rb"></a> 从报表生成器导出报表

1.  运行或预览报表。  
  
2.  在功能区上，单击 **“导出”** 。  
  
     ![报表生成器导出](../../reporting-services/report-builder/media/ssrb-export.png "报表生成器导出")  
  
3.  选择要使用的格式。  
  
     将打开“另存为”  对话框。 默认情况下，文件名为所导出报表的名称。 也可更改该文件名。  
  
##  <a name="to-export-a-report-from-the-ssrsnoversion-web-portal"></a><a name="bkmk_export_from_rm"></a> 从 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户导出报表  
  
1.  从 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户的“主页”  导航至要导出的报表。  
  
2.  单击报表来呈现并预览报表。  
  
3.  在报表查看器工具栏上，单击“导出”  下拉箭头。  
  
     ![Reporting Services Web 门户导出](../../reporting-services/report-builder/media/ssrsportal-export.png "Reporting Services Web 门户导出")  
  
4.  选择要使用的格式。  
  
5.  单击“导出”。  将出现一个对话框，询问你是否要打开或保存该文件。  
  
6.  若要以选定的导出格式查看该报表，请单击 **“打开”** 。  
  
     \- 或 -  
  
     若要立即以选定的导出格式保存该报表，请单击 **“保存”** 。  
  
     将使用与所选格式关联的应用程序显示或保存该报表。 如果单击 **“保存”** ，系统将提示您指定保存报表的位置。  
  
##  <a name="to-export-a-report-from-a-sharepoint-library"></a><a name="bkmk_export_from_sharepoint"></a> 从 SharePoint 库导出该报表。  
  
1.  预览报表。  
  
2.  在工具栏上，单击 **“操作”** ，指向 **“导出”** ，然后选择要使用的格式。  
  
     将打开 **“文件下载”** 对话框。  
  
3.  若要以选定的导出格式查看该报表，请单击 **“打开”** 。  
  
     \- 或 -  
  
     若要立即以选定的导出格式保存该报表，请单击 **“保存”** 。  
  
     将使用与所选格式关联的应用程序显示或保存该报表。 如果单击 **“保存”** ，系统将提示您指定保存报表的位置。  
  
     还可以选择更改导出的报表的文件名。  
  
     **注意** 如果因没有与此文件类型关联的程序而导致程序无法以您所选的格式打开此报表，系统会提示您保存导出的报表或在线查找可打开此报表的程序。  
  
##  <a name="rendering-extension-types"></a><a name="RendererTypes"></a> 呈现扩展插件类型  
 有三种类型的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 呈现扩展插件：  
  
-   **数据呈现扩展插件** 数据呈现扩展插件会去除报表中的所有格式设置和布局信息而仅显示数据。 可使用所产生的文件将原始报表数据导入为其他文件类型，如 Excel、其他数据库、XML 数据消息或自定义应用程序。 数据呈现器不支持分页。  
  
     支持以下数据呈现扩展插件：CSV、XML 和 Atom。  
  
-   **软分页呈现扩展插件** 软分页呈现扩展插件保留报表的布局和格式设置。 为了满足基于屏幕的查看和传递（例如在网页上或在 **ReportViewer** 控件中）需要，对所产生的文件进行优化。  
  
     支持以下软分页呈现扩展插件：[!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel、[!INCLUDE[msCoName](../../includes/msconame-md.md)] Word 和 Web 存档 (MHTML)。  
  
-   **硬分页呈现扩展插件** 硬分页呈现扩展插件保留报表的布局和格式设置。 对所产生的文件进行了优化，以便提供一致的打印体验或以书本格式联机查看报表。  
  
     支持以下硬分页呈现器扩展插件：TIFF 和 PDF。  
  
##  <a name="formats-you-can-export-while-viewing-reports"></a><a name="ExportFormats"></a> 查看报表时可以导出的格式  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 提供了用于以不同格式呈现报表的呈现扩展插件。 应针对你所选的文件格式优化报表设计。  下表列出了你可以从用户界面导出的格式。  此外，还列出了你可以结合 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅或在从 URL 访问导出报表时使用的其他格式。  请参阅本主题中的 [导出报表的其他方式](#OtherWaysExportingReports)部分。  
  
|格式|呈现扩展插件类型|说明|  
|------------|------------------------------|-----------------|  
|Acrobat (PDF) 文件|硬分页|PDF 呈现扩展插件可将报表呈现为特定格式的文件，以便在 Adobe Acrobat 和其他支持 PDF 1.3 的第三方 PDF 查看器中打开。 尽管 PDF 1.3 与 Adobe Acrobat 4.0 及更高版本兼容，但 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 支持 Adobe Acrobat 6 或更高版本。 呈现扩展插件不需要使用 Adobe 软件呈现报表。 不过，该插件需要使用 PDF 查看器（例如 Adobe Acrobat）才可查看或打印 PDF 格式的报表。<br /><br /> 有关详细信息，请参阅[导出到 PDF 文件](../../reporting-services/report-builder/exporting-to-a-pdf-file-report-builder-and-ssrs.md)。|  
|Atom|数据|Atom 呈现扩展插件将从报表生成与 Atom 兼容的数据馈送。 数据馈送是可读的，并可以与可使用与 Atom 兼容的数据馈送的应用程序（如 Power Pivot、Power BI）进行交换。<br /><br /> 输出为一个 Atom 服务文档，该文档列出报表中可用的数据馈送。 为报表中的每个数据区域至少创建一个数据馈送。 根据数据区域的类型以及数据区域显示的数据，可以生成多个数据馈送。<br /><br /> 有关详细信息，请参阅[基于报表生成数据馈送](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md)。|  
|CSV|数据|逗号分隔值 (CSV) 呈现扩展插件以平展的表示形式呈现报表中的数据，格式为标准化的纯文本，这种数据表示形式容易读取且可与多个应用程序交换。<br /><br /> 有关详细信息，请参阅[导出到 CSV 文件](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md)。|  
|EXCELOPENXML|软分页|查看报表时导出菜单中显示为“Excel”。 Excel 呈现扩展插件可将报表呈现为与 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2013 兼容的 Excel 文档 (.xlsx)。  有关详细信息，请参阅[导出到 Microsoft Excel](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md)。|  
|PowerPoint|硬分页|PowerPoint 呈现扩展插件可将报表呈现为与 PowerPoint 2013 兼容的 PowerPoint 文档 (.pptx)。|  
|TIFF 文件|硬分页|图像呈现扩展插件可以将报表呈现为位图或图元文件。 默认情况下，图像呈现扩展插件将生成报表的 TIFF 文件，您可以按多页形式查看此类文件。 客户端收到图像时，可以在图像查看器中显示图像，并可以打印图像。<br /><br /> 图像呈现扩展插件可以采用 [!INCLUDE[ndptecgdiplus](../../includes/ndptecgdiplus-md.md)] 支持的任何格式生成文件：BMP、EMF、EMFPlus、GIF、JPEG、PNG 和 TIFF。<br /><br /> 有关详细信息，请参阅[导出到图像文件](../../reporting-services/report-builder/exporting-to-an-image-file-report-builder-and-ssrs.md)。|  
|Web 存档|软分页|HTML 呈现扩展插件以 HTML 格式呈现报表。 该呈现扩展插件还可以生成完整的 HTML 页面，或生成 HTML 片段以嵌入其他 HTML 页面。 所有 HTML 都是使用 UTF-8 编码生成的。<br /><br /> 对于在报表生成器中预览并在浏览器中查看的报表（包括在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户中运行时），HTML 呈现扩展插件都是默认的呈现扩展插件。<br /><br /> 有关详细信息，请参阅[以 HTML 格式呈现](../../reporting-services/report-builder/rendering-to-html-report-builder-and-ssrs.md)。|  
|WORDOPENXML|软分页|查看报表时导出菜单中显示为“Word”。 Word 呈现扩展插件可将报表呈现为与 [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2013 兼容的 Word 文档 (.docx)。  有关详细信息，请参阅[导出到 Microsoft Word](../../reporting-services/report-builder/exporting-to-microsoft-word-report-builder-and-ssrs.md)。|  
|XML|数据|XML 呈现扩展插件可以按 XML 格式返回报表。 报表 XML 的架构专用于相应的报表，并且只包含数据。 布局信息呈现以及分页都不是由 XML 呈现扩展插件完成。 此扩展插件生成的 XML 可以导入到数据库中用作 XML 数据消息，或发送到自定义应用程序。<br/><br/> 有关详细信息，请参阅[导出到 HTML](../../reporting-services/report-builder/exporting-to-xml-report-builder-and-ssrs.md)。|  
  
##  <a name="generating-data-feeds-from-a-report"></a><a name="GeneratingDataFeedsFromReport"></a> 从报表生成数据馈送  
 若要从某个报表生成数据馈送，请在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户中运行该报表，然后单击 Web 门户工具栏上的“生成数据馈送”  图标。 系统会提示您选择保存还是打开文件。 如果您选择 **“打开”** ，则将在应用程序中打开与 .atomsvc 文件扩展名关联的 Atom 服务文档。 如果您选择 **“保存”** ，则将该文档另存为 .atomsvc 文件。 默认情况下，该文件的名称即为报表的名称。 您可以将此名称更改为一个更有意义的名称。  
  
 将该 Atom 服务文档保存到计算机上。 稍后，您可以将其上载到报表服务器或另一个服务器上以供他人使用。 有关详细信息，请参阅[基于报表生成数据馈送](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md)和[基于单个报表生成数据馈送](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md)。  
  
##  <a name="troubleshooting-exported-reports"></a><a name="Troubleshooting"></a> 排除与导出的报表有关的问题  
 有时候，在您将报表导出到不同格式后，它们看起来不同或者没有按您预期的方式工作。 出现此问题的原因是某些规则和限制可能应用于呈现器。 在创建报表时，可以考虑这些来解决许多限制。 在报表中可能需要使用稍有不同的布局，仔细对齐报表中的项，将报表表尾限制为单行文本等。  
  
 如果报表包含带有阿拉伯数字的 Unicode 文本，或包含以阿拉伯数字表示的日期，当你将报表导出为以下任何一种格式或打印报表时，这些日期和数字将不会正确呈现。  
  
-   PDF  
  
-   Word  
  
-   Excel  
  
-   图像/TIFF  
  
 如果将报表导出为 HTML，日期和数字将正确呈现。  
  
 与特定呈现器有关的主题介绍报表项和数据区域是如何呈现的以及对于每种呈现器的限制和解决方法。  
  
-   [导出到 CSV 文件（报表生成器和 SSRS）](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md)  
  
-   [导出到 Microsoft Excel（报表生成器和 SSRS）](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md)  
  
-   [导出到 Microsoft Word（报表生成器和 SSRS）](../../reporting-services/report-builder/exporting-to-microsoft-word-report-builder-and-ssrs.md)  
  
-   [以 HTML 格式呈现（报表生成器和 SSRS）](../../reporting-services/report-builder/rendering-to-html-report-builder-and-ssrs.md)  
  
-   [导出到 PDF 文件（报表生成器和 SSRS）](../../reporting-services/report-builder/exporting-to-a-pdf-file-report-builder-and-ssrs.md)  
  
-   [导出到图像文件（报表生成器和 SSRS）](../../reporting-services/report-builder/exporting-to-an-image-file-report-builder-and-ssrs.md)  
  
-   [导出到 XML（报表生成器和 SSRS）](../../reporting-services/report-builder/exporting-to-xml-report-builder-and-ssrs.md)  
  
-   [基于报表生成数据馈送（报表生成器和 SSRS）](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md)  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 提供附加的功能来帮助您创建采用其他格式可以很好地工作的报表。 Tablix 数据区域（表、矩阵和列表）、组和矩形上的分页符使您可以更好地控制报表分页。 由分页符分隔的报表页可以具有不同的页名称和重置页码编号。 通过使用表达式，可以在报表运行时动态更新页名称和页码。 有关详细信息，请参阅 [Reporting Services 中的分页](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)。  
  
 此外，您还可以使用 RenderFormat 内置全局属性，针对不同呈现器有条件地应用不同的报表布局。 有关详细信息，请参阅[内置的全局和用户引用](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md)

##  <a name="other-ways-of-exporting-reports"></a><a name="OtherWaysExportingReports"></a> 导出报表的其他方式  
 导出报表是在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户或报表生成器中打开报表时可以根据需要执行的一项任务。 若要自动执行导出操作（例如，根据重复执行的计划，将报表以特定文件类型导出到共享文件夹中），请创建一个订阅，将报表传递到共享文件夹。 有关详细信息，请参阅 [File Share Delivery in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)。  
  
 在报告工具中预览或在浏览器应用程序（如 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户）中打开的报表始终首先以 HTML 格式呈现。 不能将其他呈现扩展插件指定为用于查看的默认扩展插件。 但是，可以创建一个订阅，使用该订阅按所需呈现格式生成报表，然后再传递到电子邮件收件箱或共享文件夹。 有关详细信息，请参阅 [创建和管理本机模式报表服务器的订阅](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md) 和 [创建、修改和删除数据驱动订阅](../../reporting-services/subscriptions/create-modify-and-delete-data-driven-subscriptions.md)。  
  
 您也可以通过将呈现扩展插件指定为 URL 参数的 URL 来访问报表，并直接将报表呈现为指定格式，而不先将报表呈现为 HTML 格式。 下面的示例将报表呈现为 Excel 格式：  
  
```  
https://<Report Server Name>/reportserver?/Sales/YearlySalesSummary&rs:Format=Excel&rs:Command=Render  
```  
  
 以下将从命名实例中呈现 PowerPoint 报表：  
  
```  
https://<Report Server Name/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  
  
 有关详细信息，请参阅 [Export a Report Using URL Access](../../reporting-services/export-a-report-using-url-access.md)。  

## <a name="next-steps"></a>后续步骤

[控制分页符、标题、列和行（报表生成器和 SSRS）](../../reporting-services/report-design/controlling-page-breaks-headings-columns-and-rows-report-builder-and-ssrs.md)   
[查找、查看和管理报表（报表生成器和 SSRS）](../../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md)   
[打印报表（报表生成器和 SSRS）](../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)   
[保存报表（报表生成器）](../../reporting-services/report-builder/saving-reports-report-builder.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
