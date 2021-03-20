---
description: CURRENT_USER (Transact-SQL)
title: CURRENT_USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CURRENT_USER
- CURRENT_USER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- current user names
- niladic functions
- CURRENT_USER
- users [SQL Server], names
ms.assetid: 29248949-325b-4063-9f55-5a445fb35c6e
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bb311c186b88853cf378f6fae45fc868e965611b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749947"
---
# <a name="current_user-transact-sql"></a>CURRENT_USER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数返回当前用户的名称。 此函数等效于 `USER_NAME()`。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CURRENT_USER  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
**sysname**
  
## <a name="remarks"></a>备注  
`CURRENT_USER` 返回当前安全上下文的名称。 如果在 `EXECUTE AS` 的调用切换上下文后执行 `CURRENT_USER`，`CURRENT_USER` 将返回被模拟上下文的名称。 如果 Windows 主体通过某组中的成员身份访问数据库，则 `CURRENT_USER` 将返回 Windows 主体的名称，而不是该组名。
  
若要了解如何返回当前用户的登录名，请参阅 [SUSER_NAME (Transact-SQL)](../../t-sql/functions/suser-name-transact-sql.md) 和 [SYSTEM_USER (Transact-SQL)](../../t-sql/functions/system-user-transact-sql.md)。
  
## <a name="examples"></a>示例  
  
### <a name="a-using-current_user-to-return-the-current-user-name"></a>A. 使用 CURRENT_USER 返回当前用户名称  
此示例返回当前用户的名称。
  
```sql
SELECT CURRENT_USER;  
GO  
```  
  
### <a name="b-using-current_user-as-a-default-constraint"></a>B. 使用 CURRENT_USER 作为 DEFAULT 约束  
此示例创建的表使用 `CURRENT_USER` 作为销售行上 `DEFAULT` 列的 `order_person` 约束。
  
```sql
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
      WHERE TABLE_NAME = 'orders22')  
   DROP TABLE orders22;  
GO  
SET NOCOUNT ON;  
CREATE TABLE orders22  
(  
order_id int IDENTITY(1000, 1) NOT NULL,
cust_id  int NOT NULL,
order_date smalldatetime NOT NULL DEFAULT GETDATE(),
order_amt money NOT NULL,
order_person char(30) NOT NULL DEFAULT CURRENT_USER
);  
GO  
```  
  
此示例在表中插入记录。 名为 `Wanida` 的用户执行这些语句。
  
```sql
INSERT orders22 (cust_id, order_amt)  
VALUES (5105, 577.95);  
GO  
SET NOCOUNT OFF;  
GO  
```  
  
此查询从 `orders22` 表中选择所有信息。
  
```sql
SELECT * FROM orders22;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
order_id    cust_id     order_date           order_amt    order_person
----------- ----------- -------------------- ------------ ------------
1000        5105        2005-04-03 23:34:00  577.95       Wanida
  
(1 row(s) affected)
```
  
### <a name="c-using-current_user-from-an-impersonated-context"></a>C. 从模拟上下文使用 CURRENT_USER  
在此示例中，用户 `Wanida` 执行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码来模拟用户“Arnalfo”。
  
```sql
SELECT CURRENT_USER;  
GO  
EXECUTE AS USER = 'Arnalfo';  
GO  
SELECT CURRENT_USER;  
GO  
REVERT;  
GO  
SELECT CURRENT_USER;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Wanida
Arnalfo
Wanida
```
  
## <a name="see-also"></a>请参阅
[USER_NAME (Transact-SQL)](../../t-sql/functions/user-name-transact-sql.md)  
[SYSTEM_USER (Transact-SQL)](../../t-sql/functions/system-user-transact-sql.md)  
[sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[系统函数 (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)
  
  

