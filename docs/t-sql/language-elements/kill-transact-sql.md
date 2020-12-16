---
description: KILL (Transact-SQL)
title: KILL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/31/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- KILL_TSQL
- KILL
dev_langs:
- TSQL
helpviewer_keywords:
- WITH STATUSONLY option
- terminating distributed transactions
- distributed transactions [SQL Server], terminating
- in-doubt transactions
- IDs [SQL Server], user processes
- ending distributed transactions [SQL Server]
- stopping distributed transactions
- session IDs [SQL Server]
- system process termination [SQL Server]
- stopping processes
- ending processes [SQL Server]
- UOW [SQL Server]
- orphaned transactions [SQL Server]
- unit of work [SQL Server]
- process termination [SQL Server]
- KILL statement
- terminating process
ms.assetid: 071cf260-c794-4b45-adc0-0e64097938c0
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a42d01bead1a5d3882dcce0df67cda7785724b5e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466148"
---
# <a name="kill-transact-sql"></a>KILL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

结束基于会话 ID 或工作单元 (UOW) 的用户进程。 如果指定的会话 ID 或 UOW 有许多工作要撤消，KILL 语句可能需要一段时间才能完成。 此过程可能需要更长时间才能完成，特别是在进程涉及回滚长事务时。  
  
KILL 结束正常连接，这会在内部停止与指定会话 ID 关联的事务。 有时，可能会使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC)。 如果使用 MS DTC，也可以使用此语句来结束孤立的未决分布式事务。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for SQL Server  
  
KILL { session ID | UOW } [ WITH STATUSONLY ]   
```  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
KILL 'session_id'  
[;]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
session ID  
要结束的进程的会话 ID。 session ID 是在建立连接时为每个用户连接分配的唯一整数 (int)。 在连接期间，会话 ID 值与该连接捆绑在一起。 连接结束时，则释放该整数值，并且可以将它重新分配给新的连接。  
以下查询可帮助确定想要终止的 `session_id`：  
 ```sql  
 SELECT conn.session_id, host_name, program_name,
     nt_domain, login_name, connect_time, last_request_end_time 
FROM sys.dm_exec_sessions AS sess
JOIN sys.dm_exec_connections AS conn
    ON sess.session_id = conn.session_id;
