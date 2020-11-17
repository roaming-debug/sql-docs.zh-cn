---
title: 可用性组次要副本上不会显示更改
description: 了解如何确定对主要副本的更改在 Always On 可用性组的次要副本上未得到反映的原因。
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
ms.assetid: c602fd39-db93-4717-8f3a-5a98b940f9cc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3a3dee84b2a2e6ee68d2de4630962cf592cefd51
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583720"
---
# <a name="determine-why-changes-from-primary-replica-are-not-reflected-on-secondary-replica-for-an-always-on-availability-group"></a>确定对主要副本的更改在 Always On 可用性组的次要副本上未得到反映的原因
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  客户端应用程序在主要副本上成功完成更新，但查询次要副本显示更改未得到反映。 这种情况下，假定可用性的同步状态正常。 大多数情况下，此问题几分钟后可自行解决。  
  
 如果几分钟后，更改仍未反映在次要副本上映，那么同步工作流中可能出现了瓶颈。 瓶颈位置取决于次要副本是设置为同步提交还是异步提交。  
  
 **同步提交**  
  
 主要副本上每个成功的更新都已同步到次要副本，或已刷新日志记录，以便在次要副本上强化。 因此，瓶颈应位于次要副本上刷新日志后发生的重做过程中。  
  
 但是，一旦捕获重做，次要副本上的所有读取工作负荷都是快照隔离：  
  
  -   次要副本上长时间运行的事务  
  
  -   次要副本上的重做  


**异步提交**  
 
 由于异步提交在事务刷新到本地磁盘时即进行确认，瓶颈可位于该时间点后的任意位置：  
 
  -   次要副本上长时间运行的事务  
  
  -   网络延迟或吞吐量  
  
  -   次要副本上的日志强化  
  
  -   次要副本上的重做  


以下各节介绍对于只读查询，主要副本上的更改未在次要副本上得到反映的常见原因。  


##  <a name="long-running-active-transactions"></a><a name="BKMK_OLDTRANS"></a>长时间运行的活动事务  
 主要副本上长时间运行的事务可阻止在次要副本上读取更新。  
  
### <a name="explanation"></a>说明  
 次要副本上的所有读取工作负荷都是快照隔离查询。 在快照隔离中，只读客户端在重做日志中最早的活动事务的开始点查看次要副本上的可用性数据库。 如果已数小时未提交事务，开放事务将阻止所有只读查询查看任何新更新。  
  
### <a name="diagnosis-and-resolution"></a>诊断和解决方法  
 在主要副本上，使用 [DBCC OPENTRAN (Transact-SQL)](~/t-sql/database-console-commands/dbcc-opentran-transact-sql.md) 查看最早的活动事务，并查看其是否可以回退。 最早的活动事务回退并同步到次要副本后，次要副本上的读取工作负荷可查看可用性数据库中的更新，直到当时最早的活动事务开始。  
  
##  <a name="high-network-latency-or-low-network-throughput-causes-log-build-up-on-the-primary-replica"></a><a name="BKMK_LATENCY"></a>高网络延迟或低网络吞吐量导致主要副本上日志堆积  
 高网络延迟或低网络吞吐量可阻止日志以足够快的速度发送到次要副本。  
  
### <a name="explanation"></a>说明  
 如果日志发送包含的消息数超出了允许向次要副本发送的最大未确认消息数，主要副本将激活对日志发送的流控制。 在部分这些消息得到确认前，无法再向次要副本发送日志块。 这种情况可能对数据丢失的可能性有更严重的影响，可能会损害恢复点目标 (RPO)。  
  
### <a name="diagnosis-and-resolution"></a>诊断和解决方法  
 高 DMV 值 [log_send_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) 可指示主要副本上受阻的日志。 此值除以 [log_send_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) 可大致估计多久之后可在次要副本上捕获数据。  
  
 此外，检查 [SQL Server:Availability Replica > Flow Control Time (ms/sec)](~/relational-databases/performance-monitor/sql-server-availability-replica.md) 和 [SQL Server:Availability Replica > Flow control/sec](~/relational-databases/performance-monitor/sql-server-availability-replica.md) 这两个性能对象也很有用。将这两个值相乘，可得到最后一秒中等待清除流控制所花费的时间。 流控制等待时间越长，发送速率越低。  
  
 下表列出了诊断网络延迟和吞吐量的有用指标。 可以使用其他 Windows 工具（如 ping.exe）来评估网络利用率。  
  
