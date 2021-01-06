---
title: 疑难解答：可用性组超过了 RTO (SQL Server) | Microsoft Docs
description: 了解当故障转移花费的时间超过在 SQL Server 中的恢复时间目标时，如何对 Always On 可用性组上的故障转移进行故障排除。
ms.custom: ag-guide
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: e83e4ef8-92f0-406f-bd0b-dc48dc210517
author: rothja
ms.author: jroth
ms.openlocfilehash: 5847162edb0d962826931624f4d14a3d78468066
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641873"
---
# <a name="troubleshoot-availability-group-exceeded-rto"></a>疑难解答：可用性组超过了 RTO
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  对可用性组进行自动故障转移或计划的手动故障转移（无数据丢失）后，可能会发现故障转移时间超过恢复时间目标 (RTO)。 或者，在使用[监视 Always On 可用性组的性能](monitor-performance-for-always-on-availability-groups.md)中的方法估计同步提交次要副本的故障转移时间时（如自动故障转移伙伴），发现该时间超过了 RTO。  
  
 如果自动故障转移仍未完成，请参阅[故障排除 SQL Server 2012 Always On 环境中的自动故障转移问题](https://support.microsoft.com/kb/2833707)。  
  
 以下各节介绍超过 RTO 的故障转移时间的常见原因。  
  
1.  [报告工作负荷阻止重做线程运行](#BKMK_REDOBLOCK)  
  
2.  [重做线程因资源争用而滞后](#BKMK_CONTENTION)  
  
##  <a name="reporting-workload-blocks-the-redo-thread-from-running"></a><a name="BKMK_REDOBLOCK"></a>报告工作负荷阻止重做线程运行  
 一个长时间运行的只读查询阻止次要副本上的重做线程执行数据定义语言 (DDL) 更改。  
  
### <a name="explanation"></a>说明  
 在次要副本上，只读查询可获取架构稳定性 (`Sch-S`) 锁定。 这些 `Sch-S` 锁定可阻止重做线程获取架构修改 (`Sch-M`) 锁定并进行任何 DDL 修改。 必须先取消阻止重做线程才可应用日志记录。 取消阻止后，可继续追赶到日志结尾，并允许后续撤销和故障转移过程继续。  
  
### <a name="diagnosis-and-resolution"></a>诊断和解决方法  
 如果阻止重做线程，将生成名为 `sqlserver.lock_redo_blocked` 的扩展事件。 此外，可在次要副本上查询 DMV sys.dm_exec_request，以找出哪个会话正在阻止重做线程，然后可以采取纠正措施。 下面的查询返回阻止重做线程的只读查询的会话 ID。  
  
```sql  
select session_id, command, blocking_session_id, wait_time, wait_type, wait_resource   
from sys.dm_exec_requests where command = 'DB STARTUP'  
```  
  
 可以让报告工作负荷在取消阻止重做线程的点完成，或者通过对阻止会话 ID 执行 [KILL (Transact-SQL)](~/t-sql/language-elements/kill-transact-sql.md) 命令，立即取消阻止重做线程。  
  
##  <a name="redo-thread-falls-behind-due-to-resource-contention"></a><a name="BKMK_CONTENTION"></a>重做线程因资源争用而滞后  
 次要副本上的大型报告工作负荷降低了次要副本的性能，且重做线程已滞后。  
  
### <a name="explanation"></a>说明  
 如果对次要副本应用日志记录，重做线程将读取日志磁盘中的日志记录，然后对于每个日志记录，该线程将访问数据页，以应用日志记录。 如果页面并非已在缓冲池中，页面访问可能受到 I/O 限制（访问物理磁盘）。 如果存在受到 I/O 限制的报告工作负荷，该工作负荷将与重做线程争用 I/O 资源，可能会降低重做线程的速度。  
  
### <a name="diagnosis-and-resolution"></a>诊断和解决方法  
 可以使用以下 DMV 查询查看重做线程的滞后程度，方法是测量 `last_redone_lsn` 和 `last_received_lsn` 之间的差异。  
  
```sql  
select recovery_lsn, truncation_lsn, last_hardened_lsn, last_received_lsn,   
   last_redone_lsn, last_redone_time  
from sys.dm_hadr_database_replica_states  
  
```  
  
 如果重做线程确实滞后，需要调查次要副本性能降低的根本原因。 如果存在与报告工作负荷的 I/O 争用，可以使用 [Resource Governor](~/relational-databases/resource-governor/resource-governor.md) 来控制报告工作负荷所用的 CPU 周期，以在某种程度上间接控制采用的 I/O 周期。 例如，如果报告工作负荷要使用 10% 的 CPU，但工作负荷受到 I/O 限制，可以使用 Resource Governor 将 CPU 资源使用量限制为中止读取工作负荷的 5%，这可最大程度降低对 I/O 的影响。  
  
## <a name="next-steps"></a>后续步骤  
 [排除 SQL Server 中的性能问题（适用于 SQL Server 2012）](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100))  
  
