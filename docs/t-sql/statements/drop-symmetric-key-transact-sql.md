---
description: DROP SYMMETRIC KEY (Transact-SQL)
title: DROP SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP SYMMETRIC KEY
- DROP_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- symmetric keys [SQL Server], removing
- deleting symmetric keys
- encryption [SQL Server], symmetric keys
- removing symmetric keys
- dropping symmetric keys
- cryptography [SQL Server], symmetric keys
- DROP SYMMETRIC KEY statement
ms.assetid: 6150bc67-08cb-402e-9c24-b04c9654b434
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f07f4d02c005ee89b24a323111c05789dbbd9fac
ms.sourcegitcommit: 7f76975c29d948a9a3b51abce564b9c73d05dcf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96901036"
---
# <a name="drop-symmetric-key-transact-sql"></a>DROP SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  从当前数据库中删除对称密钥。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
DROP SYMMETRIC KEY symmetric_key_name [REMOVE PROVIDER KEY]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *symmetric_key_name*  
 要删除的对称密钥的名称。  
  
 REMOVE PROVIDER KEY  
 从 EKM 设备中删除可扩展密钥管理 (EKM) 密钥。 有关可扩展密钥管理的详细信息，请参阅[可扩展密钥管理 (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)。  
  
## <a name="remarks"></a>备注  
  
如果非对称密钥映射到 EKM 设备上的可扩展密钥管理 (EKM) 密钥并且未指定 REMOVE PROVIDER KEY 选项，则会从数据库中删除该密钥，但不会从设备上删除它，同时会发出警告。  
  
## <a name="permissions"></a>权限  
 要求对对称密钥具有 CONTROL 权限。  
  
## <a name="examples"></a>示例  
 以下示例从当前数据库中删除名为 `GailSammamishKey6` 的对称密钥。  
  
```sql  
CLOSE SYMMETRIC KEY GailSammamishKey6;  
DROP SYMMETRIC KEY GailSammamishKey6;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CLOSE SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [可扩展密钥管理 &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
