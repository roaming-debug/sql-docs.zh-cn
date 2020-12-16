---
title: 确定持有锁的查询
description: 本文介绍如何查找有锁的查询。 数据库管理员可能需要找到影响数据库性能的锁定的来源。
ms.date: 10/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- queries [SQL Server], extended events
- queries [SQL Server], holding locks
- xe
- extended events [SQL Server], locks
- extended events [SQL Server], holding locks
ms.assetid: bdfce092-3cf1-4b5e-99d5-fd8c6f9ad560
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e1e24e408df936fc2a651263218896e4c96826e7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465608"
---
# <a name="determine-which-queries-are-holding-locks"></a>确定持有锁的查询

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

数据库管理员通常需要识别影响数据库性能的锁定来源。  
  
例如，您怀疑服务器的性能问题可能是由阻塞导致的。 查询 sys.dm_exec_requests 时，您发现处于挂起模式的若干会话具有等待类型，指示锁定就是等待的资源。  
  
查询 sys.dm_tran_locks 后，结果显示许多锁定处于未完成状态，但是已授予锁定的会话没有任何以 sys.dm_exec_requests 显示的活动请求。  
  
此示例说明如何确定占用锁定的查询、查询的计划以及占用锁定时的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 堆栈。 本示例还说明了在扩展事件会话中如何使用配对目标。  
  
若要完成此任务，需使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的查询编辑器执行以下过程。  
  
> [!NOTE]  
>  本示例使用 AdventureWorks 数据库。  
  
### <a name="to-determine-which-queries-are-holding-locks"></a>确定保持锁定的查询  
  
1.  在查询编辑器中发出以下语句：  
  
    ```sql
    -- Perform cleanup.   
    IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='FindBlockers')  
        DROP EVENT SESSION FindBlockers ON SERVER  
    GO  
    -- Use dynamic SQL to create the event session and allow creating a -- predicate on the AdventureWorks database id.  
    --  
    DECLARE @dbid int  
  
    SELECT @dbid = db_id('AdventureWorks')  
  
    IF @dbid IS NULL  
    BEGIN  
        RAISERROR('AdventureWorks is not installed. Install AdventureWorks before proceeding', 17, 1)  
        RETURN  
    END  
  
    DECLARE @sql nvarchar(1024)  
    SET @sql = '  
    CREATE EVENT SESSION FindBlockers ON SERVER  
    ADD EVENT sqlserver.lock_acquired   
        (action   
            ( sqlserver.sql_text, sqlserver.database_id, sqlserver.tsql_stack,  
             sqlserver.plan_handle, sqlserver.session_id)  
        WHERE ( database_id=' + cast(@dbid as nvarchar) + ' AND resource_0!=0)   
        ),  
    ADD EVENT sqlserver.lock_released   
        (WHERE ( database_id=' + cast(@dbid as nvarchar) + ' AND resource_0!=0 ))  
    ADD TARGET package0.pair_matching   
        ( SET begin_event=''sqlserver.lock_acquired'',   
                begin_matching_columns=''database_id, resource_0, resource_1, resource_2, transaction_id, mode'',   
                end_event=''sqlserver.lock_released'',   
                end_matching_columns=''database_id, resource_0, resource_1, resource_2, transaction_id, mode'',  
        respond_to_memory_pressure=1)  
    WITH (max_dispatch_latency = 1 seconds)'  
  
    EXEC (@sql)  
    --   
    -- Create the metadata for the event session  
    -- Start the event session  
    --  
    ALTER EVENT SESSION FindBlockers ON SERVER  
    STATE = START  
    ```  
  
