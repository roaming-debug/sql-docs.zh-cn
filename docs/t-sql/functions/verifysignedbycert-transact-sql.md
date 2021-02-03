---
description: VERIFYSIGNEDBYCERT (Transact-SQL)
title: VERIFYSIGNEDBYCERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- VERIFYSIGNEDBYCERT
- VERIFYSIGNEDBYCERT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- digitally signed data for changes [SQL Server]
- verifying digitally signed data for changes
- testing digitally signed data for changes
- checking digitally signed data for changes
- VERIFYSIGNEDBYCERT
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 4e041f33-60c4-4190-91c7-220d51dd6c8f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6a0d08e5d4c85e34617685614b4305e3cd7b3529
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210613"
---
# <a name="verifysignedbycert-transact-sql"></a>VERIFYSIGNEDBYCERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  测试经过数字签名的数据在签名后是否发生了更改。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
VerifySignedByCert( Cert_ID , signed_data , signature )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 Cert_ID  
 数据库中证书的 ID。 Cert_ID 的数据类型为 int。  
  
 signed_data  
 类型为 nvarchar、char、varchar 或 nchar 的变量，上述类型包含已使用证书进行签名的数据。  
  
 *签名*  
 附加到已签名数据中的签名。 signature 的数据类型为 varbinary。  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
 如果已签名的数据未更改，则返回 1；否则返回 0。  
  
## <a name="remarks"></a>注解  
 VerifySignedBycert 使用指定证书的公钥对数据的签名进行解密，并将解密所得到的值与数据新计算出的 MD5 哈希值进行比较。 如果值匹配，则确认签名有效。  
  
## <a name="permissions"></a>权限  
 需要对证书拥有 VIEW DEFINITION 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-verifying-that-signed-data-has-not-been-tampered-with"></a>A. 验证已签名的数据尚未被篡改  
 以下示例测试 `Signed_Data` 中的信息是否自使用名为 `Shipping04` 的证书对其进行签名之后发生了更改。 签名存储在 `DataSignature` 中。 证书 `Shipping04` 将传递给 `Cert_ID`，后者则返回数据库中证书的 ID。 如果 `VerifySignedByCert` 返回 1，则表示签名正确。 如果 `VerifySignedByCert` 返回 0，则 `Signed_Data` 中的数据不是用于生成 `DataSignature` 的数据。 在这种情况下，要么 `Signed_Data` 自签名以来发生了更改，要么 `Signed_Data` 使用不同的证书进行了签名。  
  
```sql
SELECT Data, VerifySignedByCert( Cert_Id( 'Shipping04' ),  
    Signed_Data, DataSignature ) AS IsSignatureValid  
FROM [AdventureWorks2012].[SignedData04]   
WHERE Description = N'data signed by certificate ''Shipping04''';  
GO  
```  
  
### <a name="b-returning-only-records-that-have-a-valid-signature"></a>B. 仅返回包含有效签名的记录  
 此查询仅返回自使用证书 `Shipping04` 签名以来尚未更改的记录。  
  
```sql
SELECT Data FROM [AdventureWorks2012].[SignedData04]   
WHERE VerifySignedByCert( Cert_Id( 'Shipping04' ), Data,   
    DataSignature ) = 1   
AND Description = N'data signed by certificate ''Shipping04''';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CERT_ID (Transact-SQL)](../../t-sql/functions/cert-id-transact-sql.md)   
 [SIGNBYCERT (Transact-SQL)](../../t-sql/functions/signbycert-transact-sql.md)   
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE (Transact-SQL)](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE (Transact-SQL)](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
