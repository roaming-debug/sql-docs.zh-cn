---
title: 备份历史和标头 | Microsoft Docs
description: msdb 数据库存储服务器实例的 SQL Server 备份和还原操作历史记录。 了解可访问备份历史记录的 Transact-SQL 语句。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backup headers [SQL Server]
- history tables [SQL Server]
- file restores [SQL Server], status information
- backup sets [SQL Server], status information
- listing backed up databases
- status information [SQL Server], backups
- backing up [SQL Server], viewing backup sets
- restoring [SQL Server], history tables
- restoring databases [SQL Server], status information
- backups [SQL Server], status information
- headers [SQL Server]
- media headers [SQL Server]
- backup history tables [SQL Server]
- viewing backup information
- restoring files [SQL Server], viewing backup information
- restoring databases [SQL Server], history tables
- displaying backup information
- restoring files [SQL Server], status information
- historical information [SQL Server], backups
- database restores [SQL Server], history tables
- restore history tables [SQL Server]
- listing backed up files
ms.assetid: 799b9934-0ec2-4f43-960b-5c9653f18374
author: cawrites
ms.author: chadam
ms.openlocfilehash: ec866bae59d0b37803e7df72e9ba9593e0f66c7b
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130492"
---
# <a name="backup-history-and-header-information-sql-server"></a>备份历史记录和标头信息 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  服务器实例上所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份和还原操作的完整历史记录存储在 **msdb** 数据库中。 本主题介绍备份和还原历史记录表以及用于访问备份历史记录的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 本主题还讨论何时列出数据库和事务日志文件有用，以及何时使用介质标头信息与何时使用备份标头信息的比较。  
  