2.  在服务器上执行工作负荷后，在查询编辑器中发出以下语句来查找仍保持锁定的查询。  
  
    ```sql
    --  
    -- The pair matching targets report current unpaired events using   
    -- the sys.dm_xe_session_targets dynamic management view (DMV)  
    -- in XML format.  
    -- The following query retrieves the data from the DMV and stores  
    -- key data in a temporary table to speed subsequent access and  
    -- retrieval.  
    --  
    SELECT   
    objlocks.value('(action[@name="session_id"]/value)[1]', 'int')  
            AS session_id,  
        objlocks.value('(data[@name="database_id"]/value)[1]', 'int')   
            AS database_id,  
        objlocks.value('(data[@name="resource_type"]/text)[1]', 'nvarchar(50)' )   
            AS resource_type,  
        objlocks.value('(data[@name="resource_0"]/value)[1]', 'bigint')   
            AS resource_0,  
        objlocks.value('(data[@name="resource_1"]/value)[1]', 'bigint')   
            AS resource_1,  
        objlocks.value('(data[@name="resource_2"]/value)[1]', 'bigint')   
            AS resource_2,  
        objlocks.value('(data[@name="mode"]/text)[1]', 'nvarchar(50)')   
            AS mode,  
        objlocks.value('(action[@name="sql_text"]/value)[1]', 'varchar(MAX)')   
            AS sql_text,  
        CAST(objlocks.value('(action[@name="plan_handle"]/value)[1]', 'varchar(MAX)') AS xml)   
            AS plan_handle,      
        CAST(objlocks.value('(action[@name="tsql_stack"]/value)[1]', 'varchar(MAX)') AS xml)   
            AS tsql_stack  
    INTO #unmatched_locks  
    FROM (  
        SELECT CAST(xest.target_data as xml)   
            lockinfo  
        FROM sys.dm_xe_session_targets xest  
        JOIN sys.dm_xe_sessions xes ON xes.address = xest.event_session_address  
        WHERE xest.target_name = 'pair_matching' AND xes.name = 'FindBlockers'  
    ) heldlocks  
    CROSS APPLY lockinfo.nodes('//event[@name="lock_acquired"]') AS T(objlocks)  
  
    --  
    -- Join the data acquired from the pairing target with other   
    -- DMVs to return provide additional information about blockers  
    --  
    SELECT ul.*  
        FROM #unmatched_locks ul  
        INNER JOIN sys.dm_tran_locks tl ON ul.database_id = tl.resource_database_id AND ul.resource_type = tl.resource_type  
        WHERE resource_0 IS NOT NULL  
        AND session_id IN   
            (SELECT blocking_session_id FROM sys.dm_exec_requests WHERE blocking_session_id != 0)  
        AND tl.request_status='wait'  
        AND REPLACE(ul.mode, 'LCK_M_', '' ) = tl.request_mode  
  
    ```  
  
3.  识别问题后，删除所有临时表和事件会话。  
  
    ```sql
    DROP TABLE #unmatched_locks  
    DROP EVENT SESSION FindBlockers ON SERVER  
    ```  

> [!NOTE]
> 前面的 Transact-SQL 代码示例在 SQL Server 上本地运行，但可能 _不太在 Azure SQL 数据库上运行_。 该示例的核心部分直接涉及事件，例如 `ADD EVENT sqlserver.lock_acquired` 也可在 Azure SQL 数据库上运行。 但要运行示例，必须先将一些初步项（如 `sys.server_event_sessions`）编辑为其 Azure SQL 数据库对应项，如 `sys.database_event_sessions`。
> 若要详细了解本地 SQL Server 与 Azure SQL 数据库之间的细微差异，请参阅以下文章：
> - [Azure SQL 数据库中的扩展事件](/azure/sql-database/sql-database-xevent-db-diff-from-svr#transact-sql-differences)
> - [支持扩展事件的系统对象](xevents-references-system-objects.md)

## <a name="see-also"></a>另请参阅  
 [CREATE EVENT SESSION (Transact-SQL)](../../t-sql/statements/create-event-session-transact-sql.md)   
 [ALTER EVENT SESSION (Transact-SQL)](../../t-sql/statements/alter-event-session-transact-sql.md)   
 [DROP EVENT SESSION (Transact-SQL)](../../t-sql/statements/drop-event-session-transact-sql.md)   
 [sys.dm_xe_session_targets (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-targets-transact-sql.md)   
 [sys.dm_xe_sessions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-sessions-transact-sql.md)  
  
  
