---
description: sys.dm_exec_sessions (Transact-SQL)
title: sys.dm_exec_sessions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_sessions_TSQL
- sys.dm_exec_sessions
- dm_exec_sessions
- sys.dm_exec_sessions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_sessions dynamic management view
ms.assetid: 2b7e8e0c-eea0-431e-819f-8ccd12ec8cfa
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7b50b83a71df6485afae83fb371abb04209898ae
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482788"
---
# <a name="sysdm_exec_sessions-transact-sql"></a>sys.dm_exec_sessions (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中每个经过身份验证的会话都返回一行。 sys.dm_exec_sessions 是服务器范围的视图，显示了有关所有活动用户连接和内部任务的信息。 此信息包含客户端版本、客户端程序名称、客户端登录时间、登录用户、当前会话设置等。 使用 sys.dm_exec_sessions，首先可以查看当前的系统负荷并标识相关会话，然后可以通过其他动态管理视图或动态管理函数了解有关该会话的详细信息。  
  
 Sys.dm_exec_connections、sys.dm_exec_sessions 和 sys.dm_exec_requests 动态管理视图映射到 [sys.sys进程](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md) 系统表。  
  
> **注意：** 若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_exec_sessions**。  
  
|列名称|数据类型|说明和特定于版本的信息|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|标识与每个活动主连接关联的会话。 不可为 null。|  
|login_time|**datetime**|建立会话的时间。 不可为 null。 如果会话在此 DMV 查询时未完成登录，则显示登录时间为 `1900-01-01` 。|  
|host_name|**nvarchar(128)**|特定于会话的客户端工作站名称。 对于内部会话，该值为 NULL。 可以为 Null。<br /><br /> **安全说明：** 客户端应用程序提供了工作站名称并可以提供不准确的数据。 不要将 HOST_NAME 作为安全功能使用。|  
|program_name|**nvarchar(128)**|启动会话的客户端程序的名称。 对于内部会话，该值为 NULL。 可以为 Null。|  
|host_process_id|**int**|启动会话的客户端程序的进程 ID。 对于内部会话，该值为 NULL。 可以为 Null。|  
|client_version|**int**|客户端连接到服务器所用接口的 TDS 协议版本。 对于内部会话，该值为 NULL。 可以为 Null。|  
|client_interface_name|**nvarchar(32)**|客户端用于与服务器通信的库/驱动程序的名称。 对于内部会话，该值为 NULL。 可以为 Null。|  
|security_id|**varbinary(85)**|与登录名关联的 Microsoft Windows 安全 ID。 不可为 null。|  
|login_name|**nvarchar(128)**|当前执行的会话所使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 有关创建此会话的原始登录名，请参阅 original_login_name。 可以是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 经过身份验证的登录名或 Windows 身份验证的域用户名。 不可为 null。|  
|nt_domain|**nvarchar(128)**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 客户端的 Windows 域（如果使用 Windows 身份验证或可信连接进行会话）。 对于内部会话和非域用户，该值为 NULL。 可以为 Null。|  
|nt_user_name|**nvarchar(128)**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 客户端的 Windows 用户名（如果使用 Windows 身份验证或可信连接进行会话）。 对于内部会话和非域用户，该值为 NULL。 可以为 Null。|  
|status|**nvarchar(30)**|会话的状态。 可能的值：<br /><br /> **Running** - 当前正在运行一个或多个请求<br /><br /> **Sleeping** - 当前没有运行任何请求<br /><br /> **休眠** -由于连接池而重置会话，并且该会话目前处于预登录状态。<br /><br /> **Preconnect** - 会话在资源调控器分类器中。<br /><br /> 不可为 null。|  
|context_info|**varbinary(128)**|会话的 CONTEXT_INFO 值。 上下文信息由用户通过使用 [set CONTEXT_INFO](../../t-sql/statements/set-context-info-transact-sql.md) 语句设置。 可以为 Null。|  
|cpu_time|**int**|该会话所占用的 CPU 时间（毫秒）。 不可为 null。|  
|memory_usage|**int**|该会话所占用的 8 KB 内存页数。 不可为 null。|  
|total_scheduled_time|**int**|计划内含请求的会话的执行所耗用的总计时间（毫秒）。 不可为 null。|  
|total_elapsed_time|**int**|自会话建立以来已耗用的时间（毫秒）。 不可为 null。|  
|endpoint_id|**int**|与会话关联的端点的 ID。 不可为 null。|  
|last_request_start_time|**datetime**|最近一次会话请求的开始时间。 这包括当前正在执行的请求。 不可为 null。|  
|last_request_end_time|**datetime**|最近一次会话请求的完成时间。 可以为 Null。|  
|reads|**bigint**|在该会话期间该会话中的请求所执行的读取次数。 不可为 null。|  
|Writes|**bigint**|在该会话期间该会话中的请求所执行的写入次数。 不可为 null。|  
|logical_reads|**bigint**|已对该会话执行的逻辑读取数。 不可为 null。|  
|is_user_process|**bit**|如果会话是系统会话，则为 0。 否则为 1。 不可为 null。|  
|text_size|**int**|会话的 TEXTSIZE 设置。 不可为 null。|  
|语言|**nvarchar(128)**|会话的 LANGUAGE 设置。 可以为 Null。|  
|date_format|**nvarchar (3)**|会话的 DATEFORMAT 设置。 可以为 Null。|  
|date_first|**smallint**|会话的 DATEFIRST 设置。 不可为 null。|  
|quoted_identifier|**bit**|会话的 QUOTED_IDENTIFIER 设置。 不可为 null。|  
|arithabort|**bit**|会话的 ARITHABORT 设置。 不可为 null。|  
|ansi_null_dflt_on|**bit**|会话的 ANSI_NULL_DFLT_ON 设置。 不可为 null。|  
|ansi_defaults|**bit**|会话的 ANSI_DEFAULTS 设置。 不可为 null。|  
|ansi_warnings|**bit**|会话的 ANSI_WARNINGS 设置。 不可为 null。|  
|ansi_padding|**bit**|会话的 ANSI_PADDING 设置。 不可为 null。|  
|ansi_nulls|**bit**|会话的 ANSI_NULLS 设置。 不可为 null。|  
|concat_null_yields_null|**bit**|会话的 CONCAT_NULL_YIELDS_NULL 设置。 不可为 null。|  
|transaction_isolation_level|**smallint**|会话的事务隔离级别。<br /><br /> 0 = 未指定<br /><br /> 1 = ReadUncommitted<br /><br /> 2 = 已提交读取<br /><br /> 3 = RepeatableRead<br /><br /> 4 = 可序列化<br /><br /> 5 = 快照<br /><br /> 不可为 null。|  
|lock_timeout|**int**|会话的 LOCK_TIMEOUT 设置。 该值以毫秒计。 不可为 null。|  
|deadlock_priority|**int**|会话的 DEADLOCK_PRIORITY 设置。 不可为 null。|  
|row_count|**bigint**|到目前为止会话返回的行数。 不可为 null。|  
|prev_error|**int**|会话返回的最近一个错误的 ID。 不可为 null。|  
|original_security_id|**varbinary(85)**|与 original_login_name 关联的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 安全 ID。 不可为 null。|  
|original_login_name|**nvarchar(128)**|客户端用于创建此会话的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 可以是经过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的登录名、经过 Windows 身份验证的域用户名，也可以是包含数据库用户。 请注意，此会话在初次连接后可能已进行多次隐式或显式上下文切换。 例如，如果使用 [EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md) 。 不可为 null。|  
|last_successful_logon|**datetime**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 当前会话开始前 original_login_name 上一次成功登录的时间。|  
|last_unsuccessful_logon|**datetime**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 当前会话开始前，original_login_name 上一次登录失败的时间。|  
|unsuccessful_logons|**bigint**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 在 last_successful_logon 和 login_time 之间 original_login_name 的登录失败次数。|  
|group_id|**int**|此会话所属工作负荷组的 ID。 不可为 null。|  
|database_id|**smallint**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 每个会话的当前数据库的 ID。|  
|authenticating_database_id|**int**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 对主体进行身份验证的数据库的 ID。 对于登录名，该值将为 0。 对于包含数据库用户，该值将为包含数据库的数据库 ID。|  
|open_transaction_count|**int**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 每个会话的打开事务数。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
|page_server_reads|**bigint**|**适用** 于： Azure SQL 数据库超大规模<br /><br /> 在此会话期间，此会话中的请求所执行的页服务器读取次数。 不可为 null。|  
  