-   DMV [log_send_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   DMV [log_send_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   性能计数器 `SQL Server:Database > Log Bytes Flushed/sec`  
  
-   性能计数器 `SQL Server:Database Mirroring > Send/Receive Ack Time`  
  
-   性能计数器 `SQL Server:Availability Replica > Bytes Sent to Replica/sec`  
  
-   性能计数器 `SQL Server:Availability Replica > Bytes Sent to Transport/sec`  
  
-   性能计数器 `SQL Server:Availability Replica > Flow Control Time (ms/sec)`  
  
-   性能计数器 `SQL Server:Availability Replica > Flow Control/sec`  
  
-   性能计数器 `SQL Server:Availability Replica > Resent Messages/sec`  
  
 要解决此问题，请尝试升级网络带宽，或者删除或减少不必要的网络流量。  
  
##  <a name="another-reporting-workload-blocks-the-redo-thread-from-running"></a><a name="BKMK_REDOBLOCK"></a>另一报告工作负荷阻止重做线程运行  
 一个长时间运行的只读查询阻止次要副本上的重做线程执行数据定义语言 (DDL) 更改。 必须先取消阻止重做线程，然后才可对读取工作负荷进行进一步更新。  
  
### <a name="explanation"></a>说明  
 在次要副本上，只读查询可获取架构稳定性 (`Sch-S`) 锁定。 这些 `Sch-S` 锁定可阻止重做线程获取架构修改 (`Sch-M`) 锁定并进行任何 DDL 修改。 必须先取消阻止重做线程才可应用日志记录。  
  
### <a name="diagnosis-and-resolution"></a>诊断和解决方法  
 如果阻止重做线程，将生成名为 `sqlserver.lock_redo_blocked` 的扩展事件。 此外，可在次要副本上查询 DMV sys.dm_exec_request，以找出哪个会话正在阻止重做线程，然后可以采取纠正措施。 下面的查询返回阻止重做线程的报告工作负荷的会话 ID。  
  
```sql  
select session_id, command, blocking_session_id, wait_time, wait_type, wait_resource   
from sys.dm_exec_requests where command = 'DB STARTUP'  
```  
  
 可以让报告工作负荷在取消阻止重做线程的点完成，或者通过对阻止会话 ID 执行 [KILL (Transact-SQL)](~/t-sql/language-elements/kill-transact-sql.md) 命令，立即取消阻止重做线程。  
  
##  <a name="redo-thread-falls-behind-due-to-resource-contention"></a><a name="BKMK_REDOBEHIND"></a>重做线程因资源争用而滞后  
 次要副本上的大型报告工作负荷降低了次要副本的性能，且重做线程已滞后。  
  
### <a name="explanation"></a>说明  
 如果对次要副本应用日志记录，重做线程将读取日志磁盘中的日志记录，然后对于每个日志记录，该线程将访问数据页，以应用日志记录。 如果页面并非已在缓冲池中，页面访问可能受到 I/O 限制（访问物理磁盘）。 如果存在受到 I/O 限制的报告工作负荷，该工作负荷将与重做线程争用 I/O 资源，可能会降低重做线程的速度。 这种情况不仅会影响其他报告工作负荷查看更新的数据，还会影响 RTO。  
  
### <a name="diagnosis-and-resolution"></a>诊断和解决方法  
 可以使用以下 DMV 查询查看重做线程的滞后程度，方法是测量 `last_redone_lsn` 和 `last_received_lsn` 之间的差异。  
  
```sql  
select recovery_lsn, truncation_lsn, last_hardened_lsn, last_received_lsn,   
   last_redone_lsn, last_redone_time  
from sys.dm_hadr_database_replica_states  
  
```  
  
 如果重做线程确实滞后，需要调查次要副本性能降低的根本原因。 如果存在与报告工作负荷的 I/O 争用，可以使用 [Resource Governor](~/relational-databases/resource-governor/resource-governor.md) 来控制报告工作负荷所用的 CPU 周期，以在某种程度上间接控制采用的 I/O 周期。 例如，如果报告工作负荷要使用 10% 的 CPU，但工作负荷受到 I/O 限制，可以使用 Resource Governor 将 CPU 资源使用量限制为中止读取工作负荷的 5%，这可最大程度降低对 I/O 的影响。  
  
## <a name="next-steps"></a>后续步骤  
 [排除 SQL Server 2008 中的性能问题](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100))
  
