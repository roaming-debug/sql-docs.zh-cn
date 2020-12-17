---
title: 数据警报消息 | Microsoft Docs
description: 了解 SQL Server Reporting Services 数据警报如何通过电子邮件传递两种类型的数据警报消息：具有数据警报结果的消息和具有错误说明的消息。
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 6819720c-d848-4b90-9b51-89501b4f4645
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 7c7f28e92a29dd355d4b74de2121e1c386816116
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97425322"
---
# <a name="data-alert-messages"></a>数据警报消息

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../includes/ssrs-appliesto-2016.md)] [!INCLUDE [ssrs-appliesto-not-2017](../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../includes/ssrs-previous-versions.md)]

SQL Server Reporting Services 数据警报通过电子邮件传递两种类型的数据警报消息：具有数据警报结果的消息和具有错误说明的消息。 具有结果的消息向所有收件人告知有关报表数据中共同感兴趣的和对业务决策至关重要的更改。 如果由于某种原因导致错误且结果不可用，则发送错误消息。

数据警报定义的所有者还可以在数据警报管理器中查看有关数据警报实例的信息。 有关详细信息，请参阅 [Data Alert Manager for SharePoint Users](../reporting-services/data-alert-manager-for-sharepoint-users.md)。  

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。
  
##  <a name="data-alert-messages"></a><a name="DataAlertMessages"></a> 数据警报消息  
 下图显示具有结果的数据警报消息和具有错误说明的警报消息。  
  
 **结果消息**  
  
 ![包含结果的数据警报电子邮件](../reporting-services/media/rs-alertmessageresults.gif "包含结果的数据警报电子邮件")  
  
 **错误消息**  
  
 ![包含错误消息的数据警报消息](../reporting-services/media/rs-alertmessageerrror.gif "包含错误消息的数据警报消息")  
  
 这些消息包含相同类型的信息。  
  
1.  **代表** 包含创建数据警报定义的人员姓名。  
  
2.  如果您在报表定义中提供了说明，则它会在 **代表** 下显示。  
  
3.  **警报结果** 显示报表数据馈送中符合在警报定义中以表格格式指定的规则的行，或显示错误说明。 对于显示的行数没有限制。  
  
4.  **转到报表** 是指向警报定义所基于的报表的链接。 如果因为报表移动或删除而导致此链接无效，则显示一条错误消息。  
  
5.  **规则** 列出了警报定义中的规则和子句。 此信息帮助您验证和了解警报结果，并确定数据警报定义中您可能希望更改以缩小或扩大结果的规则。  
  
6.  **报表参数** 列出在运行报表时所使用的参数和参数值。 参数及参数值可帮助您了解警报结果。  
  
7.  **上下文值** 列出报表数据区域之外的报表项的名称和值。 这些项通常为文本框。 例如，带常量值的文本框（如报表主题或报表说明）。  
  
 两种消息类型的唯一区别在于项 5，即 **警报结果**。 如果在创建数据警报实例或数据警报消息时发生错误，则 **警报结果** 显示一条描述问题的错误消息。 此错误消息将发送给所有收件人，让他们知道他们预期和可能依赖来制定业务决策的警报结果不可用。  
  
  
##  <a name="related-tasks"></a><a name="HowTo"></a> 相关任务  
 本部分列出的过程说明如何创建和编辑数据警报定义，这些定义可提供您在数据警报消息中看到的大量信息。  
  
-   [在数据警报设计器中创建数据警报](../reporting-services/create-a-data-alert-in-data-alert-designer.md)  
  
-   [在警报设计器中编辑数据警报](../reporting-services/edit-a-data-alert-in-alert-designer.md)  

## <a name="see-also"></a>另请参阅

[数据警报设计器](../reporting-services/data-alert-designer.md)   
[Reporting Services 数据警报](../reporting-services/reporting-services-data-alerts.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
