---
title: 联机还原 (SQL Server) | Microsoft Docs
description: 在 SQL Server Enterprise 版本中，某些情况下，你可以在数据库保持联机状态时，通过文件、页面或段落还原来还原数据。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- online restores [SQL Server]
- online restores [SQL Server], about online restores
ms.assetid: 7982a687-980a-4eb8-8e9f-6894148e7d8c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 00cec6d2ce9fcb74151bcbc6e87a58f9b8fbbfe2
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130377"
---
# <a name="online-restore-sql-server"></a>联机还原 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  只有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition 支持联机还原。 在此版本中，文件还原、页面还原或段落还原默认处于联机状态。 本主题与包含多个文件或文件组的数据库相关；在简单恢复模式下，仅与包含只读文件组的数据库相关。  
  
 数据库联机时还原数据的过程称为“联机还原”  。 只要主文件组处于联机状态，就将数据库视为联机，即使有一个或多个辅助文件组处于脱机状态。 在任何恢复模式下，您都可以在数据库联机时还原处于脱机状态的文件。 在完整恢复模式下，您还可以在数据库联机时还原页。  
  
> [!NOTE]  
>  联机还原自动在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise 上执行，不需要用户执行任何操作。 如果不想使用联机还原，则可以在开始还原之前使数据库脱机。 有关详细信息，请参阅本主题后面的 [使数据库或文件脱机](#taking_db_or_file_offline)。  
  
 联机文件还原期间，正在还原的任何文件及其文件组均处于脱机状态。 如果联机还原开始时这些文件中有任何一个处于联机状态的话，则第一个还原语句将使该文件的文件组脱机。 相反，联机页还原期间，只有页处于脱机状态。  
  
 所有联机还原方案均涉及以下几个基本步骤：  
  
1.  还原数据。  
  
2.  使用 WITH RECOVERY 还原最近的日志。 此操作将使还原的数据联机。  

 有时，未提交的事务无法回滚，因为回滚所需的数据在启动时处于脱机状态。 在这种情况下，将延迟该事务。 有关详细信息，请参阅 [延迟的事务 (SQL Server)](../../relational-databases/backup-restore/deferred-transactions-sql-server.md)中删除失效的文件组。  
  
> [!NOTE]  
>  如果数据库正在使用大容量日志恢复模式，建议您在开始执行联机还原之前切换到完整恢复模式。 有关详细信息，请参阅[查看或更改数据库的恢复模式 (SQL Server)](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md)。  
  
> [!IMPORTANT]  
>  如果创建备份时有多个设备连接到服务器，则联机还原期间，可用设备数必须相同。  
  
> [!CAUTION]  
>  使用快照备份时，不能执行 **Online Restore**。 有关 **快照备份** 的详细信息，请参阅 [Azure 中数据库文件的文件快照备份](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)。  
  
## <a name="log-backups-for-online-restore"></a>联机还原所需的日志备份  
 对于联机还原，恢复点就是要恢复的数据最后一次变为脱机或只读时的点。 截止到该恢复点（包括该恢复点）的事务日志备份都必须是可用的。 通常在该点后都需要日志备份，以覆盖文件的恢复点。 唯一的例外情况是在使用当数据变为只读后执行的数据备份对只读数据进行联机还原的时候。 在这种情况下，不必准备日志备份。  
  
 通常，即使在启动还原顺序之后，您也可以在数据库联机时执行事务日志备份。 上次日志备份的时间取决于要还原的文件的属性：  
  
-   对于联机只读文件，可以在第一次还原顺序之前或期间执行恢复所需的上次日志备份。 如果在文件组变为只读以后执行数据备份或差异备份，则只读文件组可能不需要日志备份。  
  
    > [!NOTE]  
    >  以上信息也适用于所有脱机文件。  
  
-   值得注意的特殊情况是这样一种读写文件：在执行第一个还原语句时处于联机状态，然后被该还原语句自动变为脱机的读写文件。 在这种情况下，必须在第一个 *还原顺序* （用于还原、前滚和恢复数据的一个或多个 RESTORE 语句的顺序）期间执行日志备份。 通常，必须在还原所有完整备份之后并在恢复数据之前执行日志备份。 但是，如果特定的文件组有多个文件备份，则最小的日志备份点为文件组脱机的时候。 这个数据还原后的日志备份将捕获文件变为脱机时的点。 数据还原后的日志备份是必要的，因为 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 无法将联机日志用于联机还原。  
  
    > [!NOTE]  
    >  或者，也可以在开始还原顺序前手动使文件脱机。 有关详细信息，请参阅本主题后面的“使数据库或文件脱机”。  
  
##  <a name="taking-a-database-or-file-offline"></a><a name="taking_db_or_file_offline"></a> 使数据库或文件脱机  
 如果不想使用联机还原，则可以使用以下方法之一，在启动还原顺序之前使数据库脱机：  
  
-   在任何恢复模式下，您都可以使用以下 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) 语句使数据库脱机：  
  
     ALTER DATABASE *database_name* SET OFFLINE  
  
-   或者，在完整恢复模式下，可以通过使用以下 [BACKUP LOG](../../t-sql/statements/backup-transact-sql.md) 语句将数据库置于还原状态，强制文件还原或页还原脱机：  
  
     BACKUP LOG *database_name* WITH NORECOVERY。  
  
 只要数据库保持脱机状态，所有还原就都是脱机还原。  
  
## <a name="examples"></a>示例  
  
> [!NOTE]  
>  联机还原顺序的语法与脱机还原顺序的语法完全相同。  
  
-   [示例：数据库的段落还原（简单恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [示例：仅对某些文件组进行段落还原（简单恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
-   [示例：只读文件的联机还原（简单恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [示例：数据库的段落还原（完整恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [示例：仅对某些文件组进行段落还原（完整恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [示例：读/写文件的联机还原（完整恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)  
  
-   [示例：只读文件的联机还原（完整恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [还原文件和文件组 (SQL Server)](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   [还原页 (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md)  
  
-   [管理 suspect_pages 表 (SQL Server)](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
-   [恢复数据库但不还原数据 (Transact-SQL)](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md)  
  
-   [删除失效文件组 (SQL Server)](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [文件还原（完整恢复模式）](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [文件还原（简单恢复模式）](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [还原页 (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)  
  
  
