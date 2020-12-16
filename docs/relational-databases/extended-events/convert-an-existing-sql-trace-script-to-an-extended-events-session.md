---
title: 将 SQL 跟踪脚本转换为扩展事件会话
description: 使用这些过程可以创建一个扩展事件会话，该会话等效于要转换的现有 SQL 跟踪脚本。
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- SQL Trace, convert script to extended events
- extended events [SQL Server], convert SQL Trace script
ms.assetid: 4c8f29e6-0a37-490f-88b3-33493871b3f9
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fe788a0764f3d3d93159dd1e9782aa788d337973
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465588"
---
# <a name="convert-an-existing-sql-trace-script-to-an-extended-events-session"></a>将现有 SQL 跟踪脚本转换为扩展事件会话

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  如果您具有想要转换为扩展事件会话的现有 SQL 跟踪脚本，则可以使用本主题中的过程创建等效的扩展事件会话。 通过使用 trace_xe_action_map 和 trace_xe_event_map 系统表中的信息，你可以收集进行转换所必需的信息。  
  
 这些步骤包括以下内容：  
  
1.  执行现有脚本以便创建一个 SQL 跟踪会话，然后获取该跟踪的 ID。  
  
2.  运行一个查询，该查询使用 fn_trace_geteventinfo 函数来为每个 SQL 跟踪事件类及其关联列找到等效的扩展事件的事件和操作。  
  
3.  使用 fn_trace_getfilterinfo 函数列出要使用的筛选器和等效的扩展事件操作。  
  
4.  手动创建扩展事件会话，并且使用等效的扩展事件的事件、操作和谓词（筛选器）。  

## <a name="to-obtain-the-trace-id"></a>获取跟踪 ID  
  
1.  在查询编辑器中打开 SQL 跟踪脚本，然后执行该脚本以便创建跟踪会话。 请注意，无需运行该跟踪会话即可完成此过程。  
  
2.  获取跟踪的 ID。 为此，请使用以下查询：  
  
    ```sql
    SELECT * FROM sys.traces;  
    GO  
    ```  
  
    > [!NOTE]  
    >  跟踪 ID 1 通常指示默认跟踪。  
  
## <a name="to-determine-the-extended-events-equivalents"></a>确定等效的扩展事件  
  
1.  若要确定等效的扩展事件的事件和操作，请运行以下查询，其中，将 *trace_id* 设置为你在之前过程中获取的跟踪 ID 的值。  
  
    > [!NOTE]  
    >  在这个示例中，使用默认跟踪 (1) 的跟踪 ID。  
  
    ```sql
    USE MASTER;  
    GO  
    DECLARE @trace_id int;  
    SET @trace_id = 1;  
    SELECT DISTINCT el.eventid, em.package_name, em.xe_event_name AS 'event'  
       , el.columnid, ec.xe_action_name AS 'action'  
    FROM (sys.fn_trace_geteventinfo(@trace_id) AS el  
       LEFT OUTER JOIN sys.trace_xe_event_map AS em  
          ON el.eventid = em.trace_event_id)  
    LEFT OUTER JOIN sys.trace_xe_action_map AS ec  
       ON el.columnid = ec.trace_column_id  
    WHERE em.xe_event_name IS NOT NULL AND ec.xe_action_name IS NOT NULL;  
    ```  
  
     将返回等效的扩展事件的事件 ID、包名称、事件名称、列 ID 和操作名称。 您将在本主题后面的“创建扩展事件会话”过程中使用此输出。  
  
     在某些情况下，筛选列将映射到默认在扩展事件的事件中包括的事件数据字段。 因此，“Extended_Events_action_name”列将为 NULL。 如果发生此情况，您必须执行以下操作以便确定哪一数据字段等效于筛选列：  
  
    1.  对于返回 NULL 的操作，标识脚本中哪些 SQL 跟踪事件类包含要筛选的列。  
  
         例如，你可能使用了 SP:StmtCompleted 事件类，并且对 Duration 跟踪列名（SQL 跟踪事件类 ID 45 和 SQL 跟踪列 ID 13）指定了一个筛选器。 在此情况下，该操作名称将以 NULL 的形式出现在查询结果中。  
  
    2.  对于您在前一步骤中标识的每个 SQL 跟踪事件类，找到等效的扩展事件的事件名称。 （如果你不确定等效的事件名称，请使用 [查看与 SQL 跟踪事件类等效的扩展事件](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)主题中的查询。）  
  
    3.  使用下面的查询可以标识要用于您在前一步骤中标识的事件的正确的数据字段。 该查询将在“事件字段”列中显示扩展事件数据字段。 在该查询中，用前一步骤中指定的事件名称替换 *<event_name>* 。  
  
        ```sql
        SELECT xp.name package_name, xe.name event_name  
           ,xc.name event_field, xc.description  
        FROM sys.trace_xe_event_map AS em  
        INNER JOIN sys.dm_xe_objects AS xe  
           ON em.xe_event_name = xe.name  
        INNER JOIN sys.dm_xe_packages AS xp  
           ON xe.package_guid = xp.guid AND em.package_name = xp.name  
        INNER JOIN sys.dm_xe_object_columns AS xc  
           ON xe.name = xc.object_name  
        WHERE xe.object_type = 'event' AND xc.column_type <> 'readonly'  
           AND em.xe_event_name = '<event_name>';  
        ```  
  
         例如，SP:StmtCompleted 事件类映射到 sp_statement_completed 扩展事件的事件。 如果你在查询中将 sp_statement_completed 指定为事件名称，则“event_field”列将显示随该事件默认包括的字段。 在查看这些字段时，您会看到有一个“duration”字段。 若要在等效的扩展事件会话中创建筛选器，你需要添加一个谓词，例如“WHERE duration > 0”。 有关示例，请参阅本主题中的“创建扩展事件会话”过程。  
  
