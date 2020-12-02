---
description: DECRYPTBYKEYAUTOASYMKEY (Transact-SQL)
title: DECRYPTBYKEYAUTOASYMKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2015
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DECRYPTBYKEYAUTOASYMKEY_TSQL
- DECRYPTBYKEYAUTOASYMKEY
dev_langs:
- TSQL
helpviewer_keywords:
- DECRYPTBYKEYAUTOASYMSKEY function
ms.assetid: 5521d4cf-740c-4ede-98b6-4ba90b84e32d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 4ac2905d7466b8a2aa3e8a823ab25f89d98ae96f
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128515"
---
# <a name="decryptbykeyautoasymkey-transact-sql"></a>DECRYPTBYKEYAUTOASYMKEY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

此函数对已加密的数据进行解密。 为此，该函数首先使用单独的非对称密钥解密对称密钥，然后使用在第一个“步骤”中提取的对称密钥解密已加密的数据。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DecryptByKeyAutoAsymKey ( akey_ID , akey_password   
    , { 'ciphertext' | @ciphertext }  
  [ , { add_authenticator | @add_authenticator }   
  [ , { authenticator | @authenticator } ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 akey_ID  
用于加密对称密钥的非对称密钥的 ID。 akey_ID 具有 int 数据类型。  
  
 akey_password  
保护非对称密钥的密码。 如果数据库主密钥保护非对称私钥，则 akey_password 可具有 NULL 值。 akey_password 具有 nvarchar 数据类型。  
  
 ciphertext - 使用密钥进行加密的数据。 ciphertext 具有 varbinary 数据类型。  
  
 @ciphertext  
varbinary 类型的变量，包含使用对称密钥进行加密的数据。  
  
 add_authenticator  
指示原始加密过程是否包含验证器和纯文本以及是否对其进行加密。 必须与数据加密过程中传递给 [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) 的值相匹配。 如果加密过程使用验证器，则 add_authenticator 具有 1 值。 add_authenticator 具有 int 数据类型。  
  
 @add_authenticator  
变量，指示原始加密过程是否包含验证器和纯文本以及是否对其进行加密。 必须与数据加密过程中传递给 [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) 的值相匹配。 *\@add_authenticator* 具有 **int** 数据类型。
  
 authenticator  
用作验证器生成基础的数据。 必须与提供给 [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) 的值相匹配。 authenticator 具有 sysname 数据类型。  
  
 @authenticator  
包含验证器生成所源自的数据的变量。 必须与提供给 [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) 的值相匹配。 *\@authenticator* 具有 **sysname** 数据类型。  
  
@add_authenticator  
变量，指示原始加密过程是否包含验证器和纯文本以及是否对其进行加密。 必须与数据加密过程中传递给 [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) 的值相匹配。 *\@add_authenticator* 具有 **int** 数据类型。  

authenticator  
用作验证器生成基础的数据。 必须与提供给 [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) 的值相匹配。 authenticator 具有 sysname 数据类型。

@authenticator  
包含验证器生成所源自的数据的变量。 必须与提供给 [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) 的值相匹配。 *\@authenticator* 具有 **sysname** 数据类型。  

## <a name="return-types"></a>返回类型  
varbinary（最大大小为 8,000 个字节）。  
  
## <a name="remarks"></a>备注  
`DECRYPTBYKEYAUTOASYMKEY` 合并了 `OPEN SYMMETRIC KEY` 和 `DECRYPTBYKEY` 的功能。 在单个操作中，它首先解密对称密钥，然后使用该密钥解密已加密的 ciphertext。  
  
## <a name="permissions"></a>权限  
需要对对称密钥拥有 `VIEW DEFINITION` 权限以及对非对称密钥拥有 `CONTROL` 权限。  
  
## <a name="examples"></a>示例
此示例演示 `DECRYPTBYKEYAUTOASYMKEY` 如何简化解密代码。 应在还没有数据库主密钥的 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库上运行此代码。  

```sql  
--Create the keys and certificate.  
USE AdventureWorks2012;  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'mzkvdMlk979438teag$$ds987yghn)(*&4fdg^';  
OPEN MASTER KEY DECRYPTION BY PASSWORD = 'mzkvdMlk979438teag$$ds987yghn)(*&4fdg^';  
CREATE ASYMMETRIC KEY SSN_AKey   
    WITH ALGORITHM = RSA_2048 ;   
GO  
CREATE SYMMETRIC KEY SSN_Key_02 WITH ALGORITHM = DES  
    ENCRYPTION BY ASYMMETRIC KEY SSN_AKey;  
GO  
--  
--Add a column of encrypted data.  
ALTER TABLE HumanResources.Employee  
    ADD EncryptedNationalIDNumber2 varbinary(128);   
OPEN SYMMETRIC KEY SSN_Key_02  
   DECRYPTION BY ASYMMETRIC KEY SSN_AKey;  
UPDATE HumanResources.Employee  
SET EncryptedNationalIDNumber2  
    = EncryptByKey(Key_GUID('SSN_Key_02'), NationalIDNumber);  
GO  
--Close the key used to encrypt the data.  
CLOSE SYMMETRIC KEY SSN_Key_02;  
--  
--There are two ways to decrypt the stored data.  
--  
--OPTION ONE, using DecryptByKey()  
--1. Open the symmetric key.  
--2. Decrypt the data.  
--3. Close the symmetric key.  
OPEN SYMMETRIC KEY SSN_Key_02  
   DECRYPTION BY ASYMMETRIC KEY SSN_AKey;  
SELECT NationalIDNumber, EncryptedNationalIDNumber2    
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber2))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
CLOSE SYMMETRIC KEY SSN_Key_02;  
--  
--OPTION TWO, using DecryptByKeyAutoAsymKey()  
SELECT NationalIDNumber, EncryptedNationalIDNumber2   
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKeyAutoAsymKey ( AsymKey_ID('SSN_AKey') , NULL ,EncryptedNationalIDNumber2))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [OPEN SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [ENCRYPTBYKEY (Transact-SQL)](../../t-sql/functions/encryptbykey-transact-sql.md)   
 [DECRYPTBYKEY (Transact-SQL)](../../t-sql/functions/decryptbykey-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
