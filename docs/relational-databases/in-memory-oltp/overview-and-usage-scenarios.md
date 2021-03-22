---
title: 概述和使用方案 | Microsoft Docs
description: 了解内存中 OLTP，它是 SQL Server 和 Azure SQL 数据库中用于优化事务处理的技术。 查看示例和其他资源。
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 62c964c5-eae4-4cf1-9024-d5a19adbd652
author: kevin-farlee
ms.author: kfarlee
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d84282b089b8695b0fa325e10e7462ce2e70e93a
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793113"
---
# <a name="overview-and-usage-scenarios"></a>概述和使用方案

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

内存中 OLTP 是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 中用于优化事务处理、数据引入、数据加载和瞬态数据方案性能的核心技术。 本文概述了内存中 OLTP 的技术和使用方案。 使用该信息可确定内存中 OLTP 是否适合你的应用程序。 本文末尾部分给出的示例介绍了内存中 OLTP 对象、性能演示引用以及资源引用，可供后续步骤使用。

本文介绍了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 中的内存中 OLTP 技术。 以下博客文章深入介绍了 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]的性能和资源使用率优势：[Azure SQL 数据库中的内存中 OLTP](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

## <a name="in-memory-oltp-overview"></a>内存中 OLTP 概述

对于合适的工作负荷，In-Memory OLTP 可提供显著的性能增益。 虽然在某些情况下，客户可实现高达 30 倍的性能增益，但是增益的多少取决于工作负荷。

那么是如何实现性能增益的呢？ 本质上，内存中 OLTP 通过提高数据访问和事务执行的效率和移除并发执行事务间的锁闩连接，来提升事务处理的性能：不是因为在内存中速度才快；而是因为内存中的数据得以优化速度才快。 数据存储、访问和处理算法经完全重新设计，以此来充分利用内存中和高并发计算的最新增强功能。

现在，数据位于内存中并不就意味着故障发生时会丢失数据。 默认情况下，所有事务皆为完全耐久事务，这意味着 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的其他任何表可获得相同的耐久性保证：作为事务提交的一部分，所有更改都会写入到磁盘上的事务日志中。 事务提交后的任何时间如果出现故障，当数据库重新联机时数据仍在其原来的位置。 此外，内存中 OLTP 还适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的所有高可用性和灾难恢复功能，例如 Always On，备份/还原等。

若要在数据库中使用内存中 OLTP，应使用以下一个或多个对象类型：

- 内存优化表 用于存储用户数据。 在创建时声明一个要进行内存优化的表。
- 非持久表 用于瞬态数据，即用于缓存，或用于中间结果集（替代传统的临时表）。 非持久表是一种使用 DURABILITY=SCHEMA_ONLY 声明的内存优化表，也就是说，对这些表所做的更改不会引发任何 IO。 在不需要考虑耐久性的情况下，这可避免使用日志 IO 资源。
- 内存优化表类型 用于表值参数 (TVP) 和存储过程中的中间结果集。 这些可用于替代传统表类型。 使用内存优化表类型声明的表变量和 TVP 会继承非持久内存优化表的优点：数据访问效率高，且不会引发 IO。
- 本机编译的 T-SQL 模块 可用于通过减少处理操作所需的 CPU 周期，进一步减少单个事务所需的时间。 在创建时声明一个要本机编译的 Transact-SQL 模块。 此时，可本机编译以下 T-SQL 模块：存储过程、触发器和标量用户定义的函数。

内存中 OLTP 内置于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]中。 由于这些对象与其传统对象行为相似，因此仅对数据库和应用程序作出最小的更改，通常便可增益性能优势。 此外，还可将内存优化表和基于磁盘的传统表同时置于同一数据库，然后在二者间运行查询。 本文靠近末尾部分给出的 Transact-SQL 脚本中就每个对象类型各提供一个示例。

## <a name="usage-scenarios-for-in-memory-oltp"></a>内存中 OLTP 使用方案

内存中 OLTP 并非一个魔力加速按钮，也并不适合所有工作负荷。 例如，如果大多数查询对大范围数据执行聚合，则内存优化表实际上并不会降低 CPU 使用率；列存储索引才适合此方案。

