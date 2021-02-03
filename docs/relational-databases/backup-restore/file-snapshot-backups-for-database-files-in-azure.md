---
title: Azure 中的数据库文件的文件快照备份 | Microsoft Docs
description: SQL Server 文件快照备份使用 Azure 快照提供快速备份，并且更快速地还原使用 Azure Blob 存储服务存储的数据库文件。
ms.custom: ''
ms.date: 05/23/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 17a81fcd-8dbd-458d-a9c7-2b5209062f45
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1b484ea7b300fe53983dd26c5f7644abbebd2ca2
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236815"
---
# <a name="file-snapshot-backups-for-database-files-in-azure"></a>Azure 中数据库文件的文件快照备份
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 文件快照备份使用 Azure 快照提供近乎即时的备份，并且更快速地还原使用 Azure Blob 存储服务存储的数据库文件。 使用此功能可简化备份和还原策略。 有关实时演示，请参阅 [时间点还原的演示](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)。 有关使用 Azure Blob 存储服务存储数据库文件的详细信息，请参阅 [Microsoft Azure 中的 SQL Server 数据文件](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)。  
  
 ![快照备份体系结构图](../../relational-databases/backup-restore/media/snapshotbackups.PNG "快照备份体系结构图")  
  
 **下载**  
  
-   若要下载 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]，请转到  **[评估中心](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)** 。  
  
-   已经拥有 Azure 帐户？  然后转到 **[此处](https://azure.microsoft.com/services/virtual-machines/sql-server/)** 启动装有 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 的虚拟机。  
  
## <a name="using-azure-snapshots-to-back-up-database-files-stored-in-azure"></a>使用 Azure 快照备份 Azure 中存储的数据库文件  
  
### <a name="what-is-a-ssnoversion-file-snapshot-backup"></a>什么是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 文件快照备份  
 文件快照备份包括一组 blob 的 Azure 快照（包含数据库文件）以及备份文件（包含指向这些文件快照的指针）。 每个文件快照都会与基本 blob 一起存储在容器中。 你可以指定要将备份文件本身写入到 URL、磁盘还是磁带。 建议备份到 URL。 有关备份的详细信息，请参阅 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)，有关备份到 URL 的详细信息，请参阅 [SQL Server 备份到 URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md)。  
  
 ![快照功能的体系结构](../../relational-databases/backup-restore/media/snapshotbackups-flat.png "快照功能的体系结构")  
  
 删除基本 blob 会使备份集失效，系统将阻止你删除包含文件快照的 blob（除非你明确选择要删除 blob 及其所有文件快照）。 而且，删除数据库或数据文件不会删除基本 blob 或其任何文件快照。 另外，删除备份文件不会删除备份集中的任何文件快照。 若要删除文件快照备份集，请使用 **sys.sp_delete_backup** 系统存储过程。  
  
 **完整数据库备份：** 使用文件快照备份执行完整数据库备份将创建构成该数据库的每个数据和日志文件的 Azure 快照、建立事务日志备份链，并将文件快照的位置写入到备份文件。  
  
 **事务日志备份：** 使用文件快照备份执行事务日志备份将创建每个数据库文件（不仅仅是事务日志）的文件快照、将文件快照位置信息记录到备份文件，并截断事务日志文件。  
  
> [!IMPORTANT]  
>  在建立事务日志备份链所需的初始完整备份（可以是文件快照备份）后，你只需执行事务日志备份，因为每个事务日志文件快照备份集均包含所有数据库文件的文件快照，并可用于执行数据库还原或日志还原。 在初始完整数据库备份之后，不需要执行其他完整或差异备份，因为 Azure Blob 存储服务可处理每个文件快照和每个数据库文件的基本 blob 的当前状态之间的差异。  
  
> [!NOTE]  
>  有关将 Microsoft Azure Blob 存储服务和 SQL Server 2016 配合使用的教程，请参见[教程：将 Microsoft Azure Blob 存储服务用于 SQL Server 2016 数据库](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)  
  
### <a name="restore-using-file-snapshot-backups"></a>使用文件快照备份还原  
 由于每个文件快照备份集均包含每个数据库文件的文件快照，因此还原过程最多需要相邻的两个文件快照备份集。 无论备份集是来自完整数据库备份还是来自日志备份，都是如此。 在使用传统流备份文件执行还原过程时，这与还原过程大不相同。 使用传统流备份时，还原过程需要使用整个备份集链：完整备份、差异备份以及一个或多个事务日志备份。 无论还原是使用文件快照备份还是使用流备份集，还原过程的恢复部分将保持不变。  
  
 **还原到任何备份集的时间：** 要执行 RESTORE DATABASE 操作将数据库还原到特定文件快照备份集的时间，只需要特定备份集加上基本 blob 本身。 因为你可以使用事务日志文件快照备份集执行 RESTORE DATABASE 操作，你通常将使用事务日志备份集来执行此类型的 RESTORE DATABASE 操作，而很少使用完整数据库备份集。 本主题的末尾显示了演示此技术的示例。  
  
 **还原到两个文件快照备份集之间的时间点：** 要执行 RESTORE DATABASE 操作将数据库还原到两个相邻的事务日志备份集的时间之间的特定时间点，只需要两个事务日志备份集（在你想要将数据库还原到的时间点之前一个，在此时间点之后一个）。 若要实现此目的，应使用事务日志文件快照备份集从之前的时间点执行带 NORECOVERY 的 RESTORE DATABASE 操作，并使用事务日志文件快照备份集从之后的时间点执行带 RECOVERY 的 RESTORE LOG 操作，使用 STOPAT 参数指定要停止从事务日志备份恢复的时间点。 本主题的末尾显示了演示此技术的示例。 有关实时演示，请参阅 [时间点还原的演示](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)。  
  
