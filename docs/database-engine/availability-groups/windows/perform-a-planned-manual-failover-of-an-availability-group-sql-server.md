---
title: 执行可用性组的计划手动故障转移
description: 本主题介绍如何执行 Always On 可用性组的计划手动故障转移。
ms.custom: seodec18
ms.date: 10/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.manualfailover.f1
helpviewer_keywords:
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 419f655d-3f9a-4e7d-90b9-f0bab47b3178
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3cf135ca98a61382e12973b93eb30ab06ad6f4dd
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642563"
---
# <a name="perform-a-planned-manual-failover-of-an-always-on-availability-group-sql-server"></a>执行 Always On 可用性组的计划手动故障转移 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
本主题说明如何在 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 中使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]、[!INCLUDE[tsql](../../../includes/tsql-md.md)] 或 PowerShell 对 AlwaysOn 可用性组执行手动故障转移而不丢失数据（计划的手动故障转移）。 可用性组在可用性副本级别进行故障转移。 计划的手动故障转移类似于所有 AlwaysOn 可用性组的故障转移，将次要副本转换为主要角色。 故障转移同时会将先前的主要副本转换为次要角色。  
  
仅当主要副本和目标次要副本在同步提交模式下运行且当前同步时，才支持计划的手动故障转移。 计划的手动故障转移暂留辅助数据库中的所有数据，这些数据库加入目标次要副本上的可用性组。 将以前的主要副本转换为次要角色后，其数据库将成为辅助数据库。 然后它们开始与新的主数据库同步。 在将其全部转换为 SYNCHRONIZED 状态之后，新的辅助副本将变成适于充当将来计划的手动故障转移的目标。  
  
> [!NOTE]  
>  如果次要副本和主要副本都配置为自动故障转移模式，那么同步次要副本后，该副本还可以用作自动故障转移的目标。 有关详细信息，请参阅[可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)。  
   
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 准备工作 

>[!IMPORTANT]
>在没有群集管理器的情况下，需要通过一些特定过程对读取缩放可用性组进行故障转移。 当可用性组具有 CLUSTER_TYPE = NONE 时，请按照[故障转移读取缩放可用性组上的主要副本](#fail-over-the-primary-replica-on-a-read-scale-availability-group)中的过程进行操作。

###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限 
  
- 故障转移命令将在目标辅助副本接受它之后立即返回。 但是，在可用性组完成故障转移之后，数据库恢复操作将以异步方式执行。 
- 故障转移时，可能不维护可用性组中数据库间的跨数据库一致性。 
  
    > [!NOTE] 
    >  对跨数据库和分布式事务的支持因 SQL Server 和操作系统版本而异。 有关详细信息，请参阅[用于 AlwaysOn 可用性组和数据库镜像的跨数据库事务和分布式事务 (SQL Server)](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)。 
  
###  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a>先决条件和限制 
  
-   目标次要副本和主要副本必须同时在同步提交可用性模式下运行。 
-   目标次要副本当前必须与主要副本同步。 此次要副本上的所有辅助数据库都必须已联接到可用性组。 它们还必须与自己对应的主数据库同步（即本地辅助数据库必须为 SYNCHRONIZED）。 
  
    > [!TIP] 
    >  要确定次要副本的故障转移就绪状态，请查询 [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md) 动态管理视图中的 is_failover_ready 列。 或查看 [AlwaysOn 组仪表板](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)的“故障转移就绪”列。 
-   只有目标辅助副本支持该任务。 您必须连接到承载目标辅助副本的服务器实例。 
  
###  <a name="security"></a><a name="Security"></a> Security 
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限 
 需要具有针对可用性组的 ALTER AVAILABILITY GROUP 权限。 还要求具备 CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。 
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio 
 对可用性组执行手动故障转移： 
  
1. 在对象资源管理器中，连接到托管需要进行故障转移的可用性组的一个次要副本的服务器实例。 展开服务器树。 
  
2. 依次展开 **“AlwaysOn 高可用性”** 节点和 **“可用性组”** 节点。 
  
3. 右键单击要进行故障转移的可用性组，然后选择“故障转移”  。 
  
4. “故障转移可用性组向导”随即启动。 有关详细信息，请参阅[使用故障转移可用性组向导 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)。 
  
##  <a name="use-transact-sql"></a><a name="TsqlProcedure"></a>使用 Transact-SQL 
 对可用性组执行手动故障转移： 
  
1. 连接到承载目标辅助副本的服务器实例。 
  
2. 按如下所示使用 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) 语句： 
  
     ALTER AVAILABILITY GROUP *group_name* FAILOVER 
  
     在该语句中，group_name  是可用性组的名称。 
  
     以下示例将 *MyAg* 可用性组手动故障转移到连接的次要副本： 
  
    ```  
    ALTER AVAILABILITY GROUP MyAg FAILOVER;  
    ```  
  
##  <a name="use-powershell"></a><a name="PowerShellProcedure"></a>使用 PowerShell 
 对可用性组执行手动故障转移： 
  
1. 将目录 (cd  ) 更改为托管目标次要副本的服务器实例。 
  
2. 使用 **Switch-SqlAvailabilityGroup** cmdlet。 
  
    > [!NOTE] 
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] cmdlet。 有关详细信息，请参阅[获取有关 SQL Server PowerShell 的帮助](../../../powershell/sql-server-powershell.md)。 
  
     下面的示例将 MyAg  可用性组手动故障转移到具有指定路径的次要副本： 
  
    ```  
    Switch-SqlAvailabilityGroup -Path SQLSERVER:\Sql\SecondaryServer\InstanceName\AvailabilityGroups\MyAg  
    ```  
  
    设置和使用 SQL Server PowerShell 提供程序： 
  
    -   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md) 
    -   [获取有关 SQL Server PowerShell 的帮助](../../../powershell/sql-server-powershell.md) 

##  <a name="follow-up-after-you-manually-fail-over-an-availability-group"></a><a name="FollowUp"></a> 跟进：在对可用性组进行手动故障转移后 
 如果故障转移到可用性组的 [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)] 之外，请调整 Windows Server 故障转移群集节点的仲裁投票以反映新的可用性组配置。 有关详细信息，请参阅 [Windows Server 故障转移群集 (WSFC) 与 SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)。 

<a name = "ReadScaleOutOnly"><a/>

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>故障转移读取缩放可用性组上的主要副本

[!INCLUDE[Force failover](../../../includes/ss-force-failover-read-scale-out.md)]

## <a name="see-also"></a>另请参阅 

 * [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) 
 * [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) 
 * [执行可用性组的强制手动故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md) 
  
