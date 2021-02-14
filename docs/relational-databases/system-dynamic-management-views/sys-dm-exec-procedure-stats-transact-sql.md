---
description: sys.dm_exec_procedure_stats (Transact-SQL)
title: sys.dm_exec_procedure_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_procedure_stats_TSQL
- dm_exec_procedure_stats_TSQL
- dm_exec_procedure_stats
- sys.dm_exec_procedure_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_procedure_stats dynamic management view
ms.assetid: ab8ddde8-1cea-4b41-a7e4-697e6ddd785a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 14292947fb8ff77df9c00d28eb35de8f55ce8b33
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342991"
---
# <a name="sysdm_exec_procedure_stats-transact-sql"></a>sys.dm_exec_procedure_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回缓存存储过程的聚合性能统计信息。 该视图为每个缓存的存储过程计划都返回一行，行的生存期与存储过程保持缓存状态的时间一样长。 在从缓存中删除存储过程时，也将从该视图中删除对应行。 此时，将引发类似于 **sys.dm_exec_query_stats** 的 Performance Statistics SQL 跟踪事件。  
  
 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，动态管理视图不能公开将影响数据库包含的信息，也不能公开有关用户可以访问的其他数据库的信息。 为了避免公开此信息，每个包含不属于所连接的租户的数据的行都将被筛选掉。  
  
> [!NOTE]
> 每次执行时， **sys.dm_exec_procedure_stats**  的结果可能会有所不同，因为数据只反映完成的查询，而不是仍在进行中的查询。
> 若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_exec_procedure_stats**。 

  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------| 
|database_id|**int**|存储过程所在的数据库 ID。|  
|object_id|**int**|存储过程的对象标识号。|  
|type|**char(2)**|对象的类型：<br /><br /> P = SQL 存储过程<br /><br /> PC = 程序集 (CLR) 存储过程<br /><br /> X = 扩展存储过程|  
|**type_desc**|**nvarchar(60)**|对对象类型的说明：<br /><br /> SQL_STORED_PROCEDURE<br /><br /> CLR_STORED_PROCEDURE<br /><br /> EXTENDED_STORED_PROCEDURE|  
|**sql_handle**|**varbinary(64)**|这可用于与 **sys.dm_exec_query_stats** 中从该存储过程中执行的查询关联。|  
|**plan_handle**|**varbinary(64)**|内存中计划的标识符。 该标识符是瞬态的，仅当计划保留在缓存中时，它才保持不变。 此值可与 **sys.dm_exec_cached_plans** 动态管理视图一起使用。<br /><br /> 当本机编译的存储过程查询内存优化的表时，此项将始终为 0x000。|  
|**cached_time**|**datetime**|存储过程添加到缓存的时间。|  
|**last_execution_time**|**datetime**|上次执行存储过程的时间。|  
|**execution_count**|**bigint**|存储过程自上次编译以来所执行的次数。|  
|**total_worker_time**|**bigint**|此存储过程自编译以来执行所用的 CPU 时间总量（微秒）。<br /><br /> 对于本机编译的存储过程，如果许多执行所用的时间都不到 1 毫秒，则 **total_worker_time** 可能不精确。|  
|**last_worker_time**|**bigint**|上次执行存储过程所用的 CPU 时间（微秒）。 <sup>1</sup>|  
|**min_worker_time**|**bigint**|此存储过程在单次执行期间所用的最小 CPU 时间（微秒）。 <sup>1</sup>|  
|**max_worker_time**|**bigint**|此存储过程在单次执行期间曾占用的最大 CPU 时间（微秒）。 <sup>1</sup>|  
|**total_physical_reads**|**bigint**|此存储过程自编译以来执行的物理读取总次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**last_physical_reads**|**bigint**|上次执行存储过程时所执行的物理读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**min_physical_reads**|**bigint**|此存储过程在单次执行期间所执行的最少物理读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**max_physical_reads**|**bigint**|此存储过程在单次执行期间所执行的最大物理读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**total_logical_writes**|**bigint**|此存储过程自编译以来执行的逻辑写入总次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**last_logical_writes**|**bigint**|上次执行计划时更新的缓冲池页数。 如果页已变脏（已修改），则不计入写次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**min_logical_writes**|**bigint**|此存储过程在单次执行期间所执行的最少逻辑写入次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**max_logical_writes**|**bigint**|此存储过程在单次执行期间所执行的最大逻辑写入次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**total_logical_reads**|**bigint**|此存储过程自编译以来执行的逻辑读取总次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**last_logical_reads**|**bigint**|上次执行存储过程时所执行的逻辑读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**min_logical_reads**|**bigint**|此存储过程在单次执行期间所执行的最少逻辑读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**max_logical_reads**|**bigint**|此存储过程在单次执行期间所执行的最大逻辑读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**total_elapsed_time**|**bigint**|完成此存储过程的执行所用的总时间（微秒）。|  
|**last_elapsed_time**|**bigint**|最近一次完成此存储过程的执行所用的时间（微秒）。|  
|**min_elapsed_time**|**bigint**|任何完成此存储过程的执行所用的最短时间（微秒）。|  
|**max_elapsed_time**|**bigint**|任何完成此存储过程的执行所用的最长时间（微秒）。|  
|**total_spills**|**bigint**|此存储过程自编译以来的执行溢出的总页数。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|  
|**last_spills**|**bigint**|上次执行存储过程时溢出的页数。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|  
|**min_spills**|**bigint**|此存储过程在单次执行期间所溢出的最小页数。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|  
|**max_spills**|**bigint**|此存储过程在单次执行期间所用的最大页数。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|  
|pdw_node_id|**int**|此分发所在的节点的标识符。<br /><br />**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
|**total_page_server_reads**|**bigint**|此存储过程自编译以来执行的页服务器读取的总次数。<br /><br /> **适用** 于： Azure SQL 数据库超大规模|  
|**last_page_server_reads**|**bigint**|上次执行存储过程时所执行的页服务器读取次数。<br /><br /> **适用** 于： Azure SQL 数据库超大规模|  
|**min_page_server_reads**|**bigint**|此存储过程在单次执行期间所执行的最少页面服务器读取次数。<br /><br /> **适用** 于： Azure SQL 数据库超大规模|  
|**max_page_server_reads**|**bigint**|此存储过程在单次执行期间所执行的最大页服务器读取次数。<br /><br /> **适用** 于： Azure SQL 数据库超大规模|  
  
 <sup>1</sup> 对于启用统计信息收集时的本机编译的存储过程，以毫秒为单位收集工作线程时间。 如果查询执行不到 1 毫秒，则该值将为 0。  
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
   
## <a name="remarks"></a>备注  
 存储过程执行完成后，将更新该视图中的统计信息。  
  
## <a name="examples"></a>示例  
 以下示例返回有关按平均占用时间衡量的前十个存储过程的信息。  
  
```sql  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'proc name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_procedure_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>另请参阅  
[与执行相关的动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
[sys.dm_exec_sql_text &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
[sys.dm_exec_query_plan &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_query_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)    
[sys.dm_exec_trigger_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)    
[sys.dm_exec_cached_plans (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)    
  
  


