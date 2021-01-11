---
title: 导出到 Microsoft Excel（报表生成器）| Microsoft Docs
description: 在报表生成器中，Excel 呈现扩展插件将分页报表呈现为 Office Open XML 格式，以用于 Microsoft Excel。
ms.date: 12/23/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
ms.assetid: 74f726fc-2167-47af-9093-1644e03ef01f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4e8b43d3e9e772b06961602bf2d8bd69aa51216e
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878850"
---
# <a name="exporting-to-microsoft-excel-report-builder-and-ssrs"></a>导出到 Microsoft Excel（报表生成器和 SSRS）
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Excel 呈现扩展插件将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 分页报表呈现为 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 格式 (.xlsx)。 使用 Excel 呈现扩展插件，Excel 中的列宽度更精确地反映了报表中的列宽度。  
  
 格式为 Office Open XML。 此呈现器生成的文件的内容类型为 **application/vnd.openxmlformats-officedocument.spreadsheetml.sheet** ，并且文件扩展名为 .xlsx。  
  
 您可以通过更改设备信息设置来更改此呈现器的某些默认设置。 有关详细信息，请参阅 [Excel Device Information Settings](../../reporting-services/excel-device-information-settings.md)。  
  
 有关如何导出到 Excel 的详细信息，请参阅[导出报表（报表生成器和 SSRS）](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md)。  
  
> [!IMPORTANT]  
>  定义 **String** 类型的参数时，系统将向用户显示一个可以使用任何值的文本框。 如果报表参数未与查询参数关联，并且参数值包含在报表中，则报表用户可以在参数值中键入表达式语法、脚本或 URL，并将报表呈现为 Excel 格式。 如果其他用户查看报表并单击呈现的参数内容，则用户可能会无意中执行恶意脚本或链接。  
>   
>  若要降低无意中运行恶意脚本的风险，请仅从可信来源打开呈现的报表。 有关保护报表的详细信息，请参阅 [保护报表和资源](../../reporting-services/security/secure-reports-and-resources.md)。  
  
##  <a name="excel-limitations"></a><a name="ExcelLimitations"></a> Excel 限制  
 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 对于导出的报表存在一些限制。 最重要的限制包括：  
  
-   最大列宽限制为 255 个字符（或 1726.5 磅）。 呈现器不会验证列宽是否小于此限制。  
  
-   单元格中的最大字符数限制为 32,767 个。 如果超出此限制，呈现器会显示错误消息。  
  
-   最大行高为 409 磅。 如果该行内容导致行高超过 409 磅，则 Excel 单元格将显示最多 409 磅的部分文本。 其余的单元格内容仍在单元格内（最多可达 Excel 最大字符数 32,767）。

-  由于最大行高为 409 磅，如果报表中单元格的定义高度大于 409 磅，则 Excel 会将单元格内容拆分为多行。
  
-   在 Excel 中并没有限定工作表的最大数量，但是诸如内存和磁盘空间等外部因素可能限制工作表的最大数量。  
  
-   在大纲中，Excel 最多允许 7 层嵌套级别。  
  
