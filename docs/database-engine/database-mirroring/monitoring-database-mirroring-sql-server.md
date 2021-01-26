---
title: 监视数据库镜像 (SQL Server) | Microsoft Docs
description: 了解数据库镜像监视器和系统存储过程，并学习数据库镜像监视的工作原理（包括数据库镜像监视器作业）。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- monitoring [SQL Server], database mirroring
- database mirroring [SQL Server], monitoring
ms.assetid: a7b1b9b0-7c19-4acc-9de3-3a7c5e70694d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ece89606626172d50ba656a6545237af2ec761fe
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783398"
---
# <a name="monitoring-database-mirroring-sql-server"></a>监视数据库镜像 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本节介绍数据库镜像监视器和 **sp_dbmmonitor** 系统存储过程，说明数据库镜像监视的工作方式（包括“数据库镜像监视器作业”），并概括介绍可以监视的有关数据库镜像会话的信息。 此外，本节还介绍如何为一组预定义数据库镜像事件定义警告阈值，以及如何设置有关任意数据库镜像事件的警报。  
  
 可以在镜像会话期间监视镜像数据库，以验证数据是否流动以及流动的情况。 若要对服务器实例上的一个或多个镜像数据库进行监视设置和管理，可以使用数据库镜像监视器或 **sp_dbmmonitor** 系统存储过程。  
  
 数据库镜像监视作业（ **“数据库镜像监视器作业”** ）在后台独立于数据库镜像监视器运行。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理定期调用 **“数据库镜像监视器作业”** ，默认为每分钟调用一次；而作业将调用更新镜像状态的存储过程。 如果使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 启动镜像会话，将自动创建 **“数据库镜像监视器作业”** 。 但是，如果仅使用 ALTER DATABASE *<database_name>* SET PARTNER 来启动镜像，则必须通过运行存储过程来创建作业。  
  
 **本主题内容：**  
  
