---
title: 导出到 Microsoft Word（报表生成器）| Microsoft Docs
ms.date: 12/06/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
description: Word 呈现扩展插件将分页报表呈现为 Microsoft Word 格式 (.docx)。 格式为 Office Open XML。
ms.custom: seodec18
ms.topic: conceptual
ms.assetid: 0cd8ae26-4682-4473-8f15-af084951defd
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e1a9fd855d1b0bc8defd0e4355f490a2824fead6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015257"
---
# <a name="exporting-to-microsoft-word-report-builder-and-ssrs"></a>导出到 Microsoft Word（报表生成器和 SSRS）

  Word 呈现扩展插件将分页报表呈现为 Microsoft Word 格式 (.docx)。 格式为 Office Open XML。  
  
 此呈现器生成的文件的内容类型为 **application/vnd.openxmlformats-officedocument.wordprocessingml.document** ，并且文件扩展名为 .docx。  
  
 有关如何导出到 Word 的详细信息，请参阅[导出报表（报表生成器和 SSRS）](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md)。  
  
 将报表导出为 Word 文档之后，可以更改报表内容并设计文档样式的报表，例如邮件标签、采购订单或套用信函。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="report-items-in-word"></a><a name="ReportItemsWord"></a> Word 中的报表项  
 导出到 Word 的报表显示为表示表体的嵌套表。 Tablix 数据区域呈现为反映报表中数据区域结构的嵌套表。 每个文本框和矩形都呈现为表内的一个单元。 文本框的值显示在相应单元内。  
  
 图像、图表、数据条、迷你图、地图、指示器和仪表都呈现为表单元内的一个静态图像。 将呈现这些报表项的超链接和钻取链接。 不支持可在图表内单击的结构图和区域。  
  
 Word 中不呈现新闻报道样式的列报表。 不呈现表体及页背景图像和颜色。  
  
##  <a name="pagination"></a><a name="Pagination"></a> 分页  
 在 Word 中打开报表后，Word 将根据页大小再次对整个报表重新进行分页。 重新分页可能导致在您不想添加分页符的位置插入分页符，在某些情况下，可能使导出的报表在一行中有两个连续的分页符或者添加空页。 您可以调整页边距来尝试更改 Word 的分页情况。  
  
 此呈现器仅支持逻辑分页符。  
  
### <a name="page-sizing"></a>确定页大小  
 报表呈现时，将通过以下 RDL 属性设置 Word 页高和页宽：纸张大小（高和宽）、左右页边距以及顶部和底部页边距。  
  
### <a name="page-width"></a>页宽  
 Word 支持最大为 22 英寸的页宽。 如果报表的页宽超过 22 英寸，呈现器仍将呈现报表；不过，Word 不会在页面视图中或在读取版式视图时显示报表内容。 若要查看数据，请切换到普通视图或 Web 版式视图。 在这些视图中，Word 将减少空格数量，从而显示更多的报表内容。  
  
 呈现时，报表将根据需要增加宽度（最大为 22 英寸）以便显示内容。 报表的最小宽度基于“属性”窗格中的 RDL Width 属性。  
  
##  <a name="document-properties"></a><a name="DocumentProperties"></a> 文档属性  
 Word 呈现器会将以下元数据写入 DOCX 文件。  
  
|报表元素属性|说明|  
|-------------------------------|-----------------|  
|Report Title (report title)|标题|  
|Report.Author|作者|  
|Report.Description|注释|  
  
##  <a name="page-headers-and-footers"></a><a name="ReportHeadersFooters"></a> 页眉和页脚  
 页眉和页脚在 Word 中作为页眉和页脚区域呈现。 如果报表页码或者指示报表总页数的表达式出现在页眉或页脚，则它们将转化为 Word 域，以便在呈现的报表中显示准确的页码。 如果报表中设置了表头或表尾高度，则 Word 无法支持此设置。 在某些情况下，PrintOnFirstPage 属性可指定页眉和页脚中的文本是否打印在报表的第一页上。 如果呈现的报表具有多页并且每一页都仅包含单个部分，则可以将 PrintOnFirstPage 设置为 False，并且文本将不会显示在第一页上；否则，无论 PrintOnFirstPage 属性的值如何，都将打印文本。  
  
 在报表导出到 Word 时，Word 呈现器将尝试对页眉和页脚中的所有表达式进行分析。 许多形式的表达式将成功进行分析，并且预期值将出现所有报表页的页眉和页脚中。  
  
 但是，在某个页脚或页眉包含针对报表的不同页得出不同计算结果值的复杂表达式时，相同的值可能会显示在所有报表页上。 以下两个表达式中的页码在导出的报表中不递增。 页码将转换为所有报表页上的相同值。  
  
