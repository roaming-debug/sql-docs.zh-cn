---
description: DECRYPTBYKEYAUTOCERT (Transact-SQL)
title: DECRYPTBYKEYAUTOCERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2015
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECRYPTBYKEYAUTOCERT
- DECRYPTBYKEYAUTOCERT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DECRYPTBYKEYAUTOCERT function
ms.assetid: 6b45fa2e-ffaa-46f7-86ff-5624596eda4a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 11cc7a88ad012554fac63b08030e31686ef1cc8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207205"
---
# <a name="decryptbykeyautocert-transact-sql"></a>DECRYPTBYKEYAUTOCERT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

此函数使用对称密钥解密数据。 该对称密钥使用证书自动解密。  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DecryptByKeyAutoCert ( cert_ID , cert_password   
    , { 'ciphertext' | @ciphertext }  
  [ , { add_authenticator | @add_authenticator }   
  [ , { authenticator | @authenticator } ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 cert_ID  
用于保护对称密钥的证书的 ID。 cert_ID 具有 int 数据类型。  
  
cert_password  
用于加密证书私钥的密码。 如果数据库主密钥保护私钥，则可能具有 `NULL` 值。 cert_password 具有 nvarchar 数据类型。  

'ciphertext'  
使用密钥加密的数据字符串。 ciphertext 具有 varbinary 数据类型。  

@ciphertext  
varbinary 类型的变量，包含使用密钥加密的数据。  

add_authenticator  
指示原始加密过程是否包含验证器和纯文本以及是否对其进行加密。 必须与数据加密过程中传递给 [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) 的值相匹配。 如果加密过程使用验证器，则 add_authenticator 具有 1 值。 add_authenticator 具有 int 数据类型。  
  
@add_authenticator  
变量，指示原始加密过程是否包含验证器和纯文本以及是否对其进行加密。 必须与数据加密过程中传递给 [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) 的值相匹配。 *\@add_authenticator* 具有 **int** 数据类型。  
  
authenticator  
用作验证器生成基础的数据。 必须与提供给 [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) 的值相匹配。 authenticator 具有 sysname 数据类型。  
  
@authenticator  
包含验证器生成所源自的数据的变量。 必须与提供给 [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) 的值相匹配。 *\@authenticator* 具有 **sysname** 数据类型。  
  
## <a name="return-types"></a>返回类型  
varbinary（最大大小为 8,000 个字节）。  
  
## <a name="remarks"></a>备注  
`DECRYPTBYKEYAUTOCERT` 合并了 `OPEN SYMMETRIC KEY` 和 `DECRYPTBYKEY` 的功能。 在单个操作中，它首先解密对称密钥，然后使用该密钥解密已加密的 ciphertext。  
  
## <a name="permissions"></a>权限  
需要对对称密钥拥有 `VIEW DEFINITION` 权限以及对证书拥有 `CONTROL` 权限。   
  
## <a name="examples"></a>示例  
此示例演示 `DECRYPTBYKEYAUTOCERT` 如何简化解密代码。 应在还没有数据库主密钥的 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库上运行此代码。  
  
```sql  
--Create the keys and certificate.  
USE AdventureWorks2012;  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'mzkvdlk979438teag$$ds987yghn)(*&4fdg^';  
OPEN MASTER KEY DECRYPTION BY PASSWORD = 'mzkvdlk979438teag$$ds987yghn)(*&4fdg^';  
CREATE CERTIFICATE HumanResources037   
   WITH SUBJECT = 'Sammamish HR',   
   EXPIRY_DATE = '10/31/2009';  
CREATE SYMMETRIC KEY SSN_Key_01 WITH ALGORITHM = DES  
    ENCRYPTION BY CERTIFICATE HumanResources037;  
GO  
----Add a column of encrypted data.  
ALTER TABLE HumanResources.Employee  
    ADD EncryptedNationalIDNumber varbinary(128);   
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037 ;  
UPDATE HumanResources.Employee  
SET EncryptedNationalIDNumber  
    = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
GO  
--  
--Close the key used to encrypt the data.  
CLOSE SYMMETRIC KEY SSN_Key_01;  
--  
--There are two ways to decrypt the stored data.  
--  
--OPTION ONE, using DecryptByKey()  
--1. Open the symmetric key  
--2. Decrypt the data  
--3. Close the symmetric key  
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037;  
SELECT NationalIDNumber, EncryptedNationalIDNumber    
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
CLOSE SYMMETRIC KEY SSN_Key_01;  
--  
--OPTION TWO, using DecryptByKeyAutoCert()  
SELECT NationalIDNumber, EncryptedNationalIDNumber   
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKeyAutoCert ( cert_ID('HumanResources037') , NULL ,EncryptedNationalIDNumber))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
```  
  
## <a name="see-also"></a>另请参阅  
 [OPEN SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [ENCRYPTBYKEY (Transact-SQL)](../../t-sql/functions/encryptbykey-transact-sql.md)   
 [DECRYPTBYKEY (Transact-SQL)](../../t-sql/functions/decryptbykey-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
