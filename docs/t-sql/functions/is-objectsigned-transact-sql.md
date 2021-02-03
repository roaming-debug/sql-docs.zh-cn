---
description: IS_OBJECTSIGNED (Transact-SQL)
title: IS_OBJECTSIGNED (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_OBJECTSIGNED
- IS_OBJECTSIGNED_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IS_OBJECTSIGNED function
ms.assetid: afbc4f7f-8266-4ee6-9802-14a2dbe69ef6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e33459d72484363319449d1baba953e0165d6407
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165108"
---
# <a name="is_objectsigned-transact-sql"></a>IS_OBJECTSIGNED (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  指示对象由指定证书或非对称密钥签名。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
IS_OBJECTSIGNED (   
'OBJECT', @object_id, @class, @thumbprint  
  )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **'OBJECT'**  
 安全对象类的类型。  
  
 *\@object_id*  
 要测试的对象的 object_id。 *\@object_id* 为 **int** 类型。  
  
 *\@class*  
 对象的类：  
  
-   “证书”  
  
-   “非对称密钥”  
  
 *\@class* 为 **sysname** 类型。  
  
 *\@thumbprint*  
 对象的 SHA 指纹。 *\@thumbprint* 为 **varbinary(32)** 类型。  
  
## <a name="returned-types"></a>返回类型  
 **int**  
  
## <a name="remarks"></a>备注  
 IS_OBJECTSIGNED 返回以下值。  
  
|返回值|说明|  
|------------------|-----------------|  
|Null|对象未签名，或对象无效。|  
|0|对象已签名，但签名无效。|  
|1|该对象已签名。|  
  
## <a name="permissions"></a>权限  
 要求对证书或非对称密钥拥有 VIEW DEFINITION 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-displaying-extended-properties-on-a-database"></a>A. 显示数据库的扩展属性  
 下面的示例测试 master 数据库中的 spt_fallback_db 表是否由架构签名证书进行签名。  
  
```sql  
USE master;  
-- Declare a variable to hold a thumbprint and an object name  
DECLARE @thumbprint varbinary(20), @objectname sysname;  
  
-- Populate the thumbprint variable with the thumbprint of   
-- the master database schema signing certificate  
SELECT @thumbprint = thumbprint   
FROM sys.certificates   
WHERE name LIKE '%SchemaSigningCertificate%';  
  
-- Populate the object name variable with a table name in master  
SELECT @objectname = 'spt_fallback_db';  
  
-- Query to see if the table is signed by the thumbprint  
SELECT @objectname AS [object name],  
IS_OBJECTSIGNED(  
'OBJECT', OBJECT_ID(@objectname), 'certificate', @thumbprint  
) AS [Is the object signed?] ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.fn_check_object_signatures (Transact-SQL)](../../relational-databases/system-functions/sys-fn-check-object-signatures-transact-sql.md)  
  
  
