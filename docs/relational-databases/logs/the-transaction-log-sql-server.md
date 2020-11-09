---
title: 事务日志 (SQL Server) | Microsoft Docs
description: 了解事务日志。 每个 SQL Server 数据库都会记录系统发生故障时所需的全部事务和数据库修改。
ms.custom: ''
ms.date: 10/23/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- transaction logs [SQL Server], about
- databases [SQL Server], transaction logs
- logs [SQL Server], transaction logs
ms.assetid: d7be5ac5-4c8e-4d0a-b114-939eb97dac4d
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4d6e28a0e86a240d03ab4cdccac843488ff84446
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91869305"
---
# <a name="the-transaction-log-sql-server"></a>事务日志 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库都具有事务日志，用于记录所有事务以及每个事务对数据库所做的修改。
  
事务日志是数据库的一个关键组件。 如果系统出现故障，你将需要依靠该日志将数据库恢复到一致的状态。 

有关事务日志体系结构和内部组件的详细信息，请参阅 [SQL Server 事务日志体系结构和管理指南](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)。

> [!WARNING] 
> 永远不要删除或移动此日志，除非你完全了解执行此操作的后果。 

> [!TIP]
> 检查点会创建一些正常点，在数据库恢复期间将从这些正常点开始应用事务日志。 有关详细信息，请参阅[数据库检查点 (SQL Server)](../../relational-databases/logs/database-checkpoints-sql-server.md)。  
  
## <a name="operations-supported-by-the-transaction-log"></a>事务日志支持的操作  
 事务日志支持以下操作：  
  
-   恢复个别的事务。  
-   在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动时恢复所有未完成的事务。 
-   将还原的数据库、文件、文件组或页前滚至故障点。  
-   支持事务复制。  
-   支持高可用性和灾难恢复解决方案： [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]、数据库镜像和日志传送。

### <a name="individual-transaction-recovery"></a>恢复个别的事务
如果应用程序发出 `ROLLBACK` 语句，或者 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 检测到错误（例如失去与客户端的通信），就使用日志记录回滚未完成的事务所做的修改。 

### <a name="recovery-of-all-incomplete-transactions-when-ssnoversion-is-started"></a>在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动时恢复所有未完成的事务
当服务器发生故障时，数据库可能处于这样的状态：还没有将某些修改从缓存写入数据文件，在数据文件内有未完成的事务所做的修改。 当启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，它对每个数据库执行恢复操作。 前滚日志中记录的、可能尚未写入数据文件的每个修改。 在事务日志中找到的每个未完成的事务都将回滚，以确保数据库的完整性。 有关详细信息，请参阅[还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery)。

### <a name="rolling-a-restored-database-file-filegroup-or-page-forward-to-the-point-of-failure"></a>将还原的数据库、文件、文件组或页前滚至故障点
在硬件丢失或磁盘故障影响到数据库文件后，可以将数据库还原到故障点。 先还原上次完整数据库备份和上次差异数据库备份，然后将后续的事务日志备份序列还原到故障点。 

当还原每个日志备份时，[!INCLUDE[ssde_md](../../includes/ssde_md.md)] 重新应用日志中记录的所有修改，以前滚所有事务。 当最后的日志备份还原后，[!INCLUDE[ssde_md](../../includes/ssde_md.md)] 将使用日志信息回滚到该点未完成的所有事务。 有关详细信息，请参阅[还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery)。

### <a name="supporting-transactional-replication"></a>支持事务复制
日志读取器代理程序监视已为事务复制配置的每个数据库的事务日志，并将已设复制标记的事务从事务日志复制到分发数据库中。 有关详细信息，请参阅 [事务复制的工作原理](/previous-versions/sql/sql-server-2008-r2/ms151706(v=sql.105))。

### <a name="supporting-high-availability-and-disaster-recovery-solutions"></a>支持高可用性和灾难恢复解决方案
备用服务器解决方案、[!INCLUDE[ssHADR](../../includes/sshadr-md.md)]数据库镜像和日志传送极大程度地依赖于事务日志。 

在 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 方案中，数据库的每个更新（主要副本）在数据库的完整且独立的副本（次要副本）中直接再现。 主要副本直接将每个日志记录发送到次要副本，这可将传入日志记录应用到可用性组数据库，并不断前滚。 有关详细信息，请参阅 [AlwaysOn 故障转移群集实例](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)

