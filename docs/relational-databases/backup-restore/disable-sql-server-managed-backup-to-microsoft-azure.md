---
title: 禁用 Azure blob 存储托管备份
description: 本文演示如何使用 Transact-SQL 在数据库和实例级别禁用或暂停目标为 Microsoft Azure 的 SQL Server 托管备份。
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 3e02187f-363f-4e69-a82f-583953592544
author: cawrites
ms.author: chadam
ms.openlocfilehash: 30450a1ea8f6304e02ef2b544be31d08f69b51fd
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129213"
---
# <a name="disable-sql-server-managed-backup-to-microsoft-azure"></a>对 Microsoft Azure 禁用 SQL Server 托管备份
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍了如何在数据库级别和实例级别禁用或暂停 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 。  
  
##  <a name="disable-ss_smartbackup-for-a-database"></a><a name="DatabaseDisable"></a> 为数据库禁用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]  
 通过使用系统存储过程 [managed_backup.sp_backup_config_basic (Transact SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md)，可以禁用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 设置。 \@enable_backup 参数用于启用和禁用特定数据库的 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 配置；其中，1 表示启用配置设置，0 表示禁用配置设置  。  
  
#### <a name="to-disable-ss_smartbackup-for-a-specific-database"></a>为特定数据库禁用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] ：  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
```sql
EXEC msdb.managed_backup.sp_backup_config_basic  
                @database_name = 'TestDB'   
                ,@enable_backup = 0;  
GO
```

> [!NOTE]
> 可能还需要根据自己的配置设置 `@container_url` 参数。
  
##  <a name="disable-ss_smartbackup-for-all-the-databases-on-the-instance"></a><a name="DatabaseAllDisable"></a> 为实例上的所有数据库禁用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]  
 下面的过程适用于要从实例上当前启用了 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 的所有数据库禁用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 配置设置的情形。  存储 URL、保持和 SQL 凭据之类的配置设置将在元数据中保留；并且如果以后为数据库启用了 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] ，则可以使用这些设置。 如果你要暂停 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 服务，则可以使用在本主题后面部分中说明的主开关。  
  
#### <a name="to-disable-ss_smartbackup-for-all-the-databases"></a>为所有数据库禁用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] ：  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。 下面的示例确定是否在实例级别配置 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] ，在实例上启用了所有 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] ，并且执行系统存储过程 **sp_backup_config_basic** 以便禁用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]。  
  
```sql
-- Create a working table to store the database names  
Declare @DBNames TABLE  
  
       (  
             RowID int IDENTITY PRIMARY KEY  
             ,DBName varchar(500)  
  
       )  
-- Define the variables  
DECLARE @rowid int  
DECLARE @dbname varchar(500)  
DECLARE @SQL varchar(2000)  
-- Get the database names from the system function  
  
INSERT INTO @DBNames (DBName)  
  
SELECT db_name  
       FROM   
  
       msdb.managed_backup.fn_backup_db_config (NULL)  
       WHERE is_managed_backup_enabled = 1 
       AND is_dropped = 0
  
       --Select DBName from @DBNames  
  
       select @rowid = min(RowID)  
       FROM @DBNames  
  
       WHILE @rowID IS NOT NULL  
       Begin  
  
             Set @dbname = (Select DBName From @DBNames Where RowID = @rowid)  
             Begin  
             Set @SQL = 'EXEC msdb.managed_backup.sp_backup_config_basic    
                @database_name= '''+'' + @dbname+ ''+''',   
                @enable_backup=0'  
  
            EXECUTE (@SQL)  
  
             END  
             Select @rowid = min(RowID)  
             From @DBNames Where RowID > @rowid  
  
       END  
```  
  
 若要查看实例上所有数据库的配置设置，请使用以下查询：  
  
```sql
Use msdb;  
GO  
SELECT * FROM managed_backup.fn_backup_db_config (NULL);  
GO  
```  
  
##  <a name="disable-default-ss_smartbackup-settings-for-the-instance"></a><a name="InstanceDisable"></a> 禁用实例的默认 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 设置  
 实例级别的默认设置适用于在该实例上创建的所有新数据库。  如果不再需要默认设置，则可以通过使用 managed_backup.sp_backup_config_basic 系统存储过程（将 \@database_name 参数设置为 NULL），禁用此配置   。 禁用并不会删除存储 URL、保持设置或 SQL 凭据名称之类的其他配置设置。 如果以后为该实例启用了 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] ，将使用这些设置。  
  
#### <a name="to-disable-ss_smartbackup-default-configuration-settings"></a>禁用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 默认配置设置：  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```sql
    EXEC msdb.managed_backup.sp_backup_config_basic  
                    @enable_backup = 0;  
    GO
    ```  
  
##  <a name="pause-ss_smartbackup-at-the-instance-level"></a><a name="InstancePause"></a> 在实例级别暂停 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]  
 有时可能需要在短期内临时暂停 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 服务。  **managed_backup.sp_backup_master_switch** 系统存储过程允许在实例级别禁用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 服务。  使用相同的存储过程恢复 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]。 \@state 参数用于定义是应禁用还是启用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]。  
  
#### <a name="to-pause-ss_smartbackup-services-using-transact-sql"></a>使用 Transact-SQL 暂停 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 服务：  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击 **“执行”** 。  
  
```sql
Use msdb;  
GO  
EXEC managed_backup.sp_backup_master_switch @new_state=0;  
Go
```  
  
#### <a name="to-resume-ss_smartbackup-using-transact-sql"></a>使用 Transact-SQL 恢复 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] ：  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击 **“执行”** 。  
  
```sql
Use msdb;  
Go  
EXEC managed_backup.sp_backup_master_switch @new_state=1;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [对 Microsoft Azure 启用 SQL Server 托管备份](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md)  
  
  
