---
description: CREATE DATABASE ENCRYPTION KEY (Transact-SQL)
title: CREATE DATABASE ENCRYPTION KEY (Transact-SQL)
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATABASE_ENCRYPTION_KEY_TSQL
- ENCRYPTION_KEY_TSQL
- sql13.swb.dbencryptionkeyo.f1
- ENCRYPTION KEY
- DATABASE ENCRYPTION KEY
- CREATE_DATABASE_ENCRYPTION_KEY_TSQL
- CREATE DATABASE ENCRYPTION KEY
- sql13.swb.dbencryptionkeyg.f1
- CREATE DATABASE ENCRYPTION
- CREATE_DATABASE_ENCRYPTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database encryption key
- CREATE DATABASE ENCRYPTION KEY statement
- database encryption key, create
ms.assetid: 2ee95a32-5140-41bd-9ab3-a947b9990688
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c6fa73ff61c166bed70b941c944a85d24cce8ec5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478548"
---
# <a name="create-database-encryption-key-transact-sql"></a>CREATE DATABASE ENCRYPTION KEY (Transact-SQL)

[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

 创建用于以透明方式加密数据库的加密密钥。 有关透明数据库加密的详细信息，请参阅[透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server  

CREATE DATABASE ENCRYPTION KEY  
       WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   ENCRYPTION BY SERVER   
    {  
        CERTIFICATE Encryptor_Name |  
        ASYMMETRIC KEY Encryptor_Name  
    }  
[ ; ]  
```  
  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  

CREATE DATABASE ENCRYPTION KEY  
       WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name   
[ ; ]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

WITH ALGORITHM = { AES_128 \| AES_192 \| AES_256 \| TRIPLE_DES_3KEY  }  
指定用于加密密钥的加密算法。

> [!WARNING]
> 从 SQL Server 2016 开始，除 AES_128、AES_192 和 AES_256 以外的所有算法都不再使用。 若要使用旧算法（不推荐），必须将数据库设置为兼容级别 120 或更低。  
  
ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name  
指定用于加密数据库加密密钥的加密程序的名称。  
  
ENCRYPTION BY SERVER ASYMMETRIC KEY Encryptor_Name  
指定用于加密数据库加密密钥的非对称密钥的名称。 要使用非对称密钥对数据库加密密钥进行加密，非对称密钥必须驻留在可扩展密钥管理提供程序上。  
  
## <a name="remarks"></a>备注  
在可使用“透明数据库加密”(TDE) 加密数据库之前，需要设置一个数据库加密密钥。 以透明方式加密数据库时，将在文件级别上加密整个数据库，而无需对代码进行特殊修改。 用于加密数据库加密密钥的证书或非对称密钥必须位于 master 系统数据库中。  
  
只允许对用户数据库使用数据库加密语句。  
  
数据库加密密钥不能从数据库中导出。 它只能供系统、对服务器拥有调试权限的用户以及能够访问证书（用于加密和解密数据库加密密钥）的用户使用。  
  
数据库所有者 (dbo) 发生更改时不必重新生成数据库加密密钥。  
  
系统会为 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 数据库自动创建一个数据库加密密钥。 用户无需使用 CREATE DATABASE ENCRYPTION KEY 语句创建密钥。  
  
## <a name="permissions"></a>权限  
需要数据库的 CONTROL 权限和用于加密数据库加密密钥的证书或非对称密钥的 VIEW DEFINITION 权限。  
  
## <a name="examples"></a>示例  
有关使用 TDE 的其他示例，请参阅[透明数据加密 &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)、[使用 EKM 在 SQL Server 上启用 TDE](../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md) 和[使用 Azure Key Vault 的可扩展密钥管理 &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)。  
  
下面的示例使用 `AES_256` 算法创建一个数据库加密密钥，并使用名为 `MyServerCert` 的证书保护私钥。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_256  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
[透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)   
[SQL Server 加密](../../relational-databases/security/encryption/sql-server-encryption.md)   
[SQL Server 和数据库加密密钥（数据库引擎）](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
[加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
[ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
[ALTER DATABASE ENCRYPTION KEY (Transact-SQL)](../../t-sql/statements/alter-database-encryption-key-transact-sql.md)   
[DROP DATABASE ENCRYPTION KEY (Transact-SQL)](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)   
[sys.dm_database_encryption_keys (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)  
    
