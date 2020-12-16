---
title: 使用扩展事件监视系统活动
description: 使用扩展事件和 Windows 事件跟踪来监视系统活动。 了解 CREATE EVENT SESSION、ALTER EVENT SESSION 和 DROP EVENT SESSION。
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- xe
- extended events [SQL Server], monitoring system activity
ms.assetid: d83ad88f-818c-49fe-a9a9-299f704fca53
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b09dd7baae138fc559fd672ee43cad1761ca77c0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481398"
---
# <a name="monitor-system-activity-using-extended-events"></a>使用扩展事件监视系统活动

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  此过程说明如何将扩展事件和 Windows 事件跟踪 (ETW) 配合使用来监视系统活动。 此过程还说明如何使用 CREATE EVENT SESSION、ALTER EVENT SESSION 和 DROP EVENT SESSION 语句。  
  
 若要完成这些任务，需使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的查询编辑器执行以下过程。 此过程还要求使用命令提示符运行 ETW 命令。  
  
### <a name="to-monitor-system-activity-using-extended-events"></a>使用扩展事件监视系统活动  
  
1.  在查询编辑器中，发出下列语句创建事件会话并添加两个事件。 checkpoint_begin 和 checkpoint_end 事件在数据库检查点开始和结束时激发。  
  
    ```  
    CREATE EVENT SESSION test0  
    ON SERVER  
    ADD EVENT sqlserver.checkpoint_begin,  
    ADD EVENT sqlserver.checkpoint_end  
    WITH (MAX_DISPATCH_LATENCY = 1 SECONDS)  
    go  
    ```  
  
2.  添加具有 32 个存储桶的存储桶存储目标，以根据数据库 ID 对检查点数目进行计数。  
  
    ```  
    ALTER EVENT SESSION test0  
    ON SERVER  
    ADD TARGET package0.histogram  
    (  
          SET slots = 32, filtering_event_name = 'sqlserver.checkpoint_end', source_type = 0, source = 'database_id'  
    )  
    go  
    ```  
  
3.  发出下列语句添加 ETW 目标。 这样便能查看开始和结束事件，这些事件用于确定检查点的长度。  
  
    ```  
    ALTER EVENT SESSION test0  
    ON SERVER  
    ADD TARGET package0.etw_classic_sync_target  
    go  
    ```  
  
4.  发出下列语句启动会话并开始事件收集。  
  
    ```  
    ALTER EVENT SESSION test0  
    ON SERVER  
    STATE = start  
    go  
    ```  
  
5.  发出下列语句激发三个事件。  
  
    ```  
    USE tempdb  
          checkpoint  
    go  
    USE master  
          checkpoint  
          checkpoint  
    go  
    ```  
  
6.  发出下列语句查看事件计数。  
  
    ```  
    SELECT CAST(xest.target_data AS xml) Bucketizer_Target_Data_in_XML  
    FROM sys.dm_xe_session_targets xest  
    JOIN sys.dm_xe_sessions xes ON xes.address = xest.event_session_address  
    JOIN sys.server_event_sessions ses ON xes.name = ses.name  
    WHERE xest.target_name = 'histogram' AND xes.name = 'test0'  
    go  
    ```  
  
7.  在命令提示符下，发出下列命令查看 ETW 数据。  
  
    > [!NOTE]  
    >  若要获得 **tracerpt** 命令的帮助，请在命令提示符下输入 `tracerpt /?`。  
  
    ```  
    logman query -ets --- List the ETW sessions. This is optional.  
    logman update XE_DEFAULT_ETW_SESSION -fd -ets --- Flush the ETW log.  
    tracerpt %temp%\xeetw.etl -o xeetw.txt --- Dump the events so they can be seen.  
    ```  
  
8.  发出下列语句停止事件会话并将其从服务器上删除。  

    ```  
    ALTER EVENT SESSION test0  
    ON SERVER  
    STATE = STOP  
    go  
  
    DROP EVENT SESSION test0  
    ON SERVER  
    go  
    ```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE EVENT SESSION (Transact-SQL)](../../t-sql/statements/create-event-session-transact-sql.md)   
 [ALTER EVENT SESSION (Transact-SQL)](../../t-sql/statements/alter-event-session-transact-sql.md)   
 [DROP EVENT SESSION (Transact-SQL)](../../t-sql/statements/drop-event-session-transact-sql.md)   
 [扩展事件目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [扩展事件动态管理视图](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [SQL Server 扩展事件目标](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130))  
  