-   `="Page: " + Globals!PageNumber.ToString + " of " + Globals!TotalPages.ToString`  
  
-   `=Avg(Fields!YTDPurchase.Value, "Sales") & " Page Number " & Globals!PageNumber`  
  
 发生此情况的原因在于，Word 呈现器针对与 **PageNumber** 和 **TotalPages** 之类的分页相关的字段对报表进行分析，并且仅处理简单引用，而不处理对函数的调用。 在此情况下，该表达式将调用 **ToString** 函数。 下面两个表达式是等效的，并且当在报表生成器或报表设计器中预览报表时，或者在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户或 SharePoint 库中呈现已发布的报表时，这两个表达式都正确呈现。 但是，Word 呈现器仅对第二个表达式成功进行分析，并且呈现正确的页码。  
  
-   复杂表达式：  表达式为 `="Average Sales " & Avg(Fields!YTDPurchase.Value, "Sales") & " Page Number " & Globals!PageNumber`  
  
-   具有文本运行的表达式：  文本 Average Sales 和表达式 `=Avg(Fields!YTDPurchase.Value, "Sales)` 以及文本 Page Number 和表达式 `=Globals!PageNumber`    
  
 若要避免此问题，在表头和表尾中使用表达式时，请使用多文本运行，而非一个复杂表达式。 下面两个表达式是等效的。 第一个表达式是复杂表达式，第二个表达式使用文本运行。 Word 呈现器仅成功分析第二个表达式。  
  
##  <a name="interactivity"></a><a name="Interactivity"></a> 交互  
 Word 中支持一些交互元素。 下面是对一些特定行为的说明。  
  
### <a name="show-and-hide"></a>显示和隐藏  
 Word 呈现器根据报表呈现时的状态呈现报表项。 如果某个报表项处于隐藏状态，则该报表项将不呈现在 Word 文档中。 如果某个报表项处于显示状态，则该报表项将呈现在 Word 文档中。 Word 中不支持切换功能。  
  
### <a name="document-map"></a>文档结构图  
 如果报表中存在任何文档结构图标签，它们将呈现为各个报表项和组的 Word 目录 (TOC) 标签。 文档结构图标签用作 TOC 标签的标签文本。 目标链接位于对其设置标签的项的旁边。 如果 Word 文档中没有为您创建 TOC，则可以使用报表中呈现的文档结构图标签生成您自己的 TOC。  
  
### <a name="hyperlink-and-drillthrough-links"></a>超链接和钻取链接  
 文本框和图像报表项的超链接和钻取链接在 Word 文档中呈现为超链接。 单击超链接时，将打开默认 Web 浏览器并导航到相应的 URL。 单击钻取超链接时，将访问发起报表服务器。  
  
### <a name="interactive-sorting"></a>交互式排序  
 报表内容根据它们当前在报表数据区域内的排序方式呈现。 Word 不支持交互式排序。 报表呈现后，您可以应用 Word 内的表排序。  
  
### <a name="bookmarks"></a>书签  
 报表中的书签呈现为 Word 书签。 书签链接呈现为指向文档内的书签标签的超链接。 书签标签的长度不得超过 40 个字符。 唯一可以在书签标签中使用的特殊字符是下划线 (_)。 不支持的特殊字符将从书签标签名称中去除，如果名称长度超过 40 个字符，名称将被截断。 如果报表中有重复的书签名称，Word 将不呈现这些书签。  
  
##  <a name="word-style-rendering"></a><a name="WordStyleRendering"></a> Word 样式呈现  
 下面简要说明了 Word 中样式的呈现方式。  
  
### <a name="color-palette"></a>调色板  
 报表中呈现的颜色呈现在 Word 文档中。  
  
### <a name="border"></a>边框  
 报表项的边框（页边框除外）呈现为 Word 表单元边框。  
  