### <a name="file-backup-set-maintenance"></a>文件备份集维护  
 **删除文件快照备份集：** 不能使用 FORMAT 参数覆盖文件快照备份集。 不允许使用 FORMAT 参数以避免保留使用原始文件快照备份创建的孤立文件快照。 若要删除文件快照备份集，请使用 **sys.sp_delete_backup** 系统存储过程。 此存储过程删除组成备份集的备份文件和文件快照。 使用另一种方法删除文件快照备份集可以删除备份文件，而不删除备份集中的文件快照。  
  
 **删除孤立的备份文件快照：** 在以下情况下，你可能有孤立的文件快照：未使用“sys.sp_delete_backup”系统存储过程删除备份文件，或者在包含数据库或数据库文件的 blob 有关联的备份文件快照时，删除了该数据库或数据库文件  。 若要确定可能会孤立的文件快照，请使用 **sys.fn_db_backup_file_snapshots** 系统函数列出数据库文件的所有文件快照。 若要确定属于特定文件快照备份集的文件快照，请使用 RESTORE FILELISTONLY 系统存储过程。 然后，可以使用 **sys.sp_delete_backup_file_snapshot** 系统存储过程删除已孤立的单个备份文件快照。 可在本主题的末尾找到使用此系统函数和这些系统存储过程的示例。 有关详细信息，请参阅 [sp_delete_backup (Transact-SQL)](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)、[sys.fn_db_backup_file_snapshots (Transact-SQL)](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md)、[sp_delete_backup_file_snapshot (Transact-SQL)](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) 和 [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)。  
  
### <a name="considerations-and-limitations"></a>注意事项和限制  
 **高级存储：** 使用高级存储时，适用以下限制：  
  
-   不能使用高级存储存储备份文件本身。  
  
-   备份的频率不能短于 10 分钟。  
  
-   你可以存储的快照的最大数目为 100。  
  
-   RESTORE WITH MOVE 是必需的。  
  
