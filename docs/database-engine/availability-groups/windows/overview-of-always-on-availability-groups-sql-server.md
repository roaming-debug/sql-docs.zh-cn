---
title: 什么是 Always On 可用性组？
description: 关于用于配置和管理 AlwaysOn 可用性组的核心概念的介绍。
ms.custom: seo-lt-2019
ms.date: 04/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], data movement
- Availability Groups [SQL Server]
ms.assetid: 04fd9d95-4624-420f-a3be-1794309b3a47
author: cawrites
ms.author: chadam
ms.openlocfilehash: b30a3004ccf0d5bafa94ae9c601db287a5eef288
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783314"
---
# <a name="what-is-an-always-on-availability-group"></a>什么是 Always On 可用性组？
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

 本主题介绍用于在 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 中配置和管理一个或多个可用性组的核心 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]概念。 有关可用性组提供的优势的摘要和 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 术语的概述，请参阅 [AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)。  
  
 可用性组支持的复制环境适用于一组离散用户数据库，称为“可用性数据库”   。 可以创建可用性组以实现高可用性 (HA) 或读取缩放。 HA 可用性组是一组共同实现故障转移的数据库。 读取缩放可用性组是一组复制到其他 SQL Server 实例以实现只读工作负荷的数据库。 一个可用性组支持一组主数据库以及一至八组对应的辅助数据库。 辅助数据库 *不是* 备份。 应继续定期备份您的数据库及其事务日志。  
  
> [!TIP]  
>  您可以创建主数据库的任何类型的备份。 也可以创建辅助数据库的日志备份和仅复制完整备份。 有关详细信息，请参阅[活动次要副本：次要副本备份（Always On 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)。   

 每组可用性数据库都由一个“可用性副本”  承载。 有两种类型的可用性副本：一个“主副本”  和一到四个“辅助副本”。 它承载主数据库和一至八个“辅助副本”  ，其中每个副本承载一组辅助数据库，并用作可用性组的潜在故障转移目标。 可用性组在可用性副本级别进行故障转移。 可用性副本仅在数据库级别提供冗余（针对一个可用性组中的该组数据库）。 故障转移不是由诸如因数据文件丢失或事务日志损坏而使数据库成为可疑数据库等数据库问题导致的。  
  
 主副本使主数据库可用于客户端的读写连接。 主副本将每个主数据库的事务日志记录发送到每个辅助数据库。 此过程（称为“数据同步”）在数据库级别运行  。 每个次要副本缓存事务日志记录（“硬化”  日志），然后将它们应用到相应的辅助数据库。 主数据库与每个连接的辅助数据库独立进行数据同步。 因此，一个辅助数据库可以挂起或失败而不会影响其他辅助数据库，一个主数据库可以挂起或失败而不会影响其他主数据库。  
  
 或者，您可以配置一个或多个辅助副本以支持对辅助数据库进行只读访问，并且可以将任何辅助副本配置为允许对辅助数据库进行备份。  

 SQL Server 2017 引入了用于可用性组的两种不同的体系结构。 “AlwaysOn 可用性组”提供高可用性、灾难恢复和读取缩放均衡  。 这些可用性组需要群集管理器。 在 Windows 中，故障转移群集提供群集管理器。 在 Linux 中可以使用 Pacemaker。 另一个体系结构是“读取缩放可用性组”  。 读取缩放可用性组为只读工作负荷提供副本，但不提供高可用性。 读取缩放可用性组中没有群集管理器。 
 
 在 Windows 上为 HA 部署 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 需要 Windows Server 故障转移群集 (WSFC)。 给定可用性组的每个可用性副本必须位于相同 WSFC 的不同节点上。 唯一的例外是在迁移到另一个 WSFC 群集时，此时一个可用性组可能会暂时跨两个群集。 

 >[!NOTE]
 >有关 Linux 上可用性组的信息，请参阅 [Linux 上的 SQL Server 的 AlwaysOn 可用性组](../../../linux/sql-server-linux-availability-group-overview.md)。 

 HA 配置中会为创建的每个可用性组创建一个群集角色。 WSFC 群集将监视此角色，以便评估主要副本的运行状况。 针对 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的仲裁基于 WSFC 群集中的所有节点，而与某一给定群集节点是否承载任何可用性副本无关。 与数据库镜像相反，在 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]中没有见证服务器角色。  
  
> [!NOTE]  
>  有关 SQL Server AlwaysOn 组件与 WSFC 群集的关系的信息，请参阅 [Windows Server 故障转移群集 (WSFC) 与 SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)。  
  
 下图显示的是一个包含一个主要副本和四个次要副本的可用性组。 支持最多八个辅助副本，包括一个主副本和两个同步提交辅助副本。  
  
 ![有五个副本的可用性组](../../../database-engine/availability-groups/windows/media/aoag-agintrofigure.gif "有五个副本的可用性组")  
  
