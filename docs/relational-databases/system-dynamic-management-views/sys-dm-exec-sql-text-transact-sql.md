---
description: sys.dm_exec_sql_text (Transact-SQL)
title: sys.dm_exec_sql_text (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_sql_text
- sys.dm_exec_sql_text
- sys.dm_exec_sql_text_TSQL
- dm_exec_sql_text_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_sql_text dynamic management function
ms.assetid: 61b8ad6a-bf80-490c-92db-58dfdff22a24
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 089633aa2fb51a727d861d987bf9a628408b2540
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192941"
---
# <a name="sysdm_exec_sql_text-transact-sql"></a>sys.dm_exec_sql_text (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回由指定 *sql_handle* 标识的 SQL 批处理的文本。 该表值函数将替换系统函数 **fn_get_sql**。  
  
 
## <a name="syntax"></a>语法  
  
```  
sys.dm_exec_sql_text(sql_handle | plan_handle)  
```  
  
## <a name="arguments"></a>参数  
*sql_handle*  
是一个标记，用于唯一标识已执行或当前正在执行的批处理。 *sql_handle* 为 **varbinary (64)**。 

可以从以下动态管理对象中获取 *sql_handle* ：  
  
-   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
-   [sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)  
  
-   [sys.dm_exec_xml_handles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)  
  
-   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
  
-   [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)  
  
*plan_handle*  
是一个标记，用于唯一标识已执行并且其计划驻留在计划缓存中或当前正在执行的批处理的查询执行计划。 *plan_handle* 为 **varbinary (64)**。   

可以从以下动态管理对象中获取 *plan_handle* ：    
  
-   [sys.dm_exec_cached_plans (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)   
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**dbid**|**smallint**|数据库的 ID。<br /><br /> 对于临时和预定义 SQL 语句，指编译这些语句时所在的数据库的 ID。|  
|**objectid**|**int**|对象的 ID。<br /><br /> 对于临时和预定义 SQL 语句为 NULL。|  
|**数字**|**smallint**|对于带编号的存储过程，此列返回存储过程的编号。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.numbered_procedures ](../../relational-databases/system-catalog-views/sys-numbered-procedures-transact-sql.md)。<br /><br /> 对于临时和预定义 SQL 语句为 NULL。|  
|**过**|**bit**|1 = SQL 文本已加密。<br /><br /> 0 = SQL 文本未加密。|  
|**text**|**nvarchar (max** **)**|SQL 查询的文本。<br /><br /> 对于已加密对象为 NULL。|  
  
## <a name="permissions"></a>权限  
 要求具有对服务器的 `VIEW SERVER STATE` 权限。  
  
## <a name="remarks"></a>备注  
对于即席查询，SQL 句柄是基于提交给服务器的 SQL 文本的哈希值，并且可以来自任何数据库。 

对于诸如存储过程、触发器或函数之类的数据库对象，SQL 句柄派生自数据库 ID、对象 ID 和对象编号。 

计划句柄是派生自整个批处理的已编译计划的一个哈希值。 

> [!NOTE]
> 无法根据即席查询 *sql_handle* 确定 **dbid** 。 若要确定适用于即席查询的 **dbid** ，请改用 *plan_handle* 。
  
## <a name="examples"></a>示例 

### <a name="a-conceptual-example"></a>A. 概念示例
下面是一个基本示例，演示如何直接或通过 **交叉应用** 传递 **sql_handle** 。
  1.  创建活动。  
在的新查询窗口中执行以下 T-sql [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。   
      ```sql
      -- Identify current spid (session_id)
      SELECT @@SPID;
      GO
  
      -- Create activity
        WAITFOR DELAY '00:02:00';
      ```
      
  2.  使用 **交叉应用**。  
    将使用 **跨应用** 将 **sys.dm_exec_requests** 中的 sql_handle 传递给 **sys.dm_exec_sql_text** 。 打开一个新的查询窗口，并传递步骤1中标识的 spid。 在此示例中，spid 的行为是 `59` 。

        ```sql
        SELECT t.*
        FROM sys.dm_exec_requests AS r
        CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) AS t
        WHERE session_id = 59 -- modify this value with your actual spid
         ```      
 
  2.  直接传递 **sql_handle** 。  
从 **sys.dm_exec_requests** 中获取 **sql_handle** 。 然后，将 **sql_handle** 直接传递到 **sys.dm_exec_sql_text**。 打开一个新的查询窗口，并将步骤1中标识的 spid 传递到 **sys.dm_exec_requests**。 在此示例中，spid 的行为是 `59` 。 然后将返回的 **sql_handle** 作为参数传递给 **sys.dm_exec_sql_text**。

        ```sql
        -- acquire sql_handle
        SELECT sql_handle FROM sys.dm_exec_requests WHERE session_id = 59  -- modify this value with your actual spid
        
        -- pass sql_handle to sys.dm_exec_sql_text
        SELECT * FROM sys.dm_exec_sql_text(0x01000600B74C2A1300D2582A2100000000000000000000000000000000000000000000000000000000000000) -- modify this value with your actual sql_handle
         ```      
    
  
### <a name="b-obtain-information-about-the-top-five-queries-by-average-cpu-time"></a>B. 按平均 CPU 时间获取有关前五个查询的信息  
 以下示例返回前五个查询的 SQL 语句文本和平均 CPU 时间。  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
    SUBSTRING(st.text, (qs.statement_start_offset/2)+1,   
        ((CASE qs.statement_end_offset  
          WHEN -1 THEN DATALENGTH(st.text)  
         ELSE qs.statement_end_offset  
         END - qs.statement_start_offset)/2) + 1) AS statement_text  
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
ORDER BY total_worker_time/execution_count DESC;  
```  
  
### <a name="c-provide-batch-execution-statistics"></a>C. 提供批处理执行统计信息  
 以下示例返回按批执行的 SQL 查询的文本，并提供有关它们的统计信息。  
  
```sql  
SELECT s2.dbid,   
    s1.sql_handle,    
    (SELECT TOP 1 SUBSTRING(s2.text,statement_start_offset / 2+1 ,   
      ( (CASE WHEN statement_end_offset = -1   
         THEN (LEN(CONVERT(nvarchar(max),s2.text)) * 2)   
         ELSE statement_end_offset END)  - statement_start_offset) / 2+1))  AS sql_statement,  
    execution_count,   
    plan_generation_num,   
    last_execution_time,     
    total_worker_time,   
    last_worker_time,   
    min_worker_time,   
    max_worker_time,  
    total_physical_reads,   
    last_physical_reads,   
    min_physical_reads,    
    max_physical_reads,    
    total_logical_writes,   
    last_logical_writes,   
    min_logical_writes,   
    max_logical_writes    
FROM sys.dm_exec_query_stats AS s1   
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS s2    
WHERE s2.objectid is null   
ORDER BY s1.sql_handle, s1.statement_start_offset, s1.statement_end_offset;  
```  
  
## <a name="see-also"></a>请参阅  
 [动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与执行相关的动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_cursors &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)   
 [sys.dm_exec_xml_handles &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [使用 APPLY](../../t-sql/queries/from-transact-sql.md#using-apply)   
 [sys.dm_exec_text_query_plan &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)  

