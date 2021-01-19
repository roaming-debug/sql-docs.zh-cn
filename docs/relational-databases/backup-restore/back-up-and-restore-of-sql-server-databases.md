---
title: SQL Server 数据库的备份和还原 | Microsoft Docs
description: 本文介绍备份 SQL Server 数据库的优点，并介绍备份和还原策略以及安全注意事项。
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- disaster recovery [SQL Server], see restoring [SQL Server]
- backups [SQL Server]
- restoring databases [SQL Server]
- backup [SQL Server], see backing up [SQL Server]
- databases [SQL Server], restoring
- backing up databases [SQL Server]
- restore [SQL Server], see restoring [SQL Server]
- disaster recovery [SQL Server], see backing up [SQL Server]
- backing up [SQL Server]
- Database Engine [SQL Server], backups
- databases [SQL Server], backups
ms.assetid: 570a21b3-ad29-44a9-aa70-deb2fbd34f27
author: cawrites
ms.author: chadam
ms.openlocfilehash: 76aa3e890169e73d30d9a15aa57aa4fd832a7ef9
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171309"
---
# <a name="back-up-and-restore-of-sql-server-databases"></a>SQL Server 数据库的备份和还原
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本文介绍备份 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的优点、基本的备份和还原术语，还介绍 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的备份和还原策略以及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份和还原的安全注意事项。 