## <a name="availability-databases"></a><a name="AvDbs"></a> 可用性数据库  
 若要将数据库添加到可用性组，该数据库必须是联机的读写数据库，它位于承载主副本的服务器实例上。 当您添加一个数据库时，它将作为主数据库加入可用性组，同时保持可用于客户端。 除非新的主数据库的备份还原到承载辅助副本（使用 RESTORE WITH NORECOVERY）的服务器实例，否则不存在对应的辅助数据库。 新的辅助数据库处于 RESTORING 状态，直至其加入可用性组。 有关详细信息，请参阅本主题后面的 [启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)。  
  
 加入后，辅助数据库将进入 ONLINE 状态，并启动与对应主数据库之间的数据同步。 “数据同步”  是在辅助数据库上重新生成对主数据库的更改的过程。 数据同步涉及主数据库将事务日志记录发送到辅助数据库。  
  
> [!IMPORTANT]  
>  可用性数据库在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]、PowerShell 和 SQL Server 管理对象 (SMO) 名称中有时候被称作“数据库副本”  。 例如，“数据库副本”一词用于返回与可用性数据库有关的信息的 AlwaysOn 动态管理视图的名称中：**sys.dm_hadr_database_replica_states** 和 **sys.dm_hadr_database_replica_cluster_states**。 但在 SQL Server 联机丛书中，“副本”一词通常表示可用性副本。 例如，“主副本”和“辅助副本”始终表示可用性副本。  
  
## <a name="availability-replicas"></a><a name="AGsARsADBs"></a> 可用性副本  
 每个可用性组定义一个包含两个或更多故障转移伙伴（称为可用性副本）的集合。  “可用性副本”是可用性组的组件。 每个可用性副本都承载可用性组中的可用性数据库的一个副本。 对于某个给定可用性组，可用性副本必须位于某一 WSFC 群集的不同节点上的单独 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上。 必须为 AlwaysOn 启用这些服务器实例中的每个实例。  
  
 对于每个可用性组，一个给定实例只能承载一个可用性副本。 但是，每个实例可用于多个可用性组。 给定的实例可以是独立实例或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例 (FCI)。 如果您要求服务器级别的冗余，则使用故障转移群集实例。  
  
 每个可用性副本都被分配一个初始角色（“主角色”或“辅助角色”），角色由该副本的可用性数据库继承   。 给定副本的角色确定它承载的是读写数据库还是只读数据库。 其中一个副本（称为“主要副本”  ）被分配主角色，它承载读写数据库（称为“主数据库”  ）。 至少一个其他副本（称为“辅助副本”  ）被分配辅助角色。 辅助副本承载只读数据库（称为辅助数据库）。  
  
> [!NOTE]  
>  如果可用性副本的角色是不确定的（如在故障转移过程中），则其数据库将临时处于 NOT SYNCHRONIZING 状态。 其角色设置为 RESOLVING，直至可用性副本的角色已解析。 如果可用性副本解析为主角色，则其数据库将成为主数据库。 如果可用性副本解析为辅助角色，则其数据库将成为辅助数据库。  
  
## <a name="availability-modes"></a><a name="AvailabilityModes"></a> 可用性模式

可用性模式是每个可用性副本的一个属性。 可用性模式确定主副本是否在给定的辅助副本将事务日志记录写入磁盘（强制写入日志）之前，等待提交数据库上的事务。 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 支持两种可用性模式：“异步提交模式”和“同步提交模式”   。  
  
-   **异步提交模式**  
  
     使用此可用性模式的可用性副本称为“异步提交副本”  。 在异步提交模式下，主副本无需等待确认异步提交辅助副本已硬化日志，便可提交事务。 异步提交模式可最大限度地减少辅助数据库上的事务滞后时间，但允许它们滞后于主数据库，因此可能会导致某些数据丢失。  
  
-   **同步提交模式**  
  
     使用此可用性模式的可用性副本称为“同步提交副本”  。 在同步提交模式下，在提交事务之前，同步提交主副本要等待同步提交辅助副本确认它已完成硬化日志。 同步提交模式可确保在给定的辅助数据库与主数据库同步时，充分保护已提交的事务。 这种保护的代价是延长事务滞后时间。  
  
 有关详细信息，请参阅 [可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)。  
  
