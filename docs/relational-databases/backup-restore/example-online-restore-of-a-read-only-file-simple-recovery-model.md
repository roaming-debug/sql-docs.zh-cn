---
title: 联机还原：只读文件（简单恢复模式）
description: 此示例演示对于使用简单恢复模式以及多个文件组的数据库，如何在 SQL Server 中对只读文件执行联机还原。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restore sequences [SQL Server], online
- online restores [SQL Server], simple recovery model
- simple recovery model [SQL Server], RESTORE examples
ms.assetid: 0c691fc6-9865-46a7-b055-8097424492d6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 58c4448ecde5bfed478ad19aa961dc74c1db59a5
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130429"
---
# <a name="example-online-restore-of-a-read-only-file-simple-recovery-model"></a>示例：只读文件的联机还原（简单恢复模式）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题针对采用简单恢复模式并包含只读文件组的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。 在简单恢复模式下，如果某个文件在最后一次变为只读时进行了备份，则可以联机还原该只读文件。  
  
 在此示例中，名为 `adb` 的数据库包含三个文件组。 文件组 `A` 为读/写文件组，而文件组 `B` 和 `C` 是只读的。 最初，所有文件组都处于联机状态。 现在必须还原文件组 `B`中的只读文件 `b1`。 数据库管理员可以使用在该文件变为只读状态之后获取的备份对其进行还原。 在还原过程中，文件组 `B` 将处于脱机状态，但是数据库的其余文件组仍处于联机状态。  
  
## <a name="restore-sequence"></a>还原顺序  
  
> [!NOTE]  
>  联机还原顺序的语法与脱机还原顺序的语法完全相同。  
  
 若要还原文件，数据库管理员应使用以下还原顺序：  
  
```  
RESTORE DATABASE adb FILE='b1' FROM filegroup_B_backup   
WITH RECOVERY  
```  
  
 文件现在处于联机状态。  
  
## <a name="additional-examples"></a>其他示例  
  
-   [示例：数据库的段落还原（简单恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [示例：仅对某些文件组进行段落还原（简单恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
-   [示例：数据库的段落还原（完整恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [示例：仅对某些文件组进行段落还原（完整恢复模式）](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [示例：读/写文件的联机还原（完整恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)  
  
-   [示例：只读文件的联机还原（完整恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
## <a name="see-also"></a>另请参阅  
 [联机还原 (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [文件还原（简单恢复模式）](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
