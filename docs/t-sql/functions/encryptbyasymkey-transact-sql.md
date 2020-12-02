---
description: ENCRYPTBYASYMKEY (Transact-SQL)
title: ENCRYPTBYASYMKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ENCRYPTBYASYMKEY
- ENCRYPTBYASYMKEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTBYASYMKEY function
- encryption [SQL Server], asymmetric keys
- asymmetric keys [SQL Server], ENCRYPTBYASYMKEY function
ms.assetid: 86bb2588-ab13-4db2-8f3c-42c9f572a67b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 42ffb61535beefeee149124adf7873cce78c1535
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128504"
---
# <a name="encryptbyasymkey-transact-sql"></a>ENCRYPTBYASYMKEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函数使用非对称密钥加密数据。  
  
 ![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
EncryptByAsymKey ( Asym_Key_ID , { 'plaintext' | @plaintext } )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
Asym_Key_ID  
数据库中非对称密钥的 ID。 Asym_Key_ID 具有 int 数据类型。  
  
cleartext  
`ENCRYPTBYASYMKEY` 将使用非对称密钥对其加密的数据字符串。 cleartext 可以具有
 
+ **binary**
+ **char**
+ **nchar**
+ **nvarchar**
+ **varbinary**
  
或
  
+ **varchar**
 
数据类型。  
  
**\@plaintext**  
`ENCRYPTBYASYMKEY` 将使用非对称密钥对其加密的包含值的变量。 **\@plaintext** 可以具有
  
+ **binary**
+ **char**
+ **nchar**
+ **nvarchar**
+ **varbinary**
  
或
  
+ **varchar**
 
数据类型。  
  
## <a name="return-types"></a>返回类型  
varbinary（最大大小为 8,000 个字节）。  
  
## <a name="remarks"></a>备注  
与对称密钥加密和解密相比，使用非对称密钥的加密和解密操作消耗大量资源，因此成本很高。 我们建议开发人员避免对大型数据集执行非对称密钥加密和解密操作 - 例如，存储在数据库表中的用户数据数据集。 相反，我们建议开发人员首先使用强对称密钥对数据进行加密，然后使用非对称密钥对该对称密钥进行加密。  
  
如果输入超出一定字节数，`ENCRYPTBYASYMKEY` 将返回 NULL（具体取决于算法）。 具体限制：

+ 一个 512 位的 RSA 密钥最多可加密 53 个字节
+ 一个 1024 位的密钥最多可加密 117 个字节
+ 一个 2048 位的密钥最多可加密 245 个字节

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，证书和非对称密钥都用作 RSA 密钥的包装器。  
  
## <a name="examples"></a>示例  
此示例将用非对称密钥 `JanainaAsymKey02` 加密 `@cleartext` 中存储的文本。 该语句将加密数据插入到 `ProtectedData04` 表中。  
  
```sql  
INSERT INTO AdventureWorks2012.Sales.ProtectedData04   
    VALUES( N'Data encrypted by asymmetric key ''JanainaAsymKey02''',  
    EncryptByAsymKey(AsymKey_ID('JanainaAsymKey02'), @cleartext) );  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [DECRYPTBYASYMKEY (Transact-SQL)](../../t-sql/functions/decryptbyasymkey-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