> [!IMPORTANT]  
>  为了规避丢失对备份和还原历史记录的最新更改的风险，请经常备份 **msdb** 。 有关必须备份哪些系统数据库的信息，请参阅[备份和还原系统数据库 (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)。  
  
 **本主题内容：**  
  
-   [备份和还原历史记录表](#BnRHistoryTables)  
  
-   [用于访问备份历史记录的 Transact-SQL 语句](#TsqlStatementsForBackupHistory)  
  
-   [数据库和事务日志文件](#ListDbTlogFiles)  
  
-   [介质标头信息](#MediaHeader)  
  
-   [备份标头信息](#BackupHeader)  
  
-   [介质标头信息和备份标头信息的比较](#CompareMediaHeaderBackupHeader)  
  
-   [验证备份](#Verification)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="backup-and-restore-history-tables"></a><a name="BnRHistoryTables"></a> 备份和还原历史记录表  
 本部分介绍 **msdb** 系统数据库中存储备份和还原元数据的历史记录表。  
  
|历史记录表|说明|  
|-------------------|-----------------|  
|[backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)|每个备份的数据或日志文件在表中占一行。|  
|[backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)|备份集中的每个文件组在表中占一行。|  
|[backupmediafamily](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)|每个介质簇在表中占一行。 如果介质簇驻留在镜像介质集中，则对于介质集中的每个镜像，该介质簇都具有一个单独的行。|  
|[backupmediaset](../../relational-databases/system-tables/backupmediaset-transact-sql.md)|每个备份介质集在表中占一行。|  
|[backupset](../../relational-databases/system-tables/backupset-transact-sql.md)|每个备份集在表中占一行。|  
|[restorefile](../../relational-databases/system-tables/restorefile-transact-sql.md)|每个已还原的文件在表中占一行。 这包括按文件组名称间接还原的文件。|  
|[restorefilegroup](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)|每个已还原的文件组在表中占一行。|  
|[restorehistory](../../relational-databases/system-tables/restorehistory-transact-sql.md)|每个还原操作在表中占一行。|  
  
> [!NOTE]  
>  执行还原时，会修改备份历史记录表和还原历史记录表。  
  
##  <a name="transact-sql-statements-for-accessing-backup-history"></a><a name="TsqlStatementsForBackupHistory"></a> 用于访问备份历史记录的 Transact-SQL 语句  
 还原信息语句与特定备份历史记录表中存储的信息存在对应关系。  
  
> [!IMPORTANT]  
>  RESTORE FILELISTONLY、RESTORE HEADERONLY、RESTORE LABELONLY 和 RESTORE VERIFYONLY Transact-SQL 语句需要 CREATE DATABASE 权限。 与以前的版本相比，这项新要求为您的备份文件提高了安全性，并更周全地保护了您的备份信息。 有关此权限的信息，请参阅 [GRANT 数据库权限 (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md)。  
  
|信息语句|备份历史记录表|说明|  
|---------------------------|--------------------------|-----------------|  
|[RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)|[backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)|返回一个结果集，其中包含一个列出了指定备份集中包含的数据库和日志文件的列表。<br /><br /> 有关详细信息，请参阅本主题后面的“列出数据库文件和事务日志文件”部分。|  
|[RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)|[backupset](../../relational-databases/system-tables/backupset-transact-sql.md)|在特定的备份设备上检索所有备份集的所有备份标头信息。 执行 RESTORE HEADERONLY 的结果是一个结果集。<br /><br /> 有关详细信息，请参阅本主题后面的“查看备份标头信息”部分。|  
|[RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)|[backupmediaset](../../relational-databases/system-tables/backupmediaset-transact-sql.md)|返回一个结果集，其中包含有关指定备份设备中的备份介质的信息。<br /><br /> 有关详细信息，请参阅本主题后面的“查看介质标头信息”部分。|  
  
##  <a name="database-and-transaction-log-files"></a><a name="ListDbTlogFiles"></a> 数据库和事务日志文件  
 列出备份中的数据库文件和事务日志文件时，显示的信息包括逻辑名称、物理名称、文件类型（数据库文件或日志文件）、文件组成员资格、文件大小（字节）、允许的文件最大大小和预定义的文件增长大小（字节）。 在下列情况下，这些信息对于在还原数据库备份之前确定数据库备份中文件的名称很有用：  
  
-   丢失了包含数据库中一个或多个文件的磁盘驱动器。  
  
     可以列出数据库备份中的文件以确定哪些文件受到影响，然后在还原整个数据库时将那些文件还原到另一个驱动器上，或者只还原那些文件并应用自进行数据库备份后创建的任何事务日志备份。  
  
-   将数据库从一台服务器还原到另一台服务器上，但这台服务器上没有目录结构和驱动器映射。  
  
     列出备份中的文件使您能够确定哪些文件受到影响。 例如，备份中包含必须还原到驱动器 E 的文件，而目标服务器没有驱动器 E。还原此文件时，必须将此文件重新定位至其他位置，例如驱动器 Z。  
  
##  <a name="media-header-information"></a><a name="MediaHeader"></a> 介质标头信息  
 查看介质标头时显示有关介质本身的信息，而不显示有关介质上的备份的信息。 显示的介质标头信息包括介质名称、说明、创建介质标头的软件的名称以及介质标头的写入日期。  
  
> [!NOTE]  
>  查看介质标头的速度很快。  
  
 有关详细信息，请参阅本主题后面的 [介质标头信息和备份标头信息的比较](#CompareMediaHeaderBackupHeader)。  
  
##  <a name="backup-header-information"></a><a name="BackupHeader"></a> 备份标头信息  
 查看备份标头时，将显示有关介质上的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份集的信息。 显示的信息包括使用的备份设备类型、备份类型（数据库备份、事务备份、文件备份还是差异数据库备份）以及备份开始和停止的日期/时间。 这些信息对确定还原磁带上的哪个备份集或确定介质上包含哪些备份很有用。  
  
> [!NOTE]  
>  查看备份标头信息时需要扫描整个介质以显示有关介质上每个备份的信息，因此对高容量磁带可能需要很长时间。  
  
 有关详细信息，请参阅本主题后面的 [介质标头信息和备份标头信息的比较](#CompareMediaHeaderBackupHeader)。  
  
### <a name="which-backup-set-to-restore"></a>要还原的备份集  
 可以使用备份标头中的信息来标识要还原的备份集。 数据库引擎将对备份介质上的每个备份集进行编号。 这样，您就可以通过备份集在介质中的位置标识要还原的备份集。 例如，下面的介质包含三个备份集。  
  
 ![包含 SQL Server 备份集的备份媒体](../../relational-databases/backup-restore/media/bnr-media-backup-sets.gif "包含 SQL Server 备份集的备份媒体")  
  
 若要还原特定的备份集，请指定要还原的备份集的位置编号。 例如，若要还原第二个备份集，请指定 2 作为要还原的备份集。  
  
##  <a name="comparison-of-media-header-and-backup-header-information"></a><a name="CompareMediaHeaderBackupHeader"></a> 介质标头信息和备份标头信息的比较  
 可以下图为例了解备份标头和介质标头信息在查看方法上的区别。 获取介质标头信息只需要从磁带开头检索信息。 获取备份标头信息则需要扫描整个磁带以查看每个备份集的标头。  
  
 ![包含三个 SQL Server 备份集的媒体集](../../relational-databases/backup-restore/media/bnr-media-label.gif "包含三个 SQL Server 备份集的媒体集")  
  
> [!NOTE]  
>  使用包含多个介质簇的介质集时，介质标头和备份集都写入所有介质簇中。 因此，只需为这些报表操作提供单个介质簇即可。  
  
 有关如何查看介质标头的信息，请参阅本主题前面的“查看介质标头信息”部分。  
  
 有关如何查看备份设备上所有备份集的备份标头信息的信息，请参阅本主题前面的“查看备份标头信息”。  
  
##  <a name="backup-verification"></a><a name="Verification"></a> 验证备份  
 尽管验证备份不是必需的，但却很有用。 验证备份可以检查备份在物理上是否完好无损，以确保备份中的所有文件都是可读、可还原的，并且在您需要使用它时可以还原备份。 了解验证备份时不会验证备份中数据的结构是非常重要的。 但是，如果备份是使用 WITH CHECKSUMS 创建的，则使用 WITH CHECKSUMS 验证备份可以很好地表明备份中数据的可靠性。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **从备份中删除旧行并还原历史记录表**  
  
-   [sp_delete_backuphistory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md)  
  
 **从备份中删除特定数据库的所有行并还原历史记录表**  
  
-   [sp_delete_database_backuphistory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-database-backuphistory-transact-sql.md)  
  
 **查看备份集中的数据文件和日志文件**  
  
-   [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReadFileList%2A> (SMO)  
  
 **查看介质标头信息**  
  
-   [RESTORE LABELONLY (Transact-SQL)](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
-   [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   [查看备份磁带或文件的内容 (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReadMediaHeader%2A> (SMO)  
  
 **查看备份标头信息**  
  
-   [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [查看备份磁带或文件的内容 (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReadBackupHeader%2A> (SMO)  
  
 **从备份中删除旧行并还原历史记录表**  
  
-   [sp_delete_backuphistory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md)  
  
 **从备份中删除特定数据库的所有行并还原历史记录表**  
  
-   [sp_delete_database_backuphistory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-database-backuphistory-transact-sql.md)  
  
 **查看介质标头信息**  
  
-   [RESTORE LABELONLY (Transact-SQL)](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
-   [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   [查看备份磁带或文件的内容 (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReadMediaHeader%2A> (SMO)  
  
 **查看备份标头信息**  
  
-   [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [查看备份磁带或文件的内容 (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReadBackupHeader%2A> (SMO)  
  
 **查看备份集中的文件**  
  
-   [查看备份集中的数据文件和日志文件 (SQL Server)](../../relational-databases/backup-restore/view-the-data-and-log-files-in-a-backup-set-sql-server.md)  
  
-   [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
 **验证备份**  
  
-   [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.SqlVerify%2A> (SMO)  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [镜像备份媒体集 (SQL Server)](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md)   
 [备份和还原期间可能出现的媒体错误 (SQL Server)](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)  
  
  
