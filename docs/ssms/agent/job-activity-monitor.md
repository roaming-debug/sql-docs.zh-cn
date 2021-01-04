---
description: 作业活动监视器
title: 作业活动监视器
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.jobactivitymonitor.alljobs.f1
- SQL13.SWB.ACTIVITYMON.F1
ms.assetid: 11f2182c-5f71-46f8-8d2b-74f0fc48f2d6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 96a19ea760409d354390c171965f494f59cdff42
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477028"
---
# <a name="job-activity-monitor"></a>作业活动监视器
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

使用此页可以查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业的当前活动。 单击“筛选器”可以限制显示的作业。 “代理作业活动”网格是只读的。 单击列标题可以对网格进行排序。 若要修改作业，请双击该作业以打开“作业属性”对话框。 右键单击网格中的作业，可以启动作业以运行其所有作业步骤，在特定作业步骤处启动作业，禁用或启用作业，刷新作业，删除作业，查看作业的历史记录以及查看作业属性。 单击“刷新”可以将网格更新为当前信息。  
  
## <a name="options"></a>选项  
**名称**  
作业的名称。  
  
**Enabled**  
说明作业是已启用（“是”）还是未启用（“否”）。  
  
**状态** _  
作业的当前状态。  
  
_ *上次运行结果**  
作业上次运行时的状态。  
  
**上次运行时间**  
上次使用服务器的本地日期和时间运行作业的日期和时间。  
  
**下次运行时间** _  
计划下次使用服务器的本地日期和时间运行作业的日期和时间。  
  
_ *类别**  
分配给作业的作业类别。  
  
**可运行**  
在作业可以运行时为“是”；在作业无法运行时为“否”。 如果作业没有步骤或没有目标服务器，则无法运行该作业。  
  
**计划**  
在作业已分配给作业计划时为“是”；在作业没有计划时为“否”。  
  
*只有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sysadmin 固定服务器角色和服务器管理员组的成员才能看到此列中的值。 SQLAgentOperatorRole 角色的成员不能看到此列中的值。  
  
#### <a name="to-open-the-job-activity-monitor"></a>打开作业活动监视器  
  
-   在“对象资源管理器”中，展开服务器，展开“SQL Server 代理”，右键单击“作业活动监视器”，再单击“查看作业活动”。  
  
## <a name="see-also"></a>另请参阅  
[监视作业活动](../../ssms/agent/monitor-job-activity.md)  