-   [监视镜像状态](#MonitoringStatus)  
  
-   [有关镜像数据库的其他信息源](#AdditionalSources)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="monitoring-mirroring-status"></a><a name="MonitoringStatus"></a> 监视镜像状态  
 若要对服务器实例上一个或多个镜像数据库进行监视设置和管理，可以使用数据库镜像监视器或 **dbmmonitor** 系统存储过程。 可以在镜像会话期间监视镜像数据库，以验证数据是否流动以及流动的情况。  
  
 具体而言，监视镜像数据库可以：  
  
-   验证镜像是否正在运行。  
  
     基本状况包括了解这两个服务器实例是否正常运行，服务器是否已连接，以及是否将日志从主体服务器移至镜像服务器。  
  
-   确定镜像数据库是否与主体数据库保持同步。  
  
     在高性能模式下，主体服务器可能会积压大量仍需发送到镜像服务器的未发送日志记录。 而且在任意运行模式下，镜像服务器也有可能积压大量已写入日志文件但仍需在镜像数据库中进行还原的未还原日志记录。  
  
-   确定在高性能模式下，当主体服务器实例变得不可用时所丢失的数据量。  
  
     可以通过查看未发送的事务日志量（如果有）以及在主体服务器上提交丢失事务的时间间隔，来确定数据的丢失量。  
  
-   将当前性能与过去性能进行比较。  
  
     出现问题时，数据库管理员可以查看镜像性能的历史记录来帮助了解当前状态。 通过查看历史记录，用户可以检测性能走向，识别性能问题的模式（例如，一天当中网络变慢或进入日志中的命令数变得异常庞大的时间）。  
  
-   解决镜像伙伴之间数据流减小的问题。  
  
-   设置关键绩效指标的警告阈值。  
  
     如果新状态行中的值超过阈值，则系统便会向 Windows 事件日志发送提示性事件。 系统管理员可以随后根据这些事件手动配置警报。 有关详细信息，请参阅 [使用镜像性能度量的警告阈值和警报 (SQL Server)](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md)。  
  
###  <a name="tools-for-monitoring-database-mirroring-status"></a><a name="tools_for_monitoring_dbm_status"></a> 数据库镜像状态监视工具  
 可以使用数据库镜像监视器或 **sp_dbmmonitorresults** 系统存储过程来监视镜像状态。 两个系统管理员（即 sysadmin 固定服务器角色成员以及在 **msdb** 数据库中，由系统管理员添加到 **dbm_monitor** 固定数据库角色的用户）均可使用这些工具监视本地服务器实例上任何镜像数据库中的数据库镜像。 使用上述任意一种工具时，系统管理员还可以手动刷新镜像状态。  
  
> [!NOTE]  
>  系统管理员还可以配置并查看关键绩效指标的警告阈值。 有关详细信息，请参阅 [使用镜像性能度量的警告阈值和警报 (SQL Server)](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md)。  
  
-   数据库镜像监视器  
  
     数据库镜像监视器是一个图形用户界面工具，系统管理员可以使用此工具查看和更新状态，配置多个关键绩效指标的警告阈值。 **dbm_monitor** 固定数据库角色成员还可以使用数据库镜像监视器查看镜像状态表中的最新行，但是这些成员不能更新状态表。  
  
     监视器显示在 **“状态”** 选项卡式页面上选择的数据库的状态（包括性能指标）。 该页的内容来自主体和镜像服务器实例。 通过与主体服务器实例和镜像服务器实例的单独连接收集状态时，会异步填充该页。 监视器每隔 30 秒便会尝试更新一次状态表。 只有当状态表在 15 秒内没有更新，并且用户是 **sysadmin** 固定服务器角色的成员时，更新才能成功。 有关 **“状态”** 页中报告的信息摘要，请参阅本主题后面的“ [数据库镜像监视器显示的状态](#perf_metrics_of_dbm_monitor)”部分。  
  
     有关数据库镜像监视器界面的介绍，请参阅 [Database Mirroring Monitor Overview](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md)。 有关启动数据库镜像监视器的信息，请参阅 [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)。  
  
-   系统存储过程  
  
     还可以通过运行 **sp_dbmmonitorresults** 系统存储过程来检索或更新当前的状态。 您还可以使用其他 dbmmonitor 存储过程在服务器实例上设置监视、更改监视参数、查看当前更新持续时间以及删除监视。  
  
     下表介绍了管理和使用数据库镜像监视的存储过程，它们独立于数据库镜像监视器工作。  
  
    |过程|说明|  
    |---------------|-----------------|  
    |[sp_dbmmonitoraddmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)|创建定期更新服务器实例上每个镜像数据库的状态信息的作业。|  
    |[sp_dbmmonitorchangemonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)|更改数据库镜像监视参数的值。|  
    |[sp_dbmmonitorhelpmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)|返回当前更新持续时间。|  
    |[sp_dbmmonitorresults](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)|返回所监视数据库的状态行，使您能够选择此过程是否预先获取最新的状态。|  
    |[sp_dbmmonitordropmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)|停止并删除服务器实例上所有数据库的镜像监视器作业。|  
  
     **dbmmonitor** 系统存储过程可以用作数据库镜像监视器的附加补充。 例如，即使使用 **sp_dbmmonitoraddmonitoring** 配置监视，也可以使用数据库镜像监视器查看状态。  
  
### <a name="how-monitoring-works"></a>监视的工作原理  
 本部分介绍数据库镜像状态表、数据库镜像监视器作业和监视器，并介绍用户如何监视数据库镜像状态以及如何删除镜像作业。  
  
#### <a name="database-mirroring-status-table"></a>数据库镜像状态表  
 数据库镜像状态存储在 **msdb** 数据库内的一个内部、未记录的数据库镜像状态表中。 在服务器实例上首次更新镜像状态时，便会自动创建此状态表。  
  
 状态表可以自动更新，也可以由系统管理员手动更新，但最低更新间隔为 15 秒。 将最低更新间隔设置为 15 秒可以防止服务器实例因状态请求而导致重载。  
  
 状态表可以通过数据库镜像监视器和数据库镜像监视器作业（如果正在运行）进行自动更新。 默认情况下，“数据库镜像监视器作业”将每分钟更新一次状态表（系统管理员可以将更新持续时间指定为 1 至 120 分钟之间的一个值）。 相反，数据库镜像监视器每隔 30 秒自动更新一次状态表。 对于这些更新，“数据库镜像监视器作业”和数据库镜像监视器将调用 **sp_dbmmonitorupdate**。  
  
 **sp_dbmmonitorupdate** 第一次运行时，它将在 **msdb** 数据库中创建“数据库镜像状态”表和 **dbm_monitor** 固定数据库角色。 **sp_dbmmonitorupdate** 通常通过针对服务器实例上的每个镜像数据库将新行插入状态表来更新镜像状态；有关详细信息，请参阅本主题后面的“数据库镜像状态表”。 此过程还会计算新行中的性能指标并截断保留时间长于当前保持期（默认为 7 天）的行。 有关详细信息，请参阅 [sp_dbmmonitorupdate (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)。  
  
> [!NOTE]  
>  除非数据库镜像监视器当前正在由 **sysadmin** 固定服务器角色成员使用，否则，只有在具有“数据库镜像监视器作业”并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理正在运行时，才能自动更新状态表。  
  
#### <a name="database-mirroring-monitor-job"></a>数据库镜像监视器作业  
 数据库镜像监视作业（ **“数据库镜像监视器作业”** ）独立于数据库镜像监视器运行。 仅当使用 **启动镜像会话时，才能自动创建** “数据库镜像监视器作业” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。 如果始终使用 ALTER DATABASE *database_name* SET PARTNER 命令开始镜像，则仅当系统管理员运行 **sp_dbmmonitoraddmonitoring** 存储过程时，该作业才存在。  
  
 创建 **“数据库镜像监视器作业”** 之后，如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理正在运行，则默认情况下，每分钟调用一次作业。 然后，作业会调用 **sp_dbmmonitorupdate** 系统存储过程。  
  
 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理每分钟调用一次“数据库镜像监视器作业”，而作业随即调用 sp_dbmmonitorupdate 以更新状态表 。 系统管理员可以使用 **sp_dbmmonitorchangemonitoring** 系统存储过程更改更新持续时间，他们还可以使用 **sp_dbmmonitorchangemonitoring** 系统存储过程查看当前的更新持续时间。 有关详细信息，请参阅 [sp_dbmmonitoraddmonitoring (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md) 和 [sp_dbmmonitorchangemonitoring (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)。  
  
#### <a name="monitoring-database-mirroring-status-by-system-administrators"></a>监视数据库镜像状态（由系统管理员执行）  
 **sysadmin** 固定服务器角色成员可以查看和更新状态表。  
  
-   使用数据库镜像监视器  
  
     系统管理员可以使用数据库镜像监视器手动刷新 **“状态”** 页、导航树或 **“历史记录”** 页。 如果状态表在前 15 秒内没有更新，则此操作还会更新状态表。  
  
     若要查看给定服务器实例上镜像状态的历史记录，系统管理员还可以针对服务器实例单击“历史记录”按钮（位于“状态”页上）。 将在 **“数据库镜像历史记录”** 对话框中显示历史记录。 在此对话框中，系统管理员可以查看服务器实例状态表中的部分或全部行。  
  
     有关 **“状态”** 页指标的信息，请参阅本主题后面的“数据库镜像监视器显示的性能指标”。  
  
-   使用 **sp_dbmmonitorresults**  
  
     系统管理员可以使用 **sp_dbmmonitorresults** 系统存储过程查看状态表，如果此状态表在前 15 秒内没有更新，则还可以选择对其进行更新。 此过程将调用 **sp_dbmmonitorupdate** 过程并返回一个或多个历史记录行，具体取决于过程调用中的请求量。 有关其结果集中状态的信息，请参阅 [sp_dbmmonitorresults (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)。  
  
#### <a name="monitoring-database-mirroring-status-by-dbm_monitor-members"></a>监视数据库镜像状态（由 dbm_monitor 成员执行）  
 如上所述，当 **sp_dbmmonitorupdate** 首次运行时，便会在 **msdb** 数据库中创建 **dbm_monitor** 固定数据库角色。 **dbm_monitor** 固定数据库角色成员可以使用数据库镜像监视器或 **sp_dbmmonitorresults** 存储过程查看现有的镜像状态。 但是这些用户不能更新状态表。 若要了解所显示的状态的保留时间，用户可以在“状态”页上的“主体日志 (\<time>)”和“镜像日志 (\<time>)”标签上查看时间  。  
  
 **dbm_monitor** 固定数据库角色成员使用“数据库镜像监视器作业”定期更新状态表。 如果作业不存在，或者 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理已停止，则状态便会迅速变旧，并且不再反映镜像会话的配置。 例如，在一次故障转移之后，伙伴可能会分享相同的角色（主体或镜像）。或者，当前主体服务器可能显示为镜像，而当前的镜像服务器显示为主体。  
  
#### <a name="dropping-the-database-mirroring-monitor-job"></a>删除数据库镜像监视器作业  
 数据库镜像监视器作业（ **“数据库镜像监视器作业”** ）在删除之前将一直保留。 必须由系统管理员对监视作业进行管理。 若要删除“数据库镜像监视器作业”，请使用 **sp_dbmmonitordropmonitoring**。 有关详细信息，请参阅 [sp_dbmmonitordropmonitoring (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)。  
  
###  <a name="status-displayed-by-the-database-mirroring-monitor"></a><a name="perf_metrics_of_dbm_monitor"></a> 数据库镜像监视器显示的状态  
 数据库镜像监视器的 **“状态”** 页描述了镜像伙伴以及镜像会话的状态。 状态信息包括性能指标（如事务日志的状态）以及在会话没有同步时，有助于当前对完成故障转移所需时间和潜在数据丢失进行评估的其他信息。 此外， **“状态”** 页还概略显示有关镜像会话的状态和信息。  
  
> [!NOTE]  
>  有关数据库镜像监视器和“状态”页的说明，请参阅本主题前面的[数据库镜像状态监视工具](#tools_for_monitoring_dbm_status)。  
  
 下面将介绍上述各内容的概要。  
  
#### <a name="partners"></a>合作伙伴  
 **“状态”** 页显示每个伙伴的下列信息：  
  
-   服务器实例  
  
     在 **“状态”** 行显示状态的服务器实例的名称。  
  
-   当前角色  
  
     服务器实例的当前角色。 可能的状态包括：  
  
    -   主体  
  
    -   镜像  
  
-   镜像状态  
  
     可能的状态包括：  
  
    -   未知  
  
    -   正在同步  
  
    -   已同步  
  
    -   Suspended  
  
    -   已断开连接  
  
-   见证服务器连接  
  
     见证服务器的连接状态。 可能的状态包括：  
  
    -   未知  
  
    -   连续  
  
    -   已断开连接。  
  
#### <a name="log-on-the-principal-server"></a>主体服务器上的日志  
 **“状态”** 页显示主体服务器上的日志自所示时间起的下列状态信息：  
  
-   未发送日志  
  
     发送队列中处于等待状态的日志量 (KB)。  
  
-   最早的未发送事务  
  
     发送队列中最早的未发送事务的保留时间。 事务保留时间指示在将事务发送到镜像服务器实例之前将其保留的分钟数。 该值有助于测量数据丢失的可能性（以时间计）。  
  
-   发送日志所需的时间(估计值)  
  
     根据当前发送速率，估计主体服务器实例将当前位于发送队列的日志发送到镜像服务器实例所需的分钟数。 发送日志所需的实际时间将受传入事务速率的影响，而此速率的变化非常大。 但是，可以使用“发送日志所需的时间（估计值）”值粗略估计手动故障转移所需的时间。  
  
-   当前发送速率  
  
     事务发送到镜像服务器实例的速率（以 KB/秒计）。  
  
-   新事务的当前速率  
  
     传入事务进入主体日志的速率（以 KB/秒计）。 若要确定镜像是落后、保持同步、还是正在追赶，请将该值与 **“发送日志所需的时间(估计值)”** 值进行比较。  
  
#### <a name="log-on-the-mirror-server"></a>镜像服务器上的日志  
 **“状态”** 页显示镜像服务器上的日志自所示时间起的下列状态信息：  
  
-   未还原日志  
  
     重做队列中处于等待状态的日志量(以 KB 计)。  
  
-   还原日志所需的时间(估计值)  
  
     将当前在重做队列中的日志应用于镜像数据库所需的近似分钟数。  
  
-   当前还原速率  
  
     将事务还原到镜像数据库的速率（以 KB/秒计）。  
  
#### <a name="mirroring-session"></a>镜像会话  
 此外， **“状态”** 页还显示有关镜像会话的下列信息：  
  
-   镜像提交开销  
  
     每个事务的平均延迟时间（毫秒），仅与高安全性模式相关。 此延迟是主体服务器实例等待镜像服务器实例将事务日志记录写入重做队列时，所发生的开销量。  
  
-   发送和还原所有当前日志的所需时间(估计值)  
  
     发送在主体服务器上已提交的所有未发送日志，以及还原重做队列中当前存在的所有日志所需的估计时间。 此估计时间值可能小于“发送日志所需的时间(估计值)”和“还原日志所需的时间(估计值)”这两个字段值的总和，因为发送和还原操作可以并行执行。  
  
-   见证服务器地址  
  
     见证服务器实例的网络地址。 有关此地址格式的信息，请参阅[指定服务器网络地址（数据库镜像）](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)。  
  
-   运行模式  
  
     数据库镜像会话的运行模式：  
  
    -   高性能(异步)  
  
    -   不带自动故障转移功能的高安全(同步)  
  
    -   带自动故障转移功能的高安全(同步)  
  
##  <a name="additional-sources-of-information-about-a-mirrored-database"></a><a name="AdditionalSources"></a> 有关镜像数据库的其他信息源  
 除了使用数据库镜像监视器和 dbmmonitor 存储过程监视镜像数据库并对所监视的性能变量设置警报之外， [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 还提供了用于数据库镜像的目录视图、性能计数器和事件通知。  
  
 **本节内容：**  
  
-   [数据库镜像元数据](#DbmMetadata)  
  
-   [数据库镜像性能计数器](#DbmPerfCounters)  
  
-   [数据库镜像事件通知](#DbmEventNotif)  
  
###  <a name="database-mirroring-metadata"></a><a name="DbmMetadata"></a> 数据库镜像元数据  
 通过下列目录视图或动态管理视图公开的元数据对每个数据库镜像会话进行了说明：  
  
-   **sys.database_mirroring**  
  
     此视图显示一个服务器实例中每个镜像数据库的数据库镜像元数据。 有关详细信息，请参阅 [sys.database_mirroring (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)。  
  
-   **sys.database_mirroring_endpoints**  
  
     **sys.database_mirroring_endpoints** 目录视图显示有关服务器实例的数据库镜像端点的信息。 有关详细信息，请参阅 [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)。  
  
-   **sys.database_mirroring_witnesses**  
  
     此视图显示服务器实例为见证服务器的每个会话的数据库镜像元数据。 有关详细信息，请参阅 [sys.database_mirroring_witnesses (Transact-SQL)](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)。  
  
-   **sys.dm_db_mirroring_connections**  
  
     此动态管理视图为每个数据库镜像网络连接返回一行。  
  
     有关详细信息，请参阅 [sys.dm_db_mirroring_connections (Transact-SQL)](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md)。  
  
###  <a name="database-mirroring-performance-counters"></a><a name="DbmPerfCounters"></a> 数据库镜像性能计数器  
 使用性能计数器可以监视数据库镜像性能。 例如，可以检查 **Transaction Delay** 计数器以确定数据库镜像是否影响主体服务器的性能，可以检查 **Redo Queue** 和 **Log Send Queue** 计数器以确定镜像数据库与主体数据库之间保持同步的情况。 还可以检查 **Log Bytes Sent/sec** 计数器以监视每秒发送的日志量。  
  
 在任一伙伴的性能监视器中，性能计数器可用于数据库镜像性能对象 (**SQLServer:Database Mirroring**)。 有关详细信息，请参阅 [SQL Server, Database Mirroring Object](../../relational-databases/performance-monitor/sql-server-database-mirroring-object.md)。  
  
 **启动性能监视器**  
  
-   [启动系统监视器 (Windows)](../../relational-databases/performance/start-system-monitor-windows.md)  
  
###  <a name="database-mirroring-event-notifications"></a><a name="DbmEventNotif"></a> 数据库镜像事件通知  
 事件通知是特殊类型的数据库对象。 执行事件通知可响应各种 Transact-SQL 数据定义语言 (DDL) 语句和 SQL 跟踪事件，并将有关服务器事件和数据库事件的信息发送到 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 服务。  
  
 下列事件可用于数据库镜像：  
  
-   **Database Mirroring State Change** 事件类  
  
     这指示镜像数据库镜像状态的更改时间。 有关详细信息，请参阅 [Database Mirroring State Change Event Class](../../relational-databases/event-classes/database-mirroring-state-change-event-class.md)。  
  
-   **Audit Database Mirroring Login** 事件类  
  
     这报告与数据库镜像传输安全性相关的审核消息。 有关详细信息，请参阅 [Audit Database Mirroring Login Event Class](../../relational-databases/event-classes/audit-database-mirroring-login-event-class.md)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [使用镜像性能度量的警告阈值和警报 (SQL Server)](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md)  
  
-   [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
-   [查看镜像数据库的状态 (SQL Server Management Studio)](../../database-engine/database-mirroring/view-the-state-of-a-mirrored-database-sql-server-management-studio.md)  
  
 **存储过程**  
  
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
 [WMI Provider for Server Events 的概念](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md)  
  
  
