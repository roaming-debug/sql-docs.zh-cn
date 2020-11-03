---
title: 添加从某个移动报表到其他移动报表或 URL 的钻取 | Microsoft Docs
description: 你可以添加从某个 Reporting Services 移动报表中的任何仪表、图表或数据网格到另一个移动报表或自定义 URL 的钻取。
ms.date: 09/20/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: 30d0a3fd-5588-417e-b25d-cc5b7624cdb1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 149c074b0aacc56f192b27cfea0894fe2cd73778
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907295"
---
# <a name="add-drillthrough-from-a-mobile-report-to-other-mobile-reports-or-urls"></a>添加从某个移动报表到其他移动报表或 URL 的钻取
你可以添加从某个 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 移动报表中的任何仪表、图表或数据网格到另一个移动报表或自定义 URL 的钻取。 

*钻取*  是来自源报表的链接，可打开另一个目标报表或 URL。 目标钻取报表通常包含有关摘要报表中某个项的详细信息。 可以将一个或多个参数传递到目标移动报表或集成到自定义 URL，具体视源移动报表而定。  
  
当你查看 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] Web 门户中的源移动报表并选择具有钻取目标的元素时，将转到该目标（另一个移动报表或 URL）。  

具有钻取（指向 URL 或另一个移动报表）的报表项在右上角具有钻取图标 :::image type="icon" source="../../reporting-services/mobile-reports/media/mobile-report-drill-through-icon.png":::。

![具有钻取的移动报表仪表的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-gauge-drill-through.png)

>**提示** ：先创建目标报表并将其保存到 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] Web 门户中。 如果打算从源报表传递参数，则还需要将参数添加到目标报表中。 然后，可以设置从源报表到目标报表的钻取。 [将参数添加到移动报表](../../reporting-services/mobile-reports/add-parameters-to-a-mobile-report-reporting-services.md)。
 
## <a name="set-up-drillthrough-to-a-mobile-report"></a>设置到移动报表的钻取  

1. 在 [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)]的“布局”视图中，选择一个支持钻取的可视化效果。   

   与大多数图表和简单的数据网格一样，地图和仪表也支持钻取。
   
2. 在“视觉对象属性”窗格中，选择“钻取目标” > “移动报表”。  
3. 选择服务器和目标移动报表。  

   >注意：如果目标移动报表与源移动报表不在同一个服务器上，则改为使用自定义 URL 连接到源移动报表，如下一节所述。  
 
4. 选择目标移动报表之后，你会看到其可用的输入参数，包括可以绑定到导航器控件的属性以及对目标移动报表的数据集配置的参数。  

   ![“配置目标报表”对话框的屏幕截图，其中显示了可用的报表参数。](../../reporting-services/mobile-reports/media/mobile-report-drillthrough-target.PNG)
   
   *目标移动报表的钻取属性*  
  
5. 选择每个属性右侧的箭头以将具有匹配数据类型的属性连接到源移动报表上的可用输出属性。 你还可以在此处设置每个输出的默认值，以防报表用户尚未与源移动报表交互便钻取到目标移动报表。  
  
## <a name="set-up-a-drillthrough-to-a-custom-url"></a>设置到自定义 URL 的钻取  
  
1. 在 [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)]的“布局”视图中，选择一个支持钻取目标的可视化效果。    
2. 在“视觉对象属性”窗格中，选择“钻取目标” > “自定义 URL”。  这将打开钻取配置对话框。  
  
3. 在“设置钻取 URL”中，输入在单击可视化效果时要转到的目标 URL，然后从右侧列出的“可用参数”中进行选择。 面板下方将显示自定义 URL 与示例解析参数（如果包含）的组合预览。  
  
   ![“设置钻取 URL”对话框的屏幕截图。](../../reporting-services/mobile-reports/media/mobile-report-drillthrough-url.PNG)
  
   *钻取到自定义 URL 属性*  
  
4. 单击“ **应用** ”。  

  
当你在 [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)]中预览移动报表时，如果单击某个带有钻取的可视化效果，将看到一条指示已禁用钻取的消息。 你只能在保存或发布移动报表后实际钻取到目标并查看它，而不能从 [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] 布局内或在预览模式下查看它。  

## <a name="hide-a-target-mobile-report-on-the-web-portal"></a>在 Web 门户上隐藏目标移动报表
如果你不打算为目标报表设置默认值，请考虑在 Web 门户上隐藏它。 否则，当有人尝试不通过源报表，而是直接在 Web 门户上查看它时，它将为空。

1. 在 Web 门户中，选择你要隐藏的目标报表上的省略号 (...)，然后选择“管理”。

2. 在“属性”中，选择“在磁贴视图中隐藏”。

你可以选择在 Web 门户中查看隐藏的项： 

* 在 Web 门户的右上角选择“查看” > “显示隐藏项” 。 

隐藏的项以较浅的颜色显示。
    
### <a name="see-also"></a>请参阅  
 
* [向 Reporting Services 移动报表添加参数](../../reporting-services/mobile-reports/add-parameters-to-a-mobile-report-reporting-services.md)
* [使用 SQL Server 移动报表发布服务器创建移动报表](../../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md) 
* [Web 门户（SSRS 本机模式）](../../reporting-services/web-portal-ssrs-native-mode.md)

