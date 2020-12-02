---
title: 托管备份 - 配置高级选项
description: 本教程介绍如何在默认选项不满足需求的情况下，为目标为 Microsoft Azure 的 SQL Server 托管备份设置高级选项。
titleSuffix: to Microsoft Azure
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.managedbackup.configure.f1
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.assetid: ffd28159-8de8-4d40-87da-1586bfef3315
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.openlocfilehash: 3f59707810a698f59f73443066d6e551fbcb3e41
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130478"
---
# <a name="configure-advanced-options-for-sql-server-managed-backup-to-microsoft-azure"></a>配置 Microsoft Azure SQL Server 托管备份的高级选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  以下教程介绍了如何设置 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]的高级选项。 仅当你需要提供的这些功能时，才需要这些步骤。 否则，可以启用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 并依赖于默认行为。  
  
 在每个方案中，使用 `database_name` 参数指定备份。 当 `database_name` 为 NULL 或 * 时，这些更改会影响实例级别的默认设置。 实例级别的设置也会影响在更改后创建的新数据库。  
  
 指定这些设置后，便可以使用系统存储过程 [managed_backup.sp_backup_config_basic (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md) 为数据库或实例启用托管备份。 有关详细信息，请参阅[启用 Microsoft Azure SQL Server 托管备份](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md)。  
  
> [!WARNING]  
>  在使用 [managed_backup.sp_backup_config_basic (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md) 启用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 之前，应该始终配置高级选项和自定义计划选项。 否则，在启用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 和配置这些设置之间的这一时间段中，可能会发生不需要的备份操作。  
  
## <a name="configure-encryption"></a>配置加密  
 以下步骤说明了如何使用存储过程 [managed_backup.sp_backup_config_advanced (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-advanced-transact-sql.md) 来指定加密设置。  

1.  **确定加密算法：** 首先，确定要使用的加密算法的名称。 选择以下算法之一：  
  
    -   AES_128  
  
    -   AES_192  
  
    -   AES_256  
  
    -   TRIPLE_DES_3KEY  
  
    -   NO_ENCRYPTION  
  
2.  **创建数据库主密钥：** 选择密码来对存储于该数据库中的主密钥副本进行加密。  
  
    ```  
    -- Creates a database master key.  
    -- The key is encrypted using the password "<master key password>"  
    USE Master;  
    GO  
       CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master key password>';  
    GO  
    ```  
  
3.  **创建备份证书或非对称密钥：** 可以将证书或非对称密钥用于加密。 以下示例创建了用于加密的备份证书。  
  
    ```sql  
    USE Master;  
    GO  
       CREATE CERTIFICATE MyTestDBBackupEncryptCert  
          WITH SUBJECT = 'MyTestDBBackupEncryptCert';  
    GO  
    ```  
  
4.  **设置托管备份加密：** 使用相应值调用 managed_backup.sp_backup_config_advanced  存储过程。 例如，以下示例使用名为 `MyDB` 的证书和 `MyTestDBBackupEncryptCert` 加密算法为加密配置 `AES_128` 数据库。  
  
    ```  
    USE msdb;  
    GO  
       EXEC managed_backup.sp_backup_config_advanced  
          @database_name = 'MyDB'                
          ,@encryption_algorithm ='AES_128'  
          ,@encryptor_type = 'CERTIFICATE'  
          ,@encryptor_name = 'MyTestDBBackupEncryptCert';  
    GO  
    ```  
  
    > [!WARNING]  
    >  如果在上一示例中 `@database_name` 为 NULL，这些设置则应用于 SQL Server 实例。  
  
## <a name="configure-a-custom-backup-schedule"></a>配置自定义备份计划  
 以下步骤说明了如何使用存储过程 [managed_backup.sp_backup_config_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md) 设置自定义计划。  
  
1.  **确定完整备份的频率：** 确定多久对数据库进行一次完整备份。 可以选择“每日”或“每周”完整备份。  
  
2.  **确定日志备份的频率：** 确定多久进行一次日志备份。 此值以分钟或小时为单位。  
  
3.  **确定在星期几进行每周备份：** 如果是每周备份一次，请选择在星期几进行完整备份。  
  
4.  **确定备份开始时间：** 选择备份开始时间（使用 24 小时制）。  
  
5.  **确定允许的备份时长：** 这指定必须达到的备份时长。  
  
6.  **设置自定义备份计划：** 下面的存储过程定义 `MyDB` 数据库的自定义计划。 完整备份将在每周 `Monday` 的 `17:30`进行。 每 `5` 分钟进行日志备份。 备份完成的时间为两个小时。  
  
    ```  
    USE msdb;  
    GO  
    EXEC managed_backup.sp_backup_config_schedule   
         @database_name =  'MyDB'  
        ,@scheduling_option = 'Custom'  
        ,@full_backup_freq_type = 'Weekly'  
        ,@days_of_week = 'Monday'  
        ,@backup_begin_time =  '17:30'  
        ,@backup_duration = '02:00'  
        ,@log_backup_freq = '00:05'  
    GO  
  
    ```  
  
## <a name="next-steps"></a>后续步骤  
 在配置高级选项和自定义计划后，必须在目标数据库或 SQL Server 实例上启用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 。 有关详细信息，请参阅 [Enable SQL Server Managed Backup to Microsoft Azure](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目标为 Microsoft Azure 的 SQL Server 托管备份](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)  
  
  