-   如果控制另一个报表项是否切换的报表项不是位于要切换的报表项的上一或下一行/列，则还会禁用大纲功能。  
  
 有关 Excel 限制的详细信息，请参阅 [Excel 规范和限制](https://support.microsoft.com/en-us/office/excel-specifications-and-limits-1672b34d-7043-467e-8e27-269d656771c3)。  
  
### <a name="sizes-of-excel-2003-xls-files"></a>Excel 2003 (.xls) 文件的大小  
  
> [!IMPORTANT]  
>  [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2003 呈现扩展插件不推荐使用。 有关详细信息，请参阅 [SQL Server 2016 的 SQL Server Reporting Services 中不推荐使用的功能](../../reporting-services/deprecated-features-in-sql-server-reporting-services-ssrs.md)。  
  
 在报表第一次导出并保存到 Excel 2003 中时，它们并不会从 Excel 自动应用到其 *.xls 工作簿文件的文件优化中获益。 如果文件较大，可能会导致电子邮件订阅和附件出现问题。 若要减小导出的报表的 \*.xls 文件的大小，请打开 \*.xls 文件，然后重新保存工作簿。 重新保存工作簿通常可以将其文件大小降低 40% 到 50%。  
  
> [!NOTE]  
>  在 Excel 2003 中，Excel 工作表的单元格中大约显示 1000 个字符，但是可以在公式栏中编辑允许的最大字符数。 此限制不适用于当前 (.xlsx) Excel 文件。  
  
### <a name="text-boxes-and-text"></a>文本框和文本  
 对于文本框和文本有以下限制：  
  
-   内容为表达式文本框值不会转换为 Excel 公式。 报表处理过程中会计算每个文本框的值。 计算表达式会作为每个 Excel 单元格的内容导出。  
  
-   文本框呈现在 Excel 的一个单元格内。 Excel 单元格中的单个文本仅支持字号、字体、效果以及字形格式。  
  
-   Excel 中不支持文本效果“上横线”。  
  
-   Excel 在单元格的左右两侧将默认添加大约 3.75 磅的填充量。 如果文本框的填充设置小于 3.75 磅，并且其宽度仅足以容纳文本，则在 Excel 中文本可能换行。  
  
    > [!NOTE]  
    >  若要解决此问题，请增加报表中文本框的宽度。  
  
### <a name="images"></a>映像  
 对于图像有以下限制：  
  
-   由于 Excel 不支持单个单元格的背景图像，因此将忽略报表项的背景图像。  
  
-   Excel 呈现扩展插件仅支持表体的背景图像。 如果报表中显示有表体背景图像，则该图像呈现为工作表的背景图像。  
  
### <a name="rectangles"></a>矩形  
 对于矩形有以下限制。  
  
-   报表表尾中的矩形不导出到 Excel 中。 但是，表体或 tablix 单元等中的矩形将呈现为某个范围的 Excel 单元。  
  
### <a name="report-headers-and-footers"></a>报表表头和表尾  
 对于报表表头和表尾有以下限制：  
  
-   Excel 表头和表尾最多支持 256 个字符，其中包括标记。 呈现扩展插件将在 256 个字符处截断字符串。  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 不支持报表表头和表尾上的边距。 在导出到 Excel 时，这些边距值将设置为零，并且包含多行数据的任何表头或表尾可能不打印多行，打印的行数取决于打印机设置。  
  
-   在导出到 Excel 时，表头或表尾中的文本框保持其格式设置，但不保持其对齐方式。 导致出现此问题的原因在于，在报表向 Excel 呈现时对前导及尾随空格进行了修整。  
  
### <a name="merging-cells"></a>合并单元  
 对于合并单元有以下限制：  
  
-   如果合并单元格，则自动换行功能可能不正常。 如果行中存在任一合并单元格，并且所呈现的文本框具有 AutoSize 属性，则自动调整大小功能将会不正常。  
  
 Excel 呈现器主要是一种布局呈现器。 其目的是为了在 Excel 工作表中尽可能接近地复制呈现的报表的布局，并且最终生成的单元可以合并到工作表中以便保留报表布局。 合并的单元可能导致问题，因为 Excel 中的排序功能要求以一种非常具体的方式对单元进行合并，这样才能正确排序。 例如，Excel 要求合并的单元的范围具有相同的大小以便进行排序。  
  
 如果可以对导出到 Excel 工作表的报表进行排序十分重要，则以下内容可帮助您减少 Excel 工作表中合并的单元的数目，这是影响 Excel 排序功能的常见原因。  
  
-   无法左对齐和右对齐项是合并的单元的最常见问题。 请确保所有报表项的左边缘和右边缘都彼此对齐。 使项对齐并且具有相同宽度将解决大多数此类情况中的问题。  
  
-   尽管您正确将所有项对齐，但在很少的一些情况下，还会发现某些列继续合并。 这可能是在呈现 Excel 工作表时由内部单位转换和舍入导致的。 在报表定义语言 (RDL) 中，您可以通过不同的度量单位（例如英寸、像素、厘米和磅）指定位置和大小。 Excel 在内部使用磅。 为了尽量减少转换以及在将英寸和厘米转换为磅时进行舍入而导致的不精确性，请考虑全部用磅指定所有度量单位，以便获得最直接的结果。 一英寸等于 72 磅。  
  
### <a name="report-row-groups-and-column-groups"></a>报表行组和列组  
 当导出到 Excel 时，包含行组或列组的报表包含空单元。 假设一个报表，在上下班路程上对行进行分组。 每个上下班路程可以包含多个客户。 下图显示了此报表。  
  
 ![Reporting Services Web 门户中的报表](../../reporting-services/report-builder/media/ssrb-excelexportssrs.png "Reporting Services Web 门户中的报表")  
  
 当将报表导出到 Excel 时，上下班路程只出现在“上下班路程”列的一个单元中。 根据文本在报表中的对齐方式（顶部、中、底部），该值将位于第一个单元、中间单元或最后一个单元中。 其他单元为空。 包含客户名称的“名称”列没有空单元。 下图显示了报表导出到 Excel 之后的情况。 添加红色的单元边界是为了进行强调。 灰色框是空单元格。 （红色线条和灰色框都不是报表导出的一部分。）  
  
 ![导出到 Excel 的报表（带线条）](../../reporting-services/report-builder/media/ssrb-exportedexcellines.png "导出到 Excel 的报表（带线条）")  
  
 这意味着，对于具有行组或列组的报表而言，在将其导出到 Excel 之后，必须先进行修改，然后才能在透视表中显示导出的数据。 必须在缺少组值的单元中添加组值，以使工作表成为在所有单元中均具有值的平面表。 下图显示了更新的工作表。  
  
 ![导出到 Excel 的报表（已平展）](../../reporting-services/report-builder/media/ssrb-excelexportnomatrix.png "导出到 Excel 的报表（已平展）")  
  
 因此如果创建某个报表的目的是为了将此报表导出到 Excel 以便对报表数据进行进一步分析，请考虑不要对报表中的行或列进行分组。  
  
## <a name="excel-renderer"></a>Excel 呈现器  
  
### <a name="current-xlsx-excel-file-renderer"></a>当前 (.xlsx) Excel 文件呈现器  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]中，默认 Excel 呈现器是与当前 (.xlsx) [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 文件兼容的版本。 这是 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户和 SharePoint 列表中 “导出”  菜单中的“Excel”  选项。  
  
 当你使用默认 Excel 呈现器而不是更早版本的 Excel 2003 (.xls) 呈现器时，请安装针对 Word、Excel 和 PowerPoint 的 Microsoft Office 兼容包，以便更早版本的 Excel 可打开导出的文件。  
  
### <a name="excel-2003-xls-renderer"></a>Excel 2003 (.xls) 呈现器  
  
> [!IMPORTANT]  
>  [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2003 呈现扩展插件不推荐使用。 有关详细信息，请参阅 [SQL Server 2016 的 SQL Server Reporting Services 中不推荐使用的功能](../../reporting-services/deprecated-features-in-sql-server-reporting-services-ssrs.md)。  
  
 与 Excel 2003 兼容的更早版本的 Excel 呈现器现在名为 Excel 2003，并在使用该名称的菜单中列出。 此呈现器生成的文件的内容类型为 **application/vnd.ms-excel** ，并且文件的文件扩展名为 .xlsx。  
  
 默认情况下，“Excel 2003”  菜单选项是不可见的。 管理员可以通过更新 RSReportServer 配置文件使该选项在特定情况下可见。 若要使用 Excel 2003 呈现器从 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 导出报表，请更新 RSReportDesigner 配置文件。  
  
 **Excel 2003** 菜单选项扩展插件在以下方案中始终不可见：  
  
-   报表生成器处于断开连接模式下，而您在报表生成器中预览报表。 因为 RSReportServer 配置文件驻留在报表服务器上，所以从中导出报表的工具或产品必须连接到报表服务器才能读取配置文件。  
  
-   报表查看器 Web 部件处于本地模式，而 SharePoint 场未与 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表服务器集成。 有关详细信息，请参阅[报表查看器中的本地模式报表和报表查看器中的连接模式报表（SharePoint 模式下的 Reporting Services）](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md)  
  
 如果将 **Excel 2003** 菜单选项呈现器配置为可见，则 Excel 和 Excel 2003 选项可用于以下方案中：  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户本机模式。  
  
-   SharePoint 站点（当 Reporting Services 在 SharePoint 集成模式下安装时）。  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 和预览报表。  
  
-   连接到报表服务器的报表生成器。  
  
-   远程模式下的报表查看器 Web 部件。  
  
 下面的 XML 显示 RSReportServer 和 RSReportDesigner 配置文件中的两个 Excel 呈现扩展插件的元素：  
  
 `<Extension Name="EXCELOPENXML" Type="Microsoft.ReportingServices.Rendering.ExcelOpenXmlRenderer.ExcelOpenXmlRenderer,Microsoft.ReportingServices.ExcelRendering"/>`  
  
 `<Extension Name="EXCEL" Type="Microsoft.ReportingServices.Rendering.ExcelRenderer.ExcelRenderer,Microsoft.ReportingServices.ExcelRendering" Visible="false"/>`  
  
 EXCELOPENXML 扩展插件定义当前 (.xlsx) Excel 文件的 Excel 呈现器。 EXCEL 扩展插件定义 Excel 2003 版本。 `Visible = "false"` 指示 Excel 2003 呈现器处于隐藏状态。 有关详细信息，请参阅 [RsReportServer.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md) 和 [RSReportDesigner 配置文件](../../reporting-services/report-server/rsreportdesigner-configuration-file.md)。  
  
### <a name="differences-between-the-current-xlsx-excel-and-excel-2003-renderers"></a>当前 (.xlsx) Excel 呈现器和 Excel 2003 呈现器之间的差异  
 通过使用当前 (.xlsx) Excel 呈现器或 Excel 2003 呈现器呈现的报表通常是完全相同的，只在极少数的情况下，你会注意到这两种格式之间的差异。 下表对 Excel 呈现器和 Excel 2003 呈现器进行了比较。  
  
|properties|Excel 2003|当前 Excel|  
|--------------|----------------|-------------------|  
|每个工作表的最大列数|256|16,384|  
|每个工作表的最大行数|65,536|1,048,576|  
|工作表中允许的颜色数|56（调色板）<br /><br /> 如果报表中使用的颜色超过 56 种，呈现扩展插件就会将所需颜色与自定义调色板中已有的 56 种颜色之一匹配。|大约 1600 万种（24 位颜色）|  
|ZIP 压缩文件|无|ZIP 压缩|  
|默认字体系列|Arial|Calibri|  
|默认字号|10 磅|11 磅|  
|默认行高|12.75 磅|15 磅|  
  
 因为报表显式设置行高，所以，默认行高将只影响在导出到 Excel 后自动调整大小的行。  
  
##  <a name="report-items-in-excel"></a><a name="ReportItemsExcel"></a> Excel 格式的报表项  
 矩形、子报表、表体和数据区域呈现为一组 Excel 单元格。 文本框、图像、图表、数据条、迷你图、地图、仪表和指示器必须呈现在一个 Excel 单元格内，这些元素可能会根据报表其余部分的布局进行合并。  
  
 图像、图表、迷你图、数据条、地图、仪表、指示器和线条虽然位于一个 Excel 单元格内，但是它们却位于单元网格的顶部。 线条呈现为单元格边框。  
  
 图表、迷你图、数据条、地图、仪表和指示器导出为图片。 它们所描绘的数据（例如图表的值和成员标签）不随它们一起导出，并且在数据包括在报表内数据区域中的列或行中之前不可用于 Excel 工作簿中。  
  
 如果您想要使用图表、迷你图、数据条、地图、仪表和指示器数据，请将报表导出到 .csv 文件中或者从报表生成与 Atom 兼容的数据馈送。 有关详细信息，请参阅[导出到 CSV 文件（报表生成器和 SSRS）](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md)和[从报表生成数据馈送（报表生成器和 SSRS）](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md)。  
  
## <a name="page-sizing"></a>确定页大小  
 Excel 呈现扩展插件采用页面高度和宽度设置来确定要在 Excel 工作表中定义哪些纸张设置。 Excel 会试图将 PageHeight 和 PageWidth 属性设置与最常用的一种纸张大小相匹配。  
  
 如果未找到匹配项，Excel 将使用打印机的默认页面大小。 如果页面宽度小于页面高度，则将页面方向设置为“纵向”；否则，将方向设置为“横向”。  
  
##  <a name="worksheet-tab-names"></a><a name="WorksheetTabNames"></a> 工作表选项卡名称  
 将报表导出到 Excel 时，将分页符创建的报表页导出到不同的工作表。 如果您提供了报表的初始页名称，默认情况下 Excel 工作簿的每个工作表将具有此名称。 此名称显示在工作表选项卡上。不过，由于工作簿的每个工作表必须具有唯一的名称，因此从第二个工作表开始会将一个整数追加到每个工作表的初始页名称，从 1 开始追加，每增加一个工作表，该整数递增 1。 例如，如果初始页名称为 **Sales Report by Fiscal Year**，则第二个工作表的名称为 **Sales Report by Fiscal Year1**，第三个工作表的名称为 **Sales Report by Fiscal Year2**，依次类推。  
  
 如果分页符创建的所有报表页都提供新页名称，每个工作表将具有相关的页名称。 但是，这些页名称可能不唯一。 如果页名称不唯一，按与初始页名称相同的方式来命名工作表。 例如，如果两个组的页名称是 **Sales for NW**，则一个工作表选项卡将具有名称 **Sales for NW**，另一个工作表选项卡具有名称 **Sales for NW1**。  
  
 如果报表既不提供初始页名称也不提供与分页符有关的页名称，工作表选项卡将具有默认名称 **Sheet1**、 **Sheet2**，依次类推。  
  
 Reporting Services 提供要对报表、数据区域、组和矩形设置的属性，帮助您创建可以所需方式导出到 Excel 的报表。 有关详细信息，请参阅 [Reporting Services 中的分页（报表生成器和 SSRS）](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)。  
  
##  <a name="document-properties"></a><a name="DocumentProperties"></a> 文档属性  
 Excel 呈现器会将以下元数据写入 Excel 文件。  
  
|报表元素属性|说明|  
|-------------------------------|-----------------|  
|创建|报表执行的日期和时间，格式为 ISO 日期/时间值。|  
|作者|Report.Author|  
|说明|Report.Description|  
|LastSaved|报表执行的日期和时间，格式为 ISO 日期/时间值。|  
  
##  <a name="page-headers-and-footers"></a><a name="PageHeadersFooters"></a> 页眉和页脚  
 根据设备信息 SimplePageHeaders 设置，页眉可以按两种方式呈现：页眉可以呈现在每个工作表单元网格的顶部，或者位于实际 Excel 工作表表头部分。 默认情况下，页眉呈现至 Excel 工作表的单元网格上。  
  
 页脚始终呈现至实际 Excel 工作表表尾部分，无论 SimplePageHeaders 设置的值如何。  
  
 Excel 表头和表尾部分最多支持 256 个字符，其中包括标记。 如果超出此限制，则 Excel 呈现器将删除自表头和/或表尾字符串末尾开始的标记字符，以便减少总字符数。 如果在删除所有标记字符后，字符串长度仍然超出最大限制，则系统会自右侧开始截断字符串。  
  
### <a name="simplepageheader-settings"></a>SimplePageHeader 设置  
 默认情况下，设备信息 SimplePageHeaders 设置将设为 **False**；因此，页眉在 Excel 工作表图面上呈现为报表行。 而包含页眉的工作表行将变成锁定的行。 您可以冻结或解冻 Excel 中的窗格。 如果选中 **“打印标题”** 选项，则这些页眉将自动设置为在每个工作表页上打印。  
  
 如果在 Excel 的“页面布局”选项卡上选中 **“打印标题”** 选项，则页眉会在工作簿中每个工作表的顶部重复（文档结构图封面表除外）。 如果在“报表表头属性”或“报表表尾属性”对话框中未选中 **“在首页上打印”** 或 **“在最后一页上打印”** 选项，则表头不会分别添加到第一页或最后一页。  
  
 页脚将呈现在 Excel 表尾部分。  
  
 由于 Excel 的局限性，文本框是唯一能呈现在 Excel 表头/表尾部分的报表项类型。  
  
##  <a name="interactivity"></a><a name="Interactivity"></a> 交互  
 Excel 支持一些交互元素。 下面是对一些特定行为的说明。  
  
### <a name="show-and-hide"></a>显示和隐藏  
 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 对导出报表项时如何控制隐藏和显示报表项存在局限性。 包含可切换的报表项的组、行和列将呈现为 Excel 大纲。 Excel 可创建在整个行或列范围内扩展和折叠行与列的大纲，这样会导致折叠那些本来不应该折叠的报表项。 此外，Excel 的大纲符号会与重叠的大纲混合在一起。 为解决这些问题，在使用 Excel 呈现扩展插件时将应用以下大纲规则：  
  
-   位于左上角可切换的报表项在 Excel 中仍然可以进行切换。 与位于左上角可切换的报表项共享垂直或水平空间的可切换报表项在 Excel 中不能进行切换。  
  
-   若要确定数据区域将按行还是按列折叠，需确定控制切换的报表项的位置以及要切换的报表项的位置。 如果控制切换的报表项出现在要切换的报表项之前，则项可以按行进行折叠。 否则，项可以按列进行折叠。 如果控制切换的报表项均匀地出现在要切换的区域的旁边和上方，则在呈现项时行可以按行折叠。  
  
-   若要确定小计在所呈现报表中的位置，呈现扩展插件将检查动态成员的第一个实例。 如果对等静态成员紧靠在其上方，则假定该动态成员为小计。 大纲将设置为指示这是摘要数据。 如果动态成员没有静态同级，则该实例的第一个实例即为小计。  
  
-   由于 Excel 的局限性，大纲的嵌套级别最多不能超过 7 级。  
  
### <a name="document-map"></a>文档结构图  
 如果报表中存在任何文档结构图标签，则将呈现文档结构图。 文档结构图呈现为 Excel 的封面工作表，插在工作簿的第一个选项卡位置。 该工作表称为“文档结构图”  。  
  
 文档结构图中显示的文本由报表项或组的 DocumentMapLabel 属性决定。 文档结构图标签按它们在报表中出现的顺序列出，自第一列的第一行开始。 每个文档结构图标签单元的缩进深度级别数与报表中所出现的一样。 每个缩进级别是通过在后续列中放置一个标签来表示的。 Excel 最多可支持 256 个大纲嵌套级别。  
  
 文档结构图大纲呈现为可折叠的 Excel 大纲。 大纲结构与文档结构图的嵌套结构相匹配。 大纲的扩展和折叠状态从第二级开始。  
  
 结构图的根节点是报表名称 \<*reportname*>.rdl，它是不可交互的。 文档结构图链接字体是宋体 (10pt)。  
  
### <a name="drillthrough-links"></a>钻取链接  
 文本框中出现的钻取链接在用于呈现文本的单元格中呈现为 Excel 超链接。 图像和图表的钻取链接在呈现的图像上呈现为 Excel 超链接。 单击钻取链接时，将打开客户端的默认浏览器并导航到目标的 HTML 视图。  
  
### <a name="hyperlinks"></a>超链接  
 文本框中出现的超链接在用于呈现文本的单元格中呈现为 Excel 超链接。 图像和图表的超链接在呈现的图像上呈现为 Excel 超链接。 单击超链接时，将打开客户端的默认浏览器并导航到目标 URL。  
  
### <a name="interactive-sorting"></a>交互式排序  
 Excel 不支持交互式排序。  
  
### <a name="bookmarks"></a>书签  
 文本框中的书签链接在用于呈现文本的单元格中呈现为 Excel 超链接。 图像和图表的书签链接在呈现的图像上呈现为 Excel 超链接。 单击书签后，将转至用于呈现标有书签的报表项的 Excel 单元格。  
  
##  <a name="changing-reports-at-run-time"></a><a name="ConditionalFormat"></a> 在运行时更改报表  
 如果某个报表必须以多种格式呈现，并且不可能创建以您所需的所有格式呈现的报表布局，则可以考虑使用 RenderFormat 内置全局属性中的值，在运行时有条件地更改报表外观。 这样，您可以根据用于在每种格式中获取最佳结果的呈现器，隐藏或显示报表项。 有关详细信息，请参阅[内置的全局和用户引用（报表生成器和 SSRS）](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md)。  
  
## <a name="see-also"></a>另请参阅  
 [Reporting Services 中的分页（报表生成器和 SSRS）](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
 [呈现行为（报表生成器和 SSRS）](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
 [不同报表呈现扩展插件的交互功能（报表生成器和 SSRS）](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
 [呈现报表项（报表生成器和 SSRS）](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
 [表、矩阵和列表（报表生成器和 SSRS）](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
