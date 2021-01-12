---
description: ERROR_LINE (Transact-SQL)
title: ERROR_LINE (Transact-SQL)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ERROR_LINE
- ERROR_LINE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- errors [SQL Server], line number
- messages [SQL Server], line number
- TRY...CATCH [SQL Server]
- line number of error [SQL Server]
- ERROR_LINE function
- CATCH block
ms.assetid: 47335734-0baf-45a6-8b3b-6c4fd80d2cb8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 44402d9f0af858381041e9f5bb5ff989971f44f1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097177"
---
# <a name="error_line-transact-sql"></a>ERROR_LINE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函数返回出现错误的行号，该错误导致执行了 TRY…CATCH 构造的 CATCH 块。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法

```syntaxsql
ERROR_LINE ( )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>返回类型
**int**

## <a name="return-value"></a>返回值  
在 CATCH 块中调用时，`ERROR_LINE` 返回  
  
-   出现错误的行号    
-   如果在存储过程或触发器中出现错误，则返回例程中的行号  
-   如果在 CATCH 块作用域外调用，则返回 NULL。  
  
## <a name="remarks"></a>备注  
可在 CATCH 块作用域内的任意位置调用 `ERROR_LINE`。  
  
`ERROR_LINE` 返回出现错误的行号。 无论在 CATCH 块作用域内的任何位置调用 `ERROR_LINE`，以及无论调用 `ERROR_LINE` 多少次，都会发生这种情况。 这与函数不同，例如 @@ERROR。 @@ERROR 在导致错误的语句的后一个语句中或 CATCH 块的第一个语句中返回错误号。  
  
在嵌套 CATCH 块中，`ERROR_LINE` 返回特定于引用它的 CATCH 块的作用域的错误行号。 例如，TRY…CATCH 构造的 CATCH 块可以包含一个嵌套 TRY…CATCH 构造。 在嵌套 CATCH 块中，`ERROR_LINE` 返回调用嵌套 CATCH 块的错误的行号。 如果 `ERROR_LINE` 在 CATCH 块以外运行，则会返回调用该特定 CATCH 块的错误的行号。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-error_line-in-a-catch-block"></a>A. 在 CATCH 块中使用 ERROR_LINE  
下面的代码示例显示生成被零除错误的 `SELECT` 语句。 `ERROR_LINE` 返回出现错误的行号。  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_LINE() AS ErrorLine;  
END CATCH;  
GO  
```  
  
### <a name="b-using-error_line-in-a-catch-block-with-a-stored-procedure"></a>B. 带存储过程在 CATCH 块中使用 ERROR_LINE  
下面的示例显示生成被零除错误的存储过程。 `ERROR_LINE` 返回出现错误的行号。  
  
```sql  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_ExampleProc', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that  
-- generates a divide-by-zero error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT 1/0;  
GO  
  
BEGIN TRY  
    -- Execute the stored procedure inside the TRY block.  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_LINE() AS ErrorLine;  
END CATCH;  
GO  
``` 


### <a name="c-using-error_line-in-a-catch-block-with-other-error-handling-tools"></a>C. 带其他错误处理工具在 CATCH 块中使用 ERROR_LINE  
下面的代码示例显示生成被零除错误的 `SELECT` 语句。 `ERROR_LINE` 返回出现错误的行号，以及与错误本身相关的信息。  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_LINE() AS ErrorLine,  
        ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
``` 
  
## <a name="see-also"></a>另请参阅  
 [TRY...CATCH (Transact-SQL)](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [ERROR_NUMBER (Transact-SQL)](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY (Transact-SQL)](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE (Transact-SQL&)](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR (Transact-SQL)](../../t-sql/functions/error-transact-sql.md)  
  
  
