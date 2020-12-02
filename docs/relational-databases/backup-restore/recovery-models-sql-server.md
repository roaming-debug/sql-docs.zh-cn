---
title: 恢复模式 (SQL Server) | Microsoft Docs
description: 在 SQL Server 中，恢复模式控制如何记录事务、事务日志是否需要备份以及可以使用哪些还原操作。
ms.custom: ''
ms.date: 07/16/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- database backups [SQL Server], recovery models
- bulk-logged recovery model [SQL Server]
- recovery [SQL Server], recovery model
- restoring transaction logs [SQL Server], recovery models
- backing up databases [SQL Server], recovery models
- recovery models [SQL Server], about
- transaction log backups [SQL Server]
- simple recovery model [SQL Server]
- backups [SQL Server], recovery models
- recovery models [SQL Server]
- recovery models [SQL Server], transaction log management
- database restores [SQL Server], recovery models
- transaction log restores [SQL Server]
- logs [SQL Server], recovery models
- restoring databases [SQL Server], recovery models
- full recovery model [SQL Server]
- backing up transaction logs [SQL Server], recovery models
ms.assetid: 8cfea566-8f89-4581-b30d-c53f1f2c79eb
author: cawrites
ms.author: chadam
ms.openlocfilehash: a7b746b8bd323b7037a7a477b33596b554aed096
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130353"
---
# <a name="recovery-models-sql-server"></a>恢复模式 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份和还原操作发生在数据库的恢复模式的上下文中。 恢复模式旨在控制事务日志维护。 “恢复模式”  是一种数据库属性，它控制如何记录事务，事务日志是否需要（以及允许）进行备份，以及可以使用哪些类型的还原操作。 有三种恢复模式：简单恢复模式、完整恢复模式和大容量日志恢复模式。 通常，数据库使用完整恢复模式或简单恢复模式。 数据库可以随时切换为其他恢复模式。  
  
 **本主题内容：**  
  
-   [恢复模式概述](#RMov)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="recovery-model-overview"></a><a name="RMov"></a> 恢复模式概述  
 下表概述了这三种恢复模式。  
  
|恢复模式|说明|工作丢失的风险|能否恢复到时点？|  
|--------------------|-----------------|------------------------|-------------------------------|  
|**简单**|无日志备份。<br /><br /> 自动回收日志空间以减少空间需求，实际上不再需要管理事务日志空间。 有关简单恢复模式下数据库备份的详细信息，请参阅[完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/full-database-backups-sql-server.md)。<br /><br /> 简单恢复模式不支持要求事务日志备份的操作。 在简单恢复模式中不能使用以下功能：<br /><br /> -日志传送<br /><br /> -AlwaysOn 或数据库镜像<br /><br /> -没有数据丢失的介质恢复<br /><br /> -时点还原|最新备份之后的更改不受保护。 在发生灾难时，这些更改必须重做。|只能恢复到备份的结尾。 有关详细信息，请参阅[完整数据库还原（简单恢复模式）](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)。 <br><br> 有关简单恢复模式的更多深入说明，请参阅由 [SQL Server 简单恢复模式](https://www.mssqltips.com/sqlservertutorial/4/sql-server-simple-recovery-model/) 人员提供的 [SQL Server 简单恢复模式](https://www.mssqltips.com)。|  
|**完整**|需要日志备份。<br /><br /> 数据文件丢失或损坏不会导致丢失工作。<br /><br /> 可以恢复到任意时点（例如应用程序或用户错误之前）。 有关完整恢复模式下的数据库备份的信息，请参阅 [完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/full-database-backups-sql-server.md) 和[完整数据库还原（完整恢复模式）](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md)。|正常情况下没有。<br /><br /> 如果日志尾部损坏，则必须重做自最新日志备份之后所做的更改。|如果备份在接近特定的时点完成，则可以恢复到该时点。 有关使用日志备份还原到故障点的信息，请参阅[将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)。<br /><br /> 注意：如果有两个或更多必须在逻辑上保持一致的完整恢复模式数据库，则最好执行特殊步骤，以确保这些数据库的可恢复性。 有关详细信息，请参阅 [包含标记的事务的相关数据库的恢复](../../relational-databases/backup-restore/recovery-of-related-databases-that-contain-marked-transaction.md)。|  
|**大容量日志**|需要日志备份。<br /><br /> 是完整恢复模式的附加模式，允许执行高性能的大容量复制操作。<br /><br /> 通过使用最小方式记录大多数大容量操作，减少日志空间使用量。 有关尽量减少日志量的操作的信息，请参阅 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)。<br /><br /> 日志备份可能很大，因为日志备份中捕获了最低限度记录的操作。 有关大容量日志恢复模式下的数据库备份的信息，请参阅[完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/full-database-backups-sql-server.md) 和[完整数据库还原（完整恢复模式）](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md)。|如果在最新日志备份后发生日志损坏或执行大容量日志记录操作，则必须重做自该上次备份之后所做的更改。<br /><br /> 否则不丢失任何工作。|可以恢复到任何备份的结尾。 不支持时点恢复。|  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [查看或更改数据库的恢复模式 (SQL Server)](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md)  
  
-   [解决事务日志已满的问题（SQL Server 错误 9002）](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)  
  
## <a name="see-also"></a>另请参阅  
 [backupset (Transact-SQL)](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [自动执行管理任务（SQL Server 代理）](../../ssms/agent/automated-administration-tasks-sql-server-agent.md)   
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)  
  
