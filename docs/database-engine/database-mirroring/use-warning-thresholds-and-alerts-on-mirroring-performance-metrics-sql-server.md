---
title: 为数据库镜像性能指标配置警报
description: '提供为数据库镜像使用的性能指标配置警告阈值和警报的指南。 '
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- monitoring database mirroring [SQL Server]
- thresholds [SQL Server]
- database mirroring [SQL Server], managing in SQL Server Management Studio
- alerts [SQL Server], database mirroring
- database mirroring [SQL Server], monitoring
- warnings [database mirroring]
ms.assetid: 8cdd1515-0bd7-4f8c-a7fc-a33b575e20f6
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c323c5915752e68505e7e1e512843ae03c8197e4
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643481"
---
# <a name="use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server"></a>使用镜像性能度量的警告阈值和警报 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题包含有关一些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件的信息，可以为这些事件配置和管理用于数据库镜像的警告阈值。 可以使用数据库镜像监视器或 **sp_dbmmonitorchangealert**、 **sp_dbmmonitorhelpalert** 和 **sp_dbmmonitordropalert** 存储过程。 本主题还包含有关对数据库镜像事件配置警报的信息。  
  
 针对镜像数据库建立监视之后，系统管理员可以为多个关键绩效指标配置警告阈值。 同时，管理员还可以为这些数据库镜像事件和其他数据库镜像事件配置警报。  
  
  
##  <a name="performance-metrics-and-warning-thresholds"></a><a name="PerfMetricsAndWarningThresholds"></a> 性能指标和警告阈值  
 下表列出可为其配置警告的性能指标，说明相应的警告阈值并列出相应的数据库镜像监视器标签。  
  