## <a name="permissions"></a>权限  
所有人都可以查看自己的会话信息。  
**[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ：** 需要 `VIEW SERVER STATE` 权限 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 才能查看服务器上的所有会话。  
**[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] ：** 需要 `VIEW DATABASE STATE` 查看与当前数据库的所有连接。 `VIEW DATABASE STATE` 不能在数据库中授予 `master` 。 
  
  
## <a name="remarks"></a>备注  
 启用 " **符合标准符合性** " 的服务器配置选项时，将在以下各列中显示登录统计信息。  
  
-   last_successful_logon  
  
-   last_unsuccessful_logon  
  
-   unsuccessful_logons  
  
 如果未启用此选项，则这些列将返回 Null 值。 有关如何设置此服务器配置选项的详细信息，请参阅 " [已启用通用标准符合性" 服务器配置选项](../../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md)。  
 
 
 Azure SQL 数据库上的管理员连接将在每个经过身份验证的会话中显示一行。 在结果集中出现的 "sa" 会话不会对会话的用户配额产生任何影响。 非管理员连接将只能看到与它们的数据库用户会话相关的信息。
 
  
## <a name="relationship-cardinalities"></a>关系基数  
  
|From|功能|对于/应用|关系|  
|----------|--------|---------------|------------------|  
|sys.dm_exec_sessions|[sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|session_id|一对零或一对多|  
|sys.dm_exec_sessions|[sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)|session_id|一对零或一对多|  
|sys.dm_exec_sessions|[sys.dm_tran_session_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)|session_id|一对零或一对多|  
|sys.dm_exec_sessions|[sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md) (session_id &#124; 0) |session_id CROSS APPLY<br /><br /> OUTER APPLY|一对零或一对多|  
|sys.dm_exec_sessions|[sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)|session_id|一对一|  
  
## <a name="examples"></a>示例  
  
### <a name="a-finding-users-that-are-connected-to-the-server"></a>A. 查找连接到服务器的用户  
 下例将查找连接到服务器的用户并返回每个用户的会话数。  
  
```sql  
SELECT login_name ,COUNT(session_id) AS session_count   
FROM sys.dm_exec_sessions   
GROUP BY login_name;  
```  
  
### <a name="b-finding-long-running-cursors"></a>B. 查找长时间运行的游标  
 下例将查找打开时间超过指定时间段的游标、创建游标的用户以及游标所在的会话。  
  
```sql  
USE master;  
GO  
SELECT creation_time ,cursor_id   
    ,name ,c.session_id ,login_name   
FROM sys.dm_exec_cursors(0) AS c   
JOIN sys.dm_exec_sessions AS s   
   ON c.session_id = s.session_id   
WHERE DATEDIFF(mi, c.creation_time, GETDATE()) > 5;  
```  
  
### <a name="c-finding-idle-sessions-that-have-open-transactions"></a>C. 查找具有已打开事务的空闲会话  
 下例将查找具有已打开事务的空闲会话。 空闲会话是当前未运行请求的会话。  
  
```sql  
SELECT s.*   
FROM sys.dm_exec_sessions AS s  
WHERE EXISTS   
    (  
    SELECT *   
    FROM sys.dm_tran_session_transactions AS t  
    WHERE t.session_id = s.session_id  
    )  
    AND NOT EXISTS   
    (  
    SELECT *   
    FROM sys.dm_exec_requests AS r  
    WHERE r.session_id = s.session_id  
    );  
```  
  
### <a name="d-finding-information-about-a-queries-own-connection"></a>D. 查找查询自有连接的有关信息  
 收集查询自有连接有关信息的典型查询。  
  
```sql  
SELECT   
    c.session_id, c.net_transport, c.encrypt_option,   
    c.auth_scheme, s.host_name, s.program_name,   
    s.client_interface_name, s.login_name, s.nt_domain,   
    s.nt_user_name, s.original_login_name, c.connect_time,   
    s.login_time   
FROM sys.dm_exec_connections AS c  
JOIN sys.dm_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = @@SPID;  
```  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与执行相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  



