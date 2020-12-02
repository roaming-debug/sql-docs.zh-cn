---
title: 备份概述 (SQL Server) | Microsoft Docs
description: 了解 SQL Server 备份组件，包括备份类型和限制以及备份设备和备份媒体。
ms.custom: ''
ms.date: 07/15/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- tables [SQL Server], backing up data
- backups [SQL Server]
- database backups [SQL Server]
- backup types [SQL Server]
- data backups [SQL Server]
- backing up tables [SQL Server]
- database restores [SQL Server], backups
- backing up [SQL Server], about backing up
- restoring [SQL Server], backup types
- backups [SQL Server], about
- backups [SQL Server], table-level backups unsupported
ms.assetid: 09a6e0c2-d8fd-453f-9aac-4ff24a97dc1f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 08a9360ae2f9fa7f1ff109b4d9b6075dd5536481
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129318"
---
# <a name="backup-overview-sql-server"></a>Backup Overview (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份组件。 备份 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库对于保护您的数据至关重要。 本讨论涵盖了备份类型和备份限制。 该主题还介绍了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份设备和备份介质。  
  
  
## <a name="terms"></a>术语
 
 **备份 [动词] \(back up)**  
 从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库或其事务日志中将数据或日志记录复制到备份设备（如磁盘），以创建数据备份或日志备份。  
  
**备份 [名词] \(backup)**  
 可用于在失败后还原或恢复数据的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据副本。 在数据库级别以及针对数据库的一个或多个文件或文件组创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据的备份。 不能创建表级备份。 除了数据备份之外，完整恢复模式要求创建事务日志的备份。  
  
**[恢复模式](../../relational-databases/backup-restore/recovery-models-sql-server.md)**  
 用于控制数据库上的事务日志维护的数据库属性。 有三种恢复模式：简单恢复模式、完整恢复模式和大容量日志恢复模式。 数据库的恢复模式确定其备份和还原要求。  
  
 **[还原 (restore)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)**  
 一种包括多个阶段的过程，用于将指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份中的所有数据和日志页复制到指定数据库，然后通过应用记录的更改使该数据在时间上向前移动，以前滚备份中记录的所有事务。  
  
 ## <a name="types-of-backups"></a>备份类型  
  
 **[仅复制备份 (copy-only backup)](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)**  
 独立于正常 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份序列的特殊用途备份。  
  
**数据备份 (data backup)**    
 完整数据库的数据备份（数据库备份）、部分数据库的数据备份（部分备份）或一组数据文件或文件组的数据备份（文件备份）。  
  
**[数据库备份 (database backup)](../../relational-databases/backup-restore/full-database-backups-sql-server.md)**  
 数据库的备份。 完整数据库备份表示备份完成时的整个数据库。 差异数据库备份只包含自最近完整备份以来对数据库所做的更改。  
  
 **[差异备份 (differential backup)](../../relational-databases/backup-restore/full-database-backups-sql-server.md)**  
 基于完整数据库或部分数据库以及一组数据文件或文件组的最新完整备份的数据备份（差异基准），仅包含自差异基准以来发生了更改的数据区。  
  
 部分差异备份仅记录自上一次部分备份（称为“差异基准”）以来文件组中发生更改的数据区。  
  
**完整备份 (full backup)**  
 一种数据备份，包含特定数据库或者一组特定的文件组或文件中的所有数据，以及可以恢复这些数据的足够的日志。  
  
**[日志备份 (log backup)](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)**  
 包括以前日志备份中未备份的所有日志记录的事务日志备份。 （完整恢复模式）  
  
 **[文件备份 (file backup)](../../relational-databases/backup-restore/full-file-backups-sql-server.md)**  
 一个或多个数据库文件或文件组的备份。  
  
 **[部分备份 (partial backup)](../../relational-databases/backup-restore/partial-backups-sql-server.md)**  
 仅包含数据库中部分文件组的数据（包含主要文件组、每个读/写文件组以及任何可选指定的只读文件中的数据）。  
  
## <a name="backup-media-terms-and-definitions"></a>备份介质术语和定义  
  
 **[备份设备 (backup device)](../../relational-databases/backup-restore/backup-devices-sql-server.md)**  
 要将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份写入其中以及可从其中还原的磁盘或磁带设备。 SQL Server 备份也可以写入 Azure Blob 存储服务，并且使用 URL 格式来指定备份文件的目标和名称。 有关详细信息，请参阅[使用 Microsoft Azure Blob 存储服务执行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)。  
  
 **[备份介质](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)**  
 已写入一个或多个备份的一个或多个磁带或磁盘文件。  
  
 **[备份集 (backup set)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)**  
 通过成功的备份操作添加到介质组的备份内容。  
  
 **[介质簇 (media family)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)**  
 在介质集中的单个非镜像设备或一组镜像设备上创建的备份。  
  
 **[介质集 (media set)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)**  
 备份介质（磁带或磁盘文件）的有序集合，使用固定类型和数量的备份设备向其写入了一个或多个备份操作。  
  
 **[镜像介质集 (mirrored media set)](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md)**  
 介质集的多个副本（镜像）。  
  
