---
title: 使用快照（web 门户）| Microsoft Docs
description: 创建缓存快照，以便更快地加载特定执行属性。 还可以使用历史记录快照捕获时间点。
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 9ae20556-e243-4a60-b076-9fd9e82c7355
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c17541ff08f136b3c8c26418117df63c71b9195b
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243796"
---
# <a name="working-with-snapshots-web-portal"></a>使用快照（web 门户）

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

可以通过依次选择报表的省略号 (…)、“管理”和“缓存”或“历史记录快照”，来控制是否为报表创建快照   。  
  
> [!NOTE]
> 需要启动 SQL Server 代理服务。  
   
可以创建缓存快照，以便可以更快地加载特定执行属性。 还可以使用历史记录快照捕获时间点。  
  
## <a name="creating-a-cache-snapshot"></a>创建缓存快照  
  
可以通过执行以下操作来创建快照。  
  
![显示“编辑公司销售额”对话框的“缓存”屏幕的屏幕截图，其中三个红圈中的数字为 1、2 和 3，表示快照下的三个步骤。](../reporting-services/media/ssrswebportal-report-caching4.png)  
  
1.  在“缓存”页上，选择“始终对预先生成的快照运行此报表”，启用用于创建快照的选项 。  
  
2.  如果要计划定期执行快照，请选择“按计划创建缓存快照”。 然后可以使用共享计划，或定义自定义计划以刷新快照。  
  
3.  如果要立即创建缓存快照，请选择“当我在此页面上单击‘应用’时创建缓存快照”。 如果仅选择此选项，则不会刷新快照。  
  
## <a name="create-modify-and-delete-history-snapshots"></a>创建、修改和删除历史记录快照  
  
若要使用历史记录快照，请管理报表并选择“历史记录快照”。  
  
使用“历史记录快照”页可以查看一段时间中生成并存储的报表快照。 根据报表服务器上设置的选项，历史记录可能只包含较新的快照。  
  
报表历史记录总是显示在所源于的报表的上下文中。 您不能一起查看报表服务器中所有报表的历史记录。  
  
若要生成历史记录快照，报表必须能够以无人参与的方式运行（也就是说，报表必须使用存储的凭据；参数化报表必须包含所有参数的默认参数值）。 可以手动或以计划操作方式生成报表历史记录。 报表的历史记录属性决定报表历史记录的创建方式。  
  
![显示“编辑公司销售额”对话框的“历史记录快照”屏幕的屏幕截图，其中三个红色圆圈中的数字为 1、2 和 3，表示快照下的三个步骤。](../reporting-services/media/ssrswebportal-historysnapshots1.png)  
   
1.  若要创建历史记录快照，请选择“+ 新建历史记录快照”。 这会处理报表并向列表添加条目。  
  
2.  可以转到用于定义计划和保留策略的设置。  
  
3.  可以选择历史记录快照以查看它。 报表历史记录中显示的快照只能通过创建的日期和时间来区分。 无法通过直观方式判断出某个快照是为响应计划而生成的还是为响应某个手动操作而生成的。  
  
### <a name="schedule-and-settings"></a>计划和设置  
  
选择“计划和设置”将提供附加选项，用于计划和控制保持已创建快照的保留期。  
  
![显示“计划和高级设置”部分的屏幕截图。](../reporting-services/media/ssrswebportal-historysnapshots2.png)  
   
可以选择创建计划，以便创建快照。 还可以阻止其他人创建新快照。 取消选中“允许用户手动创建快照”会禁用“+ 新建快照历史记录”按钮 。  
  
还可以定义要用于保留快照的方式。  
  
**同时将缓存快照保存在报表历史记录中**  
  
选择此项可以根据报表执行属性将生成的报表快照添加到报表历史记录中。 您可以设置报表执行属性以便从生成的快照运行报表。 设置此报表历史记录属性后，您可以将一段时间内生成的所有报表快照的副本放置在报表历史记录中，以跟踪这些报表快照。

## <a name="next-steps"></a>后续步骤

[Web 门户](../reporting-services/web-portal-ssrs-native-mode.md)  
[使用分页报表](working-with-paginated-reports-web-portal.md)  
[使用共享数据集](../reporting-services/work-with-shared-datasets-web-portal.md)

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
