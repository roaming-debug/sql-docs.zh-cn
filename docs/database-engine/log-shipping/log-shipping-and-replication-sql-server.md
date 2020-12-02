---
title: 日志传送和复制 (SQL Server) | Microsoft Docs
description: 了解日志传送如何将由主数据库上执行的每个插入、更新或删除所组成的事务日志应用到辅助数据库上。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], log shipping and
- log shipping [SQL Server], replication and
ms.assetid: 132bebfd-0206-4d23-829a-b38e5ed17bc9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5b3518b9ffb201985c4ebef7de13c24c4345394f
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125652"
---
# <a name="log-shipping-and-replication-sql-server"></a>日志传送和复制 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  日志传送涉及单个数据库的两个副本，而这两个副本通常驻留在不同的计算机上。 在任何给定时间都只有一个数据库副本可供客户端使用。 该副本称为主数据库。 通过日志传送将客户端对主数据库所做的更新传播到数据库的另一副本（称为辅助数据库）。 日志传送涉及将由主数据库上执行的每个插入、更新或删除所组成的事务日志应用到辅助数据库上。  
  
 日志传送可以与复制一起使用，具有以下行为：  
  
-   日志传送故障转移后，复制不继续。 如果发生故障转移，复制代理不连接到辅助数据库，因此不将事务复制到订阅服务器。 如果发生到主数据库的故障回复，则复制恢复。 所有由日志传送从辅助数据库复制回主数据库的事务均被复制到订阅服务器。  
  
-   如果主数据库永久丢失，则可以重命名辅助数据库，以使复制可以继续。 本主题的其余部分说明处理此情况的要求和过程。 给出的示例是发布数据库（对日志传送最常见的数据库），但也可以对订阅数据库和分发数据库应用类似的过程。  
  
 有关无需重新配置复制即可恢复复制中所涉及数据库的信息，请参阅 [备份和还原复制的数据库](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)。  
  
> [!NOTE]  
>  建议使用数据库镜像而不使用日志传送，以便提供发布数据库的可用性。 有关详细信息，请参阅 [数据库镜像和复制 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)。  
  
## <a name="requirements-and-procedures-for-replicating-from-the-secondary-if-the-primary-is-lost"></a>主数据库丢失时从辅助数据库复制的要求和过程  
 注意以下要求和注意事项：  
  
-   如果主数据库包含多个发布数据库，则将所有发布数据库日志传送到同一个辅助数据库。  
  
-   辅助服务器实例的安装路径必须与主服务器实例的路径相同。 辅助服务器中的用户数据库位置必须与主服务器中的位置相同。  
  
-   在主服务器中备份服务主键。 该键将在辅助服务器中还原。 有关详细信息，请参阅 [BACKUP SERVICE MASTER KEY (Transact-SQL)](../../t-sql/statements/backup-service-master-key-transact-sql.md)。  
  
-   日志传送不保证不丢失数据。 主数据库上的失败可能导致丢失尚未备份的数据或失败期间所丢失备份的数据。  
  
### <a name="log-shipping-with-transactional-replication"></a>事务复制的日志传送  
 对于事务复制，日志传送的行为取决于 **sync with backup** 选项。 可以对发布数据库和分发数据库设置此选项；发布服务器的日志传送仅与对发布数据库的设置有关。  
  
 对发布数据库设置此选项可以确保事务在发布数据库上备份之前不会传递到分发数据库。 这样，便可以在辅助服务器中还原上次的发布数据库备份，而分发数据库不可能具有已还原发布数据库中没有的事务。 此选项可保证在发布服务器故障转移到辅助服务器时，发布服务器、分发服务器和订阅服务器之间保持一致性。 由于直到将事务在发布服务器中备份之后才能将其传递到分发数据库，因此滞后时间和吞吐量会受到影响；如果应用程序允许此滞后时间，则建议对发布数据库设置此选项。 如果未设置 **sync with backup** 选项，订阅服务器可能会收到辅助服务器中已恢复数据库中不再包含的更改。 有关详细信息，请参阅 [快照复制和事务复制的备份和还原策略](../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md)。  
  
 **使用 sync with backup 选项配置事务复制和日志传送**  
  
1.  如果未对发布数据库设置 sync with backup 选项，请执行 `sp_replicationdboption '<publicationdatabasename>', 'sync with backup', 'true'`。 有关详细信息，请参阅 [sp_replicationdboption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)。  
  
2.  为发布数据库配置日志传送。 有关详细信息，请参阅[配置日志传送 (SQL Server)](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)。  
  
3.  如果发布服务器出现故障，则使用 RESTORE LOG 的 KEEP_REPLICATION 选项，将上次的数据库日志还原到辅助服务器。 这将保留数据库的所有复制设置。 有关详细信息，请参阅[故障转移到日志传送辅助服务器 (SQL Server)](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) 和 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)。  
  