## <a name="to-create-the-extended-events-session"></a>创建扩展事件会话  
 使用查询编辑器可以创建扩展事件会话，并且将输出写入某一文件目标。 下面的步骤描述单个查询，并且提供介绍如何生成查询的说明。 有关完整查询示例，请参阅本主题的“示例”部分。  
  
1.  添加语句以创建事件会话，并且使用要用于扩展事件会话的名称替换session_name  。  
  
    ```sql
    IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='session_name')  
       DROP EVENT SESSION [Session_Name] ON SERVER;  
    CREATE EVENT SESSION [Session_Name]  
    ON SERVER;  
    ```  
  
2.  添加在“确定等效的扩展事件”过程中作为输出返回的扩展事件的事件和操作，并且添加在“确定在脚本中使用的筛选器”过程中标识的谓词（筛选器）。  
  
     下面的示例使用一个 SQL 跟踪脚本，该脚本包括 SQL:StmtStarting 和 SP:StmtCompleted 事件类，以及用于会话 ID 和持续时间的筛选器。 “确定等效的扩展事件”过程中查询的示例输出返回了以下结果集：  
  
    ```  
    Eventid  package_name  event                   columnid  action  
    44       sqlserver     sp_statement_starting   6         nt_username  
    44       sqlserver     sp_statement_starting   9         client_pid  
    44       sqlserver     sp_statement_starting   10        client_app_name  
    44       sqlserver     sp_statement_starting   11        server_principal_name  
    44       sqlserver     sp_statement_starting   12        session_id  
    45       sqlserver     sp_statement_completed  6         nt_username  
    45       sqlserver     sp_statement_completed  9         client_pid  
    45       sqlserver     sp_statement_completed  10        client_app_name  
    45       sqlserver     sp_statement_completed  11        server_principal_name  
    45       sqlserver     sp_statement_completed  12        session_id  
    ```  
  
     为了将其转换为等效的扩展事件，添加了 sqlserver.sp_statement_starting 和 sqlserver.sp_statement_completed 事件以及操作列表。 谓词语句作为 WHERE 子句包括。  
  
    ```sql
    ADD EVENT sqlserver.sp_statement_starting  
       (ACTION  
          (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
          )  
       WHERE sqlserver.session_id = 59   
       ),  
  
    ADD EVENT sqlserver.sp_statement_completed  
       (ACTION  
          (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
          )  
       WHERE sqlserver.session_id = 59 AND duration > 0  
       )  
    ```  
  
3.  添加异步文件目标，并且使用要用来保存输出的位置替换文件路径。 在指定文件目标时，必须包括日志文件和元数据文件路径文件。  
  
    ```sql
    ADD TARGET package0.asynchronous_file_target(  
       SET filename='c:\temp\ExtendedEventsStoredProcs.xel', metadatafile='c:\temp\ExtendedEventsStoredProcs.xem');  
    ```  
  
## <a name="to-view-the-results"></a>查看结果  
  
1.  可使用 sys.fn_xe_file_target_read_file 函数查看输出。 为此，运行以下查询，并且使用您指定的路径替换文件路径：  
  
    ```sql
    SELECT *, CAST(event_data as XML) AS 'event_data_XML'  
    FROM sys.fn_xe_file_target_read_file('c:\temp\ExtendedEventsStoredProcs*.xel', 'c:\temp\ExtendedEventsStoredProcs*.xem', NULL, NULL);  
  
    ```  
  
    > [!NOTE]  
    >  将事件数据转换为 XML 是可选的。  
  
     有关 sys.fn_xe_file_target_read_file 函数的详细信息，请参阅 [sys.fn_xe_file_target_read_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md)。  
  
    ```sql
    IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='session_name')  
       DROP EVENT SESSION [session_name] ON SERVER;  
    CREATE EVENT SESSION [session_name]  
    ON SERVER  
  
    ADD EVENT sqlserver.sp_statement_starting  
       (ACTION  
       (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
       )  
       WHERE sqlserver.session_id = 59   
       ),  
  
    ADD EVENT sqlserver.sp_statement_completed  
       (ACTION  
       (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
       )  
       WHERE sqlserver.session_id = 59 AND duration > 0  
       );  
  
    ADD TARGET package0.asynchronous_file_target  
       (SET filename='c:\temp\ExtendedEventsStoredProcs.xel', metadatafile='c:\temp\ExtendedEventsStoredProcs.xem');  
    ```  
  
## <a name="example"></a>示例  
  
```sql
IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='session_name')  
   DROP EVENT SESSION [session_name] ON SERVER;  
CREATE EVENT SESSION [session_name]  
ON SERVER  
  
ADD EVENT sqlserver.sp_statement_starting  
   (ACTION  
   (  
      sqlserver.nt_username,  
      sqlserver.client_pid,  
      sqlserver.client_app_name,  
      sqlserver.server_principal_name,  
      sqlserver.session_id  
   )  
   WHERE sqlserver.session_id = 59   
   ),  
  
ADD EVENT sqlserver.sp_statement_completed  
   (ACTION  
   (  
      sqlserver.nt_username,  
      sqlserver.client_pid,  
      sqlserver.client_app_name,  
      sqlserver.server_principal_name,  
      sqlserver.session_id  
   )  
   WHERE sqlserver.session_id = 59 AND duration > 0  
   )  
  
ADD TARGET package0.asynchronous_file_target  
   (SET filename='c:\temp\ExtendedEventsStoredProcs.xel', metadatafile='c:\temp\ExtendedEventsStoredProcs.xem');  
```  
  
## <a name="see-also"></a>另请参阅  
 [查看与 SQL 跟踪事件类等效的扩展事件](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)  
  
  
