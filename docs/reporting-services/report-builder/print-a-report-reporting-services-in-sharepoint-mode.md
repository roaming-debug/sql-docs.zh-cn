---
title: 打印报表（SharePoint 模式下的 Reporting Services）| Microsoft Docs
description: 如果报表服务器在 SharePoint 模式下运行，则可以通过 SharePoint 站点、浏览器或目标应用程序打印 SharePoint Web 应用程序中的报表。
ms.date: 05/14/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
helpviewer_keywords:
- printing reports, SharePoint Web application
- printing reports
ms.assetid: 026784f7-8cb4-4351-93ee-230b2ab0f8f5
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 25d400a063d668972092a37d57b9371f74e5d980
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97409538"
---
# <a name="print-a-report-reporting-services-in-sharepoint-mode"></a>打印报表（SharePoint 模式下的 Reporting Services）
  对于在 SharePoint 模式下运行的报表服务器，可使用三种方法从 SharePoint Web 应用程序打印报表：  
  
-   **从 SharePoint 站点** 从打开报表时显示在报表工具栏中的 **“操作”** 菜单中选择 **“打印”** 。 这将提供 Reporting Services 打印功能，其中包括用于选择打印机、指定页面和边距以及预览报表的标准 **“打印”** 对话框。 此打印功能旨在替代浏览器“文件”菜单中的“打印”命令。 通过这种方式打印报表时，报表的打印效果与其设计样式相同，只是不包含在网页打印输出中看到的额外元素。  
  
-   **从浏览器** 浏览器的打印功能最适合打印可容纳在一页中的 HTML 报表。 通常，从浏览器打印的页面包括网页上的所有可见元素，以及标识网页或网站的页眉和页脚信息。 通过浏览器打印时，仅打印当前窗口中的内容。 如果报表较长，浏览器将只打印报表的一部分（通常只打印第一页）。  
  
-   **从目标应用程序** 您可以导出报表，以使用目标应用程序（例如 Microsoft Office Excel 或 Adobe Acrobat Reader）的打印功能。 有些应用程序格式（例如 TIFF 或 PDF）最适合打印多页报表。 将报表导出至桌面应用程序后，可以使用应用程序提供的任何专用打印功能。 若要导出报表，请从打开报表时显示在报表工具栏中的 **“操作”** 菜单中选择 **“导出”** 。  
  
> [!NOTE]  
>  若要打印某个报表，您必须拥有查看它的权限。  
  
 若要获得从网页打印报表的最佳效果，请使用 **“操作”** 菜单中的 **“打印”** 。 **“打印”** 操作绑定在从报表服务器上下载的客户端打印控件中。 首次选择 **“打印”** 时，会进行一次下载。  
  
 报表作者可以专门针对打印输出或特定的应用程序格式来设计报表。 请注意，由于不同应用程序格式的分页方式不同，因此对于所有报表的每种导出格式，您可能无法都获得最佳的打印输出效果。 与针对打印输出而设计的报表相比，在屏幕上显示的报表页面的设计宗旨是容纳可变数量的数据。 例如，对于包括矩阵的报表，根据您扩展行和列的方式，报表页可能会在水平方向和垂直方向同时扩展。 打印大小可变的报表时，不扩展矩阵的用户与扩展矩阵的用户获得的打印效果将会不同。 对于大多数的导出报表，报表打印输出包括报表上的所有可见内容，与用户在计算机监视器上看到的内容没有分别。  
  
### <a name="how-to-print-reports-from-the-actions-menu"></a>如何从“操作”菜单打印报表  
  
1.  打开该报表。  
  
2.  在 **“操作”** 菜单上，单击 **“打印”** 。 如果看不到 **“操作”** 菜单，则表明报表工具栏已隐藏，并且您无法使用报表工具栏提供的功能。 如果可以看到 **“操作”** 菜单，但是上面没有 **“打印”** ，则表明报表服务器上的打印功能已禁用并且您无法使用它。  
  
3.  在 **“打印”** 对话框中，选择要使用的打印机和设置，然后单击 **“确定”** 。  
  
     若要修改默认设置，请单击 **“属性”** 按钮。 “页大小”取决于报表定义中定义的报表页大小的默认高度和宽度。 页面尺寸的可更改范围取决于所用打印机的能力。  
  
     若要在打印前查看报表，请单击 **“预览”** 按钮。 这将在单独的预览窗口中打开报表的首页。 如果报表已呈现在报表服务器上，则可以预览其他页。 预览的报表以 EMF 格式呈现。 在到达最后一页（此时会禁用 **“下一页”** 按钮）之前，您可以导航到上一页或下一页。 若要在预览页中修改打印边距，请单击 **“边距”** 按钮。 将显示 **“边距”** 对话框。 配置上、下、左、右边距，然后单击 **“确定”** 。 此时，该对话框将关闭，并且修改后的设置会存储下来，以用于呈现预览和打印。  
  
## <a name="see-also"></a>另请参阅  
 [启用和禁用 Reporting Services 的客户端打印](../../reporting-services/report-server/enable-and-disable-client-side-printing-for-reporting-services.md)  
  
  