## <a name="types-of-failover"></a><a name="FormsOfFailover"></a> 故障转移类型  
 在主副本和辅助副本之间的对话上下文中，通过称为“故障转移”  的过程，主角色和辅助角色是潜在可互换的。 在故障转移期间，目标辅助副本转换为主角色，成为新的主副本。 新的主副本使其数据库作为主数据库联机，而客户端应用程序可以连接到这些数据库。 如果以前的主副本可用，则它将转换为辅助角色，成为辅助副本。 以前的主数据库成为辅助数据库，且数据同步恢复。  
  
 有三种故障转移形式：自动、手动和强制（可能造成数据丢失）。 给定辅助副本支持的故障转移形式取决于其可用性模式，对于同步提交模式来说，取决于主副本和目标辅助副本的故障转移模式，如下所示。  
  
-   同步提交模式支持两种故障转移形式：“计划的手动故障转移”和“自动故障转移”（如果目标次要副本当前与主副本同步）   。 对这些故障转移形式的支持取决于故障转移伙伴上的“故障转移模式属性”  的设置。 如果在主副本或辅助副本上将故障转移模式设置为“手动”，则对于该辅助副本仅支持手动故障转移。 如果同时在主副本和辅助副本上将故障转移模式设置为“自动”，则该辅助副本同时支持自动故障转移和手动故障转移。  
  
    -   **计划的手动故障转移** （无数据丢失）  
  
         手动故障转移在数据库管理员发出故障转移命令之后发生，它将导致已同步的辅助副本转换为主角色（同时确保数据受到保护），而主副本转换为辅助角色。 手动故障转移要求主副本和目标辅助副本都在同步提交模式下运行，并且辅助副本必须已同步。  
  
    -   **自动故障转移** （无数据丢失）  
  
         自动故障转移是为了响应导致已同步的辅助副本转换为主角色（同时确保数据受到保护）的故障而执行的。 如果以前的主副本变为可用，则它将转换为辅助角色。 自动故障转移要求主副本和目标辅助副本都在同步提交模式下运行，并且故障转移模式设置为“自动”。 此外，次要副本必须已同步并具有 WSFC 仲裁，且满足由可用性组的 [灵活故障转移策略](./configure-flexible-automatic-failover-policy.md)指定的条件。  
  
        > [!IMPORTANT]  
        >  SQL Server 故障转移群集实例 (FCI) 不支持通过可用性组来自动进行故障转移，因此，只能为手动故障转移配置任何由 FCI 承载的可用性副本。  
  
    > [!NOTE]  
    >  请注意，如果对已同步的辅助副本发出强制故障转移命令，则辅助副本的行为与计划的手动故障转移时的行为相同。  
  
-   在异步提交模式下，唯一的故障转移形式为强制手动故障转移（可能造成数据丢失），通常称作“强制故障转移”  。 强制故障转移被认为是一种手动故障转移，因为它只能手动启动。 强制故障转移是一个灾难恢复选项。 当目标辅助副本与主副本不同步时，强制故障转移是唯一可能的故障转移形式。  
  
 有关详细信息，请参阅 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)概念。  
  
## <a name="client-connections"></a><a name="ClientConnections"></a> 客户端连接  
 您可以通过创建一个可用性组侦听器来提供到给定可用性组的主副本的客户端连接。 “可用性组侦听器”  提供一组附加到给定可用性组的资源，以便将客户端连接定向到相应的可用性副本。  
  
 一个可用性组侦听器与一个唯一的 DNS 名称（用作虚拟网络名称 (VNN)）、一个或多个虚拟 IP 地址 (VIP) 和一个 TCP 端口号关联。 有关详细信息，请参阅 [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)概念。  
  
> [!TIP]  
>  如果一个可用性组仅拥有两个可用性副本，并且未配置为允许对次要副本进行读访问，则客户端可以通过使用 [数据库镜像连接字符串](../../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md)连接到主要副本。 从数据库镜像将数据库迁移到 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]之后，这种方法暂时非常有用。 在添加其他辅助副本之前，您需要为该可用性组创建一个可用性组侦听器，并更新您的应用程序以使用该侦听器的网络名称。  
  
## <a name="active-secondary-replicas"></a><a name="ActiveSecondaries"></a> 活动辅助副本  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 支持活动辅助副本。 活动辅助功能包括对以下方面的支持：  
  
-   **对辅助副本执行备份操作**  
  
     次要副本支持对完整数据库、文件或文件组执行日志备份和 [仅复制](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md) 备份。 您可以配置可用性组，以便为备份指定一个首选位置。 理解 SQL Server 不强制使用首选备份位置十分重要，因为它对即席备份没有影响。 对此首选备份位置的解释取决于您为给定可用性组中的每个数据库撰写作业脚本的逻辑（如果有）。 对于单独的可用性副本，您可以指定对此副本（相对于同一可用性组中其他副本）执行备份的优先级。 有关详细信息，请参阅[活动次要副本：次要副本备份（Always On 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)。  
  
