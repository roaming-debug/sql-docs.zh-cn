---
description: IDENT_INCR (Transact-SQL)
title: IDENT_INCR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IDENT_INCR
- IDENT_INCR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- incremental values [SQL Server]
- IDENT_INCR function
- identity columns [SQL Server], IDENT_INCR function
ms.assetid: e13b491f-4f1f-4cb6-8b63-5084120f98cf
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 19b7d284a69997ef5b3c93713335fa7f3854849a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91114790"
---
# <a name="ident_incr-transact-sql"></a>IDENT_INCR (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回在创建表或视图的标识列时指定的递增值。  
  
![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql 
IDENT_INCR ( 'table_or_view' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
**'** *table_or_view* **'**  
指定表或视图以检查有效的标识增量值的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 table_or_view 可以是带有引号的字符串常量。 也可以是变量、函数或列名。 table_or_view 的数据类型为 char、nchar、varchar 或 nvarchar。  
  
## <a name="return-types"></a>返回类型  
numeric([@@MAXPRECISION](../../t-sql/functions/max-precision-transact-sql.md),0))  
  
## <a name="exceptions"></a>例外  
在出错或调用方无权查看对象时返回 NULL。  
  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，用户只能查看自己拥有或有权查看的安全对象的元数据。 如果没有用户对象权限，发出元数据的内置函数（如 IDENT_INCR）可能会返回 NULL。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-the-increment-value-for-a-specified-table"></a>A. 返回指定表的增量值  
 以下示例返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中 `Person.Address` 表的增量值。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT IDENT_INCR('Person.Address') AS Identity_Increment;  
GO  
```  
  
### <a name="b-returning-the-increment-value-from-multiple-tables"></a>B. 从多个表返回增量值  
 下面的示例返回所含标识列有增量值的 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的表。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TABLE_SCHEMA, TABLE_NAME,   
   IDENT_INCR(TABLE_SCHEMA + '.' + TABLE_NAME) AS IDENT_INCR  
FROM INFORMATION_SCHEMA.TABLES  
WHERE IDENT_INCR(TABLE_SCHEMA + '.' + TABLE_NAME) IS NOT NULL;  
```  
  
 以下为部分结果集。  
  
 ```
 TABLE_SCHEMA        TABLE_NAME                IDENT_INCR  
------------        ------------------------  ----------  
Person              Address                            1  
Production          ProductReview                      1  
Production          TransactionHistory                 1  
Person              AddressType                        1  
Production          ProductSubcategory                 1  
Person              vAdditionalContactInfo             1  
dbo                 AWBuildVersion                     1  
Production          BillOfMaterials                    1
```  
  
## <a name="see-also"></a>另请参阅  
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [System Functions (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md)   
 [IDENT_SEED (Transact-SQL)](../../t-sql/functions/ident-seed-transact-sql.md)   
 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [sys.identity_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-identity-columns-transact-sql.md)  
  
  
