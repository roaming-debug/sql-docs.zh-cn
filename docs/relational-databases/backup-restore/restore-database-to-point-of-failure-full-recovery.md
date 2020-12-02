---
title: 还原数据库：故障点 - 完整恢复
description: 本文说明对于使用完整恢复模式或大容量日志恢复模式的数据库，如何将 SQL Server 数据库还原到故障点。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- point of failure [SQL Server]
- restoring databases [SQL Server], point of failure
- database restores [SQL Server], point of failure
ms.assetid: 04106e18-bbf7-4a5e-a2e1-3d65319814d5
author: cawrites
ms.author: chadam
ms.openlocfilehash: a3a3bee6457f76311fa8ab1bc11afee6c222fe80
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125524"
---
# <a name="restore-database-to-point-of-failure---full-recovery"></a>将数据库还原到故障点 - 完整恢复
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何还原到故障点。 本主题仅与那些使用完整或大容量日志恢复模式的数据库相关。  
  
### <a name="to-restore-to-the-point-of-failure"></a>还原到故障点  
  
1.  通过运行以下基本 [BACKUP](../../t-sql/statements/backup-transact-sql.md) 语句来备份日志尾部：  
  
    ```  
    BACKUP LOG <database_name> TO <backup_device>   
       WITH NORECOVERY, NO_TRUNCATE;  
    ```  
  
2.  通过运行以下基本 [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md) 语句来还原完整数据库备份：  
  
    ```  
    RESTORE DATABASE <database_name> FROM <backup_device>   
       WITH NORECOVERY;  
    ```  
  
3.  或者，通过运行以下基本 RESTORE DATABASE 语句来还原差异数据库备份：  
  
    ```  
    RESTORE DATABASE <database_name> FROM <backup_device>   
       WITH NORECOVERY;  
    ```  
  
4.  通过在 RESTORE LOG 语句中指定 WITH NORECOVERY 以应用每个事务日志（包括步骤 1 中创建的结尾日志备份）：  
  
    ```  
    RESTORE LOG <database_name> FROM <backup_device>   
       WITH NORECOVERY;  
    ```  
  
5.  通过运行以下 RESTORE DATABASE 语句来恢复数据库：  

    ```  
    RESTORE DATABASE <database_name>   
       WITH RECOVERY;  
    ```  
  
## <a name="example"></a>示例  
 必须先完成下列准备工作，才能运行此示例：  
  
1.  [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的默认恢复模式是简单恢复模式。 由于该恢复模式不支持还原到故障点，因此请将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 设置为使用完整恢复模式，方法是运行以下 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) 语句：  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
    ```  
  
2.  通过使用以下 BACKUP 语句，创建数据库的完整数据库备份：  
  
    ```  
    BACKUP DATABASE AdventureWorks2012 TO DISK = 'C:\AdventureWorks2012_Data.bck';  
    ```  
  
3.  创建例程日志备份：  
  
    ```  
    BACKUP LOG AdventureWorks2012 TO DISK = 'C:\AdventureWorks2012_Log.bck';  
    ```  
  
 以下示例在创建 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的结尾日志备份后，将还原先前创建的备份。 （此步骤假设可以访问日志磁盘。）  
  
 首先，该示例将创建捕获活动日志的数据库结尾日志备份，并使数据库处于还原状态。 然后，该示例将还原数据库备份，应用先前创建的例程日志备份，并应用结尾日志备份。 最后，该示例将在单独的步骤中恢复数据库。  
  
> [!NOTE]  
>  默认行为是将数据库恢复作为还原最终备份语句的一部分。  
  
```  
/* Example of restoring a to the point of failure */  
-- Step 1: Create a tail-log backup by using WITH NORECOVERY.  
BACKUP LOG AdventureWorks2012  
   TO DISK = 'C:\AdventureWorks2012_Log.bck'  
   WITH NORECOVERY;  
GO  
-- Step 2: Restore the full database backup.  
RESTORE DATABASE AdventureWorks2012  
   FROM DISK = 'C:\AdventureWorks2012_Data.bck'  
   WITH NORECOVERY;  
GO  
-- Step 3: Restore the first transaction log backup.  
RESTORE LOG AdventureWorks2012  
   FROM DISK = 'C:\AdventureWorks2012_Log.bck'  
   WITH NORECOVERY;  
GO  
-- Step 4: Restore the tail-log backup.  
RESTORE LOG AdventureWorks2012  
   FROM  DISK = 'C:\AdventureWorks2012_Log.bck'  
   WITH NORECOVERY;  
GO  
-- Step 5: Recover the database.  
RESTORE DATABASE AdventureWorks2012  
   WITH RECOVERY;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