-   **对一个或多个辅助副本（可读辅助副本）的只读访问权限**  
  
     可以对任何次要可用性副本进行配置，以便仅允许对其本地数据库进行只读访问，尽管不会完全支持某些操作。 这会阻止对次要副本进行读写连接尝试。 还可以通过仅允许进行读写访问来阻止主要  副本上的只读工作负载。 这会阻止对主要副本建立只读连接。 有关详细信息，请参阅[活动次要副本：可读次要副本（Always On 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)。  
  
     如果某一可用性组当前拥有一个可用性组侦听程序以及一个或多个可读次要副本，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可以将读意向连接请求路由到其中一个可读次要副本（*只读路由*）。 有关详细信息，请参阅 [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)概念。  
  
## <a name="session-timeout-period"></a><a name="SessionTimeoutPerios"></a> 会话超时期限  
 会话超时期限是一个可用性副本属性，它决定在连接关闭前与另一个可用性副本的连接可处于不活动状态多长时间。 主副本和辅助副本相互 ping 以表示它们还处于活动状态。 在超时期限内从其他副本收到 ping 指示连接仍是打开的，且服务器实例正在进行通信。 收到 ping 后，可用性副本将重置此连接的会话超时计数器。  
  
 会话超时期限防止副本无限制等待接收来自另一个副本的 ping。 如果在会话超时期限内没有收到来自另一个副本的 ping，该副本将超时。连接将关闭，超时的副本进入 DISCONNECTED 状态。 即使为同步提交模式配置了断开连接的副本，事务也将不等待该副本重新连接和重新同步。  
  
 每个可用性副本的默认会话超时期限为 10 秒。 用户可配置此值，最小值为 5 秒。 通常我们建议您将超时期限保持为 10 秒或更长。 如果将值设置为低于 10 秒，则可能使高负荷系统声明虚假故障。  
  
> [!NOTE]  
>  在“正在解析”角色中，会话超时期限不适用，因为不进行 ping。  
  
## <a name="automatic-page-repair"></a><a name="APR"></a> 自动页修复  
 每个可用性副本都通过解决阻止读取数据页的一定类型的错误，自动尝试从本地数据库上损坏的页中恢复。 如果辅助副本无法读取某页，则该副本从主副本请求该页的新副本。 如果主副本无法读取某页，该副本将向所有辅助副本广播索取新副本的请求，并从响应的第一个副本中获取该页。 如果此请求成功，则将以新副本替换不可读的页，这通常会解决该错误。  
  
 有关详细信息，请参阅[自动页修复（可用性组：数据库镜像）](../../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)。  
  
## <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
  
-   [AlwaysOn 可用性组入门 (SQL Server)](../../../database-engine/availability-groups/windows/getting-started-with-always-on-availability-groups-sql-server.md)  
  
## <a name="related-content"></a><a name="RelatedContent"></a> Related content  
  
-   **博客：**  
  
     [Always On - HADRON 学习系列：启用了 HADRON 的数据库的工作线程池用法](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
     [SQL Server Always On 团队博客：SQL Server Always On 团队官方博客](/archive/blogs/sqlalwayson/)  
  
     [CSS SQL Server 工程师博客](/archive/blogs/psssql/)  
  
-   **视频：**  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 1:Introducing the Next Generation High Availability Solution](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)（Microsoft SQL Server Code-Named "Denali" Always On 系列，第 1 部分：介绍下一代高可用性解决方案）  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 2:Building a Mission-Critical High Availability Solution Using Always On](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)（Microsoft SQL Server Code-Named "Denali" Always On 系列，第 2 部分：使用 Always On 生成关键任务高可用性解决方案）  
  
-   **白皮书：**  
  
     [用于高可用性和灾难恢复的 Microsoft SQL Server AlwaysOn 解决方案指南](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [针对 SQL Server 2012 的 Microsoft 白皮书](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [SQL Server 客户咨询团队白皮书](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>另请参阅  
 [可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [AlwaysOn 可用性组的 Transact-SQL 语句概述 (SQL Server)](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)   
 [AlwaysOn 可用性组的 PowerShell Cmdlet 概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)   
 [对内存中 OLTP 数据库的高可用性支持](../../../relational-databases/in-memory-oltp/high-availability-support-for-in-memory-oltp-databases.md)   
 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [创建和配置可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [活动次要副本：可读次要副本（Always On 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [活动次要副本：次要副本备份（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)   
 [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  
