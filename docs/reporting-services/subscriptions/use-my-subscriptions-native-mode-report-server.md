---
title: 使用我的订阅（本机模式报表服务器）| Microsoft Docs
description: 了解如何使用 Reporting Services Web 门户中的“我的订阅”页面来查看、修改、启用、禁用或删除现有订阅。
ms.date: 07/01/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], My Subscriptions page
- My Subscriptions page [Reporting Services]
ms.assetid: e96623ba-677e-4748-8787-f32bed3b5c12
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bb653797aa0c89b16daeff71974a26781770a9ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100019798"
---
# <a name="use-my-subscriptions-native-mode-report-server"></a>使用我的订阅（本机模式报表服务器）
[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户中包含一个“我的订阅”  页，通过它可以将所有订阅组织在一个位置中。 可以使用“我的订阅”  来查看、修改、启用、禁用和删除现有订阅。 不过，您不能使用该页来创建订阅。  “我的订阅”仅显示您创建的订阅。 它不会列出其他用户拥有的订阅（即使您已添加为这些订阅的订阅者），也不会显示数据驱动订阅。
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]**  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式|  
  
搜索字段会动态筛选订阅列表。你既不能根据名称来搜索订阅，也不能根据触发器信息、状态信息等来搜索订阅。 有关详细信息，请参阅 [创建和管理本机模式报表服务器的订阅](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)。
  
## <a name="to-open-the-my-subscriptions-page"></a>打开“我的订阅”页  
1. 打开 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] Web 门户。
2. 单击“设置” ![ssrs_portal_settings_gear](../../reporting-services/subscriptions/media/ssrs-portal-settings-gear.png) 在工具栏。
3. 单击“我的订阅”  。

有关详细信息，请参阅 [Web portal (SSRS Native Mode)](../../reporting-services/web-portal-ssrs-native-mode.md)。

## <a name="use-windows-powershell-to-list-mysubscriptions"></a>使用 Windows PowerShell 列出 MySubscription  
 ![PowerShell 相关内容](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell 相关内容")  
  
 以下 PowerShell 脚本将返回当前用户的订阅和订阅属性的列表。 有关详细信息，请参阅 [ReportingService2010.ListMySubscriptions 方法](/dotnet/api/reportservice2010.reportingservice2010.listmysubscriptions)。  
  
```  
#server -  all subscriptions of the current user at the given server or site  
$server="[server name]/reportserver"  
$rs2010 = New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
  
$subscriptions=ListMySubscriptions(ItemPathOrSiteURL)  
$subscriptions | select Path, report, Description, Owner, SubscriptionID, lastexecuted,Status  
#uncomment the following to list all the subscription properties  
#$subscriptions

```  
  
## <a name="see-also"></a>另请参阅  
 [Data-Driven Subscriptions](../../reporting-services/subscriptions/data-driven-subscriptions.md)   
 [订阅和传递 (Reporting Services)](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [（旧）创建和管理本机模式报表服务器的订阅](./create-and-manage-subscriptions-for-native-mode-report-servers.md)  
  
