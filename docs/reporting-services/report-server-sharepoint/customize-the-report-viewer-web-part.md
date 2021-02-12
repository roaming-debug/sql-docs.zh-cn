---
title: 自定义报表查看器 Web 部件 | Microsoft Docs
description: 可以使用报表查看器 Web 部件来查看针对 SharePoint 集成配置的 SQL Server Reporting Services 服务器上运行的报表。
ms.date: 11/26/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2ff03267639a2ea62000c077330b2db547128be7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074758"
---
# <a name="customize-the-report-viewer-web-part"></a>自定义报表查看器 Web 部件

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

可以使用报表查看器 Web 部件来查看针对 SharePoint 集成配置的报表服务器上运行的报表。 可显示的报表包括报表定义 (.rdl) 文件和报表生成器报表。 报表将在新页面的报表查看器 Web 部件中自动打开。 如果要在该页面上始终显示特定报表，还可以将报表查看器 Web 部件添加到现有网页或网站。

> [!NOTE]
> 尽管名称相同，但通过 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 加载项安装的报表查看器 Web 部件与 RSWebParts.cab 文件中包含的报表查看器 Web 部件并不相同。 本主题中的说明专门针对通过 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 加载项安装的报表查看器 Web 部件。

 可以按如下方式自定义报表查看器 Web 部件：  
  
-   通过设置属性更改 Web 部件的外观。  
  
-   选择报表工具栏上可用的交互式报表功能。  
  
-   指定可用的视图区域。 报表查看器 Web 部件包含报表视图区域、参数区域和凭据区域。  
  
 不能将报表查看器 Web 部件扩展为支持不同文件类型，并且不能用自定义工具栏替换报表工具栏或向现有工具栏添加新功能。 如果需要自定义标准功能，应创建自定义 Web 部件。  

## <a name="setting-web-part-properties"></a>设置 Web 部件属性

 Web 部件具有用于配置特定功能的自定义属性。 Web 部件还具有针对所有 Web 部件的标准通用属性。  
  
### <a name="change-default-properties"></a>更改默认属性

 报表查看器 Web 部件的默认属性最适合按照需要从库或文件夹中打开报表。 默认情况下，所有可用控件将显示在工具栏上。 设置高度和宽度，以使用网页上的所有可用空间。 如果希望修改默认属性，可以通过“网站设置”自定义 Web 部件。  
  
1.  在 **“网站操作”** 菜单上，单击 **“网站设置”** 。  
  
2.  在“库”下，单击“Web 部件”。  
  
3.  单击 **ReportViewer.dwp**。  
  
4.  打开工具窗格并设置要使用的属性。  
  
### <a name="customize-an-embedded-report-viewer-in-a-web-page"></a>自定义网页中的嵌入式报表查看器

 可以设置属性以使报表查看器恰好适合网页大小。 报表查看器可以使用与所在页面相同的样式与颜色。 您可以隐藏全部或部分工具栏、文档结构图和参数区域以使所分配空间内的报表视图区域最大化。 报表将始终使用创建时为其定义的样式。 报表发布到 SharePoint 库后，则不能对其外观进行自定义。  
  
 如果要将报表查看器 Web 部件嵌入网页，应将“Report URL”属性设置为特定报表。 如果未设置，报表查看器将显示有关链接至报表的说明。 您不能自定义或删除这些说明。  
  
### <a name="custom-properties-of-the-report-viewer-web-part"></a>报表查看器 Web 部件的自定义属性

 设置自定义属性时请注意：某些属性仅在将报表查看器 Web 部件嵌入页面时才会用到。 例如，其中包括“标题”、“高度”、“宽度”、“部件版式类型”和“区域”等。 无论报表查看器是显示在页面内还是以整页模式打开报表，均可以使用其他属性（例如“工具栏”设置和“参数”设置）。  
  
 下面列出了报表查看器 Web 部件的自定义属性。  
  
