---
title: 可用性组超过了 RPO
description: Always On 可用性组超过恢复点目标 (RPO) 时的常见问题和解决方法
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: 38de1841-9c99-435a-998d-df81c7ca0f1e
author: rothja
ms.author: jroth
ms.openlocfilehash: f70696592cae8f486e7ff7b52e417bb67edfa8d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337923"
---
# <a name="troubleshoot-availability-group-exceeded-rpo"></a>排除故障：可用性组超过了 RPO
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  在可用性组上执行强制手动故障转移到异步提交次要副本后，可能会发现数据丢失超过了恢复点目标 (RPO)。 或者，在使用 [Always On 可用性组的监视性能](monitor-performance-for-always-on-availability-groups.md)中的方法计算异步提交次要副本可能的数据丢失时，发现它超过了 RPO。  
  
 同步提交次要副本可保证零数据丢失，但异步提交次要副本可能的数据丢失取决于次要副本上仍在等待强化的日志数量。  
  
 以下各节描述了异步提交次要副本时数据丢失的可能性较高的常见原因，假定服务器实例中没有与可用性组无关的系统性能问题。  
  
1.  [高网络延迟或低网络吞吐量导致主要副本上日志堆积](#BKMK_LATENCY)  
  
2.  [磁盘 I/O 瓶颈降低次要副本上的强化速度](#BKMK_IO_BOTTLENECK)  
  
##  <a name="high-network-latency-or-low-network-throughput-causes-log-build-up-on-the-primary-replica"></a><a name="BKMK_LATENCY"></a>高网络延迟或低网络吞吐量导致主要副本上日志堆积  
 数据库超过其 RPO 的最常见原因是：无法以足够快的速度将数据库发送到次要副本。  
  
### <a name="explanation"></a>说明  
 如果日志发送包含的消息数超出了允许向次要副本发送的最大未确认消息数，主要副本将激活对日志发送的流控制。 在部分这些消息得到确认前，无法再向次要副本发送日志块。 由于仅当在次要副本上强化时才可以防止数据丢失，因此未发送的日志消息堆积可能会增大数据丢失的可能性。  
  
### <a name="diagnosis-and-resolution"></a>诊断和解决方法  
 重新发送到次要副本的大量消息可能指示较高的网络延迟和网络干扰。 还可以将 DMV 值 log_send_rate 与性能对象 Log Bytes Flushed/sec 进行比较。如果日志刷新到磁盘的速度比发送速度快，则数据丢失的可能性可能会无限增加  。  
  
 此外，检查 `SQL Server:Availability Replica > Flow Control Time (ms/sec)` 和 `SQL Server:Availability Replica > Flow Control/sec` 这两个性能对象也很有用。 将这两个值相乘，可得到最后一秒中等待清除流控制所花费的时间。 流控制等待时间越长，发送速率越低。  
  
 以下指标对诊断网络延迟和吞吐量很有用。 可以使用其他 Windows 工具（如 ping.exe 和[网络监视器](https://www.microsoft.com/p/network-monitor-pro-free-edition/9n8gdvj32gp7)）来评估延迟和网络利用率  。  
  
-   DMV `sys.dm_hadr_database_replica_states, log_send_queue_size`  
  
-   DMV `sys.dm_hadr_database_replica_states, log_send_rate`  
  
-   性能计数器 `SQL Server:Database > Log Bytes Flushed/sec`  
  
-   性能计数器 `SQL Server:Database Mirroring > Send/Receive Ack Time`  
  
-   性能计数器 `SQL Server:Availability Replica > Bytes Sent to Replica/sec`  
  
-   性能计数器 `SQL Server:Availability Replica > Bytes Sent to Transport/sec`  
  
-   性能计数器 `SQL Server:Availability Replica > Flow Control Time (ms/sec)`  
  
-   性能计数器 `SQL Server:Availability Replica > Flow Control/sec`  
  
-   性能计数器 `SQL Server:Availability Replica > Resent Messages/sec`  

要解决此问题，请尝试升级网络带宽，或者删除或减少不必要的网络流量。  


##  <a name="disk-io-bottleneck-slows-down-log-hardening-on-the-secondary-replica"></a><a name="BKMK_IO_BOTTLENECK"></a>磁盘 I/O 瓶颈降低次要副本上的强化速度  
 根据数据库文件部署，日志强化速度可能因与报告工作负荷的 I/O 争用而降低。  
  
### <a name="explanation"></a>说明  
 只要在日志文件上强化了日志块，就可以防止数据丢失。 因此，将日志文件与数据文件隔离十分重要。 如果日志文件和数据文件映射到同一个硬盘，则在数据文件上进行大量读取的报告工作负荷将使用日志强化操作所需的相同 I/O 资源。 缓慢的日志强化可能会导致对主要副本的确认减慢，而这又可能导致流控制激活过多、流控制等待时间较长。  
  
### <a name="diagnosis-and-resolution"></a>诊断和解决方法  
 如果已验证了网络不存在高延迟或低吞吐量，则应调查次要副本的 I/O 争用。 [SQL Server：最小化磁盘 I/O](/previous-versions/technet-magazine/jj643251(v=msdn.10)) 中的查询对于识别争用很有帮助。 为方便起见，下面列出了该文章中的例子。  
  
 以下脚本可用于查看在 SQL Server 的实例上运行的每个可用性数据库的每个数据和日志文件的读取和写入数量。 按平均 I/O 停滞时间（以毫秒为单位）进行排序。 请注意，这些数字是自上次启动服务器实例起的累积数。 因此，经过一段时间后，应取得两次度量之间的差值。  
  
```sql  
SELECT DB_NAME(database_id) AS   
   [Database Name] ,   
   file_id ,   
   io_stall_read_ms ,   
   num_of_reads ,   
   CAST(io_stall_read_ms / ( 1.0 + num_of_reads ) AS NUMERIC(10, 1)) AS [avg_read_stall_ms] ,   
   io_stall_write_ms ,   
   num_of_writes ,  
   CAST(io_stall_write_ms / ( 1.0 + num_of_writes ) AS NUMERIC(10, 1)) AS [avg_write_stall_ms] ,   
   io_stall_read_ms + io_stall_write_ms AS [io_stalls] ,   
   num_of_reads + num_of_writes AS [total_io] ,   
   CAST(( io_stall_read_ms + io_stall_write_ms ) / ( 1.0 + num_of_reads  
+ num_of_writes) AS NUMERIC(10,1)) AS [avg_io_stall_ms]  
FROM sys.dm_io_virtual_file_stats(NULL, NULL)  
WHERE DB_NAME(database_id) IN (SELECT DISTINCT database_name FROM sys.dm_hadr_database_replica_cluster_states)  
ORDER BY avg_io_stall_ms DESC;  
```  
  
 下一个查询提供了系统上挂起的 I/O 请求的时间点（非累积）快照。  
  
```sql  
SELECT DB_NAME(mf.database_id) AS [Database] ,   
   mf.physical_name ,  
   r.io_pending ,   
   r.io_pending_ms_ticks ,   
   r.io_type ,   
   fs.num_of_reads ,   
   fs.num_of_writes  
FROM sys.dm_io_pending_io_requests AS r   
INNER JOIN sys.dm_io_virtual_file_stats(NULL, NULL) AS fs ON r.io_handle = fs.file_handle   
INNER JOIN sys.master_files AS mf ON fs.database_id = mf.database_id  
AND fs.file_id = mf.file_id  
ORDER BY r.io_pending , r.io_pending_ms_ticks DESC;  
```  
  
 可通过比较读取 I/O 和写入 I/O 如何互相匹配来识别 I/O 争用。  
  
 以下是有助于诊断 I/O 瓶颈的一些其他性能计数器：  
  
-   **物理磁盘：所有计数器**  
  
-   **物理磁盘：平均Disk sec/Transfer**  
  
-   SQL Server：  数据库 > 日志刷新等待时间  
  
-   SQL Server：  数据库 > 日志刷新等待时间/秒  
  
-   SQL Server：  数据库 > 日志池磁盘读取数/秒  
  
 如果确定了 I/O 瓶颈，并且将日志文件和数据文件放在同一硬盘上，首先需要将数据文件和日志文件放在不同的磁盘上。 此最佳做法可防止报告工作负荷干扰从主要副本到日志缓冲区的日志传输路径，以及其强化辅助副本上事务的功能。  
  
## <a name="next-steps"></a>后续步骤  
 [排除 SQL Server 中的性能问题（适用于 SQL Server 2012）](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100))  
  