在日志传送方案中，主服务器将主数据库的活动事务日志发送到一个或多个目标服务器。 每个辅助服务器将该日志还原为其本地的辅助数据库。 有关详细信息，请参阅 [关于日志传送](../../database-engine/log-shipping/about-log-shipping-sql-server.md)。 

在数据库镜像方案中，数据库（主体数据库）的每次更新都在独立的、完整的数据库（镜像数据库）副本中立即重新生成。 主体服务器实例立即将每个日志记录发送到镜像服务器实例，镜像服务器实例将传入的日志记录应用于镜像数据库，从而将其继续前滚。 有关详细信息，请参阅 [数据库镜像](../../database-engine/database-mirroring/database-mirroring-sql-server.md)。

##  <a name="transaction-log-characteristics"></a><a name="Characteristics"></a>事务日志特征
[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 事务日志的特征： 
-  事务日志是作为数据库中的单独的文件或一组文件实现的。 日志缓存与数据页的缓冲区高速缓存是分开管理的，因此可在[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]中生成简单、快速和功能强大的代码。 有关详细信息，请参阅[事务日志物理体系结构](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch)。

-  日志记录和页的格式不必遵守数据页的格式。

-  事务日志可以在几个文件上实现。 通过设置日志的 `FILEGROWTH` 值可以将这些文件定义为自动扩展。 这样可减少事务日志内空间不足的可能性，同时减少管理开销。 有关详细信息，请参阅 [ALTER DATABASE (Transact-SQL) 文件和文件组选项](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)。

-  重用日志文件中空间的机制速度快且对事务吞吐量影响最小。

有关事务日志体系结构和内部组件的详细信息，请参阅 [SQL Server 事务日志体系结构和管理指南](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)。

##  <a name="transaction-log-truncation"></a><a name="Truncation"></a> 事务日志截断  
日志截断将释放日志文件的空间，以便由事务日志重新使用。 必须定期截断事务日志，防止占满分配的空间。 几个因素可能延迟日志截断，因此监视日志大小很重要。 某些操作可以最小日志量进行记录以减少其对事务日志大小的影响。  
 
日志截断从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的逻辑事务日志中删除不活动的[虚拟日志文件 (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch)，释放逻辑日志中的空间以便物理事务日志重用这些空间。 如果事务日志从不截断，它最终将填满分配给物理日志文件的所有磁盘空间。  
  
为了避免空间不足，除非由于某些原因延迟日志截断，否则将在以下事件后自动进行截断：  
  
- 简单恢复模式下，在检查点之后发生。  
- 在完整恢复模式或大容量日志恢复模式下，如果自上一次备份后生成检查点，则在日志备份后进行截断（除非是仅复制日志备份）。  
  
 有关详细信息，请参阅本主题后面的[可能延迟日志截断的因素](#FactorsThatDelayTruncation)。  
  
> [!NOTE]
> 日志截断并不减小物理日志文件的大小。 若要减少物理日志文件的物理大小，则必须收缩日志文件。 有关收缩物理日志文件大小的信息，请参阅 [管理事务日志文件的大小](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md)。  
> 但是，请记住[可能延迟日志截断的因素](#FactorsThatDelayTruncation)。 如果在日志收缩后还需要存储空间，则会再次增加事务日志，导致在增加日志操作期间产生性能开销。
  
##  <a name="factors-that-can-delay-log-truncation"></a><a name="FactorsThatDelayTruncation"></a> 可能延迟日志截断的因素  
 在日志记录长时间处于活动状态时，事务日志截断将延迟，事务日志可能填满，这一点我们在本主题（很长）前面提到过。  
  
> [!IMPORTANT]
> 有关如何响应已满事务日志的信息，请参阅[解决事务日志已满的问题（SQL Server 错误 9002）](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)。  
  
 实际上，日志截断会由于多种原因发生延迟。 查询 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图的 **log_reuse_wait** 和 **log_reuse_wait_desc** 列，了解哪些因素（如果存在）阻止日志截断。 下表对这些列的值进行了说明。  
  
|log_reuse_wait 值|log_reuse_wait_desc 值|说明|  
|----------------------------|----------------------------------|-----------------|  
|0|NOTHING|当前有一个或多个可重复使用的[虚拟日志文件 (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch)。|  
|1|CHECKPOINT|自上次日志截断之后，尚未生成检查点，或者日志头尚未跨一个[虚拟日志 (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 文件移动。 （所有恢复模式）<br /><br /> 这是日志截断延迟的常见原因。 有关详细信息，请参阅[数据库检查点 (SQL Server)](../../relational-databases/logs/database-checkpoints-sql-server.md)。|  
|2|LOG_BACKUP|在截断事务日志前，需要进行日志备份。 （仅限完整恢复模式或大容量日志恢复模式）<br /><br /> 完成下一个日志备份后，一些日志空间可能变为可重复使用。|  
|3|ACTIVE_BACKUP_OR_RESTORE|数据备份或还原正在进行（所有恢复模式）。<br /><br /> 如果数据备份阻止了日志截断，则取消备份操作可能有助于解决备份直接导致的此问题。|  
|4|ACTIVE_TRANSACTION|事务处于活动状态（所有恢复模式）：<br /><br /> 一个长时间运行的事务可能存在于日志备份的开头。 在这种情况下，可能需要进行另一个日志备份才能释放空间。 请注意，长时间运行的事务将阻止所有恢复模式下的日志截断，包括简单恢复模式，在该模式下事务日志一般在每个自动检查点截断。<br /><br /> 延迟事务。 “延迟的事务  ”是有效的活动事务，因为某些资源不可用，其回滚受阻。 有关导致事务延迟的原因以及如何使它们摆脱延迟状态的信息，请参阅[延迟的事务 (SQL Server)](../../relational-databases/backup-restore/deferred-transactions-sql-server.md)。<br /> <br /> 长时间运行的事务也可能会填满 tempdb 的事务日志。 Tempdb 由用户事务隐式用于内部对象，例如用于排序的工作表、用于哈希的工作文件、游标工作表，以及行版本控制。 即使用户事务只包括读取数据（`SELECT` 查询），也可能会以用户事务的名义创建和使用内部对象， 然后就会填充 tempdb 事务日志。|  
|5|DATABASE_MIRRORING|数据库镜像暂停，或者在高性能模式下，镜像数据库明显滞后于主体数据库。 （仅限完整恢复模式）<br /><br /> 有关详细信息，请参阅[数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)。|  
|6|复制|在事务复制过程中，与发布相关的事务仍未传递到分发数据库。 （仅限完整恢复模式）<br /><br /> 有关事务复制的信息，请参阅 [SQL Server Replication](../../relational-databases/replication/sql-server-replication.md)。|  
|7|DATABASE_SNAPSHOT_CREATION|正在创建数据库快照。 （所有恢复模式）<br /><br /> 这是日志截断延迟的常见原因，通常也是主要原因。|  
|8|LOG_SCAN|发生日志扫描。 （所有恢复模式）<br /><br /> 这是日志截断延迟的常见原因，通常也是主要原因。|  
|9|AVAILABILITY_REPLICA|可用性组的辅助副本正将此数据库的事务日志记录应用到相应的辅助数据库。 （完整恢复模式）<br /><br /> 有关详细信息，请参阅： [AlwaysOn 可用性组概述 (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。|  
|10|-|仅供内部使用|  
|11|-|仅供内部使用|  
|12|-|仅供内部使用|  
|13|OLDEST_PAGE|如果将数据库配置为使用间接检查点，数据库中最早的页可能比检查点[日志序列号 (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) 早。 在这种情况下，最早的页可以延迟日志截断。 （所有恢复模式）<br /><br /> 有关间接检查点的信息，请参阅[数据库检查点 (SQL Server)](../../relational-databases/logs/database-checkpoints-sql-server.md)。|  
|14|OTHER_TRANSIENT|当前未使用此值。|  
|16|XTP_CHECKPOINT|需要执行内存中 OLTP 检查点。对于内存优化表，如果上次检查点后事务日志文件变得大于 1.5 GB（包括基于磁盘的表和内存优化表），则执行自动检查点<br /> 有关详细信息，请参阅[内存优化表的检查点操作](../../relational-databases/in-memory-oltp/checkpoint-operation-for-memory-optimized-tables.md)和[内存中优化表的日志记录和检查点流程](https://blogs.msdn.microsoft.com/sqlcat/2016/05/20/logging-and-checkpoint-process-for-memory-optimized-tables-2/)
  
##  <a name="operations-that-can-be-minimally-logged"></a><a name="MinimallyLogged"></a> 可尽量减少日志量的操作  
最小日志记录是指只记录在不支持时间点恢复的情况下恢复事务所需的信息。 本主题介绍在大容量日志 [恢复模式](../backup-restore/recovery-models-sql-server.md) 下（以及简单恢复模式下）按最小方式记录、但在运行备份时例外的操作。  
  
> [!NOTE]
> 内存优化表不支持最小日志记录。  
  
> [!NOTE]
> 在完整 [恢复模式](../backup-restore/recovery-models-sql-server.md)下，所有大容量操作都将被完整地记录下来。 但是，可以通过将数据库暂时切换到用于大容量操作的大容量日志恢复模式，最小化一组大容量操作的日志记录。 最小日志记录比完整日志记录更为有效，并在大容量事务期间，降低了大规模大容量操作填满可用的事务日志空间的可能性。 不过，如果在最小日志记录生效时数据库损坏或丢失，则无法将数据库恢复到故障点。  
  
 下列操作在完整恢复模式下执行完整日志记录，而在简单和大容量日志恢复模式下按最小方式记录日志：  
  
-   批量导入操作（[bcp](../../tools/bcp-utility.md)、[BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和 [INSERT...SELECT](../../t-sql/statements/insert-transact-sql.md)）。 有关在何时对大容量导入表按最小方式进行记录的详细信息，请参阅 [Prerequisites for Minimal Logging in Bulk Import](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md)。  
  
启用事务复制时，将完全记录 `BULK INSERT` 操作，即使处于大容量日志恢复模式下。  
  
-   [SELECT INTO](../../t-sql/queries/select-into-clause-transact-sql.md) 操作。  
  
启用事务复制时，将完全记录 `SELECT INTO` 操作，即使处于大容量日志恢复模式下。  
  
-   插入或追加新数据时，使用 [UPDATE](../../t-sql/queries/update-transact-sql.md) 语句中的 `.WRITE` 子句部分更新到大型值数据类型。 注意，在更新现有值时没有使用最小日志记录。 有关大型值数据类型的详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。  
  
-   在“text”、“ntext”和“image”数据类型列中插入或追加新数据时的 [WRITETEXT](../../t-sql/queries/writetext-transact-sql.md) 和 [UPDATETEXT](../../t-sql/queries/updatetext-transact-sql.md) 语句。 注意，在更新现有值时没有使用最小日志记录。  
  
    > [!WARNING]
    > `WRITETEXT` 和 `UPDATETEXT` 语句已被弃用；请避免在新的应用程序中使用它们。  
  
-   如果数据库设置为简单或大容量日志恢复模式，则无论是脱机还是联机执行操作，都会按最小方式记录一些索引 DDL 操作。 按最小方式记录的索引操作如下：  
  
    -   [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) 操作（包括索引视图）。  
  
    -   [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) REBUILD 或 DBCC DBREINDEX 操作。  
  
        > [!WARNING]
        > `DBCC DBREINDEX` 语句已被弃用；请勿在新的应用程序中使用该语句。  
  
        > [!NOTE]
        > 索引生成操作使用最小日志记录，但在并发执行备份时可能会延迟。 此延迟由使用简单或大容量日志恢复模式时的最小日志记录缓冲池页的同步要求引起。 
      
    -   [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md) 新堆重新生成（如果适用）。 `DROP INDEX` 操作期间将始终完整记录索引页的释放操作。

##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
**管理事务日志**  
  
-   [管理事务日志文件的大小](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md)  
  
-   [解决事务日志已满的问题（SQL Server 错误 9002）](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)  
  
**备份事务日志（完整恢复模式）**  
  
-   [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  

-   [在数据库损坏时备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md)

**还原事务日志（完整恢复模式）**  
  
-   [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
[SQL Server 事务日志体系结构和管理指南](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)   
[控制事务持续性](../../relational-databases/logs/control-transaction-durability.md)   
[在大容量导入中按最小方式记录日志的前提条件](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md)   
[SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)     
[还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery)      
[数据库检查点 (SQL Server)](../../relational-databases/logs/database-checkpoints-sql-server.md)   
[查看或更改数据库的属性](../../relational-databases/databases/view-or-change-the-properties-of-a-database.md)   
[恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)  
[事务日志备份 (SQL Server)](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)    
[sys.dm_db_log_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)  
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)    
  
