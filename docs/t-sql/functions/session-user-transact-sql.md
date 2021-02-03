---
description: SESSION_USER (Transact-SQL)
title: SESSION_USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SESSION_USER_TSQL
- SESSION_USER
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- current user names
- sessions [SQL Server], user names
- displaying user names
- viewing user names
- SESSION_USER function
ms.assetid: 3dbe8532-31b6-4862-8b2a-e58b00b964de
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ff931bf49d34f90c7572b0745c914ad457ed5e81
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159307"
---
# <a name="session_user-transact-sql"></a>SESSION_USER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  SESSION_USER 返回当前数据库中当前上下文的用户名。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
SESSION_USER  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 **nvarchar(128)**  
  
## <a name="remarks"></a>注解  
 SESSION_USER 可在 CREATE TABLE 或 ALTER TABLE 语句中与 DEFAULT 约束一起使用，或者将它用作任何标准函数。 如果没有指定默认值，可以将 SESSION_USER 插入表中。 此函数没有参数。 SESSION_USER 可以在查询中使用。  
  
 如果在切换上下文之后调用 SESSION_USER，SESSION_USER 将返回模拟上下文的用户名。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-session_user-to-return-the-user-name-of-the-current-session"></a>A. 使用 SESSION_USER 返回当前会话的用户名  
 以下示例将变量声明为 `nchar`，然后将当前值 `SESSION_USER` 分配给该变量，再与文本说明一起打印此变量。  
  
```sql  
DECLARE @session_usr NCHAR(30);  
SET @session_usr = SESSION_USER;  
SELECT 'This session''s current user is: '+ @session_usr;  
GO  
```  
  
 这是会话用户为 `Surya` 时的结果集：  
  
 ```
--------------------------------------------------------------
This session's current user is: Surya

(1 row(s) affected)
```  
  
### <a name="b-using-session_user-with-default-constraints"></a>B. 与 DEFAULT 约束一起使用 SESSION_USER  
 以下示例创建一个表，该表使用 `SESSION_USER` 作为记录发货回执者的名字的 `DEFAULT` 约束。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TABLE deliveries3  
(  
 order_id INT IDENTITY(5000, 1) NOT NULL,  
 cust_id  INT NOT NULL,  
 order_date SMALLDATETIME NOT NULL DEFAULT GETDATE(),  
 delivery_date SMALLDATETIME NOT NULL DEFAULT   
    DATEADD(dd, 10, GETDATE()),  
 received_shipment NCHAR(30) NOT NULL DEFAULT SESSION_USER  
);  
GO  
```  
  
 添加到表中的记录将以当前用户的用户名为戳记。 在此示例中，`Wanida`、`Sylvester` 和 `Alejandro` 将验证发货的回执。 这可以通过使用 `EXECUTE AS` 来切换用户上下文进行模拟。  
  
```sql
EXECUTE AS USER = 'Wanida'  
INSERT deliveries3 (cust_id)  
VALUES (7510);  
INSERT deliveries3 (cust_id)  
VALUES (7231);  
REVERT;  
EXECUTE AS USER = 'Sylvester'  
INSERT deliveries3 (cust_id)  
VALUES (7028);  
REVERT;  
EXECUTE AS USER = 'Alejandro'  
INSERT deliveries3 (cust_id)  
VALUES (7392);  
INSERT deliveries3 (cust_id)  
VALUES (7452);  
REVERT;  
GO  
```  
  
 下面的查询从 `deliveries3` 表中选择所有信息。  
  
```sql
SELECT order_id AS 'Order #', cust_id AS 'Customer #',   
   delivery_date AS 'When Delivered', received_shipment   
   AS 'Received By'  
FROM deliveries3  
ORDER BY order_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Order #   Customer #  When Delivered       Received By
--------  ----------  -------------------  -----------
5000      7510        2005-03-16 12:02:14  Wanida
5001      7231        2005-03-16 12:02:14  Wanida
5002      7028        2005-03-16 12:02:14  Sylvester
5003      7392        2005-03-16 12:02:14  Alejandro
5004      7452        2005-03-16 12:02:14  Alejandro

(5 row(s) affected)
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-session_user-to-return-the-user-name-of-the-current-session"></a>C：使用 SESSION_USER 返回当前会话的用户名  
 以下示例返回当前会话的会话用户。  
  
```sql
SELECT SESSION_USER;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP (Transact-SQL)](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER (Transact-SQL)](../../t-sql/functions/current-user-transact-sql.md)   
 [SYSTEM_USER (Transact-SQL)](../../t-sql/functions/system-user-transact-sql.md)   
 [System Functions (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [USER (Transact-SQL)](../../t-sql/functions/user-transact-sql.md)   
 [USER_NAME (Transact-SQL)](../../t-sql/functions/user-name-transact-sql.md)  
  
  