-   有关高级存储的其他信息，请参阅[高级存储：Azure 虚拟机工作负载的高性能存储](/azure/virtual-machines/disks-types)  
  
 **单个存储帐户：** 文件快照和目标 blob 必须使用相同的存储帐户。  
  
 **大容量恢复模式：** 使用大容量日志恢复模式并处理事务日志备份（包含以最低限度记录的事务）时，不能使用该事务日志备份执行日志还原（包括时间点恢复）。 而是应执行到文件快照备份集的时间的数据库还原。 此限制与流备份的限制相同。  
  
 **联机还原：** 使用文件快照备份时，不能执行联机还原。 有关联机还原的详细信息，请参阅[联机还原 (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md)。  
  
 **计费：** 使用 SQL Server 文件快照备份时，如果数据发生更改，将会产生额外的费用。 有关详细信息，请参阅 [了解快照如何产生费用](/rest/api/storageservices/Understanding-How-Snapshots-Accrue-Charges)。  
  
 **存档：** 如果你想要将文件快照备份存档，则可以存档到 blob 存储或流备份。 若要存档到 blob 存储，请将文件快照备份集中的 blob 复制到单独的 blob。 若要存档到流备份，请将文件快照备份还原为新数据库，然后使用压缩和/或加密执行标准流备份，并根据需要将其独立于基本 blob 进行存档。  
  
> [!IMPORTANT]  
>  维护多个文件快照备份只有较小的性能开销。 但是，维护过多文件快照备份会影响数据库的 I/O 性能。 我们建议你仅维护支持恢复点目标所需的这些文件快照备份。  
  
## <a name="backing-up-the-database-and-log-using-a-file-snapshot-backup"></a>使用文件快照备份备份数据库和日志  
 下面的示例使用文件快照备份将 AdventureWorks2016 示例数据库备份到 URL。  
  
```  
-- To permit log backups, before the full database backup, modify the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE AdventureWorks2016  
   SET RECOVERY FULL;  
GO  
-- Back up the full AdventureWorks2016 database.  
BACKUP DATABASE AdventureWorks2016   
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak'   
WITH FILE_SNAPSHOT;  
GO  
-- Back up the AdventureWorks2016 log using a time stamp in the backup file name.  
DECLARE @Log_Filename AS VARCHAR (300);  
SET @Log_Filename = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_Log_'+   
REPLACE (REPLACE (REPLACE (CONVERT (VARCHAR (40), GETDATE (), 120), '-','_'),':', '_'),' ', '_') + '.trn';  
BACKUP LOG AdventureWorks2016  
 TO URL = @Log_Filename WITH FILE_SNAPSHOT;  
GO  
```  
  
## <a name="restoring-from-a-ssnoversion-file-snapshot-backup"></a>从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 文件快照备份还原  
 下面的示例使用事务日志文件快照备份集还原 AdventureWorks2016 数据库，并显示恢复操作。 请注意，你可以从单个事务日志文件快照备份集还原数据库。  
  
```  
RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.trn'   
WITH RECOVERY, REPLACE;  
GO  
```  
  
## <a name="restoring-from-a-ssnoversion-file-snapshot-backup-to-a-point-in-time"></a>从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 文件快照备份还原到某个时间点  
 下面的示例使用两个事务日志文件快照备份集将 AdventureWorks2016 还原到其指定时间点的状态，并显示恢复操作。  
  
```  
RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.trn'   
WITH NORECOVERY,REPLACE;  
GO   
  
RESTORE LOG AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_18_00_00.trn'   
WITH RECOVERY,STOPAT = 'May 18, 2015 5:35 PM';  
GO  
```  
  
## <a name="deleting-a-database-file-snapshot-backup-set"></a>删除数据库文件快照备份集  
 若要删除文件快照备份集，请使用 **sys.sp_delete_backup** 系统存储过程。 指定数据库名称，以便让系统验证指定的文件快照备份集是否确实为指定数据库的备份。 如果未指定数据库名称，则将删除指定的备份集及其文件快照而不进行此类验证。 有关详细信息，请参阅 [sp_delete_backup (Transact-SQL)](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)。  
  
> [!WARNING]  
>  如果尝试使用其他方法（如 Microsoft Azure 管理门户或 SQL Server Management Studio 中的 Azure 存储查看器）删除文件快照备份集，则不会删除备份集中的文件快照。 这些工具将仅删除备份文件本身（其中包含指向文件快照备份集中的文件快照的指针）。 若要找到不正确地删除备份文件后保留的备份文件快照，请使用 **sys.fn_db_backup_file_snapshots** 系统函数，然后使用 **sys.sp_delete_backup_file_snapshot** 系统存储过程来删除单个备份文件快照。  
  
 以下示例删除指定的文件快照备份集，包括构成指定的备份集的备份文件和文件快照。  
  
```  
sys.sp_delete_backup 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak', 'adventureworks2016' ;  
GO  
  
```  
  
## <a name="viewing-database-backup-file-snapshots"></a>查看数据库备份文件快照  
 若要查看每个数据库文件的基本 blob 的文件快照，请使用 **sys.fn_db_backup_file_snapshots** 系统函数。 使用此系统函数可以查看使用 Azure Blob 存储服务存储的数据库的每个基本 blob 的所有备份文件快照。 此函数的主要用例是列出使用 **sys.sp_delete_backup** 系统存储过程以外的机制删除了文件快照备份集的备份文件时保留的数据库备份文件快照。 若要确定属于完整备份集的备份文件快照，和不属于完整备份集的备份文件快照，请使用 **RESTORE FILELISTONLY**  系统存储过程列出属于每个备份文件的文件快照。 有关详细信息，请参阅 [sys.fn_db_backup_file_snapshots (Transact-SQL)](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md) 和 [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)。  
  
 下面的示例返回指定数据库的所有备份文件快照的列表。  
  
```  
--Either specify the database name or set the database context  
USE AdventureWorks2016  
select * from sys.fn_db_backup_file_snapshots (null) ;  
GO  
select * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016') ;  
GO  
  
```  
  
## <a name="deleting-an-individual-database-backup-file-snapshot"></a>删除单个数据库备份文件快照  
 若要删除数据库基本 blob 的单个备份文件快照，请使用 **sys.sp_delete_backup_file_snapshot** 系统存储过程。 此系统存储过程的主要用例是删除在使用 **sys.sp_delete_backup** 系统存储过程以外的方法删除备份文件后保留的孤立的文件快照文件。 有关详细信息，请参阅 [sp_delete_backup_file_snapshot (Transact-SQL)](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md)。  
  
> [!WARNING]  
>  删除属于文件快照备份集的单个文件快照将使备份集失效。  
  
 下面的示例将删除指定的备份文件快照。 使用 **sys.fn_db_backup_file_snapshots** 系统函数获取指定备份的 URL。  
  
```  
sys.sp_delete_backup_file_snapshot N'adventureworks2016', N'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016Data.mdf?snapshot=2015-05-29T21:31:31.6502195Z';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [教程：将 Microsoft Azure Blob 存储服务用于 SQL Server 2016 数据库](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)  
  
