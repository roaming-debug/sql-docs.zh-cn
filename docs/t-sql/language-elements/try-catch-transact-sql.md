---
description: TRY...CATCH (Transact-SQL)
title: TRY...CATCH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BEGIN_TRY_TSQL
- BEGIN_CATCH_TSQL
- TRY
- BEGIN TRY
- TRY_TSQL
- BEGIN CATCH
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN CATCH statement
- uncommittable transactions
- errors [SQL Server], TRY...CATCH
- TRY block [SQL Server]
- XACT_STATE function
- TRY...CATCH [SQL Server]
- BEGIN TRY statement
- CATCH block
ms.assetid: 248df62a-7334-4bca-8262-235a28f4b07f
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6d31f8951dbf878a6ae7b6895fb060af91ad1c27
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095959"
---
# <a name="trycatch-transact-sql"></a>TRY...CATCH (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


  对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 实现与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# 和 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C++ 语言中的异常处理类似的错误处理。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句组可以包含在 TRY 块中。 如果 TRY 块内部发生错误，则会将控制传递给 CATCH 块中包含的另一个语句组。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
BEGIN TRY  
     { sql_statement | statement_block }  
END TRY  
BEGIN CATCH  
     [ { sql_statement | statement_block } ]  
END CATCH  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 sql_statement   
 任何 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
 statement_block   
 批处理或包含于 BEGIN…END 块中的任何 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句组。  
  
## <a name="remarks"></a>备注  
 TRY…CATCH 构造可对严重程度高于 10 但不关闭数据库连接的所有执行错误进行缓存。  
  
 TRY 块后必须紧跟相关联的 CATCH 块。 在 END TRY 和 BEGIN CATCH 语句之间放置任何其他语句都将生成语法错误。  
  
 TRY…CATCH 构造不能跨越多个批处理。 TRY…CATCH 构造不能跨越多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句块。 例如，TRY…CATCH 构造不能跨越 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的两个 BEGIN…END 块，且不能跨越 IF…ELSE 构造。  
  
 如果 TRY 块所包含的代码中没有错误，则当 TRY 块中最后一个语句完成运行时，会将控制传递给紧跟在相关联的 END CATCH 语句之后的语句。
 
 如果 TRY 块所包含的代码中有错误，则会将控制传递给相关联的 CATCH 块的第一个语句。 当 CATCH 块中的代码完成时，会将控制传递给紧跟在 END CATCH 语句之后的语句。 
 
 > [!NOTE] 
 > 如果 END CATCH 语句是存储过程或触发器的最后一个语句，控制将回到调用该存储过程或运行该触发器的语句。 
 
 由 CATCH 块捕获的错误不会返回到调用应用程序。 如果错误消息的任何部分都必须返回到应用程序，则 CATCH 块中的代码必须使用 SELECT 结果集或 RAISERROR 和 PRINT 语句之类的机制执行此操作。  
  
 TRY…CATCH 构造可以是嵌套式的。 TRY 块或 CATCH 块均可包含嵌套的 TRY…CATCH 构造。 例如，CATCH 块可以包含内嵌的 TRY…CATCH 构造，以处理 CATCH 代码所遇到的错误。  
  
 处理 CATCH 块中遇到的错误的方法与处理任何其他位置生成的错误一样。 如果 CATCH 块包含嵌套的 TRY…CATCH 构造，则嵌套的 TRY 块中的任何错误都会将控制传递给嵌套的 CATCH 块。 如果没有嵌套的 TRY…CATCH 构造，则会将错误传递回调用方。  
  
 TRY…CATCH 构造可以从存储过程或触发器（由 TRY 块中的代码执行）捕捉未处理的错误。 或者，存储过程或触发器也可以包含其自身的 TRY…CATCH 构造，以处理由其代码生成的错误。 例如，当 TRY 块执行存储过程且存储过程中发生错误时，可以使用以下方式处理错误：  
  
-   如果存储过程不包含自己的 TRY…CATCH 构造，错误会将控制返回到与包含 EXECUTE 语句的 TRY 块相关联的 CATCH 块。  
  
-   如果存储过程包含 TRY…CATCH 构造，则错误会将控制传输给存储过程中的 CATCH 块。 当 CATCH 块代码完成时，控制会传递回调用存储过程的 EXECUTE 语句之后的语句。  
  
 不能使用 GOTO 语句输入 TRY 或 CATCH 块， 使用 GOTO 语句可以跳转至同一 TRY 或 CATCH 块内的某个标签，或离开 TRY 或 CATCH 块。  
  
 不能在用户定义函数内使用 TRY…CATCH 构造。  
  
## <a name="retrieving-error-information"></a>检索错误信息  
 在 CATCH 块的作用域内，可以使用以下系统函数来获取导致 CATCH 块执行的错误消息：  
  
-   [ERROR_NUMBER()](../../t-sql/functions/error-number-transact-sql.md) 返回错误编号。  
  
