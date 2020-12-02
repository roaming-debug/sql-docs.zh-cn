---
title: 媒体错误：备份和还原 | Microsoft Docs
description: 在 SQL Server 2019 中，可以在恢复数据库时不必顾及检测到的错误。 将 RESTORE 和 RESTORE VERIFYONLY 与备份校验和一起使用可检查错误。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- media errors [SQL Server]
- CONTINUE_AFTER_ERROR option
- errors [SQL Server], backups
- backups [SQL Server], errors
- RESTORE VERIFYONLY statement
- backup media [SQL Server], error management
- page checksums [SQL Server]
- backup checksums [SQL Server]
- backing up [SQL Server], media errors
- RESTORE statement, media errors
- NO_CHECKSUM option
- checksums [SQL Server]
ms.assetid: 83a27b29-1191-4f8d-9648-6e6be73a9b7c
author: cawrites
ms.author: chadam
ms.openlocfilehash: f0405ddad68df7c4e34d0af17d2e9da81adfe535
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130329"
---
# <a name="possible-media-errors-during-backup-and-restore-sql-server"></a>在备份和还原期间可能的介质错误 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 允许您在恢复数据库时不必顾及检测到的错误。 一个重要的新错误检测机制是创建备份校验和（可选），可以通过备份操作创建并通过还原操作验证。 您可以控制操作是否检查错误，以及遇到错误时是停止操作还是继续操作。 如果备份包含备份校验和，则 RESTORE 和 RESTORE VERIFYONLY 语句可以检查错误。  
  
> [!NOTE]  
>  镜像备份最多提供 4 个介质集的副本（镜像），提供备用副本以便从损坏介质导致的错误中恢复。 有关详细信息，请参阅本主题后面的 [镜像备份媒体集 (SQL Server)](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md)不熟悉的读者。  
  
  
##  <a name="backup-checksums"></a><a name="BckChecksums"></a> 备份校验和  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持三种校验和：页校验和、日志块校验和以及备份校验和。 生成备份校验和时，BACKUP 将验证从数据库读取的数据是否与数据库中存在的任意校验和或页残缺指示一致。  
  
 BACKUP 语句选择性地计算备份流的备份校验和；如果给定页上存在页校验和或残缺页信息，则当备份该页时，BACKUP 还将验证它的校验和、残缺页状态以及 ID。 创建备份校验和时，备份操作不会向页中添加任何校验和。 将在这些页位于数据库中时对其进行备份，备份不会修改这些页。  
  
 由于验证和生成备份校验和引起的开销，使用备份校验和会对性能造成潜在的影响。 工作负荷和备份吞吐量都可能受到影响。 因此，不是必须使用备份校验和。 如果决定在备份过程中生成校验和，请仔细监视由此引起的 CPU 开销以及对系统中任何并发工作负荷造成的影响。  
  
 BACKUP 永远不会修改磁盘上的源页面和页面内容。  
  
 在启用备份校验和后，备份操作将执行以下步骤：  
  
1.  向备份介质写入页之前，备份操作将验证页级信息（页校验和或页残缺检测）是否存在。 如果两者都不存在，则备份无法验证页。 将按原样包含未经验证的页，并且其内容将添加到总备份校验和中。  
  
     如果备份操作在验证过程中遇到页错误，备份将失败。  
  
    > [!NOTE]  
    >  有关页校验和及页残缺检测的详细信息，请参阅 ALTER DATABASE 语句的 PAGE_VERIFY 选项。 有关详细信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。  
  
2.  无论是否存在页校验和，BACKUP 都会为备份流生成一个单独的备份校验和。 还原操作可使用（可选）备份校验和来验证该备份是否损坏。 备份校验和存储在备份介质上，而不是存储在数据库页上。 备份校验还可根据需要在还原时使用。  
  
3.  备份集标记为包含备份校验和（在 **msdb..backupset** 的 **has_backup_checksums** 列中）。 有关详细信息，请参阅 [backupset (Transact-SQL)](../../relational-databases/system-tables/backupset-transact-sql.md)。  

 在还原操作过程中，如果备份介质中存在备份校验和，则默认情况下，RESTORE 和 RESTORE VERIFYONLY 语句都将验证备份校验和及页校验和。 如果不存在备份校验和，则这两种还原操作直接执行，而不会进行验证；这是因为没有备份校验和，还原无法可靠地验证页校验和。  
  
## <a name="response-to-page-checksum-errors-during-a-backup-or-restore-operation"></a>在备份或还原操作过程中响应页校验和错误  
 默认情况下，在遇到页校验和错误后，BACKUP 或 RESTORE 操作将失败，而 RESTORE VERIFYONLY 操作将继续。 但是，您可以控制某一给定操作在遇到错误时是失败还是尽可能继续。  
  
 如果 BACKUP 操作在遇到错误后将继续，则该操作将执行以下步骤：  
  
1.  将备份介质中的备份集标记为包含错误，并跟踪 **msdb** 数据库中 **suspect_pages** 表中的页。 有关详细信息，请参阅 [suspect_pages (Transact-SQL)](../../relational-databases/system-tables/suspect-pages-transact-sql.md)。  
  
2.  记录 SQL Server 错误日志中的错误。  
  
3.  将备份集标记为包含此类型的错误（在 **msdb.backupset** 的 **is_damaged** 列中）。 有关详细信息，请参阅 [backupset (Transact-SQL)](../../relational-databases/system-tables/backupset-transact-sql.md)。  
  
4.  发出一条消息，说明已成功生成备份，但备份中包含页错误。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **启用或禁用备份校验和**  
  
-   [在备份或还原期间启用或禁用备份校验和 (SQL Server)](../../relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server.md)  
  
 **控制备份操作期间的错误响应**  
  
-   [指定备份或还原操作在遇到错误后是继续还是停止 (SQL Server)](../../relational-databases/backup-restore/specify-if-backup-or-restore-continues-or-stops-after-error.md)  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [backupset (Transact-SQL)](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [镜像备份媒体集 (SQL Server)](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
  
