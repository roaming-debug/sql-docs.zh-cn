---
description: 管理和监视变更数据捕获 (SQL Server)
title: 管理和监视变更数据捕获
ms.date: 01/02/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- change data capture [SQL Server], monitoring
- change data capture [SQL Server], administering
- change data capture [SQL Server], jobs
ms.assetid: 23bda497-67b2-4e7b-8e4d-f1f9a2236685
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: dfc040c4157cfd44a27a0b05a3bef42a1591aa2e
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128647"
---
# <a name="administer-and-monitor-change-data-capture-sql-server"></a>管理和监视变更数据捕获 (SQL Server)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]
  本主题介绍如何管理和监视变更数据捕获。  
  
## <a name="capture-job"></a><a name="Capture"></a> 捕获作业

捕获作业可通过运行无参数存储过程 `sp_MScdc_capture_job` 来启动。 此存储过程启动时，将从 msdb.dbo.cdc_jobs 中为捕获作业提取 `maxtrans`、`maxscans`、`continuous` 和 `pollinginterval` 的配置值。 然后，这些配置值会作为参数传递到存储过程 `sp_cdc_scan`。 该存储过程用于调用 `sp_replcmds` 以执行日志扫描。  
  
### <a name="capture-job-parameters"></a>捕获作业参数  

若要了解捕获作业行为，必须了解 `sp_cdc_scan` 使用可配置参数的方式。
  
#### <a name="maxtrans-parameter"></a>maxtrans 参数

`maxtrans` 参数用于指定能够在日志的单个扫描循环中处理的最大事务数。 如果在该扫描期间要处理的事务数达到该限制，则当前扫描中将不包括任何其他事务。 一个扫描循环完成后，已处理的事务数将始终小于或等于 `maxtrans`。

#### <a name="maxscans-parameter"></a>maxscans 参数

`maxscans` 参数用于指定在返回 (continuous = 0) 或执行 waitfor (continuous = 1) 之前为完成日志扫描可以尝试的最大扫描循环次数。

#### <a name="continuous-parameter"></a>continuous 参数

`continuous` 参数用于控制在完成日志扫描或执行的扫描循环达到最大次数之后，`sp_cdc_scan` 是否放弃控制（单次触发模式）。 它还控制在显式停止 `sp_cdc_scan` 之前此存储过程是否继续运行（继续模式）。  
  
##### <a name="one-shot-mode"></a>单次触发模式

在单次触发模式中，捕获作业请求 `sp_cdc_scan` 执行最多 `maxtrans` 次扫描以尝试全部扫描完日志，然后返回。 日志中存在的超出 `maxtrans` 数量的任何事务将在稍后的扫描中处理。  
  
 单次触发模式用于待处理事务量已知的受控测试中，优点是作业完成后将自动关闭。 建议不要将单次触发模式用于生产。 这是因为它依赖于作业计划来管理扫描循环的运行频率。  
  
 以单次触发模式运行时，你可以计算捕获作业的预期吞吐量的上限并以每秒的事务数来表示。其计算方法如下：  
  
 `(maxtrans * maxscans) / number of seconds between scans`  
  
 即使扫描日志和填充更改表所需的时间与 0 相差不多，作业的平均吞吐量也不能超过以如下方式所获得的值：即将单次扫描所允许的最大事务数乘以允许的最大扫描次数后除以日志处理间隔的秒数。  
  
 如果将单次触发模式用于控制日志扫描，则日志处理间隔的秒数将不得不受作业计划的控制。 如果需要采用这种方式，则以连续模式运行捕获作业是一种管理重新计划日志扫描的更好方式。  
  
##### <a name="continuous-mode-and-the-polling-interval"></a>连续模式和轮询间隔

在连续模式下，捕获作业将请求 `sp_cdc_scan` 连续运行。 这使得存储过程可通过提供 maxtrans 和 maxscans 值以及日志处理间隔（轮询间隔）的秒数值来管理自己的等待循环。 在该模式下运行，捕获作业会保持活动状态，并在日志扫描间隔内执行 `WAITFOR`。  
  
> [!NOTE]  
> 如果轮询间隔的值大于 0，则重复执行单次触发作业的吞吐量的同一上限也适用于连续模式下的作业操作。 也就是说，将 (`maxtrans` \* `maxscans`) 除以非零轮询间隔即可得到捕获作业可以处理的平均事务数的上限。  
  
### <a name="capture-job-customization"></a>捕获作业自定义

对于捕获作业，可以应用其他逻辑来确定是立即开始新扫描，还是在启动新扫描之前强制休眠，而非依赖于固定的轮询间隔。 该选择可以仅基于一天中的某个时间，可以在峰值活动期间强制长时间休眠，甚至可以在一天即将结束时（此时为完成白天处理并为夜间运行做准备的关键时刻）将轮询间隔改为 0。 也可以监视捕获进程进度，以确定何时已对午夜提交的所有事务进行扫描并将其存放在更改表中。 这将导致捕获作业结束，该作业可由计划的每日重启来重新启动。 通过将调用 `sp_cdc_scan` 的已交付作业步骤替换为针对 `sp_cdc_scan` 的用户已编写包装的调用，只需少量的额外操作即可获得高度自定义的行为。  