-   [ERROR_SEVERITY()](../../t-sql/functions/error-severity-transact-sql.md) 返回严重性。  
  
-   [ERROR_STATE()](../../t-sql/functions/error-state-transact-sql.md) 返回错误状态号。  
  
-   [ERROR_PROCEDURE()](../../t-sql/functions/error-procedure-transact-sql.md) 返回出现错误的存储过程或触发器的名称。  
  
-   [ERROR_LINE()](../../t-sql/functions/error-line-transact-sql.md) 返回导致错误的例程中的行号。  
  
-   [ERROR_MESSAGE()](../../t-sql/functions/error-message-transact-sql.md) 返回错误消息的完整文本。 该文本包括为所有可替换参数提供的值，如长度、对象名或时间。  
  
如果是在 CATCH 块的作用域之外调用这些函数，则这些函数返回空值。 可以从 CATCH 块作用域内的任何位置使用这些函数检索错误消息。 例如，下面的脚本显示了包含错误处理函数的存储过程。 在 `CATCH` 构造的 `TRY...CATCH` 块中，调用了该存储过程并返回有关错误的信息。  
  
```sql  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_GetErrorInfo', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_GetErrorInfo;  
GO  
  
-- Create procedure to retrieve error information.  
CREATE PROCEDURE usp_GetErrorInfo  
AS  
SELECT  
    ERROR_NUMBER() AS ErrorNumber  
    ,ERROR_SEVERITY() AS ErrorSeverity  
    ,ERROR_STATE() AS ErrorState  
    ,ERROR_PROCEDURE() AS ErrorProcedure  
    ,ERROR_LINE() AS ErrorLine  
    ,ERROR_MESSAGE() AS ErrorMessage;  
GO  
  
BEGIN TRY  
    -- Generate divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    -- Execute error retrieval routine.  
    EXECUTE usp_GetErrorInfo;  
END CATCH;   
```  
  
 ERROR\_\* 函数也适用于[本机编译的存储过程](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md)内的 `CATCH` 块。  
  
## <a name="errors-unaffected-by-a-trycatch-construct"></a>不受 TRY…CATCH 构造影响的错误  
 TRY…CATCH 构造在下列情况下不捕获错误：  
  
-   严重级别为 10 或更低的警告或信息性消息。  
  
-   严重级别为 20 或更高且终止会话的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]任务处理的错误。 如果所发生错误的严重级别为 20 或更高，而数据库连接未中断，则 TRY…CATCH 将处理该错误。  
  
-   需要关注的消息，如客户端中断请求或客户端连接中断。  
  
-   当系统管理员使用 KILL 语句终止会话时。  
  
如果以下类型的错误的发生级别与 TRY…CATCH 构造的执行等级相同，则 CATCH 块不会处理这些错误：  
  
-   编写错误，例如禁止运行批处理的语法错误。  
  
-   语句级重新编写过程中出现的错误，例如由于名称解析延迟而造成在编写后出现对象名解析错误。  
-   对象名解析错误   

  
这些错误会被返回到运行批处理、存储过程或触发器的级别。  
  
如果某个错误在 TRY 块内的编写或语句级别重新编写过程中并在较低的执行级别（例如，执行 sp_executesql 或用户定义存储过程时）发生，则该错误会在低于 TRY…CATCH 构造的级别上发生，并由相关联的 CATCH 块处理。  
  
以下示例显示在存储过程中执行相同的 `SELECT` 语句时，由 `TRY...CATCH` 语句生成的对象名解析错误是如何不被 `CATCH` 构造捕捉，却被 `SELECT` 块捕捉的。  
  
```sql  
BEGIN TRY  
    -- Table does not exist; object name resolution  
    -- error not caught.  
    SELECT * FROM NonexistentTable;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
       ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH  
```  
  
 未捕捉错误，控制从 `TRY...CATCH` 构造传递给下一更高级别。  
  
 在存储过程内运行 `SELECT` 语句将导致错误在低于 `TRY` 块的级别上发生。 该错误将由 `TRY...CATCH` 构造处理。  
  
```sql  
-- Verify that the stored procedure does not exist.  
IF OBJECT_ID ( N'usp_ExampleProc', N'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that will cause an   
-- object resolution error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT * FROM NonexistentTable;  
GO  
  
BEGIN TRY  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
```  
  
