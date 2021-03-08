---
title: Monitoring Performance By Using the Query Store
description: SQL Server 查询存储功能提供有关查询计划选择和性能的见解。 查询存储捕获查询、计划和运行时统计信息的历史记录。
ms.custom: ''
ms.date: 02/19/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store
- Query Store, described
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: 8d0f6ebf8eb842da0b02521de614ae953408f3b8
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835514"
---
# <a name="monitoring-performance-by-using-the-query-store"></a>使用查询存储监视性能

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询存储功能让你可以探查查询计划选项和性能。 它可帮助你快速找到查询计划更改所造成的性能差异，从而简化了性能疑难解答。 查询存储将自动捕获查询、计划和运行时统计信息的历史记录，并保留它们以供查阅。 它按时间窗口将数据分割开来，使你可以查看数据库使用模式并了解服务器上何时发生了查询计划更改。 可以使用 [ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md) 选项来配置查询存储。

有关在 Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)]中运行查询存储的信息，请参阅[在 Azure SQL 数据库中运行查询存储](best-practice-with-the-query-store.md#Insight)。

> [!IMPORTANT]
> 如果仅对 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 中正在运行的工作负载见解使用查询存储，请尽快安装 [KB 4340759](https://support.microsoft.com/help/4340759) 中的性能可伸缩性修补程序。

## <a name="enabling-the-query-store"></a><a name="Enabling"></a> 启用查询存储

 默认情况下，对于新的 SQL Server 和 Azure Synapse Analytics 数据库不启用查询存储，而为新的 Azure SQL 数据库数据库默认启用查询存储。

### <a name="use-the-query-store-page-in-ssmanstudiofull"></a>在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中使用“查询存储”页

1. 在对象资源管理器中，右键单击数据库，然后单击“属性” 。

   > [!NOTE]
   > 至少需要 16 版本的 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]。

2. 在“数据库属性”  对话框中，选择“查询存储”  页。

3. 在“操作模式(要求)”对话框中，选择“读写”。

### <a name="use-transact-sql-statements"></a>使用 Transact-SQL 语句

使用 ALTER DATABASE 语句启用给定数据库的查询存储。 例如：

```sql
SET QUERY_STORE = ON (OPERATION_MODE = READ_WRITE);
```

有关与查询存储相关的语法选项的详细信息，请参阅 [ALTER DATABASE SET 选项 &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)。

> [!NOTE]
> 无法为 master 或 tempdb 数据库启用查询存储 。

> [!IMPORTANT]
> 有关启用查询存储并使其适用于你的工作负载，请参阅[查询存储最佳做法](../../relational-databases/performance/best-practice-with-the-query-store.md#Configure)。

## <a name="information-in-the-query-store"></a><a name="About"></a>查询存储中的信息

由于统计信息更改、架构更改、索引的创建/删除等多种不同原因，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中任何特定查询的执行计划通常会随着时间而改进。过程缓存（其中存储了缓存的查询计划）仅存储最近的执行计划。 还会由于内存压力从计划缓存中逐出计划。 因此，执行计划更改造成的查询性能回归可能非常重大，且长时间才能解决。

由于查询存储会保留每个查询的多个执行计划，因此它可以强制执行策略，以引导查询处理器对某个查询使用特定执行计划。 这称为“计划强制”。 查询存储中的计划强制是通过使用类似于 [USE PLAN](../../t-sql/queries/hints-transact-sql-query.md) 查询提示的机制来提供的，但它不需要在用户应用程序中进行任何更改。 计划强制可在非常短的时间内解决由计划更改造成的查询性能回归。

> [!NOTE]
> 查询存储收集 DML 语句（如 SELECT、INSERT、UPDATE、DELETE、MERGE 和 BULK INSERT）的计划。
>
> 默认情况下，查询存储不对本机编译的存储过程收集数据。 使用 [sys.sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md) 为本机编译的存储过程启用数据收集。

“等待统计信息”是有助于排除 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 中的性能问题的另一信息来源。 长期以来，等待统计信息仅适用于实例级别，难以回溯到特定查询。 从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 开始，查询存储包括一个跟踪等待统计信息的维度。下面的示例允许查询存储收集等待统计信息。

```sql
SET QUERY_STORE = ON ( WAIT_STATS_CAPTURE_MODE = ON );
```

使用查询存储功能的常见方案为：

- 快速查找并修复通过强制使用先前查询计划而造成的计划性能回归。 修复近期由于执行计划更改而出现性能回归的查询。
- 确定在给定时间窗口中查询执行的次数，从而帮助 DBA 对性能资源问题进行故障排除。
- 标识过去 *x* 小时内的前 *n* 个查询（按执行时间、内存占用等）。
- 审核给定查询的查询计划历史记录。
- 分析特定数据库的资源（CPU、I/O 和内存）使用模式。
- 确定资源上正在等待的前 n 个查询。
- 了解特定查询或计划的等待性质。

查询存储包含三个存储：

- 计划存储：用于保存执行计划信息。
- 运行时统计信息存储：用于保存执行统计信息。
- 等待统计信息存储：用于保存等待统计信息。

**max_plans_per_query** 配置选项限制了计划存储中查询可存储的唯一计划数。 为增强性能，通过异步方式向存储写入信息。 为尽量减少空间使用量，将在按固定时间窗口上聚合运行时统计信息存储中的运行时执行统计信息。 可通过查询查询存储目录视图来查看这些存储中的信息。

以下查询返回查询存储中查询和计划的相关信息。

```sql
SELECT Txt.query_text_id, Txt.query_sql_text, Pl.plan_id, Qry.*
FROM sys.query_store_plan AS Pl
INNER JOIN sys.query_store_query AS Qry
    ON Pl.query_id = Qry.query_id
INNER JOIN sys.query_store_query_text AS Txt
    ON Qry.query_text_id = Txt.query_text_id ;
```

## <a name="use-the-regressed-queries-feature"></a><a name="Regressed"></a> 使用回归查询功能

在启用查询存储后，刷新对象资源管理器窗格的数据库部分，以添加“查询存储”部分。

![SSMS 对象资源管理器中的 SQL Server 2016 查询存储树](../../relational-databases/performance/media/objectexplorerquerystore.PNG "SSMS 对象资源管理器中的 SQL Server 2016 查询存储树") ![SSMS 对象资源管理器中的 SQL Server 2017 查询存储树](../../relational-databases/performance/media/objectexplorerquerystore_sql17.PNG "SSMS 对象资源管理器中的 SQL Server 2017 查询存储树")

选择“回归查询”  ，以在 **中打开“回归查询”** [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]窗格。 “回归查询”窗格将显示查询存储中的查询和计划。 使用顶部的下拉框根据各种条件筛选查询：持续时间（单位 ms，此为默认值）、CPU 时间 (ms)、逻辑读取 (KB)、逻辑写入 (KB)、物理读取 (KB)、CLR 时间 (ms)、DOP、内存使用量 (KB)、行计数、已用日志内存 (KB)、已用临时 DB 内存 (KB) 和等待时间 (ms)。

选择某个计划以查看图形查询计划。 可以使用按钮查看源查询、强制执行和取消强制执行查询计划、在网格和图表格式之间进行切换、比较所选的计划（如果选择多个）及刷新显示。

![SSMS 对象资源管理器中的 SQL Server 2016 回归查询](../../relational-databases/performance/media/objectexplorerregressedqueries.PNG "SSMS 对象资源管理器中的 SQL Server 2016 回归查询")

若要强制执行某一计划，请选择查询和计划，然后单击“强制计划” 。 你只可以强制执行由查询计划功能保存且仍保留在查询计划缓存中的计划。

## <a name="finding-waiting-queries"></a><a name="Waiting"></a> 查找等待查询

从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 开始，查询存储中将提供每个查询随时间变化的等待统计信息。

在查询存储中，等待类型将合并到等待类别中。 [sys.query_store_wait_stats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md#wait-categories-mapping-table).中提供从等待类别到等待类型的映射。

在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18 或更高版本中，选择“查询等待统计信息”以打开“查询等待统计信息”窗格 。 “查询等待统计信息”窗格显示包含查询存储中排在前面的等待类别的条形图。 使用顶部的下拉框选择等待时间的聚合条件：平均值、最大值、最小值、标准偏差和总计（默认）。

![SSMS 对象资源管理器中的 SQL Server 2017 查询等待统计信息](../../relational-databases/performance/media/query-store-waits.PNG "SSMS 对象资源管理器中的 SQL Server 2017 查询等待统计信息")

通过单击条形图和所选等待类别展示的详细信息视图，选择等待类别。 这个新的条形图包含对该等待类别有贡献的查询。

![SSMS 对象资源管理器中的 SQL Server 2017 查询等待统计信息详细信息视图](../../relational-databases/performance/media/query-store-waits-detail.PNG "SSMS 对象资源管理器中的 SQL Server 2017 查询等待统计信息详细信息视图")

使用顶部的下拉框根据各种等待时间条件为所选等待类别筛选查询：平均值、最大值、最小值、标准偏差和总计（默认）。 选择某个计划以查看图形查询计划。 可使用按钮来查看源查询，强制执行和取消强制执行某一查询计划，以及刷新显示内容。

等待类别可将不同等待类型按性质合并为类似的桶。 不同的等待类别需要不同的后续分析才能解决此问题，但相同类别的等待类型可引起非常相似的故障排除体验，并假定基于等待的受影响的查询会成为用于成功完成此类调查的大部分内容的缺少的部分。

下面的示例介绍如何在查询存储中引入等待类别前后更深入了解工作负荷：

|曾经的体验|新的体验|操作|
|-|-|-|
|每个数据库的高 RESOURCE_SEMAPHORE 等待|特定查询在查询存储中的高内存等待|在查询存储中查找消耗内存最多的查询。 这些查询可能会延迟受影响查询的进度。 请考虑对这些查询或受影响的查询使用 MAX_GRANT_PERCENT 查询提示。|
|每个数据库的高 LCK_M_X 等待|特定查询在查询存储中的高锁定等待|检查受影响查询的查询文本，并确定目标实体。 在查询存储中查找修改同一实体的其他查询，该实体频繁执行和/或具有较高持续时间。 确定这些查询后，请考虑更改应用程序逻辑以提高并发性，或使用限制较少的隔离级别。|
|每个数据库的高 PAGEIOLATCH_SH 等待|特定查询在查询存储中的高缓冲 IO 等待|在查询存储中查找具有大量物理读取的查询。 如果它们与含较高 IO 等待的查询匹配，执行执行搜索而不是扫描时，请考虑引入关于基础实体的索引，以便减少查询的 IO 开销。|
|每个数据库的高 SOS_SCHEDULER_YIELD 等待|特定查询在查询存储中的高 CPU 等待|查找查询存储中前几个使用 CPU 最多的查询。 其中，请确定其高 CPU 趋势与受影响查询的高 CPU 等待关联的查询。 重点优化这些查询 - 可能存在计划回归，或缺失的索引。|

## <a name="configuration-options"></a><a name="Options"></a> 配置选项

有关配置查询存储参数的可用选项，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md#query-store)。

查询 sys.database_query_store_options 视图以确定查询存储的当前选项。 有关值的详细信息，请参阅 [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)。

有关使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句来设置配置选项的示例，请参阅[选项管理](#OptionMgmt)。

## <a name="related-views-functions-and-procedures"></a><a name="Related"></a> 相关视图、功能和过程

通过 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 或使用以下视图和过程来查看和管理查询存储。

### <a name="query-store-functions"></a>查询存储函数

此函数有助于执行查询存储操作。

:::row:::
    :::column:::
        [sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL)](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="query-store-catalog-views"></a>查询存储目录视图

目录视图提供了查询存储的相关信息。

:::row:::
    :::column:::
        [sys.database_query_store_options (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.query_context_settings (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.query_store_plan (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.query_store_query (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.query_store_query_text (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.query_store_runtime_stats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.query_store_wait_stats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.query_store_runtime_stats_interval (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)
    :::column-end:::
:::row-end:::


### <a name="query-store-stored-procedures"></a>查询存储存储过程

存储过程配置了查询存储。

:::row:::
    :::column:::
        [sp_query_store_flush_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-query-store-flush-db-transact-sql.md)
    :::column-end:::
    :::column:::
        [sp_query_store_reset_exec_stats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sp_query_store_force_plan (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)
    :::column-end:::
    :::column:::
        [sp_query_store_unforce_plan (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sp_query_store_remove_plan (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md)
    :::column-end:::
    :::column:::
        [sp_query_store_remove_query (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        sp_query_store_consistency_check (Transact-SQL)<sup>1</sup>
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

<sup>1</sup> 在极端情况下，查询存储可能由于内部错误而进入 ERROR 状态。 从 SQL Server 2017 (14.x) 开始，如果出现这种情况，可通过在受影响的数据库内执行 sp_query_store_consistency_check 存储过程来恢复查询存储。 有关 actual_state_desc 列说明中所述的更多详细信息，请参阅 [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)。

## <a name="key-usage-scenarios"></a><a name="Scenarios"></a>键使用方案

### <a name="option-management"></a><a name="OptionMgmt"></a> 选项管理

本部分提供一些有关如何管理查询存储功能本身的准则。

**查询存储当前是否处于活动状态？**

查询存储将其数据存储在用户数据库内，正因为此，它具有大小限制（使用 `MAX_STORAGE_SIZE_MB` 进行配置）。 如果查询存储中的数据命中该限制，则查询存储将自动从读写状态更改为只读状态，并停止收集新数据。

查询 [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md) ，以确定当前查询存储是否可用，以及当前是否在收集运行时状态。

```sql
SELECT actual_state, actual_state_desc, readonly_reason,
    current_storage_size_mb, max_storage_size_mb
FROM sys.database_query_store_options;
```

actual_state 列决定查询存储状态。 如果不处于所需状态，请查看 `readonly_reason` 列，了解详细信息。
查询存储大小超过配额时，该功能将切换到 read_only 模式。

**获取查询存储选项**

若要了解查询存储状态的相关详细信息，请在用户数据库中执行以下操作。

```sql
SELECT * FROM sys.database_query_store_options;
```

**设置查询存储间隔**

你可以覆盖用于聚合查询运行时统计信息的时间间隔（默认值为 60 分钟）。

```sql
ALTER DATABASE <database_name>
SET QUERY_STORE (INTERVAL_LENGTH_MINUTES = 15);
```

> [!NOTE]
> `INTERVAL_LENGTH_MINUTES` 不允许使用任意值。 使用下列其中一个：1、5、10、15、30、60 或 1440 分钟。

间隔的新值通过 **sys.database_query_store_options** 视图公开。

**查询存储空间使用情况**

若要检查当前的查询存储大小和限制，请在用户数据库中执行以下语句。

```sql
SELECT current_storage_size_mb, max_storage_size_mb
FROM sys.database_query_store_options;
```

如果查询存储已满，请使用以下语句来扩展存储。

```sql
ALTER DATABASE <database_name>
SET QUERY_STORE (MAX_STORAGE_SIZE_MB = <new_size>);
```

**获取查询存储选项**

你可以使用单个 ALTER DATABASE 语句同时设置多个查询存储选项。

```sql
ALTER DATABASE <database name>
SET QUERY_STORE (
    OPERATION_MODE = READ_WRITE,
    CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30),
    DATA_FLUSH_INTERVAL_SECONDS = 3000,
    MAX_STORAGE_SIZE_MB = 500,
    INTERVAL_LENGTH_MINUTES = 15,
    SIZE_BASED_CLEANUP_MODE = AUTO,
    QUERY_CAPTURE_MODE = AUTO,
    MAX_PLANS_PER_QUERY = 1000,
    WAIT_STATS_CAPTURE_MODE = ON
);
```

有关配置选项的完整列表，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。

**清理空间**

查询存储时间间隔表是在数据库创建期间在 PRIMARY 文件组中创建的，且之后不可更改此配置。 如果空间不足，你可以要使用以下语句来清理旧的查询存储数据。

```sql
ALTER DATABASE <db_name> SET QUERY_STORE CLEAR;
```

或者，你可以只清理临时查询数据，因为此数据与查询优化和计划分析的相关性更低，但却占用了大量空间。

**删除即席查询**

这将从查询存储中清除即席查询和内部查询，以便查询存储不会耗尽空间并删除我们真正需要跟踪的查询。

```sql
SET NOCOUNT ON
-- This purges adhoc and internal queries from 
-- the Query Store in the current database 
-- so that the Query Store does not run out of space 
-- and remove queries we really need to track

DECLARE @id int;
DECLARE adhoc_queries_cursor CURSOR
FOR
    SELECT q.query_id
    FROM sys.query_store_query_text AS qt
    JOIN sys.query_store_query AS q
    ON q.query_text_id = qt.query_text_id
    JOIN sys.query_store_plan AS p
    ON p.query_id = q.query_id
    JOIN sys.query_store_runtime_stats AS rs
    ON rs.plan_id = p.plan_id
    WHERE q.is_internal_query = 1  -- is it an internal query then we dont care to keep track of it
       OR q.object_id = 0 -- if it does not have a valid object_id then it is an adhoc query and we don't care about keeping track of it
    GROUP BY q.query_id
    HAVING MAX(rs.last_execution_time) < DATEADD (minute, -5, GETUTCDATE())  -- if it has been more than 5 minutes since the adhoc query ran
    ORDER BY q.query_id;
OPEN adhoc_queries_cursor ;
FETCH NEXT FROM adhoc_queries_cursor INTO @id;
WHILE @@fetch_status = 0
BEGIN
    PRINT 'EXEC sp_query_store_remove_query ' + str(@id);
    EXEC sp_query_store_remove_query @id;
    FETCH NEXT FROM adhoc_queries_cursor INTO @id;
END
CLOSE adhoc_queries_cursor;
DEALLOCATE adhoc_queries_cursor;

```

你可以使用其他逻辑来定义自己的过程，以清理不再需要的数据。

以上示例使用 **sp_query_store_remove_query** 扩展存储过程来删除不必要的数据。 也可使用以下命令：

- sp_query_store_reset_exec_stats 用于清除给定计划的运行时统计信息。
- sp_query_store_remove_plan 用于删除单个计划。

### <a name="performance-auditing-and-troubleshooting"></a><a name="Peformance"></a> 性能审核和疑难解答

查询存储将保存整个查询过程中的编译历史记录和运行时度量，使你能询问有关工作负载的问题。

在数据库上执行的最后 n 个查询？

```sql
SELECT TOP 10 qt.query_sql_text, q.query_id,
    qt.query_text_id, p.plan_id, rs.last_execution_time
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats AS rs
    ON p.plan_id = rs.plan_id
ORDER BY rs.last_execution_time DESC;
```

**每个查询的执行数量？**

```sql
SELECT q.query_id, qt.query_text_id, qt.query_sql_text,
    SUM(rs.count_executions) AS total_execution_count
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats AS rs
    ON p.plan_id = rs.plan_id
GROUP BY q.query_id, qt.query_text_id, qt.query_sql_text
ORDER BY total_execution_count DESC;
```

**过去一小时内具有最长平均执行时间的查询数量？**

```sql
SELECT TOP 10 rs.avg_duration, qt.query_sql_text, q.query_id,
    qt.query_text_id, p.plan_id, GETUTCDATE() AS CurrentUTCTime,
    rs.last_execution_time
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats AS rs
    ON p.plan_id = rs.plan_id
WHERE rs.last_execution_time > DATEADD(hour, -1, GETUTCDATE())
ORDER BY rs.avg_duration DESC;
```

**在相应的平均行计数和执行计数下，过去 24 小时内具有最大平均物理 I/O 读取数的查询数量？**

```sql
SELECT TOP 10 rs.avg_physical_io_reads, qt.query_sql_text,
    q.query_id, qt.query_text_id, p.plan_id, rs.runtime_stats_id,
    rsi.start_time, rsi.end_time, rs.avg_rowcount, rs.count_executions
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats AS rs
    ON p.plan_id = rs.plan_id
JOIN sys.query_store_runtime_stats_interval AS rsi
    ON rsi.runtime_stats_interval_id = rs.runtime_stats_interval_id
WHERE rsi.start_time >= DATEADD(hour, -24, GETUTCDATE())
ORDER BY rs.avg_physical_io_reads DESC;
```

**具有多个计划的查询？** 这些查询特别有趣，因为计划选择更改可能造成它们的性能回归。 以下查询将这些查询和所有计划一同进行了标识：

```sql
WITH Query_MultPlans
AS
(
SELECT COUNT(*) AS cnt, q.query_id
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON p.query_id = q.query_id
GROUP BY q.query_id
HAVING COUNT(distinct plan_id) > 1
)

SELECT q.query_id, object_name(object_id) AS ContainingObject,
    query_sql_text, plan_id, p.query_plan AS plan_xml,
    p.last_compile_start_time, p.last_execution_time
FROM Query_MultPlans AS qm
JOIN sys.query_store_query AS q
    ON qm.query_id = q.query_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_query_text qt
    ON qt.query_text_id = q.query_text_id
ORDER BY query_id, plan_id;
```

 **最近性能回归的查询（对比不同时间点）？** 以下查询示例返回了其执行时间因计划选择更改而在过去 48 小时内翻倍的所有查询。 并排查询所有的运行时统计信息时间间隔。

```sql
SELECT
    qt.query_sql_text,
    q.query_id,
    qt.query_text_id,
    rs1.runtime_stats_id AS runtime_stats_id_1,
    rsi1.start_time AS interval_1,
    p1.plan_id AS plan_1,
    rs1.avg_duration AS avg_duration_1,
    rs2.avg_duration AS avg_duration_2,
    p2.plan_id AS plan_2,
    rsi2.start_time AS interval_2,
    rs2.runtime_stats_id AS runtime_stats_id_2
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p1
    ON q.query_id = p1.query_id
JOIN sys.query_store_runtime_stats AS rs1
    ON p1.plan_id = rs1.plan_id
JOIN sys.query_store_runtime_stats_interval AS rsi1
    ON rsi1.runtime_stats_interval_id = rs1.runtime_stats_interval_id
JOIN sys.query_store_plan AS p2
    ON q.query_id = p2.query_id
JOIN sys.query_store_runtime_stats AS rs2
    ON p2.plan_id = rs2.plan_id
JOIN sys.query_store_runtime_stats_interval AS rsi2
    ON rsi2.runtime_stats_interval_id = rs2.runtime_stats_interval_id
WHERE rsi1.start_time > DATEADD(hour, -48, GETUTCDATE())
    AND rsi2.start_time > rsi1.start_time
    AND p1.plan_id <> p2.plan_id
    AND rs2.avg_duration > 2*rs1.avg_duration
ORDER BY q.query_id, rsi1.start_time, rsi2.start_time;
```

如果你向查看所有性能回归（而不仅是计划选择更改造成的回归），只需从先前查询中删除条件 `AND p1.plan_id <> p2.plan_id` 。

**等待时间最长的查询？**
此查询将返回等待最多的前 10 个查询。

```sql
SELECT TOP 10
    qt.query_text_id,
    q.query_id,
    p.plan_id,
    sum(total_query_wait_time_ms) AS sum_total_wait_ms
FROM sys.query_store_wait_stats ws
JOIN sys.query_store_plan p ON ws.plan_id = p.plan_id
JOIN sys.query_store_query q ON p.query_id = q.query_id
JOIN sys.query_store_query_text qt ON q.query_text_id = qt.query_text_id
GROUP BY qt.query_text_id, q.query_id, p.plan_id
ORDER BY sum_total_wait_ms DESC
```

**最近性能回归的查询（对比近期执行和历史执行）？** 下一查询会根据执行时间段来比较查询执行。 在此特定示例中，查询对比了最近时期（1 小时）和历史时期（过去一天）中的执行，并标识了引入 `additional_duration_workload`的查询。 此度量的计算方式是最近平均执行和历史平均执行之差，再乘以最近执行数量。 它实际上表示相对于历史记录，引入了多少额外的持续时间最近执行：

```sql
--- "Recent" workload - last 1 hour
DECLARE @recent_start_time datetimeoffset;
DECLARE @recent_end_time datetimeoffset;
SET @recent_start_time = DATEADD(hour, -1, SYSUTCDATETIME());
SET @recent_end_time = SYSUTCDATETIME();

--- "History" workload
DECLARE @history_start_time datetimeoffset;
DECLARE @history_end_time datetimeoffset;
SET @history_start_time = DATEADD(hour, -24, SYSUTCDATETIME());
SET @history_end_time = SYSUTCDATETIME();

WITH
hist AS
(
    SELECT
        p.query_id query_id,
        ROUND(ROUND(CONVERT(FLOAT, SUM(rs.avg_duration * rs.count_executions)) * 0.001, 2), 2) AS total_duration,
        SUM(rs.count_executions) AS count_executions,
        COUNT(distinct p.plan_id) AS num_plans
     FROM sys.query_store_runtime_stats AS rs
        JOIN sys.query_store_plan AS p ON p.plan_id = rs.plan_id
    WHERE (rs.first_execution_time >= @history_start_time
               AND rs.last_execution_time < @history_end_time)
        OR (rs.first_execution_time <= @history_start_time
               AND rs.last_execution_time > @history_start_time)
        OR (rs.first_execution_time <= @history_end_time
               AND rs.last_execution_time > @history_end_time)
    GROUP BY p.query_id
),
recent AS
(
    SELECT
        p.query_id query_id,
        ROUND(ROUND(CONVERT(FLOAT, SUM(rs.avg_duration * rs.count_executions)) * 0.001, 2), 2) AS total_duration,
        SUM(rs.count_executions) AS count_executions,
        COUNT(distinct p.plan_id) AS num_plans
    FROM sys.query_store_runtime_stats AS rs
        JOIN sys.query_store_plan AS p ON p.plan_id = rs.plan_id
    WHERE  (rs.first_execution_time >= @recent_start_time
               AND rs.last_execution_time < @recent_end_time)
        OR (rs.first_execution_time <= @recent_start_time
               AND rs.last_execution_time > @recent_start_time)
        OR (rs.first_execution_time <= @recent_end_time
               AND rs.last_execution_time > @recent_end_time)
    GROUP BY p.query_id
)
SELECT
    results.query_id AS query_id,
    results.query_text AS query_text,
    results.additional_duration_workload AS additional_duration_workload,
    results.total_duration_recent AS total_duration_recent,
    results.total_duration_hist AS total_duration_hist,
    ISNULL(results.count_executions_recent, 0) AS count_executions_recent,
    ISNULL(results.count_executions_hist, 0) AS count_executions_hist
FROM
(
    SELECT
        hist.query_id AS query_id,
        qt.query_sql_text AS query_text,
        ROUND(CONVERT(float, recent.total_duration/
                   recent.count_executions-hist.total_duration/hist.count_executions)
               *(recent.count_executions), 2) AS additional_duration_workload,
        ROUND(recent.total_duration, 2) AS total_duration_recent,
        ROUND(hist.total_duration, 2) AS total_duration_hist,
        recent.count_executions AS count_executions_recent,
        hist.count_executions AS count_executions_hist
    FROM hist
        JOIN recent
            ON hist.query_id = recent.query_id
        JOIN sys.query_store_query AS q
            ON q.query_id = hist.query_id
        JOIN sys.query_store_query_text AS qt
            ON q.query_text_id = qt.query_text_id
) AS results
WHERE additional_duration_workload > 0
ORDER BY additional_duration_workload DESC
OPTION (MERGE JOIN);
```

### <a name="maintaining-query-performance-stability"></a><a name="Stability"></a> 维护查询性能稳定性

对于执行多次的查询，你可能注意到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用了会导致不同资源利用率和持续时间的不同计划。 借助查询存储，可以检测到查询性能何时回归，并确定在感兴趣的时间段内的最优计划。 然后你可以对未来的查询执行强制执行此最优计划。

你还可以使用参数（自动参数化或手动参数化）来标识某一查询内不一致的查询性能。 你可以在不同计划中标识出对所有或大多数参数值而言足够快和最佳的计划，并强制执行此计划，为更大范围的用户方案保留可预测的性能。

### <a name="force-a-plan-for-a-query-apply-forcing-policy"></a>强制执行查询计划（应用强制策略）

当强制执行某一查询的计划时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 尝试在优化器中强制执行该计划。 如果计划强制实施失败，将触发 XEvent，并指示优化器正常优化。

```sql
EXEC sp_query_store_force_plan @query_id = 48, @plan_id = 49;
```

使用 **sp_query_store_force_plan** 时，你只可以强制执行查询存储记录为该查询计划的那些计划。 换句话说，可用于查询的计划只有那些在查询存储处于活动状态时已用于执行该查询的计划。

#### <a name="a-namectp23-plan-forcing-support-for-fast-forward-and-static-cursors"></a><a name="ctp23"><a/> 计划强制支持快进和静态游标

从 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 和 Azure SQL 数据库（所有部署模型）开始，查询数据存储支持为快进和静态 [!INCLUDE[tsql](../../includes/tsql-md.md)] 及 API 游标强制执行查询执行计划。 支持通过 `sp_query_store_force_plan` 或通过 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 查询存储报表进行强制执行。

### <a name="remove-plan-forcing-for-a-query"></a>删除为查询强制执行的计划

若要再次依靠 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器来计算最佳查询计划，请使用 **sp_query_store_unforce_plan** 来取消强制执行为查询选定的计划。

```sql
EXEC sp_query_store_unforce_plan @query_id = 48, @plan_id = 49;
```

## <a name="see-also"></a>另请参阅

- [Query Store 最佳实践](../../relational-databases/performance/best-practice-with-the-query-store.md)
- [通过内存中 OLTP 使用查询存储](../../relational-databases/performance/using-the-query-store-with-in-memory-oltp.md)
- [Query Store 使用方案](../../relational-databases/performance/query-store-usage-scenarios.md)
- [查询存储的数据收集方式](../../relational-databases/performance/how-query-store-collects-data.md)
- [查询存储存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)
- [查询存储目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)
- [监视和优化性能](../../relational-databases/performance/monitor-and-tune-for-performance.md)
- [性能监视和优化工具](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)
- [打开活动监视器 (SQL Server Management Studio)](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)
- [实时查询统计信息](../../relational-databases/performance/live-query-statistics.md)
- [活动监视器](../../relational-databases/performance-monitor/activity-monitor.md)
- [sys.database_query_store_options (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)
