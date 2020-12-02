---
description: HASHBYTES (Transact-SQL)
title: HASHBYTES (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- HASHBYTES_TSQL
- HASHBYTES
dev_langs:
- TSQL
helpviewer_keywords:
- hash input
- HASHBYTES
ms.assetid: 0ea6a4d1-313e-4f70-b939-dd2cd570f6d6
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d70ad412676a06c9a72eac8379fc72673c41275f
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91116376"
---
# <a name="hashbytes-transact-sql"></a>HASHBYTES (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回其在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的输入的 MD2、MD4、MD5、SHA、SHA1 或 SHA2 哈希值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
HASHBYTES ( '<algorithm>', { @input | 'input' } )  
  
<algorithm>::= MD2 | MD4 | MD5 | SHA | SHA1 | SHA2_256 | SHA2_512   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

`<algorithm>`  
标识用于对输入执行哈希操作的哈希算法。 这是必选参数，无默认值。 需要使用单引号。 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，除 SHA2_256 和 SHA2_512 以外的所有算法都已过时。  
  
`@input`  
指定包含要对其执行哈希操作的数据的变量。  为 varchar、nvarchar 或 varbinary`@input`。  
  
'input'  
指定一个表达式，其计算结果为要对其执行哈希操作的字符或二进制字符串。  
  
 输出符合算法标准：MD2、MD4 和 MD5 为 128 位（即 16 个字节）；SHA 和 SHA1 为 160 位（即 20 个字节）；SHA2_256 为 256 位（即 32 个字节），SHA2_512 为 512 位（即 64 个字节）。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本
  
 对于 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 和更早版本，允许的输入值限制为 8000 个字节。  
  
## <a name="return-value"></a>返回值  
 varbinary（最大 8000 个字节）  

## <a name="remarks"></a>备注  
请考虑使用 `CHECKSUM` 或 `BINARY_CHECKSUM` 作为替代方案，以计算哈希值。

自 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 起，已弃用 MD2、MD4、MD5、SHA 和 SHA1 算法。 请改用 SHA2_256 或 SHA2_512。 旧算法将继续起作用，但会抛出弃用事件。

## <a name="examples"></a>示例  
### <a name="return-the-hash-of-a-variable"></a>返回变量的哈希  
 以下示例返回变量 `@HashThis` 中存储的 nvarchar 数据的 `SHA2_256` 哈希值。  
  
```sql  
DECLARE @HashThis NVARCHAR(32);  
SET @HashThis = CONVERT(NVARCHAR(32),'dslfdkjLK85kldhnv$n000#knf');  
SELECT HASHBYTES('SHA2_256', @HashThis);  
```  
  
### <a name="return-the-hash-of-a-table-column"></a>返回表列的哈希  
 下面的示例返回 `Test1` 表 `c1` 列中值的 SHA2_256 哈希。  
  
```sql  
CREATE TABLE dbo.Test1 (c1 NVARCHAR(32));  
INSERT dbo.Test1 VALUES ('This is a test.');  
INSERT dbo.Test1 VALUES ('This is test 2.');  
SELECT HASHBYTES('SHA2_256', c1) FROM dbo.Test1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------------------------------------  
0x741238C01D9DB821CF171BF61D72260B998F7C7881D90091099945E0B9E0C2E3 
0x91DDCC41B761ACA928C62F7B0DA61DC763255E8247E0BD8DCE6B22205197154D  
(2 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
[选择加密算法](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)
[CHECKSUM_AGG &#40;TRANSACT-SQL&#41;](../../t-sql/functions/checksum-agg-transact-sql.md)
[CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)
[BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md)
