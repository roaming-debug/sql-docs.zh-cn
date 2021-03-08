---
title: 查询存储最佳做法 | Microsoft Docs
description: 了解将 SQL Server 查询存储与工作负载结合使用的最佳做法，例如使用最新的 SQL Server Management Studio 和 Query Performance Insight。
ms.custom: ''
ms.date: 1/7/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store, best practices
ms.assetid: 5b13b5ac-1e4c-45e7-bda7-ebebe2784551
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 85c9a992660b63f70d2193acd114e7b07d3c1092
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186490"
---
# <a name="best-practices-with-query-store"></a>查询存储最佳做法

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

本文概述使用 SQL Server 查询存储处理工作负载的最佳做法。

## <a name="use-the-latest-ssmanstudiofull"></a><a name="SSMS"></a>使用最新 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 提供了一组用户界面，旨在配置查询存储和使用收集的工作负载数据。 单击[此处](../../ssms/download-sql-server-management-studio-ssms.md)下载最新版 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]。

有关如何使用查询存储进行故障排除的简要说明，请参阅[查询存储 @Azure 博客](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)。

## <a name="use-query-performance-insight-in-azure-sql-database"></a><a name="Insight"></a>在 Azure SQL 数据库中使用 Query Performance Insight

如果在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]中运行查询存储，则可以使用 [Query Performance Insight](/azure/sql-database/sql-database-query-performance) 来分析一定时段内的资源消耗情况。 虽然可以使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 和 [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) 来获取所有查询的详细资源消耗情况（例如 CPU、内存和 I/O），但使用 Query Performance Insight 可以快速且有效地确定查询对数据库总体 DTU 消耗情况的影响。 有关详细信息，请参阅 [Azure SQL Database Query Performance Insight](/azure/azure-sql/database/query-performance-insight-use)（Azure SQL 数据库的 Query Performance Insight）。

本部分介绍最佳的配置默认值，这些默认值旨在确保 Query Store 以及依赖功能能够可靠运行。 默认配置已针对持续数据收集操作进行优化，即，在 OFF/READ_ONLY 状态下花费最少的时间。 有关所有可用的查询存储选项的详细信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md#query-store)。

| 配置 | 说明 | 默认 | 注释 |
| --- | --- | --- | --- |
| MAX_STORAGE_SIZE_MB |指定 Query Store 在客户数据库中占用的数据空间的限制 |100 |对新数据库强制实施 |
| INTERVAL_LENGTH_MINUTES |定义聚合和持久化查询计划收集运行时统计信息的时段大小。 每个活动查询计划将为此配置定义的时间段包含最多一行 |60 |对新数据库强制实施 |
| STALE_QUERY_THRESHOLD_DAYS |基于时间的清理策略，控制持久化运行时统计信息和非活动查询的保留期 |30 |对新数据库和使用以前的默认值 (367) 的数据库强制实施 |
| SIZE_BASED_CLEANUP_MODE |指定当 Query Store 数据大小接近限制时是否自动清理数据 |AUTO |对所有数据库强制实施 |
| QUERY_CAPTURE_MODE |指定是要跟踪所有查询，还是只跟踪一部分查询 |AUTO |对所有数据库强制实施 |
| FLUSH_INTERVAL_SECONDS |指定捕获的运行时统计信息在刷新到磁盘之前，保留在内存中的最大期限 |900 |对新数据库强制实施 |
| | | | |

> [!IMPORTANT]
> 在查询存储的最终激活阶段，系统会在所有 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]中自动应用这些默认值。 启用后，[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]不会更改客户设置的配置值，除非这些值对主要工作负载或查询存储的可靠运行造成负面影响。

> [!NOTE]  
> 无法在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]的单一数据库和弹性池中禁用查询存储。 执行 `ALTER DATABASE [database] SET QUERY_STORE = OFF` 将返回警告 `'QUERY_STORE=OFF' is not supported in this version of SQL Server.`。 

