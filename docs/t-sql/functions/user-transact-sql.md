---
description: USER (Transact-SQL)
title: USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- USER
- USER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- system-supplied usernames [SQL Server]
- USER function
- users [SQL Server], database names
- names [SQL Server], database users
- database usernames [SQL Server]
ms.assetid: 82bbbd94-870c-4c43-9ed9-d9abc767a6be
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e81222292aa0bae66b96f5864d2ac7957f9c7742
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210644"
---
# <a name="user-transact-sql"></a>USER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  当未指定默认值时，允许将系统为当前用户的数据库用户名提供的值插入表内。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
USER  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 **nvarchar(128)**  
  
## <a name="remarks"></a>注解  
 USER 提供与 USER_NAME 系统函数相同的功能。  
  
 在 CREATE TABLE 或 ALTER TABLE 语句中将 USER 和 DEFAULT 约束一起使用，或者将 USER 作为任何标准函数使用。  
  
 USER 始终返回当前上下文的名称。 在 EXECUTE AS 语句后调用时，USER 将返回模拟上下文的名称。  
  
 如果 Windows 主体通过组中的成员身份访问数据库，USER 将返回 Windows 主体的名称，而不是该组的名称。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-user-to-return-the-database-user-name"></a>A. 使用 USER 返回数据库用户名  
 以下示例声明一个 `char` 类型的变量，将 USER 的当前值赋给它，然后输出该变量以及文本说明。  
  
```sql
DECLARE @usr CHAR(30)  
SET @usr = user  
SELECT 'The current user''s database username is: '+ @usr  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
-----------------------------------------------------------------------  
The current user's database username is: dbo  
  
(1 row(s) affected)
```  
  
### <a name="b-using-user-with-default-constraints"></a>B. 将 USER 与 DEFAULT 约束一起使用  
 以下示例生成一个表，将 `USER` 用作销售行的销售员的 `DEFAULT` 约束。  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE inventory22  
(  
 part_id INT IDENTITY(100, 1) NOT NULL,  
 description VARCHAR(30) NOT NULL,  
 entry_person VARCHAR(30) NOT NULL DEFAULT USER   
)  
GO  
INSERT inventory22 (description)  
VALUES ('Red pencil')  
INSERT inventory22 (description)  
VALUES ('Blue pencil')  
INSERT inventory22 (description)  
VALUES ('Green pencil')  
INSERT inventory22 (description)  
VALUES ('Black pencil')  
INSERT inventory22 (description)  
VALUES ('Yellow pencil')  
GO  
```  
  
 下面是从表 `inventory22` 中选择所有信息的查询：  
  
```sql
SELECT * FROM inventory22 ORDER BY part_id;  
GO  
```  
  
 下面是结果集（注意 `entry-person` 的值）：  
  
 ```
part_id     description                    entry_person
----------- ------------------------------ -------------------------
100         Red pencil                     dbo
101         Blue pencil                    dbo
102         Green pencil                   dbo
103         Black pencil                   dbo
104         Yellow pencil                  dbo
  
(5 row(s) affected)
```  
  
### <a name="c-using-user-in-combination-with-execute-as"></a>C. 将 USER 与 EXECUTE AS 一起使用  
 以下示例演示了 `USER` 在模拟会话内部被调用时的行为。  
  
```sql
SELECT USER;  
GO  
EXECUTE AS USER = 'Mario';  
GO  
SELECT USER;  
GO  
REVERT;  
GO  
SELECT USER;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
DBO
Mario
DBO
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP (Transact-SQL)](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER (Transact-SQL)](../../t-sql/functions/current-user-transact-sql.md)   
 [安全函数 (Transact-SQL)](../../t-sql/functions/security-functions-transact-sql.md)   
 [SESSION_USER (Transact-SQL)](../../t-sql/functions/session-user-transact-sql.md)   
 [SYSTEM_USER (Transact-SQL)](../../t-sql/functions/system-user-transact-sql.md)   
 [USER_NAME (Transact-SQL)](../../t-sql/functions/user-name-transact-sql.md)  
  
  