4.  将 **msdb** 数据库和 **master** 数据库从主服务器还原到辅助服务器。 有关详细信息，请参阅[备份和还原系统数据库 (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)。 如果主服务器同时也是分发服务器，则将分发数据库从主服务器还原到辅助服务器。  
  
     这些数据库在复制配置和设置方面必须与主服务器中的发布数据库一致。  
  
5.  在辅助服务器中，重命名计算机，再重命名 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，以便与主服务器名称匹配。 有关重命名计算机的信息，请参阅 Windows 文档。 有关重命名服务器的详细信息，请参阅 [重命名托管 SQL Server 的独立实例的计算机](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) 和 [重命名 SQL Server 故障转移群集实例](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md)。  
  
6.  在辅助服务器中，还原以前从主服务器备份的服务主键。 有关详细信息，请参阅 [RESTORE SERVICE MASTER KEY (Transact-SQL)](../../t-sql/statements/restore-service-master-key-transact-sql.md)。  
  
 **不设置 sync with backup 选项的情况下配置事务复制和日志传送**  
  
1.  为发布数据库配置日志传送。 有关详细信息，请参阅[配置日志传送 (SQL Server)](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)。  
  
2.  如果发布服务器出现故障，则使用 RESTORE LOG 的 KEEP_REPLICATION 选项，将上次的数据库日志还原到辅助服务器。 这将保留数据库的所有复制设置。 有关详细信息，请参阅[故障转移到日志传送辅助服务器 (SQL Server)](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) 和 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)。  
  
3.  将 **msdb** 数据库和 **master** 数据库从主服务器还原到辅助服务器。 有关详细信息，请参阅[备份和还原系统数据库 (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)。 如果主服务器同时也是分发服务器，则将分发数据库从主服务器还原到辅助服务器。  
  
     这些数据库在复制配置和设置方面必须与主服务器中的发布数据库一致。  
  
4.  在辅助服务器中，重命名计算机，再重命名 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，以便与主服务器名称匹配。 有关重命名计算机的信息，请参阅 Windows 文档。 有关重命名服务器的详细信息，请参阅 [重命名托管 SQL Server 的独立实例的计算机](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) 和 [重命名 SQL Server 故障转移群集实例](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md)。  
  
     可能从日志读取器代理会收到错误消息，指出发布数据库与分发数据库不同步。  
  
5.  在辅助服务器中，还原以前从主服务器备份的服务主键。 有关详细信息，请参阅 [RESTORE SERVICE MASTER KEY (Transact-SQL)](../../t-sql/statements/restore-service-master-key-transact-sql.md)。  
  
6.  执行 **sp_replrestart**。 该存储过程可用于强制日志读取器代理忽略发布数据库日志中所有以前复制的事务。 存储过程完成后应用的事务由日志读取器代理处理。 有关详细信息，请参阅 [sp_replrestart (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-replrestart-transact-sql.md)。  
  
7.  存储过程成功执行后，请重新启动日志读取器代理。 有关详细信息，请参阅[启动和停止复制代理 (SQL Server Management Studio)](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
8.  可以在发布服务器中应用已经分发到订阅服务器的事务。 为确保分发代理尝试在订阅服务器中重新应用这些事务时不会因错误而失败，请指定标题为“遇到数据一致性错误时继续” 的代理配置文件。  
  
### <a name="log-shipping-with-merge-replication"></a>合并复制的日志传送  
 请按照下面过程中的步骤配置合并复制和日志传送。  
  
 **配置合并复制和日志传送**  
  
1.  为发布数据库配置日志传送。 有关详细信息，请参阅[配置日志传送 (SQL Server)](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)。  
  
2.  如果发布服务器出现故障，则在辅助服务器中重命名计算机，然后重命名 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，以便与主服务器名称匹配。 有关重命名计算机的信息，请参阅 Windows 文档。 有关重命名服务器的详细信息，请参阅 [重命名托管 SQL Server 的独立实例的计算机](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) 和 [重命名 SQL Server 故障转移群集实例](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md)。  
  
3.  使用 RESTORE LOG 的 KEEP_REPLICATION 选项，将上次的数据库日志还原到辅助服务器。 这将保留数据库的所有复制设置。 有关详细信息，请参阅[故障转移到日志传送辅助服务器 (SQL Server)](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) 和 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)。  
  
4.  将 **msdb** 数据库和 **master** 数据库从主服务器还原到辅助服务器。 有关详细信息，请参阅[备份和还原系统数据库 (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)。 如果主服务器同时也是分发服务器，则将分发数据库从主服务器还原到辅助服务器。  
  
     这些数据库在复制配置和设置方面必须与主服务器中的发布数据库一致。  
  
5.  在辅助服务器中，还原以前从主服务器备份的服务主键。 有关详细信息，请参阅 [RESTORE SERVICE MASTER KEY (Transact-SQL)](../../t-sql/statements/restore-service-master-key-transact-sql.md)。  
  
6.  将发布数据库与一个或多个订阅数据库同步。 通过此操作可以上载那些以前在发布数据库中做出的但未在已还原备份中显示的更改。 可以上载的数据取决于筛选发布的方法：  
  
    -   如果发布未经筛选，则应能通过与最新订阅服务器同步来更新发布数据库。  
  
    -   如果发布经过筛选，则可能无法更新发布数据库。 假设有一个按如下方式分区的表：每个订阅仅收到一个区域（北部、东部、南部和西部）的客户数据。 如果每个数据分区至少有一个订阅服务器，那么使每个分区与订阅服务器同步会更新发布数据库。 但是，以西分区为例，如果其中的数据未复制到任何订阅服务器，那么发布服务器上的此数据就无法更新。 在这种情况下，建议重新初始化所有订阅，以使发布服务器和订阅服务器中的数据收敛。 有关详细信息，请参阅 [重新初始化订阅](../../relational-databases/replication/reinitialize-subscriptions.md)。  
  
     如果与运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之前的 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]版本的订阅服务器同步，则订阅无法匿名；它必须是客户端订阅或服务器订阅（在早期版本中称为本地订阅和全局订阅）。 有关详细信息，请参阅 [同步数据](../../relational-databases/replication/synchronize-data.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 复制](../../relational-databases/replication/sql-server-replication.md)   
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [数据库镜像和复制 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)  
  
  