##  <a name="squiggly-lines-in-exported-reports"></a><a name="SquigglyLines"></a> 导出的报表中的波浪线  
 在 Word 中导出和查看时，报表数据或常量可能带有红色或绿色的波浪下划线。 红色波浪线标识拼写错误。 绿色波浪线标识语法错误。 当报表中包含与在 Word 中指定的编辑语言的检查（拼写和语法）不符的词语时，将出现此类波浪线。 例如，在报表以 Word 的西班牙语版本呈现时，英语报表列标题就很可能带有红色的波浪下划线。 发现拼写错误比发现语法错误更常见，因为报表通常仅包含简短文本，而非完整的句子或段落。  
  
 报表中存在波浪线意味着该报表有错误，但很可能这些错误不是真正的错误。 您可以通过更改报表的校对语言，删除这些波浪线。 若要更改校对语言，请选择报表的内容，然后为这些内容指定适当的语言。 您可以选择所有或部分内容。 在 Word 中，语言选项“设置校对语言”位于“评审”选项卡上的“语言”部分中    。在更新内容后，您需要重新保存文档。  
  
 根据您的 Office 程序的语言版本，您所选的语言的校对工具（例如，字典）将在程序中随附，或者在您购买的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office 语言包中提供。  
  
 以下主题介绍有关设置 Office 和 Word 选项的其他信息。  
  
