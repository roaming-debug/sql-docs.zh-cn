---
description: KEY_NAME (Transact-SQL)
title: KEY_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- KEY_NAME_TSQL
- KEY_NAME
dev_langs:
- TSQL
helpviewer_keywords:
- KEY_NAME function
ms.assetid: 7b693e5d-2325-4bf9-9b45-ad6a23374b41
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 857dff1b1ddb9e732cb065560d922fb5efbd6559
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183962"
---
# <a name="key_name-transact-sql"></a>KEY_NAME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  从对称密钥 GUID 或密码文本返回对称密钥的名称。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
KEY_NAME ( ciphertext | key_guid )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 ciphertext  
 对称密钥加密的文本。 cyphertext 是 varbinary(8000) 类型。  
  
 key_guid  
 对称密钥的 GUID。 key_guid 是 uniqueidentifier 类型。  
  
## <a name="returned-types"></a>返回类型  
 **varchar(128)**  
  
## <a name="permissions"></a>权限  
 从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 开始，元数据的可见性仅限用户所拥有的安全对象，或用户已获得某些权限的安全对象。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-displaying-the-name-of-a-symmetric-key-using-the-key_guid"></a>A. 使用 key_guid 显示对称密钥的名称  
 master 数据库包含一个名为 ##MS_ServiceMasterKey## 的对称密钥。 下面的示例从 sys.symmetric_keys 动态管理视图获取该密钥的 GUID，并将其赋值给一个变量，然后将该变量传递到 KEY_NAME 函数，从而演示如何返回与 GUID 对应的名称。  
  
```sql  
USE master;  
GO  
DECLARE @guid uniqueidentifier ;  
SELECT @guid = key_guid FROM sys.symmetric_keys  
WHERE name = '##MS_ServiceMasterKey##' ;  
-- Demonstration of passing a GUID to KEY_NAME to receive a name  
SELECT KEY_NAME(@guid) AS [Name of Key];  
```  
  
### <a name="b-displaying-the-name-of-a-symmetric-key-using-the-cipher-text"></a>B. 使用密码文本显示对称密钥的名称  
 下面的示例对创建一个对称密钥并将数据填充到表的整个过程进行了说明。 然后，该示例显示 KEY_NAME 如何在传递加密文本后返回密钥的名称。  
  
```sql 
-- Create a symmetric key  
CREATE SYMMETRIC KEY TestSymKey   
   WITH ALGORITHM = AES_128,  
   KEY_SOURCE = 'The square of the hypotenuse is equal to the sum of the squares of the sides',  
   IDENTITY_VALUE = 'Pythagoras'  
   ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y' ;  
GO  
-- Create a table for the demonstration  
CREATE TABLE DemoKey  
(IDCol INT IDENTITY PRIMARY KEY,  
SecretCol VARBINARY(256) NOT NULL)  
GO  
-- Open the symmetric key if not already open  
OPEN SYMMETRIC KEY TestSymKey   
    DECRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y';  
GO  
-- Insert a row into the DemoKey table  
DECLARE @key_GUID uniqueidentifier;  
SELECT @key_GUID = key_guid FROM sys.symmetric_keys  
WHERE name LIKE 'TestSymKey' ;  
INSERT INTO DemoKey(SecretCol)  
VALUES ( ENCRYPTBYKEY (@key_GUID, 'EncryptedText'))  
GO  
-- Verify the DemoKey data  
SELECT * FROM DemoKey;  
GO  
-- Decrypt the data  
DECLARE @ciphertext VARBINARY(256);  
SELECT @ciphertext = SecretCol  
FROM DemoKey WHERE IDCol = 1 ;  
SELECT CAST (  
DECRYPTBYKEY( @ciphertext)  
AS VARCHAR(100) ) AS SecretText ;  
-- Use KEY_NAME to view the name of the key  
SELECT KEY_NAME(@ciphertext) AS [Name of Key] ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.symmetric_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [ENCRYPTBYKEY (Transact-SQL)](../../t-sql/functions/encryptbykey-transact-sql.md)   
 [DECRYPTBYKEYAUTOASYMKEY (Transact-SQL)](../../t-sql/functions/decryptbykeyautoasymkey-transact-sql.md)  
  
  