```  
  
UOW  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
标识分布式事务的工作单元 ID (UOW)。 UOW 是可以从 sys.dm_tran_locks 动态管理视图的 request_owner_guid 列获取的 GUID。 也可以从错误日志中或通过 MS DTC 监视器获取 UOW。 有关监视分布式事务的详细信息，请参阅 MS DTC 文档。  
  
使用 KILL UOW 可停止孤立的分布式事务。 这些事务不与任何实际会话 ID 相关联，但与会话 ID =“-2”虚拟关联。 可使标识孤立事务变得更为简单，其方法是查询 sys.dm_tran_locks、sys.dm_exec_sessions 或 sys.dm_exec_requests 动态管理视图中的会话 ID 列。  
  
WITH STATUSONLY  
生成由于更早的 KILL 语句而正在回滚的指定会话 ID 或 UOW 的进度报告。 KILL WITH STATUSONLY 不结束或回滚会话 ID 或 UOW。 此命令只显示当前回滚进度。  
  
## <a name="remarks"></a>备注  
KILL 常用于结束使用锁来阻止其他重要进程的进程。 KILL 还可用于停止执行使用必要系统资源的查询的进程。 无法结束系统进程和运行扩展存储过程的进程。  
  
应当小心使用 KILL，特别是正在运行重要进程时。 你无法终止自己的进程。 也不得终止以下进程：  
  
-   AWAITING COMMAND  
-   CHECKPOINT SLEEP  
-   LAZY WRITER  
-   LOCK MONITOR  
-   SIGNAL HANDLER  
  
使用 @@SPID 可显示当前会话的 session ID 值。  
  
若要获取有效会话 ID 值的报告，请查询 sys.dm_tran_locks、sys.dm_exec_sessions 和 sys.dm_exec_requests 动态管理视图中的 session_id 列。 也可以查看 sp_who 系统存储过程返回的 SPID 列。 如果特定 SPID 的回滚正在进行，则该 SPID 的 sp_who 结果集中的 cmd 列指示 KILLED/ROLLBACK。  
  
当特定的连接在数据库资源上有锁并阻塞其他连接的进程时，`blocking_session_id` 的 `sys.dm_exec_requests` 列或 `blk` 返回的 `sp_who` 列中将显示该阻塞连接的会话 ID。  
  
KILL 命令可用于解决有疑问的分布式事务。 这些事务是未解决的分布式事务，它们是由于无计划地重新启动数据库服务器或 MS DTC 协调器而产生的。 有关未决事务的详细信息，请参阅[使用标记的事务一致地恢复相关的数据库的事务（完全恢复模式）](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md)中的“两阶段提交”一节。  
  
## <a name="using-with-statusonly"></a>使用 WITH STATUSONLY  
如果会话 ID 或 UOW 由于前面的 KILL session ID|UOW 语句而回滚，KILL WITH STATUSONLY 生成报告。 进度报告指出已完成的回滚量（以百分比形式）和估计的剩余时间（以秒为单位）。 报告使用以下格式声明它：  
  
`Spid|UOW <xxx>: Transaction rollback in progress. Estimated rollback completion: <yy>% Estimated time left: <zz> seconds`  
  
如果在 KILL session ID|UOW WITH STATUSONLY 语句运行前会话 ID 或 UOW 的回滚就已完成，那么 KILL session ID|UOW WITH STATUSONLY 返回以下错误：  
  
```
"Msg 6120, Level 16, State 1, Line 1"  
"Status report cannot be obtained. Rollback operation for Process ID <session ID> is not in progress."
```  
如果没有要回滚的会话 ID 或 UOW，也会生成此错误


通过重复不使用 WITH STATUSONLY 选项的同一 KILL session ID|UOW 语句，可以获得相同的状态报告。 不过，不建议这样重复使用选项。 如果在新的 KILL 语句运行前回滚就已完成且会话 ID 已重新分配给新任务，那么重复 KILL session ID 语句可能会停止新进程。 通过指定 WITH STATUSONLY 来阻止新进程停止。  
  
## <a name="permissions"></a>权限  
**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]：** 要求具有 ALTER ANY CONNECTION 权限。 ALTER ANY CONNECTION 包括在 sysadmin 或 processadmin 固定服务器角色的成员身份中。  
  
**[!INCLUDE[ssSDS](../../includes/sssds-md.md)]：** 需要具有 KILL DATABASE CONNECTION 权限。 服务器级别主体登录名具有 KILL DATABASE CONNECTION。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-kill-to-stop-a-session"></a>A. 使用 KILL 停止会话  
 下面的示例展示了如何停止会话 ID `53`。  
  
```sql  
KILL 53;  
GO  
```  
  
### <a name="b-using-kill-session-id-with-statusonly-to-obtain-a-progress-report"></a>B. 使用 KILL session ID WITH STATUSONLY 获取进度报告  
以下示例为特定的会话 ID 生成回滚进程的状态。  
  
```sql  
KILL 54;  
KILL 54 WITH STATUSONLY;  
GO  
  
--This is the progress report.  
spid 54: Transaction rollback in progress. Estimated rollback completion: 80% Estimated time left: 10 seconds.  
```  
  
### <a name="c-using-kill-to-stop-an-orphaned-distributed-transaction"></a>C. 使用 KILL 停止孤立的分布式事务  
下面的示例展示了如何停止孤立的分布式事务（会话 ID = -2），其中 UOW 为 `D5499C66-E398-45CA-BF7E-DC9C194B48CF`。  
  
```sql  
KILL 'D5499C66-E398-45CA-BF7E-DC9C194B48CF';  
```  

  
## <a name="see-also"></a>另请参阅  
[KILL STATS JOB (Transact-SQL)](../../t-sql/language-elements/kill-stats-job-transact-sql.md)   
[KILL QUERY NOTIFICATION SUBSCRIPTION (Transact-SQL)](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)   
[内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
[SHUTDOWN (Transact-SQL)](../../t-sql/language-elements/shutdown-transact-sql.md)   
[@@SPID (Transact-SQL)](../../t-sql/functions/spid-transact-sql.md)   
[sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
[sys.dm_exec_sessions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
[sys.dm_tran_locks (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)   
[sp_lock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
[sp_who (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
  
  
