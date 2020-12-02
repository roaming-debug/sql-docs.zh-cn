---
title: 段落还原：仅某些文件组（简单恢复模式）
description: 此示例演示对于使用简单恢复模式的数据库，如何在 SQL Server 中仅对某些文件组执行段落还原。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- piecemeal restores [SQL Server], simple recovery model
- restore sequences [SQL Server], piecemeal
- simple recovery model [SQL Server], RESTORE examples
ms.assetid: d7ad026c-5355-4308-9560-0dc843940d4f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 977945ad8d48996c55da7125fa8f58af0a53782d
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96126923"
---
# <a name="example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model"></a>示例：仅对某些文件组进行段落还原（简单恢复模式）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题针对采用简单恢复模式并包含只读文件组的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。  
  
 段落还原顺序在文件组级别分阶段还原和恢复数据库，并从主文件组和所有读写辅助文件组开始还原和恢复。  
  
 在此示例中，使用简单恢复模式的名为 `adb`的数据库包含三个文件组。 文件组 `A` 为读/写文件组，文件组 `B` 和文件组 `C` 为只读文件组。 最初，所有文件组都处于联机状态。  
  
 数据库 `B` 的主文件组和文件组 `adb` 显示为已损坏；因此数据库管理员决定使用段落还原顺序还原这些文件组。 在简单恢复模式下，所有读/写文件组都必须从同一个部分备份还原。 尽管文件组 `A` 未损坏，但它也必须随主文件组一起还原，以确保它们保持一致（数据库将还原到在上一次部分备份结束时定义的时点）。 文件组 `C` 未损坏，但必须对其进行恢复才能使其联机。 尽管文件组 `B`已损坏，但它包含的关键数据比文件组 `C`包含的关键数据要少；因此将最后还原 `B` 。  
  
## <a name="restore-sequences"></a>还原顺序  
  
> [!NOTE]  
>  联机还原顺序的语法与脱机还原顺序的语法完全相同。  
  
1.  从部分备份中部分还原主文件组和文件组 `A` 。  
  
    ```  
    RESTORE DATABASE adb READ_WRITE_FILEGROUPS FROM partial_backup   
    WITH PARTIAL, RECOVERY  
    ```  
  
     此时，主文件组和文件组 `A` 处于联机状态。 文件组 `B` 和 `C` 中的文件处于恢复挂起状态，而文件组处于脱机状态。  
  
2.  文件组 `C`的联机还原。  
  
     文件组 `C` 处于一致状态，这是因为，尽管数据库已通过还原及时恢复，但上面还原的部分备份是在文件组 `C` 成为只读文件组后进行的。 数据库管理员将恢复文件组 `C`（但不会还原该文件组）以使其联机。  
  
    ```  
    RESTORE DATABASE adb FILEGROUP='C' WITH RECOVERY  
    ```  
  
     此时，主文件组和文件组 `A` 以及 `C` 处于联机状态。 文件组 B 中的文件将保持恢复挂起状态，而该文件组处于脱机状态。  
  
3.  文件组 的联机还原。 `B.`  
  
     必须还原文件组 `B` 中的文件。 数据库管理员在文件组 `B` 变为只读文件组之后，且在进行部分备份之前还原文件组 `B` 的备份。  
  
    ```  
    RESTORE DATABASE adb FILEGROUP='B' FROM backup   
    WITH RECOVERY  
    ```  
  
     所有文件组现在都处于联机状态。  
  
## <a name="additional-examples"></a>其他示例  
  
-   [示例：数据库的段落还原（简单恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [示例：只读文件的联机还原（简单恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [示例：数据库的段落还原（完整恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [示例：仅对某些文件组进行段落还原（完整恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [示例：读/写文件的联机还原（完整恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)  
  
-   [示例：只读文件的联机还原（完整恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
## <a name="see-also"></a>另请参阅  
 [联机还原 (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
  
