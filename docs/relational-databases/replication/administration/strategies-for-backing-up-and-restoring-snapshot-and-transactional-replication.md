---
title: 快照复制和事务复制的备份和还原策略 | Microsoft Docs
description: 了解有关在 SQL Server 中为快照和事务复制设计备份和还原策略的注意事项。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- backups [SQL Server replication], snapshot replication
- restoring [SQL Server replication], transactional replication
- snapshot replication [SQL Server], backup and restore
- restoring [SQL Server replication], snapshot replication
- recovery [SQL Server replication], transactional replication
- transactional replication, backup and restore
- recovery [SQL Server replication], snapshot replication
- sync with backup [SQL Server replication]
- backups [SQL Server replication], transactional replication
ms.assetid: a8afcdbc-55db-4916-a219-19454f561f9e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 1d395bebae8b009f4e91d8df074401f8659e1748
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467398"
---
# <a name="strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication"></a>快照复制和事务复制的备份和还原策略
[!INCLUDE[sql-asdb](../../../includes/applies-to-version/sql-asdb.md)]
  在设计快照和事务复制的备份和还原策略时，需要考虑三个方面：  
  
-   要备份哪些数据库。
-   事务复制的备份设置。
-   还原数据库需要的步骤。 这些步骤取决于所选的复制类型和选项。  
  
 本主题在下面三部分中分别介绍了这三个方面。 有关备份和还原 Oracle 发布的信息，请参阅 [Oracle 发布服务器的备份和还原](../../../relational-databases/replication/non-sql/backup-and-restore-for-oracle-publishers.md)。  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../../includes/azure-sql-db-replication-supportability-note.md)]
  
## <a name="backing-up-databases"></a>备份数据库  
 对于快照和事务复制，应定期对以下数据库进行备份：  
  
-   发布服务器上的发布数据库。  
  
-   分发服务器上的分发数据库。  
  
-   每台订阅服务器上的订阅数据库。  
  
-   发布服务器、分发服务器和所有订阅服务器上的 **master** 和 **msdb** 系统数据库。 当备份这些数据库中的一个数据库或相关的复制数据库时，应同时备份这些数据库。 例如，应在备份发布数据库的同时备份发布服务器上的 **master** 和 **msdb** 数据库。 如果还原发布数据库，请确保 **master** 和 **msdb** 数据库在复制配置和设置方面与发布数据库保持一致。  
  
 如果执行定期日志备份，则在日志备份中应捕获所有与复制相关的更改。 如果不执行日志备份，则当与复制相关的设置发生更改时，应执行备份。 有关详细信息，请参阅 [Common Actions Requiring an Updated Backup](../../../relational-databases/replication/administration/common-actions-requiring-an-updated-backup.md)。  
  
## <a name="backup-settings-for-transactional-replication"></a>事务复制的备份设置  
 事务复制包括使用 **sync with backup** 选项，可以对分发数据库和发布数据库设置此选项：  
  
-   建议您始终对分发数据库设置此选项。  
  
     对分发数据库设置此选项可以确保发布数据库日志中的事务在分发数据库上备份之前不会被截断。 分发数据库可以还原为上次的备份，并且任何缺失的事务都会从发布数据库传递到分发数据库。 复制将继续，而不会受到任何影响。  
  
     对分发数据库设置此选项不会影响复制滞后时间。 但是，设置该选项后，直到分发数据库中的相应事务备份已经完成，才会对发布数据库上的日志进行截断。 （这会导致发布数据库中生成较大的事务日志）。  
  
-   如果应用程序允许额外的滞后时间，则建议您对发布数据库设置此选项。  
  
     对发布数据库设置此选项可以确保事务在发布数据库上备份之前不会传递到分发数据库。 这样上次的发布数据库备份就可以在发布服务器上还原，而分发数据库不可能具有已还原发布数据库所没有的事务。  
  
     由于事务在发布服务器上备份之前无法传递到分发数据库，因此会影响滞后时间和吞吐量。 例如，如果事务日志每五分钟备份一次，则在发布服务器上提交事务与将事务依次传递到分发数据库和订阅服务器之间会另有五分钟的滞后时间。  
  
    > [!NOTE]  
    >  **sync with backup** 选项可确保发布数据库和分发数据库之间的一致性，但此选项不能保证数据不丢失。 例如，如果事务日志丢失，则在发布数据库或分发数据库中没有上次事务日志备份后提交的事务。 这与非复制数据库的行为相同。  
  
 **设置 sync with backup 选项**  
  
