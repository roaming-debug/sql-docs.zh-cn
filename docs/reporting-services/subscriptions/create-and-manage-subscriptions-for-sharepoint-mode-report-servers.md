---
title: 创建和管理 SharePoint 模式报表服务器的订阅 | Microsoft Docs
description: 了解如何创建 Reporting Services 订阅以便从 SharePoint 模式报表服务器集成的 SharePoint Web 应用传递报表。
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], creating
- subscriptions [Reporting Services], deleting
- subscriptions [Reporting Services], managing
ms.assetid: 44be7ee2-33ce-46e4-9d1a-a20aaf43a227
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bf68742926ef0f26c5a12a5d001ea9b14f9b199c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080089"
---
# <a name="create-and-manage-subscriptions-for-sharepoint-mode-report-servers"></a>创建和管理 SharePoint 模式报表服务器的订阅
  你可以创建 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅以便从与 SharePoint 模式报表服务器集成的 SharePoint Web 应用程序传递报表。 订阅可以将报表传递到文档库、文件夹或作为电子邮件传递。 本主题概述了创建 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅的要求和步骤。  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]**<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式 &#124; SharePoint 2010 和 SharePoint 2013|  
  
 创建订阅时，有三种方法可用于指定其传递：  
  
-   **文档库**：可以创建一个订阅，用于将基于原始报表的文档传递到与原始报表位于同一 SharePoint 站点内的库。 不能将该文档传递到位于相同网站集内的其他服务器或其他站点。 若要传递此文档，您必须对该报表所传递到的库拥有“添加项”权限。  
  
-   **文件文件夹：** 可以将基于原始报表的文档传递到文件系统上的共享文件夹中。 必须选择可通过网络连接进行访问的现有文件夹。  
  
-   **电子邮件：** 如果将报表服务器配置为使用报表服务器电子邮件传递扩展插件，则可以创建一个订阅，将报表或导出的报表文件（以输出格式保存）发送到收件箱。 若要只接收不包含报表或报表 URL 的通知，请清除 **“包含指向报表的链接”** 和 **“在邮件中显示报表”** 复选框。  
  
 **本主题内容：**  
  
