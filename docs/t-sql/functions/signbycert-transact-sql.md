---
description: SIGNBYCERT (Transact-SQL)
title: SIGNBYCERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SIGNBYCERT
- SIGNBYCERT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- text signing [SQL Server]
- encryption [SQL Server], certificates
- certificates [SQL Server], text signing
- SignByCert function
- signing text [SQL Server]
- SIGNBYCERT function
- cryptography [SQL Server], certificates
ms.assetid: b4c6bced-4473-4bae-85b9-56deced495f9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 5de3e5457e6cf408d5693e12a866e1fcf4d02cb0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212163"
---
# <a name="signbycert-transact-sql"></a>SIGNBYCERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  使用证书对文本进行签名并返回签名。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql 
SignByCert ( certificate_ID , @cleartext [ , 'password' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 certificate_ID  
 当前数据库中证书的 ID。 certificate_ID 是 int。  
  
 *\@cleartext*  
 类型为 nvarchar、char、varchar 或 nchar 的变量，其中包含要签名的数据。  
  
 ' password '  
 用来对证书私钥进行加密的密码。 password 的数据类型为 nvarchar(128)。  
  
## <a name="return-types"></a>返回类型  
 varbinary（最大大小为 8000 个字节）。  
  
## <a name="remarks"></a>注解  
 需要对证书具有 CONTROL 权限。  
  
## <a name="examples"></a>示例  
 以下示例将用证书 `ABerglundCert07` 来签署 `@SensitiveData` 中的文本，该证书已用密码“pGFD4bb925DGvbd2439587y”进行解密。 然后，它在 `SignedData04` 表中插入明文和签名。  
  
```sql  
DECLARE @SensitiveData NVARCHAR(max);  
SET @SensitiveData = N'Saddle Price Points are   
    2, 3, 5, 7, 11, 13, 17, 19, 23, 29';  
INSERT INTO [SignedData04]  
    VALUES( N'data signed by certificate ''ABerglundCert07''',  
    @SensitiveData, SignByCert( Cert_Id( 'ABerglundCert07' ),   
    @SensitiveData, N'pGFD4bb925DGvbd2439587y' ));  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [VERIFYSIGNEDBYCERT (Transact-SQL)](../../t-sql/functions/verifysignedbycert-transact-sql.md)   
 [CERT_ID (Transact-SQL)](../../t-sql/functions/cert-id-transact-sql.md)   
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE (Transact-SQL)](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE (Transact-SQL)](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