|属性|描述|  
|--------------|-----------------|  
|报表|SharePoint 站点或同一 Web 应用程序或场内站点上的报表的完全限定路径。 为了在设置其他属性时能够获得最佳结果，请在指定报表 URL 后单击“应用”。|  
|超链接目标|标准 HTML，用于指定显示当前文档内链接内容的目标框架。 对于包含指向外部网站的超链接的报表，可以指定是用目标文档替换当前窗口中的现有报表还是在新浏览器窗口中打开目标文档。 有效值包括“_Top”、“_Blank”和“_Self”。 “_Top”使用当前窗口，“_Blank”在新浏览器窗口中加载文档，而“_Self”在当前框架内打开文档。 尽管“_Parent”是 HTML 中“目标”属性的有效值，但请勿对嵌入页面的报表查看器 Web 部件使用此值。|  
|自动生成 Web 部件标题|生成的标题，包含报表查看器 Web 部件名称和报表名称，中间用短划线分隔。 如果报表没有标题，则使用报表的文件名。 将 Web 部件添加到页面时，即可看到此标题。 如果选中此复选框，则每次刷新页面时都会生成标题。|  
|自动生成 Web 部件标题详细信息链接|生成的显示在 Web 部件之上的超链接。 单击此链接可以在新页面中以整页模式打开报表。|  
|显示报表生成器菜单项。|显示或隐藏用于打开报表生成器的 **“操作”** 菜单选项。|  
|显示订阅菜单项|显示或隐藏用于为报表创建订阅的 **“操作”** 菜单选项。|  
|显示打印菜单项|显示或隐藏用于打印报表的 **“操作”** 菜单选项。|  
|显示导出菜单项|显示或隐藏用于导出报表的 **“操作”** 菜单选项。|  
|显示刷新按钮|显示或隐藏工具栏上的刷新按钮。|  
|显示页导航控件|显示或隐藏工具栏上的报表导航按钮。 此选项更改所有导航控件的可见性。|  
|显示后退按钮|显示或隐藏工具栏上的后退按钮。|  
|显示查找按钮|显示或隐藏工具栏上的查找控件。 查找控件让用户可以在所呈现的报表中搜索文本。 此选项更改所有查找控件的可见性。|  
|显示缩放控件|显示或隐藏工具栏上的缩放控件。|  
|显示 ATOM 馈送按钮|显示或隐藏工具栏上的 ATOM 馈送按钮。<br /><br /> ![htmlviewer_datafeed](../../reporting-services/media/htmlviewer-datafeed.gif "htmlviewer_datafeed")|  
|工具栏位置|确定工具栏在报表查看器中的位置。 有效值包括 **Top** 和 **Bottom**。|  
|提示区域|有效值包括 **Displayed**、 **Collapsed** 和 **Hidden**。 **Displayed** 为包括参数化值的报表显示参数区域，这需要用户在运行报表之前进行输入。 如果已指定所有报表参数并且不希望用户看到参数区域，请使用 **Hidden** 。|  
|参数区域宽度|可以选择度量单位和值。 默认值为 200 像素。 对于此属性，唯一的要求是其值大于零。|  
|文档结构图|报表中定义的报表导航控件，用于提供对报表特定部分的一键式访问。 可在 HTML 报表中使用此控件。 文档结构图显示在报表视图区域旁的可折叠区域中。 有效值包括 **Displayed**、 **Collapsed** 和 **Hidden**。 如果已为报表定义文档结构图，并且未在 Web 部件属性中将其选为隐藏或折叠，则默认情况下将展开此区域。 如果文档结构图处于折叠状态，可以单击箭头将其展开。|  
|文档结构图区域宽度|可以选择度量单位和值。 默认值为 200 像素。 对于此属性，唯一的要求是其值大于零。|  
|加载参数|检索报表的参数属性。 并非所有报表都具有参数。 如果报表不具有参数，则不返回任何值。 如果为刚上载的报表设置属性，可能会收到错误信息，指明数据源连接已删除。 如果遇到这种情况，请重置连接并在指定连接后完成参数属性的设置。 有关如何设置连接的详细信息，请参阅[创建和管理共享数据源（SharePoint 集成模式下的 Reporting Services）](/previous-versions/sql/)。<br /><br /> 为了达到最佳效果，请在单击“加载参数”之前单击 **“应用”** 。<br /><br /> 加载参数属性之后，可以设置它们，方法与在报表的参数属性页上进行设置相同。 有关如何设置参数的详细信息，请参阅[在已发布报表上设置参数（SharePoint 集成模式下的 Reporting Services）](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md)。|  

## <a name="customizing-the-toolbar"></a>自定义工具栏

 工具栏显示在标题下方并在报表的顶部扩展。 工具栏上具有 **“操作”** 菜单，并可按页浏览分页报表、刷新和缩放。 它还包括适用于具有文档结构图的报表的文档结构图控件。 **“操作”** 菜单中包括用于导出报表、搜索报表中的文本或数字、打印报表以及在报表生成器中打开报表的命令。  
  
 您不能向  **“操作”** 菜单添加新命令，但可以通过更改对用户可见的选项来对其进行自定义。 若要更改工具栏按钮和控件的可见性，需要更改 Web 部件的“工具栏项可见性”部分中的选项。 此外，还可以通过使 **“打印”** 命令或特定导出格式在报表服务器上不可用，从而将这些功能删除。 页导航控件可用于具有分页符的报表；不具分页符的报表为长度可变的单页报表。 “刷新”功能可使用报表的当前参数重新处理报表。 若要在一行中显示所有控件，请将 Web 部件的总体宽度设为至少 400 像素。  

## <a name="customizing-the-viewing-area"></a>自定义视图区域

 视图区域用于显示报表。 使用参数区域和凭据区域时，它们还将与报表视图区域共存。 需要凭据时，凭据区域将显示在空报表视图区域的旁边。 在用户提供凭据并运行报表后，凭据区域将关闭。 若要自定义提示用户设置凭据的文本，请修改数据源连接属性。 有关详细信息，请参阅[创建和管理共享数据源（SharePoint 集成模式下的 Reporting Services）](/previous-versions/sql/)。  
  
 参数区域提供用于在运行报表之前输入值的字段。 仅在报表定义包括参数时才使用此区域。 显示参数或凭据区域时，将调整报表视图以充分利用 Web 部件的其余部分。 可以通过设置 Web 部件的属性来自定义参数区域的宽度。 也可以定义显示在页面上各个参数旁的标签。 有关如何修改参数标签的详细信息，请参阅[在已发布报表上设置参数（SharePoint 集成模式下的 Reporting Services）](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md)。  
  
## <a name="see-also"></a>另请参阅

 [SharePoint 站点上的报表查看器 Web 部件](./report-viewer-web-part-sharepoint-site.md)   
 [将报表查看器 Web 部件添加到网页](../../reporting-services/report-server-sharepoint/add-the-report-viewer-web-part-to-a-web-page.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)