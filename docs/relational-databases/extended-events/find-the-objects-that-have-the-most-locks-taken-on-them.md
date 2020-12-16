---
title: 使用扩展事件查找锁定次数最多的对象
description: 本文演示如何查找锁定次数最多的对象。 数据库管理员可能需要找到大多数锁定的对象以提高数据库性能。
ms.date: 10/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- objects [SQL Server], extended events
- xe
- extended events [SQL Server], locks
- objects [SQL Server], locks
ms.assetid: fcbadbda-c91c-43f0-a1b5-601e40110e07
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 522017d6ced6039cd7e5b9a30cf60404eee9249a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465498"
---
# <a name="find-the-objects-that-have-the-most-locks-taken-on-them"></a>查找具有最多锁定的对象

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

数据库管理员通常需要识别影响数据库性能的锁定来源。  
  
例如，可以监视生产服务器是否存在任何可能的瓶颈。 您怀疑可能存在高度争用的资源，并希望了解这些对象占用多少锁定。 一旦识别锁定频率最高的对象，便可采取一些措施来优化对争用对象的访问。  
  
为此，请使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的查询编辑器。  
  
### <a name="to-find-the-objects-that-have-the-most-locks"></a>查找占用最多锁定的对象  
  
1. 在查询编辑器中发出以下语句：

    ```sql
    -- Find objects in a particular database that have the most
    -- lock acquired. This sample uses AdventureWorksDW2012.
    -- Create the session and add an event and target.
    
    IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='LockCounts')
        DROP EVENT session LockCounts ON SERVER;
    GO
    DECLARE @dbid int;
  
    SELECT @dbid = db_id('AdventureWorksDW2012');
  
    DECLARE @sql nvarchar(1024);
    SET @sql = '
        CREATE event session LockCounts ON SERVER
            ADD EVENT sqlserver.lock_acquired (WHERE database_id ='
                + CAST(@dbid AS nvarchar) +')
            ADD TARGET package0.histogram(
                SET filtering_event_name=''sqlserver.lock_acquired'',
                    source_type=0, source=''resource_0'')';
  
    EXEC (@sql);
    GO
    ALTER EVENT session LockCounts ON SERVER
        STATE=start;
    GO
    -- Create a simple workload that takes locks.
    
    USE AdventureWorksDW2012;
    GO
    SELECT TOP 1 * FROM dbo.vAssocSeqLineItems;
    GO
    -- The histogram target output is available from the
    -- sys.dm_xe_session_targets dynamic management view in
    -- XML format.
    -- The following query joins the bucketizing target output with
    -- sys.objects to obtain the object names.
    
    SELECT name, object_id, lock_count
        FROM
        (
        SELECT objstats.value('.','bigint') AS lobject_id,
            objstats.value('@count', 'bigint') AS lock_count
            FROM (
                SELECT CAST(xest.target_data AS XML)
                    LockData
                FROM     sys.dm_xe_session_targets xest
                    JOIN sys.dm_xe_sessions        xes  ON xes.address = xest.event_session_address
                    JOIN sys.server_event_sessions ses  ON xes.name    = ses.name
                WHERE xest.target_name = 'histogram' AND xes.name = 'LockCounts'
                 ) Locks
            CROSS APPLY LockData.nodes('//HistogramTarget/Slot') AS T(objstats)
        ) LockedObjects
        INNER JOIN sys.objects o  ON LockedObjects.lobject_id = o.object_id
        WHERE o.type != 'S' AND o.type = 'U'
        ORDER BY lock_count desc;
    GO
    
    -- Stop the event session.
    
    ALTER EVENT SESSION LockCounts ON SERVER
        state=stop;
    GO
    ```

> [!NOTE]
> 前面的 Transact-SQL 代码示例在 SQL Server 上本地运行，但可能 _不太在 Azure SQL 数据库上运行_。 该示例的核心部分直接涉及事件，例如 `ADD EVENT sqlserver.lock_acquired` 也可在 Azure SQL 数据库上运行。 但要运行示例，必须先将一些初步项（如 `sys.server_event_sessions`）编辑为其 Azure SQL 数据库对应项，如 `sys.database_event_sessions`。
> 若要详细了解本地 SQL Server 与 Azure SQL 数据库之间的细微差异，请参阅以下文章：
> - [Azure SQL 数据库中的扩展事件](/azure/sql-database/sql-database-xevent-db-diff-from-svr#transact-sql-differences)
> - [支持扩展事件的系统对象](xevents-references-system-objects.md)

前面的 Transact-SQL 脚本中的语句完成后，查询编辑器的“结果”选项卡将显示以下列  ：
  
- name
- object_id
- lock_count
  
## <a name="see-also"></a>另请参阅

[CREATE EVENT SESSION (Transact-SQL)](../../t-sql/statements/create-event-session-transact-sql.md)  
[ALTER EVENT SESSION (Transact-SQL)](../../t-sql/statements/alter-event-session-transact-sql.md)  
[sys.dm_xe_session_targets (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-targets-transact-sql.md)  
[sys.dm_xe_sessions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-sessions-transact-sql.md)  
[sys.server_event_sessions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)  
