---
description: KEY_GUID (Transact-SQL)
title: KEY_GUID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Key_GUID_TSQL
- Key_GUID
dev_langs:
- TSQL
helpviewer_keywords:
- symmetric keys [SQL Server], GUIDs
- KEY_GUID function
- GUIDs [SQL Server]
ms.assetid: 9246c7b2-7098-42c4-a222-cbf30267c46a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a41bc9595d6969b161a160691570125a8204b6d8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183977"
---
# <a name="key_guid-transact-sql"></a>KEY_GUID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回数据库中对称密钥的 GUID。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
Key_GUID( 'Key_Name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **'** *Key_Name* **'**  
 数据库中对称密钥的名称。  
  
## <a name="return-types"></a>返回类型  
 **uniqueidentifier**  
  
## <a name="remarks"></a>注解  
 如果创建密钥时指定了标识值，则其 GUID 为该标识值的 MD5 哈希。 如果未指定标识值，则服务器生成 GUID。  
  
 如果密钥为临时密钥，则密钥名称必须以数字符号 (#) 开头。  
  
## <a name="permissions"></a>权限  
 因为临时密钥只适用于创建它们的会话，所以访问它们不需要任何权限。 若要访问非临时密钥，调用者需要对该密钥具有相应权限，并且必须具有该密钥的 VIEW 权限。  
  
## <a name="examples"></a>示例  
 以下示例返回称为 `ABerglundKey1` 的对称密钥的 GUID。  
  
```sql  
SELECT Key_GUID('ABerglundKey1');  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [sys.symmetric_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [sys.key_encryptions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-key-encryptions-transact-sql.md)  
  
  
