---
description: " (transact-sql 的 managed_backup.fn_backup_db_config managed_backup) "
title: managed_backup managed_backup.fn_backup_db_config (transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- smart_admin.fn_backup_db_config
- smart_admin.fn_backup_db_config_TSQL
- fn_backup_db_config
- fn_backup_db_config_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_backup_db_config
- fn_backup_db_config
ms.assetid: 7c755d8a-64dd-44b2-be5e-735d30758900
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 01d2ecc1fffc66d92508b71951ca05d33a224cff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207423"
---
# <a name="managed_backupfn_backup_db_config-transact-sql"></a> (transact-sql 的 managed_backup.fn_backup_db_config managed_backup) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

   返回 0 个、1 个或多个带有 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 配置设置的行。 返回指定数据库的 1 行，或返回实例上配置了 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 的所有数据库的信息。  
  
 使用此存储过程可查看或确定 SQL Server 实例上一个数据库或所有数据库的当前 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 配置设置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sql  
managed_backup.fn_backup_db_config ('database_name' | '' | NULL)  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> 参数  
 @db_name  
 数据库的名称。 @db_name参数为 **SYSNAME**。 如果传递给此参数的是空字符串或 Null 值，则将返回 SQL Server 实例上所有数据库的有关信息。  
  
## <a name="table-returned"></a>返回的表  
  
|列名|数据类型|说明|  
|-----------------|---------------|-----------------|  
|db_name|SYSNAME|数据库名称。|  
|db_guid|UNIQUEIDENTIFIER|唯一标识数据库的标识符。|  
|is_availability_database|BIT|此数据库是否参与可用性组。 值 1 指示数据库是可用性数据库，0 指示不是。|  
|is_dropped|BIT|值 1 指示此数据库为已删除数据库。|  
|credential_name|SYSNAME|用于对存储帐户进行身份验证的 SQL 凭据的名称。 NULL 值指示未设置 SQL 凭据。|  
|retention_days|INT|当前的保留期（以天为单位）。 NULL 值指示从未为此数据库配置 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]。|  
|is_managed_backup_enabled|INT|指示当前是否为此数据库启用了 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]。 值 1 指示 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 当前已启用，值 0 指示已为此数据库禁用了 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]。|  
|storage_url|NVARCHAR (1024) |存储帐户的 URL。|  
|Encryption_algorithm|NCHAR (20) |返回当前要在加密备份时使用的加密算法。|  
|Encryptor_type|NCHAR(15)|返回加密程序设置：证书或非对称密钥。|  
|Encryptor_name|NCHAR(max_length_of_cert/asymm_key_name)|证书或非对称密钥的名称。|  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求具有 **ALTER ANY CREDENTIAL** 权限的 **db_backupoperator** 数据库角色的成员身份。 不应拒绝用户 **查看任何定义** 权限。  
  
## <a name="examples"></a>示例  
 下面的示例返回 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] "TestDB" 的配置  
  
 对于每个代码段，请在语言属性字段中选择“tsql”。  
  
```  
Use msdb  
GO  
SELECT * FROM managed_backup.fn_backup_db_config('TestDB')  
```  
  
 以下示例会返回对其执行此存储过程的 SQL Server 实例上，所有数据库的 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 配置。  
  
```  
Use msdb  
GO  
SELECT * FROM managed_backup.fn_backup_db_config (NULL)  
```  
  
  
