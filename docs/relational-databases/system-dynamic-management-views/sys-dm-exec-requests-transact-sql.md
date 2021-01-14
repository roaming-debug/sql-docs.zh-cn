---
description: sys.dm_exec_requests (Transact-SQL)
title: sys.dm_exec_requests (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_requests_TSQL
- sys.dm_exec_requests
- dm_exec_requests_TSQL
- dm_exec_requests
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_requests dynamic management view
ms.assetid: 4161dc57-f3e7-4492-8972-8cfb77b29643
author: pmasl
ms.author: pelopes
ms.reviewer: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a1ad13d4cdbcf8820a318a87f4ef9f52c488a406
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171259"
---
# <a name="sysdm_exec_requests-transact-sql"></a>sys.dm_exec_requests (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回有关在中执行的每个请求的信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关请求的详细信息，请参阅 [线程和任务体系结构指南](../../relational-databases/thread-and-task-architecture-guide.md)。
   
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|与此请求相关的会话的 ID。 不可为 null。|  
|request_id|**int**|请求的 ID。 在会话的上下文中是唯一的。 不可为 null。|  
|start_time|**datetime**|请求到达时的时间戳。 不可为 null。|  
|status|**nvarchar(30)**|请求的状态。 可以是以下位置之一：<br /><br /> 背景<br />运行<br />可运行<br />Sleeping<br />已挂起<br /><br /> 不可为 null。|  
|命令|**nvarchar(32)**|标识正在处理的命令的当前类型。 常用命令类型包括：<br /><br /> SELECT<br />INSERT<br />UPDATE<br />DELETE<br />BACKUP LOG<br />BACKUP DATABASE<br />DBCC<br />FOR<br /><br /> 可通过结合使用 sys.dm_exec_sql_text 和与请求对应的 sql_handle 检索请求的文本。 内部系统进程将基于它们所执行任务的类型来设置该命令。 这些任务可以包括：<br /><br /> LOCK MONITOR<br />CHECKPOINTLAZY<br />WRITER<br /><br /> 不可为 null。|  
|sql_handle|**varbinary(64)**|是唯一标识查询所属的批处理或存储过程的标记。 可以为 Null。| 
|statement_start_offset|**int**|指示当前正在执行的批处理或持久化对象当前正在执行的语句的开始位置（以字节为单位），从0开始。 可以与 `sql_handle` 、 `statement_end_offset` 和 `sys.dm_exec_sql_text` 动态管理函数一起使用，以检索请求的当前正在执行的语句。 可以为 Null。|  
|statement_end_offset|**int**|指示当前正在执行的批处理或持久化对象当前正在执行的语句的结束位置（以字节为单位，从0开始）。 可以与 `sql_handle` 、 `statement_start_offset` 和 `sys.dm_exec_sql_text` 动态管理函数一起使用，以检索请求的当前正在执行的语句。 可以为 Null。|  
|plan_handle|**varbinary(64)**|是一个标记，用于为当前正在执行的批处理唯一标识查询执行计划。 可以为 Null。|  
|database_id|**smallint**|对其执行请求的数据库的 ID。 不可为 null。|  
|user_id|**int**|提交请求的用户的 ID。 不可为 null。|  
|connection_id|**uniqueidentifier**|请求到达时所采用的连接的 ID。 可以为 Null。|  
|blocking_session_id|**smallint**|正在阻塞请求的会话的 ID。 如果此列的值为 NULL 或等于0，则不会阻止该请求，或者阻塞会话的会话信息不可用 (或无法) 识别。<br /><br /> -2 = 阻塞资源由孤立的分布式事务拥有。<br /><br /> -3 = 阻塞资源由延迟的恢复事务拥有。<br /><br /> -4 = 由于内部闩锁状态转换而导致此时无法确定阻塞闩锁所有者的会话 ID。|  
|wait_type|**nvarchar(60)**|如果请求当前被阻塞，则此列返回等待类型。 可以为 Null。<br /><br /> 有关等待类型的信息，请参阅 [&#40;transact-sql&#41;sys.dm_os_wait_stats ](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)。|  
|wait_time|**int**|如果请求当前被阻塞，则此列返回当前等待的持续时间（以毫秒为单位）。 不可为 null。|  
|last_wait_type|**nvarchar(60)**|如果此请求先前已经阻塞，则此列返回上次等待的类型。 不可为 null。|  
|wait_resource|**nvarchar(256)**|如果请求当前被阻塞，则此列返回请求当前等待的资源。 不可为 null。|  
|open_transaction_count|**int**|为此请求打开的事务数。 不可为 null。|  
|open_resultset_count|**int**|为此请求打开的结果集的个数。 不可为 null。|  
|transaction_id|**bigint**|在其中执行此请求的事务的 ID。 不可为 null。|  
|context_info|**varbinary(128)**|会话 CONTEXT_INFO 值。 可以为 Null。|  
|percent_complete|**real**|为以下命令完成的工作的百分比：<br /><br /> ALTER INDEX REORGANIZE<br />AUTO_SHRINK 选项（带 ALTER DATABASE）<br />BACKUP DATABASE<br />DBCC CHECKDB<br />DBCC CHECKFILEGROUP<br />DBCC CHECKTABLE<br />DBCC INDEXDEFRAG<br />DBCC SHRINKDATABASE<br />DBCC SHRINKFILE<br />RECOVERY<br />RESTORE DATABASE<br />ROLLBACK<br />TDE ENCRYPTION<br /><br /> 不可为 null。|  
|estimated_completion_time|**bigint**|仅限内部。 不可为 null。|  
|cpu_time|**int**|请求所使用的 CPU 时间（毫秒）。 不可为 null。|  
|total_elapsed_time|**int**|请求到达后经过的总时间（毫秒）。 不可为 null。|  
|scheduler_id|**int**|正在计划此请求的计划程序的 ID。 不可为 null。|  
|task_address|**varbinary(8)**|分配给与此请求关联的任务的内存地址。 可以为 Null。|  
|reads|**bigint**|此请求执行的读取数。 不可为 null。|  
|Writes|**bigint**|此请求执行的写入数。 不可为 null。|  
|logical_reads|**bigint**|此请求已经执行的逻辑读取数。 不可为 null。|  
|text_size|**int**|此请求的 TEXTSIZE 设置。 不可为 null。|  
|语言|**nvarchar(128)**|该请求的语言设置。 可以为 Null。|  
|date_format|**nvarchar (3)**|该请求的 DATEFORMAT 设置。 可以为 Null。|  
|date_first|**smallint**|该请求的 DATEFIRST 设置。 不可为 null。|  
|quoted_identifier|**bit**|1 = QUOTED_IDENTIFIER 对于该请求是 ON。 否则，为0。<br /><br /> 不可为 null。|  
|arithabort|**bit**|1 = ARITHABORT 设置对于该请求是 ON。 否则，为0。<br /><br /> 不可为 null。|  
|ansi_null_dflt_on|**bit**|1 = ANSI_NULL_DFLT_ON 设置对于该请求是 ON。 否则，为0。<br /><br /> 不可为 null。|  
|ansi_defaults|**bit**|1 = ANSI_DEFAULTS 设置对于该请求是 ON。 否则，为0。<br /><br /> 不可为 null。|  
|ansi_warnings|**bit**|1 = ANSI_WARNINGS 设置对于该请求是 ON。 否则，为0。<br /><br /> 不可为 null。|  
|ansi_padding|**bit**|1 = ANSI_PADDING 设置对于该请求是 ON。<br /><br /> 否则，为0。<br /><br /> 不可为 null。|  
|ansi_nulls|**bit**|1 = ANSI_NULLS 设置对于该请求是 ON。 否则，为0。<br /><br /> 不可为 null。|  
|concat_null_yields_null|**bit**|1 = CONCAT_NULL_YIELDS_NULL 设置对于该请求是 ON。 否则，为0。<br /><br /> 不可为 null。|  
|transaction_isolation_level|**smallint**|创建此请求的事务时使用的隔离级别。 不可为 null。<br /> 0 = 未指定<br /> 1 = 未提交读取<br /> 2 = 已提交读取<br /> 3 = 可重复<br /> 4 = 可序列化<br /> 5 = 快照|  
|lock_timeout|**int**|此请求的锁定超时时间（毫秒）。 不可为 null。|  
|deadlock_priority|**int**|请求的 DEADLOCK_PRIORITY 设置。 不可为 null。|  
|row_count|**bigint**|已由此请求返回到客户端的行数。 不可为 null。|  
|prev_error|**int**|在执行请求期间发生的最后一个错误。 不可为 null。|  
|nest_level|**int**|正在对请求执行的代码的嵌套级别。 不可为 null。|  
|granted_query_memory|**int**|为执行该请求的查询而分配的页数。 不可为 null。|  
|executing_managed_code|**bit**|指示特定请求当前是否正在执行公共语言运行时对象，例如例程、类型和触发器。 只要某个公共语言运行时对象在堆栈中，就会设置此值，甚至从公共语言运行时中运行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 时，也会设置。 不可为 null。|  
|group_id|**int**|此查询所属工作负荷组的 ID。 不可为 null。|  
|query_hash|**二进制 (8)**|对查询计算的二进制哈希值，用于标识具有类似逻辑的查询。 可以使用查询哈希确定仅仅是文字值不同的查询的聚合资源使用情况。|  
|query_plan_hash|**二进制 (8)**|对查询执行计划计算的二进制哈希值，用于标识类似的查询执行计划。 可以使用查询计划哈希查找具有类似执行计划的查询的累积成本。|  
|statement_sql_handle|**varbinary(64)**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 单个查询的 SQL 句柄。<br /><br />如果没有为数据库启用查询存储，则此列为 NULL。 |  
|statement_context_id|**bigint**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 要 sys.query_context_settings 的可选外键。<br /><br />如果没有为数据库启用查询存储，则此列为 NULL。 |  
|dop |**int** |**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本。<br /><br /> 查询的并行度。 |  
|parallel_worker_count |**int** |**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本。<br /><br /> 如果这是并行查询，则为保留的并行工作线程数。  |  
|external_script_request_id |**uniqueidentifier** |**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本。<br /><br /> 与当前请求关联的外部脚本请求 ID。 |  
|is_resumable |**bit** |**适用于**：[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 及更高版本。<br /><br /> 指示请求是否为可恢复索引操作。 |  
|page_resource |**二进制 (8)** |适用于：[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]<br /><br /> 如果列包含页，则为页资源的8字节的十六进制表示形式 `wait_resource` 。 有关详细信息，请参阅 [sys.fn_PageResCracker](../../relational-databases/system-functions/sys-fn-pagerescracker-transact-sql.md)。 |  
|page_server_reads|**bigint**|**适用** 于： Azure SQL 数据库超大规模<br /><br /> 此请求执行的页服务器读取次数。 不可为 null。|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="remarks"></a>备注 
若要执行在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以外的代码（例如，扩展存储过程和分布式查询），则必须在非抢先计划程序的控制范围以外执行该线程。 若要这样做，工作线程将切换到抢先模式。 由此动态管理视图返回的时间值不包括在抢先模式下花费的时间。

在 [行模式下](../../relational-databases/query-processing-architecture-guide.md#row-mode-execution)执行并行请求时，会 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分配一个工作线程来协调负责完成分配给它们的任务的工作线程。 在此 DMV 中，只有协调器线程对该请求可见。 **不** 会为协调器线程更新列 **读取**、**写入**、 **logical_reads** 和 **row_count** 。 **仅** 为协调器线程更新列 **wait_type**、 **wait_time**、 **last_wait_type**、 **wait_resource** 和 **granted_query_memory** 。 有关详细信息，请参阅[线程和任务体系结构指南](../../relational-databases/thread-and-task-architecture-guide.md)。

## <a name="permissions"></a>权限
如果用户对 `VIEW SERVER STATE` 服务器具有权限，则用户将看到该实例上的所有正在执行的会话 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; 否则，用户将只看到当前会话。 `VIEW SERVER STATE` 无法在 Azure SQL 数据库中授予，因此 `sys.dm_exec_requests` 始终限制为当前连接。

在 Always-On 情况下，如果将辅助副本设置为 **仅读意向**，则与辅助副本的连接必须通过添加在连接字符串参数中指定其应用程序意向 `applicationintent=readonly` 。 否则，访问检查 `sys.dm_exec_requests` 不会传递到可用性组中的数据库，即使 `VIEW SERVER STATE` 权限存在也是如此。

  
## <a name="examples"></a>示例  
  
### <a name="a-finding-the-query-text-for-a-running-batch"></a>A. 查找用于运行批处理的查询文本

 以下示例查询 `sys.dm_exec_requests` 以查找感兴趣的查询并从输出复制其 `sql_handle`。  

```sql
SELECT * FROM sys.dm_exec_requests;  
GO  
```  

然后，为了获取语句文本，将复制的 `sql_handle` 与系统函数 `sys.dm_exec_sql_text(sql_handle)` 一起使用。  

```sql
SELECT * FROM sys.dm_exec_sql_text(< copied sql_handle >);  
GO  
```

### <a name="b-finding-all-locks-that-a-running-batch-is-holding"></a>B. 查找运行的批处理持有的所有锁

下面的示例查询 **sys.dm_exec_requests** 以查找感兴趣的批，并 `transaction_id` 从输出复制其。

```sql
SELECT * FROM sys.dm_exec_requests;  
GO
```

然后，若要查找锁定信息，请使用 `transaction_id` 与系统函数一起复制的 **sys.dm_tran_locks**。  

```sql
SELECT * FROM sys.dm_tran_locks
WHERE request_owner_type = N'TRANSACTION'
    AND request_owner_id = < copied transaction_id >;
GO  
```

### <a name="c-finding-all-currently-blocked-requests"></a>C. 查找当前阻塞的所有请求

下面的示例查询 **sys.dm_exec_requests** 以查找有关被阻止的请求的信息。  

```sql
SELECT session_id ,status ,blocking_session_id  
    ,wait_type ,wait_time ,wait_resource
    ,transaction_id
FROM sys.dm_exec_requests
WHERE status = N'suspended';  
GO  
```  

### <a name="d-ordering-existing-requests-by-cpu"></a>D. 按 CPU 对现有请求进行排序

```sql
SELECT 
   req.session_id
   , req.start_time
   , cpu_time 'cpu_time_ms'
   , object_name(st.objectid,st.dbid) 'ObjectName' 
   , substring
      (REPLACE
        (REPLACE
          (SUBSTRING
            (ST.text
            , (req.statement_start_offset/2) + 1
            , (
               (CASE statement_end_offset
                  WHEN -1
                  THEN DATALENGTH(ST.text)  
                  ELSE req.statement_end_offset
                  END
                    - req.statement_start_offset)/2) + 1)
       , CHAR(10), ' '), CHAR(13), ' '), 1, 512)  AS statement_text  
FROM sys.dm_exec_requests AS req  
   CROSS APPLY sys.dm_exec_sql_text(req.sql_handle) as ST
   ORDER BY cpu_time desc;
GO
```

## <a name="see-also"></a>另请参阅
[动态管理视图和函数](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)     
[与执行相关的动态管理视图和函数](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)      
[sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)     
[sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)     
[sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)    
[sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)      
[SQL Server，SQL Statistics 对象](../../relational-databases/performance-monitor/sql-server-sql-statistics-object.md)     
[查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md#DOP)       
[线程和任务体系结构指南](../../relational-databases/thread-and-task-architecture-guide.md)    
