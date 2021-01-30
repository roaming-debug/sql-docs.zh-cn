---
description: " (transact-sql 的 managed_backup.sp_backup_config_advanced managed_backup) "
title: managed_backup managed_backup.sp_backup_config_advanced (transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_backup_config_optional
- sp_backup_config_optional_TSQL
- managed_backup.sp_backup_config_optional_TSQL
- managed_backup.sp_backup_config_optional
dev_langs:
- TSQL
helpviewer_keywords:
- sp_backup_config_optional
- managed_backup.sp_backup_config_optional
ms.assetid: 4fae8193-1f88-48fd-a94a-4786efe8d6af
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 77b56d757d685e30798c8763c948dbb72e89c2d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203725"
---
# <a name="managed_backupsp_backup_config_advanced-transact-sql"></a> (transact-sql 的 managed_backup.sp_backup_config_advanced managed_backup) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  为配置高级设置 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sql  
EXEC managed_backup.sp_backup_config_advanced   
    [@database_name = ] 'database_name'  
    ,[@encryption_algorithm = ] 'name of the encryption algorithm'  
    ,[@encryptor_type = ] {'CERTIFICATE' | 'ASYMMETRIC_KEY'}  
    ,[@encryptor_name = ] 'name of the certificate or asymmetric key'  
    ,[@local_cache_path = ] 'NOT AVAILABLE'  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> 参数  
 @database_name  
 用于在特定数据库上启用托管备份的数据库名称。 如果为 NULL 或 *，则此托管备份适用于服务器上的所有数据库。  
  
 @encryption_algorithm  
 备份过程中用于加密备份文件的加密算法的名称。 @encryption_algorithm为 **SYSNAME**。 在首次为数据库配置 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 时，它是必需的参数。 如果你不希望对备份文件进行加密，请指定 **NO_ENCRYPTION** 。 更改 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 配置设置时，此参数是可选的-如果未指定参数，则保留现有的配置值。 此参数的允许值为：  
  
-   AES_128  
  
-   AES_192  
  
-   AES_256  
  
-   TRIPLE_DES_3KEY  
  
-   NO_ENCRYPTION  
  
 有关加密算法的详细信息，请参阅 [Choose an Encryption Algorithm](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)。  
  
 @encryptor_type  
 加密程序的类型，可以是 "证书" 或 "ASYMMETRIC_KEY"。 @encryptor_type为 **nvarchar (32)**。 如果为参数指定 NO_ENCRYPTION，则此参数是可选的 @encryption_algorithm 。  
  
 @encryptor_name  
 要用于加密备份的现有证书或非对称密钥的名称。 @encryptor_name为 **SYSNAME**。 如果使用非对称密钥，则必须使用扩展密钥管理 (EKM) 进行配置。 如果为参数指定 NO_ENCRYPTION，则此参数是可选的 @encryption_algorithm 。  
  
 有关详细信息，请参阅 [&#40;EKM&#41;的可扩展密钥管理 ](../../relational-databases/security/encryption/extensible-key-management-ekm.md)。  
  
 @local_cache_path  
 此参数尚不受支持。  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求具有 **db_backupoperator** 数据库角色的成员身份，具有 **ALTER ANY CREDENTIAL** 权限以及对 **sp_delete_backuphistory** 存储过程的 **EXECUTE** 权限。  
  
## <a name="examples"></a>示例  
 下面的示例 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 为 SQL Server 实例设置的高级配置选项。  
  
```sql
Use msdb;  
Go  
   EXEC managed_backup.sp_backup_config_advanced  
                @encryption_algorithm ='AES_128'  
                ,@encryptor_type = 'CERTIFICATE'  
                ,@encryptor_name = 'MyTestDBBackupEncryptCert'  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ (transact-sql 的 managed_backup.sp_backup_config_basic managed_backup) ](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md)   
 [managed_backup.sp_backup_config_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md)  
  
  