-   复制 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 编程：[为事务复制启用协调备份（复制 Transact-SQL 编程）](../../../relational-databases/replication/administration/enable-coordinated-backups-for-transactional-replication.md)  
  
## <a name="restoring-databases-involved-in-replication"></a>还原复制中涉及的数据库  
 如果最近的备份可用并且遵循正确的步骤，则可以还原复制拓扑中的所有数据库。 发布数据库的还原步骤取决于复制类型和所用的选项；但所有其他数据库的还原步骤与类型和选项无关。  
  
 复制支持将复制的数据库还原到从中创建备份的同一服务器和数据库。 如果将复制数据库的备份还原到其他服务器或数据库，则无法保留复制设置。 在这种情况下，您必须在还原备份后重新创建所有发布和订阅。  
  
### <a name="publisher"></a>发布者  
 下面介绍了下列类型复制的还原步骤：  
  
-   快照复制  
  
-   只读事务复制  
  
-   带有更新订阅的事务复制  
  
-   对等事务复制  
  
 本部分还介绍了 **msdb** 和 **master** 数据库的还原步骤，这些步骤与所有四种复制类型的还原步骤相同。  
  
#### <a name="publication-database-snapshot-replication"></a>发布数据库：快照复制  
  
1.  还原发布数据库的最新备份。 转到步骤 2。  
  
2.  发布数据库备份是否包含所有发布和订阅的最新配置？ 如果是，则还原已完成。 如果不是，则转到步骤 3。  
  
3.  从发布服务器、分发服务器和订阅服务器中删除复制配置，然后重新创建配置。 还原完成。  
  
     有关如何删除复制的详细信息，请参阅 [sp_removedbreplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)。  
  
#### <a name="publication-database-read-only-transactional-replication"></a>发布数据库：只读事务复制  
  
1.  还原发布数据库的最新备份。 转到步骤 2。  
  
2.  是否在故障发生前对发布数据库启用了 **sync with backup** 设置？ 如果是，则转到步骤 3；如果不是，则转到步骤 5。  
  
     如果此设置已启用，查询 `SELECT DATABASEPROPERTYEX('<PublicationDatabaseName>', 'IsSyncWithBackup')` 将返回“1”。  
  
3.  还原的备份是否已完成并且是最新的？ 它是否包含所有发布和订阅的最新配置？ 如果是，则还原已完成。 如果不是，则转到步骤 4。  
  
