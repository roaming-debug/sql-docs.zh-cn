---
description: '&#x40;&#x40;IDENTITY (Transact-SQL)'
title: '@@IDENTITY (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@IDENTITY_TSQL'
- '@@IDENTITY'
dev_langs:
- TSQL
helpviewer_keywords:
- last-inserted identity values
- identity values [SQL Server], last-inserted
- '@@IDENTITY function'
ms.assetid: 912e4485-683c-41c2-97b3-8831c0289ee4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a9366f4b63cace1787d2bdb777c9765830b87843
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211453"
---
# <a name="x40x40identity-transact-sql"></a>&#x40;&#x40;IDENTITY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回最后插入的标识值的系统函数。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
@@IDENTITY  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 numeric(38,0)  
  
## <a name="remarks"></a>备注  
 在一条 INSERT、SELECT INTO 或大容量复制语句完成后，@@IDENTITY 中包含语句生成的最后一个标识值。 如果语句未影响任何包含标识列的表，则 @@IDENTITY 返回 NULL。 如果插入了多个行，生成了多个标识值，则 @@IDENTITY 将返回最后生成的标识值。 如果语句触发了一个或多个触发器，该触发器又执行了生成标识值的插入操作，那么，在语句执行后立即调用 @@IDENTITY 将返回触发器生成的最后一个标识值。 如果对包含标识列的表执行插入操作后触发了触发器，并且触发器对另一个没有标识列的表执行了插入操作，则 @@IDENTITY 将返回第一次插入的标识值。 出现 INSERT 或 SELECT INTO 语句失败或大容量复制失败，或者事务被回滚的情况时，@@IDENTITY 值不会恢复为以前的设置。  
  
 如果语句和事务失败，它们会更改表的当前标识，从而使标识列中的值出现不连贯现象。 即使未提交试图向表中插入值的事务，也永远无法回滚标识值。 例如，如果因 IGNORE_DUP_KEY 冲突而导致 INSERT 语句失败，表的当前标识值仍然会增加。  
  
 @@IDENTITY、SCOPE_IDENTITY 和 IDENT_CURRENT 是相似的函数，因为他们都返回插入到表的 IDENTITY 列的最后一个值。  
  
 @@IDENTITY 和 SCOPE_IDENTITY 可以返回当前会话中的所有表中生成的最后一个标识值。 但是，SCOPE_IDENTITY 只在当前作用域内返回值，而 @@IDENTITY 不限于特定的作用域。  
  
 IDENT_CURRENT 不受作用域和会话的限制，而受限于指定的表。 IDENT_CURRENT 可以返回任何会话和任何作用域中为特定表生成的标识值。 有关详细信息，请参阅 [IDENT_CURRENT (Transact-SQL)](../../t-sql/functions/ident-current-transact-sql.md)。  
  
 @@IDENTITY 函数的作用域是执行该函数的本地服务器上的当前会话。 此函数不能应用于远程或链接服务器。 若要获得其他服务器上的标识值，请在远程服务器或链接服务器上执行存储过程，并使（在远程或链接服务器的环境中执行的）该存储过程收集标识值，并将其返回本地服务器上的发出调用的连接。  
  
 复制可能会影响 @@IDENTITY 值，因为该值在复制触发器及存储过程中使用。 如果此列是复制项目的一部分，则 @@IDENTITY 不是最近用户创建的标识的可靠指示器。 你可以使用 SCOPE_IDENTITY() 函数语法代替 @@IDENTITY。 有关详细信息，请参阅 [SET ANSI_NULLS (Transact-SQL)](../../t-sql/functions/scope-identity-transact-sql.md)  
  
> [!NOTE]  
>  必须重新编写调用存储过程或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句才能使用 `SCOPE_IDENTITY()` 函数，该函数会返回在用户语句作用域内所用的最新标识，而不是复制所用的嵌套触发器作用域内的标识。  
  
## <a name="examples"></a>示例  
 以下示例向包含标识列 (`LocationID`) 的表中插入一行，并使用 `@@IDENTITY` 显示新行中使用的标识值。  
  
```sql  
USE AdventureWorks2012;  
GO  
--Display the value of LocationID in the last row in the table.  
SELECT MAX(LocationID) FROM Production.Location;  
GO  
INSERT INTO Production.Location (Name, CostRate, Availability, ModifiedDate)  
VALUES ('Damaged Goods', 5, 2.5, GETDATE());  
GO  
SELECT @@IDENTITY AS 'Identity';  
GO  
--Display the value of LocationID of the newly inserted row.  
SELECT MAX(LocationID) FROM Production.Location;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [System Functions (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [SCOPE_IDENTITY (Transact-SQL)](../../t-sql/functions/scope-identity-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)  
  
  
