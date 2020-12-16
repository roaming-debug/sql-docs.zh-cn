---
title: SQL Server 中扩展事件的目标
description: 本文介绍 SQL Server 中扩展事件的 package0 目标。 了解有关收集和报告数据及目标参数的目标功能。
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
ms.assetid: 47c64144-4432-4778-93b5-00496749665b
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 116d2f34bef990cca174117238981bb7e87917b4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481338"
---
# <a name="targets-for-extended-events-in-sql-server"></a>SQL Server 中扩展事件的目标

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


本文介绍何时以及如何使用 SQL Server 中扩展事件的 package0 目标。 对于每个目标，本文介绍：

- 其收集和报告由事件发送的数据的能力。
- 其参数（浅显易懂的参数除外）。


#### <a name="xquery-example"></a>XQuery 示例


[Ring_buffer 部分](#h2_target_ring_buffer) 包括举例说明如何使用 [Transact-SQL 中的 XQuery](../../xquery/xquery-language-reference-sql-server.md) 将 XML 的字符串复制到关系行集。


### <a name="prerequisites"></a>先决条件


- 需要对扩展事件的基础知识有一般性的了解，请参阅[快速入门：SQL Server 中的扩展事件](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md)。


- 已安装了经常更新的实用工具 SQL Server Management Studio (SSMS.exe) 的最新版本。 有关详细信息，请参阅：
    - [下载 SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)


- 在 SSMS.exe 中，知道如何在 **对象资源管理器** 中右键单击事件会话下的目标节点，以便 [轻松查看输出数据](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md)。
    - 事件数据被捕获为一个 XML 字符串。 但在本文中，数据将显示在关系行中。 SSMS 用于查看数据，然后将其复制并粘贴到本文中。
    - [ring_buffer 部分](#h2_target_ring_buffer)介绍了可选 T-SQL 技术，它可从 XML 中生成行集。 它涉及 XQuery。



## <a name="parameters-actions-and-fields"></a>参数、操作和字段


在 Transact-SQL 中， [CREATE EVENT SESSION](~/t-sql/statements/create-event-session-transact-sql.md) 语句是扩展事件的中心所在。 若要编写语句，经常需要以下内容的列表和说明：

- 与所选事件关联的字段。
- 与所选目标关联的参数。

从系统视图返回此类列表的 SELECT 语句可从以下文章的 C 节中复制：

- [SQL Server 中扩展事件系统视图中的 SELECT 和 JOIN](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md)
    - 事件的[C.4](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields) SELECT 字段。
    - 目标的[C.6](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_6_parameters_targets) SELECT 参数。
    - [C.3](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_3_select_all_available_objects) SELECT 操作。


可在 [此链接](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_B_2_TSQL_perspective)处查看实际 CREATE EVENT SESSION 语句的上下文中所使用的参数、字段和操作。



<a name="h2_target_etw_classic_sync_target"></a>

## <a name="etw_classic_sync_target-target"></a>etw_classic_sync_target 目标


SQL Server 扩展事件可以和 Windows 事件跟踪 (ETW) 相互操作来监视系统活动。 有关详细信息，请参阅：

- [Windows 事件跟踪目标](../../relational-databases/extended-events/event-tracing-for-windows-target.md)
- [使用扩展事件监视系统活动](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)


此 ETW 目标 *以同步方式* 处理其接收的数据，而大多数目标 *以异步方式* 进行处理。

> [!NOTE]
> Azure SQL 数据库不支持 `etw_classic_sync_target target`。

<!-- After OPS Versioning is live, the above !NOTE could be converted into a "3colon ZONE".  GeneMi = MightyPen. -->

<a name="h2_target_event_counter"></a>

## <a name="event_counter-target"></a>event_counter 目标


event_counter 目标只是对每个指定事件发生的次数进行计数。


与大多数其他目标不同：

- event_counter 不具有任何参数。


- 与大多数目标不同，event_counter 目标 *以同步方式* 处理其接收的数据。
    - 同步方式适用于简单的 event_counter 是因为 event_counter 只涉及极少的处理。
    - 数据库引擎将断开与任何速度太慢而可能降低数据库引擎性能的目标的连接。 这就是大多数目标 *以异步方式* 处理的原因。


#### <a name="example-output-captured-by-event_counter"></a>示例输入由 event_counter 捕获


```
package_name   event_name         count
------------   ----------         -----
sqlserver      checkpoint_begin   4
```


接下来介绍导致之前结果的 CREATE EVENT SESSION。 对于此测试，EVENT...WHERE 子句上的 **package0.counter** 字段用于在计数攀升到 4 后停止计数。


```sql
CREATE EVENT SESSION [event_counter_1]
    ON SERVER 
    ADD EVENT sqlserver.checkpoint_begin   -- Test by issuing CHECKPOINT; statements.
    (
        WHERE ([package0].[counter] <= (4))   -- A predicate filter.
    )
    ADD TARGET package0.event_counter
    WITH
    (
        MAX_MEMORY = 4096 KB,
        MAX_DISPATCH_LATENCY = 3 SECONDS
    );
```



<a name="h2_target_event_file"></a>

## <a name="event_file-target"></a>event_file 目标


**Event_file** 目标将缓冲区中的事件会话输出写入磁盘文件：


- 指定 ADD TARGET 子句上的 *filename =* 参数。
    - **.xel** 必须是文件扩展名。


- 系统会将你选择的文件名用作基于日期时间的长整型的前缀，后接 .xel 扩展名。

::: moniker range="= azuresqldb-current || = azuresqldb-mi-current "

> [!NOTE]
> Azure SQL 数据库仅支持在 Azure Blob 存储中存储 `xel` 文件。 
>
> 有关 SQL 数据库（和 SQL 托管实例）特定的 event_file 代码示例，请参阅 [SQL 数据库中扩展事件的事件文件目标代码](/azure/sql-database/sql-database-xevent-code-event-file)。

::: moniker-end


#### <a name="create-event-session-with-event_file-target"></a>使用 **event_file** 目标的 CREATE EVENT SESSION


接下来介绍用于测试的 CREATE EVENT SESSION。 ADD TARGET 子句之一指定 event_file。


```sql
CREATE EVENT SESSION [locks_acq_rel_eventfile_22]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET
            collect_database_name=(1),
            collect_resource_description=(1)

        ACTION (sqlserver.sql_text,sqlserver.transaction_id)

        WHERE
        (
            [database_name]=N'InMemTest2'
            AND
            [object_id]=(370100359)
        )
    ),
    ADD EVENT sqlserver.lock_released
    (
        SET
            collect_database_name=(1),
            collect_resource_description=(1)

        ACTION(sqlserver.sql_text,sqlserver.transaction_id)

        WHERE
        (
            [database_name]=N'InMemTest2'
            AND
            [object_id]=(370100359)
        )
    )
    ADD TARGET package0.event_counter,
    ADD TARGET package0.event_file
    (
        SET     filename=N'C:\Junk\locks_acq_rel_eventfile_22-.xel'
    )
    WITH
    (
        MAX_MEMORY=4096 KB,
        MAX_DISPATCH_LATENCY=10 SECONDS
    );
```


#### <a name="sysfn_xe_file_target_read_file-function"></a>sys.fn_xe_file_target_read_file 函数


Event_file 目标以二进制格式（不可人工读取）存储其接收的数据。 Transact SQL 可以通过从 [**sys.fn_xe_file_target_read_file**](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md) 函数进行选择来报告 .xel 文件的内容。


对于 SQL Server **2016** 及更高版本，由以下 T-SQL SELECT 报告数据。 *.xel 后缀 


```
SELECT f.*
        --,CAST(f.event_data AS XML)  AS [Event-Data-Cast-To-XML]  -- Optional
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\junk\locks_acq_rel_eventfile_22-*.xel',
            null, null, null)  AS f;
```


对于 SQL Server **2014**，与下面相似的 SELECT 将报告数据。 SQL Server 2014 后不再使用.xem 文件。


```
SELECT f.*
        --,CAST(f.event_data AS XML)  AS [Event-Data-Cast-To-XML]  -- Optional
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\junk\locks_acq_rel_eventfile_22-*.xel',
            'C:\junk\metafile.xem',
            null, null)  AS f;
```


当然，还可以手动使用 SSMS UI 来查看 .xel 数据：


#### <a name="data-stored-in-the-event_file-target"></a>event_file 目标中存储的数据


接下来介绍从 SQL Server 2016 的 **sys.fn_xe_file_target_read_file** 中进行选择所得的报告。


```
module_guid                            package_guid                           object_name     event_data                                                                                                                                                                                                                                                                                          file_name                                                      file_offset
-----------                            ------------                           -----------     ----------                                                                                                                                                                                                                                                                                          ---------                                                      -----------
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   lock_acquired   <event name="lock_acquired" package="sqlserver" timestamp="2016-08-07T20:13:35.827Z"><action name="transaction_id" package="sqlserver"><value>39194</value></action><action name="sql_text" package="sqlserver"><value><![CDATA[  select top 1 * from dbo.T_Target;  ]]></value></action></event>   C:\junk\locks_acq_rel_eventfile_22-_0_131150744126230000.xel   11776
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   lock_released   <event name="lock_released" package="sqlserver" timestamp="2016-08-07T20:13:35.832Z"><action name="transaction_id" package="sqlserver"><value>39194</value></action><action name="sql_text" package="sqlserver"><value><![CDATA[  select top 1 * from dbo.T_Target;  ]]></value></action></event>   C:\junk\locks_acq_rel_eventfile_22-_0_131150744126230000.xel   11776
```



<a name="h2_target_histogram"></a>

## <a name="histogram-target"></a>histogram 目标


**histogram** 目标比 event_counter 目标更精致。 histogram 可以执行下列操作：

- 单独计数多个项的出现次数。
- 计数不同类型的项的出现次数：
    - 事件字段。
    - 操作。


**source_type** 参数是控制 histogram 目标的关键：

- **source_type = 0** - 表示为事件字段  收集数据）。
- **source_type = 1** - 表示为操作  收集数据）。
    - 1 为默认值。


“slots”参数默认值为 256。 如果指定另一个值，该值将四舍五入为下一个 2 的幂。

- 例如，slots=59 四舍五入为 = 64。


### <a name="action-example-for-histogram"></a>histogram 的“操作”  示例


在其 TARGET...SET 子句上，以下 Transact-SQL CREATE EVENT SESSION 语句指定 **source_type=1** 的目标参数赋值。 1 表示 histogram 目标跟踪某项操作。

在本示例中，EVENT...ACTION 子句恰好只为目标提供了一个可选操作，即 **sqlos.system_thread_id**。 在 TARGET...SET 子句上，可以看到分配 **source=N'sqlos.system_thread_id** 赋值。

- 若要跟踪多个源操作，可以将第二个 histogram 目标添加到 CREATE EVENT SESSION 语句。


```sql
CREATE EVENT SESSION [histogram_lockacquired]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
        (
        ACTION
            (
            sqlos.system_thread_id
            )
        )
    ADD TARGET package0.histogram
        (
        SET
            filtering_event_name=N'sqlserver.lock_acquired',
            slots=(16),
            source=N'sqlos.system_thread_id',
            source_type=1
        )
    WITH
        (
        <.... (For brevity, numerous parameter assignments generated by SSMS.exe are not shown here.) ....>
        );
```


 捕获了以下数据。 **value** 列下的值是 system_thread_id 值。 例如，236 的总锁数在 线程 6540 下取得。


```
value   count
-----   -----
 6540     236
 9308      91
 9668      74
10144      49
 5244      44
 2396      28
```


#### <a name="select-to-discover-available-actions"></a>用 SELECT 发现可用操作


[C.3](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_3_select_all_available_objects) SELECT 语句可以查找系统上你可在 CREATE EVENT SESSION 语句中指定的操作。 在 WHERE 子句中，你将首先编辑 **o.name LIKE** 筛选器以匹配感兴趣的操作。


接下来介绍 C.3 SELECT 返回的示例行。 可在第二行中看到 **System_thread_id** 操作。


```
Package-Name   Action-Name                 Action-Description
------------   -----------                 ------------------
package0       collect_current_thread_id   Collect the current Windows thread ID
sqlos          system_thread_id            Collect current system thread ID
sqlserver      create_dump_all_threads     Create mini dump including all threads
sqlserver      create_dump_single_thread   Create mini dump for the current thread
```


### <a name="event-field-example-for-histogram"></a>histogram 的事件“字段”  示例


以下示例设置 **source_type=0**。 分配给 **source=** 的值就是事件字段（而非操作）。



```sql
CREATE EVENT SESSION [histogram_checkpoint_dbid]
    ON SERVER 
    ADD EVENT  sqlserver.checkpoint_begin
    ADD TARGET package0.histogram
    (
    SET
        filtering_event_name = N'sqlserver.checkpoint_begin',
        source               = N'database_id',
        source_type          = (0)
    )
    WITH
    ( <....> );
```


histogram 目标捕获下列数据。 数据显示 ID=5 的数据库经历了 7 个 checkpoint_begin 事件。


```
value   count
-----   -----
5       7
7       4
6       3
```


#### <a name="select-to-discover-available-fields-on-your-chosen-event"></a>用 SELECT 发现所选事件上的可用字段


[C.4](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields) SELECT 语句显示可从中选择的事件字段。 首先将 **o.name LIKE** 筛选器编辑为所选事件的名称。


C.4 SELECT 返回以下行集。 行集显示 database_id 是可为 histogram 目标提供值的 checkpoint_begin 事件上的唯一字段。


```
Package-Name   Event-Name         Field-Name   Field-Description
------------   ----------         ----------   -----------------
sqlserver      checkpoint_begin   database_id  NULL
sqlserver      checkpoint_end     database_id  NULL
```


<a name="h2_target_pair_matching"></a>

## <a name="pair_matching-target"></a>pair_matching 目标


pair_matching 目标可让你检测到没有对应结束事件发生的开始事件。 例如，lock_acquired 事件发生时，却没有匹配的 lock_released 事件随后及时发生，这就可能是个问题。


系统不会自动匹配开始和结束事件。 相反，你要在 CREATE EVENT SESSION 语句中向系统解释该匹配。 开始和结束事件匹配时，将弃用匹配对，以便每个人都可以专注于不匹配的开始事件。


#### <a name="finding-matchable-fields-for-the-start-and-end-event-pair"></a>为开始和结束事件对查找可匹配的字段


通过使用 [C.4 SELECT](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields)，我们看到下列行集中有大约 16 个 lock_acquired 事件的字段。 此处显示的行集已手动拆分以显示我们的示例可匹配哪些字段。 尝试匹配某些字段非常愚蠢，例如两个事件的 **duration** 上的字段。


```
Package-Name   Event-Name   Field-Name               Field-Description
------------   ----------   ----------               -----------------
sqlserver   lock_acquired   database_name            NULL
sqlserver   lock_acquired   mode                     NULL
sqlserver   lock_acquired   resource_0               The ID of the locked object, when lock_resource_type is OBJECT.
sqlserver   lock_acquired   resource_1               NULL
sqlserver   lock_acquired   resource_2               The ID of the lock partition, when lock_resource_type is OBJECT, and resource_1 is 0.
sqlserver   lock_acquired   transaction_id           NULL

sqlserver   lock_acquired   associated_object_id     The ID of the object that requested the lock that was acquired.
sqlserver   lock_acquired   database_id              NULL
sqlserver   lock_acquired   duration                 The time (in microseconds) between when the lock was requested and when it was canceled.
sqlserver   lock_acquired   lockspace_nest_id        NULL
sqlserver   lock_acquired   lockspace_sub_id         NULL
sqlserver   lock_acquired   lockspace_workspace_id   NULL
sqlserver   lock_acquired   object_id                The ID of the locked object, when lock_resource_type is OBJECT. For other lock resource types it will be 0
sqlserver   lock_acquired   owner_type               NULL
sqlserver   lock_acquired   resource_description     The description of the lock resource. The description depends on the type of lock. This is the same value as the resource_description column in the sys.dm_tran_locks view.
sqlserver   lock_acquired   resource_type            NULL
```


### <a name="example-of-pair_matching"></a>pair_matching 的示例


下面的 CREATE EVENT SESSION 语句指定两个事件和两个目标。 pair_matching 目标指定字段的两个集来将事件匹配成对。 分配给 **begin_matching_columns =** 和 **end_matching_columns =** 的以逗号分隔的字段序列必须相同。 即使空格正确，但是以逗号分隔的值中所涉及的字段之间不允许有制表符或换行符。

若要缩小结果范围，我们首先从 sys.objects 中进行选择来查找测试表的 object_id。 为该唯一 ID 向 EVENT...WHERE 子句添加筛选器。


```sql
CREATE EVENT SESSION [pair_matching_lock_a_r_33]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET
            collect_database_name = (1),
            collect_resource_description = (1)

        ACTION (sqlserver.transaction_id)

        WHERE
        (
            [database_name] = 'InMemTest2'
            AND
            [object_id] = 370100359
        )
    ),
    ADD EVENT sqlserver.lock_released
    (
        SET
            collect_database_name = (1),
            collect_resource_description = (1)

        ACTION (sqlserver.transaction_id)

        WHERE
        (
            [database_name] = 'InMemTest2'
            AND
            [object_id] = 370100359
        )
    )
    ADD TARGET package0.event_counter,
    ADD TARGET package0.pair_matching
    (
        SET
            begin_event = N'sqlserver.lock_acquired',
            begin_matching_columns =
                N'resource_0, resource_1, resource_2, transaction_id, database_id',

            end_event = N'sqlserver.lock_released',
            end_matching_columns =
                N'resource_0, resource_1, resource_2, transaction_id, database_id',

            respond_to_memory_pressure = (1)
    )
    WITH
    (
        MAX_MEMORY = 8192 KB,
        MAX_DISPATCH_LATENCY = 15 SECONDS
    );
```


若要测试事件会话，我们故意阻止已获取的锁被释放。 通过以下 T-SQL 步骤达到此目的：

1. BEGIN TRANSACTION。
2. UPDATE MyTable....
3. 故意不发出 COMMIT TRANSACTION，直到我们检查完目标。
4. 测试完成后，发出 COMMIT TRANSACTION。

简单的 **event_counter** 目标提供以下输出行。 因为 52-50=2，输出结果告诉我们，检查成对匹配目标中的输出时，应该会看到 2 个不成对的 lock_acquired 事件。


```
package_name   event_name      count
------------   ----------      -----
sqlserver      lock_acquired   52
sqlserver      lock_released   50
```


**pair_matching** 目标提供以下输出。 根据 event_counter 输出所示，确实看到 2 个 lock_acquired 行。 我们会看到这些行的这一事实，意味着两个 lock_acquired 事件不成对。


```
package_name   event_name      timestamp                     database_name   duration   mode   object_id   owner_type   resource_0   resource_1   resource_2   resource_description   resource_type   transaction_id
------------   ----------      ---------                     -------------   --------   ----   ---------   ----------   ----------   ----------   ----------   --------------------   -------------   --------------
sqlserver      lock_acquired   2016-08-05 12:45:47.9980000   InMemTest2      0          S      370100359   Transaction  370100359    3            0            [INDEX_OPERATION]      OBJECT          34126
sqlserver      lock_acquired   2016-08-05 12:45:47.9980000   InMemTest2      0          IX     370100359   Transaction  370100359    0            0                                   OBJECT          34126
```


不成对的 lock_acquired 事件中的行可能包括 T-SQL 文本或取得锁的 **sqlserver.sql_text**。 但我们不希望使显示内容过于复杂。


<a name="h2_target_ring_buffer"></a>

## <a name="ring_buffer-target"></a>ring_buffer 目标


ring_buffer 对快速和简单事件测试来说非常便利。 停止事件会话后，将弃用存储的输出。

在 ring_buffer 部分中，我们还会介绍如何使用 XQuery 的 Transact-SQL 实现来将 ring_buffer 的 XML 内容复制到更可读的关系行中。


#### <a name="create-event-session-with-ring_buffer"></a>使用 ring_buffer 的 CREATE EVENT SESSION


使用 ring_buffer 目标的 CREATE EVENT SESSION 语句并没有什么特别之处。


```sql
CREATE EVENT SESSION [ring_buffer_lock_acquired_4]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET collect_resource_description=(1)

        ACTION(sqlserver.database_name)

        WHERE
        (
            [object_id]=(370100359)  -- ID of MyTable
            AND
            sqlserver.database_name='InMemTest2'
        )
    )
    ADD TARGET package0.ring_buffer
    (
        SET max_events_limit=(98)
    )
    WITH
    (
        MAX_MEMORY=4096 KB,
        MAX_DISPATCH_LATENCY=3 SECONDS
    );
```


### <a name="xml-output-received-for-lock_acquired-by-ring_buffer"></a>由 ring_buffer 为 lock_acquired 接收的 XML 输出


由 SELECT 语句检索时，内容以 XML 字符串的形式表示。 接下来介绍测试中由 ring_buffer 目标存储的 XML 字符串。 但是，为简洁显示以下 XML，除了两个 <event> 以外的所有元素都会被清除。 此外，每个 <event> 内的少部分外部 <data> 元素已被删除。


```xml
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="6" eventCount="6" droppedCount="0" memoryUsed="1032">
  <event name="lock_acquired" package="sqlserver" timestamp="2016-08-05T23:59:53.987Z">
    <data name="mode">
      <type name="lock_mode" package="sqlserver"></type>
      <value>1</value>
      <text><![CDATA[SCH_S]]></text>
    </data>
    <data name="transaction_id">
      <type name="int64" package="package0"></type>
      <value>111030</value>
    </data>
    <data name="database_id">
      <type name="uint32" package="package0"></type>
      <value>5</value>
    </data>
    <data name="resource_0">
      <type name="uint32" package="package0"></type>
      <value>370100359</value>
    </data>
    <data name="resource_1">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="resource_2">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="database_name">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[]]></value>
    </data>
    <action name="database_name" package="sqlserver">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[InMemTest2]]></value>
    </action>
  </event>
  <event name="lock_acquired" package="sqlserver" timestamp="2016-08-05T23:59:56.012Z">
    <data name="mode">
      <type name="lock_mode" package="sqlserver"></type>
      <value>1</value>
      <text><![CDATA[SCH_S]]></text>
    </data>
    <data name="transaction_id">
      <type name="int64" package="package0"></type>
      <value>111039</value>
    </data>
    <data name="database_id">
      <type name="uint32" package="package0"></type>
      <value>5</value>
    </data>
    <data name="resource_0">
      <type name="uint32" package="package0"></type>
      <value>370100359</value>
    </data>
    <data name="resource_1">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="resource_2">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="database_name">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[]]></value>
    </data>
    <action name="database_name" package="sqlserver">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[InMemTest2]]></value>
    </action>
  </event>
</RingBufferTarget>
```


若要查看前述的 XML，可以在事件会话处于活动状态时发出以下 SELECT。 从系统视图 **sys.dm_xe_session_targets** 检索活动的 XML 数据 。


```sql
SELECT
        CAST(LocksAcquired.TargetXml AS XML)  AS RBufXml
    INTO
        #XmlAsTable
    FROM
        (
        SELECT
                CAST(t.target_data AS XML)  AS TargetXml
            FROM
                     sys.dm_xe_session_targets  AS t
                JOIN sys.dm_xe_sessions         AS s

                    ON s.address = t.event_session_address
            WHERE
                t.target_name = 'ring_buffer'
                AND
                s.name        = 'ring_buffer_lock_acquired_4'
        )
            AS LocksAcquired;


SELECT * FROM #XmlAsTable;
```


### <a name="xquery-to-see-the-xml-as-a-rowset"></a>用 XQuery 将 XML 作为行集查看


若要将前述的 XML 作为关系行集查看，通过发出以下 T-SQL 从前面的 SELECT 语句继续。 已注释的行解释了 XQuery 的每次使用。


```sql
SELECT
         -- (A)
         ObjectLocks.value('(@timestamp)[1]',
            'datetime'     )  AS [OccurredDtTm]

        -- (B)
        ,ObjectLocks.value('(data[@name="mode"]/text)[1]',
            'nvarchar(32)' )  AS [Mode]

        -- (C)
        ,ObjectLocks.value('(data[@name="transaction_id"]/value)[1]',
            'bigint' )  AS [TxnId]

        -- (D)
        ,ObjectLocks.value('(action[@name="database_name" and @package="sqlserver"]/value)[1]',
            'nvarchar(128)')  AS [DatabaseName]
    FROM
        #TableXmlCell
    CROSS APPLY
        -- (E)
        TargetDateAsXml.nodes('/RingBufferTarget/event[@name="lock_acquired"]')  AS T(ObjectLocks);
```


#### <a name="xquery-notes-from-preceding-select"></a>前述 SELECT 中的 XQuery 说明


(A)
- <event> 元素上的 timestamp= attribute's value。
- “(...)[1]”构造可确保每次迭代只返回 1 个值，这是 XML 数据类型变量和列的 value() XQuery 方法的必需限制。


(B)
- 其 name= attribute 等于“mode”的 <data> 元素内的 <text> 元素的内部值。


(C)
- 其 name= attribute 等于“transaction_id”的 <data> 元素内的 <value> 元素的内部值。


(D)
- <event> 包含 <action>。
- 其 name= attribute 等于“database_name”以及 package= attribute 等于“sqlserver”（非“package0”）的 <action>，将获取 <value> 元素的内部值。


(E)
- C.A. 导致其 name= attribute 等于“lock_acquired”的每个 <event> 元素都进行重复处理。
- 这适用于由前述 FROM 子句返回的 XML。


#### <a name="output-from-xquery-select"></a>XQuery SELECT 的输出


接下来介绍由包括 XQuery 的前述 T-SQL 生成的行集。


```
OccurredDtTm              Mode    DatabaseName
------------              ----    ------------
2016-08-05 23:59:53.987   SCH_S   InMemTest2
2016-08-05 23:59:56.013   SCH_S   InMemTest2
```



## <a name="xevent-net-namespaces-and-cx23"></a>XEvent .NET 命名空间和 C#


Package0 还有两个其他目标，但它们不能在 Transact-SQL 中使用：

- compressed_history
- event_stream


了解这两个目标无法在 T-SQL 中使用的一种方法是，列 *sys.dm_xe_objects.capabilities* 中的非 null 值不包括位 0x1。


可以在用 C# 等语言编写的 .NET 程序中使用 event_stream 目标。 C# 和其他.NET 开发人员可以通过 .NET Framework 类访问事件流，如命名空间 Microsoft.SqlServer.XEvents.Linq 中。

如果遇到错误 **25726** ，这意味着事件流填满数据的速度快于客户端使用数据的速度。 这导致数据库引擎断开与事件流的连接以避免拖慢服务器性能。


### <a name="xevent-namespaces"></a>XEvent 命名空间


- [Microsoft.SqlServer.Management.XEvent 命名空间](/dotnet/api/microsoft.sqlserver.management.xevent)

- [Microsoft.SqlServer.XEvent.Linq 命名空间](/dotnet/api/microsoft.sqlserver.xevent.linq)