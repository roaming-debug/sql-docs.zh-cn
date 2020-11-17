---
title: 管理可用性组（内容索引）
description: 链接到介绍管理 Always On 可用性组的基础知识（如更改属性、添加或删除副本、添加或删除数据库、故障转移、配置侦听程序等）的相关文章的引用索引。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], managing
ms.assetid: 0b7542fa-235e-413d-81bf-3eff9ee07480
author: cawrites
ms.author: chadam
ms.openlocfilehash: f82ca3cd976a13cfc9badf17f8dac02cbec83d1a
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584826"
---
# <a name="administration-of-an-availability-group"></a>管理可用性组
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
 在 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 中管理现有 AlwaysOn 可用性组涉及以下一个或多个任务：  
  
-   更改现有可用性副本的属性以便更改客户端连接访问（用于配置可读的辅助副本）等，更改其故障转移模式、可用性模式或会话超时设置。    
-   添加或删除辅助副本。    
-   添加或删除数据库。    
-   暂停或恢复数据库。   
-   执行计划的手动故障转移（手动故障转移  ）或强制手动故障转移（强制故障转移  ）。    
-   创建和配置可用性组侦听器。    
-   为某一给定可用性组管理 [可读次要副本](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md) 。 这涉及在以辅助角色运行时将一个或多个副本配置为只读访问以及配置只读路由。    
-   为某一给定可用性组管理 [次要副本上的备份](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md) 。 这涉及配置您希望运行备份作业的位置，然后编写备份作业脚本，以便实现您的备份首选项。 在承载可用性副本的每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上，对于可用性组中的每个数据库，您都需要为备份作业编写脚本。    
-   删除可用性组。    
-   针对操作系统升级的 AlwaysOn 可用性组的跨群集迁移  
  
## <a name="configure-an-existing-availability-group"></a>配置现有的可用性组
  
-   [将辅助副本添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [将次要副本从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [将数据库添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)    
-   [将辅助数据库从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
-   [将主数据库从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
-   [配置灵活的故障转移策略以控制自动故障转移的条件（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
 ## <a name="manage-an-availability-group"></a>管理可用性组  
  
-   [配置可用性副本备份 (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [执行可用性组的计划手动故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [执行可用性组的强制手动故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [删除可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  
 ## <a name="manage-an-availability-replica"></a>管理可用性副本  
  
-   [将辅助副本添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [将次要副本从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [更改可用性副本的可用性模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [更改可用性副本的故障转移模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [配置可用性副本备份 (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [配置对可用性副本的只读访问 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [为可用性组配置只读路由 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [更改可用性副本的会话超时期限 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
## <a name="manage-an-availability-database"></a>管理可用性数据库  
  
-   [将数据库添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)  
  
-   [将辅助数据库联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [将主数据库从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
-   [将辅助数据库从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
-   [挂起可用性数据库 (SQL Server)](../../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md)  
  
-   [恢复可用性数据库 (SQL Server)](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
## <a name="monitor-an-availability-group"></a>监视可用性组
  
-   [监视可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
 ## <a name="support-migrating-availability-groups-to-a-new-wsfc-cluster-cross-cluster-migration"></a>支持将可用性组迁移到新的 WSFC 群集（跨群集迁移）
  
-   [更改服务器实例的 HADR 群集上下文 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)  
  
-   [使可用性组脱机 (SQL Server)](../../../database-engine/availability-groups/windows/take-an-availability-group-offline-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   **博客：**  
  
     [SQL Server Always On 团队博客：SQL Server Always On 团队官方博客](/archive/blogs/sqlalwayson/)    
[CSS SQL Server 工程师博客](/archive/blogs/psssql/)  
  
-   **视频：**  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 1:Introducing the Next Generation High Availability Solution](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  Microsoft SQL Server Code-Named "Denali" Always On 系列，第 1 部分：介绍下一代高可用性解决方案）  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 2:Building a Mission-Critical High Availability Solution Using Always On](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)（Microsoft SQL Server Code-Named "Denali" Always On 系列，第 2 部分：使用 Always On 生成关键任务高可用性解决方案）  
  
-   **白皮书：**  
  
     [针对 SQL Server 2012 的 Microsoft 白皮书](https://msdn.microsoft.com/library/hh403491.aspx)    
     [SQL Server 客户咨询团队白皮书](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [为 AlwaysOn 可用性组配置服务器实例 (SQL Server)](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md)   
 [创建和配置可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [活动次要副本：可读次要副本（Always On 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [活动次要副本：次要副本备份（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)   
 [可用性组侦听程序、客户端连接和应用程序故障转移 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [针对 AlwaysOn 可用性组运行问题的 AlwaysOn 策略 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)   
 [监视可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [AlwaysOn 可用性组：互操作性 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [AlwaysOn 可用性组的 Transact-SQL 语句概述 (SQL Server)](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)   
 [AlwaysOn 可用性组的 PowerShell Cmdlet 概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)  
  
