---
title: 文件还原（完整恢复模式）| Microsoft Docs
description: SQL Server 中的文件还原是指在不还原整个数据库的情况下，对一个或多个数据文件进行复制、前滚和还原的单一还原顺序。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- file restores [SQL Server]
- full recovery model [SQL Server], performing restores
- restoring files [SQL Server], Transact-SQL restore sequence
- restoring files [SQL Server]
- file restores [SQL Server], full recovery model
- restoring files [SQL Server], full recovery model
- Transact-SQL restore sequence
- file restores [SQL Server], Transact-SQL restore sequence
ms.assetid: d2236a2a-4cf1-4c3f-b542-f73f6096e15c
author: cawrites
ms.author: chadam
ms.openlocfilehash: fd5fa04b32d9c00f810b61bbca81963e21bd5697
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076875"
---
# <a name="file-restores-full-recovery-model"></a>文件还原（完整恢复模式）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题仅与在完整恢复模式或大容量加载恢复模式下包含多个文件或文件组的数据库相关。  
  
 文件还原的目标是还原一个或多个损坏的文件，而不是还原整个数据库。 文件还原方案由复制、前滚和恢复相应数据的单一还原顺序组成。  
  
 如果正在还原的文件组为读/写文件组，则在还原上一次数据备份或差异备份以后必须应用连续的日志备份链。 这样才会使文件组记录到日志文件的当前活动日志记录中。 恢复点通常靠近日志的末端，但并非总是如此。  
  
 如果正在还原的文件组为只读文件组，则通常不需要应用日志备份，并且会跳过该操作。 如果在文件变成只读后进行了备份，则该备份为要还原的最后备份。 前滚在目标点停止。  
  
 这些文件还原方案如下：  
  
-   脱机文件还原  
  
     在“脱机文件还原”  中，还原已损坏的文件或文件组时，数据库处于脱机状态。 还原顺序结束时，数据库将联机。  
  
     所有版本的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 都支持脱机文件还原。  
  
-   联机文件还原  
  
     在“联机文件还原”  中，如果数据库在还原时处于联机状态，则该数据库在文件还原过程中将保持联机状态。 不过，各文件组中如果有文件正在被还原，则该文件组在还原操作过程中将处于脱机状态。 恢复脱机文件组中的所有文件之后，该文件组将自动变为联机状态。  
  
     有关联机页和文件还原支持的信息，请参阅 [SQL Server 2016 版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)。 有关联机还原的详细信息，请参阅[联机还原 (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md)。
  
    > [!TIP]  
    >  如果希望数据库脱机以进行文件还原，请在开始还原顺序之前执行下列 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md) 语句以使数据库脱机：ALTER DATABASE database_name SET OFFLINE  。  
  
  
##  <a name="restoring-damaged-files-from-file-backups"></a><a name="Overview"></a> 从文件备份还原损坏的文件  
  
1.  在还原一个或多个损坏的文件前，请尝试创建 [结尾日志备份](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)。  
  
     如果该日志已损坏，则不能创建结尾日志备份，您必须还原整个数据库。  
  
     有关如何备份事务日志的信息，请参阅[事务日志备份 (SQL Server)](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)。  
  
    > [!IMPORTANT]  
    >  对于脱机文件还原，在文件还原之前必须始终先进行一次结尾日志备份。 对于联机文件还原，在文件还原之后必须始终先进行一次日志备份。 此日志备份对于将文件恢复到与数据库的其余部分一致的状态至关重要。  
  
2.  从每个损坏的文件的最新文件备份还原相应文件。  
  
3.  针对每个还原的文件，还原最近的差异文件备份（如果有）。  
  
4.  按顺序还原事务日志备份，从覆盖最早还原文件的备份开始，到在步骤 1 中创建的结尾日志备份结束。  
  
     必须还原在文件备份后创建的事务日志备份才能使数据库处于一致状态。 事务日志备份可以快速前滚，因为仅应用了对还原文件所做的更改。 与还原整个数据库相比，更好的方式是还原单个文件，因为并不会复制并随后前滚未损坏的文件。 但是，仍需要读取整个日志备份链。  
  
5.  恢复数据库。  

> [!NOTE]  
>  文件备份可用于将数据库还原到早期时间点。 必须还原一组完整的文件备份，然后依次将事务日志备份还原到目标点（目标点是结束最近还原的文件备份后的时间点），才能执行此操作。 有关时点恢复的详细信息，请参阅[将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)。  
  
## <a name="transact-sql-restore-sequence-for-an-offline-file-restore-full-recovery-model"></a>脱机文件还原的 Transact-SQL 还原顺序（完整恢复模式）  
 文件还原方案由复制、前滚和恢复相应数据的单一还原顺序组成。  
  
 本节说明用于文件还原序列的基本 [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) 选项。 将省略与此目的不相关的语法和详细信息。  
  
 以下示例说明了如何使用 WITH NORECOVERY 脱机还原两个辅助文件 `A` 和 `B`。 随后，对两个日志备份应用 NORECOVERY，然后是结尾日志备份（用 WITH RECOVERY 还原）。  
  
> [!NOTE]  
>  下面的示例还原序列通过使文件脱机开始，然后创建结尾日志备份，  
  
```  
--Take the file offline.  
ALTER DATABASE database_name MODIFY FILE SET OFFLINE;  
-- Back up the currently active transaction log.  
BACKUP LOG database_name  
   TO <tail_log_backup>  
   WITH NORECOVERY;  
GO   
-- Restore the files.  
RESTORE DATABASE database_name FILE=name   
   FROM <file_backup_of_file_A>   
   WITH NORECOVERY;  
RESTORE DATABASE database_name FILE=<name> ......  
   FROM <file_backup_of_file_B>   
   WITH NORECOVERY;  
-- Restore the log backups.  
RESTORE LOG database_name FROM <log_backup>   
   WITH NORECOVERY;  
RESTORE LOG database_name FROM <log_backup>   
   WITH NORECOVERY;  
RESTORE LOG database_name FROM <tail_log_backup>   
   WITH RECOVERY;  
```  
  
## <a name="examples"></a>示例  
  
-   [示例：读/写文件的联机还原（完整恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)  
  
-   [示例：只读文件的联机还原（完整恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
-   [示例：主文件组和一个其他文件组的脱机还原（完整恢复模式）](../../relational-databases/backup-restore/example-offline-restore-of-primary-and-one-other-filegroup-full-recovery-model.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **还原文件和文件组**  
  
-   [将文件还原到新位置 (SQL Server)](../../relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
  
-   [还原文件和文件组 (SQL Server)](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.SqlRestore%2A> (SMO)  
  
  
## <a name="see-also"></a>另请参阅  
 [备份和还原：互操作性和共存 &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-and-restore-interoperability-and-coexistence-sql-server.md)   
 [差异备份 (SQL Server)](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [完整文件备份 (SQL Server)](../../relational-databases/backup-restore/full-file-backups-sql-server.md)   
 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [完整数据库还原（简单恢复模式）](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)   
 [段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
