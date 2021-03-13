---
description: DROP ASYMMETRIC KEY (Transact-SQL)
title: DROP ASYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP ASYMMETRIC KEY
- DROP_ASYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- asymmetric keys [SQL Server], removing
- removing asymmetric keys
- encryption [SQL Server], asymmetric keys
- DROP ASYMMETRIC KEY statement
- dropping asymmetric keys
- deleting asymmetric keys
- cryptography [SQL Server], asymmetric keys
ms.assetid: bf94ac07-9b62-4318-b55b-1eed8f3a1ac6
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 072e09f359ce45a2ede9531dd4e622fa71ea0df7
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2021
ms.locfileid: "103422029"
---
# <a name="drop-asymmetric-key-transact-sql"></a>DROP ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  从数据库中删除非对称密钥。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DROP ASYMMETRIC KEY key_name [ REMOVE PROVIDER KEY ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 key_name  
 要从数据库中删除的非对称密钥的名称。  
  
 REMOVE PROVIDER KEY  
 从 EKM 设备中删除可扩展密钥管理 (EKM) 密钥。 有关可扩展密钥管理的详细信息，请参阅[可扩展密钥管理 (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)。  
  
## <a name="remarks"></a>备注  
 如果数据库中的某一对称密钥使用非对称密钥进行了加密，或者有用户或登录名映射到该非对称密钥，则不能将其删除。 在删除此类密钥前，必须删除映射到该密钥的所有用户或登录名。 同时还必须删除或更改使用该非对称密钥进行加密的所有对称密钥。 可以使用 [ALTER SYMMETRIC KEY](../../t-sql/statements/alter-symmetric-key-transact-sql.md) 的 DROP ENCRYPTION 选项删除非对称密钥的加密。  
  
 可以使用 [sys.asymmetric_keys](../../relational-databases/system-catalog-views/sys-asymmetric-keys-transact-sql.md) 目录视图访问非对称密钥的元数据。 从数据库中无法直接查看密钥本身。  
  
 如果将非对称密钥映射到 EKM 设备上的可扩展密钥管理 (EKM) 密钥并且未指定 REMOVE PROVIDER KEY 选项，则会从数据库中删除该密钥，但不会从设备上删除它。 这时会发出一条警告。  
  
## <a name="permissions"></a>权限  
 需要对非对称密钥具有 CONTROL 权限。  
  
## <a name="examples"></a>示例  
 以下示例从 `MirandaXAsymKey6` 数据库删除非对称密钥 `AdventureWorks2012`。  
  
```sql  
USE AdventureWorks2012;  
DROP ASYMMETRIC KEY MirandaXAsymKey6;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [ALTER ASYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [ALTER SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/alter-symmetric-key-transact-sql.md)  
  
  