-   在“Microsoft Office 语言首选项”或 Word 的“Word 选项”对话框中更改编辑语言   。 有关详细信息，请参阅 [在您的 Office 程序中启用其他语言](https://office.microsoft.com/word-help/enable-the-use-of-other-languages-in-your-office-programs-HA010354783.aspx?CTT=1)。  
  
-   添加 Office 语言包，然后更改编辑语言。 有关详细信息，请参阅 [在 Office 程序中启用其他语言](https://office.microsoft.com/word-help/enable-the-use-of-other-languages-in-your-office-programs-HA010354783.aspx?CTT=1) 和 [Office 2010 语言选项](https://office.microsoft.com/language/)。  
  
> [!NOTE]  
>  当在“Microsoft Office 语言首选项”  或 Word 的“Word 选项”  对话框中更改编辑语言时，更改将应用于所有 Office 程序。  
  
##  <a name="word-limitations"></a><a name="WordLimitations"></a> Word 限制  
 [!INCLUDE[ofprword](../../includes/ofprword-md.md)]存在以下限制：  
  
-   Word 表最多支持 63 列。 如果报表多于 63 列，而您尝试呈现它，Word 将拆分该表。 超出的列将紧邻表体中显示的 63 列放置。 因此，报表列不能按预期方式排列。  
  
-   Word 支持的最大页宽和页高均为 22 英寸。 如果内容所需的宽度超过 22 英寸，则页面视图中将无法显示某些数据。  
  
-   Word 忽略页眉和页脚的高度设置。  
  
-   导出报表后，Word 将再次对报表进行分页。 这可能导致向呈现的报表中添加其他分页符。  
  
-   Word 不会在第 2 页和其后的页面上重复标题行，尽管你将 Tablix（表、矩阵或列表）中静态标题行的 RepeatOnNewPage 属性设置为 **True**。 您可以在报表中定义显式分页符，以便强制标题行在新页上出现。 但是，因为 Word 将其自己的分页应用于导出到 Word 的呈现的报表，所以结果可能会不同并且标题行可能不会以可预测的方式重复。 静态标题行是包含列标题的行。  
  
-   文本框在包含不间断空格时会增长。  
  
-   将文本导出到 Word 时，某些字体具有字体效果的文本可能会在呈现的报表中生成意外标志符号或缺失某些标志符号。  
  
##  <a name="benefits-of-using-the-word-renderer"></a><a name="WordBenefits"></a> 使用 Word 呈现器的优点  
 除了使 [!INCLUDE[ofprword](../../includes/ofprword-md.md)] .docx 文件中的新增功能可用于导出的报表之外，导出的报表的 *.docx 文件往往更小。 通过使用 Word 呈现器导出的报表通常显著小于通过使用 Word 2003 呈现器导出的相同报表。  
  
## <a name="backward-compatibility-of-exported-reports"></a>导出的报表的向后兼容性  
 您可以选择 Word 兼容性模式并设置兼容性选项。 Word 呈现器在创建文档时会启用兼容性模式。 因此，重新保存禁用了兼容性模式的文档可能会影响文档的布局。  
  
 如果您禁用了兼容性模式，然后重新保存报表，该报表布局可能会意外更改。  
  
##  <a name="the-word-2003-renderer"></a><a name="AvailabilityWord"></a> Word 2003 呈现器  
  
> [!IMPORTANT]  
>  [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003 (.doc) 呈现扩展插件不推荐使用。 有关详细信息，请参阅 [SQL Server 2016 的 SQL Server Reporting Services 中不推荐使用的功能](~/reporting-services/deprecated-features-in-sql-server-reporting-services-ssrs.md)。  
  
 如果安装了针对 Word、Excel 和 PowerPoint 的 [!INCLUDE[ofprword](../../includes/ofprword-md.md)] Office 兼容包，Word 呈现器将与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 2003 兼容。 有关详细信息，请参阅 [用于 Word、Excel 和 PowerPoint 的 Microsoft Office 兼容包](https://www.microsoft.com/download/office.aspx)。
  
 与 [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003 兼容的以前版本的 Word 呈现扩展插件已重命名为 Word 2003。 默认情况下，只提供 Word 呈现扩展插件。 您必须更新 Reporting Services 配置文件，才能使用 Word 2003 呈现扩展插件。 Word 2003 呈现器生成的文件的内容类型为 **application/vnd.ms-word** ，并且文件的文件扩展名为 .doc。  
  
 在 SQL Server Reporting Services 中，默认的 Word 呈现器是呈现为 [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 格式 (.docx) 的版本。 这是 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户和 SharePoint 中“导出”  菜单列出的 **Word** 选项。 仅与 [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003 兼容的早期版本现在命名为 Word 2003，并且使用该名称在菜单上列出。 **Word 2003** 菜单选项默认不可见，但是，管理员可以通过更新 RSReportServer 配置文件使该选项可见。 若要使用 Word 2003 呈现器从 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 导出报表，请更新 RSReportDesigner 配置文件。 但是，使 Word 2003 呈现器可见并不会使其可用于所有方案中。 因为 RSReportServer 配置文件驻留在报表服务器上，所以从中导出报表的工具或产品必须连接到报表服务器才能读取配置文件。 如果在断开连接或本地模式中使用工具或产品，则使 Word 2003 呈现器可见没有任何影响。 **Word 2003** 菜单选项保持不可用。 如果在 RSReportDesigner 配置文件中使 Word 2003 呈现器可见，则 **Word 2003** 菜单选项在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 报表预览中始终可用。  
  
 **Word 2003** 菜单选项在以下方案中始终不可见：  
  
-   报表生成器处于断开连接模式下，而您在报表生成器中预览报表。  
  
-   报表查看器 Web 部件处于本地模式，而 SharePoint 场未与 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表服务器集成。 有关详细信息，请参阅[报表查看器中的本地模式报表和报表查看器中的连接模式报表（SharePoint 模式下的 Reporting Services）](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md)  
  
 如果 **Word 2003** 呈现器配置为可见，则 **Word** 和 **Word 2003** 菜单选项可用于以下方案中：  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户（在本机模式下安装 Reporting Services 时）。  
  
-   SharePoint 站点（当 Reporting Services 在 SharePoint 集成模式下安装时）。  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 和预览报表。  
  
-   连接到报表服务器的报表生成器。  
  
-   远程模式下的报表查看器 Web 部件。  
  
 下面的 XML 显示 RSReportServer 配置文件和 RSReportDesigner 配置文件中两个 Word 呈现扩展插件的元素：  
  
 `<Extension Name="WORDOPENXML" Type="Microsoft.ReportingServices.Rendering.WordRenderer.WordOpenXmlRenderer.WordOpenXmlDocumentRenderer,Microsoft.ReportingServices.WordRendering"/>`  
  
 `<Extension Name="WORD" Type="Microsoft.ReportingServices.Rendering.WordRenderer.WordDocumentRenderer,Microsoft.ReportingServices.WordRendering" Visible="false"/>`  
  
 WORDOPENXML 扩展插件定义 [!INCLUDE[ofprword](../../includes/ofprword-md.md)] .docx 文件的 Word 呈现器。 WORD 扩展插件定义 [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003 版本。 `Visible = "false"` 指示 Word 2003 呈现器处于隐藏状态。 有关详细信息，请参阅 [RsReportServer.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md) 和 [RSReportDesigner 配置文件](../../reporting-services/report-server/rsreportdesigner-configuration-file.md)。  
  
### <a name="differences-between-the-word-and-word-2003-renderers"></a>Word 呈现器和 Word 2003 呈现器之间的差异  
 使用 Word 呈现器或 Word 2003 呈现器呈现的报表往往从外观上无法区分。 但是，您可以在 Word 或 Word 2003 格式之间注意到这两者之间的细微差异。  
  
##  <a name="device-information-settings"></a><a name="DeviceInfo"></a> 设备信息设置  
 通过更改设备信息设置，可以更改此呈现器的某些默认设置，例如忽略超链接和钻取链接，或者展开可以切换的所有项（不管报表呈现时该项的原始状态如何）。 有关详细信息，请参阅 [Word Device Information Settings](../../reporting-services/word-device-information-settings.md)。  

## <a name="next-steps"></a>后续步骤

[Reporting Services 中的分页](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
[呈现行为](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
[不同报表呈现扩展插件的交互功能](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
[呈现报表项](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
[表、矩阵和列表](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
