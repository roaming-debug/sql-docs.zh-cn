---
description: USER_NAME (Transact-SQL)
title: USER_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- USER_NAME
- USER_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- IDs [SQL Server], databases
- USER_NAME function
- users [SQL Server], database username
- names [SQL Server], database users
- identification numbers [SQL Server], databases
- database usernames [SQL Server]
ms.assetid: ab32d644-4228-449a-9ef0-5a975c305775
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 13c27b4f23e6361592a72082c94fb033a96ce0d7
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "90570671"
---
# <a name="user_name-transact-sql"></a>USER_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  根据指定的标识号返回数据库用户名。  
  
 ![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
USER_NAME ( [ id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *id*  
 与数据库用户关联的标识号。 id 的数据类型为 int。需要使用括号。  
  
## <a name="return-types"></a>返回类型  
 **nvarchar(128)**  
  
## <a name="remarks"></a>注解  
 省略 id 时，则假定为当前上下文中的当前用户。 如果此参数包含词 NULL，将返回 NULL。 如果在 EXECUTE AS 语句之后调用 USER_NAME，但不指定 id，则 USER_NAME 将返回模拟用户的名称。 如果 Windows 主体通过某组中的成员身份访问数据库，则 USER_NAME 将返回 Windows 主体的名称，而不是该组的名称。  
 
> [!NOTE]
> 尽管 Azure SQL 数据库支持 USER_NAME，但 Azure SQL 数据库不支持使用 USER_NAME 的 Execute as。 
  
## <a name="examples"></a>示例  
  
### <a name="a-using-user_name"></a>A. 使用 USER_NAME  
 以下示例将返回用户 ID 为 `13` 的用户名。  
  
```sql  
SELECT USER_NAME(13);  
GO  
```  
  
### <a name="b-using-user_name-without-an-id"></a>B. 使用不指定 ID 的 USER_NAME  
 以下示例在不指定 ID 的情况下查找当前用户的名称。  
  
```sql  
SELECT USER_NAME();  
GO  
```  
  
 下面是为属于 sysadmin 固定服务器角色成员的用户返回的结果集。  
  
 ```
------------------------------  
dbo  
  
(1 row(s) affected)
```  
  
### <a name="c-using-user_name-in-the-where-clause"></a>C. 在 WHERE 子句中使用 USER_NAME  
 以下示例在 `sysusers` 中查找行，该行的名称与将 `USER_NAME` 系统函数应用于用户标识号 `1` 而得出的结果相同。  
  
```sql  
SELECT name FROM sysusers WHERE name = USER_NAME(1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
name  
------------------------------  
dbo  
  
(1 row(s) affected)
```  
  
### <a name="d-calling-user_name-during-impersonation-with-execute-as"></a>D. 在使用 EXECUTE AS 的模拟过程中调用 USER_NAME  
 以下示例显示模拟过程中 `USER_NAME` 的行为方式。  
  
```sql  
SELECT USER_NAME();  
GO  
EXECUTE AS USER = 'Zelig';  
GO  
SELECT USER_NAME();  
GO  
REVERT;  
GO  
SELECT USER_NAME();  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
DBO  
Zelig  
DBO
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-user_name-without-an-id"></a>E. 使用不指定 ID 的 USER_NAME  
 以下示例在不指定 ID 的情况下查找当前用户的名称。  
  
```sql  
SELECT USER_NAME();  
```  
  
 下面是当前登录用户的结果集。  
  
```  
------------------------------   
User7                              
```  
  
### <a name="f-using-user_name-in-the-where-clause"></a>F. 在 WHERE 子句中使用 USER_NAME  
 以下示例在 `sysusers` 中查找行，该行的名称与将 `USER_NAME` 系统函数应用于用户标识号 `1` 而得出的结果相同。  
  
```sql  
SELECT name FROM sysusers WHERE name = USER_NAME(1);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
name                             
------------------------------   
User7                              
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP (Transact-SQL)](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER (Transact-SQL)](../../t-sql/functions/current-user-transact-sql.md)   
 [SESSION_USER (Transact-SQL)](../../t-sql/functions/session-user-transact-sql.md)   
 [System Functions (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [SYSTEM_USER (Transact-SQL)](../../t-sql/functions/system-user-transact-sql.md)  
  
