---
title: tempdb 数据库 | Microsoft Docs
description: 本主题提供有关在 SQL Server 和 Azure SQL 数据库中配置和使用 tempdb 数据库的详细信息。
ms.custom: P360
ms.date: 09/16/2020
ms.prod: sql
ms.prod_service: database-engine
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- temporary tables [SQL Server], tempdb database
- tempdb database [SQL Server], about tempdb
- temporary stored procedures [SQL Server]
- tempdb database [SQL Server]
ms.assetid: ce4053fb-e37a-4851-b711-8e504059a780
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0fa90cc172c07b7642ca937271fe8c6709b65ede
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489371"
---
# <a name="tempdb-database"></a>TempDB 数据库

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

`tempdb` 系统数据库是一个全局资源，可供连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例或 Azure SQL 数据库的所有用户使用。 `tempdb` 保留：  
  
- 显式创建的临时用户对象。 它们包括全局或局部临时表及索引、临时存储过程、表变量、表值函数返回的表或游标。  
- 数据库引擎创建的内部对象。 它们包括：
  - 用于储存假脱机、游标、排序和临时大型对象 (LOB) 存储的中间结果的工作表。
  - 用于哈希联接或哈希聚合操作的工作文件。
  - 用于创建或重新生成索引等操作（如果指定了 `SORT_IN_TEMPDB`）的中间排序结果，或者某些 `GROUP BY`、`ORDER BY` 或 `UNION` 查询的中间排序结果。

  每个内部对象至少使用九页：一个 IAM 页，一个八页的盘区。 有关页和盘区的详细信息，请参阅[页和盘区](../../relational-databases/pages-and-extents-architecture-guide.md#pages-and-extents)。

  > [!IMPORTANT]
  > Azure SQL 数据库单一数据库和弹性池支持存储在 `tempdb` 中并且范围为数据库级别的全局临时表和全局临时存储过程。 
  >
  > 全局临时表和全局临时存储过程供同一 SQL 数据库中的所有用户会话共享。 其他 SQL 数据库中的用户会话无法访问全局临时表。 有关详细信息，请参阅[数据库作用域内全局临时表（Azure SQL 数据库）](../../t-sql/statements/create-table-transact-sql.md#database-scoped-global-temporary-tables-azure-sql-database)。 [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)与 SQL Server 支持相同的临时对象。
  >
  > 对于 Azure SQL 数据库单一数据库和弹性池，仅 master 数据库和 `tempdb` 数据库适用。 有关详细信息，请参阅[什么是 Azure SQL 数据库服务器？](/azure/sql-database/sql-database-servers-databases#what-is-an-azure-sql-database-server) 有关 Azure SQL 数据库单一数据库和弹性池上下文中 `tempdb` 的讨论，请参阅 [Azure SQL 数据库单一数据库和弹性池中的 tempdb 数据库](#tempdb-database-in-sql-database)。 
  >
  > 对于 Azure SQL 托管实例，所有系统数据库都适用。

- 版本存储区是数据页的集合，它包含支持用于行版本控制的功能的数据行。 有两种类型：公用版本存储区和联机索引生成版本存储区。 版本存储区包含：
  - 由通过行版本控制隔离或快照隔离事务使用 `READ COMMITTED` 的数据库中的数据修改事务生成的行版本。  
  - 由数据修改事务为实现联机索引操作、多重活动结果集 (MARS) 以及 `AFTER` 触发器等功能而生成的行版本。  
  
`tempdb` 中的操作是最小日志记录操作，以便回滚事务。 每次启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时都会重新创建 `tempdb`，从而在系统启动时总是具有一个干净的数据库副本。 在断开联接时会自动删除临时表和存储过程，并且在系统关闭后没有活动连接。 

`tempdb` 不会有什么内容从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的一个会话保存到另一个会话。 不允许对 `tempdb` 执行备份和还原操作。  

## <a name="physical-properties-of-tempdb-in-sql-server"></a>SQL Server 中 tempdb 的物理属性

下表列出了 SQL Server 中 `tempdb` 数据和日志文件的初始配置值。 这些值基于 `model` 数据库的默认值。 对于不同版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，这些文件的大小可能略有不同。  
  
|文件|逻辑名称|物理名称|初始大小|文件增长|  
|----------|------------------|-------------------|------------------|-----------------|  
|主数据|tempdev|tempdb.mdf|8 MB|以 64 MB 的速度自动增长直到磁盘已满|  
|次要数据文件|temp#|tempdb_mssql_#.ndf|8 MB|以 64 MB 的速度自动增长直到磁盘已满|  
|日志|templog|templog.ldf|8 MB|以 64 MB 的速度自动增长直到达到上限 2 TB|  
  
辅助数据文件数取决于计算机上的（逻辑）处理器数。 一般而言，如果逻辑处理器数目小于或等于 8，则使用的数据文件数与逻辑处理器数相同。 如果逻辑处理器数大于 8，请指定 8 个数据文件。 如果仍然存在争用，则以 4 的倍数增加数据文件的数量，直到争用减少到可接受的级别或对工作负荷/代码进行更改。

> [!NOTE]
> 数据文件数的默认值遵循 [KB 2154845](https://support.microsoft.com/kb/2154845/)中的一般准则。  

> [!NOTE]
> 要检查 `tempdb` 的当前大小和增长参数，请查询视图 `tempdb.sys.database_files`。
  
### <a name="moving-the-tempdb-data-and-log-files-in-sql-server"></a>在 SQL Server 中移动 tempdb 数据和日志文件

若要移动 `tempdb` 数据和日志文件，请参阅[移动系统数据库](../../relational-databases/databases/move-system-databases.md)。  
  
### <a name="database-options-for-tempdb-in-sql-server"></a>SQL Server 中 tempdb 的数据库选项

下表列出了 `tempdb` 数据库中每个数据库选项的默认值以及该选项是否可以修改。 若要查看这些选项的当前设置，请使用 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图。  
  
|数据库选项|默认值|是否可修改|  
|---------------------|-------------------|---------------------|  
|ALLOW_SNAPSHOT_ISOLATION|OFF|是|  
|ANSI_NULL_DEFAULT|OFF|是|  
|ANSI_NULLS|OFF|是|  
|ANSI_PADDING|OFF|是|  
|ANSI_WARNINGS|OFF|是|  
|ARITHABORT|OFF|是|  
|AUTO_CLOSE|OFF|否|  
|AUTO_CREATE_STATISTICS|ON|是|  
|AUTO_SHRINK|OFF|否|  
|AUTO_UPDATE_STATISTICS|ON|是|  
|AUTO_UPDATE_STATISTICS_ASYNC|OFF|是|  
|CHANGE_TRACKING|OFF|否|  
|CONCAT_NULL_YIELDS_NULL|OFF|是|  
|CURSOR_CLOSE_ON_COMMIT|OFF|是|  
|CURSOR_DEFAULT|GLOBAL|是|  
|数据库可用性选项|ONLINE<br /><br /> MULTI_USER<br /><br /> READ_WRITE|否<br /><br /> 否<br /><br /> 否|  
|DATE_CORRELATION_OPTIMIZATION|OFF|是|  
|DB_CHAINING|ON|否|  
|ENCRYPTION|OFF|否|  
|MIXED_PAGE_ALLOCATION|OFF|否|  
|NUMERIC_ROUNDABORT|OFF|是|  
|PAGE_VERIFY|对于新安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，为 CHECKSUM<br /><br /> 对于升级的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，为 NONE|是|  
|PARAMETERIZATION|SIMPLE|是|  
|QUOTED_IDENTIFIER|OFF|是|  
|READ_COMMITTED_SNAPSHOT|OFF|否|  
|RECOVERY|SIMPLE|否|  
|RECURSIVE_TRIGGERS|OFF|是|  
|Service Broker 选项|ENABLE_BROKER|是|  
|TRUSTWORTHY|OFF|否|  
  
有关这些数据库选项的说明，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。  
  
## <a name="tempdb-database-in-sql-database"></a>SQL 数据库中的 tempdb 数据库

### <a name="tempdb-sizes-for-dtu-based-service-tiers"></a>基于 DTU 的服务层的 tempdb 大小

<!-- tempdb being larger for Basic and 50 eDTU pools than for 100-400 eDTU pools reflects actual config (historical reasons) --> 

|服务级别目标|最大 `tempdb` 数据文件大小 (GB)|`tempdb` 数据文件数|最大 `tempdb` 数据大小 (GB)|
|---|---:|---:|---:|
|基本|13.9|1|13.9|
|S0|13.9|1|13.9|
|S1|13.9|1|13.9|
|S2|13.9|1|13.9|
|S3|32|1|32
|S4|32|2|64|
|S6|32|3|96|
|S7|32|6|192|
|S9|32|12|384|
|S12|32|12|384|
|P1|13.9|12|166.7|
|P2|13.9|12|166.7|
|P4|13.9|12|166.7|
|P6|13.9|12|166.7|
|P11|13.9|12|166.7|
|P15|13.9|12|166.7|
|基本弹性池（所有 DTU 配置）|13.9|12|166.7|
|标准弹性池 (50 eDTU)|13.9|12|166.7|
|标准弹性池 (100 eDTU)|32|1|32|
|标准弹性池 (200 eDTU)|32|2|64|
|标准弹性池 (300 eDTU)|32|3|96|
|标准弹性池 (400 eDTU)|32|3|96|
|标准弹性池 (800 eDTU)|32|6|192|
|标准弹性池 (1200 eDTU)|32|10|320|
|标准弹性池 (1600-3000 eDTU)|32|12|384|
|高级弹性池（所有 DTU 配置）|13.9|12|166.7|
||||

### <a name="tempdb-sizes-for-vcore-based-service-tiers"></a>基于 vCore 的服务层的 tempdb 大小

请参阅[基于 vCore 的资源限制](/azure/sql-database/sql-database-vcore-resource-limits)。

## <a name="restrictions"></a>限制

不能在 `tempdb` 数据库中执行下列操作：  
  
- 添加文件组。
- 备份或还原数据库。
- 更改排序规则。 默认排序规则为服务器排序规则。
- 更改数据库所有者。 `tempdb` 的所有者是 sa。
- 创建数据库快照。
- 删除数据库。
- 从数据库中删除 *guest* 用户。
- 启用变更数据捕获。
- 参与数据库镜像。
- 删除主文件组、主数据文件或日志文件。
- 重命名数据库或主文件组。
- 正在运行 `DBCC CHECKALLOC`。
- 正在运行 `DBCC CHECKCATALOG`。
- 将数据库设置为 `OFFLINE`。
- 将数据库或主文件组设置为 `READ_ONLY`。
  
## <a name="permissions"></a>权限

任何用户都可以在 `tempdb` 中创建临时对象。 用户只能访问自己的对象，除非他们获得更多的权限。 可以撤销对 `tempdb` 的连接权限以阻止用户使用 `tempdb`。 我们不建议这样做，因为一些例程操作需要使用 `tempdb`。  

## <a name="optimizing-tempdb-performance-in-sql-server"></a>在 SQL Server 中优化 tempdb 性能
`tempdb` 数据库的大小和物理位置可能会影响系统的性能。 例如，如果为 `tempdb` 定义的大小过小，则每次重启 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，都可能会占用部分系统处理负荷，以使 `tempdb` 自动增长到支持工作负荷所需的大小。

如果可以，请使用[即时文件初始化](../../relational-databases/databases/database-instant-file-initialization.md)来提高数据文件增长操作的性能。

通过将文件大小设置为足够容纳环境中典型工作负载的值来预分配所有 `tempdb` 文件的空间。 预先分配可避免 `tempdb` 因扩展得过于频繁而影响性能。 `tempdb` 数据库应设置为自动增长，以便在出现意外情况时增加磁盘空间。

每个[文件组](../../relational-databases/databases/database-files-and-filegroups.md#filegroups)中的数据文件应大小一致，因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用比例填充算法，这种算法可增加可用空间，便于文件分配。 将 `tempdb` 分割成大小相等的多个数据文件，可以为使用 `tempdb` 的操作提供更高的并行效率。

将文件增量设置为合理的大小以免 `tempdb` 数据库文件的增量过小。 如果文件的增量与写入 `tempdb` 的数据量相比过小，则 `tempdb` 可能需要不断扩大。 这将影响性能。

要检查 `tempdb` 的当前大小和增长参数，请使用以下查询：

```sql
 SELECT name AS FileName,
    size*1.0/128 AS FileSizeInMB,
    CASE max_size
        WHEN 0 THEN 'Autogrowth is off.'
        WHEN -1 THEN 'Autogrowth is on.'
        ELSE 'Log file grows to a maximum size of 2 TB.'
    END,
    growth AS 'GrowthValue',
    'GrowthIncrement' =
        CASE
            WHEN growth = 0 THEN 'Size is fixed.'
            WHEN growth > 0 AND is_percent_growth = 0
                THEN 'Growth value is in 8-KB pages.'
            ELSE 'Growth value is a percentage.'
        END
FROM tempdb.sys.database_files;
GO
```

将 `tempdb` 数据库放置在快速 I/O 子系统中。 如果有许多直接连接的磁盘，则请使用磁盘条带化。 单个或成组的 `tempdb` 数据文件并不一定要位于不同的磁盘或主轴上，除非存在 I/O 瓶颈。

将 `tempdb` 数据库放置在用户数据库使用的磁盘以外的磁盘中。

## <a name="performance-improvements-in-tempdb-for-sql-server"></a>SQL Server 中 tempdb 的性能提高
从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始，已通过以下方式进一步优化 `tempdb` 性能：  
  
- 已缓存的临时表和表变量。 缓存允许删除和创建临时对象的操作非常快速地运行。 缓存还可以减少页分配和元数据争用问题。  
- 改进了分配页闩锁协议，减少了所用 `UP`（更新）闩锁的数量。  
- 减少了 `tempdb` 的日志记录开销，从而减少了 `tempdb` 日志文件的磁盘 I/O 带宽消耗。  
- 在新的实例安装过程中，安装程序会添加多个 `tempdb` 数据文件。 可以使用“数据库引擎配置”部分中新增的 UI 输入控件和命令行参数 `/SQLTEMPDBFILECOUNT` 来完成此任务。 默认情况下，安装程序添加的 `tempdb` 数据文件数为逻辑处理器计数或 8，以较小者为准。  
- 如果有多个 `tempdb` 数据文件，那么所有文件都会同时自动增长相同的量，具体取决于增长设置。 不再需要[跟踪标志 1117](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。  
- `tempdb` 中的所有分配使用统一盘区。 不再需要[跟踪标志 1118](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。  
- 对于主文件组，`AUTOGROW_ALL_FILES` 属性已启用，且不能修改此属性。

有关 `tempdb` 中性能改进的详细信息，请参阅博客文章 [TEMPDB - Files and Trace Flags and Updates, Oh My!](/archive/blogs/sql_server_team/tempdb-files-and-trace-flags-and-updates-oh-my)（TEMPDB - 文件和跟踪标志以及更新，天哪！）。

## <a name="memory-optimized-tempdb-metadata"></a>内存优化 tempdb 元数据
对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上运行的许多工作负载，`tempdb` 元数据争用历来是可伸缩性的瓶颈。 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 引入了一项新功能，它属于[内存数据库](../in-memory-database.md)功能系列：内存优化 tempdb 元数据。 

此功能有效地消除了这种瓶颈，并为 tempdb 繁重的工作负荷提供了新级别的可伸缩性。 在 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 中，管理临时表元数据时所涉及的系统表可以移动到无闩锁的非持久内存优化表中。

本视频时长 7 分钟，请观看它来大致了解如何及何时使用经过内存优化的 tempdb 元数据：

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/How-and-When-To-Memory-Optimized-TempDB-Metadata/player?WT.mc_id=dataexposed-c9-niner]


### <a name="configuring-and-using-memory-optimized-tempdb-metadata"></a>配置和使用内存优化 tempdb 元数据

要选择加入此新功能，请使用以下脚本：

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON;
```

此配置更改需要重新启动服务才能生效。

可使用以下 T-SQL 命令验证 `tempdb` 是否经过内存优化：

```sql
SELECT SERVERPROPERTY('IsTempdbMetadataMemoryOptimized');
```

如果启用内存优化的 `tempdb` 元数据后，服务器因任何原因未能启动，则可以通过 -f 启动选项以[最小配置](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md)启动 SQL Server 实例，从而绕过该功能。 然后，你可以禁用该功能，并在正常模式下重启 SQL Server。

若要防止服务器可能出现内存不足的情况，可以将 `tempdb` 绑定到[资源池](../in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md)。 这是通过 [`ALTER SERVER`](../../t-sql/statements/alter-server-configuration-transact-sql.md) 命令（而不是将资源池绑定到数据库时通常遵循的步骤）完成的。

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON (RESOURCE_POOL = 'pool_name');
```

此更改还需要重新启动才能生效，即使已启用内存优化 tempdb 元数据也是如此。

### <a name="memory-optimized-tempdb-limitations"></a>内存优化 tempdb 限制

- 该功能的打开和关闭不是动态的。 由于需要对 `tempdb` 结构进行内部更改，因此需要重新启动才能启用或禁用该功能。

- 单个事务无法访问多个数据库中的内存优化表。 涉及用户数据库中内存优化表的任何事务都无法访问同一事务中的 `tempdb` 系统视图。 如果尝试在与用户数据库中内存优化表相同的事务中访问 `tempdb` 系统视图，将收到以下错误：
    
  ```
  A user transaction that accesses memory optimized tables or natively compiled modules cannot access more than one user database or databases model and msdb, and it cannot write to master.
  ```
    
  示例：
    
  ```sql
  BEGIN TRAN;
  
  SELECT *
  FROM tempdb.sys.tables;  -----> Creates a user in-memory OLTP transaction in tempdb
  
  INSERT INTO <user database>.<schema>.<mem-optimized table>
  VALUES (1); ----> Tries to create a user in-memory OLTP transaction in the user database but will fail
  
  COMMIT TRAN;
  ```
    
- 针对内存优化表的查询不支持锁定和隔离提示，因此针对内存优化 `tempdb` 目录视图的查询将不会遵循锁定和隔离提示。 与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的其他系统目录视图一样，针对系统视图的所有事务都将处于 `READ COMMITTED`（或在本例中为 `READ COMMITTED SNAPSHOT`）隔离。

- 如果启用内存优化的 `tempdb` 元数据，则无法在临时表上创建[列存储索引](../indexes/columnstore-indexes-overview.md)。

- 由于对列存储索引的限制，启用内存优化的 `tempdb` 元数据时，不支持将 `sp_estimate_data_compression_savings` 系统存储过程与 `COLUMNSTORE` 或 `COLUMNSTORE_ARCHIVE` 数据压缩参数一起使用。

> [!NOTE] 
> 仅当引用 `tempdb` 系统视图时，这些限制才适用。 如果需要，可以在用户数据库中访问内存优化表时，在同一个事务中创建一个临时表。

## <a name="capacity-planning-for-tempdb-in-sql-server"></a>SQL Server 中的 tempdb 容量计划
确定 `tempdb` 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生产环境中的适当大小取决于多种因素。 如前文所述，这些因素包括现有工作负荷以及使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。 建议你通过在 SQL Server 测试环境中执行下列任务来分析现有的工作负荷：

- 设置 `tempdb` 的自动增长。
- 运行单独的查询或工作负荷跟踪文件，并监视 `tempdb` 空间使用情况。
- 执行索引维护操作（例如重新生成索引），并监视 `tempdb` 空间。
- 使用前面步骤中的空间使用值来预测工作负荷总使用量。 为计划的并发活动调整此值，然后相应地设置 `tempdb` 的大小。

## <a name="monitoring-tempdb-use"></a>监视 tempdb 的使用情况
`tempdb` 中的磁盘空间不足可能会导致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生产环境中出现严重中断。 它还可能会阻止正在运行的应用程序完成操作。 可以使用 [sys.dm_db_file_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md) 动态管理视图来监视 `tempdb` 文件中使用的磁盘空间：

```sql
 -- Determining the amount of free space in tempdb
SELECT SUM(unallocated_extent_page_count) AS [free pages],
  (SUM(unallocated_extent_page_count)*1.0/128) AS [free space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by the version store
SELECT SUM(version_store_reserved_page_count) AS [version store pages used],
  (SUM(version_store_reserved_page_count)*1.0/128) AS [version store space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by internal objects
SELECT SUM(internal_object_reserved_page_count) AS [internal object pages used],
  (SUM(internal_object_reserved_page_count)*1.0/128) AS [internal object space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by user objects
SELECT SUM(user_object_reserved_page_count) AS [user object pages used],
  (SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]
FROM sys.dm_db_file_space_usage;
 ```

若要在会话级或任务级监视 `tempdb` 中的页分配或页释放活动，可以使用 [sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md) 和 [sys.dm_db_task_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md) 动态管理视图。 这些视图有助于标识使用 `tempdb` 中大量磁盘空间的大型查询、临时表或表变量。 还可使用若干个计数器来监视 `tempdb` 中的可用空间以及正在使用 `tempdb` 的资源。

```sql
-- Obtaining the space consumed by internal objects in all currently running tasks in each session
SELECT session_id,
  SUM(internal_objects_alloc_page_count) AS task_internal_objects_alloc_page_count,
  SUM(internal_objects_dealloc_page_count) AS task_internal_objects_dealloc_page_count
FROM sys.dm_db_task_space_usage
GROUP BY session_id;

-- Obtaining the space consumed by internal objects in the current session for both running and completed tasks
SELECT R2.session_id,
  R1.internal_objects_alloc_page_count
  + SUM(R2.internal_objects_alloc_page_count) AS session_internal_objects_alloc_page_count,
  R1.internal_objects_dealloc_page_count
  + SUM(R2.internal_objects_dealloc_page_count) AS session_internal_objects_dealloc_page_count
FROM sys.dm_db_session_space_usage AS R1
INNER JOIN sys.dm_db_task_space_usage AS R2 ON R1.session_id = R2.session_id
GROUP BY R2.session_id, R1.internal_objects_alloc_page_count,
  R1.internal_objects_dealloc_page_count;;
```

## <a name="related-content"></a>相关内容
[用于索引的 SORT_IN_TEMPDB 选项](../../relational-databases/indexes/sort-in-TempDB-option-for-indexes.md)    
[系统数据库](../../relational-databases/databases/system-databases.md)    
[sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)    
[sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)    
[移动数据库文件](../../relational-databases/databases/move-database-files.md)    
