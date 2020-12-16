---
title: 快速入门：SQL Server 中的扩展事件
description: 此快速入门可帮助你使用扩展事件（轻型性能监视系统）收集监视和解决 SQL Server 中的问题的数据。
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xevents
ms.topic: quickstart
f1_keywords:
- sql11.ssms.XeNewEventSession.General.f1
- sql11.ssms.XeNewEventSession.Events.f1
- sql11.ssms.XeNewEventSession.Targets.f1
- sql11.ssms.XeNewEventSession.Advanced.f1
ms.assetid: 7bb78b25-3433-4edb-a2ec-c8b2fa58dea1
author: MightyPen
ms.author: genemi
ms.reviewer: maghan
ms.date: 04/16/2020
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 37fdafe41500e8b146aed8e9840ee3027cc86281
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481378"
---
# <a name="quickstart-extended-events-in-sql-server"></a>快速入门：SQL Server 中的扩展事件

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

扩展事件是一种轻型性能监视系统，它允许用户收集监视和解决 SQL Server 中的问题所需的数据。 若要了解有关扩展事件体系结构的详细信息，请参阅[扩展事件概述](extended-events.md)。  本文旨在为不熟悉扩展事件以及想在几分钟内创建一个事件会话的 SQL 开发人员提供帮助。 通过使用扩展事件，你可以查看有关 SQL 系统和应用程序的内部操作的详细信息。 创建扩展事件会话时，需要告诉系统：

- 你感兴趣的事件实例。
- 希望系统以哪种方式向你报告数据。

本文将执行以下操作：

- 使用屏幕截图演示用于在 SSMS.exe 中创建事件会话的单击操作。
- 将屏幕截图与等效的 Transact-SQL 语句相互关联。
- 详细解释使用单击操作和 T-SQL 创建事件会话背后所用的术语和概念。
- 演示如何测试事件会话。
- 介绍关于结果的备选项：
  - 结果的存储捕获。
  - 已处理的结果与原始结果。
  - 用于在不同时间范围以不同方式查看结果的工具。
- 演示如何搜索并发现所有可用事件。
- 提供扩展事件动态管理视图 (DMV) 之间隐式的主键和外键关系。
- 介绍可从相关文章中了解的更多信息。

在博客和其他非正式对话中，有时将扩展事件简称为 *xevents*。

> [!NOTE]
> 有关 Azure SQL 数据库中的扩展事件的信息（包括代码示例），请参阅 [SQL 数据库中的扩展事件](/azure/azure-sql/database/xevent-db-diff-from-svr)。

## <a name="preparations-before-demo"></a>演示前的准备工作

实际执行即将进行的演示时，需做好以下准备工作。

1. [下载 SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)

   每个月应安装 SSMS 最新的每月更新。
