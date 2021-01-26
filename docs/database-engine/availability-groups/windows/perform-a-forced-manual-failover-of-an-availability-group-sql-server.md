---
title: 对可用性组强制执行手动故障转移
description: 本主题介绍如何使用 Transact-SQL (T-SQL)、PowerShell 或 SQL Server Management Studio 对 Always On 可用性组执行强制故障转移（可能会丢失数据）。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.forcefailover.f1
helpviewer_keywords:
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 222288fe-ffc0-4567-b624-5d91485d70f0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9b029efb566fef24aed4748b51bfc15050d495c0
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783486"
---
# <a name="perform-a-forced-manual-failover-of-an-always-on-availability-group-sql-server"></a>执行 Always On 可用性组的强制手动故障转移 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]中通过使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)]、 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]或 PowerShell 对 AlwaysOn 可用性组执行强制故障转移（可能会丢失数据）。 强制故障转移是一种在不可能进行 [计划的手动故障转移](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md) 时严格限制用于灾难恢复的手动故障转移。 如果您强制故障转移到某一未同步的辅助副本，则可能会丢失一些数据。 因此，我们强烈建议您仅在以下情况下才强制故障转移：您必须立即将服务还原到可用性组并且您愿意承担丢失数据的风险。  
  
 执行强制故障转移之后，可用性组故障转移到的故障转移目标将变成新的主副本。 剩余辅助副本中的辅助数据库将会挂起并且必须手动恢复。 当以前的主副本变为可用时，该副本将转换为辅助角色，这导致以前的主数据库变成辅助数据库并转换为 SUSPENDED 状态。 在恢复某一给定的辅助数据库之前，您可能能够从其还原丢失的数据。 但请注意，在其任何辅助数据库被挂起时，事务日志截断在给定的主数据库上被延迟。  
  
