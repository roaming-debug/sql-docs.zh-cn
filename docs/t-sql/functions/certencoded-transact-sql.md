---
description: CERTENCODED (Transact-SQL)
title: CERTENCODED (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CERTENCODED
- CERTENCODED_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CERTENCODED
ms.assetid: 677a0719-7b9a-4f0b-bc61-41634563f924
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 974485667667296edd9a8d4d6c4187fd93900c1e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193431"
---
# <a name="certencoded-transact-sql"></a>CERTENCODED (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

此函数返回二进制格式的证书的公共部分。 此函数将证书 ID 作为参数，并返回已编码的证书。 若要创建新证书，请将二进制结果传递给 **CREATE CERTIFICATE ...WITH BINARY**。
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CERTENCODED ( cert_id )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
cert_id  
证书的 certificate_id。 在 sys. 证书中找到此值；[CERT_ID (Transact-SQL)](../../t-sql/functions/cert-id-transact-sql.md) 函数也将返回该值。 cert_id 为 int 数据类型。
  
## <a name="return-types"></a>返回类型
**varbinary**
  
## <a name="remarks"></a>备注  
同时使用 CERTENCODED 和 CERTPRIVATEKEY 以二进制格式返回证书的其他部分。
  
## <a name="permissions"></a>权限  
CERTENCODED 向用户开放使用。
  
## <a name="examples"></a>示例  
  
### <a name="simple-example"></a>简单示例  
此示例创建一个名为 `Shipping04` 的证书，然后使用 CERTENCODED 函数返回该证书的二进制编码。 此示例将该证书的到期日期设置为 2040 年 10 月 31 日。
  
```sql
CREATE DATABASE TEST1;
GO
USE TEST1
CREATE CERTIFICATE Shipping04
ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'
WITH SUBJECT = 'Sammamish Shipping Records',
EXPIRY_DATE = '20401031';
GO
SELECT CERTENCODED(CERT_ID('Shipping04'));
  
```  
  
### <a name="b-copying-a-certificate-to-another-database"></a>B. 将证书复制到另一个数据库  
这个更为复杂的示例创建了两个数据库：`SOURCE_DB` 和 `TARGET_DB`。 然后，在 `SOURCE_DB` 中创建证书，并在之后将该证书复制到 `TARGET_DB`。 最后，演示可以使用该证书的副本在 `TARGET_DB` 中解密已加密到 `SOURCE_DB` 中数据。
  
若要创建示例环境，请创建 `SOURCE_DB` 和 `TARGET_DB` 数据库，并在每个数据库中创建一个主密钥。 然后在 `SOURCE_DB` 中创建证书。
  
```sql
USE master;  
GO  
CREATE DATABASE SOURCE_DB;  
GO  
USE SOURCE_DB;  
GO  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0URCE_DB KEY Pa$$W0rd';  
GO  
CREATE DATABASE TARGET_DB;  
GO  
USE TARGET_DB  
GO  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Pa$$W0rd in TARGET_DB';  
GO  
  
-- Create a certificate in SOURCE_DB  
USE SOURCE_DB;  
GO  
CREATE CERTIFICATE SOURCE_CERT WITH SUBJECT = 'SOURCE_CERTIFICATE';  
GO  
```  
  
接下来，提取该证书的二进制说明。
  
```sql
DECLARE @CERTENC VARBINARY(MAX);  
DECLARE @CERTPVK VARBINARY(MAX);  
SELECT @CERTENC = CERTENCODED(CERT_ID('SOURCE_CERT'));  
SELECT @CERTPVK = CERTPRIVATEKEY(CERT_ID('SOURCE_CERT'),  
       'CertEncryptionPa$$word');  
SELECT @CERTENC AS BinaryCertificate;  
SELECT @CERTPVK AS EncryptedBinaryCertificate;  
GO  
```  
  
然后，在 `TARGET_DB` 数据库中创建副本证书。 修改以下代码使其生效，插入在上一步中返回的两个二进制值（@CERTENC 和 @CERTPVK）。 不要给这些值加上引号。
  
```sql
-- Create the duplicate certificate in the TARGET_DB database  
USE TARGET_DB  
GO  
CREATE CERTIFICATE TARGET_CERT  
FROM BINARY = <insert the binary value of the @CERTENC variable>  
WITH PRIVATE KEY (  
BINARY = <insert the binary value of the @CERTPVK variable>  
, DECRYPTION BY PASSWORD = 'CertEncryptionPa$$word');  
-- Compare the certificates in the two databases  
-- The two certificates should be the same   
-- except for name and (possibly) the certificate_id  
SELECT * FROM SOURCE_DB.sys.certificates  
UNION  
SELECT * FROM TARGET_DB.sys.certificates;  
```  
  
这个作为一批执行的代码将演示 `TARGET_DB` 可以解密最初在 `SOURCE_DB` 中加密的数据。
  
```sql
USE SOURCE_DB;  
  
DECLARE @CLEARTEXT nvarchar(100);  
DECLARE @CIPHERTEXT varbinary(8000);  
DECLARE @UNCIPHEREDTEXT_Source nvarchar(100);  
SET @CLEARTEXT = N'Hello World';  
SET @CIPHERTEXT = ENCRYPTBYCERT(CERT_ID('SOURCE_CERT'), @CLEARTEXT);  
SET @UNCIPHEREDTEXT_Source =   
    DECRYPTBYCERT(CERT_ID('SOURCE_CERT'), @CIPHERTEXT)  
-- Encryption and decryption result in SOURCE_DB  
SELECT @CLEARTEXT AS SourceClearText, @CIPHERTEXT AS SourceCipherText,   
       @UNCIPHEREDTEXT_Source AS SourceDecryptedText;  
  
-- SWITCH DATABASE  
USE TARGET_DB;  
  
DECLARE @UNCIPHEREDTEXT_Target nvarchar(100);  
SET @UNCIPHEREDTEXT_Target = DECRYPTBYCERT(CERT_ID('TARGET_CERT'), @CIPHERTEXT);  
-- Encryption and decryption result in TARGET_DB  
SELECT @CLEARTEXT AS ClearTextInTarget, @CIPHERTEXT AS CipherTextInTarget, @UNCIPHEREDTEXT_Target AS DecriptedTextInTarget;   
GO  
```  
  
## <a name="see-also"></a>另请参阅
[安全函数 (Transact-SQL)](../../t-sql/functions/security-functions-transact-sql.md)  
[CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)  
[CERTPRIVATEKEY (Transact-SQL)](../../t-sql/functions/certprivatekey-transact-sql.md)  
[sys.certificates (Transact-SQL)](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)
  
  
