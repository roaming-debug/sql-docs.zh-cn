---
title: 智能查询处理
description: 智能查询处理功能，用于提高 SQL Server 和 Azure SQL 数据库中的查询性能。
ms.custom: seo-dt-2019
ms.date: 11/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords: ''
author: joesackmsft
ms.author: josack
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 83f19187abd2395ef453e59508b2404a7b10a728
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469188"
---
# <a name="intelligent-query-processing-in-sql-databases"></a>SQL 数据库中的智能查询处理

[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

智能查询处理 (IQP) 功能系列包含有广泛影响的功能，既能提升现有工作负荷的性能，还能最大限度地减少实现工作量。 

![智能查询处理](./media/iqp-feature-family.png)

本视频时长 6 分钟，请观看本视频大致了解智能查询处理：

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Overview-Intelligent-Query-processing-in-SQL-Server-2019/player?WT.mc_id=dataexposed-c9-niner]


可以通过对数据库启用适当的数据库兼容性级别使工作负荷自动符合只能查询处理条件。 可使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 进行此设置。 例如：  

```sql
ALTER DATABASE [WideWorldImportersDW] SET COMPATIBILITY_LEVEL = 150;
```

下表详细列出了所有智能查询处理功能，以及针对数据库兼容性级别必须具备的任何要求。

