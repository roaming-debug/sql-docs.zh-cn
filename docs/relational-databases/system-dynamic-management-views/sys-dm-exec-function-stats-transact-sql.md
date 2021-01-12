---
description: 'sys.dm_exec_function_stats (Transact-sql) '
title: sys.dm_exec_function_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_function_stats
- sys.dm_exec_function_stats_tsql
- dm_exec_function_stats
- dm_exec_function_stats_tsql
helpviewer_keywords:
- sys.dm_exec_function_stats dynamic management view
ms.assetid: 4c3d6a02-08e4-414b-90be-36b89a0e5a3a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 788596936cf6ce9c8d2ca0a618b441fcbd0f7624
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099785"
---
# <a name="sysdm_exec_function_stats-transact-sql"></a>sys.dm_exec_function_stats (Transact-sql) 
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  返回缓存函数的聚合性能统计信息。 视图为每个缓存的函数计划返回一行，行的生存期与函数保持缓存的时间相同。 从缓存中移除某个函数后，将从此视图中删除相应的行。 此时，将引发类似于 **sys.dm_exec_query_stats** 的 Performance Statistics SQL 跟踪事件。 返回有关标量函数的信息，其中包括内存中函数和 CLR 标量函数。 不返回有关表值函数的信息。  
  
 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，动态管理视图不能公开将影响数据库包含的信息，也不能公开有关用户可以访问的其他数据库的信息。 为了避免公开此信息，每个包含不属于所连接的租户的数据的行都将被筛选掉。  
  
> [!NOTE]
> 每次执行时， **sys.dm_exec_function_stats**  的结果可能会有所不同，因为数据只反映完成的查询，而不是仍在进行中的查询。 

  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|函数所在的数据库 ID。|  
|object_id|**int**|函数的对象标识号。|  
|type|**char(2)**|对象的类型： FN = 标量值函数|  
|**type_desc**|**nvarchar(60)**|对象类型的说明： SQL_SCALAR_FUNCTION|  
|**sql_handle**|**varbinary(64)**|这可用于与在此函数中执行的 **sys.dm_exec_query_stats** 中的查询相关联。|  
|**plan_handle**|**varbinary(64)**|内存中计划的标识符。 该标识符是瞬态的，仅当计划保留在缓存中时，它才保持不变。 此值可与 **sys.dm_exec_cached_plans** 动态管理视图一起使用。<br /><br /> 当本机编译的函数查询内存优化表时，将始终为0x000。|  
|**cached_time**|**datetime**|将函数添加到缓存的时间。|  
|**last_execution_time**|**datetime**|上次执行函数的时间。|  
|**execution_count**|**bigint**|函数自上次编译以来已执行的次数。|  
|**total_worker_time**|**bigint**|此函数自编译以来执行所使用的 CPU 时间总量（微秒）。<br /><br /> 对于本机编译的函数，如果许多执行所需的时间不到1毫秒， **total_worker_time** 可能不准确。|  
|**last_worker_time**|**bigint**|上次执行函数所用的 CPU 时间（微秒）。 <sup>1</sup>|  
|**min_worker_time**|**bigint**|此函数在单次执行期间所用的最小 CPU 时间（微秒）。 <sup>1</sup>|  
|**max_worker_time**|**bigint**|此函数在单次执行期间所用的最大 CPU 时间（微秒）。 <sup>1</sup>|  
|**total_physical_reads**|**bigint**|此函数自编译以来执行的物理读取总次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**last_physical_reads**|**bigint**|上次执行函数时执行的物理读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**min_physical_reads**|**bigint**|此函数在单次执行期间所执行的最少物理读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**max_physical_reads**|**bigint**|此函数在单次执行期间所执行的最大物理读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**total_logical_writes**|**bigint**|此函数自编译以来执行的逻辑写入总次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**last_logical_writes**|**bigint**|上次执行计划时变脏的缓冲池页数。 如果页已变脏（已修改），则不计入写次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**min_logical_writes**|**bigint**|此函数在单次执行期间所执行的最少逻辑写入次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**max_logical_writes**|**bigint**|此函数在单次执行期间所执行的最大逻辑写入次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**total_logical_reads**|**bigint**|此函数自编译以来执行的逻辑读取总次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**last_logical_reads**|**bigint**|上次执行函数时执行的逻辑读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**min_logical_reads**|**bigint**|此函数在单次执行期间所执行的最少逻辑读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**max_logical_reads**|**bigint**|此函数在单次执行期间所执行的最大逻辑读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**total_elapsed_time**|**bigint**|完成此函数的执行所用的总时间（微秒）。|  
|**last_elapsed_time**|**bigint**|最近完成此函数的执行所用的时间（微秒）。|  
|**min_elapsed_time**|**bigint**|完成此函数的任何执行所用的最短时间（微秒）。|  
|**max_elapsed_time**|**bigint**|完成此函数的任何执行所用的最长时间（微秒）。|  
|**total_page_server_reads**|**bigint**|此函数自编译以来执行的页服务器读取的总次数。<br /><br /> **适用于：** Azure SQL 数据库超大规模。|  
|**last_page_server_reads**|**bigint**|上次执行函数时已执行的页服务器读取次数。<br /><br /> **适用于：** Azure SQL 数据库超大规模。|  
|**min_page_server_reads**|**bigint**|此函数在单次执行期间所执行的最少页面服务器读取次数。<br /><br /> **适用于：** Azure SQL 数据库超大规模。|  
|**max_page_server_reads**|**bigint**|此函数在单次执行期间所执行的最大页服务器读取次数。<br /><br /> **适用于：** Azure SQL 数据库超大规模。|
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="examples"></a>示例  
 下面的示例返回有关按平均占用时间标识的前十个函数的信息。  
  
```  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'function name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_function_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>另请参阅  
 [与执行相关的动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_query_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 
 [sys.dm_exec_trigger_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)   
 [sys.dm_exec_procedure_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  
  
  