以下是方案和应用程序模式列表，其中展示了客户通过使用内存中 OLTP 获得了成功。

### <a name="high-throughput-and-low-latency-transaction-processing"></a>高吞吐量和低延迟事务处理

我们创建内存中 OLTP 正是为了此核心方案：支持大量事务，且各个事务延迟低而稳定。

常见工作负荷方案是：金融工具交易、体育博彩、移动游戏和广告投放。 我们注意到的另一种常见模式是会频繁读取和/或更新的“目录”。 例如，假如有多个文件，每个文件分布于多个群集节点上，则需在内存优化表中对每个文件的每个分片的位置编写目录。

#### <a name="implementation-considerations"></a>实现注意事项

将内存优化表用于核心事务表，即包含性能要求最高的事务的表。 使用本机编译的存储过程来优化执行与商业事务关联的逻辑。 放入数据库中存储过程中的逻辑越多，则内存中 OLTP 带来的益处越大。

在现有应用程序中开始使用：

1. 使用 [事务性能分析报表](determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md) 来确定要迁移的对象，
2. 然后使用 [内存优化](memory-optimization-advisor.md) 和 [本机编译](native-compilation-advisor.md) 顾问帮助进行迁移。

#### <a name="customer-case-studies"></a>客户案例研究

- CMC 市场利用 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 中的内存中 OLTP 来实现一致的低延迟：[由于第二次等待时间过长，因此该金融服务公司目前正在更新其交易软件。](https://customers.microsoft.com/story/because-a-second-is-too-long-to-wait-this-financial-services-firm-is-updating-its-trading-software)
- Derivco 利用 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 中的内存中 OLTP 来支持增加的吞吐量并处理剧增的工作负荷：[当联机游戏公司不希望在未来冒险时，就会在 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 上下赌注。](https://customers.microsoft.com/story/when-an-online-gaming-company-doesnt-want-to-risk-its-future-it-bets-on-sql-server-2016)

### <a name="data-ingestion-including-iot-internet-of-things"></a>数据引入，包括 IoT（物联网）

内存中 OLTP 擅长同时从多个不同的来源同时引入大量数据。 与其他目标相比，将数据引入到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库通常更有利，因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会加快数据的查询速度，让你可以获得实时见解。

常见的应用程序模式是：

- 引入传感器读数和事件，并允许通知和历史分析。
- 管理来自多个来源的批量更新，同时将对并发读取工作负荷的影响降至最低。

#### <a name="implementation-considerations"></a>实现注意事项

对数据引入使用内存优化表。 如果引入主要包括插入（而非更新），且需考虑到数据的内存中 OLTP 存储占用，则请

- 使用执行 [的作业，通过](../indexes/columnstore-indexes-overview.md)群集列存储索引 `INSERT INTO <disk-based table> SELECT FROM <memory-optimized table>`定期将数据批量卸载到基于磁盘的表中；或者
- 使用 [临时内存优化表](../tables/system-versioned-temporal-tables-with-memory-optimized-tables.md) 管理历史数据 - 在此模式下，历史数据则驻留在磁盘上，并且数据移动由系统管理。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 示例存储库包含一个智能网格应用程序，该应用程序使用临时内存优化表、内存优化表类型和本机编译的存储过程来提高数据引入速度，同时管理传感器数据的内存中 OLTP 存储占用：

- [smart-grid-release](https://github.com/Microsoft/sql-server-samples/releases/tag/iot-smart-grid-v1.0)
- [smart-grid-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/applications/iot-smart-grid)

#### <a name="customer-case-studies"></a>客户案例研究

- [Quorum doubles key database’s workload while lowering utilization by 70% by leveraging In-Memory OLTP in Azure SQL Database](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)（Quorum 利用 Azure SQL 数据库中的内存中 OLTP，将主要数据库的工作负荷翻倍，同时降低了 70% 的资源使用率）
- EdgeNet 使用 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中的内存中 OLTP 改进了批量数据加载性能，并且不再需要维护中间层缓存：[数据服务公司使用内存中技术获得实时访问产品数据的权限](https://customers.microsoft.com/story/data-services-firm-gains-real-time-access-to-product-d)
- 通过 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中的内存中 OLTP，Beth Israel Deaconess Medical Center 显着提高了域控制器中的数据引入率，并处理工作负载高峰： https://customers.microsoft.com/story/strengthening-data-security-and-creating-more-time-for

### <a name="caching-and-session-state"></a>缓存和会话状态

内存中 OLTP 技术使得 SQL 在维持会话状态（例如，用于 ASP.NET 应用程序）和缓存方面非常出色。

内存中 OLTP 的一个成功用例便是 ASP.NET 会话状态。 通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，客户可实现每秒 120 万次的请求。 同时，客户已开始将内存中 OLTP 用于企业中所有中间层应用程序的缓存需求。 详细信息：[bwin 如何使用 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 内存中 OLTP 达到前所未有的性能和规模](https://blogs.msdn.microsoft.com/sqlcat/2016/10/26/how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/)

#### <a name="implementation-considerations"></a>实现注意事项

通过将 BLOB 存储在 varbinary(max) 列中，可将非持久内存优化表用作简单的键值存储。 或者，可通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 中的 [JSON 支持](https://azure.microsoft.com/blog/json-support-is-generally-available-in-azure-sql-database/)实现半结构化缓存。 最后，可通过具有完整关系架构的非持久表（包括各种数据类型和约束）来创建完整的关系缓存。

通过利用 GitHub 上发布的脚本替换由内置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会话状态提供程序创建的对象，开始使用内存优化 ASP.NET 会话状态：[aspnet-session-state](https://github.com/Microsoft/sql-server-samples/tree/master/samples/applications/aspnet-session-state)

#### <a name="customer-case-studies"></a>客户案例研究

- bwin 通过 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中的内存中 OLTP，成功大幅提高了吞吐量，并减少了 ASP.NET 会话状态对硬件的占用：[游戏网站每秒可处理 250,000 次请求，改善了玩家体验](https://customers.microsoft.com/story/gaming-site-can-scale-to-250000-requests-per-second-an)
- bwin 利用 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 中的内存中 OLTP 技术，通过 ASP.NET 会话状态进一步提高了吞吐量，并实现了企业级中间层缓存系统：[bwin 如何使用 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 内存中 OLTP 达到前所未有的性能和规模](https://blogs.msdn.microsoft.com/sqlcat/2016/10/26/how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/)

### <a name="tempdb-object-replacement"></a>Tempdb 对象替换

使用非持久表和内存优化表类型替换传统的基于 TempDB 的结构，例如临时表、表变量和表值参数 (TVP)。

与传统表变量和 #temp 表相比，内存优化表变量和非持久表通常会降低 CPU 使用率，并完全删除日志 IO。

#### <a name="implementation-considerations"></a>实现注意事项

若要开始操作，请参阅：[使用内存优化改进临时表和表变量性能。](/archive/blogs/sqlserverstorageengine/improving-temp-table-and-table-variable-performance-using-memory-optimization)

#### <a name="customer-case-studies"></a>客户案例研究

- 一位客户仅通过使用内存优化 TVP 替换传统 TVP，便将性能成功提升了 40%：[在 Azure 中使用内存中 OLTP 来实现高速 IoT 数据引入](/archive/blogs/sqlserverstorageengine/a-technical-case-study-high-speed-iot-data-ingestion-using-in-memory-oltp-in-azure)
- 作为企业可扩展性改进的一部分，SentryOne 通过将 tempdb 中的表交换到内存中 OLTP 表，显著提高了数据引入能力，且监视解决方案的延迟几乎为零：[解决方案提供商通过数据监视创新突破性能上限。](https://customers.microsoft.com/story/sentryone-partner-professional-services-sql-server-azure)

### <a name="etl-extract-transform-load"></a>ETL（提取、转换、加载）

ETL 工作流通常包括将数据加载到临时表、转换数据和将数据加载到最终表。

#### <a name="implementation-considerations"></a>实现注意事项

将非持久内存优化表用于数据暂存。 非持久内存优化表会完全删除所有 IO，使数据访问更加高效。

如果工作流中要在临时表上执行转换，可使用本机编译的存储过程来加速转换进度。 如果可以并行执行此转换，则内存优化会扩大优势。

## <a name="sample-script"></a>示例脚本

开始使用内存中 OLTP 前，需要创建一个 MEMORY_OPTIMIZED_DATA 文件组。 此外，我们建议使用数据库兼容级别 130（或更高级别），并将数据库选项 MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT 设置为 ON。

可使用下面的脚本在默认数据文件夹中创建文件组，然后配置推荐设置：

- [enable-in-memory-oltp.sql](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/enable-in-memory-oltp.sql)

下面的脚本介绍了可在数据库中创建的内存中 OLTP 对象：

```sql
-- configure recommended DB option
ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON
GO
-- memory-optimized table
CREATE TABLE dbo.table1
( c1 INT IDENTITY PRIMARY KEY NONCLUSTERED,
  c2 NVARCHAR(MAX))
WITH (MEMORY_OPTIMIZED=ON)
GO
-- non-durable table
CREATE TABLE dbo.temp_table1
( c1 INT IDENTITY PRIMARY KEY NONCLUSTERED,
  c2 NVARCHAR(MAX))
WITH (MEMORY_OPTIMIZED=ON,
      DURABILITY=SCHEMA_ONLY)
GO
-- memory-optimized table type
CREATE TYPE dbo.tt_table1 AS TABLE
( c1 INT IDENTITY,
  c2 NVARCHAR(MAX),
  is_transient BIT NOT NULL DEFAULT (0),
  INDEX ix_c1 HASH (c1) WITH (BUCKET_COUNT=1024))
WITH (MEMORY_OPTIMIZED=ON)
GO
-- natively compiled stored procedure
CREATE PROCEDURE dbo.usp_ingest_table1
  @table1 dbo.tt_table1 READONLY
WITH NATIVE_COMPILATION, SCHEMABINDING
AS
BEGIN ATOMIC
    WITH (TRANSACTION ISOLATION LEVEL=SNAPSHOT,
          LANGUAGE=N'us_english')

  DECLARE @i INT = 1

  WHILE @i > 0
  BEGIN
    INSERT dbo.table1
    SELECT c2
    FROM @table1
    WHERE c1 = @i AND is_transient=0

    IF @@ROWCOUNT > 0
      SET @i += 1
    ELSE
    BEGIN
      INSERT dbo.temp_table1
      SELECT c2
      FROM @table1
      WHERE c1 = @i AND is_transient=1

      IF @@ROWCOUNT > 0
        SET @i += 1
      ELSE
        SET @i = 0
    END
  END

END
GO
-- sample execution of the proc
DECLARE @table1 dbo.tt_table1
INSERT @table1 (c2, is_transient) VALUES (N'sample durable', 0)
INSERT @table1 (c2, is_transient) VALUES (N'sample non-durable', 1)
EXECUTE dbo.usp_ingest_table1 @table1=@table1
SELECT c1, c2 from dbo.table1
SELECT c1, c2 from dbo.temp_table1
GO
```

## <a name="resources-to-learn-more"></a>可了解更多信息的资源

- [通过内存中 OLTP 技术加速 T-SQL 性能](./survey-of-initial-areas-in-in-memory-oltp.md)
- 有关使用内存中 OLTP 的性能演示，请参见： [in-memory-oltp-perf-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- [介绍和演示内存中 OLTP 的 17 分钟视频](in-memory-oltp-in-memory-optimization.md#anchorname-17minute-video)
- [Script to enable In-Memory OLTP and set recommended options](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/enable-in-memory-oltp.sql)
- [Main In-Memory OLTP documentation](in-memory-oltp-in-memory-optimization.md)
- [Azure SQL 数据库中的内存中 OLTP 的性能和资源使用率优势](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)
- [Improving temp table and table variable performance using memory optimization](/archive/blogs/sqlserverstorageengine/improving-temp-table-and-table-variable-performance-using-memory-optimization)
- [在 SQL 数据库中使用内存中技术优化性能](/azure/sql-database/sql-database-in-memory)
- [系统版本控制临时表与内存优化表](../tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