## <a name="cleanup-job"></a><a name="Cleanup"></a> 清除作业

本部分提供有关变更数据捕获清除作业工作方式的信息。  
  
### <a name="structure-of-the-cleanup-job"></a>清除作业的结构

变更数据捕获使用基于保持期的清理策略来管理更改表的大小。 清除机制包含一个在启用第一个数据库表时所创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理 [!INCLUDE[tsql](../../includes/tsql-md.md)] 作业。 单个清除作业可处理所有数据库更改表的清除工作并将相同的保持值应用到所有定义的捕获实例。
  
清除作业可通过运行无参数存储过程 `sp_MScdc_cleanup_job` 来启动。 此存储过程开始时，将从 `msdb.dbo.cdc_jobs` 中提取为清除作业配置的保持期和阈值。 保持值用于计算更改表的新低水印。 从 `cdc.lsn_time_mapping` 表的最大 `tran_end_time` 值中减去指定分钟数即可获得日期时间值形式的新低水印。 然后，使用 CDC.lsn_time_mapping 表将该日期时间值转换为相应的 `lsn` 值。 如果表中多个项共享相同的提交时间，则会选择与具有最小 `lsn` 的项相对应的 `lsn` 作为新低水印。 该 `lsn` 值将传递到 `sp_cdc_cleanup_change_tables` 以从数据库更改表中删除更改表项。  
  
> [!NOTE]  
> 使用最近事务的提交时间作为计算新低水印的基准的优点在于：它可使更改在更改表中保留指定的时间。 即使在捕获进程运行落后时也会出现这种情况。 通过为实际低水印选择具有共享提交时间的最小 `lsn`，与当前低水印具有相同提交时间的所有项会继续保留在更改表中。  

执行清除时，所有捕获实例的低水印会在单个事务中初次更新。 然后，它会尝试从更改表和 cdc.lsn_time_mapping 表中删除过时项。 可配置的阈值用于限制使用任何单个语句中所删除的项数。 对任何单个表执行删除操作失败并不会阻止对其他表尝试执行该操作。  
  
### <a name="cleanup-job-customization"></a>清除作业自定义

 对于清除作业，是否可以进行自定义取决于在确定要放弃哪些更改表项时所采用的策略。 传递的清除作业中唯一支持的策略是基于时间的策略。 在这种情况下，新低水印是通过从处理的最后一个事务的提交时间减去允许的保持期而计算得到的。 因为基础清除过程基于 `lsn` 而不是时间，所以可使用任何数量的策略来确定要保存在更改表中的最小 `lsn`。 只有某些过程是严格基于时间的。 例如，如果需要访问更改表的下游进程无法运行，则可以使用有关客户端的知识来提供故障保护。 此外，尽管默认策略应用相同的 `lsn` 来清除所有数据库的更改表，但还可以调用基础清除过程，以在捕获实例级别上进行清除。  

## <a name="monitor-the-change-data-capture-process"></a><a name="Monitor"></a> 监视变更数据捕获进程

通过监视变更数据捕获进程，可以确定更改是否正以合理的滞后时间正确写入更改表中。 监视还可以帮助您标识可能发生的任何错误。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包括两个动态管理视图，用于帮助你监视变更数据捕获： [sys.dm_cdc_log_scan_sessions](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md) 和 [sys.dm_cdc_errors](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md)。  
  
### <a name="identify-sessions-with-empty-result-sets"></a>标识包含空结果集的会话

sys.dm_cdc_log_scan_sessions 中的每一行表示一个日志扫描会话（ID 为 0 的行除外）。 一个日志扫描会话等效于执行一次 [sp_cdc_scan](../../relational-databases/system-stored-procedures/sys-sp-cdc-scan-transact-sql.md)。 在会话期间，扫描可以返回更改，也可以返回空结果。 如果结果集为空，则 sys.dm_cdc_log_scan_sessions 中的 empty_scan_count 列将设置为 1。 如果有连续的空结果集（例如，当捕获作业正在连续运行时），则最后一个现有行中的 empty_scan_count 将递增。 例如，如果 sys.dm_cdc_log_scan_sessions 已经包含与返回了更改的扫描相对应的 10 行，并且存在五个连续的空结果，则该视图包含 11 行。 最后一行在 empty_scan_count 列的值是 5。 若要确定有空扫描的会话，请运行以下查询：  

```sql
SELECT * from sys.dm_cdc_log_scan_sessions where empty_scan_count <> 0
```

### <a name="determine-latency"></a>确定滞后时间

