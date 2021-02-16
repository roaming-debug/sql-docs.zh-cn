---
title: 可用性组入门
description: 了解按照步骤该如何配置 SQL Server 实例来支持 Always On 可用性组以及创建、管理和监视可用性组。
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], about
ms.assetid: 33f2f2d0-79e0-4107-9902-d67019b826aa
author: cawrites
ms.author: chadam
ms.openlocfilehash: 50bfb7b8fdef0c516c3b5fe62d739fe90612f38f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348475"
---
# <a name="getting-started-with-always-on-availability-groups"></a>AlwaysOn 可用性组入门
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

本主题将介绍一些步骤，包括用于配置 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 的实例以便支持 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的步骤，以及用于创建、管理和监视可用性组的步骤。  
  
  
##  <a name="recommended-reading"></a><a name="RecommendedReading"></a> 推荐阅读的主题  
 在创建您的第一个可用性组之前，我们建议您阅读以下主题：  
  
-   [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
-   [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
##  <a name="configuring-an-instance-of-sql-server-to-support-always-on-availability-groups"></a><a name="ConfigSI"></a> Configuring an Instance of SQL Server to Support Always On Availability Groups  
  
|步骤|链接|  
|----------|-----------|  
|**启用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]。** 必须在参与某一可用性组的每个 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 实例上都启用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 功能。<br /><br /> **先决条件：** 主机必须是 Windows Server 故障转移群集 (WSFC) 节点。<br /><br /> 有关其他先决条件的信息，请参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。|[启用和禁用 AlwaysOn 可用性组](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)|  
|**创建数据库镜像端点（如果没有）。** 确保每个服务器实例都拥有 [数据库镜像端点](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)。 服务器实例使用此端点从其他服务器实例接收 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 连接。|确定数据库镜像端点是否存在： <br />                    [sys.database_mirroring_endpoints](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)<br /><br /> **对于 Windows 身份验证**。  若要创建数据库镜像端点，请使用：<br /><br /> [新建可用性组向导](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)<br /><br /> [Transact-SQL](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)<br /><br /> [SQL Server PowerShell](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)<br /><br /> **对于证书身份验证**。 若要创建数据库镜像端点，请使用：[Transact-SQL](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)|  
  
##  <a name="creating-and-configuring-a-new-availability-group"></a><a name="ConfigAG"></a> Creating and Configuring a New Availability Group  
  
|步骤|链接|  
|----------|-----------|  
|**创建可用性组。** 在承载要添加到可用性组的数据库的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上创建可用性组。<br /><br /> 至少在创建可用性组的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上创建初始主副本。 您可以指定一到四个辅助副本。 有关可用性组和副本属性的信息，请参阅[CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md)。<br /><br /> 强烈建议您创建 [可用性组侦听器](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)。<br /><br /> **先决条件：** 承载给定可用性组的可用性副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例必须位于单个 WSFC 群集的单独节点上。 唯一的例外是在迁移到另一个 WSFC 群集时，此时一个可用性组可能会暂时跨两个群集。<br /><br /> 有关其他先决条件的信息，请参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。|若要创建一个可用性组，您可以使用以下任何工具：<br /><br /> [新建可用性组向导](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)<br /><br /> [Transact-SQL](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)<br /><br /> [SQL Server PowerShell](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)|  
|**将辅助副本联接到可用性组。** 连接到正在承载某一辅助副本的各 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 实例上，并且将该本地辅助副本联接到可用性组。|[将辅助副本联接到可用性组](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)<br /><br /> 提示：如果使用“新建可用性组”向导，此步骤将自动执行。|  
|**准备辅助数据库。** 在正在承载辅助副本的每个服务器实例上，使用 RESTORE WITH NORECOVERY 还原主数据库的备份。|[手动准备辅助数据库](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)<br /><br /> 提示：“新建可用性组”向导能够为你准备辅助数据库。 有关详细信息，请参阅[选择初始数据同步页 (Always On 可用性组向导)](../../../database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards.md) 中的“使用完全初始数据同步的先决条件”。|  
|**将辅助数据库联接到可用性组。** 在正在承载辅助副本的每个服务器实例上，将各本地辅助数据库联接到可用性组。 在联接可用性组后，给定辅助数据库将开始与相应的主数据库的数据同步。|[将辅助数据库联接到可用性组](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)<br /><br /> 提示：如果每个辅助数据库均存在于各次要副本上，那么“新建可用性组”向导会执行此步骤。|  
|**创建可用性组侦听器。**  除非在创建可用性组时已创建了可用性组侦听器，否则此步骤是必需的。|[创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)|  
|**将侦听器的 DNS 主机名提供给应用程序开发人员。**  开发人员需要在连接字符串中指定此 DNS 名称以对可用性组侦听器进行直接连接请求。 有关详细信息，请参阅 [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)概念。|“跟进：创建可用组侦听程序之后”，见[创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)|  
|**配置运行备份作业的位置。**  如果要对辅助数据库执行备份，则必须创建一个备份作业脚本，该脚本将会考虑到自动备份首选项。 为承载可用性组的可用性副本的每个服务器实例上可用性组中的每个数据库都创建一个脚本。|“跟进：配置次要副本备份之后”，见[配置可用性副本备份 (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)|  
  
##  <a name="managing-availability-groups-replicas-and-databases"></a><a name="ManageAGsEtc"></a> Managing Availability Groups, Replicas, and Databases  
  
> [!NOTE]  
>  有关可用性组和副本属性的信息，请参阅[CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md)。  
  
 管理现有可用性组涉及以下一个或多个任务：  
  
|任务|链接|  
|----------|----------|  
|修改可用性组的 [灵活的故障转移策略](./configure-flexible-automatic-failover-policy.md) ，以便控制导致自动故障转移的条件。 此策略仅适用于可进行自动故障转移的情况。|[配置可用性组的灵活的故障转移策略](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)|  
|执行计划的手动故障转移或强制的手动故障转移（可能有数据丢失），通常称作“强制故障转移”。 有关详细信息，请参阅 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)概念。|[执行计划的手动故障转移](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)<br /><br /> [执行强制的手动故障转移](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)|  
|使用一组预定义策略，以便查看某一可用性组及其副本和数据库的运行状况。|[使用基于策略的管理查看可用性组的运行状况](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)<br /><br /> [使用 Always On 组面板](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)|  
|添加或删除辅助副本。|[添加辅助副本](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)<br /><br /> [删除辅助副本](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)|  
|挂起或恢复可用性数据库。 暂停某一辅助数据库会保持在其当前时间点，直到您恢复该数据库。|[挂起数据库](../../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md)<br /><br /> [恢复数据库](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)|  
|添加或删除数据库。|[添加数据库](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)<br /><br /> [删除辅助数据库](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)<br /><br /> [删除主数据库](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)|  
|重新配置或创建可用性组侦听器。|[创建或配置可用性组侦听器](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)|  
|删除可用性组。|[删除可用性组](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)|  
|排除添加文件操作的问题。 如果主数据库和辅助数据库具有不同的文件路径，则此操作可能是必需的。|[排除失败的添加文件操作的问题](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)|  
|更改可用性副本属性。|[更改可用性模式](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)<br /><br /> [更改故障转移模式](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)<br /><br /> [配置备份优先级（以及自动备份首选项）](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)<br /><br /> [配置只读访问](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)<br /><br /> [配置只读路由](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)<br /><br /> [更改会话超时期限](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)|  
  
##  <a name="monitoring-availability-groups"></a><a name="MonitorAGsEtc"></a> 监视可用性组  
 若要监视 AlwaysOn 可用性组的属性和状态，可以使用以下工具。  
  
|工具|简要说明|链接|  
|----------|-----------------------|-----------|  
|SQL Server 的系统中心监视包|对于 IT 管理员，建议使用 SQL Server (SQLMP) 的监视包这一解决方案来监视可用性组、可用性副本和可用性数据库。 与 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 尤为相关的监视功能包括：<br /><br /> 可自动发现数百台计算机中的可用性组、可用性副本和可用性数据库。 这使您能够轻松地跟踪 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 清单。<br /><br /> 功能完善的 System Center Operations Manager (SCOM) 报警和票证。 这些功能提供了详细的知识，使您可以更快地解决问题。<br /><br /> 使用基于策略的管理 (PBM) 对 AlwaysOn 运行状况监视进行自定义扩展。<br /><br /> 从可用性数据库到可用性副本累积运行状况信息。<br /><br /> 用于从 System Center Operations Manager 控制台中管理 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的自定义任务。|若要下载监视包 (SQLServerMP.msi) 和 *用于 System Center Operations Manager 的 SQL Server 管理包指南* (SQLServerMPGuide.doc)，请参阅：<br /><br /> [SQL Server 的系统中心监视包](https://www.microsoft.com/download/details.aspx?displaylang=en&id=10631)|  
|[!INCLUDE[tsql](../../../includes/tsql-md.md)]|[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 目录和动态管理视图提供了有关可用性组及其副本、数据库、侦听器和 WSFC 群集环境的大量信息。|[监视可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)|  
|[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]|**“对象资源管理器详细信息”** 窗格显示有关您连接到的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例所承载的可用性组的基本信息。<br /><br /> 提示：使用此窗格可以选择多个可用性组、副本或数据库，并能对选定对象执行常规管理任务；例如，从可用性组中删除多个可用性副本或数据库。|[使用“对象资源管理器详细信息”来监视可用性组](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)|  
|[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]|**“属性”** 对话框使您能够查看可用性组、副本或侦听器的属性，并在某些情况下可更改这些属性的值。|[可用性组属性](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)<br /><br /> [可用性副本属性](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)<br /><br /> [可用性组侦听器属性](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)|  
|系统监视器|**SQLServer:Availability Replica** 性能对象包含性能计数器，可报告可用性副本的相关信息。|[SQL Server，可用性副本](../../../relational-databases/performance-monitor/sql-server-availability-replica.md)|  
|系统监视器|**SQLServer:Database Replica** 性能对象包含性能计数器，可报告给定次要副本上的辅助数据库的相关信息。<br /><br /> SQL Server 中的 **SQLServer:Databases** 对象包含用于监视事务日志活动（但不仅限于此）的性能计数器。 下列计数器特别适用于监视可用性数据库上的事务日志活动： **日志刷新写入时间(秒)** 、 **日志刷新次数/秒**、 **日志池缓存未命中数/秒**、 **日志池磁盘读取次数/秒** 和 **日志池请求数/秒**。|[SQL Server - 数据库副本](../../../relational-databases/performance-monitor/sql-server-database-replica.md)<br /><br /> [SQL Server, Databases Object](../../../relational-databases/performance-monitor/sql-server-databases-object.md)|  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   **视频 - Always On 简介：** [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 1:Introducing the Next Generation High Availability Solution](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)（Microsoft SQL Server Code-Named "Denali" Always On 系列，第 1 部分：介绍下一代高可用性解决方案）  
  
-   **视频 - 深入了解 Always On：**  [Microsoft SQL Server Code 命名为“Denali”的 Always On 系列，第二部分：使用 Always On 生成任务关键型高可用性解决方案](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **白皮书：** [用于高可用性和灾难恢复的 Microsoft SQL Server AlwaysOn 解决方案指南](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   **博客：** [SQL Server Always On 团队博客：SQL Server Always On 团队官方博客](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [为 AlwaysOn 可用性组配置服务器实例 (SQL Server)](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md)   
 [创建和配置可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [监视可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [AlwaysOn 可用性组的 Transact-SQL 语句概述 (SQL Server)](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)   
 [AlwaysOn 可用性组的 PowerShell Cmdlet 概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)  
  