|性能指标|警告阈值|数据库镜像监视器标签|  
|------------------------|-----------------------|--------------------------------------|  
|未发送日志|指定未发送日志达到多少 KB 后，在主体服务器实例上生成一个警告。 此警告有助于根据 KB 度量数据丢失的可能性，尤其与高性能模式相关。 但是，当镜像因伙伴断开连接而暂停或挂起时，该警告也适用于高安全模式。|**如果未发送日志超出了阈值，则发出警告**|  
|未还原日志|指定未还原日志达到多少 KB 后，会在镜像服务器实例上生成一个警告。 此警告可以帮助度量故障转移时间。 “故障转移时间  ”主要包括前一个镜像服务器前滚其重做队列中剩余的任意日志所需的时间，以及一小段额外时间。<br /><br /> 注意：对于自动故障转移，系统识别错误所需的时间与故障转移时间无关。<br /><br /> 有关详细信息，请参阅 [估计在角色切换期间服务的中断（数据库镜像）](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md)。|**如果未还原日志超出了阈值，则发出警告**|  
|最早的未发送事务|指定在主体服务器实例上生成警告之前，发送队列中可以累积的事务的分钟数。 此警告有助于根据时间度量数据丢失的可能性，尤其与高性能模式相关。 但是，当镜像因伙伴断开连接而暂停或挂起时，该警告也适用于高安全模式。|**如果最早的未发送事务的保留时间超出了阈值，则发出警告**|  
|镜像提交开销|指定在主体服务器上生成警告之前，每个事务可允许的平均延迟的毫秒数。 此延迟是主体服务器实例等待镜像服务器实例将事务日志记录写入重做队列时，所发生的开销量。 该值只适用于高安全模式。|**如果镜像提交开销超过了阈值则发出警告**|  
  
 对于上述任何一个性能指标，系统管理员都可以指定镜像数据库的阈值。 有关详细信息，请参阅本主题后面的 [设置和管理警告阈值](#SetUpManageWarningThresholds)。  
  
##  <a name="setting-up-and-managing-warning-thresholds"></a><a name="SetUpManageWarningThresholds"></a> 设置和管理警告阈值  
 系统管理员可以为关键镜像性能指标配置一个或多个警告阈值。 我们建议为伙伴双方都设置给定警告的阈值，以确保即使出现数据库故障转移的情况，警告也会一直保留。 每个伙伴的适当阈值都取决于该伙伴系统的性能。  
  
 可以使用下列任意一项配置和管理警告阈值：  
  
-   数据库镜像监视器  
  
     在数据库镜像监视器中，管理员可以通过选择 **“警告”** 选项卡式页面，同时查看主体服务器实例和镜像服务器实例上选定数据库的当前警告配置。 在此页上，管理员可以打开 **“设置警告阈值”** 对话框以启用和配置一个或多个警告阈值。  
  
     有关数据库镜像监视器界面的介绍，请参阅 [Database Mirroring Monitor Overview](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md)。 有关启动数据库镜像监视器的信息，请参阅 [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)。  
  
-   系统存储过程  
  
     管理员可以使用下面一组系统存储过程，针对伙伴双方的镜像数据库，分别设置和管理警告阈值。  
  
    |过程|说明|  
    |---------------|-----------------|  
    |[sp_dbmmonitorchangealert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)|添加或更改指定镜像性能指标的警告阈值。|  
    |[sp_dbmmonitorhelpalert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)|返回若干个关键数据库镜像监视器性能指标中的一个或所有指标的警告阈值信息。|  
    |[sp_dbmmonitordropalert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)|删除指定性能指标的警告。|  
  
## <a name="performance-threshold-events-sent-to-the-windows-event-log"></a>发送到 Windows 事件日志的性能阈值事件  
 如果为性能指标定义了警告阈值，则在更新状态表时，将针对阈值计算最新的值。 如果已达到阈值，则更新过程“sp_dbmmonitorupdate”会针对指标生成一个信息性事件（“性能阈值事件”），然后将此事件写入 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 事件日志。 下表列出性能阈值事件的 ID。  
  
|性能指标|事件 ID|  
|------------------------|--------------|  
|未发送日志|32042|  
|未还原日志|32043|  
|最早的未发送事务|32040|  
|镜像提交开销|32044|  
  
> [!NOTE]  
>  管理员可以针对这些事件中的任何一个或多个定义警报。 有关详细信息，请参阅本主题后面的 [将警报用于镜像数据库](#UseAlerts)  
>   
>  。  
  
##  <a name="using-alerts-for-a-mirrored-database"></a><a name="UseAlerts"></a> 将警报用于镜像数据库  
 监视镜像数据库的一个重要组成部分就是针对重要的数据库镜像事件配置警报。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可生成下列数据库镜像事件类型：  
  
-   性能阈值事件  
  
     有关详细信息，请参阅本主题前面的“发送到 Windows 事件日志的性能阈值事件”。  
  
-   状态更改事件  
  
     这些事件为 Windows Management Instrumentation (WMI) 事件，在数据库镜像会话的内部状态发生更改时生成。  
  
    > [!NOTE]  
    >  有关详细信息，请参阅 [WMI Provider for Server Events 的概念](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md)。  
  
 系统管理员可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理或其他应用程序（例如 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Operations Manager）针对这些事件配置警报。  
  
 当针对数据库镜像事件定义警报时，我们建议在伙伴双方服务器实例上同时定义警告阈值和警报。 可以在主体服务器或镜像服务器上生成单独的事件，但是每个伙伴都可以随时执行其中任意一种角色。 为了确保警报在故障转移后继续有效，必须同时在伙伴双方上定义警报。  
  
> [!IMPORTANT]  
>  对于所有镜像会话，我们极力建议您将数据库配置为针对任意状态更改事件发送警报。 除非专门通过手动配置更改来实现状态更改，否则会出现危及数据安全的情况。 为了帮助保护数据，请找出状态发生意外更改的原因并予以纠正。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **使用 SQL Server Management Studio 创建警报**  
  
-   [使用错误号创建警报](../../ssms/agent/create-an-alert-using-an-error-number.md)  
  
-   [创建 WMI 事件警报](../../ssms/agent/create-a-wmi-event-alert.md)  
  
 **监视数据库镜像**  
  
-   [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
-   [sp_dbmmonitoraddmonitoring (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorchangealert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)  
  
-   [sp_dbmmonitorchangemonitoring (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)  
  
-   [sp_dbmmonitordropalert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)  
  
-   [sp_dbmmonitordropmonitoring (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorhelpalert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)  
  
-   [sp_dbmmonitorhelpmonitoring (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorresults (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
-   [sp_dbmmonitorupdate (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
  
