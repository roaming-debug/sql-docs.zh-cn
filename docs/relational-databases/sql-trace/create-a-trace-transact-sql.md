---
description: 创建跟踪 (Transact-SQL)
title: 创建跟踪 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- traces [SQL Server], example
- traces [SQL Server], creating
ms.assetid: 79dd4254-e3c6-467a-bb6f-f99e51757e99
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ff2970bf4d450c425f169be7b2bb72c24db7d2d0
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364789"
---
# <a name="create-a-trace-transact-sql"></a>创建跟踪 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍了如何使用存储过程创建跟踪。  
  
### <a name="to-create-a-trace"></a>创建跟踪  
  
1.  执行带所需参数的 **sp_trace_create** 以创建新的跟踪。 新的跟踪将处于停止状态（状态为 **0** ）。  
  
2.  执行带所需参数的 **sp_trace_setevent** 以选择要跟踪的事件和列。  
  
3.  也可以执行 **sp_trace_setfilter** 以设置任意筛选或筛选组合。  

     **sp_trace_setevent** 和 **sp_trace_setfilter** 只能在已停止的现有跟踪上执行。  
  
    > [!IMPORTANT]  
    >  与常规存储过程不同的是，必须严格键入所有 SQL Server Profiler 存储过程的参数 (<strong>sp_trace_xx</strong>)，而且这些参数不支持数据类型自动转换。 如果这些参数不是使用正确的输入参数数据类型（正如参数说明中指定的一样）调用的，则存储过程会返回错误。  
  
## <a name="examples"></a>示例

 下面的代码演示如何使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]创建跟踪。 代码分为三部分：创建跟踪、填充跟踪文件以及停止跟踪。 通过添加要跟踪的事件来自定义跟踪。 有关事件和列的列表，请参阅 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)创建跟踪。  
  
### <a name="a-create-a-trace"></a>A. 创建跟踪
 下面的代码创建跟踪，向跟踪添加事件，随后启动跟踪：  
  
```  
DECLARE @RC int, @TraceID int, @on BIT  
EXEC @rc = sp_trace_create @TraceID output, 0, N'C:\SampleTrace'  
  
-- Select the return code to see if the trace creation was successful.  
SELECT RC = @RC, TraceID = @TraceID  
  
-- Set the events and data columns you need to capture.  
SELECT @on = 1  
  
-- 10 is RPC:Completed event. 1 is TextData column.   
EXEC sp_trace_setevent @TraceID, 10, 1, @on   
-- 13 is SQL:BatchStarting, 11 is LoginName  
EXEC sp_trace_setevent @TraceID, 13, 11, @on   
-- 13 is SQL:BatchStarting, 14 is StartTime  
EXEC sp_trace_setevent @TraceID, 13, 14, @on   
-- 12 is SQL:BatchCompleted, 15 is EndTime  
EXEC sp_trace_setevent @TraceID, 12, 15, @on   
-- 13 is SQL:BatchStarting, 1 is TextData  
EXEC sp_trace_setevent @TraceID, 13, 1, @on   
  
-- Set any filter. Not provided in this example  
--EXEC sp_trace_setfilter 1, 10, 0, 6, N'%Profiler%'  
  
-- Start Trace (status 1 = start)  
EXEC @RC = sp_trace_setstatus @TraceID, 1  
GO  
  
```  
  
### <a name="b-populate-the-trace-file"></a>B. 填充跟踪文件
 现在已创建并启动跟踪，接下来执行下面的代码以便用活动填充跟踪。  
  
```  
SELECT * FROM master.sys.databases  
GO  
SELECT * FROM ::fn_trace_getinfo(default)  
GO  
  
```  
  
### <a name="c-stop-the-trace"></a>C. 停止跟踪
 可以随时停止和重新启动跟踪。 在本示例中，执行下面的代码以停止跟踪、关闭跟踪并删除跟踪定义。  
  
```  
DECLARE @TraceID int  
-- Populate a variable with the trace_id of the current trace  
SELECT  @TraceID = TraceID FROM ::fn_trace_getinfo(default) WHERE VALUE = N'C:\SampleTrace.trc'  
  
-- First stop the trace.   
EXEC sp_trace_setstatus @TraceID, 0  
  
-- Close and then delete its definition from SQL Server.   
EXEC sp_trace_setstatus @TraceID, 2  
  
```  
  
### <a name="d-examine-the-trace-file"></a>D. 检查跟踪文件
 若要检查跟踪文件，请使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]打开 SampleTrace.trc 文件。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Profiler 存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)   
 [sp_trace_create (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)  
  
  
