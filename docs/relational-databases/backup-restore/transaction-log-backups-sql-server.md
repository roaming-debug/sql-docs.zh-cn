---
title: 事务日志备份 (SQL Server) | Microsoft Docs
description: 可以经常独立于数据库备份来备份 SQL Server 事务日志。 事务日志备份的序列是日志链。
ms.custom: ''
ms.date: 01/05/2018
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backing up [SQL Server], transaction logs
- transaction log backups [SQL Server], creating
- log backups [SQL Server]
- transaction log backups [SQL Server], sequencing
ms.assetid: f4a44a35-0f44-4a42-91d5-d73ac658a3b0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5e8978ed490360c4ef49b7747606bf9cd82870d0
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125465"
---
# <a name="transaction-log-backups-sql-server"></a>事务日志备份 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题仅与使用完整恢复模式或大容量日志恢复模式的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库相关。 本主题讨论备份 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的事务日志。  
  
 在创建任何日志备份之前，您必须至少创建一个完整备份。 然后，可以随时备份事务日志，除非已备份此日志。 
 
建议经常执行日志备份，这样既可尽量减少丢失工作的风险，也可以截断事务日志。 
 
数据库管理员通常偶尔（如每周）会创建完整数据库备份，还可以选择以较短间隔（如每天）创建一系列差异备份。 与数据库备份无关，数据库管理员可以比较频繁地创建事务日志备份。 对于给定的备份类型，最恰当的备份间隔取决于一系列因素，如数据的重要性、数据库的大小和服务器的工作负荷。 有关实现好策略的详细信息，请参阅本主题中的[建议](#Recommendations)。 
   
##  <a name="how-a-sequence-of-log-backups-works"></a><a name="LogBackupSequence"></a> 日志备份顺序的工作方式  
 事务日志备份“日志链”  的序列与数据备份无关。 例如，假设有下列事件顺序。  
  
|时间|事件|  
|----------|-----------|  
|上午 8:00|备份数据库。|  
|中午|备份事务日志。|  
|下午 4:00|备份事务日志。|  
|晚上 6:00|备份数据库。|  
|晚上 8:00|备份事务日志。|  
  
 晚上 8:00 创建的事务日志备份包含从下午 4:00 到晚上 8:00 的事务日志记录，跨越了在晚上 6:00 创建完整数据库备份的时间。从上午 8:00 创建的初始完整数据库备份一直到晚上 8:00 创建的最后事务日志备份，事务日志备份序列保持连续。 有关如何应用这些日志备份的信息，请参阅 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).中的示例。  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   如果事务日志损坏，则最新有效备份以后执行的工作将丢失。 因此，我们强烈建议您将日志文件存储在容错的存储设备中。  
  
-   如果数据库已损坏，或者你要还原数据库，建议你创建一个 [结尾日志备份](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) ，使你可以将数据库还原到当前时间点。  
  
-   默认情况下，每个成功的备份操作都会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志和系统事件日志中添加一个条目。 如果非常频繁地备份日志，这些成功消息会迅速累积，从而产生一个巨大的错误日志，这样会使查找其他消息变得非常困难。 在这些情况下，如果任何脚本均不依赖于这些日志条目，则可以使用跟踪标志 3226 取消这些条目。 有关详细信息，请参阅[跟踪标志 (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。  

-   请经常进行日志备份，其频率应足够支持业务需求，尤其是对损坏的日志存储可能导致的数据丢失的容忍程度。 
   -   适当的日志备份频率取决于您对工作丢失风险的容忍程度与所能存储、管理和潜在还原的日志备份数量之间的平衡。 实现恢复策略时，请考虑必需的 [RTO](https://wikipedia.org/wiki/Recovery_time_objective) 和 [RPO](https://wikipedia.org/wiki/Recovery_point_objective)，特别是日志备份频率。
   -   每 15 到 30 分钟进行一次日志备份可能就已足够。 但是如果您的业务要求将工作丢失的风险最小化，请考虑进行更频繁的日志备份。 频繁的日志备份还有增加日志截断频率的优点，其结果是日志文件较小。  
  
> [!IMPORTANT]
> 若要限制需要还原的日志备份的数量，必须定期备份数据。 例如，可以制定这样一个计划：每周进行一次完整数据库备份，每天进行若干次差异数据库备份。  
> 同样，实现恢复策略时，请考虑所需 [RTO](https://wikipedia.org/wiki/Recovery_time_objective) 和 [RPO](https://wikipedia.org/wiki/Recovery_point_objective)，尤其是完整和差异的数据库备份频率。
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **创建事务日志备份**  
  
-   [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Backup.SqlBackup%2A> (SMO)  
  
 若要计划备份作业，请参阅 [Use the Maintenance Plan Wizard](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)。  
  

## <a name="see-also"></a>另请参阅  
 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [SQL Server 事务日志体系结构和管理指南中的事务日志备份](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Backups)     
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [结尾日志备份 (SQL Server)](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)   
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)  
  
  