> [!IMPORTANT]  
>  在恢复辅助数据库之前，不会与主数据库进行数据同步。 有关恢复辅助数据库的信息，请参阅本文后面部分的[跟进：强制故障转移后的重要任务](#FollowUp)。  
  
 在以下紧急情况下需要执行强制的故障转移：  
  
-   在对 WSFC 群集执行强制仲裁（“强制仲裁”  ）后，你需要强制故障转移每个可用性组（可能会丢失数据）。 强制故障转移是必需的，因为 WSFC 群集值的真实状态可能已丢失。 但是，如果您在强制仲裁前能够在承载作为主副本的副本的服务器实例上强制故障转移，或者在强制仲裁前能够故障转移到已同步的辅助副本，则可以避免数据丢失。 有关详细信息，请参阅本主题后面的 [强制仲裁后避免数据丢失的可能方法](#WaysToAvoidDataLoss)。  
  
    > [!IMPORTANT]  
    >  如果仲裁以自然的方式重新获得而不是强制进行的，则可用性副本将经历正常的恢复工程。 如果在重新获得仲裁后主副本仍不可用，则您可以执行计划的手动故障转移到同步的辅助副本。  
  
     有关强制仲裁的详细信息，请参阅 [通过强制仲裁进行 WSFC 灾难恢复 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)或 PowerShell 对 AlwaysOn 可用性组执行强制故障转移（可能会丢失数据）。 有关为什么需要在强制仲裁后强制执行故障转移的信息，请参阅 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)或 PowerShell 对 AlwaysOn 可用性组执行强制故障转移（可能会丢失数据）。  
  
-   如果在 WSFC 群集具有运行状况正常的仲裁时主副本变得不可用，则您可以强制故障转移（可能会丢失数据）到其角色处于 SECONDARY 或 RESOLVING 状态的任何副本。 如果可能，应强制故障转移到在主副本丢失时处于同步状态的同步提交辅助副本。  
  
    > [!TIP]  
    >  在 WSFC 群集具有运行状况正常的仲裁时，如果对已同步的辅助副本发出强制故障转移命令，则该副本实际将执行计划的手动故障转移。  
  
> [!NOTE]  
>  有关强制故障转移的先决条件和建议的详细信息，以及使用强制故障转移从灾难性故障中恢复的示例应用场景，请参阅本主题后面部分的[示例应用场景：使用强制故障转移从灾难性故障中恢复](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md#ExampleRecoveryFromCatastrophy)。  
  
  
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   不能执行强制故障转移的唯一时间是在 Windows Server 故障转移群集 (WSFC) 群集缺少仲裁时。  
  
-   在可用性组强制故障转移期间可能会丢失数据。 此外，如果在启动强制故障转移时主副本正在运行，客户端可能仍连接到以前的主数据库。 因此，我们强烈建议您仅在以下情况下才强制执行故障转移：主副本不再运行，并且您为了还原对可用性组中数据库的访问而愿意承担丢失数据的风险。  
  
-   当辅助数据库处于 REVERTING 或 INITIALIZING 状态时，强制故障转移将导致该数据库无法作为主数据库启动。 如果数据库处于 INITIALIZING 状态，则需要从数据库备份中应用缺失的日志记录，或者从头开始完全还原数据库。 如果数据库处于 REVERTING 状态，则您将需要从备份完全还原数据库。  
  
-   故障转移命令将在故障转移目标接受它之后立即返回。 但是，在可用性组完成故障转移之后，数据库恢复操作将以异步方式执行。  
  
-   故障转移时，可能不维护可用性组中数据库间的跨数据库一致性。  
  
    > [!NOTE]  
    >  对跨数据库和分布式事务的支持因 SQL Server 和操作系统版本而异。 有关详细信息，请参阅[用于 AlwaysOn 可用性组和数据库镜像的跨数据库事务和分布式事务 (SQL Server)](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)。  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   WSFC 群集具有仲裁。 如果群集缺乏仲裁，请参阅 [通过强制仲裁进行 WSFC 灾难恢复 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)或 PowerShell 对 AlwaysOn 可用性组执行强制故障转移（可能会丢失数据）。  
  
-   您必须能够连接到承载其角色处于 SECONDARY 或 RESOLVING 状态的副本的服务器实例。  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   主副本仍在运行时，请勿进行强制故障转移。  
  
-   如果可能，仅强制故障转移到其辅助数据库处于 NOT SYNCHRONIZED、SYNCHRONIZED 或 SYNCHRONIZING 状态的故障转移目标。 若要了解在辅助数据库处于 INITIALIZING 或 REVERTING 状态时强制执行故障转移的作用，请参阅本主题中前面的[限制和约束](#Restrictions)。  
  
-   通常情况下，给定的辅助数据库的延迟（相对于主数据库）在不同的异步提交辅助副本上应该是相似的。 但在强制故障转移时，数据丢失问题可能会比较突出。 因此，应该考虑花些时间来确定数据库在不同辅助副本上的相对延迟。 若要确定某一给定辅助数据库的哪一副本具有最少的延迟，请比较其日志末尾 LSN。 更高的日志末尾 LSN 指示更少的延迟。  
  
    > [!TIP]  
    >  若要比较日志末尾 LSN，请连接到各在线次要副本，并且针对每个本地辅助数据库的 [end_of_log_lsn](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) 值查询 **sys.dm_hadr_database_replica_states** 。 然后，比较每个数据库的不同副本的日志末尾 LSN。 请注意，不同的数据库可能在不同的辅助副本上具有其最高 LSN。 在本例中，最合适的故障转移目标取决于对于不同数据库中的数据的相对重要性。 也就是说，在这些数据库中，您最希望减小哪个数据库中可能的数据丢失？  
  
-   如果客户端无法连接到原始主副本，则强制故障转移可能会导致某些“裂脑”风险。 在强制故障转移之前，强烈建议您禁止客户端访问原始主副本。 否则，在强制故障转移之后，原始主数据库和当前主数据库便会分别进行更新。  
  
##  <a name="potential-ways-to-avoid-data-loss-after-quorum-is-forced"></a><a name="WaysToAvoidDataLoss"></a> 强制仲裁后避免数据丢失的可能方法  
 在仲裁后的一些失败条件下，您可以避免数据丢失，如下所示：  
  
-   **如果原始主副本处于联机状态**  
  
     如果仲裁丢失并且强制 WSFC 仲裁还原承载某一可用性组的主副本的群集节点，则对于此可用性组您可以避免数据丢失。 连接到主副本并且执行强制故障转移 (FAILOVER_ALLOW_DATA_LOSS)。 此操作将使主副本恢复联机状态。 因为您执行强制故障转移到原始主副本，所以不会有数据丢失。  
  
-   **如果已同步的同步提交辅助副本处于联机状态**  
  
     如果仲裁丢失并且强制 WSFC 仲裁还原承载某一可用性组的已同步辅助副本的群集节点，则对于此可用性组您应该能够避免数据丢失。 如果在丢失仲裁时还原的节点已启动，则可以通过查询 **sys.dm_hadr_database_replica_cluster_states** 动态管理视图的 [is_failover_ready](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md) 列确定给定数据库上是否发生了数据丢失。 例如，对于名为 `sql108w2k8r22`的服务器实例，发出以下查询：  
  
    ```  
    SELECT * FROM sys.dm_hadr_database_replica_cluster_states  
       WHERE replica_id=(SELECT replica_id FROM sys.availability_replicas   
          WHERE replica_server_name ='sql108w2k8r22')  
    ```  
  
    > [!CAUTION]  
    >  如果在丢失仲裁时还原的节点未启动，则 **is_failover_ready** 可能不会反映在主要副本处于脱机状态时该群集的实际状态。 因此， **is_failover_ready** 值是在主机节点处于失败时唯一合适的值。 有关详细信息，请参阅 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)或 PowerShell 对 AlwaysOn 可用性组执行强制故障转移（可能会丢失数据）。  
  
     如果 **is_failover_ready** = 1，则数据库将在群集中标记为已同步并且可供故障转移。 如果在某一给定次要副本的每个数据库上 **is_failover_ready** = 1，则你可以执行强制故障转移 (FORCE_FAILOVER_ALLOW_DATA_LOSS) 并且在此次要副本上不会丢失数据。 该同步的辅助副本在主角色中处于联机状态，也就是说，作为新的主副本并且所有数据保持不变。  
  
     如果 **is_failover_ready** = 0，则数据库在群集中将不标记为已同步并且 *不* 可用于计划的手动故障转移。 如果您强制故障转移到主机辅助副本，则在此数据库上数据将丢失。  
  
    > [!NOTE]  
    >  在您强制故障转移到辅助副本时，丢失的数据量将取决于故障转移目标落在主副本之后有多远。 但是，在 WSFC 群集缺少仲裁或者仲裁已被强制时，您不能评估可能的数据丢失量。 但请注意，一旦 WSFC 群集重新获得运行状况正常的仲裁后，您可以开始跟踪可能的数据丢失。 有关详细信息，请参阅 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)或 PowerShell 对 AlwaysOn 可用性组执行强制故障转移（可能会丢失数据）。  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **强制故障转移（可能丢失数据）**  
  
1.  在对象资源管理器中，连接到一个服务器实例，该服务器实例承载需要进行故障转移的可用性组中其角色处于 SECONDARY 或 RESOLVING 状态的副本，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  右键单击要进行故障转移的可用性组，然后选择“故障转移”  命令。  
  
4.  这将启动“故障转移可用性组向导”。 有关详细信息，请参阅本主题后面的 [使用故障转移可用性组向导 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)或 PowerShell 对 AlwaysOn 可用性组执行强制故障转移（可能会丢失数据）。  
  
5.  强制可用性组进行故障转移之后，请完成必要的后续步骤。 有关详细信息，请参阅本主题后面部分的[跟进：强制故障转移后的重要任务](#FollowUp)。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **强制故障转移（可能丢失数据）**  
  
1.  连接到一个服务器实例，该服务器实例承载需要进行故障转移的可用性组中其角色处于 SECONDARY 或 RESOLVING 状态的副本。  
  
2.  按如下所示使用 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) 语句：  
  
     ALTER AVAILABILITY GROUP *group_name* FORCE_FAILOVER_ALLOW_DATA_LOSS  
  
     其中， *group_name* 是可用性组的名称。  
  
     下面的示例强制 `AccountsAG` 可用性组故障转移到本地辅助副本。  
  
    ```  
    ALTER AVAILABILITY GROUP AccountsAG FORCE_FAILOVER_ALLOW_DATA_LOSS;  
    ```  
  
3.  强制可用性组进行故障转移之后，请完成必要的后续步骤。 有关详细信息，请参阅本主题后面部分的[跟进：强制故障转移后的重要任务](#FollowUp)。  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **强制故障转移（可能丢失数据）**  
  
1.  将目录 (cd) 更改为一个服务器实例，该实例托管需要进行故障转移的可用性组中其角色处于 SECONDARY 或 RESOLVING 状态的副本  。  
  
2.  采用以下形式之一将 **Switch-SqlAvailabilityGroup** cmdlet 与 **AllowDataLoss** 参数结合使用：  
  
    -   **-AllowDataLoss**  
  
         默认情况下， **-AllowDataLoss** 参数会让 **Switch-SqlAvailabilityGroup** 向你进行提示，以提醒你强制故障转移可能会导致丢失未提交的事务，并请求确认。 若要继续，请输入 **Y**；若要取消操作，请输入 **N**。  
  
         下面的示例对可用性组 `MyAg` 执行强制故障转移（可能会造成数据丢失），以将故障转移到名为 `SecondaryServer\InstanceName`服务器实例上的次要副本。 系统将提示您确认此操作。  
  
        ```  
        Switch-SqlAvailabilityGroup `  
           -Path SQLSERVER:\Sql\SecondaryServer\InstanceName\AvailabilityGroups\MyAg `  
           -AllowDataLoss  
        ```  
  
    -   **-AllowDataLoss-Force**  
  
         若要启动强制故障转移而无需确认，请同时指定 **-AllowDataLoss** 和 **-Force** 参数。 如果您要在脚本中包含此命令而无需用户交互来运行它，此操作很有用。  但是，请谨慎使用 **-Force** 选项，因为强制故障转移可能会导致参与可用性组的数据库中出现数据丢失。  
  
         下面的示例将对可用性组 `MyAg` 执行强制故障转移（可能造成数据丢失），以将故障转移到名为 `SecondaryServer\InstanceName`的服务器实例。 **-Force** 选项将取消此操作的确认。  
  
        ```  
        Switch-SqlAvailabilityGroup `  
           -Path SQLSERVER:\Sql\SecondaryServer\InstanceName\AvailabilityGroups\MyAg `  
           -AllowDataLoss -Force  
        ```  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
3.  强制可用性组进行故障转移之后，请完成必要的后续步骤。 有关详细信息，请参阅本主题后面部分的[跟进：强制故障转移后的重要任务](#FollowUp)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-essential-tasks-after-a-forced-failover"></a><a name="FollowUp"></a> 跟进：强制故障转移后的重要任务  
  
1.  执行强制故障转移之后，故障转移到的辅助副本将变成新的主副本。 但是，要使客户端可访问可用性副本，您可能需要重新配置 WSFC 仲裁或调整可用性组的可用性模式配置，如下所示：  
  
    -   如果将故障转移到 [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)] 之外  ：调整 WSFC 节点的仲裁投票，以反映新的可用性组配置。 如果承载目标辅助副本的 WSFC 节点不具有 WSFC 仲裁投票，则可能需要强制 WSFC 仲裁。  
  
        > [!NOTE]  
        >  仅当两个可用性副本（包含之前的主副本）都配置为使用同步提交模式和自动故障转移时， [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)] 才存在。  
  
         **调整仲裁投票**  
  
        -   [查看群集仲裁 NodeWeight 设置](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
        -   [配置群集仲裁 NodeWeight 设置](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
        -   [在无仲裁情况下强制启动 WSFC 群集](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
    -   如果将故障转移到 [!INCLUDE[ssFosSync](../../../includes/ssfossync-md.md)] 之外  ：我们建议你考虑调整新的主要副本上和其他次要副本上的可用性模式和故障转移模式，以反映你所需的同步提交模式和自动故障转移配置。  
  
        > [!NOTE]  
        >  只有在当前主副本配置为同步提交模式时， [!INCLUDE[ssFosSync](../../../includes/ssfossync-md.md)] 才存在。  
  
         **更改可用性模式和故障转移模式**  
  
        -   [更改可用性副本的可用性模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
        -   [更改可用性副本的故障转移模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
2.  强制故障转移后，所有辅助数据库都处于挂起状态。 这包括以前的主数据库（在以前的主副本返回到联机状态并且发现它现在是辅助副本后）。 您必须单独在每个辅助副本上手动恢复每个挂起的数据库。  
  
     在恢复辅助数据库时，辅助数据库启动与相应主数据库之间的初始数据同步。 辅助数据库将回滚从未在新的主数据库上提交的所有日志记录。 因此，如果您担心在故障转移后主数据库上可能会丢失数据，则应该尝试在一个同步提交辅助数据库上创建已挂起数据库的数据库快照。  
  
    > [!IMPORTANT]  
    >  在其任何辅助数据库被挂起时，事务日志截断在主数据库上被延迟。 此外，只要任何本地数据库保持挂起状态，同步提交辅助副本的同步运行状况就无法转换到“正常”。  
  
     **创建数据库快照**  
  
    -   [创建数据库快照 (Transact-SQL)](../../../relational-databases/databases/create-a-database-snapshot-transact-sql.md)  
  
     **恢复可用性数据库**  
  
    -   [恢复可用性数据库 (SQL Server)](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
    > [!CAUTION]  
    >  在恢复所有辅助数据库之后，但在再次尝试故障转移该组之前，请等待下一个故障转移目标上的每个辅助数据库进入 SYNCHRONIZING 状态。 如果任何数据库尚未处于 SYNCHRONIZING 状态，则将阻止该数据库作为主数据库联机，并且重新建立该数据库的数据同步可能要求还原事务日志、还原完整的数据库备份或故障转移回之前的主副本。  
  
3.  如果失败的可用性副本将不返回到可用性副本，或返回太迟而延迟了新的主数据库上的事务日志截断，则考虑从该可用性组中删除此失败的副本，以免在针对您的日志文件的磁盘空间不足的情况下运行。  
  
     **删除辅助副本**  
  
    -   [将次要副本从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
4.  如果在一个或多个附加强制故障转移后执行一个强制故障转移，请在系列中的每个附加强制故障转移后执行日志备份。 有关这样做的原因的详细信息，请参阅 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)或 PowerShell 对 AlwaysOn 可用性组执行强制故障转移（可能会丢失数据）。  
  
     **执行日志备份**  
  
    -   [备份事务日志 (SQL Server)](../../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
##  <a name="example-scenario-using-a-forced-failover-to-recover-from-a-catastrophic-failure"></a><a name="ExampleRecoveryFromCatastrophy"></a> 示例应用场景：使用强制故障转移从灾难性故障中恢复  
 如果主副本失败并且没有同步的辅助副本可用，则强制可用性组进行故障转移可能是适当的反应。 强制执行故障转移是否合适取决于以下几个方面：(1) 是否希望主要副本处于脱机状态的时间超过服务级别协议 (SLA) 的容限，(2) 是否愿意为使主数据库更快处于可用状态，而承担数据可能丢失的风险。 如果您决定可用性组需要强制故障转移，则实际的强制故障转移将是多步骤过程中的一个步骤。  
  
 为了说明使用强制故障转移从灾难性故障中恢复所需的步骤，本主题提供了一个可能的灾难恢复应用场景。 此示例应用场景假定某一可用性组的原始拓扑结构由一个主数据中心和一个远程数据中心构成。主数据中心承载三个同步提交可用性副本，包括主副本；而远程数据中心承载两个异步提交辅助副本。 下图说明了此示例可用性组的原始拓扑结构。 该可用性组由一个多子网 WSFC 群集承载，并且在主数据中心具有三个节点（**节点 01**、 **节点 02** 和 **节点 03**），在远程数据中心有两个节点（**节点 04** 和 **节点 05**）。  
  
 ![可用性组的原始拓扑](../../../database-engine/availability-groups/windows/media/aoag-failurerecovery-origtopology.gif "可用性组的原始拓扑")  
  
 主数据中心意外关闭。 其三个可用性副本进入脱机状态并且其数据库变得不可用。 下图说明了此失败对该可用性组的拓扑的影响。  
  
 ![主数据中心故障之后的拓扑](../../../database-engine/availability-groups/windows/media/aoag-failurerecovery-catastrophy.gif "主数据中心故障之后的拓扑")  
  
 数据库管理员 (DBA) 确定可能的最佳响应是将可用性组强制故障转移到远程异步提交辅助副本中的一个。 此示例说明在您将该可用性组强制故障转移到远程副本并且最终将该可用性组返回到其原始拓扑时涉及的典型步骤。  
  
 这里介绍的故障响应包括以下两个阶段：  
  
-   [响应主数据中心的灾难性故障](#FailureResponse)  
  
-   [将可用性组返回到其原始拓扑](#ReturnToOrigTopology)  
  
###  <a name="responding-to-the-catastrophic-failure-of-the-main-data-center"></a><a name="FailureResponse"></a> Responding to the Catastrophic Failure of the Main Data Center  
 下图说明了为响应在主数据中心发生的灾难性故障而在远程数据中心执行的一系列操作。  
  
 ![主数据中心故障响应步骤](../../../database-engine/availability-groups/windows/media/aoag-failurerecovery-actions-part1.gif "主数据中心故障响应步骤")  
  
 在此图中的步骤说明以下步骤：  
  
|步骤|操作|链接|  
|----------|------------|-----------|  
|**1.**|数据库管理员或网络管理员确保 WSFC 群集具有运行状况正常的仲裁。 在此示例中，需要强制仲裁。|[WSFC 仲裁模式和投票配置 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md)<br /><br /> [通过强制仲裁进行 WSFC 灾难恢复 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)|  
|**2.**|数据库管理员连接到具有最少的延迟的服务器示例（在 **节点 04** 上）并且执行强制手动故障转移。 此强制故障转移将此次要副本转换为主角色并且挂起其余次要副本上的辅助数据库（在 **节点 05** 上）。|[sys.dm_hadr_database_replica_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) （查询 **sys.dm_hadr_database_replica_states** 列）。 有关详细信息，请参阅本主题前面的 [建议](#Recommendations)。）|  
|**3.**|数据库管理员手动恢复剩余辅助副本上的每个辅助数据库。|[恢复可用性数据库 (SQL Server)](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)|  
  
###  <a name="returning-the-availability-group-to-its-original-topology"></a><a name="ReturnToOrigTopology"></a> 将可用性组返回到其原始拓扑  
 下图说明了在主数据中心返回到联机状态并且 WSFC 节点重新建立与 WSFC 群集的通信后将该可用性组返回到其原始拓扑的一系列操作。  
  
> [!IMPORTANT]  
>  如果已强制 WSFC 群集仲裁，则在脱机节点重新启动时，只要下列两个条件均成立，它们就可以构成新的仲裁：(a) 在强制仲裁集的任何节点之间没有网络连接，(b) 重新启动的节点是群集中的大多数节点。 这可能会导致“裂脑”情形，即可用性组将拥有两个独立的主副本，在各数据中心各有一个。 强制仲裁以创建少数仲裁集之前，请参阅 [通过强制仲裁进行 WSFC 灾难恢复 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)或 PowerShell 对 AlwaysOn 可用性组执行强制故障转移（可能会丢失数据）。  
  
 ![将组返回至其原始拓扑的步骤](../../../database-engine/availability-groups/windows/media/aoag-failurerecovery-actions-part2.gif "将组返回至其原始拓扑的步骤")  
  
 在此图中的步骤说明以下步骤：  
  
||步骤|链接|  
|-|----------|-----------|  
|**1.**|主数据中心中的节点返回到联机状态，并且重新建立与 WSFC 群集的通信。 其可用性副本将作为具有挂起的数据库的辅助副本进入联机状态，数据库管理员将需要手动尽快恢复这些数据库中的每个数据库。|[恢复可用性数据库 (SQL Server)](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)<br /><br /> 提示：如果担心在故障转移后主数据库上可能会丢失数据，则应该尝试在同步提交辅助数据库上创建已挂起数据库的数据库快照。 请记住，在其任何辅助数据库被挂起时，事务日志截断在主数据库上被延迟。 此外，只要任何本地数据库保持挂起状态，则同步提交辅助副本的同步运行状况将无法转换到 HEALTHY。|  
|**2.**|在恢复数据库后，数据库管理员暂时将新的主副本更改为同步提交模式。 这涉及两个步骤：<br /><br /> 1) 将一个脱机可用性副本更改为异步提交模式。<br /><br /> 2) 将新的主副本更改为同步提交模式。 注意：此步骤将使已恢复的同步提交辅助数据库能够成为 SYNCHRONIZED。|[更改可用性副本的可用性模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)|  
|**3.**|在 **节点 03** 上同步提交次要副本（原始主要副本）进入 HEALTHY 同步状态后，数据库管理员将执行到该副本的计划的手动故障转移，使其再次成为主要副本。 **节点 04** 上的副本将恢复成为辅助副本。|[sys.dm_hadr_database_replica_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)<br /><br /> [使用 AlwaysOn 策略查看可用性组的运行状况 (SQL Server)](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)<br /><br /> [执行可用性组的计划手动故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)|  
|**4.**|数据库管理员将连接到新的主副本，并且：<br /><br /> 1) 将以前的主副本（在远程中心中）更改回异步提交模式。<br /><br /> 2) 将主数据中心中的异步提交辅助副本更改回同步提交模式。|[更改可用性副本的可用性模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)|  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **调整仲裁投票**  
  
-   [查看群集仲裁 NodeWeight 设置](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [配置群集仲裁 NodeWeight 设置](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
-   [在无仲裁情况下强制启动 WSFC 群集](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
 **计划的手动故障转移：**  
  
-   [执行可用性组的计划手动故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [使用故障转移可用性组向导 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)  
  
 **排除故障：**  
  
-   [AlwaysOn 可用性组配置故障排除 (SQL Server)](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
-   [添加文件操作失败的故障排除（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   **博客：**  
  
     [SQL Server Always On 团队博客：SQL Server Always On 团队官方博客](/archive/blogs/sqlalwayson/)  
  
     [CSS SQL Server 工程师博客](/archive/blogs/psssql/)  
  
-   **白皮书：**  
  
     [用于高可用性和灾难恢复的 Microsoft SQL Server AlwaysOn 解决方案指南](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [针对 SQL Server 2012 的 Microsoft 白皮书](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [SQL Server 客户咨询团队白皮书](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [关于对可用性副本的客户端连接访问 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 [监视可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [Windows Server 故障转移群集 (WSFC) 与 SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)  
  
