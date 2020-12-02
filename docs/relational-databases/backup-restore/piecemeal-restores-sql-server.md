---
title: 段落还原 (SQL Server) | Microsoft Docs
description: 在 SQL Server 中，段落还原允许分阶段还原/恢复包含多个文件组的数据库。 段落还原适用于所有恢复模式。
ms.custom: ''
ms.date: 10/23/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- partial updates [SQL Server]
- staged restores [SQL Server]
- piecemeal restores [SQL Server]
- restoring [SQL Server], piecemeal restore scenario
ms.assetid: 208f55e0-0762-4cfb-85c4-d36a76ea0f5b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3681cad0b17f2241ae388c19ba34c18578bc258f
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130345"
---
# <a name="piecemeal-restores-sql-server"></a>段落还原 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 Enterprise 版本（联机还原）或 Standard 版本（脱机还原）中包含多个文件或文件组的数据库相关；在简单模式下，仅与包含只读文件组的数据库相关。  
  
 有关段落还原和内存优化表的信息，请参阅 [对具有内存优化表的数据库进行段落还原](../../relational-databases/in-memory-oltp/piecemeal-restore-of-databases-with-memory-optimized-tables.md)。  
  
  “段落还原”允许分阶段还原和恢复包含多个文件组的数据库。 段落还原包括从主文件组开始（有时也从一个或多个辅助文件组开始）的一系列还原顺序。 段落还原保持进行检查，以便确保数据库在结束时将是一致的。 在还原顺序结束后，如果恢复的文件有效并且与数据库一致，则恢复的文件将直接变为联机状态。  
  
 段落还原适用于所有恢复模式，但在完整恢复模式和大容量日志恢复模式下比在简单恢复模式下更灵活。  
  
 所有的段落还原都以称为“部分还原顺序”  的初始还原顺序开始。 部分还原顺序至少还原和恢复主文件组，在简单恢复模式下还会还原和恢复所有读/写文件组。 在段落还原顺序中，整个数据库都必须脱机。 随后，数据库将处于联机状态，并且还原的文件组都处于可用状态。 但是，所有未还原的文件组都将保持脱机状态，无法访问。 不过，对于任何脱机文件组，都可以在以后通过文件还原进行还原并进入联机状态。  
  
 无论数据库采用何种恢复模式，部分还原顺序都从 RESTORE DATABASE 语句开始，该语句将还原完整备份并指定 PARTIAL 选项。 PARTIAL 选项总是会启动一个新的段落还原；因此，在部分还原顺序的初始语句中，只能指定 PARTIAL 一次。 当部分还原顺序完成并且数据库联机后，由于余下文件的恢复被推迟，这些文件的状态将变为“恢复已挂起”。  
  
 此后，段落还原通常包括一个或多个还原顺序，这些还原顺序称为“文件组还原顺序”  。 您可以等待执行特定的文件组还原顺序，等待的时间长短由您决定。 每个文件组还原顺序将一个或多个脱机文件组还原并恢复到与数据库一致的点。 文件组还原顺序的时间安排和数量取决于您的恢复目标、您想要还原的脱机文件组数量以及每个文件组还原顺序中还原的脱机文件组的数量。  
  
 执行段落还原的精确要求取决于数据库的恢复模式。 有关详细信息，请参阅本主题后面的“简单恢复模式下的段落还原”和“完整恢复模式下的段落还原”。  
  
## <a name="piecemeal-restore-scenarios"></a>段落还原方案  
 所有版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 都支持脱机段落还原。 在 Enterprise Edition 中，段落还原可以联机进行也可以脱机进行。 脱机和联机段落还原的含义如下：  
  
-   脱机段落还原方案  
  
     在脱机段落还原中，数据库在部分还原顺序之后处于联机状态。 尚未还原的文件组保持脱机状态，但是在数据库脱机后，可以根据需要还原这些文件组。  
  
-   联机段落还原方案  
  
     在进行联机段落还原时，数据库在部分还原顺序后处于联机状态，并且主文件组和所有已恢复的辅助文件组都处于可用状态。 尚未还原的文件组保持脱机状态，但是在数据库保持联机状态时，可以根据需要还原这些文件组。  
  
     联机段落还原可能引起事务延迟。 如果仅还原了一部分文件组，则数据库中依赖联机文件组的事务可能会延迟。 这是正常现象，因为整个数据库必须一致。 有关详细信息，请参阅 [延迟的事务 (SQL Server)](../../relational-databases/backup-restore/deferred-transactions-sql-server.md)中删除失效的文件组。  
  
