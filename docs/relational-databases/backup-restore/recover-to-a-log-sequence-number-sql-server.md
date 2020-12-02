---
title: 恢复到日志序列号 (SQL Server) | Microsoft Docs
description: 在 SQL Server 中，可以使用日志序列号 (LSN) 恢复到特定点。 此功能面向工具供应商。
ms.custom: ''
ms.date: 10/23/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- log sequence numbers [SQL Server]
- STOPBEFOREMARK option [RESTORE statement]
- STOPATMARK option [RESTORE statement]
- point in time recovery [SQL Server]
- restoring databases [SQL Server], point in time
- recovery [SQL Server], databases
- restoring [SQL Server], point in time
- LSNs
- database recovery [SQL Server]
- database restores [SQL Server], point in time
ms.assetid: f7b3de5b-198d-448d-8c71-1cdd9239676c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 685f7f2ec51688045a528a3cd04f89b043600c9d
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129158"
---
# <a name="recover-to-a-log-sequence-number-sql-server"></a>恢复到日志序列号 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题仅与使用完整恢复模式或大容量日志恢复模式的数据库相关。  
  
 您可以使用日志序列号 (LSN) 定义还原操作的恢复点。 但是，这是为工具供应商提供的专用功能，不太可能广泛使用。  
  
##  <a name="overview-of-log-sequence-numbers"></a><a name="LSNs"></a> 日志序列号的概述  
 RESTORE 顺序期间，在内部使用 LSN 跟踪数据还原到的时间点。 还原备份后，数据被还原到与进行备份的时间点相对应的 LSN。 差异和日志备份将还原的数据库推到稍后的时间，该时间与一个更高的 LSN 相对应。 有关 LSN 的详细信息，请参阅 [SQL Server 事务日志体系结构和管理指南](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)。  
  
> [!NOTE]  
> LSN 是数据类型为 numeric(25,0) 的值。 算术运算（例如加法或减法）对 LSN 没有任何意义，请不要与 LSN 一起使用。  
 
## <a name="viewing-lsns-used-by-backup-and-restore"></a>查看备份和还原使用的 LSN  
 使用下列一种或几种方法可以查看发生给定备份和还原事件的日志记录的 LSN：  
  
-   [backupset](../../relational-databases/system-tables/backupset-transact-sql.md)  
  
-   [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)  
  
-   [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)； [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
-   [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
> [!NOTE]  
>  LSN 还显示在错误日志的某些消息中。  
  
## <a name="transact-sql-syntax-for-restoring-to-an-lsn"></a>还原到 LSN 的 Transact-SQL 语法  
 通过使用 [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) 语句，可以在 LSN 处或刚好在 LSN 之前停止，如下所示：  
  
-   使用 WITH STOPATMARK ='lsn:<lsn_number>' 子句，其中 lsn:\<lsnNumber> 是一个字符串，它指出包含指定 LSN 的日志记录是恢复点。  
  
     STOPATMARK 前滚到 LSN，并且前滚中包括该日志记录。  
  
-   使用 WITH STOPBEFOREMARK ='lsn:<lsn_number>' 子句，其中 lsn:\<lsnNumber> 是一个字符串，它指出包含指定 LSN 编号的日志记录之前的日志记录是恢复点。  
  
     STOPBEFOREMARK 前滚到 LSN，并从前滚中排除该日志记录。  
  
 通常会选择要包括或排除的特定事务。 虽然实践中并不总是如此，但指定的日志记录就是事务提交记录。  
  
## <a name="examples"></a>示例  
 以下示例假定 `AdventureWorks` 数据库已更改为使用完整恢复模式。  
  
```sql  
RESTORE LOG AdventureWorks FROM DISK = 'c:\adventureworks_log.bak'   
WITH STOPATMARK = 'lsn:15000000040000037'  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [在完整恢复模式下将数据库还原到故障点 (Transact-SQL)](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [将数据库还原到标记的事务 (SQL Server Management Studio)](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
-   [将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
  
## <a name="see-also"></a>另请参阅  
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)     
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)     
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery)       
 [SQL Server 事务日志体系结构和管理指南](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)      
  