如果想要保持使用自定义设置，请[结合 Query Store 选项使用 ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md#query-store)，将配置还原到以前的状态。 请查看[查询存储最佳做法](../../relational-databases/performance/best-practice-with-the-query-store.md)，了解如何选择最佳的配置参数。

## <a name="use-query-store-with-elastic-pool-databases"></a>将查询存储与弹性池数据库配合使用

可以毫无顾忌地在所有数据库中使用 Query Store，甚至是在密集打包的池中。 与资源过度使用（为弹性池中的大量数据库启用了查询存储时可能会遇到这种情况）相关的所有问题都已得到了解决。

## <a name="keep-query-store-adjusted-to-your-workload"></a><a name="Configure"></a> 始终根据工作负载调整查询存储

根据工作负荷和性能故障排除要求来配置 Query Store。
默认参数是启动的理想参数，但应监视查询存储在一定时段内的行为表现，并对其配置进行相应的调整。

 ![查询存储属性](../../relational-databases/performance/media/query-store-properties.png "query-store-properties")

 下面是设置参数值时应遵循的准则：

**最大大小 (MB)** ：为查询存储在数据库中占用的数据空间指定一个限制。 这是最重要的设置，直接影响查询存储的操作模式。

当查询存储收集查询、执行计划和统计信息时，其在数据库中的大小会一直增长，直至达到此限制。 达到此限制后，Query Store 会自动将操作模式更改为只读，并停止收集新数据，这意味着你的性能分析自此不再精确。

[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 中的默认值为 100 MB。 如果工作负载会生成大量不同的查询和计划，或者想要让查询历史记录保存较长的时间，则此大小可能不足。 从 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 开始，默认值是 1 GB。 请跟踪当前的空间使用情况，增大“最大大小 (MB)”的值以防查询存储转换到只读模式。

> [!IMPORTANT]
> 没有严格执行“最大大小 (MB)”限制。 仅当查询存储将数据写入磁盘时才检查存储大小。 此间隔由“数据刷新间隔（分钟）”选项设置。 如果查询存储已违反存储大小检查之间的最大大小限制，则它将转换为只读模式。 如果启用了“基于大小的清理模式”，则也会触发强制实施最大大小限制的清理机制。

使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 或执行以下脚本，以便获取有关 Query Store 大小的最新信息：

```sql
USE [QueryStoreDB];
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
 max_storage_size_mb, readonly_reason
FROM sys.database_query_store_options;
```

以下脚本将设置新的“最大大小 (MB)”值：

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);
```

 **数据刷新间隔（分钟）** ：它定义将收集的运行时统计信息保存到磁盘的频率。 它在图形用户界面 (GUI) 中以分钟为单位，但在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中以秒为单位。 默认值为 900 秒，在图形用户界面中为 15 分钟。 如果工作负载不生成大量不同的查询和计划或者你能够接受在数据库关闭之前花更长的时间来保留数据，可考虑使用更大的值。

> [!NOTE]
> 如果出现故障转移或关闭命令，使用跟踪标志 7745 会阻止查询存储数据写入磁盘。 有关更多信息，请参阅[在任务关键型服务器上使用跟踪标志](#Recovery)部分。

使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 为“数据刷新间隔”设置不同的值：

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (DATA_FLUSH_INTERVAL_SECONDS = 900);
```

**统计信息收集间隔**：定义已收集的运行时统计信息的粒度级别（以分钟为单位）。 默认值为 60 分钟。 如果需要更细的粒度或更短的时间来检测和缓解问题，可考虑使用较小的值。 请记住，该值会直接影响查询存储数据的大小。 使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 为“统计信息收集间隔”设置不同的值：

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (INTERVAL_LENGTH_MINUTES = 60);
```

**过时查询阈值（天）** ：基于时间的清除策略，用于控制持久化运行时统计信息和非活动查询的保持期（以天为单位）。 查询存储默认配置为将数据保留 30 天，这对于你的方案来说可能过长。

避免保留你并不打算使用的历史数据。 这样可以减少变为只读状态的次数。 查询存储数据的大小以及检测和解决问题的时间将会变得更可预测。 使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 或以下脚本配置基于时间的清理策略：

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 90));
```

**基于大小的清除模式**：指定在查询存储数据大小达到限制时，是否启用自动数据清理功能。 请激活基于大小的清理功能，确保查询存储始终以读写模式运行并收集最新数据。  请注意，在繁重的工作负载下，不能保证查询存储清理会始终将数据大小保持在限制范围内。 自动数据清除可能会落后并切换（暂时）到只读模式。

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
```

**查询存储捕获模式**：指定查询存储的查询捕获策略。

- **全部**：捕获所有查询。 此选项在 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 中为默认值。
- **AUTO**：忽略不太频繁的查询以及编译和执行持续时间不长的查询。 执行计数、编译和运行时持续时间的阈值由内部决定。 从 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 开始，这是默认选项。
- **无**：查询存储停止捕获新查询。
- **自定义**：支持额外控件和微调数据收集策略功能。 新的自定义设置定义在内部捕获策略时间阈值期间执行的操作。 这是评估配置条件的时间边界，如果所有值为 true，则查询存储可以捕获查询。

> [!IMPORTANT]
> 当查询存储捕获模式设置为“全部”、“自动”或“自定义”时，始终捕获游标、存储过程中的查询和本机编译的查询  。 若要捕获本机编译的查询，请使用 [sys.sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md) 启用每个查询统计信息的收集。

 以下脚本将 QUERY_CAPTURE_MODE 设置为 AUTO：

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);
```

### <a name="examples"></a>示例

以下示例将 QUERY_CAPTURE_MODE 设置为 AUTO，并在 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 中设置其他建议选项：

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1000,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

以下示例将 QUERY_CAPTURE_MODE 设置为 AUTO，并在 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 中设置其他建议选项以包括等待统计信息：

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1000,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON
    );
