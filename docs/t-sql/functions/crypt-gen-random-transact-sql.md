---
description: CRYPT_GEN_RANDOM (Transact-SQL)
title: CRYPT_GEN_RANDOM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CRYPT_GEN_RANDOM_TSQL
- CRYPT_GEN_RANDOM
dev_langs:
- TSQL
helpviewer_keywords:
- CRYPT_GEN_RANDOM function
ms.assetid: b74bd9d4-758e-4b94-89a0-76dcda6d8c42
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d0cdca25e14d58270185d4605287d22d1af5e0d2
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96118619"
---
# <a name="crypt_gen_random-transact-sql"></a>CRYPT_GEN_RANDOM (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函数返回 Crypto API (CAPI) 随机生成的加密随机数。 `CRYPT_GEN_RANDOM` 返回具有指定字节数长度的十六进制数。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CRYPT_GEN_RANDOM ( length [ , seed ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*length*  
`CRYPT_GEN_RANDOM` 将创建数的长度数，以字节为单位。 length 参数的数据类型为“int”且值范围在 1 和 8000 之间。 `CRYPT_GEN_RANDOM` 为此范围外的“int”值返回 NULL。 
  
seed  
一个可选的十六进制数字，用作随机种子值。 种子的长度必须匹配 length 参数的值。 种子参数的数据类型为“varbinary (8000)”。
  
## <a name="returned-types"></a>返回类型  
varbinary(8000)
  
## <a name="permissions"></a>权限  
此函数是公用的，因此不需要任何特殊权限。
  
## <a name="examples"></a>示例  
  
### <a name="a-generating-a-random-number"></a>A. 生成随机数  
此示例会生成长度为 50 个字节的随机数：
  
```sql
SELECT CRYPT_GEN_RANDOM(50) ;  
```  
  
此示例会使用 4 字节的种子生成长度为 4 个字节的随机数：
  
```sql
SELECT CRYPT_GEN_RANDOM(4, 0x25F18060) ;  
```  
  
## <a name="see-also"></a>另请参阅
[RAND (Transact-SQL)](../../t-sql/functions/rand-transact-sql.md)
  
  
