---
description: DECRYPTBYCERT (Transact-SQL)
title: DECRYPTBYCERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DecryptByCert_TSQL
- DECRYPTBYCERT
dev_langs:
- TSQL
helpviewer_keywords:
- certificates [SQL Server], decryption
- decryption [SQL Server], certificates
- DECRYPTBYCERT function
ms.assetid: 4950d787-40fa-4e26-bce8-2cb2ceca12fb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 849826f084126d5b0ccc6c25896b642ce8c7f5f0
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96116293"
---
# <a name="decryptbycert-transact-sql"></a>DECRYPTBYCERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函数使用证书的私钥解密已加密数据。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DecryptByCert ( certificate_ID , { 'ciphertext' | @ciphertext }   
    [ , { 'cert_password' | @cert_password } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 certificate_ID  
数据库中证书的 ID。 certificate_ID 具有 int 数据类型。  
  
 ciphertext  
使用证书的公钥加密的数据的字符串。  
  
 @ciphertext  
varbinary 类型的变量，包含使用证书进行加密的数据。  
  
 cert_password  
用于加密证书私钥的密码。 *cert_password* 必须具有 Unicode 数据格式。  
  
 @cert_password  
类型为 nchar 或 nvarchar 的变量，其中包含用来加密证书私钥的密码。 *\@cert_password* 必须具有 Unicode 数据格式。  

## <a name="return-types"></a>返回类型  
varbinary（最大大小为 8,000 个字节）。  
  
## <a name="remarks"></a>备注  
此函数用证书的私钥解密数据。 使用非对称密钥进行的加密转换会消耗大量资源。 因此，建议开发人员避免使用 [ENCRYPTBYCERT](./encryptbycert-transact-sql.md) 和 DECRYPTBYCERT 进行用户数据的常规加密/解密。  

## <a name="permissions"></a>权限  
`DECRYPTBYCERT` 需要对证书具有 CONTROL 权限。  
  
## <a name="examples"></a>示例  
此示例从 `[AdventureWorks2012].[ProtectedData04]` 选择行，选择范围标记为最初使用证书 `JanainaCert02` 加密的数据。 该示例首先使用证书 `pGFD4bb925DGvbd2439587y` 的密码解密证书 `JanainaCert02` 的私钥。 然后使用此私钥解密已加密文本。 该示例将解密后的数据从 varbinary 转换为 nvarchar。  

```sql  
SELECT CONVERT(NVARCHAR(max), DecryptByCert(Cert_Id('JanainaCert02'),  
    ProtectedData, N'pGFD4bb925DGvbd2439587y'))  
FROM [AdventureWorks2012].[ProtectedData04]   
WHERE Description   
    = N'data encrypted by certificate '' JanainaCert02''';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ENCRYPTBYCERT (Transact-SQL)](../../t-sql/functions/encryptbycert-transact-sql.md)   
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE (Transact-SQL)](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE (Transact-SQL)](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