```

下面的示例将 QUERY_CAPTURE_MODE 设置为 AUTO，并在 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 中设置其他建议选项，同时视需要使用默认值设置 CUSTOM 捕获策略，而不是使用新的默认 AUTO 捕获模式：

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1000,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="start-with-query-performance-troubleshooting"></a>开始进行查询性能故障排除

查询存储工作流的故障排除很简单，如下图所示：

![查询存储排除故障](../../relational-databases/performance/media/query-store-troubleshooting.png "query-store-troubleshooting")

按上一节的说明通过 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 来启用查询存储，或者执行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：

```sql
ALTER DATABASE [DatabaseOne] SET QUERY_STORE = ON;
```

查询存储收集能够准确代表工作负载的数据集需要一定的时间。 通常情况下，即使是很复杂的工作负荷，一天的时间也足够了。 但是，在启用此功能后，就可以立即开始浏览数据并确定需要注意的查询。 前往 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 的对象资源管理器中数据库节点下的查询存储子文件夹，然后打开特定方案的故障排除视图。

[!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 查询存储视图在操作时使用一组执行度量值，每个度量值都表示为下述任意统计函数：

|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本|执行度量值|统计函数|
|----------------------|----------------------|------------------------|
|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|CPU 时间、持续时间、执行计数、逻辑读取次数、逻辑写入次数、内存消耗、物理读取次数、CLR 时间、并行度 (DOP) 和行计数|平均值、最大值、最小值、标准偏差、总数|
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|CPU 时间、持续时间、执行计数、逻辑读取次数、逻辑写入次数、内存消耗、物理读取次数、CLR 时间、并行度、行计数、日志内存、TempDB 内存和等待时间|平均值、最大值、最小值、标准偏差、总数|

下图显示了如何查找 Query Store 视图：

![查询存储视图](../../relational-databases/performance/media/objectexplorerquerystore_sql17.png "查询存储视图")

下表说明了何时使用每个 Query Store 视图：

|SQL Server Management Studio|场景|
|---------------|--------------|
|**回归查询**|查明哪些查询的执行度量值最近进行了回归（例如，变得更糟）。 <br />使用此视图将应用程序中观察到的性能问题与需要进行修复或改进的实际查询关联起来。|
|**总体资源消耗**|针对任意执行度量值分析数据库的总资源消耗量。<br />使用此视图可以确定资源模式（白天工作负荷与夜间工作负荷的比较），并优化数据库的总体消耗。|
|**资源使用排名靠前的查询**|选择所关注的执行度量值，确定在指定的时间间隔内具有最极端值的查询。 <br />此视图可以帮助你关注最相关的查询，这些查询对数据库资源消耗的影响最大。|
|**具有强制计划的查询**|使用查询存储列出以前的强制计划。 <br />使用此视图快速访问当前的所有强制计划。|
|**变化程度高的查询**|分析执行变化程度较高的查询，此类变化可涉及任何可用的维度，例如所需时间间隔内的持续时间、CPU 时间、IO 和内存使用情况。<br />使用此视图可以标识性能有很大差异且可能会影响用户跨应用程序体验的查询。|
|**查询等待统计信息**|分析数据库中最活跃的等待类别和对所选等待类别贡献最大的查询。<br />使用此视图分析等待统计信息并识别可能在应用程序中影响用户体验的查询。<br /><br />适用对象：从 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18.0 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始。|
|**跟踪的查询**|实时跟踪最重要查询的执行情况。 通常情况下，使用此视图是因为你计划强制执行相关查询，因此需确保查询性能的稳定性。|

> [!TIP]
> 如需详细了解如何使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 来确定资源使用排名靠前的查询并修复那些因计划选择变化而导致回归的查询，请参阅 [@Azure 博客中的查询存储](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)。

如果确定某个查询的性能不够理想，则可根据问题性质进行操作。

- 如果所执行的查询具有多个计划，而最后一个计划明显不如前面的计划，则可通过计划强制机制来强制使用最佳计划。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 尝试强制实施优化器中的计划。 如果计划强制实施失败，将触发 XEvent，并指示优化器正常优化。

  ![查询存储强制计划](../../relational-databases/performance/media/query-store-force-plan.png "query-store-force-plan")

  > [!NOTE]
  > 前面的图形针对特定的查询计划会显示不同的形状，以下是可能出现的每种形状的对应含义：<br />
  >
  > |形状|含义|
  > |-------------------|-------------|
  > |圆形|查询已完成，这意味着常规执行成功完成。|
  > |Square|已取消，这意味着客户端发起的执行中止。|
  > |Triangle|失败，这意味着异常执行中止。|
  >
  > 此外，形状大小反映指定时间间隔内的查询执行计数。 如果执行次数较多，该形状会变大。

- 你可以认为，你的查询因为缺少索引而无法达到最佳执行效果。 此信息显示在查询执行计划中。 使用查询存储创建缺失的索引并检查查询性能。

   ![查询存储显示计划](../../relational-databases/performance/media/query-store-show-plan.png "query-store-show-plan")

如果你在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]上运行工作负荷，可注册获取 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 索引顾问，然后即可自动接收索引建议。

- 在某些情况下，如果你看到执行计划中估计的行数和实际的行数存在显著差异，则可强制执行统计信息的重新编译。
- 重写有问题的查询，例如可以充分利用查询参数化或实现更优化的逻辑。

## <a name="verify-that-query-store-collects-query-data-continuously"></a><a name="Verify"></a>确保查询存储持续收集查询数据

查询存储可在无提示的情况下更改操作模式。 请定期监视查询存储的状态以确保查询存储正常运行，并采取相应措施，避免发生不必要的故障。 执行以下查询，以便确定操作模式并查看最相关的参数：

```sql
USE [QueryStoreDB];
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
    max_storage_size_mb, readonly_reason, interval_length_minutes,
    stale_query_threshold_days, size_based_cleanup_mode_desc,
    query_capture_mode_desc
