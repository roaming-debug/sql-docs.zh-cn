---
description: sys.dm_exec_query_stats (Transact-SQL)
title: sys.dm_exec_query_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_stats_TSQL
- dm_exec_query_stats
- sys.dm_exec_query_stats
- sys.dm_exec_query_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_stats dynamic management view
ms.assetid: eb7b58b8-3508-4114-97c2-d877bcb12964
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a3097ba3a920a049dd69b54a8a9f7b84638ff3fb
ms.sourcegitcommit: 2991ad5324601c8618739915aec9b184a8a49c74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97328037"
---
# <a name="sysdm_exec_query_stats-transact-sql"></a>sys.dm_exec_query_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中缓存查询计划的聚合性能统计信息。 缓存计划中的每个查询语句在该视图中对应一行，并且行的生存期与计划本身相关联。 在从缓存删除计划时，也将从该视图中删除对应行。  
  
> [!NOTE]
> - 每次执行时， **sys.dm_exec_query_stats**  的结果可能会有所不同，因为数据只反映完成的查询，而不是仍在进行中的查询。
> - 若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_exec_query_stats**。    

  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**sql_handle**|**varbinary(64)**  |是唯一标识查询所属的批处理或存储过程的标记。<br /><br /> 通过调用 **sys.dm_exec_sql_text** 动态管理函数，**sql_handle** 可以和 **statement_start_offset** 及 **statement_end_offset** 一起用于检索查询的 SQL 文本。|  
