---
title: 向用户和警报管理员授予权限 | Microsoft Docs
description: 了解如何在 SQL Server Reporting Services (SSRS) 中向用户授予权限并向管理员发出警报。
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 166808e1-ada7-48d2-bda8-8f7c017fb3aa
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 87c453d6b896a998d6a3229d57db905cf57cc0fb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472528"
---
# <a name="grant-permissions-to-users-and-alerting-administrators"></a>向用户和警报管理员授予权限

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

用户和警报管理员必须首先被授予 SharePoint 权限，然后才能创建、编辑、删除和查看数据警报。 没有用于使用 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 数据警报功能的特殊权限，你将使用内置的 SharePoint 权限。

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

**信息工作者** - 权限必须包括“创建警报”和“查看项”等 SharePoint 权限。 名为“设计”、“参与讨论”、“读取”和“仅查看”的内置 SharePoint 权限级别包括“创建警报”和“查看项”SharePoint 权限。 您还可以创建自定义权限级别，该级别具有支持创建、编辑、运行和查看数据警报的用户所需的权限。

**警报管理员** - 权限必须包括“管理警报”SharePoint 权限。 默认情况下，对于使用“工作组网站”站点模板创建的站点，只有“完全控制”权限级别包括此权限。 如果您使用其他站点模板，您将看到不同的默认 SharePoint 组列表。 您可以将“管理警报”权限添加到内置权限级别之一，或者创建具有支持查看和删除数据警报的警报管理员所需权限的自定义权限级别。

若要了解有关 SharePoint 权限的详细信息，请参阅 [用户权限和权限级别 (SharePoint Server 2010)](/SharePoint/sites/user-permissions-and-permission-levels)。

## <a name="grant-permissions"></a>授予权限
  
1.  转到要授予权限的 SharePoint 站点。  
  
2.  在工具栏上，单击 **“网站操作”** ，然后单击 **“网站权限”** 。  
  
     如果未看到此选项，则表明您没有向他人授予权限所需的足够权限。  
  
3.  单击“授予权限”。  
  
4.  在“用户/组”中，键入要授予权限的用户名、组名或电子邮件地址。  
  
5.  选择 **“向 SharePoint 组添加用户”** 或 **“直接授予用户权限”** 选项。 根据您是选择 **“向 SharePoint 组添加用户”** 还是选择 **“直接授予用户权限”** ，执行下列操作之一：  
  
    -   如果选择了“向 SharePoint 组添加用户”，则在下拉列表中选择某一权限级别。  
  
    -   如果您选择了 **“直接授予用户权限”** ，则选择某一权限级别。  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  

## <a name="see-also"></a>另请参阅

[在 SharePoint 站点上为报表服务器项设置权限（SharePoint 集成模式下的 Reporting Services）](../reporting-services/security/set-permissions-for-report-server-items-on-a-sharepoint-site.md)   
[Reporting Services 数据警报](../reporting-services/reporting-services-data-alerts.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)