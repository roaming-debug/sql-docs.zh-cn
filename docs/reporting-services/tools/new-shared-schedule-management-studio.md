---
title: 新建共享计划 (Management Studio) | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 中“新建计划”页上的选项，创建新的共享计划来运行已发布的报表和订阅。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
f1_keywords:
- sql13.swb.reportserver.newschedule.f1
ms.assetid: b2c69586-d98e-4933-827d-f5e6c15c5203
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2ec7ca644642125280bdc2f6821faec2b511a45d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596474"
---
# <a name="new-shared-schedule-management-studio"></a>新建共享计划 (Management Studio)
  使用此页可以创建一个共享计划来运行已发布的报表和订阅。 共享计划可以用于替代报表特定计划或订阅特定计划。 集中的计划信息与能够暂停和恢复计划操作的功能是共享计划区别于项特定计划的两个关键功能。  
  
 在一个计划中无法支持所有形式的频率组合。 例如，如果要在每周五的中午 12:00 和下午 4:00 运行报表，那么您必须创建两个每日计划，并指定运行日期为星期五，一个计划的开始时间为中午 12:00， 另一个计划的开始时间为下午 4:00。  
  
 计划处理过程以承载和处理计划的报表服务器的本地时间为准。  
  
 若要打开此页，请启动 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]，连接到报表服务器，右键单击“共享计划”，然后选择“新建计划”。 若要保存计划，必须运行 SQL Server 代理服务。  
  
> [!NOTE]  
>  并非在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的每个版本中均提供此功能。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2012 各个版本支持的功能](/previous-versions/sql/sql-server-2012/cc645993(v=sql.110)) (https://go.microsoft.com/fwlink/?linkid=232473) 。  
  
## <a name="options"></a>选项  
 **名称**  
 键入共享计划的名称。 当用户为报表和订阅选择共享计划时，此名称会出现在下拉列表中。 请确保提供能够很方便地适合列表并能够轻松地区分不同共享计划的描述性名称。 名称必须至少包含一个字母数字字符。 还可以包含空格和某些符号。 在指定名称时请不要使用以下字符：  
  
 ; ? : \@ & = + , $ / * < >  
  
 " /  
  
 **运行此计划的开始日期**  
 指定此计划的开始日期。  
  
 **此计划的结束日期**  
 指定此计划的过期日期。  
  
 类型  
 指定重复执行模式是主要基于小时、天、周还是月。  
  
 **小时(重复执行模式)**  
 选择相应的选项，以小时为间隔运行计划的操作（例如，每 6 小时运行一次报表）。 可以按小时和分钟指定时间间隔。  
  
 **天(重复执行模式)**  
 选择相应的选项，以天为间隔运行计划的操作（例如，每 2 天运行一次报表）。 可以按天指定时间间隔，并指定希望计划运行的具体时间（小时和分钟）。  
  
 **周(重复执行模式)**  
 选择相应的选项，以周为间隔运行计划的操作或者需要按周重复执行操作（例如，每隔一周运行一次报表）。 可以指定每周计划，并指定希望计划运行的具体日期和时间（天、小时和分钟）。  
  
 **月(重复执行模式)**  
 选择相应的选项，以月为间隔运行计划的操作或者需要按月重复执行操作。 可以指定每月计划，并指定希望计划运行的具体日期和时间（天、小时和分钟）。 可以在计划中忽略特定的月份。  
  
 **一次**  
 选择此选项可以创建只在特定日期和时间运行一次的计划。  
  
## <a name="see-also"></a>另请参阅  
 [Management Studio 中报表服务器的 F1 帮助](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)   
 [在 Management Studio 中连接到报表服务器](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [创建、修改和删除计划](../../reporting-services/subscriptions/create-modify-and-delete-schedules.md)   
 [“计划”](../../reporting-services/subscriptions/schedules.md)   
 [Management Studio 中报表服务器的 F1 帮助](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)  
  
