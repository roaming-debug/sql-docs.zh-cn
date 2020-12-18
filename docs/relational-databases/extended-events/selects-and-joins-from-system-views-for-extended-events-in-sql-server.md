---
title: 扩展事件系统视图中的 SELECT 和 JOIN
description: SQL Server 和 Azure SQL 数据库中提供了扩展事件的系统视图。 了解如何以不同视角表示事件会话信息。
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
ms.assetid: 04521d7f-588c-4259-abc2-1a2857eb05ec
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2dc811a3e3217c3aa6bf2d9a006cfd1ff0c7796b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481468"
---
# <a name="selects-and-joins-from-system-views-for-extended-events-in-sql-server"></a>SQL Server 中扩展事件系统视图中的 SELECT 和 JOIN

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


本文介绍了与 SQL Server 和 Azure SQL 数据库中的扩展事件相关的两组系统视图。 本文阐释了：

- 如何联接各种系统视图。
- 如何从系统视图中选择特定种类的信息。
- 如何从各种技术角度展示相同的事件会话信息，并以此加强对各个角度的理解。


大多数示例是针对 SQL Server 编写的。 但是只要进行少量编辑就可以使它们在 SQL 数据库上运行。



## <a name="a-foundational-information"></a>A. 基本信息


扩展事件有两组系统视图：


#### <a name="catalog-views"></a>目录视图：

- 这些视图用于存储由 [CREATE EVENT SESSION](../../t-sql/statements/create-event-session-transact-sql.md) 或 SSMS UI 中的对等功能创建的所有事件会话的定义信息。  但是，这些视图不知道曾经是否启动运行了任何会话。
    - 例如，如果 SSMS 的 **对象资源管理器** 显示未定义任何事件会话，则针对视图 *sys.server_event_session_targets* 的 SELECT 语句将不返回任何行。


- 名称前缀为：
    - sys.server\_event\_session\* 是 SQL Server 上的名称前缀。 
    - sys.database\_event\_session\* 是 SQL 数据库上的名称前缀。 


#### <a name="dynamic-management-views-dmvs"></a>动态管理视图 (DMV)：

- 用于存储正在运行的事件会话的当前活动  信息。 但是这些 DMV 对会话定义知之甚少。
    - 即使所有事件会话当前已停止，针对视图 *sys.dm_xe_packages* 的 SELECT 语句仍将返回行，因为各种包已加载到服务器启动的活动内存。
    - 出于同一原因，sys.dm_xe_objects sys.dm_xe_object_columns 也仍将返回行   。


- 扩展事件的 DMV 的名称前缀为：
    - *sys.dm\_xe\_\** 是 SQL Server 上的名称前缀。
    - *sys.dm\_xe\_database\_\** 通常是 SQL 数据库上的名称前缀。


#### <a name="permissions"></a>权限：


要对系统视图执行 SELECT 语句，需要以下权限：

- VIEW SERVER STATE - 如果是在 Microsoft SQL Server 上。
- VIEW DATABASE STATE - 如果是在 Azure SQL 数据库上。


<a name="section_B_catalog_views"></a>

## <a name="b-catalog-views"></a>B. 目录视图


本部分针对相同定义的事件会话匹配和关联三种不同的技术角度。 已在 SQL Server Management Studio (SSMS.exe) 的 **对象资源管理器** 中定义并显示会话，但是该会话当前未运行。

每月 [安装 SSMS 的最新更新](../../ssms/download-sql-server-management-studio-ssms.md)是明智之举，以避免意外故障。


有关扩展事件的目录视图的参考文档位于 [Extended Events Catalog Views (Transact-SQL)](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)（扩展事件目录视图 (Transact-SQL)）。


&nbsp;



#### <a name="the-sequence-in-this-section-b"></a>B 部分的内容顺序


