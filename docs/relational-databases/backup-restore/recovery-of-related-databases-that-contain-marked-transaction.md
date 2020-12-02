---
title: 恢复具有标记事务的相关数据库
description: SQL Server 支持将事务日志中的命名标记，以便可恢复到特定标记。 标记可以绑定到特定工作。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- transaction logs [SQL Server], marks
- STOPBEFOREMARK option [RESTORE statement]
- STOPATMARK option [RESTORE statement]
- point in time recovery [SQL Server]
- restoring databases [SQL Server], point in time
- recovery [SQL Server], databases
- restoring [SQL Server], point in time
- transactions [SQL Server], recovering to a mark
- database recovery [SQL Server]
- marked transactions [SQL Server], restoring
- database restores [SQL Server], point in time
ms.assetid: 77a0d9c0-978a-4891-8b0d-a4256c81c3f8
author: cawrites
ms.author: chadam
ms.openlocfilehash: e43b37dd96a931d98555f05fe6e70b9f8a4f99e3
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129175"
---
# <a name="recovery-of-related--databases-that-contain-marked-transaction"></a>包含标记事务的相关数据库恢复
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题仅适用于那些包含标记事务和使用完整或大容量日志恢复模式的数据库。  
  
 有关还原到特定恢复点的要求的详细信息，请参阅 [将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持将命名标记插入到事务日志中，以便恢复到该特定标记。 日志标记因事务而异，并且只有在提交与其关联的事务时才会插入。 因此可将标记绑定到特定的工作上，而且可恢复到包含或排除此工作的点。  
  
 将命名标记插入事务日志之前，请先考虑以下事项：  
  
-   由于事务标记会消耗日志空间，应只对在数据库恢复策略中起重要作用的事务使用标记。  
  
-   标记的事务提交之后，在 [msdb](../../relational-databases/system-tables/logmarkhistory-transact-sql.md) 的 **logmarkhistory** 表中插入一行。  
  
-   如果一个标记的事务跨同一数据库服务器或不同服务器上的多个数据库，这些标记将记录在所有受影响的数据库的日志内。 有关详细信息，请参阅 [使用标记的事务一致地恢复相关的数据库的事务（完全恢复模式）](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md)。  
  
> [!NOTE]  
>  有关如何标记事务的信息，请参阅 [使用标记的事务一致地恢复相关的数据库的事务（完全恢复模式）](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md)。  
  
## <a name="transact-sql-syntax-for-inserting-named-marks-into-a-transaction-log"></a>在事务日志中插入命名标记的 Transact-SQL 语法  
 若要将标记插入到事务日志中，请使用 [BEGIN TRANSACTION](../../t-sql/language-elements/begin-transaction-transact-sql.md) 语句和 WITH MARK [*description*] 子句。 标记的名称与事务的名称相同。 可选的 *description* 是标记的文本说明而不是标记名。 例如，在以下 `BEGIN TRANSACTION` 语句中创建的事务和标记的名称均为 `Tx1`：  
  
```sql  
BEGIN TRANSACTION Tx1 WITH MARK 'not the mark name, just a description'    
```  
  
 事务日志记录了标记名（事务名）、说明、数据库、用户、 **datetime** 信息和日志序列号 (LSN)。 **datetime** 信息与标记名一起用于唯一地标识标记。  
  
 有关如何将标记插入跨多个数据库的事务的信息，请参阅 [使用标记的事务一致地恢复相关的数据库的事务（完全恢复模式）](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md)。  
  
## <a name="transact-sql-syntax-for-recovering-to-a-mark"></a>恢复到某个标记的 Transact-SQL 语法  
 使用[RESTORE LOG](../../t-sql/statements/restore-statements-transact-sql.md)语句将某个标记的事务作为目标时，可以使用以下子句之一在标记处或在紧邻其之前的位置处停止：  
  
-   使用 WITH STOPATMARK = **'** _<mark_name>_ **'** 子句将标记事务指定为恢复点。  
  
     STOPATMARK 前滚到标记处，并在前滚中包含标记的事务。  
  
-   使用 WITH STOPBEFOREMARK = **'** <mark_name>  **'** 子句将紧邻标记之前的日志记录指定为恢复点。  
  
     STOPBEFOREMARK 前滚到标记处，但在前滚中不包含标记的事务。  
  
 STOPATMARK 和 STOPBEFOREMARK 选项均支持可选的 AFTER *datetime* 子句。 使用 *datetime* 时，标记名不必是唯一的。  
  
 如果省略了 AFTER *datetime* ，则前滚操作将于达到含有指定名称的第一个标记处停止。 如果指定了 AFTER *datetime* ，则前滚操作将于达到 *datetime* 时或之后在具有指定名称的第一个标记处停止。  
  
> [!NOTE]  
>  对于所有时点还原操作而言，如果数据库正在执行成批记录的操作，则不允许恢复到标记。  
  
 **还原到标记的事务**  
  
 [将数据库还原到标记的事务 (SQL Server Management Studio)](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
### <a name="preparing-the-log-backups"></a>为日志备份做准备  
 在此示例中，适合这两个相关数据库的备份策略如下：  
  
1.  对于这两个数据库，使用完整恢复模式。  
  
2.  创建每个数据库的完整备份。  
  
     可以按顺序或同时备份数据库。  
  
3.  备份事务日志之前，标记在所有数据库中执行的事务。 有关如何创建标记事务的详细信息，请参阅 [使用标记的事务一致地恢复相关的数据库的事务（完全恢复模式）](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md)。  
  
4.  备份每个数据库中的事务日志。  
  
### <a name="recovering-the-database-to-a-marked-transaction"></a>将数据库恢复到标记事务  
 **还原备份**  
  
1.  如有可能，创建未损坏的数据库的 [结尾日志备份](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) 。  
  
2.  还原每个数据库最近的完整数据库备份。  
  
3.  标识可用于所有事务日志备份的最新标记事务。 此信息存储在每个服务器的 **msdb** 数据库中的 **logmarkhistory** 表中。  
  
4.  标识包含此标记的所有相关数据库的日志备份。  
  
5.  还原每个日志备份，在标记事务处停止。  
  
6.  恢复每个数据库。  
  
## <a name="see-also"></a>另请参阅  
 [BEGIN TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [使用标记的事务一致地恢复相关的数据库的事务（完全恢复模式）](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md)   
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)   
 [计划和执行还原顺序（完整恢复模式）](../../relational-databases/backup-restore/plan-and-perform-restore-sequences-full-recovery-model.md)  
  
  
