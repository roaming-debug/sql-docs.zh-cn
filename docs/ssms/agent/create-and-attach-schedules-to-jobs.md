---
description: 创建计划并将计划附加到作业
title: 创建计划并将计划附加到作业
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server]
- scheduling jobs [SQL Server]
- jobs [SQL Server], scheduling
- CPU [SQL Server], idle conditions
- automatic job processing
- SQL Server Agent jobs, scheduling
- idle time [SQL Server]
ms.assetid: 079c2984-0052-4a37-a2b8-4ece56e6b6b5
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 1cb7585003f74bd3dc3e0bf12fefac97a8fe2dcb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477078"
---
# <a name="create-and-attach-schedules-to-jobs"></a>创建计划并将计划附加到作业
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

计划 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业就是定义使作业在没有用户交互的情况下开始运行的条件。 通过为作业创建新计划或将现有计划附加到作业可以将作业计划为自动运行。  
  
以下是两种用来创建计划的方法：  
  
-   在创建作业的时候创建计划。  
  
-   在对象资源管理器中创建计划。  
  
创建计划后，可将该计划附加到多个作业，即使该计划是为特定作业创建的也是如此。 还可以从作业分离计划。  
  
计划可以基于时间，也可以基于事件。 例如，可以计划在以下时间运行作业：  
  
-   每当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理启动时。  
  
-   每当计算机的 CPU 使用率处于定义的空闲状态水平时。  
  
-   在特定日期和时间运行一次。  
  
-   按重复的计划运行。  
  
除了创建作业计划之外，还可以创建警报，通过运行作业来响应事件。  
  
> [!NOTE]  
> 一次只能运行一个作业实例。 如果在作业按计划运行时尝试手动运行该作业， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理将拒绝该请求。  
  
若要阻止已计划的作业运行，必须执行以下操作之一：  
  
-   禁用计划。  
  
-   禁用作业。  
  
-   从作业分离计划。  
  
-   停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服务。  
  
-   删除计划。  
  
即使计划未启用，作业仍可以为响应警报而运行，或者由用户手动运行。 如果作业计划未启用，则任何使用该计划的作业都不会启用该计划。  
  
必须显式重新启用已禁用的计划。 编辑计划不会自动重新启用计划。  
  
## <a name="scheduling-start-dates"></a>计划开始日期  
计划的开始日期必须不早于 19900101。  
  
将计划附加到作业时，应查看计划用于首次运行作业的开始日期。 开始日期取决于将计划附加到作业时的日期和时间。 例如，创建的计划为星期一的上午 8:00 运行，并且隔周运行。 如果您在 2008 年 3 月 3 日星期一上午 10:00 创建一个作业， 则计划开始日期是 2008 年 3 月 17 日星期一。 如果您在 2008 年 3 月 4 日星期二创建另一个作业，则计划开始日期是 2008 年 3 月 10 日星期一。  
  
在将计划附加到作业后可更改计划的开始日期。  
  
## <a name="cpu-idle-schedules"></a>CPU 空闲计划  
若要最大限度地利用 CPU 资源，可以为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理定义一个 CPU 空闲条件。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理使用 CPU 空闲条件设置来确定运行作业的最佳时间。 例如，可计划作业，使其在 CPU 空闲时间和业务量较低时重新生成索引。  
  
将作业定义为在 CPU 空闲时间运行之前，应确定正常处理过程中 CPU 的负荷。 若要执行此操作，请使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 或性能监视器来监视服务器流量并收集统计信息。 然后，利用收集到的信息设置 CPU 空闲时间百分比和持续时间。  
  
将 CPU 空闲条件定义为一个百分比，在该百分比以下，CPU 使用率必须持续指定的时间。 然后，设置持续时间长度。 如果 CPU 使用率在指定时间内低于指定的百分比，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理将启动具有 CPU 空闲时间计划的所有作业。 有关使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 或性能监视器来监视 CPU 使用率的详细信息，请参阅 [监视 CPU 使用率](../../relational-databases/performance-monitor/monitor-cpu-usage.md)。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|说明|主题|  
|-|-|  
|介绍如何为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业创建计划。|[Create a Schedule](../../ssms/agent/create-a-schedule.md)|  
|介绍如何安排 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业计划。|[安排作业计划](../../ssms/agent/schedule-a-job.md)|  
|说明如何定义服务器的 CPU 空闲条件。|[设置 CPU 空闲时间和持续时间 (SQL Server Management Studio)](../../ssms/agent/set-cpu-idle-time-and-duration-sql-server-management-studio.md)|  
  
## <a name="see-also"></a>另请参阅  
[sp_help_jobschedule](../../relational-databases/system-stored-procedures/sp-help-jobschedule-transact-sql.md)  
[sysjobschedules](../../relational-databases/system-tables/dbo-sysjobschedules-transact-sql.md)  
