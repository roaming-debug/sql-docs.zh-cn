---
description: 本机到 SharePoint 迁移 (SSRS)
title: 本机到 SharePoint 的迁移 | Microsoft Docs
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: c5b15bec-6fde-4174-bcde-d043307244dd
author: maggiesMSFT
ms.author: maggies
monikerRange: = sql-server-2016
ms.openlocfilehash: f6020b7c7288ff9ed5a31e345dc26b06a07071bf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439332"
---
# <a name="native-to-sharepoint-migration-ssrs"></a>本机到 SharePoint 迁移 (SSRS)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  不能从一个 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务器模式升级或转换到另一个服务器模式。 例如，不能将本机模式报表服务器升级或转换到 SharePoint 模式。 您不能在模式之间复制报表服务器数据库，因为它们使用不同的数据库架构。 可以将内容从一个报表服务器迁移到另一个服务器。 您使用的工具依赖于为源和目标服务器配置的报表服务器模式的类型。  
  
##  <a name="reporting-services-migration-tool"></a><a name="bkmk_native_to_sharepoint"></a> Reporting Services 迁移工具  
 此工具支持将内容从本机模式部署迁移到 SharePoint 模式部署。 此工具不支持从 SharePoint 模式迁移到 SharePoint 模式或从 SharePoint 模式迁移到本机模式。  
  
 有关详细信息，请参阅 [Reporting Services 迁移工具](https://www.microsoft.com/download/details.aspx?id=29560) (https://www.microsoft.com/download/details.aspx?id=29560) 。  
  
## <a name="use-script-to-migrate-content"></a>使用脚本迁移内容  
 如果迁移工具不满足您的要求，您可以手动迁移报表服务器数据。 下面概述了要将报表项从一个 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 部署迁移到另一个时所需完成的步骤。 此方法支持将本机或 SharePoint 模式作为源或目标服务器。  
  
1.  备份和还原加密密钥。 这是用于加密数据的密钥。 加密密钥还用于对密码进行加密，例如为数据源连接存储的密码。 但是，不能迁移密码，并且您将需要在目标环境中重新输入这些密码。  
  
2.  **[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] RSS 脚本：** 编写调用报表服务器 Web 服务 SOAP 方法的 Visual Basic 脚本，以便在各数据库之间复制数据。 使用 **RS.exe** 实用工具运行此脚本。 Rs.exe 随 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]一起安装。  
  
    -   [用于在报表服务器之间复制内容的示例 Reporting Services rs.exe 脚本](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md)。 以下主题说明如何使用可从 CodePlex 下载的示例脚本。  
  
    -   CodePlex 上的示例 rss 脚本，可将一个报表服务器中的内容迁移到另一个中的 [Reporting Services RS.exe 脚本](https://azuresql.codeplex.com/releases/view/115207)  
  
    -   [脚本编写和带 Reporting Services 的 PowerShell](../../reporting-services/tools/scripting-and-powershell-with-reporting-services.md)  
  
 下表概述了可以使用脚本迁移的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 对象：  
  
|对象|是否可编写脚本|注释|  
|------------|---------------------|--------------|  
|报表|是|在迁移后，重新为数据源输入密码。|  
|Datasources|是|在迁移后，重新将报表链接到数据源。|  
|模型|是||  
|数据集|是||  
|报表部件||在迁移后，验证或更新指向报表部件的路径。|  
|计划|是|请参阅 ListSchedules 方法 [Subscription and Delivery Methods](../../reporting-services/report-server-web-service/methods/subscription-and-delivery-methods.md)|  
|订阅|是|请参阅列表订阅方法 [Subscription and Delivery Methods](../../reporting-services/report-server-web-service/methods/subscription-and-delivery-methods.md) 和 <xref:ReportService2010.ReportingService2010.ChangeSubscriptionOwner%2A> 方法。|  
|快照|||

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
