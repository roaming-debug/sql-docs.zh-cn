---
description: IDENT_CURRENT (Transact-SQL)
title: IDENT_CURRENT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IDENT_CURRENT
- IDENT_CURRENT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- last identity value generated for table
- identity values [SQL Server], last generated
- identity columns, current value
- IDENT_CURRENT function
ms.assetid: 21517ced-39f5-4cd8-8d9c-0a0b8aff554a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 608f7c4745d2d5590fc4d2956f7fca580c9b4111
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184015"
---
# <a name="ident_current-transact-sql"></a>IDENT_CURRENT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回为指定的表或视图生成的最后一个标识值。 所生成的最后一个标识值可以针对任何会话和任何作用域。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
IDENT_CURRENT( 'table_or_view' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
table_or_view  
返回其标识值的表或视图的名称。 table_or_view 是 varchar，无默认值。  
  
## <a name="return-types"></a>返回类型  
numeric([@@MAXPRECISION](../../t-sql/functions/max-precision-transact-sql.md),0))  
  
## <a name="exceptions"></a>例外  
出现错误时或调用方没有查看对象的权限时，将返回 NULL。  
  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，用户只能查看其拥有的安全对象的元数据，或者已对其授予权限的安全对象的元数据。 这意味着，如果用户对对象没有任何权限，则元数据生成的内置函数（如 IDENT_CURRENT）可能返回 NULL。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="remarks"></a>注解  
IDENT_CURRENT 类似于 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 标识函数 SCOPE_IDENTITY 和 @@IDENTITY。 这三个函数都返回最后生成的标识值。 但是，上述每个函数中定义的“最后”的作用域和会话有所不同：  

-   IDENT_CURRENT 返回为某个会话和用域中的指定表生成的最新标识值。  
-   @@IDENTITY 返回为跨所有作用域的当前会话中的任何表生成的最后一个标识值。  
-   SCOPE_IDENTITY 返回为当前会话和当前作用域中的某个表生成的最新标识值。  
  
如果 IDENT_CURRENT 值为 NULL（因为表从未包含行或已被截断），IDENT_CURRENT 函数将返回种子值。  
  
如果语句和事务失败，它们会更改表的当前标识，从而使标识列中的值出现不连贯现象。 即使未提交试图向表中插入值的事务，也永远无法回滚标识值。 例如，如果因 IGNORE_DUP_KEY 冲突而导致 INSERT 语句失败，表的当前标识值仍然会增加。  

对包含联接的视图使用 IDENT_CURRENT 时，返回的是 NULL。 这与是只有一个还是有多个联接表有标识列无关。 
  
> [!IMPORTANT]
> 请谨慎使用 IDENT_CURRENT 来预测下一个生成的标识值。 由于其他会话执行的插入，实际生成的值可能与 IDENT_CURRENT 加上 IDENT_INCR 不同。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-the-last-identity-value-generated-for-a-specified-table"></a>A. 返回为指定表生成的最后一个标识值  
 以下示例返回为 `Person.Address` 数据库中的 `AdventureWorks2012` 表生成的最后一个标识值。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT IDENT_CURRENT ('Person.Address') AS Current_Identity;  
GO  
```  
  
### <a name="b-comparing-identity-values-returned-by-ident_current-identity-and-scope_identity"></a>B. 比较 IDENT_CURRENT、@@IDENTITY 和 SCOPE_IDENTITY 返回的标识值  
 以下示例将显示由 `IDENT_CURRENT`、`@@IDENTITY` 和 `SCOPE_IDENTITY` 返回的不同标识值。  
  
```sql 
USE AdventureWorks2012;  
GO  
IF OBJECT_ID(N't6', N'U') IS NOT NULL   
    DROP TABLE t6;  
GO  
IF OBJECT_ID(N't7', N'U') IS NOT NULL   
    DROP TABLE t7;  
GO  
CREATE TABLE t6(id INT IDENTITY);  
CREATE TABLE t7(id INT IDENTITY(100,1));  
GO  
CREATE TRIGGER t6ins ON t6 FOR INSERT   
AS  
BEGIN  
   INSERT t7 DEFAULT VALUES  
END;  
GO  
--End of trigger definition  
  
SELECT id FROM t6;  
--IDs empty.  
  
SELECT id FROM t7;  
--ID is empty.  
  
--Do the following in Session 1  
INSERT t6 DEFAULT VALUES;  
SELECT @@IDENTITY;  
/*Returns the value 100. This was inserted by the trigger.*/  
  
SELECT SCOPE_IDENTITY();  
/* Returns the value 1. This was inserted by the   
INSERT statement two statements before this query.*/  
  
SELECT IDENT_CURRENT('t7');  
/* Returns value inserted into t7, that is in the trigger.*/  
  
SELECT IDENT_CURRENT('t6');  
/* Returns value inserted into t6. This was the INSERT statement four statements before this query.*/  
  
-- Do the following in Session 2.  
SELECT @@IDENTITY;  
/* Returns NULL because there has been no INSERT action   
up to this point in this session.*/  
  
SELECT SCOPE_IDENTITY();  
/* Returns NULL because there has been no INSERT action   
up to this point in this scope in this session.*/  
  
SELECT IDENT_CURRENT('t7');  
/* Returns the last value inserted into t7.*/  
```  
  
## <a name="see-also"></a>另请参阅  
 [@@IDENTITY (Transact-SQL)](../../t-sql/functions/identity-transact-sql.md)   
 [SCOPE_IDENTITY (Transact-SQL)](../../t-sql/functions/scope-identity-transact-sql.md)   
 [IDENT_INCR (Transact-SQL)](../../t-sql/functions/ident-incr-transact-sql.md)   
 [IDENT_SEED (Transact-SQL)](../../t-sql/functions/ident-seed-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [系统函数 (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
