---
description: ENCRYPTBYCERT (Transact-SQL)
title: ENCRYPTBYCERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ENCRYPTBYCERT
- ENCRYPTBYCERT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- certificates [SQL Server], encryption
- encryption [SQL Server], certificates
- ENCRYPTBYCERT function
ms.assetid: ab66441f-e2d2-4e3a-bcae-bcc09e12f3c1
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f1e7e8de070f7b6811a1225e4c5158f35e3deb96
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158350"
---
# <a name="encryptbycert-transact-sql"></a>ENCRYPTBYCERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

使用证书的公钥加密数据。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
EncryptByCert ( certificate_ID , { 'cleartext' | @cleartext } )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
_certificate\_ID_  
数据库中证书的 ID。 int。  
  
cleartext  
将使用证书进行加密的数据字符串。  
  
**\@cleartext**  
以下其中一种类型的变量，其中包含将使用证书的公钥进行加密的数据：

* **nvarchar** 
* **char**
* **varchar**
* **binary** 
* **varbinary**
* **nchar**
  
## <a name="return-types"></a>返回类型  
varbinary（最大大小为 8000 个字节）。  
  
## <a name="remarks"></a>注解  
此函数使用证书的公钥对数据进行加密。 只能使用相应的私钥对加密文本进行解密。 相较使用对称密钥进行加密和解密的方法，这些非对称转换的开销更大。 因此，建议在处理大型数据集时不要使用非对称加密。
  
## <a name="examples"></a>示例  
此示例将以称为 `@cleartext` 的证书对在 `JanainaCert02` 中存储的纯文本进行加密。 经过加密的数据将插入表 `ProtectedData04` 中。  
  
```sql  
INSERT INTO [AdventureWorks2012].[ProtectedData04]   
    VALUES ( N'Data encrypted by certificate ''Shipping04''',  
    EncryptByCert(Cert_ID('JanainaCert02'), @cleartext) );  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
[DECRYPTBYCERT (Transact-SQL)](../../t-sql/functions/decryptbycert-transact-sql.md)   
[CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
[ALTER CERTIFICATE (Transact-SQL)](../../t-sql/statements/alter-certificate-transact-sql.md)   
[DROP CERTIFICATE (Transact-SQL)](../../t-sql/statements/drop-certificate-transact-sql.md)   
[BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md)   
[加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