## <a name="uncommittable-transactions-and-xact_state"></a>不可提交的事务和 XACT_STATE  
 如果 TRY 块内生成的错误导致当前事务的状态失效，则将该事务归类为不可提交的事务。 如果通常在 TRY 块外中止事务的错误在 TRY 内发生时，就会导致事务进入不可提交状态。 不可提交的事务只能执行读操作或 ROLLBACK TRANSACTION。 该事务不能执行任何可能生成写操作或 COMMIT TRANSACTION 的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 如果事务被分类为不可提交的事务，则 XACT_STATE 函数会返回值 -1。 当批处理结束时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将回滚所有不可提交的活动事务。 如果事务进入不可提交状态时未发送错误消息，则当批处理结束时，将向客户端应用程序发送一个错误消息。 该消息指示检测到并回滚了一个不可提交的事务。  
  
 有关不可提交的事务和 XACT_STATE 函数的详细信息，请参阅 [XACT_STATE (Transact-SQL)](../../t-sql/functions/xact-state-transact-sql.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-trycatch"></a>A. 使用 TRY...CATCH  
 以下示例显示一个会生成被零除错误的 `SELECT` 语句。 该错误会使执行跳转到关联的 `CATCH` 块。  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
### <a name="b-using-trycatch-in-a-transaction"></a>B. 在事务内使用 TRY…CATCH  
 以下示例显示 `TRY...CATCH` 块在事务内的工作方式。 `TRY` 块内的语句会生成违反约束的错误。  
  
```sql  
BEGIN TRANSACTION;  
  
BEGIN TRY  
    -- Generate a constraint violation error.  
    DELETE FROM Production.Product  
    WHERE ProductID = 980;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
  
    IF @@TRANCOUNT > 0  
        ROLLBACK TRANSACTION;  
END CATCH;  
  
IF @@TRANCOUNT > 0  
    COMMIT TRANSACTION;  
GO  
```  
  
### <a name="c-using-trycatch-with-xact_state"></a>C. 将 TRY…CATCH 与 XACT_STATE 配合使用  
 以下示例显示如何使用 `TRY...CATCH` 构造来处理事务内发生的错误。 `XACT_STATE` 函数确定应提交事务还是应回滚事务。 在本示例中，`SET XACT_ABORT` 状态为 `ON`。 在发生违反约束的错误时，这会使事务处于不可提交状态。  
  
```sql  
-- Check to see whether this stored procedure exists.  
IF OBJECT_ID (N'usp_GetErrorInfo', N'P') IS NOT NULL  
    DROP PROCEDURE usp_GetErrorInfo;  
GO  
  
-- Create procedure to retrieve error information.  
CREATE PROCEDURE usp_GetErrorInfo  
AS  
    SELECT   
         ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_LINE () AS ErrorLine  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_MESSAGE() AS ErrorMessage;  
GO  
  
-- SET XACT_ABORT ON will cause the transaction to be uncommittable  
-- when the constraint violation occurs.   
SET XACT_ABORT ON;  
  
BEGIN TRY  
    BEGIN TRANSACTION;  
        -- A FOREIGN KEY constraint exists on this table. This   
        -- statement will generate a constraint violation error.  
        DELETE FROM Production.Product  
            WHERE ProductID = 980;  
  
    -- If the DELETE statement succeeds, commit the transaction.  
    COMMIT TRANSACTION;  
END TRY  
BEGIN CATCH  
    -- Execute error retrieval routine.  
    EXECUTE usp_GetErrorInfo;  
  
    -- Test XACT_STATE:  
        -- If 1, the transaction is committable.  
        -- If -1, the transaction is uncommittable and should   
        --     be rolled back.  
        -- XACT_STATE = 0 means that there is no transaction and  
        --     a commit or rollback operation would generate an error.  
  
    -- Test whether the transaction is uncommittable.  
    IF (XACT_STATE()) = -1  
    BEGIN  
        PRINT  
            N'The transaction is in an uncommittable state.' +  
            'Rolling back transaction.'  
        ROLLBACK TRANSACTION;  
    END;  
  
    -- Test whether the transaction is committable.
    -- You may want to commit a transaction in a catch block if you want to commit changes to statements that ran prior to the error.
    IF (XACT_STATE()) = 1  
    BEGIN  
        PRINT  
            N'The transaction is committable.' +  
            'Committing transaction.'  
        COMMIT TRANSACTION;     
    END;  
END CATCH;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-trycatch"></a>D. 使用 TRY...CATCH  
 以下示例显示一个会生成被零除错误的 `SELECT` 语句。 该错误会使执行跳转到关联的 `CATCH` 块。  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [THROW (Transact-SQL)](../../t-sql/language-elements/throw-transact-sql.md)   
 [数据库引擎错误严重性](../../relational-databases/errors-events/database-engine-error-severities.md)   
 [ERROR_LINE (Transact-SQL)](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER (Transact-SQL)](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY (Transact-SQL)](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE (Transact-SQL&)](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR (Transact-SQL)](../../t-sql/functions/error-transact-sql.md)   
 [GOTO (Transact-SQL)](../../t-sql/language-elements/goto-transact-sql.md)   
 [BEGIN...END (Transact-SQL)](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [XACT_STATE (Transact-SQL)](../../t-sql/functions/xact-state-transact-sql.md)   
 [SET XACT_ABORT (Transact-SQL)](../../t-sql/statements/set-xact-abort-transact-sql.md)  
  
