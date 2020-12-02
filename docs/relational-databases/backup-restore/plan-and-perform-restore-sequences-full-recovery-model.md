---
title: 还原顺序（完整恢复模式）| Microsoft Docs
description: 了解如何为通常使用完整恢复模式的 SQL Server 数据库计划和执行还原顺序（由 RESTORE 语句组成）。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restore sequences [SQL Server], planning for
- full recovery model [SQL Server], planning restore sequences
ms.assetid: 9cbefaf8-d2b6-41c9-83fc-b3807a841fe2
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8ac088e916736ee852cd828735c988f134ec4978
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129194"
---
# <a name="plan-and-perform-restore-sequences-full-recovery-model"></a>计划和执行还原顺序（完整恢复模式）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何为通常使用完整恢复模式的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库计划和执行一个还原顺序。 “还原顺序”  是一个或多个 [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) 语句的顺序。 通常，还原顺序会初始化要还原的数据库、文件和/或页的内容（数据复制阶段），前滚记录的事务（重做阶段）以及回滚未提交的事务（撤消阶段）。  
  
 在简单情况下，还原操作只需要一个完整数据库备份、一个差异数据库备份和后续日志备份。 在这些情况下，很容易构造一个正确的还原顺序。 例如，若要将整个数据库还原到故障点，请首先备份活动事务日志（日志的尾部  ）。 然后，按备份的创建顺序还原最新的完整数据库备份、最新的差异备份（如果有）以及所有后续日志备份。  
  
 在更复杂的情况下，构造一个正确的还原顺序可能是个复杂的过程。 例如，还原顺序可能需要多个文件备份，或者需要将数据还原到特定时间点。 在非常复杂的情况下，您甚至可能需要遍历跨一个或多个恢复分叉的分叉恢复路径。  
  
> [!NOTE]  
>  恢复路径  是使数据库到达特定时间点（称为恢复点）的一组数据和日志备份。 恢复路径是一组特定的转换，这些转换随着时间的变化对数据库进行演变并保持数据库的一致性。 恢复路径描述从“起点”(LSN,GUID) 到“终点”(LSN,GUID) 的一组 LSN。 恢复路径中的 LSN 可以从起点到终点遍历一个或多个恢复分支。  
  
## <a name="to-plan-a-restore-sequence"></a>计划还原顺序  
 启动还原顺序之前，请执行下列步骤：  
  
1.  创建数据库的结尾日志备份（如果可以）。 有关详细信息，请参阅[结尾日志备份 (SQL Server)](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)。  
  
2.  确定目标恢复点。  
  
     目标恢复点可以是事务日志备份中的任何时间点或标记。 有关详细信息，请参阅[将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)或[使用标记的事务一致恢复相关数据库（完全恢复模式）](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md)。  
  
3.  确定要执行的还原类型。 有关详细信息，请参阅 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)。  
  
4.  标识您需要的备份，并确保必要的介质集和备份设备可用。 有关详细信息，请参阅[备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)和[媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)。  
  
## <a name="to-perform-a-restore-sequence"></a>执行还原顺序  
 若要执行还原顺序，请执行下列步骤：  
  
1.  若要启动该顺序，请还原一个或多个数据备份（例如数据库备份、部分备份、一个或多个文件备份）。  
  
2.  也可以还原基于这些完整备份的最新差异备份。  
  
     对于计划还原的每个完整备份，确定它是否是任何差异备份的基础。 如果是，还原最新的差异备份（如果可以）。 有关详细信息，请参阅 [差异备份 (SQL Server)](../../relational-databases/backup-restore/differential-backups-sql-server.md)。  
  
3.  通过按顺序还原日志备份、完成包含恢复点的备份来前滚数据库。 是否必须应用所有日志备份取决于日志备份包含什么样的目标恢复点，如下所示：  
  
    -   如果恢复点是故障点，则必须还原自上一次还原数据（完整或差异）备份以来创建的所有日志备份。 有关详细信息，请参阅[应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)。  
  
    -   对于时点还原，您可能不需要最新的日志备份。 如果使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]，数据库恢复顾问确保仅选择需要恢复到指定时点的那些备份。 这些备份构成了为您的时点还原建议的还原计划。 有关详细信息，请参阅[将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)。  
  
## <a name="restarting-a-restore-sequence"></a>重新启动还原顺序  
 如果还原顺序的结果有问题，则可以退出，并从头开始重新启动还原顺序。 例如，如果意外还原了过多的日志备份并超过了想要的恢复点，则必须重新开始还原顺序，直至包含目标恢复点的日志备份。  
  
## <a name="see-also"></a>另请参阅  
 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [完整数据库还原（完整恢复模式）](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md)   
 [联机还原 (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [文件还原（完整恢复模式）](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [还原页 (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
  