`sys.dm_cdc_log_scan_sessions` 管理视图包括一个用于记录每个捕获会话滞后时间的列。 滞后时间是指在源表上提交的事务与在更改表上提交的最后一个捕获的事务之间所经过的时间。 只为活动会话填充滞后时间列。 对于其 empty_scan_count 列的值大于 0 的会话，滞后时间列将设置为 0。 以下查询返回最近进行的会话的平均滞后时间：  

```sql
SELECT latency FROM sys.dm_cdc_log_scan_sessions WHERE session_id = 0
```

可以使用滞后时间数据确定捕获进程正在以多快或多慢的速度处理事务。 当捕获进程连续运行时，该数据最有用。 如果捕获进程正在按计划运行，那么，由于在源表上提交的事务与按计划时间运行的捕获进程之间存在滞后，因此滞后时间可能会很长。  
  
 捕获进程效率的另一个重要度量值是吞吐量。 它是在每个会话期间每秒处理的平均命令数。 若要确定会话的吞吐量，请将 command_count 列中的值除以持续时间列中的值。 以下查询返回最近会话的平均吞吐量：  

```sql
SELECT command_count/duration AS [Throughput] FROM sys.dm_cdc_log_scan_sessions WHERE session_id = 0
```

### <a name="use-data-collector-to-collect-sampling-data"></a>使用数据收集器收集抽样数据

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据收集器用于从任何表或动态管理视图中收集数据的快照，并生成性能数据仓库。 对数据库启用变更数据捕获时，最好按固定时间间隔取得 sys.dm_cdc_log_scan_sessions 视图和 sys.dm_cdc_errors 视图的快照，以便随后进行分析。 以下过程设置一个数据收集器，用于从 sys.dm_cdc_log_scan_sessions 管理视图收集示例数据。

#### <a name="configuring-data-collection"></a>配置数据集合

1. 启用数据收集器，并配置管理数据仓库。 有关详细信息，请参阅 [管理数据收集](../../relational-databases/data-collection/manage-data-collection.md)。  
  
2. 执行以下代码，为变更数据捕获创建自定义收集器。  

    ```sql  
    USE msdb;  
  
    DECLARE @schedule_uid uniqueidentifier;  

    -- Collect and upload data every 5 minutes  
    SELECT @schedule_uid = (  
    SELECT schedule_uid from sysschedules_localserver_view
    WHERE name = N'CollectorSchedule_Every_5min')  
  
    DECLARE @collection_set_id int;  
  
    EXEC dbo.sp_syscollector_create_collection_set  
    @name = N' CDC Performance Data Collector',  
    @schedule_uid = @schedule_uid,
    @collection_mode = 0,
    @days_until_expiration = 30,
    @description = N'This collection set collects CDC metadata',  
    @collection_set_id = @collection_set_id output;  
  
    -- Create a collection item using statistics from
    -- the change data capture dynamic management view.  
    DECLARE @parameters xml;  
    DECLARE @collection_item_id int;  
  
    SELECT @parameters = CONVERT(xml,
        N'<TSQLQueryCollector>  
            <Query>  
              <Value>SELECT * FROM sys.dm_cdc_log_scan_sessions</Value>  
              <OutputTable>cdc_log_scan_data</OutputTable>  
            </Query>  
          </TSQLQueryCollector>');  
  
    EXEC dbo.sp_syscollector_create_collection_item  
    @collection_set_id = @collection_set_id,  
    @collector_type_uid = N'302E93D1-3424-4BE7-AA8E-84813ECF2419',  
    @name = ' CDC Performance Data Collector',  
    @frequency = 5,
    @parameters = @parameters,  
    @collection_item_id = @collection_item_id output;
  
    GO  
    ```  
  
3. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，展开 **“管理”**，然后展开 **“数据收集”**。 右键单击“CDC 性能数据收集器”，然后单击“启动数据收集组”。  
  
4. 在步骤 1 配置的数据仓库中，找到表 custom_snapshots.cdc_log_scan_data。 该表提供日志扫描会话中的数据的历史快照。 此数据可以用于分析与时间有关的滞后时间、吞吐量和其他性能度量值。  

## <a name="script-upgrade-mode"></a><a name="ScriptUpgrade"></a> 脚本升级模式

在向实例应用累积更新或服务包时，在重启时，实例可进入脚本升级模式。 在此模式下，SQL Server 可能会运行一个步骤以分析和升级内部 CDC 表，这可能会导致在捕获表上重新创建对象，如索引。 根据涉及的数据量，此步骤可能会需要一些时间，或者导致已启用的 CDC 数据库的高事务日志使用情况。


## <a name="see-also"></a>另请参阅

- [跟踪数据更改 (SQL Server)](../../relational-databases/track-changes/track-data-changes-sql-server.md)
- [关于变更数据捕获 (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)
- [启用和禁用变更数据捕获 (SQL Server)](../../relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server.md)
- [处理变更数据 (SQL Server)](../../relational-databases/track-changes/work-with-change-data-sql-server.md)  