|**statement_start_offset**|**int**|指示行所说明的查询在其批查询或持久化对象文本中的开始位置（以字节为单位，从 0 开始）。|  
|**statement_end_offset**|**int**|指示行所说明的查询在其批查询或持久化对象文本中的结束位置（以字节为单位，从 0 开始）。 对于之前的版本 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ，值-1 指示批处理的结束。 不再包含尾随的注释。|  
|**plan_generation_num**|**bigint**|可用于在重新编译后区分不同计划实例的序列号。|  
|**plan_handle**|**varbinary(64)**|是一个标记，用于唯一标识已执行并且其计划驻留在计划缓存中或当前正在执行的批处理的查询执行计划。 可以将此值传递给 [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md) 动态管理函数来获取查询计划。<br /><br /> 当本机编译的存储过程查询内存优化的表时，此项将始终为 0x000。|  
|**creation_time**|**datetime**|编译计划的时间。|  
|**last_execution_time**|**datetime**|上次开始执行计划的时间。|  
|**execution_count**|**bigint**|计划自上次编译以来所执行的次数。|  
|**total_worker_time**|**bigint**|此计划自编译以来执行所用的 CPU 时间总量（以微秒为单位报告，但仅精确到毫秒）。<br /><br /> 对于本机编译的存储过程，如果许多执行所用的时间都不到 1 毫秒，则 **total_worker_time** 可能不精确。|  
|**last_worker_time**|**bigint**|上次执行计划所用的 CPU 时间（以微秒为单位报告，但仅精确到毫秒）。 <sup>1</sup>|  
|**min_worker_time**|**bigint**|此计划在单次执行期间所用的最小 CPU 时间（以微秒为单位报告，但仅精确到毫秒）。 <sup>1</sup>|  
|**max_worker_time**|**bigint**|此计划在单次执行期间所用的最大 CPU 时间（以微秒为单位报告，但仅精确到毫秒）。 <sup>1</sup>|  
|**total_physical_reads**|**bigint**|此计划自编译后在执行期间所执行的物理读取总次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**last_physical_reads**|**bigint**|上次执行计划时所执行的物理读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**min_physical_reads**|**bigint**|此计划在单个执行期间所执行的最少物理读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**max_physical_reads**|**bigint**|此计划在单个执行期间所执行的最多物理读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**total_logical_writes**|**bigint**|此计划自编译后在执行期间所执行的逻辑写入总次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**last_logical_writes**|**bigint**|最近完成的计划执行期间更新的缓冲池页数。<br /><br />读取某个页面后，该页面仅在第一次修改时才会更新。 页面变得脏时，此数字会递增。 对已更新的页面进行的后续修改不会影响此数字。<br /><br />查询内存优化表时，此数字始终为0。|  
|**min_logical_writes**|**bigint**|此计划在单个执行期间所执行的最少逻辑写入次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**max_logical_writes**|**bigint**|此计划在单个执行期间所执行的最多逻辑写入次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**total_logical_reads**|**bigint**|此计划自编译后在执行期间所执行的逻辑读取总次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**last_logical_reads**|**bigint**|上次执行计划时所执行的逻辑读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**min_logical_reads**|**bigint**|此计划在单个执行期间所执行的最少逻辑读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**max_logical_reads**|**bigint**|此计划在单个执行期间所执行的最多逻辑读取次数。<br /><br /> 当查询内存优化的表时，此项将始终为 0。|  
|**total_clr_time**|**bigint**|时间，以微秒为单位报告 (但仅精确到毫秒) ，在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 公共语言运行)  (时内使用此计划自编译后的执行此计划。 CLR 对象可以是存储过程、函数、触发器、类型和聚合。|  
|**last_clr_time**|**bigint**|在上一次执行此计划期间，在 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 公共语言运行时 (CLR) 对象内执行所用的时间（以微秒为单位报告，但仅精确到毫秒）。 CLR 对象可以是存储过程、函数、触发器、类型和聚合。|  
|**min_clr_time**|**bigint**|此计划在单次执行期间在 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] CLR 对象内所用的最小时间（以微秒为单位报告，但仅精确到毫秒）。 CLR 对象可以是存储过程、函数、触发器、类型和聚合。|  
|**max_clr_time**|**bigint**|此计划在单次执行期间在 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] CLR 内所用的最大时间（以微秒为单位报告，但仅精确到毫秒）。 CLR 对象可以是存储过程、函数、触发器、类型和聚合。|  
|**total_elapsed_time**|**bigint**|上次完成执行此计划所用的总时间（以微秒为单位报告，但仅精确到毫秒）。|  
|**last_elapsed_time**|**bigint**|最近一次完成执行此计划所用的时间（以微秒为单位报告，但仅精确到毫秒）。|  
|**min_elapsed_time**|**bigint**|任何一次完成执行此计划所用的最小时间（以微秒为单位报告，但仅精确到毫秒）。|  
|**max_elapsed_time**|**bigint**|任何一次完成执行此计划所用的最大时间（以微秒为单位报告，但仅精确到毫秒）。|  
|**query_hash**|**二进制 (8)**|对查询计算的二进制哈希值，用于标识具有类似逻辑的查询。 可以使用查询哈希确定仅仅是文字值不同的查询的聚合资源使用情况。|  
|**query_plan_hash**|**二进制 (8)**|对查询执行计划计算的二进制哈希值，用于标识类似的查询执行计划。 可以使用查询计划哈希查找具有类似执行计划的查询的累积成本。<br /><br /> 当本机编译的存储过程查询内存优化的表时，此项将始终为 0x000。|  
|**total_rows**|**bigint**|查询返回的总行数。 不能为 null。<br /><br /> 当本机编译的存储过程查询内存优化的表时，此项将始终为 0。|  
|**last_rows**|**bigint**|上一次执行查询返回的行数。 不能为 null。<br /><br /> 当本机编译的存储过程查询内存优化的表时，此项将始终为 0。|  
|**min_rows**|**bigint**|查询在一次执行过程中所返回的最小行数。 不能为 null。<br /><br /> 当本机编译的存储过程查询内存优化的表时，此项将始终为 0。|  
|**max_rows**|**bigint**|一次执行期间查询返回的最大行数。 不能为 null。<br /><br /> 当本机编译的存储过程查询内存优化的表时，此项将始终为 0。|  
|**statement_sql_handle**|**varbinary(64)**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 仅当打开查询存储并为该特定查询收集统计信息时，才填充非 NULL 值。|  
|**statement_context_id**|**bigint**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 仅当打开查询存储并为该特定查询收集统计信息时，才填充非 NULL 值。|  
|**total_dop**|**bigint**|此计划自编译后使用的并行度的总和。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**last_dop**|**bigint**|上一次执行此计划时的并行度。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**min_dop**|**bigint**|此计划在一次执行期间所用的最小并行度。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**max_dop**|**bigint**|此计划在一次执行期间所用的最大并行度。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**total_grant_kb**|**bigint**|此计划自编译后收到的总保留内存授予量（KB）。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**last_grant_kb**|**bigint**|上次执行此计划时的保留内存授予量（KB）。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**min_grant_kb**|**bigint**|在一次执行过程中，此计划以前收到的保留内存授予的最小数量（KB）。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**max_grant_kb**|**bigint**|在一次执行过程中，此计划以前收到的保留内存授予的最大数量（KB）。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**total_used_grant_kb**|**bigint**|此计划自编译以来使用的保留内存授予总数（KB）。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**last_used_grant_kb**|**bigint**|上一次执行此计划时使用的内存授予量（KB）。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**min_used_grant_kb**|**bigint**|在一次执行期间，此计划使用的最小内存授予量（KB）。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**max_used_grant_kb**|**bigint**|在一次执行期间，此计划使用的最大内存授予量（KB）。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**total_ideal_grant_kb**|**bigint**|此计划自编译后估计的理想内存授予总量（KB）。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**last_ideal_grant_kb**|**bigint**|上次执行此计划时的理想内存授予量（KB）。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**min_ideal_grant_kb**|**bigint**|此计划在一次执行期间估计的最小理想内存授予量（KB）。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**max_ideal_grant_kb**|**bigint**|此计划在一次执行期间估计的最大理想内存授予量（KB）。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**total_reserved_threads**|**bigint**|此计划自编译以来曾使用过的保留并行线程的总数。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**last_reserved_threads**|**bigint**|上次执行此计划时保留的并行线程的数目。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**min_reserved_threads**|**bigint**|此计划在一次执行期间所用的保留并行线程的最小数目。  查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**max_reserved_threads**|**bigint**|此计划在一次执行过程中使用的保留并行线程的最大数目。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**total_used_threads**|**bigint**|此计划自编译以来曾使用过的已用并行线程的总数。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**last_used_threads**|**bigint**|上次执行此计划时使用的并行线程数。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**min_used_threads**|**bigint**|此计划在一次执行期间使用的最小并行线程数。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**max_used_threads**|**bigint**|此计划在一次执行过程中使用的最大并行线程数。 查询内存优化表时，它将始终为0。<br /><br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本。|  
|**total_columnstore_segment_reads**|**bigint**|查询读取的列存储段的总数。 不能为 null。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|    
|**last_columnstore_segment_reads**|**bigint**|上次执行查询所读取的列存储段的数目。 不能为 null。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|    
|**min_columnstore_segment_reads**|**bigint**|查询在一次执行期间读取的列存储段的最小数目。 不能为 null。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|    
|**max_columnstore_segment_reads**|**bigint**|查询在一次执行过程中读取的最大列存储段数。 不能为 null。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|    
|**total_columnstore_segment_skips**|**bigint**|查询跳过的列存储段的总数。 不能为 null。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|    
|**last_columnstore_segment_skips**|**bigint**|上次执行查询时跳过的列存储段数。 不能为 null。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|    
|**min_columnstore_segment_skips**|**bigint**|查询在一次执行过程中跳过的列存储段的最小数目。 不能为 null。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|    
|**max_columnstore_segment_skips**|**bigint**|查询在一次执行过程中跳过的列存储段的最大数目。 不能为 null。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|
|**total_spills**|**bigint**|自编译以来执行此查询所溢出的总页数。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|  
|**last_spills**|**bigint**|上次执行查询时溢出的页数。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|  
|**min_spills**|**bigint**|此查询在一次执行期间溢出的最小页数。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|  
|**max_spills**|**bigint**|此查询在一次执行期间溢出的最大页数。<br /><br /> **适用** 于：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始|  
|pdw_node_id|**int**|此分发所在的节点的标识符。<br /><br /> **适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]| 
|**total_page_server_reads**|**bigint**|此计划自编译以来执行的远程页面服务器读取的总次数。<br /><br /> **适用于：** Azure SQL Database 超大规模 |  
|**last_page_server_reads**|**bigint**|上次执行计划时所执行的远程页面服务器读取次数。<br /><br /> **适用于：** Azure SQL Database 超大规模 |  
|**min_page_server_reads**|**bigint**|此计划在单次执行期间所执行的最少远程页面服务器读取次数。<br /><br /> **适用于：** Azure SQL Database 超大规模 |  
|**max_page_server_reads**|**bigint**|此计划在单次执行期间所执行的远程页面服务器读取次数上限。<br /><br /> **适用于：** Azure SQL Database 超大规模 |  
> [!NOTE]
> <sup>1</sup> 对于启用统计信息收集时的本机编译的存储过程，以毫秒为单位收集工作线程时间。 如果查询执行的时间不到1毫秒，则该值将为0。  
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
   
