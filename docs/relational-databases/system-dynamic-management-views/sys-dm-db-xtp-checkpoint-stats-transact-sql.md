---
title: sys.dm_db_xtp_checkpoint_stats (Transact-sql) |Microsoft Docs
description: 返回与当前数据库中的内存中 OLTP 检查点操作有关的统计信息。 了解此视图与 SQL Server 版本的不同之处。
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: reference
f1_keywords:
- dm_db_xtp_checkpoint_stats
- dm_db_xtp_checkpoint_stats_TSQL
- sys.dm_db_xtp_checkpoint_stats
- sys.dm_db_xtp_checkpoint_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_stats dynamic management view
ms.assetid: 8d0b18ca-db4d-4376-9905-3e4457727c46
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b6211155c51a2751e3f4625a4c838bec29695905
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210898"
---
# <a name="sysdm_db_xtp_checkpoint_stats-transact-sql"></a>sys.dm_db_xtp_checkpoint_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  返回与当前数据库中的内存中 OLTP 检查点操作有关的统计信息。 如果数据库中没有内存中 OLTP 对象，则返回空结果集。  
  
 有关详细信息，请参阅[内存中 OLTP&#40;内存中优化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。  
  
```  
USE In_Memory_db_name
SELECT * FROM sys.dm_db_xtp_checkpoint_stats;  
```  
  
**[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 与更新的版本有很大差异，在 [SQL Server 2014](#bkmk_2014)的主题中对此进行了深入讨论。**
  
## <a name="sssql15-and-later"></a>[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本  
 下表对中的列进行了说明 `sys.dm_db_xtp_checkpoint_stats` ，从开始 **[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]** 。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|last_lsn_processed|**bigint**|控制器观察到的最后一个 LSN。|  
|end_of_log_lsn|**数值 (38)**|日志结尾的 LSN。|  
|bytes_to_end_of_log|**bigint**|控制器处理的日志字节数，对应于和之间的字节数 `last_lsn_processed` `end_of_log_lsn` 。|  
|log_consumption_rate|**bigint**|控制器 (的事务日志消耗速率（KB/秒) ）。|  
|active_scan_time_in_ms|**bigint**|控制器主动扫描事务日志所花费的时间。|  
|total_wait_time_in_ms|**bigint**|控制器在未扫描日志时的累积等待时间。|  
|waits_for_io|**bigint**|控制器线程引发的日志 IO 等待次数。|  
|io_wait_time_in_ms|**bigint**|控制器线程等待日志 IO 所用的累计时间。|  
|waits_for_new_log_count|**bigint**|控制器线程为生成新日志而产生的等待数。|  
|new_log_wait_time_in_ms|**bigint**|控制器线程等待新日志所用的累计时间。|  
|idle_attempts_count|**bigint**|控制器转换到空闲状态的次数。|  
|tx_segments_dispatched|**bigint**|控制器查看并调度到序列化程序的段数。 段是构成序列化单元的日志的连续部分。 当前已将其大小调整为1MB，但将来可能会更改。|  
|segment_bytes_dispatched|**bigint**|数据库重新启动后，控制器向序列化程序调度的字节总数（字节数）。|  
|bytes_serialized|**bigint**|自数据库重新启动以来序列化的字节总数。|  
|serializer_user_time_in_ms|**bigint**|序列化程序在用户模式下所用的时间。|  
|serializer_kernel_time_in_ms|**bigint**|序列化程序在内核模式下所用的时间。|  
|xtp_log_bytes_consumed|**bigint**|数据库重新启动后使用的日志字节总数。|  
|checkpoints_closed|**bigint**|自数据库重新启动以来关闭的检查点计数。|  
|last_closed_checkpoint_ts|**bigint**|最后关闭的检查点的时间戳。|  
|hardened_recovery_lsn|**数值 (38)**|将从此 LSN 开始恢复。|  
|hardened_root_file_guid|**uniqueidentifier**|作为上次完成的检查点的结果强制执行的根文件的 GUID。|  
|hardened_root_file_watermark|**bigint**|**仅限内部**。 将根文件读取到 (，这是一个仅限内部相关的类型-称为 BSN) 的有效程度。|  
|hardened_truncation_lsn|**数值 (38)**|截断点的 LSN。|  
|log_bytes_since_last_close|**bigint**|上次接近日志末尾的字节数。|  
|time_since_last_close_in_ms|**bigint**|上次关闭检查点的时间。|  
|current_checkpoint_id|**bigint**|当前已将新段分配到此检查点。 检查点系统是一个管道。 当前检查点是要将日志中的段分配到的检查点。 一旦达到了某个限制，控制器就会释放该检查点，并创建一个新的检查点作为当前。|  
|current_checkpoint_segment_count|**bigint**|当前检查点中段的计数。|  
|recovery_lsn_candidate|**bigint**|**仅限内部**。 在 current_checkpoint_id 关闭时要选取为 recoverylsn 的候选项。|  
|outstanding_checkpoint_count|**bigint**|管道中等待关闭的检查点的数量。|  
|closing_checkpoint_id|**bigint**|结束检查点的 ID。<br /><br /> 序列化程序是并行运行的，因此完成后，检查点就是关闭线程要关闭的候选项。 但关闭线程只能一次关闭一个，并且它必须按顺序关闭，因此结束检查点就是关闭线程正在处理的。|  
|recovery_checkpoint_id|**bigint**|要在恢复中使用的检查点的 ID。|  
|recovery_checkpoint_ts|**bigint**|恢复检查点的时间戳。|  
|bootstrap_recovery_lsn|**数值 (38)**|启动的恢复 LSN。|  
|bootstrap_root_file_guid|**uniqueidentifier**|启动的根文件的 GUID。|  
|internal_error_code|**bigint**|任何控制器、序列化程序、关闭和合并线程均出现错误。|
|bytes_of_large_data_serialized|**bigint**|已序列化的数据量。 |  
  
##  <a name="sssql14"></a><a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 下表对中的列 `sys.dm_db_xtp_checkpoint_stats` 进行了说明 **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** 。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|log_to_process_in_bytes|**bigint**|该线程的当前日志序列号 (LSN) 和日志结尾之间的日志字节数。|  
|total_log_blocks_processed|**bigint**|自服务器启动以来处理的日志块总数。|  
|total_log_records_processed|**bigint**|自服务器启动以来处理的日志记录总数。|  
|xtp_log_records_processed|**bigint**|自服务器启动以来处理的内存中 OLTP 日志记录总数。|  
|total_wait_time_in_ms|**bigint**|累计等待时间（毫秒）。|  
|waits_for_io|**bigint**|日志 IO 的等待数。|  
|io_wait_time_in_ms|**bigint**|等待日志 IO 所用的累计时间。|  
|waits_for_new_log|**bigint**|生成新日志的等待数。|  
|new_log_wait_time_in_ms|**bigint**|等待新日志所用的累计时间。|  
|log_generated_since_last_checkpoint_in_bytes|**bigint**|自上一个内存中 OLTP 检查点以来生成的日志量。|  
|ms_since_last_checkpoint|**bigint**|自上一个内存中 OLTP 检查点以来的时间量（毫秒）。|  
|checkpoint_lsn|**数值 (38)**|与上次完成的内存中 OLTP 检查点关联的恢复日志序列号 (LSN)。|  
|current_lsn|**数值 (38)**|当前正在处理的日志记录的 LSN。|  
|end_of_log_lsn|**数值 (38)**|日志结尾的 LSN。|  
|task_address|**varbinary(8)**|SOS_Task 的地址。 联接到 sys.dm_os_tasks 以查找其他信息。|  
  
## <a name="permissions"></a>权限  
 要求具有对服务器的 `VIEW DATABASE STATE` 权限。  
  
## <a name="see-also"></a>另请参阅  
 [内存优化表动态管理视图 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
