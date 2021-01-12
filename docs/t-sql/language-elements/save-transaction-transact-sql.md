---
description: SAVE TRANSACTION (Transact-SQL)
title: SAVE TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SAVE
- SAVE_TSQL
- SAVE_TRANSACTION_TSQL
- SAVE TRANSACTION
dev_langs:
- TSQL
helpviewer_keywords:
- rolling back transactions, SAVE TRANSACTION
- SAVE TRANSACTION statement
- transactions [SQL Server], rolling back
- marking transactions [SQL Server]
- savepoints [SQL Server]
- marked transactions [SQL Server], SAVE TRANSACTION statement
- duplicate savepoints
ms.assetid: b953c3f1-f96d-42f1-95a2-30e314292b35
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4f7d9aac54c323527c91f671d25e9ebdf436f6f7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100302"
---
# <a name="save-transaction-transact-sql"></a>SAVE TRANSACTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  在事务内设置保存点。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

 ## <a name="syntax"></a>语法  
  
```syntaxsql
SAVE { TRAN | TRANSACTION } { savepoint_name | @savepoint_variable }  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 savepoint_name  
 分配给保存点的名称。 保存点名称必须符合标识符的规则，但长度不能超过 32 个字符。 savepoint_name 始终区分大小写，即使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例不区分大小写也是如此。  
  
 @*savepoint_variable*  
 包含有效保存点名称的用户定义变量的名称。 必须使用 char、varchar、nchar 或 nvarchar 数据类型声明该变量   。 如果长度超过 32 个字符，也可以传递到变量，但只使用前 32 个字符。  
  
## <a name="remarks"></a>注解  
 用户可以在事务内设置保存点或标记。 保存点可以定义在按条件取消某个事务的一部分后，该事务可以返回的一个位置。 如果将事务回滚到保存点，则根据需要必须完成其他剩余的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句和 COMMIT TRANSACTION 语句，或者必须通过将事务回滚到起始点完全取消事务。 若要取消整个事务，请使用 ROLLBACK TRANSACTION transaction_name。 这将撤消事务的所有语句和过程。  
  
 在事务中允许有重复的保存点名称，但指定保存点名称的 ROLLBACK TRANSACTION 语句只将事务回滚到使用该名称的最近的 SAVE TRANSACTION。  
  
 在使用 BEGIN DISTRIBUTED TRANSACTION 显式启动或从本地事务升级的分布式事务中，不支持 SAVE TRANSACTION。  
  
> [!IMPORTANT]  
>  指定了 savepoint_name 的 ROLLBACK TRANSACTION 语句释放在保存点之后获得的任何锁，但升级和转换除外。 这些锁不会被释放，而且不会转换回先前的锁模式。  
  
## <a name="permissions"></a>权限  
 要求具有 public 角色的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例说明如果活动事务是在执行存储过程之前启动的，如何使用事务保存点仅回滚存储过程所做的修改。  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.objects  
           WHERE name = N'SaveTranExample')  
    DROP PROCEDURE SaveTranExample;  
GO  
CREATE PROCEDURE SaveTranExample  
    @InputCandidateID INT  
AS  
    -- Detect whether the procedure was called  
    -- from an active transaction and save  
    -- that for later use.  
    -- In the procedure, @TranCounter = 0  
    -- means there was no active transaction  
    -- and the procedure started one.  
    -- @TranCounter > 0 means an active  
    -- transaction was started before the   
    -- procedure was called.  
    DECLARE @TranCounter INT;  
    SET @TranCounter = @@TRANCOUNT;  
    IF @TranCounter > 0  
        -- Procedure called when there is  
        -- an active transaction.  
        -- Create a savepoint to be able  
        -- to roll back only the work done  
        -- in the procedure if there is an  
        -- error.  
        SAVE TRANSACTION ProcedureSave;  
    ELSE  
        -- Procedure must start its own  
        -- transaction.  
        BEGIN TRANSACTION;  
    -- Modify database.  
    BEGIN TRY  
        DELETE HumanResources.JobCandidate  
            WHERE JobCandidateID = @InputCandidateID;  
        -- Get here if no errors; must commit  
        -- any transaction started in the  
        -- procedure, but not commit a transaction  
        -- started before the transaction was called.  
        IF @TranCounter = 0  
            -- @TranCounter = 0 means no transaction was  
            -- started before the procedure was called.  
            -- The procedure must commit the transaction  
            -- it started.  
            COMMIT TRANSACTION;  
    END TRY  
    BEGIN CATCH  
        -- An error occurred; must determine  
        -- which type of rollback will roll  
        -- back only the work done in the  
        -- procedure.  
        IF @TranCounter = 0  
            -- Transaction started in procedure.  
            -- Roll back complete transaction.  
            ROLLBACK TRANSACTION;  
        ELSE  
            -- Transaction started before procedure  
            -- called, do not roll back modifications  
            -- made before the procedure was called.  
            IF XACT_STATE() <> -1  
                -- If the transaction is still valid, just  
                -- roll back to the savepoint set at the  
                -- start of the stored procedure.  
                ROLLBACK TRANSACTION ProcedureSave;  
                -- If the transaction is uncommitable, a  
                -- rollback to the savepoint is not allowed  
                -- because the savepoint rollback writes to  
                -- the log. Just return to the caller, which  
                -- should roll back the outer transaction.  
  
        -- After the appropriate rollback, echo error  
        -- information to the caller.  
        DECLARE @ErrorMessage NVARCHAR(4000);  
        DECLARE @ErrorSeverity INT;  
        DECLARE @ErrorState INT;  
  
        SELECT @ErrorMessage = ERROR_MESSAGE();  
        SELECT @ErrorSeverity = ERROR_SEVERITY();  
        SELECT @ErrorState = ERROR_STATE();  
  
        RAISERROR (@ErrorMessage, -- Message text.  
                   @ErrorSeverity, -- Severity.  
                   @ErrorState -- State.  
                   );  
    END CATCH  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [BEGIN TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION (Transact-SQL)](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK (Transact-SQL)](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ERROR_LINE (Transact-SQL)](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER (Transact-SQL)](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY (Transact-SQL)](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE (Transact-SQL&)](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [ROLLBACK TRANSACTION (Transact-SQL)](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK (Transact-SQL)](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [TRY...CATCH (Transact-SQL)](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [XACT_STATE (Transact-SQL)](../../t-sql/functions/xact-state-transact-sql.md)  
  
  
