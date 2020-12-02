---
title: 延迟的事务 (SQL Server) | Microsoft Docs
description: 如果回滚所需的数据处于脱机状态，则会在 SQL Server Enterprise 中出现延迟的事务。 了解如何将这些事务移出延迟状态。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- I/O [SQL Server], database recovery
- restoring pages [SQL Server]
- deferred transactions
- modifying transaction deferred state
ms.assetid: 6fc0f9b6-d3ea-4971-9f27-d0195d1ff718
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8d748a5997df33ab9448fc9ba604ae700b87c0ee
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127030"
---
# <a name="deferred-transactions-sql-server"></a>延迟的事务 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 企业版中，如果在数据库启动过程中回滚（撤消）所需的数据处于脱机状态，则损坏的事务可能延迟。 “延迟的事务”  是指前滚阶段结束时未提交的事务或遇到错误而无法回滚的事务。 因为无法回滚事务，所以事务将延迟。  
  
> [!NOTE]  
>  仅在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise 中才会延迟损坏的事务。 在其他版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，事务损坏将导致启动失败。  
  
 一般来说，如果前滚数据库时遇到 I/O 错误而无法读取事务所需的页面，则将导致事务延迟。 但是，文件级的错误也会导致延迟的事务。 如果部分还原顺序在某点停止，在该点上必须对事务进行回滚而事务所需的数据处于脱机状态，这亦会导致事务延迟。  
  
 如果用户事务在回滚过程中遇到 I/O 错误，则会导致整个数据库脱机。 当数据库恢复联机时，重做操作将重新获得它曾持有的所有锁，并尝试回滚所有未提交的事务。 事务所修改的所有数据都将保持适当锁定的状态，直到可以回滚事务时为止。 在修复损坏并重新启动数据库时，或通过联机还原在数据库保持联机的同时解决了延迟的事务时，无法回滚的事务将放弃其持有的锁。 在此之前，延迟的事务可持有锁，以防止对整个数据库执行某些操作。 例如，如果延迟的事务包含 CREATE TABLE 指令，则在解决延迟的事务之前，任何用户都无法创建表。  
  
 如果段落还原将数据库恢复到某个点，在该点上有一个或多个活动事务会影响尚未还原并处于脱机状态的文件组，则将发生事务延迟。 因为无法回滚事务，所以事务将延迟。  
  
 下表列出了导致数据库执行恢复的操作以及发生 I/O 问题的后果。  
  
|操作|解决方法（如果遇到 I/O 问题或所需数据处于脱机状态）|  
|------------|-----------------------------------------------------------------------|  
|服务器启动|延迟的事务|  
|还原|延迟的事务|  
|附加|附加失败|  
|自动重新启动|延迟的事务|  
|创建数据库或数据库快照|创建失败|  
|在数据库镜像上执行的重做操作|延迟的事务|  
|文件组脱机|延迟的事务|  
  
### <a name="requirements-and-limitations"></a>要求和限制

 - 数据库必须使用完整恢复模式或大容量日志恢复模式。
 - 必须为数据库完成至少一个数据库和日志备份
 - 延迟的事务不适用于数据库联机后回滚事务期间遇到的错误。 （例如运行时错误）
 - 数据库附加期间，无法为恢复失败延迟事务
 - 诸如系统事务 （例如页面分配）之类的某些事务无法延迟

## <a name="moving-a-transaction-out-of-the-deferred-state"></a>将事务移出 DEFERRED 状态  
  
> [!IMPORTANT]  
>  延迟的事务会使事务日志保持活动状态。 在延迟的事务脱离延迟状态之前无法截断包含这些事务的虚拟日志文件。 有关日志截断的详细信息，请参阅[事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)。  
  
 若要使事务脱离延迟状态，数据库必须在没有任何 I/O 错误的情况下顺利启动。 如果存在延迟的事务，则必须修复 I/O 错误源。 下面便是可用的解决方案，它们按照通常尝试执行的顺序列出：  
  
-   重新启动数据库。 如果问题是暂时的，数据库应该会启动，而且没有延迟的事务。  
  
-   如果事务由于文件组脱机而延迟，请将文件组重新联机。  
  
     若要使脱机文件组恢复联机，请使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```  
    RESTORE DATABASE database_name FILEGROUP=<filegroup_name>  
    ```  
  
-   还原数据库。 进行联机还原后，将解决所有延迟的事务。  
  
     在完整恢复模式或大容量日志恢复模式下，如果延迟的事务仅仅是由少量的损坏页引起的，则可以通过联机页面还原解决这些错误（如果支持）。  
  
-   如果不再需要因脱机状态而导致事务延迟的文件组，请使这样的脱机文件组失效。 这样的文件组失效之后，由于这些文件组的脱机而延迟的事务将脱离延迟状态。  
  
    > [!IMPORTANT]  
    >  从不恢复失效的文件组。  
  
     有关详细信息，请参阅 [删除失效文件组 (SQL Server)](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md)。  
  
-   如果由于页的错误而致使事务延迟，并且没有数据库的完好备份，请按照以下步骤修复数据库：  
  
    -   首先通过执行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句将数据库置为紧急模式：  
  
        ```  
        ALTER DATABASE <database_name> SET EMERGENCY  
        ```  
  
         有关紧急模式的信息，请参阅 [Database States](../../relational-databases/databases/database-states.md)。  
  
    -   然后，通过在以下 DBCC 语句之一中使用 DBCC REPAIR_ALLOW_DATA_LOSS 选项修复数据库：[DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)、[DBCC CHECKALLOC](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md) 或 [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)。  
  
         在遇到错误的页时，DBCC 将释放该页并修复所有相关错误。 此方法可以使数据库重新联机并处于物理上一致的状态。 但是，还可能会丢失其他数据；因此，应在不得已的情况下才使用此方法。  
  
## <a name="see-also"></a>另请参阅  
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [删除失效文件组 (SQL Server)](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md)   
 [文件还原（完整恢复模式）](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [文件还原（简单恢复模式）](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [还原页 (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
