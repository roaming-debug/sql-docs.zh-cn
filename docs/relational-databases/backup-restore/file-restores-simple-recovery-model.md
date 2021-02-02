---
title: 文件还原（简单恢复模式）| Microsoft Docs
description: 在 SQL Server 中，文件还原适用于一个或多个损坏的文件，而不是还原整个数据库。
ms.custom: ''
ms.date: 03/24/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- file restores [SQL Server]
- simple recovery model [SQL Server]
- restoring files [SQL Server], Transact-SQL restore sequence
- restoring files [SQL Server]
- Transact-SQL restore sequence
- restoring files [SQL Server], simple recovery model
- file restores [SQL Server], simple recovery model
- file restores [SQL Server], Transact-SQL restore sequence
ms.assetid: b6d07386-7c6f-4cc6-be32-93289adbd3d6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5e2df8edb5b31ff2ffcd45eba9b5d9cf234c528d
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076885"
---
# <a name="file-restores-simple-recovery-model"></a>文件还原（简单恢复模式）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题仅适用于至少包含一个只读辅助文件组的简单模式数据库。  
  
 文件还原的目标是还原一个或多个损坏的文件，而不是还原整个数据库。 在简单恢复模式下，仅只读文件支持文件备份。 在还原数据库备份或部分备份时，将始终一同还原主文件组和读/写辅助文件组。  
  
 这些文件还原方案如下：  
  
-   脱机文件还原  
  
     在“脱机文件还原”  中，还原已损坏的文件或文件组时，数据库处于脱机状态。 还原顺序结束时，数据库将联机。  
  
     所有版本的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 都支持脱机文件还原。  
  
-   联机文件还原  
  
     在“联机文件还原”  中，如果数据库在还原时处于联机状态，则该数据库在文件还原过程中将保持联机状态。 不过，各文件组中如果有文件正在被还原，则该文件组在还原操作过程中将处于脱机状态。 恢复脱机文件组中的所有文件之后，该文件组将自动变为联机状态。  
  
     有关对联机页和文件还原的支持的信息，请参阅[数据库引擎功能和任务](../../sql-server/what-s-new-in-sql-server-ver15.md)。 有关联机还原的详细信息，请参阅[联机还原 (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md)。  
  
    > [!TIP]  
    >  如果希望数据库脱机以进行文件还原，请在开始还原顺序之前执行下列 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md) 语句以使数据库脱机：ALTER DATABASE database_name SET OFFLINE  。  
  
 **本主题内容：**  
  
-   [在简单恢复模式下还原文件和文件组的概述](#Overview)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="overview-of-file-and-filegroup-restore-under-the-simple-recovery-model"></a><a name="Overview"></a> 在简单恢复模式下还原文件和文件组的概述  
 文件还原方案由复制、前滚和恢复相应数据的单一还原顺序组成，如下所示：  
  
1.  从各个损坏文件的最新文件备份还原每个文件。  
  
2.  针对每个还原的文件，还原最新的差异文件备份并恢复数据库。  
  
### <a name="transact-sql-steps-for-file-restore-sequence-simple-recovery-model"></a>文件还原序列的 Transact-SQL 步骤（简单恢复模式）  
 本节说明用于简单文件还原序列的基本 [!INCLUDE[tsql](../../includes/tsql-md.md)][RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) 选项。 将省略与此目的不相关的语法和详细信息。  
  
 该还原序列仅包含两个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 第一个语句还原辅助文件（即文件 `A`），这是使用 WITH NORECOVERY 还原的。 第二项操作是还原其他两个文件（ `B` 和 `C` ），这两个文件是使用 WITH RECOVERY 从不同的备份设备还原的：  
  
1.  RESTORE DATABASE *database* FILE **=** _name_of_file_A_  
  
     FROM *file_backup_of_file_A*  
  
     WITH NORECOVERY **;**  
  
2.  RESTORE DATABASE *database* FILE **=** _name_of_file_B_ **,** _name_of_file_C_  
  
     FROM *file_backup_of_files_B_and_C*  
  
     WITH RECOVERY **;**  
  
### <a name="examples"></a>示例  
  
-   [示例：只读文件的联机还原（简单恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [示例：主文件组和一个其他文件组的脱机还原（完整恢复模式）](../../relational-databases/backup-restore/example-offline-restore-of-primary-and-one-other-filegroup-full-recovery-model.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **还原文件和文件组**  
  
-   [在现有文件上还原文件和文件组 (SQL Server)](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
-   [还原文件和文件组 (SQL Server)](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   [还原文件和文件组 (SQL Server)](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   [Restore.SqlRestore 方法（服务器）(SMO)](/dotnet/api/microsoft.sqlserver.management.smo.restore.sqlrestore)   
  
## <a name="see-also"></a>另请参阅  
 [备份和还原：互操作性和共存 &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-and-restore-interoperability-and-coexistence-sql-server.md)   
 [差异备份 (SQL Server)](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [完整文件备份 (SQL Server)](../../relational-databases/backup-restore/full-file-backups-sql-server.md)   
 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [完整数据库还原（简单恢复模式）](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)   
 [段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
