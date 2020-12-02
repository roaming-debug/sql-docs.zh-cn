---
title: 联机还原：读/写文件（完整恢复模式）
description: 此示例演示对于使用简单恢复模式以及多个文件组的数据库，如何在 SQL Server 中对读/写文件执行联机还原。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full recovery model [SQL Server], RESTORE example
- online restores [SQL Server], full recovery model
- restore sequences [SQL Server], online
ms.assetid: 0dbeda81-1464-44ba-9011-914900096368
author: cawrites
ms.author: chadam
ms.openlocfilehash: ce68e817070765a6f84a12c518e71de221734b34
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96126995"
---
# <a name="example-online-restore-of-a-read-write-file-full-recovery-model"></a>示例：读/写文件的联机还原（完整恢复模式）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题与完整恢复模式下包含多个文件或文件组的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库相关。  
  
 在此示例中，名为 `adb`的数据库（使用完整恢复模式）包含三个文件组。 文件组 `A` 为读/写文件组，文件组 `B` 和文件组 `C` 为只读文件组。 最初，所有文件组都处于联机状态。  
  
 文件组 `a1` 中的文件 `A` 似乎已损坏，数据库管理员决定在数据库处于联机状态时还原该文件。  
  
> [!NOTE]  
>  在简单恢复模式下，不允许联机还原读/写数据。  
  
## <a name="restore-sequences"></a>还原顺序  
  
> [!NOTE]  
>  联机还原顺序的语法与脱机还原顺序的语法完全相同。  
  
1.  联机还原文件 `a1`。  
  
    ```  
    RESTORE DATABASE adb FILE='a1' FROM backup   
    WITH NORECOVERY;  
    ```  
  
     此时，文件 a1 处于 RESTORING 状态，文件组 A 处于脱机状态。  
  
2.  完成文件还原之后，数据库管理员进行新的日志备份以确保捕获到该文件脱机时的点。  
  
    ```  
    BACKUP LOG adb TO log_backup3;   
    ```  
  
3.  联机还原日志备份。  
  
     管理员还原自从还原了文件备份后一直到获得最新日志备份（在步骤 2 中创建的 *log_backup3*）所进行的所有日志备份。 还原最后一个备份之后，应当恢复数据库。  
  
    ```  
    RESTORE LOG adb FROM log_backup1 WITH NORECOVERY;  
    RESTORE LOG adb FROM log_backup2 WITH NORECOVERY;  
    RESTORE LOG adb FROM log_backup3 WITH NORECOVERY;  
    RESTORE DATABASE adb WITH RECOVERY;  
    ```  
  
     文件 `a1` 现处于联机状态。  
  
## <a name="additional-examples"></a>其他示例  
  
-   [示例：数据库的段落还原（简单恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [示例：仅对某些文件组进行段落还原（简单恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
-   [示例：只读文件的联机还原（简单恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [示例：数据库的段落还原（完整恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [示例：仅对某些文件组进行段落还原（完整恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [示例：只读文件的联机还原（完整恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
## <a name="see-also"></a>另请参阅  
 [联机还原 (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
