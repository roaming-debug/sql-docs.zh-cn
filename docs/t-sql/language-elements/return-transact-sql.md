---
description: RETURN (Transact-SQL)
title: RETURN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RETURN_TSQL
- RETURN
dev_langs:
- TSQL
helpviewer_keywords:
- unconditionally exiting program
- stored procedures [SQL Server], exiting
- batches [SQL Server], exiting
- statement blocks [SQL Server]
- queries [SQL Server], exiting
- exiting queries [SQL Server]
- exiting procedures [SQL Server]
- RETURN statement
ms.assetid: 1d9c8247-fd89-4544-be9c-01c95b745db0
author: rothja
ms.author: jroth
ms.openlocfilehash: 837c84bbeda46bf78d670124cc2d33f820fe9891
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92195516"
---
# <a name="return-transact-sql"></a>RETURN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  从查询或过程中无条件退出。 RETURN 的执行是即时且完全的，可在任何时候用于从过程、批处理或语句块中退出。 RETURN 之后的语句是不执行的。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
RETURN [ integer_expression ]   
```  
  
## <a name="arguments"></a>参数  
 *integer_expression*  
 返回的整数值。 存储过程可向执行调用的过程或应用程序返回一个整数值。  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 可以选择返回 int。  
  
> [!NOTE]  
>  除非另外说明，否则所有系统存储过程都将返回一个 0 值。 此值表示成功，非 0 值表示失败。  
  
## <a name="remarks"></a>备注  
 如果用于存储过程，RETURN 不能返回 null 值。 如果某个过程试图返回空值（例如，使用 @status，而 @status 为 NULL），则将生成警告消息并返回 0 值。  
  
 在执行了当前过程的 batch 或过程中，返回状态值可包含在后续 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中，但必须按以下格式输入：`EXECUTE @return_status = <procedure_name>`。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-from-a-procedure"></a>A. 从过程返回  
 下面的示例说明，如果在执行 `findjobs` 时没有指定用户名作为参数，则 `RETURN` 将使过程向用户屏幕发送一条消息后退出。 如果指定了用户名，则将从相应的系统表中检索此用户在当前数据库创建的所有对象名。  
  
```sql  
CREATE PROCEDURE findjobs @nm sysname = NULL  
AS   
IF @nm IS NULL  
    BEGIN  
        PRINT 'You must give a user name'  
        RETURN  
    END  
ELSE  
    BEGIN  
        SELECT o.name, o.id, o.uid  
        FROM sysobjects o INNER JOIN master..syslogins l  
            ON o.uid = l.sid  
        WHERE l.name = @nm  
    END;  
```  
  
### <a name="b-returning-status-codes"></a>B. 返回状态代码  
 以下示例将检查指定联系人的 ID 的状态。 如果所在的州是 Washington (`WA`)，将返回状态代码 `1`。 在其他情况下（`2` 的值是 `WA` 以外的值，或者 `StateProvince` 没有匹配的行），返回状态代码 `ContactID`。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PROCEDURE checkstate @param VARCHAR(11)  
AS  
IF (SELECT StateProvince FROM Person.vAdditionalContactInfo WHERE ContactID = @param) = 'WA'  
    RETURN 1  
ELSE  
    RETURN 2;  
GO  
```  
  
 以下示例显示执行 `checkstate` 返回的状态。 第一个示例显示的联系人在华盛顿；第二个示例显示的联系人不在华盛顿；第三个示例显示的联系人无效。 局部变量 `@return_status` 必须在使用前声明。  
  
```sql  
DECLARE @return_status INT;  
EXEC @return_status = checkstate '2';  
SELECT 'Return Status' = @return_status;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Return Status 
  
 ------------- 
  
 1
 ```  
  
 指定一个不同的联系人编号，再执行一次该查询。  
  
```sql  
DECLARE @return_status INT;  
EXEC @return_status = checkstate '6';  
SELECT 'Return Status' = @return_status;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Return Status  
 -------------  
  
 2
 ```  
  
 指定另一个联系人编号，再执行一次该查询。  
  
```sql  
DECLARE @return_status INT  
EXEC @return_status = checkstate '12345678901';  
SELECT 'Return Status' = @return_status;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Return Status  
 -------------  
  
 2
 ```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER PROCEDURE (Transact-SQL)](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)   
 [SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)   
 [THROW (Transact-SQL)](../../t-sql/language-elements/throw-transact-sql.md)  
  
  
