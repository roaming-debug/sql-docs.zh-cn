---
description: SCOPE_IDENTITY (Transact-SQL)
title: SCOPE_IDENTITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SCOPE_IDENTITY
- SCOPE_IDENTITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- identity columns [SQL Server], SCOPE_IDENTITY function
- SCOPE_IDENTITY function
- last-inserted identity values
- identity values [SQL Server], last-inserted
ms.assetid: eef24670-059b-4f10-91d4-a67bc1ed12ab
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: bb49f217e40b13b106743da61169eaf35a58ef1c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183459"
---
# <a name="scope_identity-transact-sql"></a>SCOPE_IDENTITY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回插入到同一作用域中的标识列内的最后一个标识值。 一个范围是一个模块：存储过程、触发器、函数或批处理。 因此，如果两个语句处于同一个存储过程、函数或批处理中，则它们位于相同的作用域中。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
SCOPE_IDENTITY()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 numeric(38,0)  
  
## <a name="remarks"></a>备注  
 SCOPE_IDENTITY、IDENT_CURRENT 和 @@IDENTITY 是相似的函数，因为它们都返回插入到标识列中的值。  
  
 IDENT_CURRENT 不受作用域和会话的限制，而受限于指定的表。 IDENT_CURRENT 返回为任何会话和作用域中的特定表所生成的值。 有关详细信息，请参阅 [IDENT_CURRENT (Transact-SQL)](../../t-sql/functions/ident-current-transact-sql.md)。  
  
 SCOPE_IDENTITY 和 @@IDENTITY 返回在当前会话中的任何表内所生成的最后一个标识值。 但是，SCOPE_IDENTITY 只返回插入到当前作用域中的值；@@IDENTITY 不受限于特定的作用域。  
  
 例如，有两个表 T1 和 T2，并且在 T1 上定义了 INSERT 触发器。 当将某行插入 T1 时，触发器被激发，并在 T2 中插入一行。 该方案演示了两个作用域：在 T1 上的插入，以及在 T2 通过触发器的插入。  
  
 假设 T1 和 T2 都有标识列，@@IDENTITY 和 SCOPE_IDENTITY 将在 T1 上的 INSERT 语句的最后返回不同的值。 @@IDENTITY 返回在当前会话中的任何作用域内插入的最后一个标识列的值。 这是在 T2 中插入的值。 SCOPE_IDENTITY() 返回在 T1 中插入的 IDENTITY 值。 这是在同一个作用域内发生的最后的插入。 如果在任何 INSERT 语句作用于作用域中的标识列之前调用 SCOPE_IDENTITY() 函数，则该函数返回 NULL。  
  
 如果语句和事务失败，它们会更改表的当前标识，从而使标识列中的值出现不连贯现象。 即使未提交试图向表中插入值的事务，也永远无法回滚标识值。 例如，如果因 IGNORE_DUP_KEY 冲突而导致 INSERT 语句失败，表的当前标识值仍然会增加。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-identity-and-scope_identity-with-triggers"></a>A. 将 @@IDENTITY 和 SCOPE_IDENTITY 用于触发器  
 下面的示例创建两个表，`TZ` 和 `TY`，并对 `TZ` 创建一个 INSERT 触发器。 当将某行插入表 `TZ` 中时，触发器 (`Ztrig`) 将激发并在 `TY` 中插入一行。  
  
```sql  
USE tempdb;  
GO  
CREATE TABLE TZ (  
   Z_id  INT IDENTITY(1,1)PRIMARY KEY,  
   Z_name VARCHAR(20) NOT NULL);  
  
INSERT TZ  
   VALUES ('Lisa'),('Mike'),('Carla');  
  
SELECT * FROM TZ;  
```     
结果集：表 TZ 如下所示。  
  
```  
Z_id   Z_name  
-------------  
1      Lisa  
2      Mike  
3      Carla  
```  
```sql 
CREATE TABLE TY (  
   Y_id  INT IDENTITY(100,5)PRIMARY KEY,  
   Y_name VARCHAR(20) NULL);  
  
INSERT TY (Y_name)  
   VALUES ('boathouse'), ('rocks'), ('elevator');  
  
SELECT * FROM TY;  
```   
结果集：TY 如下所示：  
```  
Y_id  Y_name  
---------------  
100   boathouse  
105   rocks  
110   elevator  
```  

创建一个触发器，在表 TZ 中插入行时，该触发器会在表 TY 中插入行。  
```sql  
CREATE TRIGGER Ztrig  
ON TZ  
FOR INSERT AS   
   BEGIN  
   INSERT TY VALUES ('')  
   END;  
```  
触发该触发器，并确定使用 @@IDENTITY 和 SCOPE_IDENTITY 函数得到的值。   
```sql
INSERT TZ VALUES ('Rosalie');  
  
SELECT SCOPE_IDENTITY() AS [SCOPE_IDENTITY];  
GO  
SELECT @@IDENTITY AS [@@IDENTITY];  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
```
/*SCOPE_IDENTITY returns the last identity value in the same scope. This was the insert on table TZ.*/`  
SCOPE_IDENTITY  
4  

/*@@IDENTITY returns the last identity value inserted to TY by the trigger. 
  This fired because of an earlier insert on TZ.*/
@@IDENTITY  
115  
```  
  
### <a name="b-using-identity-and-scope_identity-with-replication"></a>B. 将 @@IDENTITY 和 SCOPE_IDENTITY() 用于复制  
 下面的示例说明如何针对为合并复制发布的数据库中的插入内容使用 `@@IDENTITY` 和 `SCOPE_IDENTITY()`。 示例中的两个表都在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库中，其中 `Person.ContactType` 未发布，`Sales.Customer` 已发布。 合并复制将把触发器添加到已发布的表中。 因此，`@@IDENTITY` 可以从复制系统表中的插入内容而非用户表中的插入内容返回值。  
  
 `Person.ContactType` 表的最大标识值为 20。 如果在该表中插入一行，`@@IDENTITY` 和 `SCOPE_IDENTITY()` 将返回相同的值。  
  
```sql  
USE AdventureWorks2012;  
GO  
INSERT INTO Person.ContactType ([Name]) VALUES ('Assistant to the Manager');  
GO  
SELECT SCOPE_IDENTITY() AS [SCOPE_IDENTITY];  
GO  
SELECT @@IDENTITY AS [@@IDENTITY];  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
```  
SCOPE_IDENTITY  
21  
@@IDENTITY  
21
```  
  
 `Sales.Customer` 表的最大标识值为 29483。 如果在该表中插入一行，`@@IDENTITY` 和 `SCOPE_IDENTITY()` 将返回不同的值。 `SCOPE_IDENTITY()` 从用户表的插入内容返回值，而 `@@IDENTITY` 从复制系统表中的插入内容返回值。 请对需要访问插入的标识值的应用程序使用 `SCOPE_IDENTITY()`。  
  
```sql  
INSERT INTO Sales.Customer ([TerritoryID],[PersonID]) VALUES (8,NULL);  
GO  
SELECT SCOPE_IDENTITY() AS [SCOPE_IDENTITY];  
GO  
SELECT @@IDENTITY AS [@@IDENTITY];  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
 ```
 SCOPE_IDENTITY  
 29484  
 @@IDENTITY  
 89
 ```  
  
## <a name="see-also"></a>另请参阅  
 [@@IDENTITY (Transact-SQL)](../../t-sql/functions/identity-transact-sql.md)  
  
  
