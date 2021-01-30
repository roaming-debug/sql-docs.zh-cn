---
description: sys.dm_exec_trigger_stats (Transact-SQL)
title: sys.dm_exec_trigger_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_trigger_stats
- dm_exec_trigger_stats_TSQL
- sys.dm_exec_trigger_stats_TSQL
- sys.dm_exec_trigger_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_trigger_stats dynamic management function
ms.assetid: 863498b4-849c-434d-b748-837411458738
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eef7f4d6a9dd20dd12d5248ba4b09b1f7031e237
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192927"
---
# <a name="sysdm_exec_trigger_stats-transact-sql"></a>sys.dm_exec_trigger_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回缓存触发器的聚合性能统计信息。 每个触发器在该视图中对应一行，行的生存期与触发器保持缓存状态的时间一样长。 在从缓存中删除触发器时，也将从该视图中删除对应行。 此时，将引发类似于 **sys.dm_exec_query_stats** 的 Performance Statistics SQL 跟踪事件。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|触发器所在的数据库 ID。|  
|object_id|**int**|触发器的对象标识号。|  
|type|**char(2)**|对象的类型：<br /><br /> TA = 程序集 (CLR) 触发器<br /><br /> TR = SQL 触发器|  
|**Type_desc**|**nvarchar(60)**|对对象类型的说明：<br /><br /> CLR_TRIGGER<br /><br /> SQL_TRIGGER|  
|**sql_handle**|**varbinary(64)**|这可用于与在此触发器内执行的 **sys.dm_exec_query_stats** 中的查询相关联。|  
|**plan_handle**|**varbinary(64)**|内存中计划的标识符。 该标识符是瞬态的，仅当计划保留在缓存中时，它才保持不变。 此值可与 **sys.dm_exec_cached_plans** 动态管理视图一起使用。|  
|**cached_time**|**datetime**|触发器添加到缓存的时间。|  
|**last_execution_time**|**datetime**|上次执行触发器的时间。|  
|**execution_count**|**bigint**|触发器自上次编译以来所执行的次数。|  
|**total_worker_time**|**bigint**|此触发器自编译以来执行所用的 CPU 时间总量（微秒）。|  
|**last_worker_time**|**bigint**|上次执行触发器所用的 CPU 时间（微秒）。|  
|**min_worker_time**|**bigint**|此触发器在单次执行期间曾占用的最大 CPU 时间（微秒）。|  
|**max_worker_time**|**bigint**|此触发器在单次执行期间曾占用的最大 CPU 时间（微秒）。|  
|**total_physical_reads**|**bigint**|此触发器自编译以来执行的物理读取总次数。|  
|**last_physical_reads**|**bigint**|上次执行触发器时所执行的物理读取次数。|  
|**min_physical_reads**|**bigint**|此触发器在单次执行期间所执行的最少物理读取次数。|  
|**max_physical_reads**|**bigint**|此触发器在单次执行期间所执行的最大物理读取次数。|  
|**total_logical_writes**|**bigint**|此触发器自编译后执行的逻辑写入总次数。|  
|**last_logical_writes**|**bigint**|上次执行触发器时所执行的逻辑写入次数。|  
|**min_logical_writes**|**bigint**|此触发器在单次执行期间所执行的最少逻辑写入次数。|  
|**max_logical_writes**|**bigint**|此触发器在单次执行期间所执行的最大逻辑写入次数。|  
|**total_logical_reads**|**bigint**|此触发器自编译后执行的逻辑读取总次数。|  
|**last_logical_reads**|**bigint**|上次执行触发器时所执行的逻辑读取次数。|  
|**min_logical_reads**|**bigint**|此触发器在单次执行期间所执行的最少逻辑读取次数。|  
|**max_logical_reads**|**bigint**|此触发器在单次执行期间所执行的最大逻辑读取次数。|  
|**total_elapsed_time**|**bigint**|完成此触发器的执行所用的总时间（微秒）。|  
|**last_elapsed_time**|**bigint**|最近完成此触发器的执行所用的时间（微秒）。|  
|**min_elapsed_time**|**bigint**|完成此触发器的执行所用的最短时间（微秒）。|  
|**max_elapsed_time**|**bigint**|完成此触发器的执行所用的最长时间（微秒）。| 
|**total_spills**|**bigint**|此触发器自编译后的执行溢出的总页数。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|  
|**last_spills**|**bigint**|上次执行触发器时溢出的页数。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|  
|**min_spills**|**bigint**|此触发器在单次执行期间所溢出的最小页数。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|  
|**max_spills**|**bigint**|此触发器在单次执行期间所用的最大页数。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|  
|**total_page_server_reads**|**bigint**|此触发器自编译后执行的总页数。<br /><br /> **适用** 于： Azure SQL 数据库超大规模|  
|**last_page_server_reads**|**bigint**|上次执行触发器时所执行的页服务器读取次数。<br /><br /> **适用** 于： Azure SQL 数据库超大规模|  
|**min_page_server_reads**|**bigint**|此触发器在单次执行期间所执行的最少页数。<br /><br /> **适用** 于： Azure SQL 数据库超大规模|  
|**max_page_server_reads**|**bigint**|此触发器在单次执行期间所执行的最大页服务器读取次数。<br /><br /> **适用** 于： Azure SQL 数据库超大规模|  

  
## <a name="remarks"></a>备注  
 在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 中，动态管理视图不能公开将影响数据库包含的信息，也不能公开有关用户可以访问的其他数据库的信息。 为了避免公开此信息，每个包含不属于所连接的租户的数据的行都将被筛选掉。  

查询完成后，将更新该视图中的统计信息。  
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="examples"></a>示例  
 以下示例返回有关按平均占用时间衡量的前五个触发器的信息。  
  
```sql  
SELECT TOP 5 d.object_id, d.database_id, DB_NAME(database_id) AS 'database_name',   
    OBJECT_NAME(object_id, database_id) AS 'trigger_name', d.cached_time,  
    d.last_execution_time, d.total_elapsed_time,   
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],   
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_trigger_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>另请参阅  
[与执行相关的动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
[sys.dm_exec_sql_text &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
[sys.dm_exec_query_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
[sys.dm_exec_procedure_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)   
[sys.dm_exec_cached_plans (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
  