##  <a name="backup-compression"></a><a name="BackupCompression"></a> 备份压缩  
 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] 及更高版本支持压缩备份，并且 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本可以还原压缩后的备份。 有关详细信息，请参阅[备份压缩 (SQL Server)](../../relational-databases/backup-restore/backup-compression-sql-server.md)。  
  
##  <a name="backup-operations-restrictions"></a><a name="Restrictions"></a>  备份操作限制 
 可以在数据库在线并且正在使用时进行备份。 但是，存在下列限制：  
  
### <a name="cannot-back-up-offline-data"></a>无法备份脱机数据  
 隐式或显式引用脱机数据的任何备份操作都会失败。 一些典型示例包括：  
  
-   您请求完整数据库备份，但是数据库的一个文件组脱机。 由于所有文件组都隐式包含在完整数据库备份中，因此，此操作将会失败。  
  
     若要备份此数据库，可以使用文件备份并仅指定联机的文件组。  
  
-   请求部分备份，但是有一个读/写文件组处于脱机状态。 由于部分备份需要使用所有读/写文件组，因此该操作失败。  
  
-   请求特定文件的文件备份，但是其中有一个文件处于脱机状态。 该操作失败。 若要备份联机文件，可以省略文件列表中的脱机文件并重复该操作。  
  
 通常，即使一个或多个数据文件不可用，日志备份也会成功。 但如果某个文件包含大容量日志恢复模式下所做的大容量日志更改，则所有文件都必须都处于联机状态才能成功备份。  
  
### <a name="concurrency-restrictions"></a>并发限制   
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以使用联机备份过程来备份数据库。 在备份过程中，可以进行多个操作；例如：在执行备份操作期间允许使用 INSERT、UPDATE 或 DELETE 语句。 但是，如果在正在创建或删除数据库文件时尝试启动备份操作，则备份操作将等待，直到创建或删除操作完成或者备份超时。  
  
 在数据库备份或事务日志备份的过程中无法执行的操作包括：  
  
-   文件管理操作，如含有 ADD FILE 或 REMOVE FILE 选项的 ALTER DATABASE 语句。  
  
-   收缩数据库或文件操作。 这包括自动收缩操作。  
  
-   如果在进行备份操作时尝试创建或删除数据库文件，则创建或删除操作将失败。  
  
 如果备份操作与文件管理操作或收缩操作重叠，则产生冲突。 无论哪个冲突操作首先开始，第二个操作总会等待第一个操作设置的锁超时。（超时期限由会话超时设置控制。）如果在超时期限内释放锁，第二个操作将继续执行。 如果锁超时，则第二个操作失败。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
 **备份设备和备份媒体**  
  
-   [为磁盘文件定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)  
  
-   [为磁带驱动器定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
-   [将磁盘或磁带指定为备份目标 (SQL Server)](../../relational-databases/backup-restore/specify-a-disk-or-tape-as-a-backup-destination-sql-server.md)  
  
-   [删除备份设备 (SQL Server)](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md)  
  
-   [设置备份的过期日期 (SQL Server)](../../relational-databases/backup-restore/set-the-expiration-date-on-a-backup-sql-server.md)  
  
-   [查看备份磁带或文件的内容 (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [查看备份集中的数据文件和日志文件 (SQL Server)](../../relational-databases/backup-restore/view-the-data-and-log-files-in-a-backup-set-sql-server.md)  
  
-   [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   [从设备还原备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-backup-from-a-device-sql-server.md)  
  
-   [教程：将 SQL Server 备份和还原到 Azure Blob 存储服务](~/relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md)  
  
 **创建备份**  
  
> [!NOTE]  
>  对于部分备份或仅复制备份，必须分别使用带 PARTIAL 或 COPY_ONLY 选项的 [!INCLUDE[tsql](../../includes/tsql-md.md)] [BACKUP](../../t-sql/statements/backup-transact-sql.md) 语句。  
  
-   [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [备份文件和文件组 (SQL Server)](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)  
  
-   [创建差异数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
-   [在数据库损坏时备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md)  
  
-   [在备份或还原期间启用或禁用备份校验和 (SQL Server)](../../relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server.md)  
  
-   [指定备份或还原操作在遇到错误后是继续还是停止 (SQL Server)](../../relational-databases/backup-restore/specify-if-backup-or-restore-continues-or-stops-after-error.md)  
  
-   [使用资源调控器限制备份压缩的 CPU 使用量 (Transact-SQL)](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)  
  
-   [教程：将 SQL Server 备份和还原到 Azure Blob 存储服务](~/relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md)  
  
## <a name="and-more"></a>以及更多！ 
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [维护计划](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)  
  
  