2. 登录到 Microsoft SQL Server 2014 或更高版本。
3. 确保你的帐户具有 **更改任意事件会话**[服务器权限](../../t-sql/statements/grant-server-permissions-transact-sql.md)。

  如果有兴趣了解有关扩展事件的安全性和权限的更多详细信息，请参阅本文末尾的[附录](#appendix1)。

## <a name="demo-of-ssms-integration"></a>SSMS 集成演示

SSMS.exe 为扩展事件提供一个出色的用户界面 (UI)。 之所以出色，是因为许多用户通过使用 Transact-SQL 或面向扩展事件的动态管理视图 (DMV)，便无需参与到扩展事件中。

在本节中，你不仅可以看到创建扩展事件所需执行的 UI 步骤，还可以看到它报告的数据。 在完成这些步骤后，你可以更深入地了解步骤中涉及的概念。

### <a name="steps-of-demo"></a>演示步骤

即使你决定不执行这些操作，也可以试着了解一下。 此演示将启动“新建会话”  对话框。 我们将处理其四个页面，分别为：

- 常规
- 事件
- 数据存储
- 高级

在对 SSMS UI 进行月度或年度调整时，文本和支持屏幕截图可能会变得不太准确。 不过，如果差异较小，仍可使用屏幕截图来解释说明。

1. 与 SSMS 连接。

2. 在对象资源管理器中，依次单击“管理”   > “扩展事件”   > “新建会话”  。 “新建会话”  对话框优于“新建会话向导”  ，虽然两者非常相似。

3. 在左上角，单击“常规”  页。 然后在“会话名称”  文本框中，键入 *YourSession* 或者任何你喜欢的名称。 暂时 *不要* 按“确定”  按钮，该按钮要在演示结束时按。

   ![新建会话 > 常规 > 会话名称](../../relational-databases/extended-events/media/xevents-session-newsessions-10-general-ssms-yoursessionnode.png)

4. 在左上角，单击“事件”  页，然后单击“选择”  按钮。

   ![新建会话 > 事件 > 选择 > 事件库、所选事件](../../relational-databases/extended-events/media/xevents-session-newsessions-14-events-ssms-rightclick-not-wizard.png)

5. 在“事件库”  区域的下拉列表中，选择“仅事件名称”  。
    - 在文本框中键入 **sql**，这可以通过使用“包含”  运算符筛选并缩短可用事件长列表。
    - 滚动并单击名为 **sql_statement_completed** 的事件。
    - 单击右箭头按钮 **>** 将该事件移到“所选事件”  框中。

6. 继续留在“事件”  页，单击最右边的“配置”  按钮。

   为方便显示，左边已被截掉，在下面的屏幕截图中，你可以看到“事件配置选项”  区域。

    ![新建会话 > 事件 > 配置 > 筛选器(谓词) > 字段](../../relational-databases/extended-events/media/xevents-session-newsessions-20b-events-ssms-yoursessionnode.png)

7. 单击“筛选器(谓词)”  选项卡。接下来，单击“单击此处可添加子句”  ，以便捕获所有具有 HAVING 子句的 SQL SELECT 语句。

8. 在“字段”  下拉列表中，选择“sqlserver.sql_text”  。
   - 在“运算符”  中，选择 LIKE 运算符。
   - 在“值”  中，键入 **%SELECT%HAVING%** 。

   > [!NOTE]
   > 该名称由两部分组成，其中 sqlserver 是包名称，sql_text 是字段名称 。 我们之前选择的事件 sql_statement_completed 必须与我们选择的字段位于同一个包中。

9. 在左上角，单击“数据存储”  页。

10. 在“目标”区域中，单击“单击此处添加目标”   。
    - 在“类型”  下拉列表中，选择“event_file”  。
    - 这意味着，事件数据将存储在我们可以查看的文件中。
    
    > [!NOTE]
    > 不可在 SQL Server 的本地实例中将 Azure Blob 存储用作数据存储目标。

    ![新建会话 > 数据存储 > 目标 > 类型 > event_file](../../relational-databases/extended-events/media/xevents-session-newsessions-30-datastorage-ssms-yoursessionnode.png)

11. 在“属性”  区域的“服务器上的文件名”  文本框中，键入完整路径和文件名。
    - 文件扩展名必须为 *.xel*。
    - 我们的小测试需要小于 1 MB 的文件。

    ![新建会话 > 高级 > 最大调度滞后时间 > 确定](../../relational-databases/extended-events/media/xevents-session-newsessions-40-advanced-ssms-yoursessionnode.png)

12. 在左上角，单击“高级”  页。
    - 将“最大调度滞后时间”  减少至 3 秒。
    - 最后，单击底部的“确定”  按钮。

13. 返回到 **对象资源管理器**，展开“管理”   > “会话”  ，并查看 **YourSession** 的新节点。

    ![名为 YourSession 的新*事件会话*的节点，在对象资源管理器中，在“管理”>“扩展事件”>“会话”下面](../../relational-databases/extended-events/media/xevents-session-newsessions-50-objectexplorer-ssms-yoursessionnode.png)

#### <a name="edit-your-event-session"></a>编辑事件会话

在 SSMS 的 **对象资源管理器** 中，可以通过右键单击事件会话的节点来编辑该会话，然后单击“属性”  。 此时将显示相同的多页对话框。

### <a name="corresponding-t-sql-for-your-event-session"></a>事件会话的相应 T-SQL

你之前使用 SSMS UI 生成了一个创建事件会话的 T-SQL 脚本。 你可以看到生成的脚本，如下所示：

- 右键单击会话节点，依次单击“编写会话脚本为”   > “CREATE 到”   > “剪贴板”  。
- 粘贴到任意文本编辑器中。

下面是 *YourSession* 的 T-SQL CREATE EVENT SESSION 语句，该语句是通过在 UI 中单击生成的：

```sql
CREATE EVENT SESSION [YourSession]
    ON SERVER
    ADD EVENT sqlserver.sql_statement_completed
    (
        ACTION(sqlserver.sql_text)
        WHERE
        ( [sqlserver].[like_i_sql_unicode_string]([sqlserver].[sql_text], N'%SELECT%HAVING%')
        )
    )
    ADD TARGET package0.event_file
    (SET
        filename = N'C:\Junk\YourSession_Target.xel',
        max_file_size = (2),
        max_rollover_files = (2)
    )
    WITH (
        MAX_MEMORY = 2048 KB,
        EVENT_RETENTION_MODE = ALLOW_MULTIPLE_EVENT_LOSS,
        MAX_DISPATCH_LATENCY = 3 SECONDS,
        MAX_EVENT_SIZE = 0 KB,
        MEMORY_PARTITION_MODE = NONE,
        TRACK_CAUSALITY = OFF,
        STARTUP_STATE = OFF
    );
GO
```

#### <a name="pre-drop-of-the-event-session"></a>事件会话的预删除

在 CREATE EVENT SESSION 语句之前，你可能想要在名称已存在的情况下有条件地发出 DROP EVENT SESSION。

```sql
IF EXISTS (SELECT *
      FROM sys.server_event_sessions
      WHERE name = 'YourSession')
BEGIN
    DROP EVENT SESSION YourSession
          ON SERVER;
END
go
```

#### <a name="alter-to-start-and-stop-the-event-session"></a>使用 ALTER 启动和停止事件会话

在创建事件会话时，默认为不自动开始运行该会话。 你可以随时使用以下 T-SQL ALTER EVENT SESSION 语句启动或停止事件会话。

```sql
ALTER EVENT SESSION [YourSession]
      ON SERVER
    --ON DATABASE
    STATE = START;   -- STOP;
```

你可以选择指示事件会话在 SQL Server 实例启动时自动启动。 请参阅 CREATE EVENT SESSION 上的 **STARTUP STATE = ON** 关键字。

- SSMS UI 在“新建会话”   > “常规”  页上提供相应的复选框。

## <a name="test-your-event-session"></a>测试事件会话

通过执行以下这些简单步骤来测试事件会话：

1. 在 SSMS 的 **对象资源管理器** 中，右键单击事件会话节点，然后单击“启动会话”  。
2. 多次运行以下 `SELECT...HAVING` 语句。
    - 理想情况下，可能会在两次运行之间将 `HAVING Count` 值在 2 和 3 之间切换。 这样你就可以看到结果中的差异。
3. 右键单击会话节点，然后单击“停止会话”  。
4. 有关 [如何使用 SELECT 查看结果](#select-the-full-results-xml-37)，请阅读下一小节。

```sql
SELECT
        c.name,
        Count(*)  AS [Count-Per-Column-Repeated-Name]
    FROM
             sys.syscolumns  AS c
        JOIN sys.sysobjects  AS o
            ON o.id = c.id
    WHERE
        o.type = 'V'
        AND
        c.name like '%event%'
    GROUP BY
        c.name
    HAVING
        Count(*) >= 3   --2     -- Try both values during session.
    ORDER BY
        c.name;
```

为了保持完整性，下面提供了上述 SELECT...HAVING 的近似输出。

```
/*** Approximate output, 6 rows, all HAVING Count >= 3:
name                   Count-Per-Column-Repeated-Name
---------------------  ------------------------------
event_group_type       4
event_group_type_desc  4
event_session_address  5
event_session_id       5
is_trigger_event       4
trace_event_id         3
**_/
```

<a name="select-the-full-results-xml-37"/>

### <a name="select-the-full-results-as-xml"></a>使用 SELECT 查看 XML 形式的完整结果

在 SSMS 中，运行以下 T-SQL SELECT 以返回结果，其中每行提供一个事件实例的相关数据。 通过 CAST AS XML 可以轻松查看结果。

> [!NOTE]
> 事件系统始终向你指定的 .xel event_file 文件名追加一长串数字。 必须先复制系统提供的完整名称并将其粘贴到 SELECT 中，才能从该文件运行以下 SELECT。

```sql
SELECT
        object_name,
        file_name,
        file_offset,
        event_data,
        'CLICK_NEXT_CELL_TO_BROWSE_XML RESULTS!'
                AS [CLICK_NEXT_CELL_TO_BROWSE_XML_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]
                -- TODO: In ssms.exe results grid, double-click this xml cell!
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\Junk\YourSession_Target_0_131085363367310000.xel',
            null, null, null
        );
```

上述 SELECT 为你提供两种方法来查看任何给定事件行的完整结果：

- 在 SSMS 中运行 SELECT，然后单击 **event_data_XML** 列中的单元格。 这种方法非常方便。
- 从 **event_data** 列的单元格中复制长 XML 字符串。 粘贴到任意简单的文本编辑器中，如 Notepad.exe，并将字符串保存在扩展名为 .XML 的文件中。 然后使用浏览器打开该 .XML 文件。

#### <a name="display-of-results-for-one-event"></a>显示一个事件的结果

我们下面看到的是结果的一部分，其格式为 XML。 此处的 XML 经过编辑，已将其缩短以便于显示。 请注意， `<data name="row_count">` 显示值 `6`，这与前面显示的 6 行结果匹配。 我们可以看到整个 SELECT 语句。

```xml
<event name="sql_statement_completed" package="sqlserver" timestamp="2016-05-24T04:06:08.997Z">
  <data name="duration">
    <value>111021</value>
  </data>
  <data name="cpu_time">
    <value>109000</value>
  </data>
  <data name="physical_reads">
    <value>0</value>
  </data>
  <data name="last_row_count">
    <value>6</value>
  </data>
  <data name="offset">
    <value>0</value>
  </data>
  <data name="offset_end">
    <value>584</value>
  </data>
  <data name="statement">
    <value>SELECT
        c.name,
        Count(*)  AS [Count-Per-Column-Repeated-Name]
    FROM
             sys.syscolumns  AS c
        JOIN sys.sysobjects  AS o

            ON o.id = c.id
    WHERE
        o.type = 'V'
        AND
        c.name like '%event%'
    GROUP BY
        c.name
    HAVING
        Count(*) &gt;= 3   --2     -- Try both values during session.
    ORDER BY
        c.name</value>
  </data>
</event>
```

## <a name="ssms-to-display-results"></a>使用 SSMS 显示结果

你可以使用 SSMS UI 中的多项高级功能来查看从扩展事件捕获的数据。 有关详细信息，请参阅：

- [SQL Server 中扩展事件的目标数据的高级查看功能](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md)

先从基础的开始，即，标为“查看目标数据”  和“查看实时数据”  的上下文菜单选项。

### <a name="view-target-data"></a>查看目标数据

在 SSMS 的 **对象资源管理器** 中，可以右键单击事件会话节点下的目标节点。 在上下文菜单中单击“查看目标数据”  。 SSMS 将显示数据。

事件报告新数据时，显示内容不会更新。 不过，你可以再次单击“查看目标数据”  。

![查看目标数据，在 SSMS 中，管理 > 扩展事件 > 会话 > YourSession > package0.event_file，右键单击](../../relational-databases/extended-events/media/xevents-viewtargetdata-ssms-targetnode-61.png)

### <a name="watch-live-data"></a>查看实时数据

在 SSMS 的 **对象资源管理器** 中，可以右键单击事件会话节点。 在上下文菜单中单击“查看实时数据”  。 SSMS 将在传入数据持续实时到达时显示这些数据。

![查看实时数据，在 SSMS 中，管理 > 扩展事件 > 会话 > YourSession，右键单击](../../relational-databases/extended-events/media/xevents-watchlivedata-ssms-yoursessionnode-63.png)

## <a name="scenarios"></a>方案

在大量应用场景中，可以有效使用扩展事件。 以下文章提供了一些示例应用场景，这些场景涉及查询期间持有的锁。

以下文章介绍了一些旨在评估锁的事件会话的特定应用场景。 这些文章还介绍了一些高级技术，例如，使用 **\@dbid**，以及使用动态 `EXECUTE (@YourSqlString)`：

- [查找具有最多锁定的对象](../../relational-databases/extended-events/find-the-objects-that-have-the-most-locks-taken-on-them.md)
  - 此应用场景使用目标 package0.histogram，该目标会将原始事件数据处理后再向你显示。
- [确定持有锁的查询](../../relational-databases/extended-events/determine-which-queries-are-holding-locks.md)
  - 此应用场景使用 [目标 package0.pair_matching](/previous-versions/sql/sql-server-2016/ff878062(v=sql.130))，其中的事件对为 sqlserver.lock_acquire 和 lock_release。

## <a name="terms-and-concepts-in-extended-events"></a>扩展事件中的术语和概念

下表列出了用于扩展事件的术语，并描述了它们的含义。

| 术语 | 说明 |
| :--- | :---------- |
| 事件会话 | 一种构造，该构造以一个或多个事件为中心，再加上一些支持项（如操作或目标）。 CREATE EVENT SESSION 语句用于构造每个事件会话。 你可以随时使用 ALTER 来启动和停止事件会话。 <br/> <br/> 事件会话有时简称为 *会话*，但前提是上下文表明了它指的是 *事件会话*。 <br/> <br/> 有关事件会话的更多详细信息，请参阅：[SQL Server 扩展事件会话](../../relational-databases/extended-events/sql-server-extended-events-sessions.md)。 |
| event | 系统中发生的由活动事件会话监视的特定事件。 <br/> <br/> 例如， *sql_statement_completed* 事件表示任何给定 T-SQL 语句完成的那一刻。 该事件可以报告其持续时间和其他数据。 |
| 目标 | 从捕获的事件接收输出数据的项。 目标会向你显示数据。 <br/> <br/> 示例包括 *event_file*，以及它非常方便的轻量级同类 — 内存 *ring_buffer*。 更加高级的 *histogram* 目标可在显示数据之前对其进行一些处理。 <br/> <br/> 任何目标均可用于任何事件会话。 有关详细信息，请参阅 [SQL Server 中扩展事件的目标](../../relational-databases/extended-events/targets-for-extended-events-in-sql-server.md)。 |
| action | 事件已知的字段。 该字段中的数据将发送到目标。 操作字段与 *谓词筛选器* 密切相关。 |
| 谓词筛选器 | 对事件字段中数据的测试，通过使用该筛选器，可以仅将相关的一部分事件实例发送到目标。 <br/> <br/> 例如，筛选器可以只包含那些 T-SQL 语句含字符串 *HAVING* 的 *sql_statement_completed* 事件实例。 |
| 程序包 | 一种名称限定符，它附加到以事件核心为中心的一组项中的每个项。 <br/> <br/> 例如，包可能包含与 T-SQL 文本有关的事件。 一个事件可能与某个 GO 分隔批处理中的所有 T-SQL 有关。 同时，另一个更狭窄的事件与单个 T-SQL 语句有关。 此外，任何一个 T-SQL 语句都有开始事件和已完成事件。 <br/> <br/> 适用于事件的字段也位于事件所在的包中。 大多数目标位于 *package0* 中，可与其他许多包中的事件一起使用。 |

## <a name="how-to-discover-the-available-events-in-packages"></a>如何发现包中的可用事件

以下 T-SQL SELECT 语句将为每个名称中包含“sql”（由三个字符组成的字符串）的可用事件返回一行。 当然，你可以编辑 LIKE 值，以搜索不同的事件名称。 该行还会指定包含该事件的包。

```sql
SELECT   -- Find an event you want.
        p.name         AS [Package-Name],
        o.object_type,
        o.name         AS [Object-Name],
        o.description  AS [Object-Descr],
        p.guid         AS [Package-Guid]
    FROM
              sys.dm_xe_packages  AS p
        JOIN  sys.dm_xe_objects   AS o
                ON  p.guid = o.package_guid
    WHERE
        o.object_type = 'event'   --'action'  --'target'
        AND
        p.name LIKE '%'
        AND
        o.name LIKE '%sql%'
    ORDER BY
        p.name, o.object_type, o.name;
```

以下显示内容将显示返回的行，此处经过编辑后采用“列名称 = 值”的格式。 数据来自前面的示例步骤中所使用的 *sql-statement_completed* 事件。 Object-Descr 列的句子特别有用。

```
Package-Name = sqlserver
object_type  = event
Object-Name  = sql_statement_completed
Object-Descr = Occurs when a Transact-SQL statement has completed.
Package-Guid = 655FD93F-3364-40D5-B2BA-330F7FFB6491
```

### <a name="ssms-ui-for-search"></a>使用 SSMS UI 进行搜索

另一个搜索选项是使用 SSMS UI 的“新建会话”   > “事件”   > “事件库”  对话框（如前面的屏幕截图所示）。

### <a name="sql-trace-event-classes-with-extended-events"></a>SQL 跟踪事件类与扩展事件

有关通过 SQL 跟踪事件类和列使用扩展事件的说明，请参阅：[查看与 SQL 跟踪事件类等效的扩展事件](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)

### <a name="event-tracing-for-windows-etw-with-extended-events"></a>Windows 事件跟踪 (ETW) 与扩展事件

以下文章介绍了如何将扩展事件与 Windows 事件跟踪 (ETW) 配合使用：

- [Windows 事件跟踪目标](../../relational-databases/extended-events/event-tracing-for-windows-target.md)
- [使用扩展事件监视系统活动](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)

## <a name="additional-items"></a>其他项

本节简要介绍几个杂项。

### <a name="event-sessions-installed-with-sql-server"></a>随 SQL Server 安装的事件会话

SQL Server 附带了几个已创建的扩展事件。 它们都配置为每当 SQL 系统启动时启动。 这些事件会话收集的数据在发生系统错误时可能有用。 与所有扩展事件一样，它们占用的资源极少，因此，Microsoft 建议不干涉它们的运行。

你可以在 SSMS 的 **对象资源管理器** 中的“管理”   > “扩展事件”   > “会话”  下面看到这些事件会话。  从 2016 年 6 月起，已安装事件会话的列表如下：

- AlwaysOn_health
- system_health
- telemetry_events

### <a name="powershell-provider-for-extended-events"></a>用于扩展事件的 PowerShell 提供程序

你可以使用 SQL Server PowerShell 提供程序来管理 SQL Server 扩展事件。 有关详细信息，请参阅：[对扩展事件使用 PowerShell 提供程序](../../relational-databases/extended-events/use-the-powershell-provider-for-extended-events.md)

### <a name="system-views-for-extended-events"></a>扩展事件的系统视图

扩展事件的系统视图包括：

- *目录视图：* 提供已由 CREATE EVENT SESSION 定义的事件会话的相关信息。

- *动态管理视图 (DMV)：* 提供目前正在运行的事件会话的相关信息。

[SQL Server 中扩展事件系统视图中的 SELECT 和 JOIN](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md) — 提供以下内容的相关信息：

- 如何使视图彼此联接。
- 视图中几个有用的 SELECT。
- 以下对象之间的相关性：
  - 视图列。
  - CREATE EVENT SESSION 子句。
  - SSMS UI 控件。

## <a name="appendix-selects-to-ascertain-permission-owner-in-advance"></a><a name="appendix1"></a> 附录：使用 SELECT 提前确定权限所有者

本文中提到的权限包括：

- ALTER ANY EVENT SESSION
- VIEW SERVER STATE
- CONTROL SERVER

以下 Transact-SQL SELECT 语句可以报告拥有这些权限的人员。

### <a name="union-direct-permissions-plus-role-derived-permissions"></a>UNION 直接权限加上角色派生的权限

以下 SELECT...UNION ALL 语句返回的行显示了具有为扩展事件创建事件会话和查询系统目录视图所需的权限的人员。

```sql
-- Ascertain who has the permissions listed in the ON clause.
-- 'CONTROL SERVER' permission includes the permissions
-- 'ALTER ANY EVENT SESSION' and 'VIEW SERVER STATE'.
SELECT
        'Owner-is-Principal'  AS [Type-That-Owns-Permission],
        NULL                  AS [Role-Name],
        prin.name             AS [Owner-Name],

        perm.permission_name
            COLLATE Latin1_General_CI_AS_KS_WS
            AS [Permission-Name]
    FROM
             sys.server_permissions  AS perm
        JOIN sys.server_principals   AS prin

            ON prin.principal_id = perm.grantee_principal_id
    WHERE
        perm.permission_name IN
            ('ALTER ANY EVENT SESSION',
            'VIEW SERVER STATE',
            'CONTROL SERVER')
UNION ALL

-- Plus check for members of the 'sysadmin' fixed server role,
-- because 'sysadmin' includes the 'CONTROL SERVER' permission.
SELECT
        'Owner-is-Role'
        , prin.name  -- [Role-Name]
        , CAST( (IsNull(pri2.name, N'No members'))
            AS nvarchar(128))
        , NULL
    FROM
        sys.server_role_members  AS rolm
        RIGHT OUTER JOIN sys.server_principals    AS prin
            ON prin.principal_id = rolm.role_principal_id
        LEFT OUTER JOIN sys.server_principals     AS pri2
            ON rolm.member_principal_id = pri2.principal_id
    WHERE
        prin.name = 'sysadmin'
    ORDER BY
        1,2,3,4;
```

### <a name="has_perms_by_name-function"></a>HAS_PERMS_BY_NAME 函数

以下 SELECT 将报告你的权限。 它依赖于内置函数 [HAS_PERMS_BY_NAME](../../t-sql/functions/has-perms-by-name-transact-sql.md)。

此外，如果你有权暂时 *模拟* 其他帐户，则可以取消注释 [EXECUTE AS LOGIN](../../t-sql/statements/execute-as-transact-sql.md) 和 REVERT 语句，以便查询其他帐户。

```sql
--EXECUTE AS LOGIN = 'AccountNameHere';
SELECT HAS_PERMS_BY_NAME
    (
       null
       , null
       , 'ALTER ANY EVENT SESSION'
    );
--REVERT;
```

### <a name="security-links"></a>安全性链接

下面是与这些 SELECT 和权限相关的文档的链接：

- 内置函数 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)的详细信息
- [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)
- [GRANT 服务器权限 (Transact-SQL)](../../t-sql/statements/grant-server-permissions-transact-sql.md)
- [sys.server_principals (Transact-SQL)](../system-catalog-views/sys-server-principals-transact-sql.md)
- 博客：[Effective Database Engine Permissions](https://social.technet.microsoft.com/wiki/contents/articles/15180.effective-database-engine-permissions.aspx)（有效的数据库引擎权限）
- PDF 形式的可缩放 [海报](https://aka.ms/sql-permissions-poster)，显示所有 SQL Server 权限的层次结构。

## <a name="links-to-supporting-information"></a>支持信息链接

- [sys.fn_xe_file_target_read_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md)