4.  还原的发布数据库中的配置信息不是最新的。 因此，必须确保订阅服务器包含分发数据库中所有未完成的命令，然后删除并重新创建复制配置。  
  
    1.  运行分发代理，直到所有订阅服务器都与分发数据库中的未完成命令同步。 通过使用复制监视器中的 **“未分发的命令”** 选项卡或通过查询分发数据库中的 [MSdistribution_status](../../../relational-databases/system-views/msdistribution-status-transact-sql.md) 视图，确保所有命令都已传递到订阅服务器。 转到步骤 b。  
  
         有关如何运行分发代理的详细信息，请参阅[启动和停止复制代理 &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) 和[复制代理可执行文件概念](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)。  
  
         有关如何验证命令的详细信息，请参阅[在分发数据库中查看复制的命令和其他信息（复制 Transact-SQL 编程）](../../../relational-databases/replication/monitor/view-replicated-commands-and-information-in-distribution-database.md)和[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。  
  
    2.  从发布服务器、分发服务器和订阅服务器中删除复制配置，然后重新创建配置。 重新创建订阅时，指定订阅服务器已包含数据。 还原完成。  
  
         有关如何删除复制的详细信息，请参阅 [sp_removedbreplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)。  
  
         有关如何指定订阅服务器已包含数据的详细信息，请参阅 [Initialize a Subscription Manually](../../../relational-databases/replication/initialize-a-subscription-manually.md)。  
  
5.  未对发布数据库设置 **sync with backup** 选项。 因此，还原备份中不包括的事务可能已传递到分发服务器和订阅服务器。 现在必须确保订阅服务器包含分发数据库中所有未完成的命令，然后手动将还原备份中未包含的所有事务都应用到发布数据库。  
  
    > [!IMPORTANT]  
    >  执行此过程会导致已发布的表还原到的时间点晚于从备份还原的其他未发布表的时间点。  
  
    1.  运行分发代理，直到所有订阅服务器都与分发数据库中的未完成命令同步。 通过使用复制监视器中的 **“未分发的命令”** 选项卡或通过查询分发数据库中的 [MSdistribution_status](../../../relational-databases/system-views/msdistribution-status-transact-sql.md) 视图，确保所有命令都已传递到订阅服务器。 转到步骤 b。  
  
         有关如何运行分发代理的详细信息，请参阅[启动和停止复制代理 &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) 和[复制代理可执行文件概念](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)。  
  
         有关如何验证命令的详细信息，请参阅[在分发数据库中查看复制的命令和其他信息（复制 Transact-SQL 编程）](../../../relational-databases/replication/monitor/view-replicated-commands-and-information-in-distribution-database.md)和[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。  
  
    2.  使用 [tablediff 实用工具](../../../tools/tablediff-utility.md) 或其他工具手动将发布服务器和订阅服务器同步。 这使您能够从订阅数据库恢复发布数据库备份中未包含的数据。 转到步骤 c。  
  
         有关 **tablediff** 实用工具的详细信息，请参阅 [比较所复制表的差异（复制编程）](../../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md)。  
  
    3.  还原的备份是否已完成并且是最新的？ 它是否包含所有发布和订阅的最新配置？ 如果是，执行 [sp_replrestart](../../../relational-databases/system-stored-procedures/sp-replrestart-transact-sql.md) 存储过程，使发布服务器元数据与分发服务器元数据重新同步。 还原完成。 如果不是，则转到步骤 d。  
  
    4.  从发布服务器、分发服务器和订阅服务器中删除复制配置，然后重新创建配置。 重新创建订阅时，指定订阅服务器已包含数据。 还原完成。  
  
         有关如何删除复制的详细信息，请参阅 [sp_removedbreplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)。  
  
         有关如何指定订阅服务器已包含数据的详细信息，请参阅 [Initialize a Subscription Manually](../../../relational-databases/replication/initialize-a-subscription-manually.md)。  
  
#### <a name="publication-database-transactional-replication-with-updating-subscriptions"></a>发布数据库：带有更新订阅的事务复制  
  
1.  还原发布数据库的最新备份。 转到步骤 2。  
  
2.  运行分发代理，直到所有订阅服务器都与分发数据库中的未完成命令同步。 通过使用复制监视器中的 **“未分发的命令”** 选项卡或通过查询分发数据库中的 [MSdistribution_status](../../../relational-databases/system-views/msdistribution-status-transact-sql.md) 视图，确保所有命令都已传递到订阅服务器。 转到步骤 3。  
  
     有关如何运行分发代理的详细信息，请参阅[启动和停止复制代理 &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) 和[复制代理可执行文件概念](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)。  
  
     有关如何验证命令的详细信息，请参阅[在分发数据库中查看复制的命令和其他信息（复制 Transact-SQL 编程）](../../../relational-databases/replication/monitor/view-replicated-commands-and-information-in-distribution-database.md)和[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。  
  
3.  如果使用的是排队更新订阅，则连接到每台订阅服务器，并从订阅数据库中的 [MSreplication_queue &#40;Transact-SQL&#41;](../../../relational-databases/system-tables/msreplication-queue-transact-sql.md) 表中删除所有行。 转到步骤 4。  
  
    > [!NOTE]  
    >  如果使用的是排队更新并且任何表中都包含标识列，则必须确保还原后分配正确的标识范围。 有关详细信息，请参阅[复制标识列](../../../relational-databases/replication/publish/replicate-identity-columns.md)。  
  
4.  现在必须确保订阅服务器包含分发数据库中所有未完成的命令，然后手动将还原备份中未包含的所有事务都应用到发布数据库。  
  
    > [!IMPORTANT]  
    >  执行此过程会导致已发布的表还原到的时间点晚于从备份还原的其他未发布表的时间点。  
  
    1.  运行分发代理，直到所有订阅服务器都与分发数据库中的未完成命令同步。 通过使用复制监视器或通过查询分发数据库中的 [MSdistribution_status](../../../relational-databases/system-views/msdistribution-status-transact-sql.md) 视图，确保所有命令都已传递到订阅服务器。 转到步骤 b。  
  
    2.  使用 [tablediff Utility](../../../tools/tablediff-utility.md) 或其他工具手动将发布服务器和订阅服务器同步。 这使您能够从订阅数据库恢复发布数据库备份中未包含的数据。 转到步骤 c。  
  
         有关 **tablediff** 实用工具的详细信息，请参阅 [比较所复制表的差异（复制编程）](../../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md)。  
  
    3.  还原的备份是否已完成并且是最新的？ 它是否包含所有发布和订阅的最新配置？ 如果是，执行 [sp_replrestart](../../../relational-databases/system-stored-procedures/sp-replrestart-transact-sql.md) 存储过程，使发布服务器元数据与分发服务器元数据重新同步。 还原完成。 如果不是，则转到步骤 d。  
  
    4.  从发布服务器、分发服务器和订阅服务器中删除复制配置，然后重新创建配置。 重新创建订阅时，指定订阅服务器已包含数据。 还原完成。  
  
         有关如何删除复制的详细信息，请参阅 [sp_removedbreplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)。  
  
         有关如何指定订阅服务器已包含数据的详细信息，请参阅 [Initialize a Subscription Manually](../../../relational-databases/replication/initialize-a-subscription-manually.md)。  
  
#### <a name="publication-database-peer-to-peer-transactional-replication"></a>发布数据库：@loopback_detection  
 在下列步骤中，复制数据库 **A**、 **B** 和 **C** 位于对等事务复制拓扑中。 数据库 **A** 和 **C** 处于联机状态，并且可以正常工作；数据库 **B** 是要还原的数据库。 在此介绍的过程，尤其是步骤 7、10 和 11，非常类似于向对等拓扑添加节点时所需的过程。 执行这些步骤最简单的方法是使用配置对等拓扑向导，但是您也可以使用存储过程。  
  
1.  运行分发代理以同步数据库 **A** 和 **C** 上的订阅。转到步骤 2。  
  
     有关如何运行分发代理的详细信息，请参阅[启动和停止复制代理 &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) 和[复制代理可执行文件概念](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)。  
  
2.  如果 **B** 使用的分发数据库仍可用，请运行分发代理以同步数据库 **B** 和 **A** 之间的订阅以及数据库 B 和 **C** 之间的订阅。转到步骤 3。  
  
3.  通过在 **B** 的分发数据库上执行 [sp_removedistpublisherdbreplication](../../../relational-databases/system-stored-procedures/sp-removedistpublisherdbreplication-transact-sql.md)，从 **B** 使用的分发数据库中删除元数据。转到步骤 4。  
  
4.  在数据库 **A** 和 **C** 上，删除对数据库 **B** 上的发布的订阅。转到步骤 5。  
  
     有关如何删除订阅的详细信息，请参阅 [Subscribe to Publications](../../../relational-databases/replication/subscribe-to-publications.md)。  
  
5.  对数据库 **A** 执行日志备份或完整备份。转到步骤 6。  
  
6.  在数据库 **B** 上还原数据库 **A** 的备份。数据库 **B** 现在具有数据库 **A** 的数据，但是不包含复制配置。 在将备份还原到其他服务器时，将删除复制；因此，已从数据库 **B** 中删除复制。转到步骤 7。  
  
7.  在数据库 **B** 上重新创建发布，然后重新创建数据库 **A** 和 **B** 之间的订阅。（在更后面的阶段处理涉及数据库 **C** 的订阅）。  
  
    1.  在数据库 **B** 上重新创建发布。转到步骤 b。  
  
    2.  在数据库 B 上重新创建对数据库 A 上的发布的订阅，同时指定该订阅应使用备份进行初始化（[sp_addsubscription ](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) 的 `@sync_type` 参数的值为 initialize with backup）  。 转到步骤 c。  
  
    3.  在数据库 A 上重新创建对数据库 B 上的发布的订阅，同时指定订阅服务器已包含数据（[sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) 的 `@sync_type` 参数的值为 replication support only）  。 转到步骤 8。  
  
8.  运行分发代理以同步数据库 **A** 和 **B** 上的订阅。如果发布的表中有标识列，则转到步骤 9。 否则，转到步骤 10。  
  
9. 还原后，在数据库 **A** 中为每个表分配的标识范围也将在数据库 **B** 中使用。确保还原的数据库 **B** 已收到发生故障的数据库 **B** 中传播到数据库 **A** 和数据库 **C** 的所有更改；然后重设每个表的标识范围种子。  
  
    1.  在数据库 B 上执行 [sp_requestpeerresponse](../../../relational-databases/system-stored-procedures/sp-requestpeerresponse-transact-sql.md)，并检索输出参数 `@request_id`。 转到步骤 b。  
  
    2.  默认情况下，分发代理设置为连续运行；因此，令牌应该自动发送到所有节点。 如果分发代理未以连续模式运行，请运行该代理。 有关详细信息，请参阅[复制代理可执行文件概念](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)或[启动和停止复制代理 (SQL Server Management Studio)](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)。 转到步骤 c。  
  
    3.  执行 [sp_helppeerresponses](../../../relational-databases/system-stored-procedures/sp-helppeerresponses-transact-sql.md)，执行时为其提供在步骤 b 中检索到的 `@request_id` 值。 请等到所有节点都指示它们已接收到对等请求。 转到步骤 d。  
  
    4.  使用 [DBCC CHECKIDENT](../../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md) 在数据库 **B** 中对每个表重设种子以确保使用适当的范围。 转到步骤 10。  
  
     有关如何管理标识范围的更多信息，请参阅[复制标识列](../../../relational-databases/replication/publish/replicate-identity-columns.md)的“为手动标识范围管理分配范围”部分。  
  
10. 此时，数据库 **B** 和数据库 **C** 没有直接连接，但它们将通过数据库 **A** 接收更改。如果拓扑包含任何运行 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 的节点，则转到步骤 11；否则转到步骤 12。  
  
11. 静默系统，在数据库 **B** 和 **C** 之间重新创建订阅。使系统静默包括停止对所有节点上已发布的表执行活动，并确保每个节点都已收到来自所有其他节点的所有更改。  
  
    1.  停止对等拓扑中已发布表上的所有活动。 转到步骤 b。  
  
    2.  在数据库 B 上执行 [sp_requestpeerresponse](../../../relational-databases/system-stored-procedures/sp-requestpeerresponse-transact-sql.md)，并检索输出参数 `@request_id`。 转到步骤 c。  
  
    3.  默认情况下，分发代理设置为连续运行；因此，令牌应该自动发送到所有节点。 如果分发代理未以连续模式运行，请运行该代理。 转到步骤 d。  
  
    4.  执行 [sp_helppeerresponses](../../../relational-databases/system-stored-procedures/sp-helppeerresponses-transact-sql.md)，执行时为其提供在步骤 b 中检索到的 `@request_id` 值。 请等到所有节点都指示它们已接收到对等请求。 转到步骤 e。  
  
    5.  在数据库 **B** 上重新创建对数据库 **C** 上的发布的订阅，同时指定订阅服务器已包含数据。 转到步骤 b。  
  
    6.  在数据库 **C** 上重新创建对数据库 **B** 上的发布的订阅，同时指定订阅服务器已包含数据。 转到步骤 13。  
  
12. 在数据库 **B** 和 **C** 之间重新创建订阅：  
  
    1.  在数据库 **B** 上，查询 [MSpeer_lsns](../../../relational-databases/system-tables/mspeer-lsns-transact-sql.md) 表，以检索数据库 **B** 从 **C** 收到的最近事务的日志序列号 (LSN)。  
  
    2.  在数据库 B 上重新创建对数据库 C 上的发布的订阅，同时指定该订阅应基于 LSN 进行初始化（[sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)的 `@sync_type` 参数的值为 initialize from lsn）  。 转到步骤 b。  
  
    3.  在数据库 **C** 上重新创建对数据库 **B** 上的发布的订阅，同时指定订阅服务器已包含数据。 转到步骤 13。  
  
13. 运行分发代理以同步数据库 **B** 和 **C** 上的订阅。还原完成。  
  
#### <a name="msdb-database-publisher"></a>msdb 数据库（发布服务器）  
  
1.  还原 **msdb** 数据库的最新备份。  
  
2.  还原的备份是否已完成并且是最新的？ 它是否包含所有发布和订阅的最新配置？ 如果是，则恢复完成。 如果不是，则转到步骤 3。  
  
3.  从复制脚本重新创建订阅清除作业。 恢复完成。  
  
#### <a name="master-database-publisher"></a>master 数据库（发布服务器）  
  
1.  还原 **master** 数据库的最新备份。  
  
2.  确保该数据库在复制配置和设置方面与发布数据库一致。  
  
### <a name="databases-at-the-distributor"></a>分发服务器上的数据库  
  
#### <a name="distribution-database"></a>分发数据库  
  
1.  还原分发数据库的最新备份。  
  
2.  是否在故障发生前对分发数据库启用了 **sync with backup** 设置？ 如果是，则转到步骤 3；如果不是，则转到步骤 4。  
  
     如果此设置已启用，查询 `SELECT DATABASEPROPERTYEX('<DistributionDatabaseName>', 'IsSyncWithBackup')` 将返回“1”。  
  
3.  还原的备份是否已完成并且是最新的？ 它是否包含所有发布和订阅的最新配置？ 如果是，则恢复完成。 如果不是，则转到步骤 4。  
  
4.  已还原的分发数据库中的配置信息不是最新的，或未对分发数据库设置 **sync with backup** 选项。 （还原后，分发数据库可能会丢失已在发布服务器上提交但尚未传递到订阅服务器的事务）。删除并重新创建复制，然后运行验证。  
  
    1.  从发布服务器、分发服务器和订阅服务器中删除复制配置，然后重新创建配置。 重新创建订阅时，指定订阅服务器已包含数据。 转到步骤 b。  
  
         有关如何删除复制的详细信息，请参阅 [sp_removedbreplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)。  
  
         有关如何指定订阅服务器已包含数据的详细信息，请参阅 [Initialize a Subscription Manually](../../../relational-databases/replication/initialize-a-subscription-manually.md)。  
  
    2.  将所有发布标记为要验证。 重新初始化所有验证失败的订阅。 恢复完成。  
  
         有关验证的详细信息，请参阅 [Validate Replicated Data](../../../relational-databases/replication/validate-data-at-the-subscriber.md)。 有关重新初始化的详细信息，请参阅[重新初始化订阅](../../../relational-databases/replication/reinitialize-subscriptions.md)。  
  
#### <a name="msdb-database-distributor"></a>msdb 数据库（分发服务器）  
  
1.  还原 **msdb** 数据库的最新备份。  
  
2.  还原的备份是否已完成并且是最新的？ 它是否包含所有发布和订阅的最新配置？ 如果是，则恢复完成。 如果不是，则转到步骤 3。  
  
3.  从发布服务器、分发服务器和订阅服务器中删除复制配置，然后重新创建配置。 重新创建订阅时，指定订阅服务器已包含数据。 转到步骤 4。  
  
     有关如何删除复制的详细信息，请参阅 [sp_removedbreplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)。  
  
     有关如何指定订阅服务器已包含数据的详细信息，请参阅 [Initialize a Subscription Manually](../../../relational-databases/replication/initialize-a-subscription-manually.md)。  
  
4.  将所有发布标记为要验证。 重新初始化所有验证失败的订阅。 恢复完成。  
  
     有关验证的详细信息，请参阅 [Validate Replicated Data](../../../relational-databases/replication/validate-data-at-the-subscriber.md)。 有关重新初始化的详细信息，请参阅[重新初始化订阅](../../../relational-databases/replication/reinitialize-subscriptions.md)。  
  
#### <a name="master-database-distributor"></a>master 数据库（分发服务器）  
  
1.  还原 **master** 数据库的最新备份。  
  
2.  确保该数据库在复制配置和设置方面与发布数据库一致。  
  
### <a name="databases-at-the-subscriber"></a>订阅服务器上的数据库  
  
#### <a name="subscription-database"></a>订阅数据库  
  
1.  最新的订阅数据库备份是否晚于分发数据库上的最小分发保持期设置？ （这可确定分发服务器是否仍具有使订阅服务器保持最新所需的所有命令？）如果是，则转到步骤 2。 如果否，则重新初始化订阅。 恢复完成。  
  
     若要确定最大分发保持期设置，请执行 [sp_helpdistributiondb](../../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md) ，并从 **max_distretention** 列检索值（此值以小时为单位）。  
  
     有关如何重新初始化订阅的详细信息，请参阅 [Reinitialize a Subscription](../../../relational-databases/replication/reinitialize-a-subscription.md)。  
  
2.  还原订阅数据库的最新备份。 转到步骤 3。  
  
3.  如果订阅数据库仅包含推送订阅，则转到步骤 4。 如果订阅数据库包含任何请求订阅，则询问以下问题：订阅信息是否是最新的？ 数据库是否包含故障发生时设置的所有表和选项？ 如果是，则转到步骤 4。 如果否，则重新初始化订阅。 恢复完成。  
  
4.  若要同步订阅服务器，运行分发代理。 恢复完成。  
  
     有关如何运行分发代理的详细信息，请参阅[启动和停止复制代理 &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) 和[复制代理可执行文件概念](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)。  
  
#### <a name="msdb-database-subscriber"></a>msdb 数据库（订阅服务器）  
  
1.  还原 **msdb** 数据库的最新备份。 是否在此订阅服务器上使用请求订阅？ 如果是，则还原完成。 如果是，则转到步骤 2。  
  
2.  还原的备份是否已完成并且是最新的？ 它是否包含所有请求订阅的最新配置？ 如果是，则恢复完成。 如果不是，则转到步骤 3。  
  
3.  删除并重新创建请求订阅。 重新创建订阅时，指定订阅服务器已包含数据。 还原完成。  
  
     有关如何删除订阅的详细信息，请参阅 [Subscribe to Publications](../../../relational-databases/replication/subscribe-to-publications.md)。  
  
     有关如何指定订阅服务器已包含数据的详细信息，请参阅 [Initialize a Subscription Manually](../../../relational-databases/replication/initialize-a-subscription-manually.md)。  
  
#### <a name="master-database-subscriber"></a>master 数据库（订阅服务器）  
  
1.  还原 **master** 数据库的最新备份。  
  
2.  确保该数据库在复制配置和设置方面与发布数据库一致。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 数据库的备份和还原](../../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [备份和还原复制的数据库](../../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
 [“配置分发”](../../../relational-databases/replication/configure-distribution.md)   
 [发布数据和数据库对象](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Subscribe to Publications](../../../relational-databases/replication/subscribe-to-publications.md)   
 [初始化订阅](../../../relational-databases/replication/initialize-a-subscription.md)   
 [同步数据](../../../relational-databases/replication/synchronize-data.md)  
  
  
