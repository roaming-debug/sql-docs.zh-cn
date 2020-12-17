---
title: 在数据警报设计器中创建数据警报| Microsoft Docs
description: 了解如何在数据警报设计器中创建数据警报定义。 另外了解如何编辑警报并在完成后保存编辑。
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 8464ab9d-afe1-4490-955f-9f3319bcbf8d
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 28b62c4f3bc15e00337099fd3f954406e3f04122
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97425492"
---
# <a name="create-a-data-alert-in-data-alert-designer"></a>在数据警报设计器中创建数据警报

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

您在数据警报设计器中创建数据警报定义。 在您保存警报定义后，可以在数据警报设计器中重新打开、编辑和重新保存它们。 有关编辑警报定义的信息，请参阅 [在数据警报管理器中管理我的数据警报](../reporting-services/manage-my-data-alerts-in-data-alert-manager.md) 和 [在警报设计器中编辑数据警报](../reporting-services/edit-a-data-alert-in-alert-designer.md)。

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

## <a name="create-a-data-alert-definition"></a>创建数据警报定义
 
1.  找到包含您要为其创建数据警报定义的报表的 SharePoint 库。  
  
2.  单击该报表。  
  
     该报表将运行。 如果对该报表进行参数化，则确认该报表显示您要接收有关其警报消息的数据。 如果您看不到感兴趣的列或值，则可能要使用不同的参数值重新运行报表。  
  
    > [!NOTE]  
    >  您选择运行报表的参数值保存在警报定义中，并在作为处理警报定义的一个步骤重新运行报表时使用。 若要使用不同的参数值，您必须创建一个新的警报定义。  
  
3.  在 **“操作”** 菜单上，单击 **“新建数据警报”** 。  
  
     下图显示该 **“操作”** 菜单。  
  
     ![从 SharePoint 库打开警报设计器](../reporting-services/media/rs-openalertdesigneriw.gif "从 SharePoint 库打开警报设计器")  
  
     数据警报设计器将打开，并且显示报表在表中生成的第一个数据馈送的前 100 行。  
  
    > [!NOTE]  
    >  如果看不到 **“新建数据警报”** 选项，则未在 SharePoint 站点上配置警报服务，或 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本不包含数据警报。 有关详细信息，请参阅 [Reporting Services SharePoint 服务和服务应用程序](../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md)。  
    >   
    >  如果 **“新建数据警报”** 选项灰显，则报表数据源配置为使用集成的安全凭据或提示输入凭据。 若要使 **“新建数据警报”** 选项可用，则必须更新数据源以使用存储的凭据或无凭据。  
  
     数据馈送的名称将出现在“报表数据名称”下拉列表中。  
  
4.  还可以在“报表数据名称”下拉列表中选择其他数据馈送。  
  
     如果没有从报表生成任何数据馈送，您无法为报表创建警报定义。 报表的布局确定每个数据馈送的内容。 有关详细信息，请参阅[基于报表生成数据馈送（报表生成器和 SSRS）](../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md)。  
  
5.  或者在 **“警报名称”** 文本框中，将默认名称更新为更有意义的名称。  
  
     警报定义的默认名称即为报表的名称。 警报定义名称不一定是唯一的，因此，在您以后在数据警报管理器中查看警报的列表时，可能很难区分这些警报。 建议使用有意义且唯一的警报定义名称。  
  
6.  或者，将默认数据选项从“在数据馈送中具有任何数据”更改为“在数据馈送中没有任何数据” 。  
  
7.  单击 **“添加规则”** 。  
  
     将显示数据馈送中列的列表。  
  
8.  在此列表中，选择要在规则中使用的列，然后选择比较运算符并输入阈值。  
  
     根据所选列的数据类型，将列出不同的比较运算符。 如果该列具有日期数据类型，则在规则的阈值旁将显示一个日历图标。 您可以通过单击日历中的日期或者键入日期，输入数据。  
  
     数据警报设计器提供两种比较模式：“值输入模式”和“字段选择模式”。 默认模式为 **“值输入模式”** 。 仅当处于 **“值输入模式”** 且正在使用 **“是”** 比较时，才可添加 OR 子句。  
  
9. 若要添加 OR 子句，请单击向下箭头，然后单击“值输入模式”。  
  
10. 键入比较值。  
  
11. 也可以再次单击省略号 (…)。  
  
     省略号 (…) 将显示在包含第一个子句的行中。  
  
     OR 子句将添加在 AND 规则下面和内部。  
  
12. 还可以单击向下箭头，选择“字段选择模式”，然后选择该列表中的列。  
  
     你将会发现单击以添加 OR 子句的省略号 (…) 已消失。  
  
13. 或者，再次单击 **“添加规则”** 以便添加其他规则。  
  
     这些规则由使用 AND 逻辑运算符组合在一起。  
  
14. 在重复执行列表中选择一个选项。 根据重复执行的类型，输入一个间隔。  
  
15. 或者，单击 **“高级”** 。  
  
16. 还可以通过键入不同的日期，或通过打开日历然后在日历中单击某个日期，更改警报消息开始的日期。  
  
     默认开始日期为当前日期。  
  
17. 或者，选中 **“停止警报时间”** 旁的复选框，然后选择用于停止警报消息的日期。  
  
     默认情况下，警报消息没有停止日期。  
  
    > [!NOTE]  
    >  停止警报消息并不会删除警报定义。 停止警报消息之后，可以通过更新开始日期和停止日期来重新启动警报消息。 有关删除警报定义的信息，请参阅 [在数据警报管理器中管理我的数据警报](../reporting-services/manage-my-data-alerts-in-data-alert-manager.md)。  
  
18. 还可以清除 **“仅当结果更改时才发送消息”** 复选框。  
  
     如果频繁地发送警报消息，则不欢迎冗余信息，您不应清除此复选框。  
  
19. 输入警报消息收件人的电子邮件地址。 用分号分隔地址。  
  
     如果创建了警报定义的人士的电子邮件地址可用，则该地址将添加到“收件人”框中。  
  
20. 或者，在 **“主题”** 文本框中，更新警报消息的“主题”行。  
  
     默认“主题”为“\<data alert name> 的数据警报”。  
  
21. 还可以在 **“说明”** 文本框中键入对警报消息的说明。  
  
22. 单击“ **保存**”。  

## <a name="see-also"></a>另请参阅

[数据警报设计器](../reporting-services/data-alert-designer.md)   
[向管理员提出警报的数据警报管理器](../reporting-services/data-alert-manager-for-alerting-administrators.md)   
[Reporting Services 数据警报](../reporting-services/reporting-services-data-alerts.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
