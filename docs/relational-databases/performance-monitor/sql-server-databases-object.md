---
title: SQL Server - Databases 对象 | Microsoft Docs
description: 了解 SQLServer:Databases 对象，该对象提供计数器来监视大容量复制操作、备份和还原吞吐量以及事务日志活动。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Databases object
- SQLServer:Databases
- Availability Groups [SQL Server], performance counters
ms.assetid: a7f9e7d4-fff4-4c72-8b3e-3f18dffc8919
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0c98d04b9b82feabe1c6ad96cc28faf828223e68
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505737"
---
# <a name="sql-server-databases-object"></a>SQL Server，Databases 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  SQL Server 中的 **SQLServer:Databases** 对象提供了计数器，来监视大容量复制操作、备份和还原吞吐量以及事务日志活动。 监视事务及事务日志确定数据库中进行的用户活动数量及事务日志的饱满程度。 用户活动数量可以确定数据库的性能并影响日志大小、锁和复制。 监视低级日志活动以测量用户活动和资源使用情况，有助于查明性能瓶颈。  
  
 可以同时监视 **Databases** 对象的多个实例，每个实例代表一个数据库。  
  
 下表说明了 SQL Server **Databases** 计数器。  
  
|SQL Server Databases 计数器|说明|  
|-----------------------------------|-----------------|  
|**Active Transactions**|数据库的活动事务数。|  
|**Avg Dist From EOL/LP Request**|对于最后一个 VLF 中的请求，每日志池请求与日志结尾的平均距离（字节）。| 
|**Backup/Restore Throughput/sec**|每秒数据库的备份和还原操作的读取/写入吞吐量。 例如，并行使用多个备份设备或使用更快的设备时，可以测量数据库备份操作性能的变化情况。 数据库的备份或还原操作的吞吐量可以确定备份和还原操作的进程和性能。|  
|**Bulk Copy Rows/sec**|每秒大容量复制的行数。|  
|**Bulk Copy Throughput/sec**|每秒大容量复制的数据量 (KB)。|  
|**Commit table entries**|数据库提交表的内存部分的大小（行数）。 有关详细信息，请参阅 [sys.dm_tran_commit_table (Transact-SQL)](../../relational-databases/system-dynamic-management-views/change-tracking-sys-dm-tran-commit-table.md)。|  
|**Data File(s) Size (KB)**|数据库中所有数据文件的累计大小 (KB)，包括任何自动增长。 监视此计数器非常有用，例如可以确定 **tempdb** 的准确大小。|  
|**DBCC Logical Scan Bytes/sec**|每秒数据库控制台命令 (DBCC) 的逻辑读取扫描字节数。|  
|**Group Commit Time/sec**|每秒的组停滞时间（微秒）。|
|**Log Bytes Flushed/sec**|刷新的日志字节总数。|  
|**Log Cache Hit Ratio**|日志缓存所满足的日志缓存读取数所占的百分比。|  
|**Log Cache Hit Ratio Base**|仅供内部使用。| 
|**Log Cache Reads/sec**|每秒通过日志管理器缓存执行的读取数。|  
|**Log File(s) Size (KB)**|数据库中所有事务日志文件的累计大小 (KB)。|  
|**Log File(s) Used Size (KB)**|数据库中所有日志文件的累计已用大小。|  
|**Log Flush Wait Time**|刷新日志的总等待时间（毫秒）。 在 AlwaysOn 辅助数据库上，此值表示日志记录强制写入磁盘的等待时间。|  
|**Log Flush Waits/sec**|每秒等待日志刷新的提交数目。|  
|**Log Flush Write Time (ms)**|执行在最后一秒完成的写入日志刷新信息的时间（毫秒）。|  
|**Log Flushes/sec**|每秒日志刷新数目。|  
|**Log Growths**|数据库事务日志增长的总次数。|  
|**Log Pool Cache Misses/sec**|日志块在日志池中不可用的请求数。 “日志池”是事务日志的内存中缓存。 这种缓存用于优化对恢复、事务复制、回滚和 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]日志的读取操作。|  
|**Log Pool Disk Reads/sec**|日志池发出的提取日志块的磁盘读取数。|  
|**Log Pool Hash Deletes/sec**|从日志池删除原始哈希条目的速率。|
|**Log Pool Hash Inserts/sec**|原始哈希条目插入日志池的速率。|
|**Log Pool Invalid Hash Entry/sec**|因无效导致的哈希查询失败率。|
|**Log Pool Log Scan Pushes/sec**|日志扫描（可能来自磁盘或内存）的日志块推送速率。|
|**Log Pool LogWriter Pushes/sec**|日志编写器线程的日志块推送速率。|
|**Log Pool Push Empty FreePool/sec**|可用池为空导致的日志块推送失败率。|
|**Log Pool Push Low Memory/sec**|内存过低导致的日志块推送失败率。|
|**Log Pool Push No Free Buffer/sec**|可用缓冲区无法可用导致的日志块推送失败率。|
|**Log Pool Req.Behind Trunc/sec**|日志池缓存由于请求的块位于截断 LSN 之后而未命中。|
|**Log Pool Requests Base**|仅供内部使用。| 
|**Log Pool Requests Old VLF/sec**|不在日志最后一个 VLF 中的日志池请求数。|  
|**Log Pool Requests/sec**|日志池处理的日志块请求数。|  
|**Log Pool Total Active Log Size**|共享缓存缓冲区管理器中存储的当前总活动日志（以字节为单位）。|
|**Log Pool Total Shared Pool Size**|共享缓存缓冲区管理器的当前总内存使用情况（以字节为单位）。|
|**Log Shrinks**|此数据库的日志收缩总数。|  
|**Log Truncations**|事务日志被截断的次数（在简单恢复模式中）。|  
|**Percent Log Used**|日志中已用空间所占的百分比。|  
|**Repl.Pending Xacts**|发布数据库事务日志中已做复制标记但尚未传递到分发数据库的事务数。|  
|**Repl.Trans.Rate**|每秒从发布数据库事务日志中读出并传递到分发数据库的事务数。|  
|**Shrink Data Movement Bytes/sec**|每秒由自动收缩操作或者 DBCC SHRINKDATABASE 或 DBCC SHRINKFILE 语句移动的数据量。|  
|**Tracked transactions/sec**|数据库提交表中记录的已提交的事务数。|  
|**Transactions/sec**|每秒为数据库启动的事务数。<br /><br /> **Transactions/sec** 不对仅限 XTP 的事务（本机编译存储过程启动的事务）进行计数。|  
|**Write Transactions/sec**|在上一秒钟内写入数据库并提交的事务数。|  
|**XTP 控制器 DLC 延迟基准**|仅供内部使用。| 
|**XTP 控制器 DLC 延迟/提取**|输入直接日志使用者和由 XTP 控制器检索的日志块之间的每秒平均延迟（以毫秒为单位）。|
|**XTP 控制器 DLC 最高延迟**|XTP 控制器从直接日志使用者中提取时最高记录的延迟（以毫秒为单位）。|
|**每秒处理的 XTP 控制器日志数**|XTP 控制器线程每秒处理的日志字节数。|
|**已使用的 XTP 内存(KB)**|数据库中由 XTP 使用的内存量。| 
  
## <a name="see-also"></a>另请参阅  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server - 数据库副本](../../relational-databases/performance-monitor/sql-server-database-replica.md)  
  
  
