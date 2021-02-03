---
description: DECRYPTBYPASSPHRASE (Transact-SQL)
title: DECRYPTBYPASSPHRASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECRYPTBYPASSPHRASE
- DECRYPTBYPASSPHRASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- decryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], DECRYPTBYPASSPHRASE function
- DECRYPTBYPASSPHRASE function
ms.assetid: ca34b5cd-07b3-4dca-b66a-ed8c6a826c95
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 81e6e954a597e3ea6b7e2b6e983e063c4a1bf8e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207241"
---
# <a name="decryptbypassphrase-transact-sql"></a>DECRYPTBYPASSPHRASE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函数可对最初使用密码加密的数据进行解密。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DecryptByPassPhrase ( { 'passphrase' | @passphrase }   
    , { 'ciphertext' | @ciphertext }  
  [ , { add_authenticator | @add_authenticator }  
    , { authenticator | @authenticator } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 passphrase  
用于生成解密密钥的密码。  
  
 @passphrase  
类型为 char、nchar、nvarchar 或 varchar 的变量，其中包含用于生成解密密钥的密码   。  
  
'ciphertext'  
使用密钥加密的数据字符串。 ciphertext 具有 varbinary 数据类型。  
 
@ciphertext  
varbinary 类型的变量，包含使用密钥加密的数据。 *\@ciphertext* 变量的最大大小为 8,000 字节。  
  
add_authenticator  
指示原始加密过程是否包含验证器和纯文本以及是否对其进行加密。 如果加密过程使用验证器，则 add_authenticator 具有 1 值。 add_authenticator 具有 int 数据类型。  
  
@add_authenticator  
变量，指示原始加密过程是否包含验证器和纯文本以及是否对其进行加密。 如果加密过程使用验证器，则 *\@add_authenticator* 具有值 1。 *\@add_authenticator* 具有 **int** 数据类型。  

authenticator  
用作验证器生成基础的数据。 authenticator 具有 sysname 数据类型。  
  
@authenticator  
包含用作验证器生成基础的数据的变量。 *\@authenticator* 具有 **sysname** 数据类型。  
  
## <a name="return-types"></a>返回类型  
varbinary（最大大小为 8,000 个字节）。  
  
## <a name="remarks"></a>备注  
`DECRYPTBYPASSPHRASE` 不需要执行权限。 如果 `DECRYPTBYPASSPHRASE` 收到错误的密码或错误的验证器信息，则返回 NULL。  
  
`DECRYPTBYPASSPHRASE` 使用密码生成解密密钥。 此解密密钥不会保留。  
  
如果在 ciphertext 加密时包含验证器，`DECRYPTBYPASSPHRASE` 必须接收该解密过程的同一验证器。 如果解密过程中提供的验证器值与最初用于加密数据的验证器值不匹配，则 `DECRYPTBYPASSPHRASE` 操作将失败。  
  
## <a name="examples"></a>示例  
此示例解密 [EncryptByPassPhrase](../../t-sql/functions/encryptbypassphrase-transact-sql.md) 中更新的记录。  
  
```sql  
USE AdventureWorks2012;  
-- Get the passphrase from the user.  
DECLARE @PassphraseEnteredByUser NVARCHAR(128);  
SET @PassphraseEnteredByUser   
= 'A little learning is a dangerous thing!';  
  
-- Decrypt the encrypted record.  
SELECT CardNumber, CardNumber_EncryptedbyPassphrase   
    AS 'Encrypted card number', CONVERT(varchar,  
    DecryptByPassphrase(@PassphraseEnteredByUser, CardNumber_EncryptedbyPassphrase, 1   
    , CONVERT(varbinary, CreditCardID)))  
    AS 'Decrypted card number' FROM Sales.CreditCard   
    WHERE CreditCardID = '3681';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [选择加密算法](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ENCRYPTBYPASSPHRASE (Transact-SQL)](../../t-sql/functions/encryptbypassphrase-transact-sql.md)  
  
  
