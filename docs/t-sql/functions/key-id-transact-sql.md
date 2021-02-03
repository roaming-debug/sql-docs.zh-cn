---
description: KEY_ID (Transact-SQL)
title: KEY_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Key_ID
- Key_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- identification numbers [SQL Server], symmetric keys
- KEY_ID function
- symmetric keys [SQL Server], IDs
- IDs [SQL Server], symmetric keys
ms.assetid: d7309542-dbbe-41dc-b42e-5d9a1c8b4838
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c1481c85e999ebccdc47e18da315ddd302c77679
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183954"
---
# <a name="key_id-transact-sql"></a>KEY_ID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回当前数据库中对称密钥的 ID。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
Key_ID ( 'Key_Name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **'** *Key_Name* **'**  
 数据库中对称密钥的名称。  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="remarks"></a>备注  
 临时密钥的名称必须以数字符号 (#) 开头。  
  
## <a name="permissions"></a>权限  
 因为临时密钥只适用于创建它们的会话，所以访问它们不需要任何权限。 若要访问非临时密钥，调用者需要对该密钥具有相应权限，并且对该密钥的 VIEW 权限不得被拒绝。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-the-id-of-a-symmetric-key"></a>A. 返回对称密钥的 ID  
 以下示例返回名为 `ABerglundKey1` 的对称密钥的 ID。  
  
```sql  
SELECT KEY_ID('ABerglundKey1');  
```  
  
### <a name="b-returning-the-id-of-a-temporary-symmetric-key"></a>B. 返回临时对称密钥的 ID  
 以下示例返回临时对称密钥的 ID。 请注意，`#` 附加到密钥名称前。  
  
```sql  
SELECT KEY_ID('#ABerglundKey2');  
```  
  
## <a name="see-also"></a>另请参阅  
 [KEY_GUID (Transact-SQL)](../../t-sql/functions/key-guid-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [sys.symmetric_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [sys.key_encryptions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-key-encryptions-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
