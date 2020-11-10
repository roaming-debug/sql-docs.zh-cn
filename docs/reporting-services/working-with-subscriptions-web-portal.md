---
title: 使用订阅（Web 门户）| Microsoft Docs
description: 了解如何使用“订阅”页在 Reporting Services 中列出当前报表的所有订阅。
ms.date: 01/31/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 09e8ece5-0200-41f2-87c1-9fab19e261be
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4efd72f1c2d6f9098e2af4840483d38d4749d264
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243740"
---
# <a name="working-with-subscriptions-web-portal"></a>使用订阅（Web 门户）

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

使用“订阅”页可以列出当前报表的所有订阅。 如果有足够的权限（指“管理所有订阅”任务），则可以查看所有用户的订阅。 否则，此页仅显示您所拥有的订阅。  
  
必须确认报表数据源使用的是存储凭据，才能创建新订阅。 使用“数据源”属性页可以存储凭据。  
  
> [!NOTE]
> 需要启动 SQL Server 代理服务。   
  
![管理订阅](../reporting-services/media/working-with-subscriptions-web-portal/ssrs-manage-subscriptions.png)  
可通过依次选择报表的省略号 (…)、“管理”和“订阅”访问“订阅”页面  。  
  
从“订阅”页中，可以通过选择“+ 新订阅”来创建新订阅。 还可以编辑现有订阅，或删除选择的订阅。  
  
此页还在“结果”列中提供订阅运行的结果状态。 如果订阅出错，则需要首先检查结果列以查看消息内容。 

还可随时通过选择“订阅”页上的“立即运行”来运行订阅。
  
## <a name="creating-or-editing-a-subscription"></a>创建或编辑订阅  
使用“新建订阅”或“编辑订阅”页，可为报表创建新订阅或修改报表的现有订阅。 此页包含的选项取决于您的角色分配。 具有高级权限的用户可以使用附加选项。  
  
以无人参与的方式运行的报表支持订阅。 报表必须最起码使用已存储的凭据或不使用凭据。 如果报表使用参数，则必须指定默认值。 如果更改报表执行设置或删除参数属性使用的默认值，订阅可能进入非活动状态。 有关详细信息，请参阅[创建和管理本机模式报表服务器的订阅](subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)。  
  
## <a name="type-of-subscription"></a>订阅的类型  
可以在“标准订阅”与“数据驱动订阅”之间进行选择 。  
  
![显示“订阅类型”部分的屏幕截图。](../reporting-services/media/working-with-subscriptions-web-portal/ssrswebportal-subscriptions3.png)  
   
数据驱动订阅是在每次运行订阅时，都会在订阅服务器数据库中查询订阅信息的订阅。 数据驱动订阅使用查询结果来确定订阅的收件人、传递设置和报表参数值。 在运行时，报表服务器将运行一个查询，以获取订阅设置所需的值。   
  
要创建数据驱动订阅，您必须了解如何编写查询或命令来获取订阅的数据。 还必须有一个数据存储，其中包含要用于订阅的订阅服务器数据（例如订阅服务器名称和电子邮件地址）。  
  
具有高级权限的用户可以使用此选项。 如果使用的是默认的安全设置，则位于“我的报表”文件夹的报表将无法使用数据驱动订阅。  
  
## <a name="destination"></a>目标  
选择用于分发报表的传递扩展插件。   
  
传递扩展插件的可用性取决于其是否在报表服务器上进行了安装和配置。 报表服务器电子邮件是默认的传递扩展插件，但是使用前必须先行配置。 文件共享传递不需要配置，但是使用前必须定义一个共享文件夹。  
  
![显示目标和传递选项（Windows 文件共享）部分的屏幕截图。](../reporting-services/media/working-with-subscriptions-web-portal/ssrswebportal-subscriptions2.png)  
  
根据选定的传递扩展插件，会显示下列设置：  
  
-   电子邮件订阅提供了电子邮件用户所熟悉的字段（例如“收件人”、“主题”和“优先级”字段）。 指定 **“包括报表”** 可以嵌入或附加报表，而指定 **“包括链接”** 可以将 URL 包括在报表中。 指定 **“呈现格式”** 可以选择附加报表或嵌入报表的显示格式。 有关详细信息，请参阅[创建电子邮件订阅](subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md#bkmk_create_email_subscription)。 
  
-   文件共享订阅提供了允许您指定目标位置的字段。 您可以将任何报表传递到文件共享位置。 但是，支持交互式功能的报表（包括支持深化以及支持行和列的矩阵报表）将以静态文件的形式呈现。 无法查看静态文件中的深化行和深化列。 必须以通用命名约定 (UNC) 格式指定文件共享名（例如，\mycomputer\public\myreportfiles）。 不能在路径名的末尾包含反斜杠。 报表文件将以基于呈现格式的文件格式进行传递（例如，如果选择 Excel，则报表以 .xlsx 文件格式进行传递）。  有关详细信息，请参阅[创建文件共享订阅](subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md#bkmk_create_fileshare_subscription)。
  
## <a name="data-driven-subscription-dataset"></a>数据驱动订阅数据集  
对于数据驱动订阅，需要定义用于订阅的数据集。 选择“编辑数据集”以提供该信息。  
  
![显示数据集部分的屏幕截图。](../reporting-services/media/working-with-subscriptions-web-portal/ssrswebportal-subscriptions4.png)  
  
需要首先提供要用于查询的 **数据源** 。 这可以是共享数据源，也可以提供自定义数据源。  
  
随后需要提供列出运行订阅所需的不同选项的 **查询** 。 屏幕会提供需要返回的字段。 这些字段因传递方法和报表参数而异。  
  
为了实现最佳效果，在数据驱动的订阅中使用该查询之前，请先在 SQL Server Management Studio 中运行该查询。 之后可以检查查询结果，验证它是否包含所需的信息。 对于查询结果，请注意下面的几个要点：  
  
-   结果集内的列确定可以为传递选项和报表参数指定的值。 例如，如果要为电子邮件传递创建数据驱动订阅，则应具有一列电子邮件地址。  
  
-   结果集内的行决定了所生成的报表传递的数量。 如果您有 10,000 行，则报表服务器将生成 10,000 个通知和传递。  
  
![显示查询部分的屏幕截图。](../reporting-services/media/working-with-subscriptions-web-portal/ssrswebportal-subscriptions5.png)  
  
随后可以验证查询。 还可以定义 **查询超时** 。  
  
创建了查询之后，随后可以将值分配给必填字段。 可以输入手动数据，也可以从创建的数据集选择字段。 

## <a name="next-steps"></a>后续步骤

[创建和管理本机模式报表服务器的订阅](subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md) 
[Web 门户](../reporting-services/web-portal-ssrs-native-mode.md)  
[使用分页报表](working-with-paginated-reports-web-portal.md)  
[使用共享数据集](../reporting-services/work-with-shared-datasets-web-portal.md)

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
