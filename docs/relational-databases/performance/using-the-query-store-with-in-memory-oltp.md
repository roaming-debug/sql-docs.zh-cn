---
title: 通过内存中 OLTP 使用 Query Store | Microsoft Docs
description: 了解 SQL Server 查询存储让你可以监视运行内存中 OLTP 的工作负荷的本机编译代码的性能。
ms.custom: ''
ms.date: 03/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store, in-memory
ms.assetid: aae5ae6d-7c90-4661-a1c5-df704319888a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e21d436502329d7df0682e661de3548377af397d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469158"
---
# <a name="using-the-query-store-with-in-memory-oltp"></a>通过内存中 OLTP 使用查询存储
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询存储让你可以监视运行内存中 OLTP 的工作负荷的本机编译代码的性能。  
编译和运行时统计与基于磁盘的工作负荷的收集和公开方式相同。   
迁移到内存中 OLTP 时，你可以继续使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的查询存储视图，以及在迁移之前为基于磁盘的的工作负荷开发的自定义脚本。 这样省去了学习查询存储技术的投资，并让其普遍可用于所有类型的工作负荷的疑难解答。  
有关使用查询存储的一般信息，请参阅 [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)。  
  
 通过内存中 OLTP 使用查询存储不需要任何其他功能配置。 在你的数据库上启用它时，它将适用于所有类型的工作负荷。   
但是，通过内存中 OLTP 使用查询存储时，用户应注意一些特定方面：  
  
-   启用查询存储后，将按默认方式收集查询、计划和编译时统计信息。 但是，不会激活运行时统计信息收集，除非你使用 [sys.sp_xtp_control_query_exec_stats (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md) 显式启用它。  
  
-   当你将 \@new_collection_value 设置为 0 时，查询存储将停止为受影响的过程或整个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例收集运行时统计信息。  
  
-   不会保留使用 [sys.sp_xtp_control_query_exec_stats (Transact SQL)](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md) 配置的值。 请确保重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]后再次检查和配置统计信息收集。  
  
-   正如常规查询统计信息集合的情况，当你使用查询存储跟踪负荷执行时，性能可能会降低。 因此，建议仅对本机编译的存储过程的重要子集启用统计信息收集。  
  
-   在首次进行本机编译时将捕获并存储查询和计划，并将在每次重新编译时进行更新。  
  
