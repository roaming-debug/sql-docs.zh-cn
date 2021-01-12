---
description: sys.query_store_plan (Transact-SQL)
title: sys.query_store_plan (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 01/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- QUERY_STORE_PLAN_TSQL
- SYS.QUERY_STORE_PLAN
- SYS.QUERY_STORE_PLAN_TSQL
- QUERY_STORE_PLAN
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_plan catalog view
- sys.query_store_plan catalog view
ms.assetid: b4d05439-6360-45db-b1cd-794f4a64935e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1b13c6f42eb967c6ef5d01e189860c9de3204646
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095459"
---
# <a name="sysquery_store_plan-transact-sql"></a>sys.query_store_plan (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  包含有关与查询关联的每个执行计划的信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|
|**plan_id**|**bigint**|主密钥。|  
|**query_id**|**bigint**|外键。 联接到 [&#40;transact-sql&#41;sys.query_store_query ](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)。|  
|**plan_group_id**|**bigint**|计划组的 ID。 游标查询通常需要多 (填充和提取) 计划。 填充和提取一起编译的计划位于同一个组中。<br /><br /> 0表示计划不在组中。|  
|**engine_version**|**nvarchar(32)**|用于以 **"主版本. 内部版本. 修订版本"** 格式编译计划的引擎版本。|  
|**compatibility_level**|**smallint**|查询中引用的数据库的数据库兼容级别。|  
|**query_plan_hash**|**二进制 (8)**|单个计划的 MD5 哈希。|  
|**query_plan**|**nvarchar(max)**|查询计划的显示计划 XML。|  
|**is_online_index_plan**|**bit**|在联机索引生成过程中使用了 Plan。 <br/>**注意：** Azure Synapse Analytics 将始终返回零 (0) 。|  
|**is_trivial_plan**|**bit**|计划是查询优化器) 的阶段0中 (输出的普通计划。 <br/>**注意：** Azure Synapse Analytics 将始终返回零 (0) 。|  
|**is_parallel_plan**|**bit**|计划是并行的。 <br/>**注意：** Azure Synapse Analytics 将始终返回一个 (1) 。|  
|**is_forced_plan**|**bit**|当用户执行存储过程 **sys.sp_query_store_force_plan** 时，计划将标记为 "强制"。 强制机制 *并不保证* 确切地将此计划用于 **query_id** 引用的查询。 计划强制再次编译查询，并通常为 **plan_id** 所引用的计划生成完全相同或类似的计划。 如果计划强制不成功，则 **force_failure_count** 会递增，并且 **last_force_failure_reason** 会按失败原因进行填充。 <br/>**注意：** Azure Synapse Analytics 将始终返回零 (0) 。|  
|**is_natively_compiled**|**bit**|计划包括本机编译的内存优化过程。  (0 = FALSE，1 = TRUE) 。 <br/>**注意：** Azure Synapse Analytics 将始终返回零 (0) 。|  
|**force_failure_count**|**bigint**|强制此计划失败的次数。 仅当重新编译查询时，它才会递增 (*不会在每次执行*) 时递增。 每次将 **is_plan_forced** 从 **FALSE** 更改为 **TRUE** 时，它都会重置为0。 <br/>**注意：** Azure Synapse Analytics 将始终返回零 (0) 。|  
|**last_force_failure_reason**|**int**|计划强制失败的原因。<br /><br /> 0：无故障，否则导致强制失败的错误的错误号<br /><br /> 8637： ONLINE_INDEX_BUILD<br /><br /> 8683： INVALID_STARJOIN<br /><br /> 8684： TIME_OUT<br /><br /> 8689： NO_DB<br /><br /> 8690： HINT_CONFLICT<br /><br /> 8691： SETOPT_CONFLICT<br /><br /> 8694： DQ_NO_FORCING_SUPPORTED<br /><br /> 8698： NO_PLAN<br /><br /> 8712： NO_INDEX<br /><br /> 8713： VIEW_COMPILE_FAILED<br /><br /> \<other value>： GENERAL_FAILURE <br/>**注意：** Azure Synapse Analytics 将始终返回零 (0) 。|  
|**last_force_failure_reason_desc**|**nvarchar(128)**|Last_force_failure_reason_desc 的文本说明。<br /><br /> ONLINE_INDEX_BUILD：查询在目标表具有正在联机生成的索引时尝试修改数据<br /><br /> INVALID_STARJOIN：计划包含无效的 StarJoin 规范<br /><br /> TIME_OUT：优化器在搜索由强制计划指定的计划时超出了允许的操作数<br /><br /> NO_DB：在计划中指定的数据库不存在<br /><br /> HINT_CONFLICT：无法编译查询，因为计划与查询提示冲突<br /><br /> DQ_NO_FORCING_SUPPORTED：无法执行查询，因为计划与分布式查询或全文操作的使用冲突。<br /><br /> NO_PLAN：查询处理器无法生成查询计划，因为无法验证强制计划是否对查询有效<br /><br /> NO_INDEX：计划中指定的索引已不存在<br /><br /> VIEW_COMPILE_FAILED：由于在计划中引用的索引视图中存在问题，无法强制执行查询计划<br /><br /> GENERAL_FAILURE：上述原因未涵盖 (常规强制错误)  <br/>**注意：** Azure Synapse Analytics 将始终返回 *NONE*。|  
|**count_compiles**|**bigint**|规划编译统计信息。|  
|**initial_compile_start_time**|**datetimeoffset**|规划编译统计信息。|  
|**last_compile_start_time**|**datetimeoffset**|规划编译统计信息。|  
|**last_execution_time**|**datetimeoffset**|上次执行时间是指查询/计划的最后结束时间。|  
|**avg_compile_duration**|**float**|规划编译统计信息。 <br/>**注意：** Azure Synapse Analytics 将始终返回零 (0) 。|  
|**last_compile_duration**|**bigint**|规划编译统计信息。 <br/>**注意：** Azure Synapse Analytics 将始终返回零 (0) 。|  
|**plan_forcing_type**|**int**|计划强制类型。<br /><br />0：无<br /><br />1：手动<br /><br />2：自动|  
|**plan_forcing_type_desc**|**nvarchar(60)**|Plan_forcing_type 的文本说明。<br /><br />无：无计划强制<br /><br />手动：由用户强制执行的计划<br /><br />自动：由自动优化强制执行计划|  

## <a name="plan-forcing-limitations"></a>计划强制限制
查询存储中具有一种可用于强制查询优化器使用特定执行计划的机制。 但是，有些限制可能会阻止计划强制执行。 

首先，计划是否包含以下构造：
* INSERT BULK 语句。
* 对外部表的引用
* 分布式查询或全文操作
* 使用全局查询 
* 动态或键集游标 
* 无效的星型联接规范 

> [!NOTE]
> Azure SQL Database 和 SQL Server 2019 支持计划强制用于静态和快进游标。

其次，计划依赖的对象何时不再可用：
* 数据库（计划来自的数据库不再存在时）
* 索引（不再存在或已禁用）

最后，计划本身的问题：
* 用于查询不合法
* 查询优化器超出了允许的操作数
* 格式不正确的计划 XML

## <a name="permissions"></a>权限  
 需要 **VIEW DATABASE STATE** 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.database_query_store_options &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_query &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [相关视图、函数和过程](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询存储存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
  
  
