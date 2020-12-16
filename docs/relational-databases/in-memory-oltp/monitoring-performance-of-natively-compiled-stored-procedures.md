---
title: 监视本机编译存储过程的性能
description: 了解如何监视本机编译的存储过程和其他本机编译的 T-SQL 模块的性能。
ms.custom: seo-dt-2019
ms.date: 04/03/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 55548cb2-77a8-4953-8b5a-f2778a4f13cf
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d87657183c53568781785cf2aa2e85ade81fbb18
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460400"
---
# <a name="monitoring-performance-of-natively-compiled-stored-procedures"></a>监视本机编译的存储过程的执行

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本文介绍如何监视本机编译的存储过程和其他本机编译的 T-SQL 模块的性能。  
  
## <a name="using-extended-events"></a>使用扩展事件  
 使用 **sp_statement_completed** 扩展事件可以跟踪查询的执行情况。 使用此事件或者可以选择使用针对某一特定本机编译的存储过程的 object_id 的筛选器创建一个扩展事件会话。 在执行各种查询之后引发扩展事件。 该扩展事件报告的 CPU 时间和持续时间指示该查询占用了多少 CPU 以及执行时间。 占用大量 CPU 时间的本机编译的存储过程可能具有性能问题。  
  
line_number，连同扩展事件中的 object_id 可用于调查该查询 。 可以使用以下查询检索过程定义。 可以使用行号标识该定义内的查询：  
  
```sql  
SELECT [definition]
FROM sys.sql_modules
WHERE object_id=object_id;
```  
    
## <a name="using-data-management-views-and-query-store"></a>使用数据管理视图和查询存储
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 支持在过程级别和查询级别收集本机编译的存储过程的执行统计信息。 由于对性能的影响，默认不启用收集执行统计信息。  

执行统计信息反映在系统视图 [sys.dm_exec_procedure_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) 和 [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) 以及[查询存储](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)中。

## <a name="procedure-level-execution-statistics"></a>过程级别执行统计信息

**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** ：可使用 [sys.sp_xtp_control_proc_exec_stats (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md) 对本机编译的存储过程启用或禁用过程级别统计信息收集。  以下语句针对当前实例上所有本机编译的 T-SQL 模块启用过程级别的执行统计信息收集：

```sql
EXEC sys.sp_xtp_control_proc_exec_stats 1
```

[!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]： 可使用[数据库范围配置](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)选项 `XTP_PROCEDURE_EXECUTION_STATISTICS` 对本机编译的存储过程启用或禁用过程级别的统计信息收集。 以下语句对当前数据库中的所有本地编译的 T-SQL 模块启用过程级别的执行统计信息收集：

```sql
ALTER DATABASE SCOPED CONFIGURATION SET XTP_PROCEDURE_EXECUTION_STATISTICS = ON;
```

## <a name="query-level-execution-statistics"></a>查询级别执行统计信息

**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** ：可使用 [sys.sp_xtp_control_query_exec_stats (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md) 对本机编译的存储过程启用或禁用查询级别的统计信息收集。  以下语句对当前实例的所有本机编译的 T-SQL 模块启用查询级别的执行统计信息收集：

```sql
EXEC sys.sp_xtp_control_query_exec_stats 1
```

[!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]： 可使用[数据库范围配置](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)选项 `XTP_QUERY_EXECUTION_STATISTICS` 对本机编译的存储过程启用或禁用语句级别统计信息收集。 以下语句对当前数据库中的所有本机编译的 T-SQL 模块启用查询级别的执行统计信息收集：

```sql
ALTER DATABASE SCOPED CONFIGURATION SET XTP_QUERY_EXECUTION_STATISTICS = ON;
```

## <a name="sample-queries"></a>示例查询

 在你收集统计信息后，可以使用 [sys.dm_exec_procedure_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) 针对某一过程查询本机编译的存储过程的执行统计信息，以及使用 [sys.dm_exec_query_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) 针对查询来查询本机编译的存储过程的执行统计信息。  
 
  
 下面的查询在统计信息收集后返回当前数据库中本机编译的存储过程的过程名称和执行统计信息：  

```sql
SELECT object_id, object_name(object_id) AS 'object name',
       cached_time, last_execution_time, execution_count,
       total_worker_time, last_worker_time,
       min_worker_time, max_worker_time,
       total_elapsed_time, last_elapsed_time,
       min_elapsed_time, max_elapsed_time
FROM sys.dm_exec_procedure_stats
WHERE database_id = DB_ID()
      AND object_id IN (SELECT object_id FROM sys.sql_modules WHERE uses_native_compilation = 1)
ORDER BY total_worker_time desc;
```

下面的查询按总工作线程时间的降序返回当前数据库中已为其收集了统计信息的本机编译的存储过程中所有查询的查询文本以及执行统计信息：  

```sql
SELECT st.objectid,
        OBJECT_NAME(st.objectid) AS 'object name',
        SUBSTRING(
            st.text,
            (qs.statement_start_offset/2) + 1,
            ((qs.statement_end_offset-qs.statement_start_offset)/2) + 1
            ) AS 'query text',
        qs.creation_time, qs.last_execution_time, qs.execution_count,
        qs.total_worker_time, qs.last_worker_time, qs.min_worker_time, 
        qs.max_worker_time, qs.total_elapsed_time, qs.last_elapsed_time,
        qs.min_elapsed_time, qs.max_elapsed_time
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(sql_handle) st
WHERE database_id = DB_ID()
      AND object_id IN (SELECT object_id FROM sys.sql_modules WHERE uses_native_compilation = 1)
ORDER BY total_worker_time desc;
```

## <a name="query-execution-plans"></a>查询执行计划

 本机编译的存储过程支持 SHOWPLAN_XML（估计的执行计划）。 可以使用该估计的执行计划检查查询计划，以便找到任何错误计划问题。 错误计划的常见原因是：  
  
-   在创建过程前未更新统计信息。  
  
-   缺失索引  
  
 通过执行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)]获取 Showplan XML：  
  
```sql  
SET SHOWPLAN_XML ON  
GO  
EXEC my_proc   
GO  
SET SHOWPLAN_XML OFF  
GO  
```  
  
 或者，在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，选择过程名称并且单击 **“显示估计的执行计划”** 。  
  
 本机编译的存储过程的估计的执行计划显示过程中查询的查询运算符和表达式。 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 对于本机编译的存储过程，并不支持所有 SHOWPLAN_XML 属性。 例如，与查询优化器开销相关的属性不是针对过程的 SHOWPLAN_XML 的一部分。  
  
## <a name="see-also"></a>另请参阅  
 [本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
