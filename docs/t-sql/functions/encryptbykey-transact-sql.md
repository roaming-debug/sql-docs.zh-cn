---
description: ENCRYPTBYKEY (Transact-SQL)
title: ENCRYPTBYKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ENCRYPTBYKEY_TSQL
- ENCRYPTBYKEY
dev_langs:
- TSQL
helpviewer_keywords:
- authenticators [SQL Server]
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], ENCRYPTBYKEY function
- ENCRYPTBYKEY function
ms.assetid: 0e11f8c5-f79d-46c1-ab11-b68ef05d6787
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b9d1a8d6e4fcc765ac0638e31e97ed8d9e7b685b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199550"
---
# <a name="encryptbykey-transact-sql"></a>ENCRYPTBYKEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  使用对称密钥加密数据。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
EncryptByKey ( key_GUID , { 'cleartext' | @cleartext }  
    [, { add_authenticator | @add_authenticator }  
     , { authenticator | @authenticator } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 key_GUID  
 用于加密 cleartext 的密钥的 GUID。 Uniqueidentifier。  
  
 'cleartext'  
 要使用密钥加密的数据。  
  
 @cleartext  
 类型为 nvarchar、char、varchar、binary、varbinary 或 nchar 的变量，其中包含要使用密钥加密的数据。  
  
 add_authenticator  
 指示是否将验证器与 cleartext 一起加密。 在使用验证器时必须为 1。 int。  
  
 @add_authenticator  
 指示是否将验证器与 cleartext 一起加密。 在使用验证器时必须为 1。 int。  
  
 authenticator  
 从中派生验证器的数据。 sysname。  
  
 @authenticator  
 包含用于派生验证器的数据的变量。  
  
## <a name="return-types"></a>返回类型  
 varbinary（最大大小为 8000 个字节）。  
  
 如果密钥未打开，如果密钥不存在，或者如果密钥是不推荐使用的 RC4 密钥且数据库不处于兼容性级别 110 或更高级别，则返回 NULL。  
 
 如果 cleartext 值为 NULL，则返回 NULL。
  
## <a name="remarks"></a>注解  
 EncryptByKey 使用对称密钥。 该密钥必须打开。 如果在当前会话中已打开该对称密钥，则无需在查询上下文中再次打开它。  
  
 验证器有助于禁止对加密字段进行整个值替换。 例如，请考虑下面的工资单数据表。  
  
|Employee_ID|Standard_Title|Base_Pay|  
|------------------|---------------------|---------------|  
|345|Copy Room Assistant|Fskj%7^edhn00|  
|697|Chief Financial Officer|M0x8900f56543|  
|694|Data Entry Supervisor|Cvc97824%^34f|  
  
 恶意用户无需进行解密即可根据存储密码文本的上下文推断出重要信息。 由于 Chief Financial Officer 的工资高于 Copy Room Assistant 的工资，因此 M0x8900f56543 的加密值必须大于 Fskj%7^edhn00 的加密值。 如果是这样，则对表具有 ALTER 权限的任何用户都能提高 Copy Room Assistant 的工资，方法是：使用 Chief Financial Officer 的 Base_Pay 字段中存储的数据的副本替换 Copy Room Assistant Base_Pay 字段中的数据。 这种整个值替换攻击可以完全绕过加密。  
  
 加密纯文本前在该文本中添加上下文信息，可避免上述整个值替换攻击。 此上下文信息用于验证未移动的纯文本数据。  
  
 如果加密数据时指定了验证器参数，则使用 DecryptByKey 对数据进行解密时，需要相同的验证器。 在加密时，验证器的哈希将与纯文本一起加密。 解密时，必须将同一验证器传递给 DecryptByKey。 如果这两个验证器不匹配，则解密会失败。 这指示该值在加密值之后已发生移动。 我们建议将包含唯一和未更改的值的列用作验证器。 如果验证器值发生更改，则可能无法访问此数据。  
  
 对称加密和解密速度相对较快，适于处理大量数据。  
  
> [!IMPORTANT]  
>  将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 加密函数与 ANSI_PADDING OFF 设置一起使用会因隐式转换而导致数据可能丢失。 有关 ANSI_PADDING 的详细信息，请参阅 [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)。  
  
## <a name="examples"></a>示例  
 以下示例所说明的功能依赖于在[如何加密数据列](../../relational-databases/security/encryption/encrypt-a-column-of-data.md)中创建的密钥和证书。  
  
### <a name="a-encrypting-a-string-with-a-symmetric-key"></a>A. 使用对称密钥加密字符串  
 以下示例向 `Employee` 表添加列，然后加密存储在 `NationalIDNumber` 列中的社会安全号码的值。  
  
```sql  
USE AdventureWorks2012;  
GO  
  
-- Create a column in which to store the encrypted data.  
ALTER TABLE HumanResources.Employee  
    ADD EncryptedNationalIDNumber varbinary(128);   
GO  
  
-- Open the symmetric key with which to encrypt the data.  
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037;  
  
-- Encrypt the value in column NationalIDNumber with symmetric key  
-- SSN_Key_01. Save the result in column EncryptedNationalIDNumber.  
UPDATE HumanResources.Employee  
SET EncryptedNationalIDNumber  
    = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
GO  
```  
  
### <a name="b-encrypting-a-record-together-with-an-authentication-value"></a>B. 将记录与身份验证值一起加密  
  
```sql 
USE AdventureWorks2012;  
  
-- Create a column in which to store the encrypted data.  
ALTER TABLE Sales.CreditCard   
    ADD CardNumber_Encrypted varbinary(128);   
GO  
  
-- Open the symmetric key with which to encrypt the data.  
OPEN SYMMETRIC KEY CreditCards_Key11  
    DECRYPTION BY CERTIFICATE Sales09;  
  
-- Encrypt the value in column CardNumber with symmetric   
-- key CreditCards_Key11.  
-- Save the result in column CardNumber_Encrypted.    
UPDATE Sales.CreditCard  
SET CardNumber_Encrypted = EncryptByKey(Key_GUID('CreditCards_Key11'),   
    CardNumber, 1, CONVERT( varbinary, CreditCardID) );  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [DECRYPTBYKEY (Transact-SQL)](../../t-sql/functions/decryptbykey-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [HASHBYTES (Transact-SQL)](../../t-sql/functions/hashbytes-transact-sql.md)  
  
  