| **IQP 功能** | 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 和 [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] 中受支持 | 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中受支持 |**说明** |
| ---------------- | ------- | ------- | ---------------- |
| [自适应联接（批处理模式）](#batch-mode-adaptive-joins) | 是，兼容性级别为 140| 是，自 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 起，兼容性级别为 140|自适应联接在运行时期间根据实际输入行自动选择联接类型。|
| [非重复近似计数](#approximate-query-processing) | 是| 是，自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起|由于高性能和低内存占用量，可针对大数据方案提供近似的 COUNT DISTINCT。 |
| [行存储上的批处理模式](#batch-mode-on-rowstore) | 是，兼容性级别为 150| 是，自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起，兼容性级别为 150|可为 CPU 绑定关系的 DW 工作负载提供批处理模式，无需列存储索引。  | 
| [交错执行](#interleaved-execution-for-mstvfs) | 是，兼容性级别为 140| 是，自 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 起，兼容性级别为 140|请使用在首次编译时遇到的多语句表值函数的实际基数，而不是一个固定猜测值。|
| [内存授予反馈（批处理模式）](#batch-mode-memory-grant-feedback) | 是，兼容性级别为 140| 是，自 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 起，兼容性级别为 140|如果批处理模式查询有溢出到磁盘的操作，则需为以后的执行添加更多内存。 如果查询浪费分配给它的超过 50% 内存，请对连续的执行减少内存授予。|
| [内存授予反馈（行模式）](#row-mode-memory-grant-feedback) | 是，兼容性级别为 150| 是，自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起，兼容性级别为 150|如果行模式查询有溢出到磁盘的操作，则需为以后的执行添加更多内存。 如果查询浪费分配给它的超过 50% 内存，请对连续的执行减少内存授予。|
| [标量 UDF 内联](#scalar-udf-inlining) | 否 | 是，自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起，兼容性级别为 150|标量 UDF 转换为“内联”在调用查询中的等效关系表达式，这通常会大幅提升性能。|
| [表变量延迟编译](#table-variable-deferred-compilation) | 是，兼容性级别为 150| 是，自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起，兼容性级别为 150|请使用在首次编译时遇到的表变量的实际基数，而不是一个固定猜测值。|

## <a name="batch-mode-adaptive-joins"></a>批处理模式自适应联接
利用一个缓存计划，批处理模式自适应联接功能，可延迟选择[哈希联接或嵌套循环联接](../../relational-databases/performance/joins.md)方法，直到扫描第一个输入后  。 自适应联接运算符可定义用于决定何时切换到嵌套循环计划的阈值。 因此，计划可在执行期间动态切换到较好的联接策略。

有关详细信息，包括如何在不更改兼容性级别的情况下禁用自适应连接，请参阅[了解自适应连接](../../relational-databases/performance/joins.md#adaptive)。

## <a name="batch-mode-memory-grant-feedback"></a>批处理模式内存授予反馈
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中查询执行后的计划包括执行所需的最小内存和能将所有行纳入内存的理想内存授予大小。 如果内存授予大小不正确，性能将受到影响。 如果授予过量，则会导致内存浪费，减少并发执行。 如果内存授予不足，则会导致到磁盘的昂贵溢出。 通过解决重复工作负荷，批处理模式内存授予反馈可重新计算查询所需的实际内存，并更新缓存计划的授予值。 执行相同的查询语句时，查询将使用修改后的内存授予大小，减少影响并发的过量内存授予，并修复造成到磁盘的昂贵溢出的估计不足的内存授予。
下图是使用批处理模式自适应内存授予反馈的一个示例。 对于首次执行查询，由于高溢出，持续时间为 88 秒  ：   

```sql
DECLARE @EndTime datetime = '2016-09-22 00:00:00.000';
DECLARE @StartTime datetime = '2016-09-15 00:00:00.000';
SELECT TOP 10 hash_unique_bigint_id
FROM dbo.TelemetryDS
WHERE Timestamp BETWEEN @StartTime and @EndTime
GROUP BY hash_unique_bigint_id
ORDER BY MAX(max_elapsed_time_microsec) DESC;
```

![高溢出](./media/2_AQPGraphHighSpills.png)

启用内存授予反馈后，对于第二次执行，持续时间为 1 秒  （从 88 秒减少），完全消除溢出，且授予内存更高： 

![无溢出](./media/3_AQPGraphNoSpills.png)

### <a name="memory-grant-feedback-sizing"></a>内存授予反馈大小调整
对于内存授予过量的情况，如果授予的内存是实际使用内存大小的两倍，内存授予反馈将重新计算内存授予并更新缓存的计划。 内存授予不足 1 MB 的计划将不会针对超额重新进行计算。
对于内存授予大小不足，造成批处理模式运算符溢出到磁盘的情况，内存授予反馈将触发内存授予的重新计算。 将向内存授予反馈报告溢出事件，并可通过 spilling_report_to_memory_grant_feedback xEvent 显露  。 此事件将返回计划的节点 ID 和该节点溢出的数据大小。

### <a name="memory-grant-feedback-and-parameter-sensitive-scenarios"></a>内存授予反馈和参数敏感型方案
为保持最优，不同的参数值可能还需要不同的查询计划。 此类查询被定义为“参数敏感型”。 对于参数敏感型计划，如果查询具有不稳定的内存要求，则内存授予反馈对该查询禁用。 在重复运行查询后禁用计划，并且可以通过监视 memory_grant_feedback_loop_disabled xEvent 观察到这一切  。 有关参数截取和参数敏感度的详细信息，请参阅[查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing)。

### <a name="memory-grant-feedback-caching"></a>内存授予反馈缓存
反馈可以存储在单个执行的缓存计划中。 它是该语句的连续执行，但受益于内存授予反馈调整。 此功能适用于重复执行语句。 内存授予反馈将只更改缓存的计划。 当前未在查询存储中捕获更改。
如果从缓存中逐出计划，则不会保存反馈。 如果存在故障转移，则反馈也将丢失。 使用 `OPTION (RECOMPILE)` 的语句可创建新的计划，但不会缓存它。 由于它未被缓存，因此不会产生内存授予反馈，也不会针对编译和执行存储。 但是，如果已缓存未使用 `OPTION (RECOMPILE)` 的等效语句（即包含相同的查询哈希）并重新执行，连续语句可从内存授予反馈中受益  。

### <a name="tracking-memory-grant-feedback-activity"></a>跟踪内存授予反馈活动
可以使用 memory_grant_updated_by_feedback xEvent 跟踪内存授予反馈事件  。 此事件可跟踪当前执行计数历史记录，内存授予反馈更新计划的次数，修改前理想的额外内存授予，以及内存授予反馈修改缓存计划后理想的额外内存授予。

### <a name="memory-grant-feedback-resource-governor-and-query-hints"></a>内存授予反馈、资源调控器和查询提示
实际内存授予服从资源调控器或查询提示确定的查询内存限制。

### <a name="disabling-batch-mode-memory-grant-feedback-without-changing-the-compatibility-level"></a>在不更改兼容级别的情况下禁用批处理模式内存授予反馈
可在数据库或语句范围内禁用内存授予反馈，同时将数据库兼容级别维持在 140 或更高。 若要对源自数据库的所有查询执行禁用批处理模式内存授予反馈，请在对应数据库的上下文中执行以下命令：

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK = ON;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_MEMORY_GRANT_FEEDBACK = OFF;
```

启用后，此设置在 [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) 将显示为已启用。

若要对源自数据库的所有查询执行重新启用批处理模式内存授予反馈，请在对应数据库的上下文中执行以下命令：

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK = OFF;

-- Azure SQL Database, SQL Server 2019 and higher
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_MEMORY_GRANT_FEEDBACK = ON;
```

此外，将 `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` 指定为 [USE HINT 查询提示](../../t-sql/queries/hints-transact-sql-query.md#use_hint)也可为特定查询禁用批处理模式内存授予反馈。 例如：

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (USE HINT ('DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK')); 
```

USE HINT 查询提示的优先级高于数据库范围的配置或跟踪标志设置。

## <a name="row-mode-memory-grant-feedback"></a>行模式内存授予反馈

**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

通过调整批处理模式和行模式运算符的内存授予大小，行模式内存授予反馈扩展了批处理模式内存授予反馈功能。  

若要在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中启用行模式内存授予反馈，请为执行查询时连接到的数据库启用数据库兼容性级别 150。

通过 memory_grant_updated_by_feedback  XEvent，可以查看行模式内存授予反馈活动。 

从行模式内存授予反馈开始，将会对实际执行后计划显示两个新查询计划属性：“IsMemoryGrantFeedbackAdjusted”*_和“LastRequestedMemory”（它们添加到“MemoryGrantInfo”_*查询计划 XML 元素中）。 

LastRequestedMemory 显示上一次查询执行中的授予内存（以千字节 (KB) 为单位）  。 使用 IsMemoryGrantFeedbackAdjusted 属性，可以查看实际查询执行计划内语句的内存授予反馈状态  。 下面列出了此属性的可取值：

| IsMemoryGrantFeedbackAdjusted 值 | 说明 |
|---|---|
| No:First Execution | 内存授予反馈不调整用于首次编译和相关执行的内存。  |
| No:Accurate Grant | 如果没有溢出到磁盘，且语句使用至少 50% 的授予内存，就不会触发内存授予反馈。 |
| No:Feedback disabled | 如果内存授予反馈不断触发，且在内存增加和内存减少操作之间波动，就会对语句禁用内存授予反馈。 |
| Yes:Adjusting | 内存授予反馈已应用，并且可能会针对下一次执行进行进一步调整。 |
| Yes:Stable | 内存授予反馈已应用，并且授予内存现在处于稳定状态。也就是说，为上一次执行最后授予的内存是为当前执行授予的内存。 |

### <a name="disabling-row-mode-memory-grant-feedback-without-changing-the-compatibility-level"></a>在不更改兼容级别的情况下禁用行模式内存授予反馈
可在数据库或语句范围内禁用行模式内存授予反馈，同时将数据库兼容级别维持在 150 或更高。 若要对源自数据库的所有查询执行禁用行模式内存授予反馈，请在对应数据库的上下文中执行以下命令：

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ROW_MODE_MEMORY_GRANT_FEEDBACK = OFF;
```

若要对源自数据库的所有查询重新启用行模式内存授予反馈，请在对应数据库的上下文中执行以下命令：

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ROW_MODE_MEMORY_GRANT_FEEDBACK = ON;
```

此外，将 `DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK` 指定为 [USE HINT 查询提示](../../t-sql/queries/hints-transact-sql-query.md#use_hint)也可对特定查询禁用行模式内存授予反馈。 例如：

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (USE HINT ('DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK')); 
```

USE HINT 查询提示的优先级高于数据库范围的配置或跟踪标志设置。

## <a name="interleaved-execution-for-mstvfs"></a>适用于 MSTVF 的交错执行
通过交错执行，函数中的实际行计数可用于做出更明智的下游查询计划决策。 若要详细了解多语句表值函数 (MSTVF)，请参阅[表值函数](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF)。

交错执行可更改单一查询执行的优化和执行阶段之间的单向边界，并使计划能够根据修订后的基数估值进行调整。 如果遇到交错执行的候选项，该项当前为“多语句表值函数 (MSTVF)”，将暂停优化，执行适用的子树，捕获准确的基数估值，然后针对下游操作继续优化  。   

自 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 起，MSTVF 的固定基数猜测为“100”，而早期 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本为“1”。 交错执行有助于解决由于与 MSTVF 关联的这些固定基数估值而导致的工作负荷性能问题。 有关 MSTVF 的详细信息，请参阅[创建用户定义函数（数据库引擎）](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF)。

下图描绘了[实时查询统计信息](../../relational-databases/performance/live-query-statistics.md)输出，一个显示 MSTVF 的固定基数估值影响的整体执行计划的子集。 可查看实际行流与估计行数。 有三个值得注意的计划区域（从右到左显示）：
1. MSTVF 表扫描具有 100 行的固定估值。 然而，对于此示例，有 527,597 行流经此 MSTVF 表扫描，如通过“527597 行，共 100 行”的实际与估值的实时查询统计信息中所示，因此固定估值偏差显著  。
1. 对于嵌套循环操作，仅假定联接的外侧将返回 100 行。 如果 MSTVF 实际返回惊人的行数，最好将另一联接算法一起使用。
1. 对于哈希匹配操作，请注意小型警告符号，在本示例中指示到磁盘的溢出。

![行流与估计行数](./media/7_AQPFlowThreeAreas.png)

将之前的计划与通过启用交替执行生成的实际计划进行对比：

![交错的计划](./media/8_AQPInterleavedEnabledPlan.png)

1. 请注意，MSTVF 表扫描现可反映准确的基数估值。 另请注意此表扫描的重新排序和其他操作。
1. 而关于联接算法，我们已改为从嵌套循环操作切换到哈希匹配操作，后者在涉及大量行时更优。
1. 另请注意，我们不再发出溢出警告，因为我们将基于从 MSTVF 表扫描流出的真实行数授予更多内存。

### <a name="interleaved-execution-eligible-statements"></a>交错执行符合条件的语句
交错执行中的 MSTVF 引用语句当前必须处于只读，且不为数据修改操作的一部分。 此外，如果 MSTVF 不使用运行时常数，则其不适合用于交错执行。

### <a name="interleaved-execution-benefits"></a>交错执行的优点
一般情况下，估计行数与实际行数之间的偏差越大，下游计划操作数翻倍，则性能影响越大。
一般来说，交错执行有益于以下情况中的查询：
1. 对于中间结果集（本例中为 MSTVF），估计行数和实际行数之间存在巨大偏差。
1. 整体查询对中间结果的大小更改十分敏感。 这通常发生在查询计划中的该子树上存在复杂树的情况。
MSTVF 中简单的 `SELECT *` 不会获益于交错执行。

### <a name="interleaved-execution-overhead"></a>交错执行的开销
开销应为最少-到-无。 MSTVF 已在引入交错执行前具体化，但区别是现在已允许延迟优化，并可利用具体化行集的基数估值。
与任何影响计划的更改一样，某些计划更改后虽可获得更好的子树基数，但同时也会使整体查询计划变得更糟。 缓解可包括还原兼容级别或使用查询存储来强制计划的非回归版本。

### <a name="interleaved-execution-and-consecutive-executions"></a>交错执行和连续执行
缓存交错执行计划后，包含首次执行的修订后的估值的计划将用于连续执行，无需重新实例化交错执行。

### <a name="tracking-interleaved-execution-activity"></a>跟踪交错执行活动
可在实际查询执行计划中查看使用情况属性：

| 执行计划属性 | 说明 |
| --- | --- |
| ContainsInterleavedExecutionCandidates | 适用于 QueryPlan 节点  。 如果为“true”，表示计划包含交错执行候选项  。 |
| IsInterleavedExecuted | TVF 节点的 RelOp 下的 RuntimeInformation 元素的属性  。 如果为“true”，表示该操作已具体化为交错执行操作的一部分  。 |

还可以通过以下 xEvent 跟踪交错执行匹配项：

| XEvent | 说明 |
| ---- | --- |
| interleaved_exec_status | 发生交错执行时将引发此事件。 |
| interleaved_exec_stats_update | 此事件介绍交错执行更新的基数估值。 |
| Interleaved_exec_disabled_reason | 包含交错执行可能的候选项的查询实际未获取交错执行时，将引发此事件。 |

必须执行查询才能允许交错执行修改 MSTVF 基数估值。 但是，如果通过 `ContainsInterleavedExecutionCandidates` 显示计划属性存在交错执行候选项，则估计的执行计划仍将显示。    

### <a name="interleaved-execution-caching"></a>交错执行缓存
如果已从缓存中逐出或清除计划，则在执行查询时将出现使用交错执行的全新编译。
使用 `OPTION (RECOMPILE)` 的语句将创建使用交错执行的新计划，但不会进行缓存。     

### <a name="interleaved-execution-and-query-store-interoperability"></a>交错执行和查询存储互操作性
无法强制使用交错执行的计划。 该计划是具有基于初始执行的正确基数估值的版本。    

### <a name="disabling-interleaved-execution-without-changing-the-compatibility-level"></a>在不更改兼容级别的情况下禁用交错执行
可在数据库或语句范围内禁用交错执行，同时将数据库兼容性级别维持在 140 或更高。  若要对源自数据库的所有查询执行禁用交错执行，请在对应数据库的上下文中执行以下命令：

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_INTERLEAVED_EXECUTION_TVF = ON;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET INTERLEAVED_EXECUTION_TVF = OFF;
```

启用后，此设置在 [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) 将显示为已启用。
若要对源自数据库的所有查询执行重新启用交错执行，请在适用的数据库的上下文中执行以下命令：

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_INTERLEAVED_EXECUTION_TVF = OFF;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET INTERLEAVED_EXECUTION_TVF = ON;
```

此外，将 `DISABLE_INTERLEAVED_EXECUTION_TVF` 指定为 [USE HINT 查询提示](../../t-sql/queries/hints-transact-sql-query.md#use_hint)也可对特定查询禁用交错执行。 例如：

```sql
SELECT [fo].[Order Key], [fo].[Quantity], [foo].[OutlierEventQuantity]
FROM [Fact].[Order] AS [fo]
INNER JOIN [Fact].[WhatIfOutlierEventQuantity]('Mild Recession',
                            '1-01-2013',
                            '10-15-2014') AS [foo] ON [fo].[Order Key] = [foo].[Order Key]
                            AND [fo].[City Key] = [foo].[City Key]
                            AND [fo].[Customer Key] = [foo].[Customer Key]
                            AND [fo].[Stock Item Key] = [foo].[Stock Item Key]
                            AND [fo].[Order Date Key] = [foo].[Order Date Key]
                            AND [fo].[Picked Date Key] = [foo].[Picked Date Key]
                            AND [fo].[Salesperson Key] = [foo].[Salesperson Key]
                            AND [fo].[Picker Key] = [foo].[Picker Key]
OPTION (USE HINT('DISABLE_INTERLEAVED_EXECUTION_TVF'));
```

USE HINT 查询提示的优先级高于数据库范围的配置或跟踪标志设置。

## <a name="table-variable-deferred-compilation"></a>表变量延迟编译

**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

表变量延迟编译  功能提升了计划质量和引用表变量的查询的整体性能。 在优化和初始计划编译期间，此功能会传播基于实际表变量行计数的基数估计。 然后，这种准确的行计数信息将用于优化下游计划操作。

使用“表变量延迟编译”，引用表变量的语句会延迟编译，直到首次实际执行语句后。 此延迟编译行为与临时表的行为相同。 此更改导致使用实际基数，而不是原始单行猜测。 

要启用表变量延迟编译，请为查询运行时连接到的数据库启用数据库兼容性级别 150。

表变量延迟编译不会更改表变量的任何其他特性  。 例如，此功能不会向表变量添加列统计信息。

表变量延迟编译不会增加重新编译频率  。 相反，它将转移初始编译出现的位置。 生成的缓存计划是基于初始延迟编译表变量行计数生成的。 缓存计划由连续查询重复使用。 会对计划重复使用，直至此计划已逐出或进行重新编译。 

用于初始计划编译的表变量行计数表示典型值可能不同于固定的猜测行计数。 如果不同，下游操作会有优势。 如果表变量行计数在整个执行过程中差别很大，则可能无法通过此功能来提升性能。

### <a name="disabling-table-variable-deferred-compilation-without-changing-the-compatibility-level"></a>在不更改兼容性级别的情况下禁用表变量延迟编译
可在数据库或语句范围内禁用表变量延迟编译，同时将数据库兼容性级别维持在 150 或更高。 若要对源自数据库的所有查询禁用表变量延迟编译，请在对应数据库的上下文中执行以下示例：

```sql
ALTER DATABASE SCOPED CONFIGURATION SET DEFERRED_COMPILATION_TV = OFF;
```

若要对源自数据库的所有查询重新启用表变量延迟编译，请在对应数据库的上下文中执行以下示例：

```sql
ALTER DATABASE SCOPED CONFIGURATION SET DEFERRED_COMPILATION_TV = ON;
```

此外，还可以通过将 DISABLE_DEFERRED_COMPILATION_TV 分配为 USE HINT 查询提示，为特定查询禁用表变量延迟编译。  例如：

```sql
DECLARE @LINEITEMS TABLE 
    (L_OrderKey INT NOT NULL,
     L_Quantity INT NOT NULL
    );

INSERT @LINEITEMS
SELECT L_OrderKey, L_Quantity
FROM dbo.lineitem
WHERE L_Quantity = 5;

SELECT O_OrderKey,
    O_CustKey,
    O_OrderStatus,
    L_QUANTITY
FROM    
    ORDERS,
    @LINEITEMS
WHERE    O_ORDERKEY    =    L_ORDERKEY
    AND O_OrderStatus = 'O'
OPTION (USE HINT('DISABLE_DEFERRED_COMPILATION_TV'));
```

## <a name="scalar-udf-inlining"></a>标量 UDF 内联

**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）

标量 UDF 内联会自动将[标量 UDF](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#Scalar) 转换为关系表达式， 并将它们嵌入正在调用的 SQL 查询中。 此转换提升了利用标量 UDF 的工作负载的性能。 标量 UDF 内联可便于实现 UDF 内部基于成本的操作优化。 生成的是高效、面向集、并行的（而不是低效、迭代、串行的）执行计划。 在数据库兼容性级别 150 下默认启用此功能。

有关详细信息，请参阅[标量 UDF 内联](../user-defined-functions/scalar-udf-inlining.md)。

## <a name="approximate-query-processing"></a>近似查询处理

**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

近似查询处理是新的功能系列。 它可以跨响应速度比绝对精度更为关键的大型数据集进行聚合。 例如，要跨 100 亿行计算 COUNT(DISTINCT())  ，以供显示在仪表板上。 在这种情况下，绝对精度并不重要，而响应速度则至关重要。 新增的 APPROX_COUNT_DISTINCT  聚合函数返回组中唯一非 NULL 值的近似数。

有关详细信息，请参阅 [APPROX_COUNT_DISTINCT (Transact-SQL)](../../t-sql/functions/approx-count-distinct-transact-sql.md)。

## <a name="batch-mode-on-rowstore"></a>行存储上的批处理模式 

**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

行存储上的批处理模式可实现分析工作负载的批处理模式执行，而无需列存储索引。  此功能支持用于磁盘上堆和 B 树索引的批处理模式执行和位图筛选器。 行存储上的批处理模式可实现对所有现有支持批处理模式的运算符的支持。

### <a name="background"></a>背景
[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 引入了一项可加速分析工作负载的新功能，即列存储索引。 我们扩展了用例，并改进了每个后续版本中列存储索引的性能。 到目前为止，我们已将所有这些功能作为单一功能进行了表述和记录。 可以在表上创建列存储索引。 分析工作负载的速度会变快。 不过，有两套相关但不同的技术：
- 使用列存储  索引，分析查询仅访问所需列中的数据。 与传统行存储  索引中的压缩相比，列存储格式的页压缩更高效。 
- 使用批处理模式  处理，查询运算符可以更高效地处理数据。 它们一次处理一批行，而不是一行。 许多其他可伸缩性改进都与批处理模式处理相关。 若要详细了解批处理模式，请参阅[执行模式](../../relational-databases/query-processing-architecture-guide.md#execution-modes)。

两组功能协同工作，以改进输入/输出 (I/O) 和 CPU 利用率：
- 通过使用列存储索引，更多数据适合内存。 这会减少 I/O 工作负荷。
- 批处理模式处理可更有效地使用 CPU。

这两种技术尽可能利用彼此。 例如，批处理模式聚合可以计算为列存储索引扫描的一部分。 此外，通过批处理模式联接和批处理模式聚合，使用运行长度编码可以更有效地处理压缩的列存储数据。 
 
但是，重要的是要了解这两个功能是独立的：
* 你可以获取使用列存储索引的行模式计划。
* 你可以获取仅使用行存储索引的批处理模式计划。 

结合使用这两种功能时，通常效果最佳。 因此，到目前为止，SQL Server 查询优化器仅对涉及至少一个有列存储索引的表的查询考虑使用批处理模式处理。

列存储索引可能不适用于某些应用程序。 应用程序可能会使用列存储索引不支持的其他一些功能。 例如，就地修改与列存储压缩不兼容。 因此，带有聚集列存储索引的表不支持触发器。 更重要的是，列存储索引会增加 DELETE 和 UPDATE 语句的开销   。 

对于一些事务与分析混合工作负荷，事务工作负荷的开销超过了使用列存储索引的获益。 此类方案可以通过仅使用批处理模式处理来提高 CPU 使用率，以从中获益。 这就是为什么无论涉及哪种索引类型，行存储上的批处理模式功能都会考虑所有查询的批处理模式。

### <a name="workloads-that-might-benefit-from-batch-mode-on-rowstore"></a>可能会受益于行存储上的批处理模式的工作负载
以下工作负载可能会受益于行存储上的批处理模式：
* 工作负载的很大一部分由分析查询组成。 通常情况下，这些查询使用联接或聚合等运算符，可处理数十万行或更多行。
* 工作负载受 CPU 限制。 如果瓶颈是 I/O，仍建议尽可能考虑使用列存储索引。
* 创建列存储索引会增加太多的工作负载事务部分开销。 或者，创建列存储索引不可行，因为应用程序依赖列存储索引尚不支持的功能。


> [!NOTE]
> 只有通过减少 CPU 利用率，行存储上的批处理模式才能起到帮助作用。 如果瓶颈与 I/O 相关，且数据尚未缓存（“冷”缓存），那么行存储上的批处理模式将不会改善查询运行时间。 同样，如果计算机上没有足够的内存来缓存所有数据，性能也不可能得到提高。

### <a name="what-changes-with-batch-mode-on-rowstore"></a>行存储上的批处理模式带来哪些变化？

请将数据库兼容性级别设置为 150。 不需要进行其他更改。

即使查询不访问任何具有列存储索引的表，查询处理器也将使用启发式方法来决定是否考虑批处理模式。 启发式方法包括以下检查：
1. 初始检查输入查询中的表大小、使用的运算符和估计的基数。
2. 其他检查点，因为优化器会发现新的、成本更低的查询计划。 如果这些替代计划没有大量使用批处理模式，优化器会停止探索批处理模式替代方案。

如果使用行存储上的批处理模式，则会发现在查询计划中实际运行模式为批处理模式  。 扫描运算符对磁盘堆和 B 树索引使用批处理模式。 此批处理模式扫描可以评估批处理模式位图筛选器。 还可以在计划中看到其他批处理模式运算符。 例如，哈希联接、基于哈希的聚合、排序、窗口聚合、筛选器、串联和计算标量运算符。

### <a name="remarks"></a>备注

查询计划并不总是使用批处理模式。 查询优化器可能会认为批处理模式对查询没有益处。 

查询优化器搜索空间正在发生变化。 因此，如果获取行模式计划，它可能与在较低兼容性级别获取的计划不同。 另外，如果获取批处理模式计划，它可能与通过列存储索引获取的计划不同。 

鉴于新增的行存储上的批处理模式扫描，计划也可能会对混合列存储索引和行存储索引的查询发生变化。

新增的行存储上的批处理模式扫描当前存在以下限制： 
- 它不会为内存中 OLTP 表，或除磁盘堆和 B 树以外的任何索引启动。 
- 如果提取或筛选大型对象 (LOB) 列，它也不会启动。 此限制包括稀疏列集和 XML 列。

有些查询即使使用列存储索引，也无法使用批处理模式。 例如，涉及游标的查询。 这些相同的排除项也扩展到行存储上的批处理模式。

### <a name="configure-batch-mode-on-rowstore"></a>配置行存储上的批处理模式
BATCH_MODE_ON_ROWSTORE  数据库范围内配置默认处于启用状态。 无需更改数据库兼容性级别，即可禁用行存储上的批处理模式：

```sql
-- Disabling batch mode on rowstore
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ON_ROWSTORE = OFF;

-- Enabling batch mode on rowstore
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ON_ROWSTORE = ON;
```

可以通过数据库范围内配置来禁用行存储上的批处理模式。 但仍可使用 ALLOW_BATCH_MODE  查询提示，在查询一级替代设置。 以下示例启用行存储的批处理模式，即使通过数据库作用域配置禁用了该功能：

```sql
SELECT [Tax Rate], [Lineage Key], [Salesperson Key], SUM(Quantity) AS SUM_QTY, SUM([Unit Price]) AS SUM_BASE_PRICE, COUNT(*) AS COUNT_ORDER
FROM Fact.OrderHistoryExtended
WHERE [Order Date Key]<=DATEADD(dd, -73, '2015-11-13')
GROUP BY [Tax Rate], [Lineage Key], [Salesperson Key]
ORDER BY [Tax Rate], [Lineage Key], [Salesperson Key]
OPTION(RECOMPILE, USE HINT('ALLOW_BATCH_MODE'));
```

还可以使用 DISALLOW_BATCH_MODE  查询提示，对特定查询禁用行存储上的批处理模式。 请参阅以下示例：

```sql
SELECT [Tax Rate], [Lineage Key], [Salesperson Key], SUM(Quantity) AS SUM_QTY, SUM([Unit Price]) AS SUM_BASE_PRICE, COUNT(*) AS COUNT_ORDER
FROM Fact.OrderHistoryExtended
WHERE [Order Date Key]<=DATEADD(dd, -73, '2015-11-13')
GROUP BY [Tax Rate], [Lineage Key], [Salesperson Key]
ORDER BY [Tax Rate], [Lineage Key], [Salesperson Key]
OPTION(RECOMPILE, USE HINT('DISALLOW_BATCH_MODE'));
```

## <a name="see-also"></a>另请参阅
[SQL Server 数据库引擎和 Azure SQL 数据库的性能中心](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md)     
[查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md)    
[显示计划逻辑运算符和物理运算符参考](../../relational-databases/showplan-logical-and-physical-operators-reference.md)    
[联接](../../relational-databases/performance/joins.md)       
[演示智能查询处理](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/intelligent-query-processing)   
