---
description: WAITFOR (Transact-SQL)
title: WAITFOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WAITFOR
- WAITFOR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- TIME option
- delaying executions [SQL Server]
- batches [SQL Server], execution times
- stored procedures [SQL Server], executing
- DELAY
- time [SQL Server], execution delays
- blocking executions
- transactions [SQL Server], execution times
- WAITFOR statement
- timing executions
ms.assetid: 8e896e73-af27-4cae-a725-7a156733f3bd
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0933c90f711f5c86425c7e2c6b4b350c47612ddc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095867"
---
# <a name="waitfor-transact-sql"></a>WAITFOR (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  阻止执行批处理、存储过程或事务，直到已过指定时间或时间间隔，或者指定语句发生修改或至少返回一行为止。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
WAITFOR   
{  
    DELAY 'time_to_pass'   
  | TIME 'time_to_execute'   
  | [ ( receive_statement ) | ( get_conversation_group_statement ) ]   
    [ , TIMEOUT timeout ]  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 DELAY  
 可以继续执行批处理、存储过程或事务之前必须经过的指定时段，最长可为 24 小时。  
  
 '*time_to_pass*'  
 等待的时段。 time_to_pass 可以以“datetime”数据格式指定，也可以指定为局部变量。 不能指定日期；因此，不允许指定“datetime”值的日期部分。 time_to_pass 将被格式化为 hh:mm[[:ss].mss]。
  
 TIME  
 指定的运行批处理、存储过程或事务的时间。  
  
 '*time_to_execute*'  
 WAITFOR 语句完成的时间。 可以使用“datetime”数据格式指定 time_to_execute，也可以将其指定为局部变量。 不能指定日期；因此，不允许指定“datetime”值的日期部分。 time_to_execute 将被格式化为 hh:mm[[:ss].mss]，并且可以选择包括 1900-01-01 的日期。
  
 *receive_statement*  
 有效的 RECEIVE 语句。  
  
> [!IMPORTANT]  
>  包含 receive_statement 的 WAITFOR 仅适用于 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 消息。 有关详细信息，请参阅 [RECEIVE (Transact-SQL)](../../t-sql/statements/receive-transact-sql.md)。  
  
 *get_conversation_group_statement*  
 有效的 GET CONVERSATION GROUP 语句。  
  
> [!IMPORTANT]  
>  包含 get_conversation_group_statement 的 WAITFOR 仅适用于 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 消息。 有关详细信息，请参阅 [GET CONVERSATION GROUP (Transact-SQL)](../../t-sql/statements/get-conversation-group-transact-sql.md)。  
  
 TIMEOUT timeout  
 指定消息到达队列前等待的时间（以毫秒为单位）。  
  
> [!IMPORTANT]  
>  指定包含 TIMEOUT 的 WAITFOR 仅适用于 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 消息。 有关详细信息，请参阅 [RECEIVE (Transact-SQL)](../../t-sql/statements/receive-transact-sql.md) 和 [GET CONVERSATION GROUP (Transact-SQL)](../../t-sql/statements/get-conversation-group-transact-sql.md)。  
  
## <a name="remarks"></a>备注  
 执行 WAITFOR 语句时，事务正在运行，并且其他请求不能在同一事务下运行。  
  
 实际的时间延迟可能与 time_to_pass、time_to_execute 或 timeout 中指定的时间不同，它依赖于服务器的活动级别。 计划 WAITFOR 语句线程时，计时器开始计时。 如果服务器忙碌，则可能不会立即计划线程；因此，时间延迟可能比指定的时间要长。  
  
 WAITFOR 不更改查询的语义。 如果查询不能返回任何行，WAITFOR 将一直等待，或等到满足 TIMEOUT 条件（如果已指定）。  
  
 不能对 WAITFOR 语句打开游标。  
  
 不能为 WAITFOR 语句定义视图。  
  
 如果查询超出了 query wait 选项的值，则 WAITFOR 语句参数不运行即可完成。 有关详细信息，请参阅[配置 query wait 服务器配置选项](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md)。 若要查看活动进程和正在等待的进程，请使用 [sp_who](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)。  
  
 每个 WAITFOR 语句都有与其关联的线程。 如果对同一服务器指定了多个 WAITFOR 语句，可将等待这些语句运行的多个线程关联起来。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将监视 WAITFOR 语句线程数，并在服务器开始遇到线程资源不足的问题时，随机选择其中部分线程退出。  
  
 如果某个事务锁定了 WAITFOR 语句试图访问的行集以防止对行集进行更改，则可以在该事务中运行包含 WAITFOR 语句的查询来创建死锁。 如果存在上述死锁，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会标识这些情况并返回空结果集。  
  
> [!CAUTION]  
>  包含 WAITFOR 将减慢 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 过程的完成速度，并会导致应用程序中的超时消息。 如有必要，请在应用程序级别调整连接的超时设置。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-waitfor-time"></a>A. 使用 WAITFOR TIME  
 下面的示例在晚上 10:20 在 msdb 数据库中执行 `sp_update_job` 存储过程。 (`22:20`).  
  
```sql  
EXECUTE sp_add_job @job_name = 'TestJob';  
BEGIN  
    WAITFOR TIME '22:20';  
    EXECUTE sp_update_job @job_name = 'TestJob',  
        @new_name = 'UpdatedJob';  
END;  
GO  
```  
  
### <a name="b-using-waitfor-delay"></a>B. 使用 WAITFOR DELAY  
 以下示例在两小时的延迟后执行存储过程。  
  
```sql  
BEGIN  
    WAITFOR DELAY '02:00';  
    EXECUTE sp_helpdb;  
END;  
GO  
```  
  
### <a name="c-using-waitfor-delay-with-a-local-variable"></a>C. 在 WAITFOR DELAY 中使用局部变量  
 以下示例显示如何对 `WAITFOR DELAY` 选项使用局部变量。 该存储过程将等待可变的时间段，然后将经过的小时、分钟和秒数信息返回给用户。  
  
```sql  
IF OBJECT_ID('dbo.TimeDelay_hh_mm_ss','P') IS NOT NULL  
    DROP PROCEDURE dbo.TimeDelay_hh_mm_ss;  
GO  
CREATE PROCEDURE dbo.TimeDelay_hh_mm_ss   
    (  
    @DelayLength char(8)= '00:00:00'  
    )  
AS  
DECLARE @ReturnInfo VARCHAR(255)  
IF ISDATE('2000-01-01 ' + @DelayLength + '.000') = 0  
    BEGIN  
        SELECT @ReturnInfo = 'Invalid time ' + @DelayLength   
        + ',hh:mm:ss, submitted.';  
        -- This PRINT statement is for testing, not use in production.  
        PRINT @ReturnInfo   
        RETURN(1)  
    END  
BEGIN  
    WAITFOR DELAY @DelayLength  
    SELECT @ReturnInfo = 'A total time of ' + @DelayLength + ',   
        hh:mm:ss, has elapsed! Your time is up.'  
    -- This PRINT statement is for testing, not use in production.  
    PRINT @ReturnInfo;  
END;  
GO  
/* This statement executes the dbo.TimeDelay_hh_mm_ss procedure. */  
EXEC TimeDelay_hh_mm_ss '00:00:10';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `A total time of 00:00:10, in hh:mm:ss, has elapsed. Your time is up.`  
  
## <a name="see-also"></a>另请参阅  
 [控制流语言 (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)   
 [datetime (Transact-SQL)](../../t-sql/data-types/datetime-transact-sql.md)   
 [sp_who (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
  