-   [针对订阅的一般要求](#bkmk_subscription_requirements)  
  
-   [创建订阅以便将报表传递到 SharePoint 库](#bkmk_tosharepoint_library)  
  
-   [创建订阅以便将报表传递到 SharePoint 库](#bkmk_tosharepoint_library)  
  
-   [为报表服务器电子邮件传递创建订阅](#bkmk_subscription_for_email)  
  
-   [删除或修改订阅](#bkmk_to_modify_subscription)  
  
-   [删除订阅](#bkmk_to_delete_subscription)  
  
##  <a name="general-requirements-for-subscriptions"></a><a name="bkmk_subscription_requirements"></a> 针对订阅的一般要求  
 若要创建订阅，报表必须使用已存储凭据，并且您必须拥有查看该报表和创建警报的权限。  
  
 在创建订阅时，可以选择输出文件格式。 并不是每个报表在每种格式下都能正常显示。 在订阅中选择格式之前，请打开报表并将其导出为不同格式以验证是否像预期的那样显示。  
  
 如果用户希望能够创建 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅，则他们需要具有在 SharePoint 中“编辑项”列表的权限  。 有关详细信息，请参阅 [报表服务器项的 SharePoint 站点和列表权限参考](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md)  
  
> [!IMPORTANT]  
>  用来将报表传递到库或共享文件夹的订阅会创建一个基于原始报表的新静态文件，但它不是在报表查看器 Web 部件中运行的真正报表定义。 如果原始报表具有交互功能（如钻取链接）或动态内容，则这些功能在传递到目标位置的静态文件中将不可用。 如果选择“网页”，则可以保留一些交互功能，但由于该文档不是在报表查看器中运行的 .rdl 文件，因此在报表中单击浏览时将会在浏览器会话中创建一些新页，必须在这些新页中滚动才能返回站点。  
  
 无法将导出报表的文件扩展名重命名为“.rdl”并使该报表在报表查看器 Web 部件中运行。 如果要创建可提供实际报表的订阅，则使用报表服务器电子邮件传递扩展插件并设置选项以包括指向报表的链接。  
  
 包含所传递文档的库的版本设置确定每次传递是否要创建文档的新版本。 默认情况下，为每个库启用版本设置。 除非专门选择 **“无版本控制”** ，否则传递时将为文档创建新的主版本。 只创建文档的主版本；订阅传递从不创建次版本，即使选择了允许次版本的版本控制选项。 如果您限制保留的主版本数量，则当达到最大限制值时，较早的传递将被更新的传递替代。  
  
 为订阅选择的输出格式基于在报表服务器上安装的呈现扩展插件。 您只能选择报表服务器上的呈现扩展插件所支持的输出格式。  
  
###  <a name="to-create-a-subscription-to-deliver-a-report-to-a-sharepoint-library"></a><a name="bkmk_tosharepoint_library"></a> 创建订阅以便将报表传递到 SharePoint 库  
  
1.  浏览到包含报表的 SharePoint 库。  
  
2.  单击该报表旁边的向下箭头，然后选择“管理订阅”  。  
  
3.  单击“添加订阅”  。  
  
4.  在 **“传递扩展插件”** 中选择 **“SharePoint 文档库”** 。  
  
5.  在 **“文档库”** 中，选择同一站点中的库。  
  
6.  在 **“文件选项”** 中，为将由订阅创建的文档指定文件名和标题。  
  
7.  在 **“输出格式”** 中，选择应用程序格式。  
  
     “Web 存档 (MHTML)”是默认值，这是因为它会生成自包含的 HTML 文件，但不会保留原始报表中可能存在的交互式报表功能。  
  
8.  在 **“覆盖选项”** 中，指定用于确定后续传递是否覆盖文件的选项。 如果要保留以前的传递，可以选择 **“使用唯一名称创建文件”** 。 将向新文件追加数字以创建唯一的文件名。  
  
9. 在 **“传递事件”** 中，指定会导致订阅运行的计划或事件。 您可以创建自定义计划，选择共享计划（如果可用），或者在使用快照数据运行的报表的数据被刷新时运行订阅。 有关计划和数据处理的详细信息，请参阅[设置处理选项（SharePoint 集成模式下的 Reporting Services）](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md)。  
  
10. 在 **“参数”** 中，如果所创建的是参数化报表的订阅，请指定处理订阅时要在该报表中使用的值。 如果所选报表不包含参数，则“参数”部分在此页上不可见。 有关参数的详细信息，请参阅[在已发布报表上设置参数（SharePoint 集成模式下的 Reporting Services）](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md)。  
  
###  <a name="to-create-a-subscription-for-shared-folder-delivery"></a><a name="bkmk_subscription_for_sharedfolder"></a> 为共享文件夹传递创建订阅  
  
1.  浏览到包含报表的 SharePoint 库。  
  
2.  单击该报表旁边的向下箭头，然后选择“管理订阅”  。  
  
3.  单击“添加订阅”  。  
  
4.  在 **“传递扩展插件”** 中选择 **“Windows 文件共享”** 。  
  
5.  在 **“文件名”** 中，输入将在共享文件夹中创建的文件的名称。  
  
6.  在“路径”中，使用包含计算机网络名称的通用命名约定 (UNC) 格式输入文件夹路径  。 不能在文件夹路径中包含尾随反斜杠。 示例路径可如下所示： `\\ComputerName01\Public\MyReports`，其中 Public 和 MyReports 是共享文件夹。  
  
7.  在 **“呈现格式”** 中，为该报表选择应用程序格式。  
  
8.  在 **“写入模式”** 中，选择 **“无”** 、 **“Autoincrement”** 或 **“覆盖”** 。 这些选项将确定后续传递是否会覆盖文件。 如果要保留以前的传递，可以选择 **“Autoincrement”** 。 将向新文件追加数字以创建唯一的文件名。 如果选择 **“无”** ，当目标位置已存在具有相同名称的文件时，将不会进行传递。  
  
9. 在 **“文件扩展名”** 中，选择 **True** 以添加与应用程序文件格式相对应的文件扩展名，或选择 False 以创建不具有扩展名的文件。  
  
10. 在 **“用户名”** 和 **“密码”** 中，输入对共享文件夹具有写权限的凭据。  
  
11. 在 **“传递事件”** 中，指定会导致订阅运行的计划或事件。 您可以创建自定义计划，选择共享计划（如果可用），或者在使用快照数据运行的报表的数据被刷新时运行订阅。 有关计划和数据处理的详细信息，请参阅[设置处理选项（SharePoint 集成模式下的 Reporting Services）](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md)。  
  
12. 在 **“参数”** 中，如果所创建的是参数化报表的订阅，请指定处理订阅时要在该报表中使用的值。 有关参数的详细信息，请参阅[在已发布报表上设置参数（SharePoint 集成模式下的 Reporting Services）](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md)。  
  
###  <a name="to-create-a-subscription-for-report-server-e-mail-delivery"></a><a name="bkmk_subscription_for_email"></a> 为报表服务器电子邮件传递创建订阅  
  
1.  浏览到包含报表的 SharePoint 库。  
  
2.  单击该报表旁边的向下箭头，然后选择“管理订阅”  。  
  
3.  单击“添加订阅”  。  
  
4.  在“传递扩展插件”中，选择“电子邮件”   。  
  
5.  在“传递选项”中，指定要将该报表发送到的电子邮件地址  。  
  
6.  您也可以修改“主题”行。 “主题”行使用内置参数，这些内置参数可捕获报表的名称和处理时间。 这些是仅有的可供使用的内置参数。 参数是用于自定义在“主题”行中显示的文本的占位符，但您可以将该文本替换为静态文本。  
  
7.  要在邮件的正文中嵌入报表 URL，请选择 **“包含指向报表的链接”** 。  
  
8.  在 **“报表内容”** 中，指定是否要在邮件正文嵌入实际的报表。  
  
     呈现格式和浏览器决定了报表是嵌入的还是作为附件发送。 如果您的浏览器支持 HTML 4.0 和 MHTML，并且您选择了 Web 存档呈现格式，那么报表将嵌入为邮件的一部分。 其他所有呈现格式（CSV、PDF 等）都将报表作为附件进行传递。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 不会检查附件或邮件的大小。 如果附件或邮件的大小超过了邮件服务器所允许的最大限制，则无法传递报表。 为大型报表选择其他传递选项之一（例如 URL 或通知）。  
  
9. 在 **“传递事件”** 中，指定会导致订阅运行的计划或事件。 您可以创建自定义计划，选择共享计划（如果可用），或者在使用快照数据运行的报表的数据被刷新时运行订阅。 有关计划和数据处理的详细信息，请参阅[设置处理选项（SharePoint 集成模式下的 Reporting Services）](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md)。  
  
10. 在 **“参数”** 中，如果所创建的是参数化报表的订阅，请指定处理订阅时要在该报表中使用的值。 有关参数的详细信息，请参阅[在已发布报表上设置参数（SharePoint 集成模式下的 Reporting Services）](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md)。  
  
###  <a name="to-view-or-modify-a-subscription"></a><a name="bkmk_to_modify_subscription"></a> 删除或修改订阅  
  
1.  浏览到包含报表的 SharePoint 库。  
  
2.  单击该报表旁边的向下箭头，然后单击“管理订阅”  。  
  
3.  每个订阅都按照传递类型进行标识。 单击订阅类型，以便查看和更改现有属性。  
  
###  <a name="to-delete-a-subscription"></a><a name="bkmk_to_delete_subscription"></a> 删除订阅  
  
1.  浏览到包含报表的 SharePoint 库。  
  
2.  单击该报表旁边的向下箭头，然后单击“管理订阅”  。  
  
3.  单击订阅旁的复选框，然后单击 **“删除”** 。  
  
## <a name="see-also"></a>另请参阅  
 [订阅和传递 (Reporting Services)](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Reporting Services 中的电子邮件传递](../../reporting-services/subscriptions/e-mail-delivery-in-reporting-services.md)   
 [Reporting Services 中的文件共享传递](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)   
 [Reporting Services 中的 SharePoint 库传递](../../reporting-services/subscriptions/sharepoint-library-delivery-in-reporting-services.md)   
 [为电子邮件传递配置报表服务器（报表服务器配置管理器）](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)  
  
  