## <a name="remarks"></a>备注  
 查询完成后，将更新该视图中的统计信息。  
  
## <a name="examples"></a>示例  
  
### <a name="a-finding-the-top-n-queries"></a>A. 查找 TOP N 查询  
 下列示例返回了按平均 CPU 时间排名的前五个查询的信息。 此示例将根据查询的查询哈希对查询进行聚合，以便按照查询的累积资源消耗来分组在逻辑上等效的查询。  
  
```sql  
SELECT TOP 5 query_stats.query_hash AS "Query Hash",   
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",  
    MIN(query_stats.statement_text) AS "Statement Text"  
FROM   
    (SELECT QS.*,   
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,  
    ((CASE statement_end_offset   
        WHEN -1 THEN DATALENGTH(ST.text)  
        ELSE QS.statement_end_offset END   
            - QS.statement_start_offset)/2) + 1) AS statement_text  
     FROM sys.dm_exec_query_stats AS QS  
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats  
GROUP BY query_stats.query_hash  
ORDER BY 2 DESC;  
```  
  
### <a name="b-returning-row-count-aggregates-for-a-query"></a>B. 对查询返回行计数聚合  
 以下示例返回查询的行计数聚合信息（总行数、最小行数、最大行数和上一次行数）。  
  
```sql  
SELECT qs.execution_count,  
    SUBSTRING(qt.text,qs.statement_start_offset/2 +1,   
                 (CASE WHEN qs.statement_end_offset = -1   
                       THEN LEN(CONVERT(nvarchar(max), qt.text)) * 2   
                       ELSE qs.statement_end_offset end -  
                            qs.statement_start_offset  
                 )/2  
             ) AS query_text,   
     qt.dbid, dbname= DB_NAME (qt.dbid), qt.objectid,   
     qs.total_rows, qs.last_rows, qs.min_rows, qs.max_rows  
FROM sys.dm_exec_query_stats AS qs   
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS qt   
WHERE qt.text like '%SELECT%'   
ORDER BY qs.execution_count DESC;  
```  
  
## <a name="see-also"></a>请参阅  
[与执行相关的动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)    
[sys.dm_exec_sql_text &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)    
[sys.dm_exec_query_plan &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_procedure_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)     
[sys.dm_exec_trigger_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)     
[sys.dm_exec_cached_plans (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)    
  


