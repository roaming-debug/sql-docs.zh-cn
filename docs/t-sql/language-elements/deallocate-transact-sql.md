---
description: DEALLOCATE (Transact-SQL)
title: DEALLOCATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DEALLOCATE
- DEALLOCATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], cursors
- DEALLOCATE statement
- deallocations [SQL Server]
- deleting cursor references
- removing cursor references
ms.assetid: c75cf73d-0268-4c57-973d-b8a84ff801fa
author: rothja
ms.author: jroth
ms.openlocfilehash: b29ee9539e8b6d4da64dfcc0da7c2ef9f4296a87
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124452"
---
# <a name="deallocate-transact-sql"></a>DEALLOCATE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  删除游标引用。 当释放最后的游标引用时，组成该游标的数据结构由 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 释放。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DEALLOCATE { { [ GLOBAL ] cursor_name } | @cursor_variable_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 cursor_name  
 已声明游标的名称。 当同时存在以 cursor_name 作为名称的全局游标和局部游标时，如果指定 `GLOBAL`，则 cursor_name 指全局游标，如果未指定 `GLOBAL`，则指局部游标。  
  
 @cursor_variable_name  
 cursor 变量的名称。 @cursor_variable_name 必须为 cursor 类型。  
  
## <a name="remarks"></a>注解  
对游标进行操作的语句使用游标名称或游标变量引用游标。 `DEALLOCATE` 删除游标与游标名称或游标变量之间的关联。 如果一个名称或变量是最后引用游标的名称或变量，则将释放游标，游标使用的任何资源也随之释放。 用于保护提取隔离的滚动锁在 `DEALLOCATE` 上释放。 用于保护更新（包括通过游标进行的定位更新）的事务锁一直到事务结束才释放。  
  
`DECLARE CURSOR` 语句分配游标并将其与游标名称关联。  
  
```sql  
DECLARE abc SCROLL CURSOR FOR  
SELECT * FROM Person.Person;  
```  
  
游标名称与某个游标关联之后，该名称在游标释放之前不能用作相同作用域（GLOBAL 或 LOCAL）内另一个游标的名称。  
  
 游标变量使用下列两种方法之一与游标关联：  
  
-   通过名称，使用 `SET` 语句将游标设置为游标变量。  
  
    ```sql  
    DECLARE @MyCrsrRef CURSOR;  
    SET @MyCrsrRef = abc;  
    ```  
  
-   也可以不定义游标名称而创建游标并将其与变量关联。  
  
    ```sql  
    DECLARE @MyCursor CURSOR;  
    SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Person.Person;  
    ```  
  
 `DEALLOCATE <@cursor_variable_name>` 语句只删除对游标命名变量的引用。 直到批处理、存储过程或触发器结束时变量离开作用域，才释放变量。 在 `DEALLOCATE <@cursor_variable_name>` 语句之后，可以使用 SET 语句将变量与另一个游标关联。  
  
```sql  
USE AdventureWorks2012;  
GO  
  
DECLARE @MyCursor CURSOR;  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Sales.SalesPerson;  
  
DEALLOCATE @MyCursor;  
  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Sales.SalesTerritory;  
GO  
```  
  
不必显式释放游标变量。 变量在离开作用域时被隐式释放。  
  
## <a name="permissions"></a>权限  
 `DEALLOCATE` 语句的权限默认情况下授予任何有效用户。  
  
## <a name="examples"></a>示例  
 以下脚本显示游标如何持续到最后的名称或持续到引用它们的变量已释放。  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create and open a global named cursor that  
-- is visible outside the batch.  
DECLARE abc CURSOR GLOBAL SCROLL FOR  
    SELECT * FROM Sales.SalesPerson;  
OPEN abc;  
GO  
-- Reference the named cursor with a cursor variable.  
DECLARE @MyCrsrRef1 CURSOR;  
SET @MyCrsrRef1 = abc;  
-- Now deallocate the cursor reference.  
DEALLOCATE @MyCrsrRef1;  
-- Cursor abc still exists.  
FETCH NEXT FROM abc;  
GO  
-- Reference the named cursor again.  
DECLARE @MyCrsrRef2 CURSOR;  
SET @MyCrsrRef2 = abc;  
-- Now deallocate cursor name abc.  
DEALLOCATE abc;  
-- Cursor still exists, referenced by @MyCrsrRef2.  
FETCH NEXT FROM @MyCrsrRef2;  
-- Cursor finally is deallocated when last referencing  
-- variable goes out of scope at the end of the batch.  
GO  
-- Create an unnamed cursor.  
DECLARE @MyCursor CURSOR;  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
SELECT * FROM Sales.SalesTerritory;  
-- The following statement deallocates the cursor  
-- because no other variables reference it.  
DEALLOCATE @MyCursor;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CLOSE (Transact-SQL)](../../t-sql/language-elements/close-transact-sql.md)   
 [游标](../../relational-databases/cursors.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [FETCH (Transact-SQL)](../../t-sql/language-elements/fetch-transact-sql.md)   
 [OPEN (Transact-SQL)](../../t-sql/language-elements/open-transact-sql.md)  
  
  
