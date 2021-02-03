---
description: CLOSE SYMMETRIC KEY (Transact-SQL)
title: CLOSE SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CLOSE SYMMETRIC KEY
- CLOSE_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- closing symmetric keys
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], closing
- CLOSE SYMMETRIC KEY statement
- cryptography [SQL Server], symmetric keys
ms.assetid: 3b083cbb-3c6a-4f59-8d34-601db1efcc83
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f393cc44ba44907f05561419235a84f1254f81e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184456"
---
# <a name="close-symmetric-key-transact-sql"></a>CLOSE SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  关闭对称密钥，或关闭在当前会话中打开的所有对称密钥。  
  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CLOSE { SYMMETRIC KEY key_name | ALL SYMMETRIC KEYS }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 Key_name  
 要关闭的对称密钥的名称。  
  
## <a name="remarks"></a>注解  
 打开的对称密钥将绑定到会话而不是安全上下文。 打开的密钥将持续有效，直到它显式关闭或会话终止。 CLOSE ALL SYMMETRIC KEYS 将通过使用 [OPEN MASTER KEY](../../t-sql/statements/open-master-key-transact-sql.md) 语句，关闭在当前会话中打开的任何数据库主密钥。  有关打开密钥的信息，请参阅 [sys.openkeys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-openkeys-transact-sql.md) 目录视图。  
  
## <a name="permissions"></a>权限  
 关闭对称密钥不需要显式权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-closing-a-symmetric-key"></a>A. 关闭对称密钥  
 以下示例关闭对称密钥 `ShippingSymKey04`。  
  
```sql  
CLOSE SYMMETRIC KEY ShippingSymKey04;  
GO  
```  
  
### <a name="b-closing-all-symmetric-keys"></a>B. 关闭所有对称密钥  
 以下示例关闭在当前会话中打开的所有对称密钥，还将关闭显式打开的数据库主密钥。  
  
```sql  
CLOSE ALL SYMMETRIC KEYS;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [OPEN SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/drop-symmetric-key-transact-sql.md)  
  
  
