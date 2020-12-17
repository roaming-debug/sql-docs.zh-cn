---
title: 在数据警报管理器中管理我的数据警报 | Microsoft Docs
description: 了解如何在数据警报管理器中查看他们所创建的数据警报列表以及与这些警报有关的信息。
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- managing, alerts
- managing, data alerts
ms.assetid: e0e4ffdf-bd4c-4ebd-872b-07486cbb47c2
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: db36e1374b94c95691b73e97837aa025d4fb07d1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477098"
---
# <a name="manage-my-data-alerts-in-data-alert-manager"></a>在数据警报管理器中管理我的数据警报

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

SharePoint 用户可以查看他们所创建的数据警报列表以及与这些警报有关的信息。 用户还可以删除其警报，打开警报定义以便在数据警报设计器中进行编辑，以及运行其警报。 下图显示数据警报管理器中可用于用户的功能。

 ![面向 SharePoint 用户的警报管理器功能](../reporting-services/media/rs-alertmanageriw.gif "面向 SharePoint 用户的警报管理器功能")

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

### <a name="to-view-a-list-of-your-alerts"></a>查看您的警报列表  
  
1.  转至您在其中保存了您已创建数据警报的报表的 SharePoint 库。  
  
2.  单击针对报表展开下拉菜单的图标，然后单击“管理数据警报”。 下图显示该下拉菜单。  
  
     ![从报表上下文菜单打开警报管理器](../reporting-services/media/rs-openalertmanager.gif "从报表上下文菜单打开警报管理器")  
  
     数据警报管理器随之打开。 默认情况下，它列出了针对您在库中选择的报表的警报。  
  
3.  单击 **“查看报表警报”** 列表旁的向下箭头并且选择要查看其警报的报表，或者单击 **“全部显示”** 以列出所有警报。  
  
    > [!NOTE]  
    >  如果所选报表没有任何警报，则不必返回 SharePoint 库来查找和选择具有警报的报表。 而是单击 **“全部显示”** 以查看所有警报的列表。  
  
     一个表将列出警报名称、报表名称、警报创建者的姓名、发送警报的数目、上次修改警报定义的时间以及警报的状态。 如果无法生成或发送警报，则状态列将包含有关该错误的信息并且帮助您纠正该问题。  
  
### <a name="to-edit-an-alert-definition"></a>编辑警报定义  
  
-   右键单击要编辑其警报定义的数据警报，然后单击“编辑”。  
  
     该警报定义将在数据警报设计器中打开。 有关详细信息，请参阅 [在警报设计器中编辑数据警报](../reporting-services/edit-a-data-alert-in-alert-designer.md) 和 [数据警报设计器](../reporting-services/data-alert-designer.md)。  
  
    > [!NOTE]  
    >  只有创建了数据警报定义的用户才能对其进行编辑。  
  
    > [!NOTE]  
    >  如果报表已更改并且从该报表生成的数据馈送已更改，则警报定义将不再有效。 在发生以下情况之一时警报定义将不再有效：警报在其规则中引用的列从报表中删除、更改数据类型或包括在其他数据馈送中，或删除或移动了报表。 您可以打开无效的警报定义，但在其根据报表数据馈送所基于的当前版本变为有效之前无法重新保存。 若要深入了解如何从报表中生成数据馈送，请参阅[基于报表生成数据馈送（报表生成器和 SSRS）](../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md)。  
  
### <a name="to-delete-an-alert-definition"></a>删除警报定义  
  
-   右键单击要删除的数据警报，然后单击“删除”。  
  
     删除警报后，将不会发送进一步的警报消息。  
  
### <a name="to-run-an-alert"></a>运行警报  
  
-   右键单击要运行的数据警报，然后单击“运行”。  
  
     创建警报实例，并立即发送数据警报消息，而不考虑数据警报设计器中指定的计划选项。 例如，配置为每周发送的警报，并且仅当结果更改时才发送。  

## <a name="see-also"></a>另请参阅

[向管理员提出警报的数据警报管理器](../reporting-services/data-alert-manager-for-alerting-administrators.md)   
[Reporting Services 数据警报](../reporting-services/reporting-services-data-alerts.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
