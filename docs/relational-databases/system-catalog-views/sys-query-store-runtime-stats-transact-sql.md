---
description: 'sys.query_store_runtime_stats (Transact-sql) '
title: sys.query_store_runtime_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 07/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- SYS.QUERY_STORE_RUNTIME_STATS_TSQL
- QUERY_STORE_RUNTIME_STATS_TSQL
- SYS.QUERY_STORE_RUNTIME_STATS
- QUERY_STORE_RUNTIME_STATS
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_runtime_stats catalog view
- sys.query_store_runtime_stats catalog view
ms.assetid: ccf7a57c-314b-450c-bd34-70749a02784a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a6f0f21100ca68575c8db8dcde8e70f688d0ef5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098001"
---
# <a name="sysquery_store_runtime_stats-transact-sql"></a>sys.query_store_runtime_stats (Transact-sql) 
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  包含有关查询的运行时执行统计信息的信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**runtime_stats_id**|**bigint**|表示 **plan_id**、 **execution_type** 和 **runtime_stats_interval_id** 的运行时执行统计信息的行标识符。 只有过去运行时统计信息间隔才是唯一的。 对于当前处于活动状态的时间间隔，可能有多个行表示 **plan_id** 引用的计划的运行时统计信息，并且执行类型由 **execution_type** 表示。 通常，一行代表刷新到磁盘上的运行时统计信息，而其他 () 表示内存中状态。 因此，若要获取每个间隔的实际状态，需要聚合指标，按 **plan_id**、 **execution_type** 和 **runtime_stats_interval_id** 进行分组。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**plan_id**|**bigint**|外键。 联接到 [&#40;transact-sql&#41;sys.query_store_plan ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)。|  
|**runtime_stats_interval_id**|**bigint**|外键。 联接到 [&#40;transact-sql&#41;sys.query_store_runtime_stats_interval ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)。|  
|**execution_type**|**tinyint**|确定查询执行的类型：<br /><br /> 0-常规执行 (成功完成) <br /><br /> 3-客户端启动的已中止执行<br /><br /> 4-异常中止执行|  
|**execution_type_desc**|**nvarchar(128)**|执行类型字段的文本说明：<br /><br /> 0-常规<br /><br /> 3-已中止<br /><br /> 4-异常|  
|**first_execution_time**|**datetimeoffset**|查询计划在聚合间隔内的第一次执行时间。 这是指查询执行的结束时间。|  
|**last_execution_time**|**datetimeoffset**|查询计划在聚合间隔内的上次执行时间。 这是指查询执行的结束时间。|  
|**count_executions**|**bigint**|聚合间隔内查询计划执行的总次数。|  
|**avg_duration**|**float**|聚合间隔内查询计划的平均持续时间，以微秒)  (报告。|  
|**last_duration**|**bigint**|聚合间隔内查询计划的上次持续时间（以微秒)  (报告）。|  
|**min_duration**|**bigint**|聚合间隔内查询计划的最短持续时间（以微秒)  (报告）。|  
|**max_duration**|**bigint**|聚合间隔内查询计划的最长持续时间（以微秒)  (报告）。|  
|**stdev_duration**|**float**|聚合间隔内查询计划的持续时间标准偏差 (以微秒为单位报告) 。|  
|**avg_cpu_time**|**float**|聚合间隔内查询计划的平均 CPU 时间（以微秒)  (报告）。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**last_cpu_time**|**bigint**|聚合间隔内查询计划的上次 CPU 时间（以微秒为单位 (报告）) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**min_cpu_time**|**bigint**|聚合间隔内查询计划的最小 CPU 时间（以微秒)  (报告）。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**max_cpu_time**|**bigint**|聚合间隔内查询计划的最大 CPU 时间（以微秒)  (报告）。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**stdev_cpu_time**|**float**|聚合间隔内查询计划的 CPU 时间标准偏差 (以微秒为单位报告) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**avg_logical_io_reads**|**float**|聚合间隔内查询计划的平均逻辑 i/o 数。  (表示为多个8KB 页面，) 读取。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**last_logical_io_reads**|**bigint**|聚合间隔内查询计划的最后逻辑 i/o 读取次数。  (表示为多个8KB 页面，) 读取。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**min_logical_io_reads**|**bigint**|聚合间隔内查询计划的最小逻辑 i/o 读取次数。  (表示为多个8KB 页面，) 读取。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**max_logical_io_reads**|**bigint**|聚合间隔内查询计划的最大逻辑 i/o 读取次数。 (表示为多个8KB 页面读取) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**stdev_logical_io_reads**|**float**|逻辑 i/o 读取聚合间隔内查询计划的标准偏差。  (表示为多个8KB 页面，) 读取。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**avg_logical_io_writes**|**float**|聚合间隔内查询计划的平均逻辑 i/o 写入数。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**last_logical_io_writes**|**bigint**|聚合间隔内查询计划的最后逻辑 i/o 写入次数。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**min_logical_io_writes**|**bigint**|聚合间隔内查询计划的最小逻辑 i/o 写入次数。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**max_logical_io_writes**|**bigint**|聚合间隔内查询计划的逻辑 i/o 写入最大次数。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**stdev_logical_io_writes**|**float**|逻辑 i/o 数在聚合间隔内写入查询计划的标准偏差。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**avg_physical_io_reads**|**float**|聚合间隔内查询计划的平均物理 i/o 读取次数 (表示为多个8KB 页面读取) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**last_physical_io_reads**|**bigint**|聚合间隔内查询计划的最后物理 i/o 读取次数 (表示为多个8KB 页面读取) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**min_physical_io_reads**|**bigint**|聚合间隔内查询计划的最小物理 i/o 读取次数 (表示为多个8KB 页面读取) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**max_physical_io_reads**|**bigint**|聚合间隔内查询计划的最大物理 i/o 读取次数 (表示为多个8KB 页面读取) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**stdev_physical_io_reads**|**float**|物理 i/o 读取聚合间隔内查询计划的标准偏差 (表示为多个8KB 页面读取) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**avg_clr_time**|**float**|聚合间隔内查询计划的平均 CLR 时间 (以微秒为单位报告) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**last_clr_time**|**bigint**|聚合间隔内查询计划的上次 CLR 时间 (以微秒为单位报告) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**min_clr_time**|**bigint**|聚合间隔内查询计划的最小 CLR 时间 (以微秒为单位报告) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**max_clr_time**|**bigint**|聚合间隔内查询计划的最大 CLR 时间 (以微秒为单位报告) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**stdev_clr_time**|**float**|聚合间隔内查询计划的 CLR 时间标准偏差 (以微秒为单位报告) 。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**avg_dop**|**float**|聚合间隔内查询计划) 平均 DOP (并行度。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**last_dop**|**bigint**|上次 DOP (聚合间隔内查询计划) 的并行度。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**min_dop**|**bigint**|聚合间隔内查询计划) 的最小 DOP (并行度。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**max_dop**|**bigint**|最大 DOP (了聚合间隔内查询计划) 的并行度。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**stdev_dop**|**float**|DOP (聚合间隔内查询计划的标准偏差) 并行度。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**avg_query_max_used_memory**|**float**|平均内存授予 (报告为聚合间隔内查询计划) 的 8 KB 页的数目。 对于使用本机编译的内存优化过程的查询，始终为0。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**last_query_max_used_memory**|**bigint**|上次内存授予 (报告为聚合间隔内查询计划) 的 8 KB 页的数目。 对于使用本机编译的内存优化过程的查询，始终为0。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**min_query_max_used_memory**|**bigint**|在聚合间隔内，将 (报告为查询计划) 的 8 KB 页的数目。 对于使用本机编译的内存优化过程的查询，始终为0。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**max_query_max_used_memory**|**bigint**|最大内存授予 (报告为聚合间隔内查询计划) 的 8 KB 页的数目。 对于使用本机编译的内存优化过程的查询，始终为0。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**stdev_query_max_used_memory**|**float**|内存授予标准偏差 (报告为聚合间隔内查询计划) 的 8 KB 页的数目。 对于使用本机编译的内存优化过程的查询，始终为0。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**avg_rowcount**|**float**|聚合间隔内查询计划的返回行的平均数目。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**last_rowcount**|**bigint**|上次在聚合间隔内执行查询计划时返回的行数。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**min_rowcount**|**bigint**|聚合间隔内查询计划返回的最小行数。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**max_rowcount**|**bigint**|聚合间隔内查询计划返回的最大行数。|  
|**stdev_rowcount**|**float**|聚合间隔内查询计划的返回行数标准偏差。|
|**avg_log_bytes_used**|**float**|查询计划在聚合间隔内使用的数据库日志中的平均字节数。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**last_log_bytes_used**|**bigint**|上次执行查询计划时在聚合间隔内使用的数据库日志中的字节数。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**min_log_bytes_used**|**bigint**|查询计划在聚合间隔内使用的数据库日志中的最小字节数。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**max_log_bytes_used**|**bigint**|查询计划在聚合间隔内使用的数据库日志中的最大字节数。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|
|**stdev_log_bytes_used**|**float**|查询计划在聚合间隔内使用的数据库日志中的字节数的标准偏差。<br/>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将始终返回零 (0) 。|  
|**avg_tempdb_space_used**|**float**|聚合间隔内 tempdb 中用于查询计划的平均页数 (表示为多个8KB 页面) 。<br><br/>**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|**last_tempdb_space_used**|**bigint**|聚合间隔内 tempdb 中用于查询计划的最后几个页面数 (表示为多个8KB 页面) 。<br><br/>**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|**min_tempdb_space_used**|**bigint**|聚合间隔内 tempdb 中用于查询计划的最小页数 (表示为多个8KB 页面) 。<br><br/>**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|**max_tempdb_space_used**|**bigint**|聚合间隔内 tempdb 中用于查询计划的最大页数 (表示为多个8KB 页面) 。<br><br/>**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|**stdev_tempdb_space_used**|**float**|聚合间隔内的查询计划的 tempdb 标准偏差中使用的页数 (表示为多个8KB 页面) 。<br><br/>**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|**avg_page_server_io_reads**|**float**|聚合间隔内查询计划的平均页面服务器 i/o 数 (表示为多个8KB 页面读取) 。<br><br/>**适用于：** Azure SQL Database 超大规模</br>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 、 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 、 [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (非超大规模) 将始终返回零 (0) 。|
|**last_page_server_io_reads**|**bigint**|聚合间隔内查询计划的上次页面服务器 i/o 读取次数 (表示为多个8KB 页面读取) 。<br><br/>**适用于：** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 超大规模</br>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 、 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 、 [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (非超大规模) 将始终返回零 (0) 。|
|**min_page_server_io_reads**|**bigint**|聚合间隔内查询计划的最小页服务器 i/o 读取数 (表示为多个8KB 页读取) 。<br><br/>**适用于：** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 超大规模</br>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 、 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 、 [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (非超大规模) 将始终返回零 (0) 。|
|**max_page_server_io_reads**|**bigint**|聚合间隔内查询计划的最大页服务器 i/o 读取数 (表示为多个8KB 页读取) 。<br><br/>**适用于：** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 超大规模</br>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 、 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 、 [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (非超大规模) 将始终返回零 (0) 。|
|**stdev_page_server_io_reads**|**float**|页面服务器 i/o 数在聚合间隔内读取查询计划的标准偏差 (表示为多个8KB 页面读取) 。<br><br/>**适用于：** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 超大规模</br>**注意：** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 、 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 、 [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (非超大规模) 将始终返回零 (0) 。|
  
## <a name="permissions"></a>权限  
需要 `VIEW DATABASE STATE` 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.database_query_store_options &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_wait_stats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [相关视图、函数和过程](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询存储存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)    
 [Query Store 最佳实践](../../relational-databases/performance/best-practice-with-the-query-store.md)   
  
