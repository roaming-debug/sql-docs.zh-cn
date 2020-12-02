---
title: 结尾日志备份 (SQL Server) | Microsoft Docs
description: 在 SQL Server 中，“结尾日志备份”捕获尚未备份的任何日志记录，以防数据丢失并确保日志链完好无损。
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backing up [SQL Server], tail of log
- transaction log backups [SQL Server], tail-log backups
- NO_TRUNCATE clause
- backups [SQL Server], log backups
- tail-log backups
- backups [SQL Server], tail-log backups
ms.assetid: 313ddaf6-ec54-4a81-a104-7ffa9533ca58
author: cawrites
ms.author: chadam
ms.openlocfilehash: 546029b8615745c64d62da49af5299893d4d7c15
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129025"
---
# <a name="tail-log-backups-sql-server"></a>结尾日志备份 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题仅与备份和还原使用完整恢复模式或大容量日志恢复模式的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库相关。  
  
 “结尾日志备份”  捕获尚未备份的任何日志记录（“结尾日志”  ），以防丢失所做的工作并确保日志链完好无损。 在将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库恢复到其最近一个时间点之前，必须先备份数据库的事务日志。 结尾日志备份将是数据库还原计划中相关的最后一个备份。  
  
> **注意：** 并非所有还原方案都要求执行结尾日志备份。 如果恢复点包含在较早的日志备份中，则无需结尾日志备份。 此外，如果您准备移动或替换（覆盖）数据库，并且在最新备份后不需要将该数据库还原到某一时间点，则不需要结尾日志备份。  
  
   ##  <a name="scenarios-that-require-a-tail-log-backup"></a><a name="TailLogScenarios"></a> 需要结尾日志备份的方案  
 建议您在以下方案中执行结尾日志备份：  
  
-   如果数据库处于联机状态并且您计划对数据库执行还原操作，则从备份日志结尾开始。 若要避免联机数据库错误，必须使用...[BACKUP](../../t-sql/statements/backup-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的 WITH NORECOVERY 选项。  
  
-   如果数据库处于脱机状态而无法启动，则需要还原数据库，从备份日志结尾开始。 由于此时不会发生任何事务，因此 WITH NORECOVERY 是可选的。  
  
-   如果数据库损坏，则尝试使用 BACKUP 语句的 WITH CONTINUE_AFTER_ERROR 选项执行结尾日志备份。  
  
     在损坏的数据库上，仅当日志文件未受损、数据库处于支持结尾日志备份的状态并且数据库不包含任何大容量日志更改时，日志结尾备份才会成功。 如果无法创建结尾日志备份，则最新日志备份后提交的任何事务都将丢失。  
  
 下表总结了 BACKUP NORECOVERY 和 CONTINUE_AFTER_ERROR 选项。  
  
|BACKUP LOG 选项|注释|  
|-----------------------|--------------|  
|NORECOVERY|每当您准备对数据库继续执行还原操作时，请使用 NORECOVERY。 NORECOVERY 使数据库进入还原状态。 这确保了数据库在结尾日志备份后不会更改。 除非同时指定 NO_TRUNCATE 或 COPY_ONLY 选项，否则将截断日志。<br /><br /> **重要提示：** 除非数据库受损，否则不建议使用 NO_TRUNCATE。 在使用 NORECOVERY 执行还原之前，可能需要将数据库置于[单用户模式](../../relational-databases/databases/set-a-database-to-single-user-mode.md)以获得独占访问权限。 还原后，请将数据库重新设置为多用户模式。 |  
|CONTINUE_AFTER_ERROR|仅当您要备份受损数据库的尾部时，才使用 CONTINUE_AFTER_ERROR。<br /><br /> 对受损数据库备份日志尾部时，日志备份中正常捕获的部分元数据可能不可用。 有关详细信息，请参阅本主题中的 [包含不完整备份元数据的结尾日志备份](#IncompleteMetadata)。|  
  
##  <a name="tail-log-backups-that-have-incomplete-backup-metadata"></a><a name="IncompleteMetadata"></a> 包含不完整备份元数据的结尾日志备份  
 结尾日志备份可捕获日志尾部，即使数据库脱机、损坏或缺少数据文件。 这可能导致还原信息命令和 **msdb** 生成不完整的元数据。 但只有元数据是不完整的，而捕获的日志是完整且可用的。  
  
 如果结尾日志备份包含不完整的元数据，则 [backupset](../../relational-databases/system-tables/backupset-transact-sql.md) 表中的 **has_incomplete_metadata** 将设置为 **1**。 此外，在 [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)的输出中， **HasIncompleteMetadata** 将设置为 **1**。  
  
 如果结尾日志备份中的元数据不完整，则 [backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md) 表在结尾日志备份时将丢失文件组的大多数相关信息。 大多数 **backupfilegroup** 表列为 NULL；只有以下几列有意义：  
  
-   **backup_set_id**  
-   **filegroup_id**  
-   type   
-   **type_desc**  
-   **is_readonly**  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 要创建结尾日志备份，请参阅[在数据库损坏时备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md)。  
  
 若要还原事务日志备份，请参阅[还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)。  
    
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [仅复制备份 (SQL Server)](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)   
 [事务日志备份 (SQL Server)](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)   
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)    
 [SQL Server 事务日志体系结构和管理指南](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)
  
