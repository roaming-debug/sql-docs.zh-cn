---
description: IDENT_SEED (Transact-SQL)
title: IDENT_SEED (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IDENT_SEED_TSQL
- IDENT_SEED
dev_langs:
- TSQL
helpviewer_keywords:
- identity columns [SQL Server], IDENT_SEED function
- seed values [SQL Server]
- IDENT_SEED function
ms.assetid: e4cb8eb8-affb-4810-a8a9-0110af3c247a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c75867165d29be5af4073aa6c8e2281687db7f0c
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91114775"
---
# <a name="ident_seed-transact-sql"></a>IDENT_SEED (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回在表或视图中创建标识列时指定的原始种子值。 使用 DBCC CHECKIDENT 更改标识列的当前值不会更改此函数返回的值。  
  
 ![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
IDENT_SEED ( 'table_or_view' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **'** *table_or_view* **'**  
 指定表或视图以检查标识种子值的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 table_or_view 可以是带有引号的字符串常量，也可以是变量、函数或列名。 table_or_view 的数据类型为 char、nchar、varchar 或 nvarchar。  
  
## <a name="return-types"></a>返回类型  
numeric([@@MAXPRECISION](../../t-sql/functions/max-precision-transact-sql.md),0))  
  
## <a name="exceptions"></a>例外  
 出现错误时或调用方没有查看对象的权限时将返回 NULL。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，用户只能查看用户拥有或被授予权限的安全对象的元数据。 这种安全性意味着，如果用户对该对象没有任何权限，则某些会产生元数据的内置函数（如 IDENT_SEED）可能返回 NULL。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-the-seed-value-from-a-specified-table"></a>A. 从指定表返回种子值  
 以下示例返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中 `Person.Address` 表的种子值。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT IDENT_SEED('Person.Address') AS Identity_Seed;  
GO  
```  
  
### <a name="b-returning-the-seed-value-from-multiple-tables"></a>B. 从多个表返回种子值  
 下面的示例返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中带有种子值的标识列的表。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TABLE_SCHEMA, TABLE_NAME,   
   IDENT_SEED(TABLE_SCHEMA + '.' + TABLE_NAME) AS IDENT_SEED  
FROM INFORMATION_SCHEMA.TABLES  
WHERE IDENT_SEED(TABLE_SCHEMA + '.' + TABLE_NAME) IS NOT NULL;  
GO  
```  
  
 以下为部分结果集。  
  
 ```
 TABLE_SCHEMA       TABLE_NAME                   IDENT_SEED  
------------       ---------------------------  -----------  
Person             Address                                1  
Production         ProductReview                          1  
Production         TransactionHistory                100000  
Person             AddressType                            1  
Production         ProductSubcategory                     1  
Person             vAdditionalContactInfo                 1  
dbo                AWBuildVersion                         1
```  
  
## <a name="see-also"></a>另请参阅  
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [System Functions (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md)   
 [IDENT_INCR (Transact-SQL)](../../t-sql/functions/ident-incr-transact-sql.md)   
 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [sys.identity_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-identity-columns-transact-sql.md)  
  
