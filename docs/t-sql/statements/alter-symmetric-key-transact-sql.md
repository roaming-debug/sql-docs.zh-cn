---
description: ALTER SYMMETRIC KEY (Transact-SQL)
title: ALTER SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER SYMMETRIC KEY
- ALTER_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- modifying symmetric keys
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], modifying
- cryptography [SQL Server], symmetric keys
- ALTER SYMMETRIC KEY statement
ms.assetid: d3c776a4-7d71-4e6f-84fc-1db47400c465
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a5635a7a0cc1d4c90ea70cf5973f0fc09b938f63
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124197"
---
# <a name="alter-symmetric-key-transact-sql"></a>ALTER SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  更改对称密钥的属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER SYMMETRIC KEY Key_name <alter_option>  
  
<alter_option> ::=  
   ADD ENCRYPTION BY <encrypting_mechanism> [ , ... n ]  
   |   
   DROP ENCRYPTION BY <encrypting_mechanism> [ , ... n ]  
<encrypting_mechanism> ::=  
   CERTIFICATE certificate_name  
   |  
   PASSWORD = 'password'  
   |  
   SYMMETRIC KEY Symmetric_Key_Name  
   |  
   ASYMMETRIC KEY Asym_Key_Name  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 Key_name  
 要更改的对称密钥在数据库中所使用的名称。  
  
 ADD ENCRYPTION BY  
 使用指定的方法添加加密。  
  
 DROP ENCRYPTION BY  
 通过指定的方法删除加密。 您不能从对称密钥中删除所有的加密。  
  
 CERTIFICATE Certificate_name  
 指定用于对对称密钥进行加密的证书。 该证书必须已存在于数据库中。  
  
 PASSWORD ='password'  
 指定用于对对称密钥进行加密的密码。 password 必须符合运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机的 Windows 密码策略要求。  
  
 SYMMETRIC KEY Symmetric_Key_Name  
 指定用于对要更改的对称密钥进行加密的对称密钥。 该对称密钥必须已存在于数据库中，并且必须打开。  
  
 ASYMMETRIC KEY Asym_Key_Name  
 指定用于对要更改的对称密钥进行加密的非对称密钥。 此非对称密钥必须已经存在于数据库中。  
  
## <a name="remarks"></a>备注  
  
> [!CAUTION]  
>  当使用密码（而不是数据库主密钥的公钥）对对称密钥进行加密时，便会使用 TRIPLE_DES 加密算法。 因此，用强加密算法（如 AES）创建的密钥本身受较弱算法的保护。  
  
 若要更改对称密钥的加密，请使用 ADD ENCRYPTION 和 DROP ENCRYPTION 短语。 密钥始终不可能完全不进行加密。 因此，最佳实践是在删除旧加密格式之前添加新的加密格式。  
  
 若要更改对称密钥的所有者，请使用 [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md)。  
  
> [!NOTE]  
>  RC4 算法仅用于支持向后兼容性。 仅当数据库兼容级别为 90 或 100 时，才能使用 RC4 或 RC4_128 对新材料进行加密。 （建议不要使用。）而是使用一种较新的算法，如 AES 算法之一。 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中，可以通过任何兼容级别对使用 RC4 或 RC4_128 加密的材料进行解密。  
  
## <a name="permissions"></a>权限  
 要求对对称密钥具有 ALTER 权限。 如果使用证书或非对称密钥添加加密，则要求对证书或非对称密钥具有 VIEW DEFINITION 权限。 如果使用证书或非对称密钥删除加密，则要求对证书或非对称密钥具有 CONTROL 权限。  
  
## <a name="examples"></a>示例  
 以下示例更改用于保护对称密钥的加密方法。 当创建对称密钥 `JanainaKey043` 时，使用证书 `Shipping04` 对该密钥进行加密。 由于密钥始终不可能在不加密的情况下进行存储，因此在本例中，首先使用密码添加加密，然后使用证书删除加密。  
  
```sql  
CREATE SYMMETRIC KEY JanainaKey043 WITH ALGORITHM = AES_256   
    ENCRYPTION BY CERTIFICATE Shipping04;  
-- Open the key.   
OPEN SYMMETRIC KEY JanainaKey043 DECRYPTION BY CERTIFICATE Shipping04  
    WITH PASSWORD = '<enterStrongPasswordHere>';   
-- First, encrypt the key with a password.  
ALTER SYMMETRIC KEY JanainaKey043   
    ADD ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>';  
-- Now remove encryption by the certificate.  
ALTER SYMMETRIC KEY JanainaKey043   
    DROP ENCRYPTION BY CERTIFICATE Shipping04;  
CLOSE SYMMETRIC KEY JanainaKey043;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [OPEN SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [CLOSE SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