-   [!INCLUDE[hek_1](../../includes/hek-1-md.md)] 段落还原方案  
  
     有关内存中 OLTP 数据库的段落还原信息，请参阅 [对具有内存优化表的数据库进行段落备份和还原](../../relational-databases/in-memory-oltp/piecemeal-restore-of-databases-with-memory-optimized-tables.md)。  
  
## <a name="restrictions"></a>限制  
 如果部分还原顺序不包括任何 [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md) 文件组，则不支持时间点还原。 可以强制该还原顺序以继续执行操作。 但在 RESTORE 语句中省略的 FILESTREAM 文件组将永远无法还原。 若要强制执行时点还原，请指定 CONTINUE_AFTER_ERROR 选项以及 STOPAT、STOPATMARK 或 STOPBEFOREMARK 选项，还必须在随后的 RESTORE LOG 语句中指定后面的三个选项。 如果指定 CONTINUE_AFTER_ERROR，则部分还原顺序将成功，但 FILESTREAM 文件组将不可恢复。  
  
## <a name="piecemeal-restore-under-the-simple-recovery-model"></a>简单恢复模式下的段落还原  
 在简单恢复模式下，段落还原顺序必须从完整数据库备份或部分备份开始。 随后，如果还原的备份为差异基准，则接着还原最新差异备份。  
  
 在第一个部分还原顺序期间，如果您仅还原读/写文件组的一个子集，当您恢复部分还原的数据库时，未还原的文件组会失效。 从部分还原顺序中省略一个读/写文件组仅适用于下列情况：  
  
-   需要使未还原的文件组失效。  
  
-   还原顺序将到达某个恢复点，在此点处的每个未还原文件组（在部分还原顺序中的前一个还原过程中）已变成只读、已删除或失效。  
  
-   完整备份是在数据库使用简单恢复模式时进行的，但恢复点却位于数据库使用完整恢复模式的某个时刻。 有关详细信息，请参阅本主题后面的“执行已从简单恢复模式切换到完整恢复模式的数据库段落还原”。  
  
### <a name="requirements-for-piecemeal-restore-under-the-simple-recovery-model"></a>简单恢复模式下的段落还原要求  
 在简单恢复模式下，初始阶段还原并恢复主文件组和所有读/写辅助文件组。 在初始阶段结束后，如果恢复的文件有效并且与数据库一致，则恢复的文件将直接变为联机状态。  
  
 随后，可以在一个或多个其他阶段中还原只读文件组。  
  
 只有满足以下条件时，段落还原才适用于只读辅助文件组：  
  
-   在备份时处于只读状态。  
  
-   保持只读状态（逻辑上与主文件组保持一致）。  
  
 若要执行段落还原，必须遵照以下原则：  
  
-   简单恢复模式数据库的段落还原的完整备份集必须包含：  
  
    -   部分数据库备份或完整数据库备份，其中包含在备份时处于读/写状态的主文件组和所有文件组。  
  
    -   每个只读文件的备份。  
  
-   从备份开始直到包含主文件组的备份完成为止，辅助文件组必须始终为只读，才能使只读文件的备份与主文件组保持一致。 如果差异文件备份是在文件组变为只读状态之后进行的，则可以使用差异文件备份。  
  
### <a name="piecemeal-restore-stages-simple-recovery-model"></a>段落还原阶段（简单恢复模式）  
 段落还原方案包括下列阶段：  
  
-   初始阶段（还原并恢复主文件组和所有读/写文件组）  
  
     初始阶段执行部分还原。 部分还原顺序将还原主文件组、所有读/写辅助文件组和（可选的）部分只读文件组。 在初始阶段中，整个数据库都必须脱机。 初始阶段后，数据库将处于联机状态，并且已还原的文件组都处于可用状态。 但是，尚未还原的任何只读文件组将保持脱机状态。  
  
     初始阶段中的第一条 RESTORE 语句必须执行以下操作：  
  
    -   使用包含了在备份时处于读/写状态的主文件组和所有文件组的部分数据库备份或完整数据库备份。 常见的是通过还原部分备份来启动部分还原顺序。  
  
    -   指定 PARTIAL 选项，该选项指示段落还原的开头。  
  
    > [!NOTE]  
    >  PARTIAL 选项会执行安全检查，确保生成的数据库适合用作生产数据库。  
  
    -   如果备份是完整数据库备份，则指定 READ_WRITE_FILEGROUPS 选项。  
  