FROM sys.database_query_store_options;
```

`actual_state_desc` 和 `desired_state_desc` 之间存在差异，这表明自动更改了操作模式。 最常见的更改是查询存储在无提示的情况下切换到只读模式。 在极罕见的情况下，查询存储可能会因内部错误而导致处于错误状态。

当实际状态为只读时，可使用 **readonly_reason** 列来确定根本原因。 通常情况下，你会发现，查询存储转换为只读模式是因为超出了大小配额。 这种情况下，readonly_reason 设置为 65536。 有关其他原因，请参阅 [sys.database_query_store_options (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)。

考虑执行以下步骤将 Query Store 切换为读写模式并激活数据收集功能：

- 使用 **ALTER DATABASE** 的 **MAX_STORAGE_SIZE_MB** 选项增大最大存储大小。
- 使用以下语句清理 Query Store 数据：

  ```sql
  ALTER DATABASE [QueryStoreDB] SET QUERY_STORE CLEAR;
  ```

在应用这两项或其中一项步骤时，可以执行以下语句，通过显式方式将操作模式改回为读写：

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
```

采用以下前摄性步骤：

- 遵循最佳实践规范即可避免在无提示情况下更改操作模式。 如果可以确保查询存储大小始终小于最大允许值，则会极大地降低转换为只读模式的几率。 根据[配置查询存储](#Configure)部分所述，可激活基于大小的策略，使查询存储在大小接近极限时自动清除数据。
- 为了确保最新的数据能够得到保留，可将基于时间的策略配置为定期删除过时信息。
- 最后，请考虑将“查询存储捕获模式”设置为“Auto”，因为这样通常可以筛选掉与工作负载不太相关的查询 。

### <a name="error-state"></a>错误状态

若要恢复查询存储，可尝试以显式方式设置读写模式，然后再次检查实际状态。

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
    max_storage_size_mb, readonly_reason, interval_length_minutes,
    stale_query_threshold_days, size_based_cleanup_mode_desc,
    query_capture_mode_desc
FROM sys.database_query_store_options;
```

如果问题仍然存在，则表明磁盘上的查询存储数据已永久损坏。

从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始，可通过在受影响的数据库内执行 sp_query_store_consistency_check 存储过程来恢复查询存储。 必须先禁用查询存储，然后才能尝试恢复操作。 对于 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]，需要从查询存储中清除数据，如下所示。

如果恢复失败，可先尝试清除查询存储，然后再设置读写模式。

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE CLEAR;
GO

ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
    max_storage_size_mb, readonly_reason, interval_length_minutes,
    stale_query_threshold_days, size_based_cleanup_mode_desc,
    query_capture_mode_desc
FROM sys.database_query_store_options;
```

## <a name="set-the-optimal-query-store-capture-mode"></a>设置最佳查询存储捕获模式

在 Query Store 中保留最相关数据。 下表描述了每个查询存储捕获模式的典型方案：

|Query Store 捕获模式|场景|
|------------------------|--------------|
|**全部**|对工作负载进行彻底地分析，分析所有查询的形状及其执行频率和其他统计信息。<br /><br /> 识别工作负荷中的新查询。<br /><br /> 检测是否使用即席查询来识别用户或自动参数化的机会。<br /><br />注意：这是 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 中的默认捕获模式。|
|**Auto**|关注相关且可操作的查询。 例如，那些定期执行的查询或资源消耗很大的查询。<br /><br />注意：从 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 开始，这是默认捕获模式。|
|无|你已经捕获了需要在运行时监视的查询集，因此需消除其他查询可能会带来的干扰。<br /><br /> “无”适用于测试和基准测试环境。<br /><br /> “无”也适用于需要提供已配置的 Query Store 配置来监视其应用程序工作负荷的软件供应商。<br /><br /> 在使用“无”时应格外小心，因为可能无法跟踪和优化重要的新查询。 避免使用“无”，除非你的特定方案需要使用它。|
|**自定义**|[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 在 `ALTER DATABASE SET QUERY_STORE` 命令下引入自定义捕获模式。 启用后，在新的“查询存储捕获策略”设置下有额外可用的查询存储配置，可用于微调特定服务器中的数据收集。<br /><br />新的自定义设置定义在内部捕获策略时间阈值期间执行的操作。 这是评估配置条件的时间边界，如果所有值为 true，则查询存储可以捕获查询。 有关详细信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。|

> [!NOTE]
> 当查询存储捕获模式设置为“全部”、“自动”或“自定义”时，始终捕获游标、存储过程中的查询和本机编译的查询  。 若要捕获本机编译的查询，请使用 [sys.sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md) 启用每个查询统计信息的收集。

## <a name="keep-the-most-relevant-data-in-query-store"></a>在 Query Store 中保留最相关数据

将查询存储配置为只包含最相关的数据，这样在持续运行的时候对常规工作负载的影响最小，方便进行故障排除。
下表提供最佳实践：

|最佳做法|设置|
|-------------------|-------------|
|对保留的历史数据进行限制。|配置基于时间的策略以激活自动清理功能。|
|筛选掉不相关的查询。|将“查询存储捕获模式”配置为“自动” 。|
|达到最大大小时，删除不太相关的查询。|激活基于大小的清理策略。|

## <a name="avoid-using-non-parameterized-queries"></a><a name="Parameterize"></a> 避免使用非参数化查询

在不必要的情况下使用非参数化查询不是最佳做法。 临时分析就是一个示例。 不能重复使用缓存的计划，因为这会强制查询优化器在碰到每个唯一的查询文本时都进行查询编译。 有关详细信息，请参阅[强制参数化使用指南](../../relational-databases/query-processing-architecture-guide.md#ForcedParamGuide)。

此外，查询存储可能会很快超过大小配额，因为可能会存在大量不同的查询文本，导致存在大量不同但具有相同形状的执行计划。 结果就是，工作负载性能无法优化，查询存储可能会切换为只读模式，或者可能会不断删除数据以应对传入的查询。

请考虑以下选项：

- 在适用时进行参数化查询。 例如，在存储过程或 sp_executesql 中包装查询。 有关详细信息，请参阅[参数和执行计划重用](../../relational-databases/query-processing-architecture-guide.md#PlanReuse)。
- 如果工作负载包含许多一次性使用的临时批处理且查询计划各不相同，请使用[针对临时工作负载进行优化](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md)选项。
  - 将不同 query_hash 值的数目与 sys.query_store_query 中项的总数进行比较。 如果该比率接近 1，则说明临时工作负载生成了不同的查询。
- 如果不同查询计划的数量不多，请对数据库或部分查询应用[强制参数化](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)。
  - 请参阅[计划指南](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md)，仅对选定查询强制执行参数化操作。
  - 如果工作负载中不同查询计划的数目很小，则使用 [parameterization database option](../../relational-databases/databases/database-properties-options-page.md#miscellaneous) 命令配置强制的参数化操作。 例如，当不同 query_hash 的计数与 sys.query_store_query 中项的总数之比远小于 1 时。
- 将 QUERY_CAPTURE_MODE 设置为 AUTO 即可自动筛选掉资源消耗小的即席查询。

## <a name="avoid-a-drop-and-create-pattern-for-containing-objects"></a><a name="Drop"></a> 避免对包含对象使用 DROP 和 CREATE 模式

查询存储会将查询条目与包含对象（例如存储过程、函数和触发器）相关联。 重新创建包含对象时，会针对同一查询文本生成新的查询条目。 这会阻止你跟踪该查询在一定时段内的性能统计信息，并会使用计划强制机制。 若要避免这种情况，请尽可能使用 `ALTER <object>` 过程来更改包含对象定义。

## <a name="check-the-status-of-forced-plans-regularly"></a><a name="CheckForced"></a> 定期检查强制计划的状态

可以方便地使用计划强制机制来修复关键查询的性能问题，使这些查询的结果更可预测。 与计划提示和计划指南一样，强制实施某项计划并不能确保在今后的执行过程中会用到它。 通常情况下，如果对数据库架构的更改导致执行计划所引用的对象被更改或删除，计划强制就会失败。 在这种情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会回退到重新编译查询，而强制失败的实际原因则显示在 [sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md) 中。 以下查询返回强制计划的相关信息：

```sql
USE [QueryStoreDB];
GO

SELECT p.plan_id, p.query_id, q.object_id as containing_object_id,
    force_failure_count, last_force_failure_reason_desc
FROM sys.query_store_plan AS p
JOIN sys.query_store_query AS q on p.query_id = q.query_id
WHERE is_forced_plan = 1;
```

有关原因的完整列表，请参阅 [sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)。 你还可以使用 query_store_plan_forcing_failed XEvent 来跟踪和故障排除计划强制失败情况。

## <a name="avoid-renaming-databases-for-queries-with-forced-plans"></a><a name="Renaming"></a> 避免在使用强制计划执行查询时重命名数据库

执行计划使用由三个部分组成的名称（例如 `database.schema.object`）来引用对象。

如果重命名数据库，计划强制就会失败，导致在执行所有后续的查询时都需要重新编译。

## <a name="using-query-store-in-mission-critical-servers"></a><a name="Recovery"></a> 在任务关键型服务器中使用查询存储

全局跟踪标志 7745 和 7752 可用于使用查询存储来提高数据库的可用性。 有关更多信息，请参见[跟踪标记](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。

- 跟踪标志 7745 会阻止以下默认行为：在可关闭 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之前，查询存储将数据写入磁盘。 这意味着在 `DATA_FLUSH_INTERVAL_SECONDS` 定义的时间窗口之前，已收集但尚未保留到磁盘的查询存储数据将会丢失。
- 跟踪标志 7752 启用了查询存储的异步加载。 这会使数据库变为联机状态，并且在查询存储完全恢复之前执行查询。 默认行为是同步加载查询存储。 默认行为可在恢复查询存储之前防止执行查询，但同时也可在数据集合中防止遗漏任何查询。

> [!NOTE]
> 从 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 开始，此行为由引擎控制，跟踪标志 7752 不再有效。

> [!IMPORTANT]
> 如果仅对 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 中的实时工作负载见解使用查询存储，请尽快安装 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU2 ([KB 4340759](https://support.microsoft.com/help/4340759)) 中的性能可伸缩性改进。 如果没有这些改进，则当数据库处于繁重的工作负载下时，可能会发生旋转锁争用，并且服务器性能可能会变慢。 特别是，你可能会发现 `QUERY_STORE_ASYNC_PERSIST` 旋转锁或 `SPL_QUERY_STORE_STATS_COOKIE_CACHE` 旋转锁上出现繁重的争用情况。 应用此改进后，查询存储将不再导致旋转锁争用。

> [!IMPORTANT]
> 如果仅对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 到 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]）中的实时工作负载见解使用查询存储，请考虑尽快安装 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU15、[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU23 和 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU9 中的性能可伸缩性改进。 如果没有此改进，则当数据库处于繁重的即席工作负载下时，查询存储可能会占用大量内存，并且服务器性能可能会变慢。 应用此改进后，查询存储会对其各个组件可使用的内存量施加内部限制，并且可以自动将操作模式更改为只读，直到有足够的内存返回到 [!INCLUDE[ssde_md](../../includes/ssde_md.md)]。 请注意，不会记录查询存储内部内存限制，因为它们随时可能更改。  


## <a name="using-query-store-in-azure-sql-database-active-geo-replication"></a><a name="geosyncreplicas"></a> 在 Azure SQL 数据库活动异地复制中使用查询存储

Azure SQL 数据库的辅助活动异地复制上的查询存储将是主要副本上的活动的只读副本。 

避免不匹配的 Azure SQL 数据库层参与异地复制。 辅助数据库在大小方面应与主数据库相同或相近，并且应与主数据库处于同一服务层。 在 [sys.dm_db_wait_stats](../system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database.md) 中查找 HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO 等待类型，该类型表示由于辅助延迟而导致主副本上的事务日志速率受限。

若要详细了解如何估计和配置活动异地复制的辅助 Azure SQL 数据库的大小，请参阅[配置辅助数据库](/azure/azure-sql/database/active-geo-replication-overview#configuring-secondary-database)。


## <a name="see-also"></a>另请参阅

- [ALTER DATABASE SET 选项 &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)
- [查询存储目录视图 &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)
- [查询存储存储过程 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)
- [通过内存中 OLTP 使用查询存储](../../relational-databases/performance/using-the-query-store-with-in-memory-oltp.md)
- [使用查询存储来监视性能](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md)