- [B.1 SSMS UI 角度](#section_B_1_SSMS_UI_perspective)
    - 使用 SSMS UI 创建事件会话的定义。 显示分步屏幕截图。


- [B.2 Transact-SQL 角度](#section_B_2_TSQL_perspective)
    - 使用 SSMS 上下文菜单将已定义的事件会话反向工程为等效的 Transact-SQL **CREATE EVENT SESSION** 语句。 T-SQL 与 SSMS 屏幕截图的选项完全对应。


- [B.3 目录视图 SELECT JOIN UNION 角度](#section_B_3_Catalog_view_S_J_UNION)
    - 从我们的事件会话的系统目录视图发出 T-SQL SELECT 语句。 该结果与 **CREATE EVENT SESSION** 语句规范一致。


&nbsp;



<a name="section_B_1_SSMS_UI_perspective"></a>

### <a name="b1-ssms-ui-perspective"></a>B.1 SSMS UI 角度


在 SSMS 的“对象资源管理器”中，可以通过展开“管理” > “扩展事件”，然后右键单击“会话” > “新建会话”打开“新建会话”对话框。      

在“新建会话”的大对话框中标记为“常规”的第一部分中，我们看到已选中选项“在服务器启动时启动事件会话”。   

![新建会话 > 常规，在服务器启动时启动事件会话。](../../relational-databases/extended-events/media/xevents-ssms-ac105-eventname-startup.png)


接下来在“事件”  部分中，我们看到已选中 **lock_deadlock** 事件。 对于该事件，我们看到已选择三个 **操作**。 这意味着已单击“配置”  按钮，该按钮单击后变成灰色。

![新建会话 > 事件，全局字段 (操作)](../../relational-databases/extended-events/media/xevents-ssms-ac110-actions-global.png)


<a name="resource_type_PAGE_cat_view"></a>

接下来，还是在“事件” > “配置”部分中，我们看到 [resource_type 已设置为 PAGE](#resource_type_dmv_actual_row)。     这意味着如果 **resource_type** 的值是除 **PAGE** 以外的任何值，那么事件数据不会从事件引擎发送到目标。

另外我们还看到数据库名称和计数器的谓词筛选器。

![新建会话 > 事件，筛选谓词字段 (操作)](../../relational-databases/extended-events/media/xevents-ssms-ac115-predicate-db.png)


接下来在“数据存储”  部分中，我们看到已选择 **event_file** 作为目标。 此外，我们看到已选中“启用文件滚动更新”  选项。

![新建会话 > 数据存储，eventfile_enablefileroleover](../../relational-databases/extended-events/media/xevents-ssms-ac120-target-eventfile.png)


最后，在“高级”  部分中，我们看到“最大调度滞后时间”  的值已减少到 4 秒。

![新建会话 > 高级，最大调度滞后时间](../../relational-databases/extended-events/media/xevents-ssms-ac125-latency4.png)


上述设置从 SSMS UI 角度完成了事件会话定义。


<a name="section_B_2_TSQL_perspective"></a>

### <a name="b2-transact-sql-perspective"></a>B.2 Transact-SQL 角度


不管如何创建事件会话定义，都可以在 SSMS UI 中将会话反向工程为完全匹配的 Transact-SQL 脚本。 你可以检查前面的“新建会话”屏幕截图，将图中显示的设置与下面生成的 T-SQL **CREATE EVENT SESSION** 脚本中的子句进行比较。

若要对事件会话进行反向工程，在“对象资源管理器”  中右键单击会话节点，然后选择“编写会话脚本为”   > “创建到”   > “剪贴板”  。

下面的 T-SQL 脚本通过使用 SSMS 进行反向工程创建。 然后仅通过空格的策略性操作手动修饰该脚本。


```sql
CREATE EVENT SESSION [event_session_test3]
    ON SERVER  -- Or, if on Azure SQL Database, ON DATABASE.

    ADD EVENT sqlserver.lock_deadlock
    (
        SET
            collect_database_name = (1)
        ACTION
        (
            package0  .collect_system_time,
            package0  .event_sequence,
            sqlserver .client_hostname
        )
        WHERE
        (
            [database_name]           = N'InMemTest2'
            AND [package0].[counter] <= (16)
            AND [resource_type]       = (6)
        )
    )

    ADD TARGET package0.event_file
    (
        SET
            filename           = N'C:\Junk\event_session_test3_EF.xel',
            max_file_size      = (20),
            max_rollover_files = (2)
    )

    WITH
    (
        MAX_MEMORY            = 4096 KB,
        EVENT_RETENTION_MODE  = ALLOW_SINGLE_EVENT_LOSS,
        MAX_DISPATCH_LATENCY  = 4 SECONDS,
        MAX_EVENT_SIZE        = 0 KB,
        MEMORY_PARTITION_MODE = NONE,
        TRACK_CAUSALITY       = OFF,
        STARTUP_STATE         = ON
    );
```


这就完成了从 T-SQL 角度来展示事件会话的信息。


<a name="section_B_3_Catalog_view_S_J_UNION"></a>

### <a name="b3-catalog-view-select-join-union-perspective"></a>B.3 目录视图 SELECT JOIN UNION 角度


别害怕！ 以下 T-SQL SELECT 语句很长只是因为它 UNION 了几个小的 SELECT 语句。 任何一个小的 SELECT 语句都可以自己运行。 这些小的 SELECT 语句显示如何将各种系统目录视图 JOIN 在一起。


```sql
SELECT
        s.name        AS [Session-Name],
        '1_EVENT'     AS [Clause-Type],
        'Event-Name'  AS [Parameter-Name],
        e.name        AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_events   AS e

            ON  e.event_session_id = s.event_session_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name         AS [Session-Name],
        '2_EVENT_SET'  AS [Clause-Type],
        f.name         AS [Parameter-Name],
        f.value        AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_events   AS e

            ON  e.event_session_id = s.event_session_id

        JOIN  sys.server_event_session_fields   As f

            ON  f.event_session_id = s.event_session_id
            AND f.object_id        = e.event_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name              AS [Session-Name],
        '3_EVENT_ACTION'    AS [Clause-Type],

        a.package + '.' + a.name
                            AS [Parameter-Name],

        '(Not_Applicable)'  AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_events   AS e

            ON  e.event_session_id = s.event_session_id

        JOIN  sys.server_event_session_actions  As a

            ON  a.event_session_id = s.event_session_id
            AND a.event_id         = e.event_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name                AS [Session-Name],
        '4_EVENT_PREDICATES'  AS [Clause-Type],
        e.predicate           AS [Parameter-Name],
        '(Not_Applicable)'    AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_events   AS e

            ON  e.event_session_id = s.event_session_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name              AS [Session-Name],
        '5_TARGET'          AS [Clause-Type],
        t.name              AS [Parameter-Name],
        '(Not_Applicable)'  AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_targets  AS t

            ON  t.event_session_id = s.event_session_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name          AS [Session-Name],
        '6_TARGET_SET'  AS [Clause-Type],
        f.name          AS [Parameter-Name],
        f.value         AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_targets  AS t

            ON  t.event_session_id = s.event_session_id

        JOIN  sys.server_event_session_fields   As f

            ON  f.event_session_id = s.event_session_id
            AND f.object_id        = t.target_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name               AS [Session-Name],
        '7_WITH_MAX_MEMORY'  AS [Clause-Type],
        'max_memory'         AS [Parameter-Name],
        s.max_memory         AS [Parameter-Value]
    FROM
              sys.server_event_sessions  AS s
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name                  AS [Session-Name],
        '7_WITH_STARTUP_STATE'  AS [Clause-Type],
        'startup_state'         AS [Parameter-Name],
        s.startup_state         AS [Parameter-Value]
    FROM
              sys.server_event_sessions  AS s
    WHERE
        s.name = 'event_session_test3'

ORDER BY
    [Session-Name],
    [Clause-Type],
    [Parameter-Name]
;
```


#### <a name="output"></a>输出


下表显示了运行上述 SELECT JOIN UNION 的输出。 输出参数名称和值与前面的 CREATE EVENT SESSION 语句中的平面文本格式的输出相对应。

| Session-Name | Clause-Type | Parameter-Name | Parameter-Value |
|---|---|---|---|
|event_session_test3  | 1_EVENT |                Event-Name |                       lock_deadlock |
|event_session_test3  |  2_EVENT_SET |             collect_database_name |            1 |
|event_session_test3  |  3_EVENT_ACTION |          sqlserver.client_hostname |       (Not_Applicable) |
|event_session_test3  |  3_EVENT_ACTION |         sqlserver.collect_system_time |   (Not_Applicable) |
|event_session_test3  |  3_EVENT_ACTION |         sqlserver.event_sequence |        (Not_Applicable) |
|event_session_test3  |  4_EVENT_PREDICATES |     (\[sqlserver\].\[equal_i_sql_unicode_string\]\(\[database_name\],N'InMemTest2'\) AND \[package0\].\[counter\]<=\(16\)\) |   (Not_Applicable) |
|event_session_test3  |  5_TARGET |               event_file |                      (Not_Applicable) |
|event_session_test3  |  6_TARGET_SET |           filename  |                       C:\Junk\event_session_test3_EF.xel |
|event_session_test3  |  6_TARGET_SET |           max_file_size |                   20 |
|event_session_test3  |  6_TARGET_SET |           max_rollover_files |              2 |
|event_session_test3  |  7_WITH_MAX_MEMORY |      max_memory |                      4096 |
|event_session_test3  |  7_WITH_STARTUP_STATE |   startup_state |                   1 |

至此完成了有关目录视图的章节。



<a name="section_C_DMVs"></a>

## <a name="c-dynamic-management-views-dmvs"></a>C. 动态管理视图 (DMV)


现在我们过渡到 DMV。 本部分提供了几个具有特定业务用途的 Transact-SQL SELECT 语句。 此外，这些 SELECT 语句演示了如何针对你需要的新用途将 DMV JOIN 在一起。


相关 DMV 的参考文档可从 [《Extended Events Dynamic Management Views》](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)（扩展事件动态管理视图）中获取


在本文中，以下 SELECT 语句的任何实际输出行都来自 SQL Server 2016，除非另行指定。


下面是此 DMV 小节 C 中 SELECT 语句列表：

- [C.1 列出所有包](#section_C_1_list_packages)
- [C.2 统计每个对象类型的数量](#section_C_2_count_object_type)
- [C.3 使用 SELECT 语句查询所有可用项，按类型排序](#section_C_3_select_all_available_objects)
- [C.4 查询可用于你的事件的数据字段](#section_C_4_data_fields)
- [C.5 *sys.dm_xe_map_values* 和事件字段](#section_C_5_map_values_fields)
- [C.6 查询目标的参数](#section_C_6_parameters_targets)
- [C.7 使用 DMV SELECT 语句将 target_data 列转换为 XML](#section_C_7_dmv_select_target_data_column)
- [C.8 针对函数执行 SELECT 语句，以检索磁盘驱动器中的 event_file 数据](#section_C_8_select_function_disk)



<a name="section_C_1_list_packages"></a>

### <a name="c1-list-of-all-packages"></a>C.1 列出所有包


可以在扩展事件区域中使用的所有对象均来自加载到系统的包。 本节将列出所有包及其说明。


```sql
SELECT  --C.1
        p.name         AS [Package],
        p.description  AS [Package-Description]
    FROM
        sys.dm_xe_packages  AS p
    ORDER BY
        p.name;
```


#### <a name="output"></a>输出

以下是包的列表。

| 程序包        |Package-Description|
|---|---|
|文件流 (filestream)|     SQL Server FILESTREAM 和 FileTable 的扩展事件 |
|package0   |    默认包。 包含所有标准类型、映射、比较运算符、操作和目标 |
|qds         |   查询存储的扩展事件 |
|SecAudit     |  安全审核事件 |
|sqlclr        | SQL CLR 的扩展事件 |
|sqlos         | SQL 操作系统的扩展事件 |
|SQLSatellite |  SQL Satellite 的扩展事件 |
|sqlserver   |   Microsoft SQL Server 的扩展事件 |
|sqlserver  |    Microsoft SQL Server 的扩展事件 |
|sqlserver  |    Microsoft SQL Server 的扩展事件 |
|sqlsni     |    Microsoft SQL Server 的扩展事件 |
|ucs        |    统一通信堆栈的扩展事件 |
|XtpCompile |    XTP 编译的扩展事件 |
|XtpEngine  |    XTP 引擎的扩展事件 |
|XtpRuntime |    XTP 运行时的扩展事件 |


*上面出现的缩略词的定义：*

- clr = .NET 公共语言运行时
- qds = 查询数据存储
- sni = 服务器网络接口
- ucs = 统一通信堆栈
- xtp = 极限事务处理


<a name="section_C_2_count_object_type"></a>

### <a name="c2-count-of-every-object-type"></a>C.2 统计每个对象类型的数量


本节将告诉我们事件包包含的对象类型。 将显示 sys.dm\_xe\_objects 中的所有对象类型的完整列表以及每个类型的计数。 


```sql
SELECT  --C.2
        Count(*)  AS [Count-of-Type],
        o.object_type
    FROM
        sys.dm_xe_objects  AS o
    GROUP BY
        o.object_type
    ORDER BY
        1  DESC;
```


#### <a name="output"></a>输出

下面是每个对象类型的对象的计数。 大约有 1915 个对象。

|Count-of-Type |   object_type |
|---|---|
|1303|            event |
|351  |           map |
|84    |          message |
|77     |         pred_compare |
|53     |        action |
|46     |         pred_source |
|28     |         type |
|17     |         目标 |

<a name="section_C_3_select_all_available_objects"></a>

### <a name="c3-select-all-available-items-sorted-by-type"></a>C.3 使用 SELECT 语句查询所有可用项，按类型排序


以下 SELECT 语句返回大约 1915 个行，每一行针对一个对象。



```sql
SELECT  --C.3
        o.object_type  AS [Type-of-Item],
        p.name         AS [Package],
        o.name         AS [Item],
        o.description  AS [Item-Description]
    FROM
             sys.dm_xe_objects  AS o
        JOIN sys.dm_xe_packages AS p  ON o.package_guid = p.guid
    WHERE
        o.object_type IN ('action' , 'target' , 'pred_source')
        AND
        (
            (o.capabilities & 1) = 0
            OR
            o.capabilities IS NULL
        )
    ORDER BY
        [Type-of-Item],
        [Package],
        [Item];
```


#### <a name="output"></a>输出

为了激发你的兴趣，下面是前面的 SELECT 语句返回的对象的任意采样。


|Type-of-Item|   程序包|        Item|                          Item-Description|
|---|---|---|---|
|action|         package0  |     callstack                     |收集当前调用堆栈|
|action |        package0  |     debug_break                   |中断默认调试器中的进程|
|action |       sqlos      |    task_time                     |收集当前任务执行时间|
|action |        sqlserver |     sql_text                     | 收集 SQL 文本|
|event  |        qds       |     query_store_aprc_regression  | 查询存储检测到查询计划性能回归时触发|
|event  |        SQLSatellite |  connection_accept            | 接受一个新连接时发生。 此事件用于记录所有连接尝试。|
|event  |        XtpCompile  |   cgen                          |开始生成 C 代码时发生。|
|map    |        qds         |   aprc_state                    |查询存储自动计划回归校正状态|
|message |       package0    |   histogram_event_required      |源类型为 0 时，需要指定“filtering_event_name”参数的值。|
|pred_compare |  package0   |    equal_ansi_string             |两个 ANSI 字符串值之间的相等运算符|
|pred_compare |  sqlserver  |    equal_i_sql_ansi_string       |两个 SQL ANSI 字符串值之间的相等运算符|
|pred_source |   sqlos      |    task_execution_time           |获取当前任务执行时间|
|pred_source |   sqlserver  |    client_app_name               |获取当前客户端应用程序名称|
|目标 |        package0   |    etw_classic_sync_target       |针对 Windows (ETW) 同步目标的事件跟踪|
|目标 |        package0   |    event_counter                 |使用 event_counter 目标计算各事件在事件会话中出现的次数。|
|目标  |       package0  |     event_file                    |使用 event_file 目标将事件数据保存到 XEL 文件中，可以存档该文件以用于以后的分析和审阅。 可以合并多个 XEL 文件以查看来自不同事件会话的组合数据。|
|目标  |       package0   |    histogram                     |使用直方图目标根据特定事件数据字段或与事件关联的操作聚合事件数据。 直方图可用于分析事件会话期间事件数据的分布情况。|
|目标   |      package0  |     pair_matching                 |Pairing target|
|目标   |      package0  |     ring_buffer                   |异步环形缓冲区目标|
|type     |      package0  |     xml                           |格式正确的 XML 片段|

<a name="section_C_4_data_fields"></a>

### <a name="c4-data-fields-available-for-your-event"></a>C.4 查询可用于你的事件的数据字段


以下 SELECT 语句将返回特定于事件类型的所有数据字段。

- 请注意 WHERE 子句项：column_type = 'data'。 
- 此外，你需要编辑 WHERE 子句 o.name = 的值。 


```sql
SELECT  -- C.4
        p.name         AS [Package],
        c.object_name  AS [Event],
        c.name         AS [Column-for-Predicate-Data],
        c.description  AS [Column-Description]
    FROM
              sys.dm_xe_object_columns  AS c
        JOIN  sys.dm_xe_objects         AS o

            ON  o.name = c.object_name

        JOIN  sys.dm_xe_packages        AS p

            ON  p.guid = o.package_guid
    WHERE
        c.column_type = 'data'
        AND
        o.object_type = 'event'
        AND
        o.name        = '\<EVENT-NAME-HERE!>'  --'lock_deadlock'
    ORDER BY
        [Package],
        [Event],
        [Column-for-Predicate-Data];
```


#### <a name="output"></a>输出

以下行由前面的 SELECT 语句返回，其中 `o.name = 'lock_deadlock'`：

- 每一行代表针对 sqlserver.lock_deadlock  事件的可选筛选器。
- 以下显示中忽略了 *\[Column-Description\]* 列。 它的值通常为 NULL。
- 这是实际输出，但省略的“说明”列（通常为 NULL）除外。
- 这些行是 object_type = 'lock_deadlock' 的位置。

|程序包|     事件|           Column-for-Predicate-Data|
|---|---|---|
|sqlserver|   lock_deadlock|   associated_object_id|
|sqlserver|  lock_deadlock |  database_id|
|sqlserver|  lock_deadlock |  database_name|
|sqlserver|   lock_deadlock|   deadlock_id|
|sqlserver|   lock_deadlock|   duration|
|sqlserver|   lock_deadlock|   lockspace_nest_id|
|sqlserver|   lock_deadlock|   lockspace_sub_id|
|sqlserver|   lock_deadlock|   lockspace_workspace_id|
|sqlserver|   lock_deadlock|   mode|
|sqlserver|   lock_deadlock|   object_id|
|sqlserver|   lock_deadlock|   owner_type|
|sqlserver|   lock_deadlock|   resource_0|
|sqlserver|   lock_deadlock|  resource_1|
|sqlserver|   lock_deadlock|   resource_2|
|sqlserver|   lock_deadlock|   resource_description|
|sqlserver|   lock_deadlock|   resource_type|
|sqlserver|   lock_deadlock|   transaction_id|




<a name="section_C_5_map_values_fields"></a>

### <a name="c5-sysdm_xe_map_values-and-event-fields"></a>C.5 *sys.dm_xe_map_values* 和事件字段


下面的 SELECT 语句包括对名为 sys.dm_xe_map_values 的复杂视图的联接。 

该 SELECT 语句的目的是显示可以为事件会话选择的许多字段。 可采用两种方式使用事件字段：

- 选择每个事件发生时要写入目标的字段值
- 筛选要将哪些发生的事件发送到目标和从目标中排除。


```sql
SELECT  --C.5
        dp.name         AS [Package],
        do.name         AS [Object],
        do.object_type  AS [Object-Type],
        'o--c'     AS [O--C],
        dc.name         AS [Column],
        dc.type_name    AS [Column-Type-Name],
        dc.column_type  AS [Column-Type],
        dc.column_value AS [Column-Value],
        'c--m'     AS [C--M],
        dm.map_value    AS [Map-Value],
        dm.map_key      AS [Map-Key]
    FROM
              sys.dm_xe_objects         AS do
        JOIN  sys.dm_xe_object_columns  AS dc

            ON  dc.object_name = do.name

        JOIN  sys.dm_xe_map_values      AS dm

            ON  dm.name = dc.type_name

        JOIN  sys.dm_xe_packages        AS dp

            ON  dp.guid = do.package_guid
    WHERE
        do.object_type = 'event'
        AND
        do.name        = '\<YOUR-EVENT-NAME-HERE!>'  --'lock_deadlock'
    ORDER BY
        [Package],
        [Object],
        [Column],
        [Map-Value];
```


#### <a name="output"></a>输出

<a name="resource_type_dmv_actual_row"></a>

接下来是前面的 T-SQL SELECT 语句的实际的 153 行输出采样。 **resource_type** 这一行与本文中别处的 [event_session_test3](#resource_type_PAGE_cat_view) 示例中使用的谓词筛选 **相关** 。


```
/**_  5 sampled rows from the actual 153 rows returned.
    NOTE:  'resource_type' under 'Column'.

Package     Object          Object-Type   O--C   Column          Column-Type-Name     Column-Type   Column-Value   C--M   Map-Value        Map-Key
-------     ------          -----------   ----   ------          ----------------     -----------   ------------   ----   ---------        -------
sqlserver   lock_deadlock   event         o--c   CHANNEL         etw_channel          readonly      2              c--m   Operational      4
sqlserver   lock_deadlock   event         o--c   KEYWORD         keyword_map          readonly      16             c--m   access_methods   1024
sqlserver   lock_deadlock   event         o--c   mode            lock_mode            data          NULL           c--m   IX               8
sqlserver   lock_deadlock   event         o--c   owner_type      lock_owner_type      data          NULL           c--m   Cursor           2
sqlserver   lock_deadlock   event         o--c   resource_type   lock_resource_type   data          NULL           c--m   PAGE             6

Therefore, on your CREATE EVENT SESSION statement, in its ADD EVENT WHERE clause,
you could put:
    WHERE( ... resource_type = 6 ...)  -- Meaning:  6 = PAGE.
_*_/
```


<a name="section_C_6_parameters_targets"></a>

### <a name="c6-parameters-for-targets"></a>C.6 查询目标的参数


以下 SELECT 语句将返回目标的每个参数。 每个参数都标记了是否为必选参数。 为参数指定的值将影响目标的行为。

- 请注意 WHERE 子句项：_object_type = 'customizable'*。
- 此外，你需要编辑 WHERE 子句 o.name = 的值。 


```sql
SELECT  --C.6
        p.name        AS [Package],
        o.name        AS [Target],
        c.name        AS [Parameter],
        c.type_name   AS [Parameter-Type],

        CASE c.capabilities_desc
            WHEN 'mandatory' THEN 'YES_Mandatory'
            ELSE 'Not_mandatory'
        END  AS [IsMandatoryYN],

        c.description AS [Parameter-Description]
    FROM
              sys.dm_xe_objects   AS o
        JOIN  sys.dm_xe_packages  AS p

            ON  o.package_guid = p.guid

        LEFT OUTER JOIN  sys.dm_xe_object_columns  AS c

            ON  o.name        = c.object_name
            AND c.column_type = 'customizable'  -- !
    WHERE
        o.object_type = 'target'
        AND
        o.name     LIKE '%'    -- Or '\<YOUR-TARGET-NAME-HERE!>'.
    ORDER BY
        [Package],
        [Target],
        [IsMandatoryYN]  DESC,
        [Parameter];
```


#### <a name="output"></a>输出

以下参数行是由前面的 SQL Server 2016 中的 SELECT 语句所返回内容的子集。


```
/***  Actual output, all rows, where target name = 'event_file'.
Package    Target       Parameter            Parameter-Type       IsMandatoryYN   Parameter-Description
-------    ------       ---------            --------------       -------------   ---------------------
package0   event_file   filename             unicode_string_ptr   YES_Mandatory   Specifies the location and file name of the log
package0   event_file   increment            uint64               Not_mandatory   Size in MB to grow the file
package0   event_file   lazy_create_blob     boolean              Not_mandatory   Create blob upon publishing of first event buffer, not before.
package0   event_file   max_file_size        uint64               Not_mandatory   Maximum file size in MB
package0   event_file   max_rollover_files   uint32               Not_mandatory   Maximum number of files to retain
package0   event_file   metadatafile         unicode_string_ptr   Not_mandatory   Not used
**_/
```


<a name="section_C_7_dmv_select_target_data_column"></a>

### <a name="c7-dmv-select-casting-target_data-column-to-xml"></a>C.7 使用 DMV SELECT 语句将 target_data 列转换为 XML


此 DMV SELECT 语句将返回活动事件会话目标的数据行。 数据被转换为 XML 格式，使其返回的单元格可单击，从而可在 SSMS 中轻松显示。

- 如果事件会话已停止，则 SELECT 语句将不返回任何行。
- 你需要编辑 WHERE 子句 _s.name =* 的值。


```sql
SELECT  --C.7
        s.name,
        t.target_name,
        CAST(t.target_data AS XML)  AS [XML-Cast]
    FROM
              sys.dm_xe_session_targets  AS t
        JOIN  sys.dm_xe_sessions         AS s

            ON s.address = t.event_session_address
    WHERE
        s.name = '\<Your-Session-Name-Here!>';
```


#### <a name="output-the-only-row-including-its-xml-cell"></a>输出，唯一行，包括其 XML 单元格

下面是上述 SELECT 语句输出的唯一行。 列 XML-Cast 包含的 SSMS 理解的字符串为 XML 格式。  因此 SSMS 知道这可以使 XML-Cast 单元格可单击。


对于该运行：

- s.name = 的值已设为 checkpoint_begin 事件的事件会话。  
- 目标为 ring_buffer  。


```XML
name                              target_name   XML-Cast
----                              -----------   --------
checkpoint_session_ring_buffer2   ring_buffer   <RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="104"><event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T01:28:23.508Z"><data name="database_id"><type name="uint32" package="package0" /><value>5</value></data></event><event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T01:28:26.975Z"><data name="database_id"><type name="uint32" package="package0" /><value>5</value></data></event></RingBufferTarget>
```


#### <a name="output-xml-displayed-pretty-when-cell-is-clicked"></a>输出中，单击单元格后显示完美的 XML


单击 XML-Cast 单元格后，将显示以下完美的内容。


```xml
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="104">
  <event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T01:28:23.508Z">
    <data name="database_id">
      <type name="uint32" package="package0" />
      <value>5</value>
    </data>
  </event>
  <event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T01:28:26.975Z">
    <data name="database_id">
      <type name="uint32" package="package0" />
      <value>5</value>
    </data>
  </event>
</RingBufferTarget>
```


<a name="section_C_8_select_function_disk"></a>

### <a name="c8-select-from-a-function-to-retrieve-event_file-data-from-disk-drive"></a>C.8 针对函数执行 SELECT 语句，以检索磁盘驱动器中的 event_file 数据


假设事件会话收集了一些数据，之后停止了。 如果会话定义为使用 event_file 目标，则仍可以通过调用函数 sys.fn_xe_target_read_file  来检索数据。

- 在运行该 SELECT 语句之前必须编辑该函数调用的参数的路径和文件名称。
    - 不用在意每次重新启动会话时 SQL 系统在实际的 XEL 文件名称中嵌入的额外的数字。 只需指定一般的根名称和扩展名。


```sql
SELECT  --C.8
        f.module_guid,
        f.package_guid,
        f.object_name,
        f.file_name,
        f.file_offset,
        CAST(f.event_data AS XML)  AS [Event-Data-As-XML]
    FROM
        sys.fn_xe_file_target_read_file(

            '\<YOUR-PATH-FILE-NAME-ROOT-HERE!>*.xel',
            --'C:\Junk\Checkpoint_Begins_ES*.xel',  -- Example.

            NULL, NULL, NULL
        )  AS f;
```


#### <a name="output-rows-returned-by-select-from-the-function"></a>输出，针对函数执行 SELECT 语句返回的行


以下是上面的针对函数执行 SELECT 语句返回的行。 最右端的 XML 列包含专门针对发生的事件的数据。


```
module_guid                            package_guid                           object_name        file_name                                                           file_offset   Event-Data-As-XML
-----------                            ------------                           -----------        ---------                                                           -----------   -----------------
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   checkpoint_begin   C:\Junk\Checkpoint_Begins_ES_20160615bb-_0_131125086091700000.xel   5120          <event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T03:30:14.023Z"><data name="database_id"><value>5</value></data><action name="session_id" package="sqlserver"><value>60</value></action><action name="database_id" package="sqlserver"><value>5</value></action></event>
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   checkpoint_end     C:\Junk\Checkpoint_Begins_ES_20160615bb-_0_131125086091700000.xel   5120          <event name="checkpoint_end" package="sqlserver" timestamp="2016-07-09T03:30:14.025Z"><data name="database_id"><value>5</value></data></event>
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   checkpoint_begin   C:\Junk\Checkpoint_Begins_ES_20160615bb-_0_131125086091700000.xel   5632          <event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T03:30:17.704Z"><data name="database_id"><value>5</value></data><action name="session_id" package="sqlserver"><value>60</value></action><action name="database_id" package="sqlserver"><value>5</value></action></event>
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   checkpoint_end     C:\Junk\Checkpoint_Begins_ES_20160615bb-_0_131125086091700000.xel   5632          <event name="checkpoint_end" package="sqlserver" timestamp="2016-07-09T03:30:17.709Z"><data name="database_id"><value>5</value></data></event>
```


#### <a name="output-one-xml-cell"></a>输出，一个 XML 单元格


以下是前面返回的行集的第一个 XML 单元格的内容。


```xml
<event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T03:30:14.023Z">
  <data name="database_id">
    <value>5</value>
  </data>
  <action name="session_id" package="sqlserver">
    <value>60</value>
  </action>
  <action name="database_id" package="sqlserver">
    <value>5</value>
  </action>
</event>
```