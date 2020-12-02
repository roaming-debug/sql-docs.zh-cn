---
description: RESTORE SERVICE MASTER KEY (Transact-SQL)
title: RESTORE SERVICE MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RESTORE SERVICE MASTER KEY
- RESTORE_SERVICE_MASTER_KEY_TSQL
- LOAD SERVICE MASTER KEY
- LOAD_SERVICE_MASTER_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- importing Service Master Keys
- copying Service Master Keys
- service master key [SQL Server], importing
- RESTORE SERVICE MASTER KEY statement
- transferring Service Master Keys
ms.assetid: a68fd0ee-70ce-4104-aca0-fcae5f41fc38
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 38193c05ecfa6c030954c6cbcbfcc50a1927d913
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92300492"
---
# <a name="restore-service-master-key-transact-sql"></a>RESTORE SERVICE MASTER KEY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从备份文件中导入服务主密钥。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
RESTORE SERVICE MASTER KEY FROM FILE = 'path_to_file'   
    DECRYPTION BY PASSWORD = 'password' [FORCE]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 FILE ='path\_to\_file'  
 指定存储服务主密钥的完整路径（包括文件名）。 path_to_file 可以是本地路径，也可以是网络位置的 UNC 路径。  
  
 PASSWORD ='password'  
 指定对从文件中导入的服务主密钥进行解密时所需的密码。  
  
 FORCE  
 即使存在数据丢失的风险，也要强制替换服务主密钥。  
  
## <a name="remarks"></a>注解  
 当还原服务主密钥时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将对所有已使用当前服务主密钥加密的密钥和机密内容进行解密，然后使用从备份文件中加载的服务主密钥对这些密钥和机密内容进行加密。  
  
 如果有任意一种解密操作失败，则还原操作将会失败。 您可以使用 FORCE 选项忽略错误，但是该选项会使无法进行解密的数据丢失。  
  
> [!CAUTION]  
>  服务主密钥为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 加密层次结构的根。 服务主密钥直接或间接地保护树中的所有其他密钥。 如果在强制的还原过程中不能对某个相关密钥进行解密，则由该密钥所保护的数据便会丢失。  
  
 重新生成加密层次结构是一种消耗大量资源的操作。 您应当将该操作安排在资源需求较低的时段进行。  
  
## <a name="permissions"></a>权限  
 需要对服务器的 CONTROL SERVER 权限。  
  
## <a name="examples"></a>示例  
 以下示例从备份文件中还原服务主密钥。  
  
```sql  
RESTORE SERVICE MASTER KEY   
    FROM FILE = 'c:\temp_backups\keys\service_master_key'   
    DECRYPTION BY PASSWORD = '3dH85Hhk003GHk2597gheij4';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [服务主密钥](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [ALTER SERVICE MASTER KEY (Transact-SQL)](../../t-sql/statements/alter-service-master-key-transact-sql.md)   
 [BACKUP SERVICE MASTER KEY (Transact-SQL)](../../t-sql/statements/backup-service-master-key-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)