-   如果启用查询存储或在编译所有本机存储过程后清除了其内容，则必须手动重新编译它们，以使查询存储能够捕获它们。 如果使用 [sp_query_store_remove_query (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md) 或 [sp_query_store_remove_plan (Transct-SQL)](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md) 手动删除查询，这同样适用。 使用 [sp_recompile (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md) 强制进行过程重新编译。  
  
-   在编译过程中，查询存储利用内存中 OLTP 中的计划生成机制捕获查询执行计划。 存储的计划在语义上等效于使用 `SET SHOWPLAN_XML ON` 所获取的计划，但有一处不同；查询存储中的计划按每个单独的语句进行拆分与存储。  
    
-   当你使用混合工作负荷在数据库中运行查询存储，可以使用 [sys.query_store_plan (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md) 中的 **is_natively_compiled** 快速查找由本机代码编译生成的查询计划。  
  
-   查询存储捕获模式（**ALTER TABLE** 语句中的 QUERY_CAPTURE_MODE 参数）不会对来自本机编译模块的查询产生影响，因为无论配置值为何，始终都会捕获它们。 这包括设置 `QUERY_CAPTURE_MODE = NONE`。  
  
-   查询存储捕获的查询编译的持续时间仅包括在生成本机代码之前，查询优化所用的时间。 更确切地说，持续时间不包括 C 代码编译的时间，以及 C 代码生成所需的内部结构生成的时间。  
  
-   [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md) 范围内的内存授予度量值并不会填充本机编译查询；它们的值始终是 0。 内存授予列有：avg_query_max_used_memory、last_query_max_used_memory、min_query_max_used_memory、max_query_max_used_memory 和 stdev_query_max_used_memory。  
  
## <a name="enabling-and-using-query-store-with-in-memory-oltp"></a>通过内存中 OLTP 启用并使用查询存储  
 下面的简单示例演示了在端到端用户方案中，通过内存中 OLTP 使用查询存储。 在此示例中，我们假定为内存中 OLTP 启用了一个数据库 (`MemoryOLTP`)。  
    有关内存优化表必备组件的详细信息，请参阅 [创建内存优化表和本机编译的存储过程](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md)。  
  
```  
USE MemoryOLTP;  
GO  
  
-- Create a simple memory-optimized table   
CREATE TABLE dbo.Ord  
   (OrdNo INTEGER not null PRIMARY KEY NONCLUSTERED,   
    OrdDate DATETIME not null,   
    CustCode NVARCHAR(5) not null)   
WITH (MEMORY_OPTIMIZED=ON);  
GO  
  
-- Enable Query Store before native module compilation  
ALTER DATABASE MemoryOLTP SET QUERY_STORE = ON;  
GO  
  
-- Create natively compiled stored procedure  
CREATE PROCEDURE dbo.OrderInsert(@OrdNo integer, @CustCode nvarchar(5))  
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS   
    BEGIN ATOMIC WITH  
    (TRANSACTION ISOLATION LEVEL = SNAPSHOT,  
    LANGUAGE = N'English')  
  
    DECLARE @OrdDate DATETIME = GETDATE();  
    INSERT INTO dbo.Ord (OrdNo, CustCode, OrdDate)   
        VALUES (@OrdNo, @CustCode, @OrdDate);  
END;  
GO  
  
-- Enable runtime stats collection for queries from dbo.OrderInsert stored procedure  
DECLARE @db_id INT = DB_ID()  
DECLARE @proc_id INT = OBJECT_ID('dbo.OrderInsert');  
DECLARE @collection_enabled BIT;  
  
EXEC [sys].[sp_xtp_control_query_exec_stats] @new_collection_value = 1,   
    @database_id = @db_id, @xtp_object_id = @proc_id;  
  
-- Check the state of the collection flag  
EXEC sp_xtp_control_query_exec_stats @database_id = @db_id,   
    @xtp_object_id = @proc_id,   
    @old_collection_value= @collection_enabled output;  
SELECT @collection_enabled AS 'collection status';  
  
-- Execute natively compiled workload  
EXEC dbo.OrderInsert 1, 'A';  
EXEC dbo.OrderInsert 2, 'B';  
EXEC dbo.OrderInsert 3, 'C';  
EXEC dbo.OrderInsert 4, 'D';  
EXEC dbo.OrderInsert 5, 'E';  
  
-- Check Query Store Data  
-- Compile time data  
SELECT q.query_id, plan_id, object_id, query_hash, p.query_plan,  
    p.initial_compile_start_time, p.last_compile_start_time,   
    p.last_execution_time, p.avg_compile_duration,  
    p.last_force_failure_reason, p.force_failure_count  
FROM sys.query_store_query AS q  
JOIN sys.query_store_plan AS p   
    ON q.query_id = p.plan_id  
WHERE q.object_id = OBJECT_ID('dbo.OrderInsert');  
  
-- Get runtime stats  
-- Check count_executions field to verify that runtime statistics   
-- have been collected by the Query Store  
SELECT q.query_id, p.plan_id, object_id, rsi.start_time, rsi.end_time,    
    p.last_force_failure_reason, p.force_failure_count, rs.*  
FROM sys.query_store_query AS q  
JOIN sys.query_store_plan AS p   
    ON q.query_id = p.plan_id  
JOIN sys.query_store_runtime_stats AS rs   
    ON rs.plan_id = p.plan_id  
JOIN sys.query_store_runtime_stats_interval AS rsi   
    ON rs.runtime_stats_interval_id = rsi.runtime_stats_interval_id  
WHERE q.object_id = OBJECT_ID('dbo.OrderInsert');  
```  
  
## <a name="see-also"></a>另请参阅  
 [相关视图、函数和过程](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [创建内存优化表和本机编译的存储过程](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md)   
 [查询存储最佳实践](../../relational-databases/performance/best-practice-with-the-query-store.md)   
 [查询存储存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [查询存储目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)  
  
  