> 本文介绍了 SQL Server 备份。 有关备份 SQL Server 数据库的特定步骤，请参阅[创建备份](#creating-backups)。
  
 SQL Server 备份和还原组件为保护存储在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中的关键数据提供了基本安全保障。 为了尽量降低灾难性数据丢失的风险，需备份数据库，以便定期保存对数据的修改。 计划良好的备份和还原策略有助于保护数据库，使之免受各种故障导致的数据丢失的威胁。 测试策略，方法是先还原一组备份，然后恢复数据库，以便准备好对灾难进行有效的响应。
  
 除了在本地存储中存储备份外，SQL Server 还支持备份到 Azure Blob 存储服务和从其还原。 有关详细信息，请参阅[使用 Microsoft Azure Blob 存储服务执行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)。 对于使用 Microsoft Azure Blob 存储服务存储的数据库文件， [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 提供相应的选项让你使用 Azure 快照来实现接近实时的备份和更快的还原。 有关详细信息，请参阅 [Azure 中数据库文件的文件快照备份](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)。 Azure 还为 Azure VM 中运行的 SQL Server 提供企业级备份解决方案。 作为完全托管的备份解决方案，它支持 AlwaysOn 可用性组、长期保留、时点恢复以及集中管理和监视。 有关详细信息，请参阅 [Azure VM 中 SQL Server 的 Azure 备份](/azure/backup/backup-azure-sql-database)。
  
##  <a name="why-back-up"></a>为何备份？  
-   备份 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库、在备份上运行测试还原过程以及在另一个安全位置存储备份副本可防止可能的灾难性数据丢失。 备份是保护数据的唯一方法  。

     使用有效的数据库备份，可从多种故障中恢复数据，例如：  
  
    -   介质故障。    
    -   用户错误（例如，误删除了某个表）。    
    -   硬件故障（例如，磁盘驱动器损坏或服务器报废）。    
    -   自然灾难。 通过使用 SQL Server 备份到 Azure Blob 存储服务，可以在本地位置之外的其他区域创建一个站外备份，这样在发生影响本地位置的自然灾难时仍可以使用数据库。  
  
-   此外，数据库备份对于进行日常管理（如将数据库从一台服务器复制到另一台服务器、设置 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 或数据库镜像以及进行存档）非常有用。  
  
##  <a name="glossary-of-backup-terms"></a>备份术语的术语表
 **备份** [动词]  
 创建备份 [名词]  的过程，方法是通过复制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中的数据记录或复制其事务日志中的日志记录。  
  
 **备份** [名词]  
 可用于在出现故障后还原或恢复数据的数据副本。 数据库备份还可用于将数据库副本还原到新位置。  
  
**备份** 设备  
 要写入 SQL Server 备份及能从中还原这些备份的磁盘或磁带设备。 SQL Server 备份也可以写入 Azure Blob 存储服务，并且使用 URL  格式来指定备份文件的目标和名称。 有关详细信息，请参阅[使用 Microsoft Azure Blob 存储服务执行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)。  
  
**备份介质**  
 已写入一个或多个备份的一个或多个磁带或磁盘文件。  
  
**数据备份 (data backup)**  
 完整数据库的数据备份（数据库备份）、部分数据库的数据备份（部分备份）或一组数据文件或文件组的数据备份（文件备份）。  
  
**数据库备份 (database backup)**  
 数据库的备份。 完整数据库备份表示备份完成时的整个数据库。 差异数据库备份只包含自最近完整备份以来对数据库所做的更改。  
  
**差异备份 (differential backup)**  
 一种数据备份，基于完整数据库或部分数据库或一组数据文件或文件组（差异基准）的最新完整备份，并且仅包含自确定差异基准以来发生更改的数据。  
  
**完整备份 (full backup)**  
 一种数据备份，包含特定数据库或者一组特定的文件组或文件中的所有数据，以及可以恢复这些数据的足够的日志。  
  
**日志备份 (log backup)**  
 包括以前日志备份中未备份的所有日志记录的事务日志备份。 （完整恢复模式）  
  
**recover**  
 将数据库恢复到稳定且一致的状态。  
  
**recovery**  
 将数据库恢复到事务一致状态的数据库启动阶段或 Restore With Recovery 阶段。  
  
**恢复模式**  
 用于控制数据库上的事务日志维护的数据库属性。 有三种恢复模式：简单恢复模式、完整恢复模式和大容量日志恢复模式。 数据库的恢复模式确定其备份和还原要求。  
  
**还原 (restore)**  
 一种包括多个阶段的过程，用于将指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份中的所有数据和日志页复制到指定数据库，然后通过应用记录的更改使该数据在时间上向前移动，以前滚备份中记录的所有事务。  
  
 ##  <a name="backup-and-restore-strategies"></a>备份和还原策略  
 备份和还原数据必须根据特定环境进行自定义，并且必须使用可用资源。 因此，要可靠地使用备份和还原进行恢复，需要制定备份和还原策略。设计完善的备份和还原策略可以平衡业务需求，以实现最大的数据可用性和最小的数据丢失，同时考虑维护和存储备份的成本。  

 备份和还原策略包含备份部分和还原部分。 策略的备份部分定义备份的类型和频率、备份所需硬件的特性和速度、备份的测试方法以及备份介质的存储位置和方法（包括安全注意事项）。 策略的还原部分定义负责执行还原的人员、如何执行还原以满足数据库可用性和最大程度减少数据丢失的目标，以及如何测试还原。 
  
 设计有效的备份和还原策略需要仔细计划、实现和测试。 需要进行测试：直到成功还原了还原策略中包含的所有组合中的备份并且测试了还原的数据库是否具有物理一致性后，才会生成备份策略。 必须考虑各种因素。 其中包括：  
  
- 组织在生产数据库方面的目标，尤其是对可用性和防止数据丢失或损坏的要求。  
  
-  每个数据库的特性包括：大小、使用模式、内容特性以及数据要求等。  
  
-   对资源的约束，例如：硬件、人员、备份介质的存储空间以及所存储介质的物理安全性等。  

## <a name="best-practice-recommendations"></a>最佳做法建议

### <a name="use-separate-storage"></a>使用独立的存储 
> [!IMPORTANT] 
> 确保将数据库备份放在与数据库文件不同的物理位置或设备上。 存储数据库的物理驱动器出现故障或崩溃时，可恢复性取决于能否访问存储备份的独立驱动器或远程设备以执行还原。 请记住，你可以在同一个物理磁盘驱动器中创建多个逻辑卷或分区。 在为备份选择存储位置之前，请仔细研究磁盘分区和逻辑卷布局。

### <a name="choose-appropriate-recovery-model"></a>选择适当的恢复模式
 备份和还原操作发生在[恢复模式](../backup-restore/recovery-models-sql-server.md)的上下文中。 恢复模式是一种数据库属性，用于控制事务日志的管理方式。 因此，数据库的恢复模式决定了数据库支持的备份类型和还原方案，以及事务日志备份的大小。 通常，数据库使用简单恢复模式或完整恢复模式。 可以在执行大容量操作之前切换到大容量日志恢复模式，以补充完整恢复模式。 有关这些恢复模式以及它们是如何影响事务日志管理方式的说明，请参阅 [事务日志 (SQL Server)](../logs/the-transaction-log-sql-server.md)。  
  
 数据库的最佳恢复模式取决于您的业务要求。 若要免去事务日志管理工作并简化备份和还原，请使用简单恢复模式。 若要在管理开销一定的情况下使工作丢失的可能性降到最低，请使用完整恢复模式。 为了在大容量日志操作期间最大程度减少对日志大小的影响，同时允许这些操作的可恢复性，请使用大容量日志恢复模式。 有关恢复模式对备份和还原存在哪些影响的信息，请参阅 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)。  
  
### <a name="design-your-backup-strategy"></a>设计备份策略  
 当为特定数据库选择了满足业务要求的恢复模式后，需要计划并实现相应的备份策略。 最佳备份策略取决于各种因素，以下因素尤其重要：  
  
-   一天中应用程序访问数据库的时间有多长？  
  
     如果存在一个可预测的非高峰时段，则建议您将完整数据库备份安排在此时段。  
  
-   更改和更新可能发生的频率如何？  
  
     如果更改经常发生，请考虑下列事项：  
  
    -   在简单恢复模式下，请考虑将差异备份安排在完整数据库备份之间。 差异备份只能捕获自上次完整数据库备份之后的更改。  
  
    -   在完整恢复模式下，应安排经常的日志备份。 在完整备份之间安排差异备份可减少数据还原后需要还原的日志备份数，从而缩短还原时间。  
  
-   可能只是更改数据库的小部分内容，还是需要更改数据库的大部分内容？  
  
     对于更改集中于部分文件或文件组的大型数据库，部分备份和/或文件备份非常有用。 有关详细信息，请参阅[部分备份 (SQL Server)](../../relational-databases/backup-restore/partial-backups-sql-server.md) 和[完整文件备份 (SQL Server)](../../relational-databases/backup-restore/full-file-backups-sql-server.md)。  
  
-   完整数据库备份需要多少磁盘空间？  
-   你的企业需要维护过去多久的备份？ 

    确保你已根据应用程序需求和业务需求制定了适当的备份计划。 随着备份变得陈旧，数据丢失风险会更高，除非你有办法重新生成故障点之前的所有数据。 由于存储资源限制而选择处理旧备份之前，请考虑是否需要以前的可恢复性

  
 ### <a name="estimate-the-size-of-a-full-database-backup"></a>估计完整数据库备份的大小  
 在实现备份与还原策略之前，应当估计完整数据库备份将使用的磁盘空间。 备份操作会将数据库中的数据复制到备份文件。 备份仅包含数据库中的实际数据，而不包含任何未使用的空间。 因此，备份通常小于数据库本身。 你可以使用 **sp_spaceused** 系统存储过程估计完整数据库备份的大小。 有关详细信息，请参阅 [sp_spaceused (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)。  
  
### <a name="schedule-backups"></a>计划备份  
 执行备份操作对运行中的事务影响很小，因此可以在正常操作过程中执行备份操作。 您可以在对生产工作负荷的影响很小的情况下执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份。  
   
>  有关备份过程中的并发限制的信息，请参阅 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)。  
  
 确定所需的备份类型和必须执行每种备份类型的频率后，建议您将定期备份计划为数据库维护计划的一部分。 有关维护计划以及如何为数据库备份和日志备份创建维护计划的信息，请参阅 [Use the Maintenance Plan Wizard](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)。
  
### <a name="test-your-backups"></a>测试备份！  
 直到完成备份测试后，才会生成还原策略。 必须通过将数据库副本还原到测试系统，针对每个数据库的备份策略进行全面测试。 您必须对每种要使用的备份类型进行还原测试。 另外建议在还原备份后，通过数据库的 DBCC CHECKDB 执行数据库一致性检查，以验证备份媒体是否未损坏。 

### <a name="verify-media-stability-and-consistency"></a>验证媒体稳定性和一致性
使用备份实用工具提供的验证选项（BACKUP T-SQL 命令、SQL Server 维护计划、备份软件或解决方案等）。 有关示例，请参阅 [RESTORE VERIFYONLY] (../t-sql/statements/restore-statements-verifyonly-transact-sql.md) 使用 BACKUP CHECKSUM 等高级功能来检测备份媒体本身的问题。 有关详情，请参阅[在备份和还原期间可能的介质错误 (SQL Server)](../backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)

### <a name="document-backuprestore-strategy"></a>文档备份/还原策略 
建议您将备份和还原过程记录下来并在运行手册中保留记录文档的副本。
我们还建议你为每个数据库维护一个操作手册。 此操作手册应记录备份的位置、备份设备名称（如果有），以及还原测试备份所需的时间。



## <a name="monitor-progress-with-xevent"></a>使用 xEvent 监视进度
由于数据库的大小和所涉及操作的复杂性，备份和还原操作可能需要很长时间。 当任一操作出现问题时，可使用 backup_restore_progress_trace 扩展事件来监控实时进度  。 有关扩展事件的详细信息，请参阅 [扩展事件](../extended-events/extended-events.md)。

  >[!WARNING]
  > 使用 backup_restore_progress_trace 扩展事件可能会导致性能问题并使用大量磁盘空间。 请在短时间内谨慎使用，并在生产中实现前进行彻底测试。


```sql
-- Create the backup_restore_progress_trace extended event esssion
CREATE EVENT SESSION [BackupRestoreTrace] ON SERVER 
ADD EVENT sqlserver.backup_restore_progress_trace
ADD TARGET package0.event_file(SET filename=N'BackupRestoreTrace')
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=5 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)
GO

-- Start the event session  
ALTER EVENT SESSION [BackupRestoreTrace]  
ON SERVER  
STATE = start;  
GO  

-- Stop the event session  
ALTER EVENT SESSION [BackupRestoreTrace]  
ON SERVER  
STATE = stop;  
GO  
```

### <a name="sample-output-from-extended-event"></a>扩展事件的示例输出 

![备份 xevent 输出示例](media/back-up-and-restore-of-sql-server-databases/backup-xevent-example.png)
![还原 xevent 输出示例](media/back-up-and-restore-of-sql-server-databases/restore-xevent-example.png)
 
  
## <a name="more-about-backup-tasks"></a>有关备份任务的详细信息  
-   [创建维护计划](../../relational-databases/maintenance-plans/create-a-maintenance-plan.md)  
  
-   [创建作业](../../ssms/agent/create-a-job.md)  
  
-   [安排作业计划](../../ssms/agent/schedule-a-job.md)  
  
## <a name="working-with-backup-devices-and-backup-media"></a>使用备份设备和备份媒体  
-   [为磁盘文件定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)  
  
-   [为磁带驱动器定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
-   [将磁盘或磁带指定为备份目标 (SQL Server)](../../relational-databases/backup-restore/specify-a-disk-or-tape-as-a-backup-destination-sql-server.md)  
  
-   [删除备份设备 (SQL Server)](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md)  
  
-   [设置备份的过期日期 (SQL Server)](../../relational-databases/backup-restore/set-the-expiration-date-on-a-backup-sql-server.md)  
  
-   [查看备份磁带或文件的内容 (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [查看备份集中的数据文件和日志文件 (SQL Server)](../../relational-databases/backup-restore/view-the-data-and-log-files-in-a-backup-set-sql-server.md)  
  
-   [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   [从设备还原备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-backup-from-a-device-sql-server.md)  
  
## <a name="creating-backups"></a>创建备份  
**注意！** 对于部分备份或仅复制备份，必须分别使用带 PARTIAL 或 COPY_ONLY 选项的 [!INCLUDE[tsql](../../includes/tsql-md.md)][BACKUP](../../t-sql/statements/backup-transact-sql.md) 语句。  
  
 ### <a name="using-ssms"></a>使用 SSMS   
-   [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [备份文件和文件组 (SQL Server)](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)  
  
-   [创建差异数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
 ### <a name="using-t-sql"></a>使用 T-SQL  
-   [使用资源调控器限制备份压缩的 CPU 使用量 (Transact-SQL)](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)  
  
-   [在数据库损坏时备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md)  
  
-   [在备份或还原期间启用或禁用备份校验和 (SQL Server)](../../relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server.md)  
  
-   [指定备份或还原操作在遇到错误后是继续还是停止 (SQL Server)](../../relational-databases/backup-restore/specify-if-backup-or-restore-continues-or-stops-after-error.md)  
  
## <a name="restore-data-backups"></a>还原数据备份 
### <a name="using-ssms"></a>使用 SSMS 
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [将数据库还原到新位置 (SQL Server)](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
  
-   [还原差异数据库备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-differential-database-backup-sql-server.md)  
  
-   [还原文件和文件组 (SQL Server)](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
### <a name="using-t-sql"></a>使用 T-SQL
-   [在简单恢复模式下还原数据库备份 (Transact-SQL)](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md)  
  
-   [在完整恢复模式下将数据库还原到故障点 (Transact-SQL)](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [在现有文件上还原文件和文件组 (SQL Server)](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
-   [将文件还原到新位置 (SQL Server)](../../relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
  
-   [还原 master 数据库 (Transact-SQL)](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md)  

## <a name="restore-transaction-logs-full-recovery-model"></a>还原事务日志（完整恢复模式）
### <a name="using-ssms"></a>使用 SSMS  
-   [将数据库还原到标记的事务 (SQL Server Management Studio)](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
-   [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
  
 ### <a name="using-t-sql"></a>使用 T-SQL 
-   [将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
 
-   [重新启动中断的还原操作 (Transact-SQL)](../../relational-databases/backup-restore/restart-an-interrupted-restore-operation-transact-sql.md)  
  
-   [恢复数据库但不还原数据 (Transact-SQL)](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md)  
 
## <a name="more-information-and-resources"></a>更多信息和资源
 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [备份和还原 Analysis Services 数据库](/analysis-services/multidimensional-models/backup-and-restore-of-analysis-services-databases)   
 [备份和还原全文目录和索引](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)   
 [备份和还原复制的数据库](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)  
  