-   数据库处于联机状态时，可以使用一个或多个联机文件还原来还原并恢复在备份时处于只读状态的脱机只读文件。 联机文件还原的时间安排取决于您希望数据联机的时间。  
  
     您是否必须将数据还原到文件取决于：  
  
    -   通过恢复文件而不还原任何数据，可以直接使与数据库一致的有效只读文件联机。  
  
    -   恢复文件之前，必须先还原已损坏的或与数据库不一致的文件。  
  
### <a name="examples"></a>示例  
  
-   [示例：数据库的段落还原（简单恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [示例：仅对某些文件组进行段落还原（简单恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
## <a name="piecemeal-restore-under-the-full-recovery-model"></a>完整恢复模式下的段落还原  
 在完整恢复模式或大容量日志恢复模式下，任何包含多个文件组的数据库都可以使用段落还原，并且可以将数据库还原到任何时间点。 段落还原的还原顺序如下：  
  
-   部分还原顺序  
  
     部分还原顺序会还原主文件组和（可选的）部分辅助文件组。  
  
     第一个 RESTORE DATABASE 语句必须执行以下操作：  
  
    -   指定 PARTIAL 选项。 它表示段落还原的开始。  
  
    -   使用包含主文件组的任何完整数据库备份。 常见的做法是通过还原部分备份来启动部分还原顺序。  
  
    -   若要还原到特定的时间点，必须在部分还原顺序中指定该时间。 还原顺序的每个后续步骤都必须指定相同的时间点。  
  
-   文件组还原顺序会使其他文件组联机并处于与数据库一致的某个点。  
  
     在 Enterprise Edition 中，当数据库保持联机状态时，可还原和恢复任何脱机辅助文件组。 如果特定只读文件未损坏且与数据库一致，则该文件无需还原。 有关详细信息，请参阅[恢复数据库而不还原数据 (Transact-SQL)](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md)。  
  
### <a name="applying-log-backups"></a>应用日志备份  
 如果在文件备份创建之前，只读文件组就已处于只读状态，则该文件组无需应用日志备份，并且文件还原会跳过日志备份的应用过程。 如果文件组是读/写文件组，则必须将未中断的日志备份链应用于上一次完整还原或差异还原，文件组才能前进到当前的日志文件。 有关恢复过程的详细信息，请参阅[还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery)。
  
### <a name="examples"></a>示例  
  
-   [示例：数据库的段落还原（完整恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [示例：仅对某些文件组进行段落还原（完整恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
## <a name="performing-a-piecemeal-restore-of-a-database-whose-recovery-model-has-been-switched-from-simple-to-full"></a>执行已从简单恢复模式切换到完整恢复模式的数据库段落还原  
 可以对自完整或部分数据库备份后已从简单恢复模式切换到完整恢复模式的数据库执行段落还原。 例如，假设要对某个数据库执行下列步骤：  
  
1.  创建简单模式数据库的一个部分备份 (backup_1)。  
  
2.  一段时间后，将恢复模式更改为完整恢复模式。  
  
3.  创建一个差异备份。  
  
4.  开始进行日志备份。  
  
 此后，下列顺序是有效的：  
  
1.  省略一些辅助文件组的部分还原。  
  
2.  后面跟有其他所需还原的差异还原。  
  
3.  随后为 backup_1 部分备份中读/写辅助文件组 (WITH NORECOVERY) 的文件还原  
  
4.  后面跟有在原始段落还原顺序中还原的用于将数据还原到原始恢复点的任何其他备份的差异备份。  

## <a name="see-also"></a>另请参阅  
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)   
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [计划和执行还原顺序（完整恢复模式）](../../relational-databases/backup-restore/plan-and-perform-restore-sequences-full-recovery-model.md)    
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)     
  
