---
title: SQL Server 2019 中的新增功能 | Microsoft Docs
description: 了解 SQL Server 2019 (15.x) 的新功能，该功能可让你选择开发语言、数据类型、环境和操作系统。
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: article
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: aadb59645e474557a92e3e87a0cd9c0c50df0b46
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271744"
---
# <a name="whats-new-in-sql-server-2019"></a>[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 的新增功能

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 在早期版本的基础上构建，旨在将 SQL Server 发展成一个平台，以提供开发语言、数据类型、本地或云环境以及操作系统选项。

本文总结了 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 的新功能和增强功能。

有关详细信息和已知问题，请参阅 [[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 发行说明](sql-server-version-15-release-notes.md)。

要获得 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 的最佳体验，请使用[最新工具](../azure-data-studio/download-azure-data-studio.md)。

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 为 [!INCLUDE[sql-server](../includes/ssnoversion-md.md)] 引入了 [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)]。 它还为 SQL Server 数据库引擎、SQL Server Analysis Services、SQL Server 机器学习服务、Linux 上的 SQL Server 和 SQL Server Master Data Services 提供了附加功能和改进。

下面的一个 13 分钟的视频提供了对 SQL Server 2019 的介绍：

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-SQL-Server-2019/player?WT.mc_id=dataexposed-c9-niner]


下面几个部分概述了这些功能。

## <a name="data-virtualization-and-big-data-clusters-2019"></a>数据虚拟化和 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]

当代企业通常掌管着庞大的数据资产，这些数据资产由托管在整个公司的孤立数据源中的各种不断增长的数据集组成。 利用 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]，你可以从所有数据中获得近乎实时的见解，该群集提供了一个完整的环境来处理包括机器学习和 AI 功能在内的大量数据。

| 新增功能或更新 | 详细信息 |
|:---|:---|
| 可缩放的大数据解决方案 | [部署](../big-data-cluster/deploy-get-started.md) SQL Server、Spark 和在 Kubernetes 上运行的 HDFS 容器的可缩放群集。 <br/><br/> 在 Transact-SQL 或 Spark 中读取、写入和处理大数据。<br/><br/> 通过大容量大数据轻松合并和分析高价值关系数据。<br/><br/>查询外部数据源。<br/><br/>在由 SQL Server 管理的 HDFS 中存储大数据。<br/><br/>通过群集查询多个外部数据源的数据。<br/><br/> 将数据用于 AI、机器学习和其他分析任务。<br/><br/> 在 [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] 中[部署和运行应用程序](../big-data-cluster/concept-application-deployment.md)。 <br/><br/> SQL Server 主实例使用 Always On 可用性组技术为所有数据库提供高可用性和灾难恢复。<br/>|
|通过 PolyBase 进行数据虚拟化 | 使用外部表从外部 SQL Server、Oracle、Teradata、MongoDB 和 ODBC 数据源查询数据，现在提供 [UTF-8 编码支持](../relational-databases/collations/collation-and-unicode-support.md)。 有关详细信息，请参阅[什么是 PolyBase？](../relational-databases/polybase/polybase-guide.md)。|
| &nbsp; | &nbsp; |

有关详细信息，请参阅[什么是 SQL Server [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)]](../big-data-cluster/big-data-cluster-overview.md)。

## <a name="intelligent-database"></a>智能数据库
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 在早期版本中的创新的基础上构建，旨在提供开箱即用的业界领先性能。 从[智能查询处理](../relational-databases/performance/intelligent-query-processing.md)到对永久性内存设备的支持，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 智能数据库功能提高了所有数据库工作负荷的性能和可伸缩性，而无需更改应用程序或数据库设计。

### <a name="intelligent-query-processing"></a>智能查询处理
通过[智能查询处理](../relational-databases/performance/intelligent-query-processing.md)，可以发现关键的并行工作负荷在大规模运行时，其性能得到了改进。 同时，它们仍可适应不断变化的数据世界。 默认情况下，最新的[数据库兼容性级别](../t-sql/statements/alter-database-transact-sql-compatibility-level.md#differences-between-compatibility-level-140-and-level-150)设置上支持智能查询处理，这会产生广泛影响，可通过最少的实现工作量改进现有工作负荷的性能。

|新增功能或更新 | 详细信息 |
|:---|:---|
|行模式内存授予反馈 |通过调整批处理模式和行模式运算符的内存授予大小，扩展了批处理模式内存授予反馈功能。 此调整可以自动纠正过度授予，过度授予会导致内存浪费和并发减少。 此调整还可纠正内存授予不足（会导致到磁盘的昂贵溢出）。 请参阅[行模式内存授予反馈](../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback)。 |
|行存储上的批处理模式 | 支持批处理模式执行，而无需使用列存储索引。 批处理模式执行在分析工作负荷期间更高效地使用 CPU，但低于 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 的版本中，只有当查询包含具有列存储索引的运算时才使用它。 然而，有些应用程序可能会使用列存储索引不支持的功能，因此无法利用批处理模式。 自 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 起，批处理模式在符合条件的分析工作负载上启用，这些工作负载的查询包含具有任何类型索引（行存储或列存储）的运算。 请参阅[行存储上的批处理模式](../relational-databases/performance/intelligent-query-processing.md#batch-mode-on-rowstore)。 |
|标量 UDF 内联|自动将标量 UDF 转换为关系表达式，并将它们嵌入调用 SQL 查询。 此转换提升了利用标量 UDF 的工作负载的性能。 请参阅[标量 UDF 内联](../relational-databases/performance/intelligent-query-processing.md#scalar-udf-inlining)。|
|表变量延迟编译|提升了引用表变量的查询的计划质量和整体性能。 在优化和初始编译期间，此功能传播基于实际表变量行计数的基数估计。 这种准确的行计数信息可优化下游计划操作。 请参阅[表变量延迟编译](../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation)。 |
|使用 `APPROX_COUNT_DISTINCT` 进行近似查询处理 |对于绝对精度不重要、但响应速度很关键的情况，`APPROX_COUNT_DISTINCT` 使用比 `COUNT(DISTINCT())` 更少的资源的同时跨大型数据集进行聚合，以实现高级并发。 请参阅[近似查询处理](../relational-databases/performance/intelligent-query-processing.md#approximate-query-processing)。|
| &nbsp; | &nbsp; |


### <a name="in-memory-database"></a>内存数据库
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [内存数据库](../relational-databases/in-memory-database.md)技术利用现代硬件创新提供无与伦比的性能和规模。 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 在此领域早期创新的基础上构建（例如内存中联机事务处理 (OLTP)），旨在为所有数据库工作负荷实现新的可伸缩性级别。  

|新增功能或更新 | 详细信息 |
|:---|:---|
|混合缓冲池| [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]的新功能，可以在需要时直接访问位于永久性内存 (PMEM) 设备上数据库文件中的数据库页。 请参阅[混合缓冲池](../database-engine/configure-windows/hybrid-buffer-pool.md)。|
|内存优化 TempDB 元数据| [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 引入了一个新功能，该功能属于[内存数据库](../relational-databases/in-memory-database.md)功能系列内存优化 TempDB 元数据，它可有效消除此瓶颈，并为 TempDB 繁重的工作负荷解锁新级别的可伸缩性。 在 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 中，管理临时表元数据时所涉及的系统表可以移动到无闩锁的非持久内存优化表中。 请参阅[内存优化 TempDB 元数据](../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata)。|
| 内存中 OLTP 对数据库快照的支持 | [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 引入了对创建包含内存优化文件组的数据库的[数据库快照](../relational-databases/databases/database-snapshots-sql-server.md)的支持。 |
| &nbsp; | &nbsp; |

### <a name="intelligent-performance"></a>智能性能
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 在早期版本的智能数据库创新的基础上构建，旨在确保[提高运行速度](/archive/blogs/bobsql/)。 这些改进有助于克服已知的资源瓶颈，并提供配置数据库服务器的选项，以在所有工作负荷中提供可预测性能。

|新增功能或更新 | 详细信息 |
|:---|:---|
|`OPTIMIZE_FOR_SEQUENTIAL_KEY`|在 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]内启用优化，有助于提高索引中高并发插入的吞吐量。 此选项旨在用于易发生最后一页插入争用的索引，此情况常见于有顺序键（如标识列、序列或日期/时间列）的索引。 请参阅[创建索引](../t-sql/statements/create-index-transact-sql.md#sequential-keys)。|
|强制快进和静态游标 | 提供对快速向前移动和静态游标的查询存储计划强制支持。 请参阅[计划强制支持快进和静态游标](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#ctp23)。|
|资源调控| `REQUEST_MAX_MEMORY_GRANT_PERCENT` 和 `ALTER WORKLOAD GROUP` 的 `CREATE WORKLOAD GROUP` 选项的可配置值已从整数更改为浮点数数据类型，以允许更精细地控制内存限制。 请参阅[修改工作负载组](../t-sql/statements/alter-workload-group-transact-sql.md)和[创建工作负载组](../t-sql/statements/create-workload-group-transact-sql.md)。|
|减少了对工作负荷的重新编译| 通过减少不必要的重新编译，改进了跨多个作用域使用临时表的性能。 请参阅[减少了对工作负荷的重新编译](../relational-databases/tables/tables.md#ctp23)。 |
|间接检查点可伸缩性 |请参阅[改进了间接检查点可伸缩性](../relational-databases/logs/database-checkpoints-sql-server.md#ctp23)。|
|并发 PFS 更新|[页可用空间 (PFS) 页](https://techcommunity.microsoft.com/t5/SQL-Server/Under-the-covers-GAM-SGAM-and-PFS-pages/ba-p/383125)是数据库文件中的特殊页面，SQL Server 用来在为对象分配空间时帮助定位可用空间。 PFS 页上的页闩锁争用通常与 [TempDB](https://support.microsoft.com/help/2154845/recommendations-to-reduce-allocation-contention-in-sql-server-tempdb-d) 关联，但当有许多并发对象分配线程时，也可能会在用户数据库上发生。 此改进改变了使用 PFS 更新来管理并发的方式，这样就能在共享闩锁（而不是排他闩锁）下更新它们。 自 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 起，此行为在所有数据库（包括 TempDB）中默认处于启用状态。|
|计划程序辅助角色迁移 |通过辅助角色迁移，空闲的计划程序可将辅助角色从同一 NUMA 节点上其他计划程序的可运行队列中迁移，并立即恢复已迁移的辅助角色的任务。 在将长时间运行的任务恰好分配给同一计划程序的情况下，此增强功能可提供更均衡的 CPU 使用率。 有关详细信息，请参阅 [SQL Server 2019 智能性能 - 辅助角色迁移](https://techcommunity.microsoft.com/t5/SQL-Server/SQL-Server-2019-Intelligent-Performance-Worker-Migration/ba-p/939610)。 |
| &nbsp; | &nbsp; |

### <a name="monitoring"></a>监视
监视改善情况可供你在需要时随时对任何数据库工作负荷解锁性能见解。

|新增功能或更新 | 详细信息 |
|:---|:---|
|`WAIT_ON_SYNC_STATISTICS_REFRESH` | `sys.dm_os_wait_stats` 动态管理视图中新的等待类型。 它显示了针对同步统计信息刷新操作耗费的实例级别累计时间。 请参阅 [`sys.dm_os_wait_stats`](../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)。|
|查询存储的自定义捕获策略 | 启用此策略后，在新的“查询存储捕获策略”设置下有额外可用的查询存储配置，可用于微调特定服务器中的数据收集。 请参阅 [ALTER DATABASE SET 选项](../t-sql/statements/alter-database-transact-sql-set-options.md)。|
|`LIGHTWEIGHT_QUERY_PROFILING`| 新数据库范围配置。 请参阅 [`LIGHTWEIGHT_QUERY_PROFILING`](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#lqp)。 |
|`sys.dm_exec_requests` 列 `command` | 如果 `SELECT` 在继续执行查询之前等待同步统计信息更新操作完成，则显示 `SELECT (STATMAN)`。 请参阅 [`sys.dm_exec_requests`](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)。|
|`sys.dm_exec_query_plan_stats` | 新的动态管理函数 (DMF)，可返回所有查询的最新已知实际执行计划的等效值。 请参阅 [sys.dm_exec_query_plan_stats](../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md)。|
|`LAST_QUERY_PLAN_STATS` | 新的数据库范围的配置，可启用 `sys.dm_exec_query_plan_stats`。 请参阅 [ALTER DATABASE SCOPED CONFIGURATION](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)。|
|`query_post_execution_plan_profile` | 扩展事件，基于轻型分析收集实际执行计划的等效项，与使用标准分析的 `query_post_execution_showplan` 不同。 请参阅[查询分析基础结构](../relational-databases/performance/query-profiling-infrastructure.md)。|
|`sys.dm_db_page_info(database_id, file_id, page_id, mode)` | 新的 DMF，返回有关数据库中页面的信息。 请参阅 [sys.dm_db_page_info (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md)。|
| &nbsp; | &nbsp; |

## <a name="developer-experience"></a>开发人员体验
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 继续提供一流的开发人员体验，并增强了图形和空间数据类型、UTF-8 支持以及新扩展性框架，该框架使开发人员可以使用他们选择的语言来获取其所有数据的见解。

### <a name="graph"></a>图形

|新增功能或更新 | 详细信息 |
|:---|:---|
|边缘约束级联删除操作 | 现可在图形数据库中，在边缘约束上定义级联删除操作。 请参阅[边缘约束](../relational-databases/tables/graph-edge-constraints.md)。 |
|新增图形函数 - `SHORTEST_PATH` | 现在可以使用 `MATCH` 内的 `SHORTEST_PATH` 来查找图中任意两个节点之间的最短路径，或执行任意长度遍历。|
|分区表和索引| 图形表现在支持表和索引分区。 |
|在图形匹配查询中使用派生表或视图别名 |请参阅[图形匹配查询](../t-sql/queries/match-sql-graph.md)。 |
| &nbsp; | &nbsp; |

### <a name="unicode-support"></a>Unicode 支持
支持不同国家/地区和区域的业务，其中提供全球多语言数据库应用程序和服务的要求对于满足客户需求和符合特定市场规范至关重要。 

|新增功能或更新 | 详细信息 |
|:---|:---|
|支持 UTF-8 字符编码 |支持使用 UTF-8 进行导入和导出编码，并用作字符串数据的数据库级别或列级别排序规则。 支持包括 PolyBase 外部表和 Always Encrypted（未用于 Enclave 时）。 请参阅[排序规则和 Unicode 支持](../relational-databases/collations/collation-and-unicode-support.md)。|
| &nbsp; | &nbsp; |

### <a name="language-extensions"></a>语言扩展

|新增功能或更新 | 详细信息 |
|:---|:---|
|新 Java 语言 SDK | 简化了可从 SQL Server 运行的 Java 程序的开发。 请参阅 [SQL Server 的用于 Java 的 Microsoft 扩展性 SDK](../language-extensions/how-to/extensibility-sdk-java-sql-server.md)。 |
|Java 语言 SDK 是开放源代码的 |[Microsoft SQL Server 用于 Java 的 Microsoft 扩展性 SDK](../language-extensions/how-to/extensibility-sdk-java-sql-server.md) 目前是开源的，可[在 GitHub 上获取](https://github.com/microsoft/sql-server-language-extensions)。|
|对 Java 数据类型的支持|请参阅 [Java 数据类型](../language-extensions/how-to/java-to-sql-data-types.md)。|
|新默认 Java 运行时 | SQL Server 现在包括 Azul System 的 Zulu Embedded，用于在整个产品中提供 Java 支持。 请参阅 [Free supported Java in SQL Server 2019 is now available](https://cloudblogs.microsoft.com/sqlserver/2019/07/24/free-supported-java-in-sql-server-2019-is-now-available/)（SQL Server 2019 现已提供免费支持的 Java）。 |
|SQL Server 语言扩展| 使用扩展性框架执行外部代码。 请参阅 [SQL Server 语言扩展](../language-extensions/language-extensions-overview.md)。
|注册外部语言|`CREATE EXTERNAL LANGUAGE` 是一种新的数据定义语言 (DDL)，可在 SQL Server 中注册外部语言（如 Java）。 请参阅 [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md)。 |
| &nbsp; | &nbsp; |

### <a name="spatial"></a>空间

|新增功能或更新 | 详细信息 |
|:---|:---|
| 新的空间引用标识符 (SRID) |[Australian GDA2020](http://www.ga.gov.au/scientific-topics/positioning-navigation/geodesy/datums-projections/gda2020) 提供了更为可靠和准确的数据，这些数据与全球定位系统提供的数据更加接近。 新 SRID 为：<ul><li>7843 表示地理 2D</li><li>7844 表示地理 3D</li></ul> 有关新 SRID 的定义，请参阅 [sys.spatial_reference_systems](../relational-databases/system-catalog-views/sys-spatial-reference-systems-transact-sql.md) 视图。 |
| &nbsp; | &nbsp; |

### <a name="error-messages"></a>错误消息
当提取、转换和加载 (ETL) 进程由于源和目标没有匹配的数据类型和/或长度而失败时，故障排除会很耗时，尤其是在大型数据集中。 通过 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 可更快速地深入了解数据截断错误。

|新增功能或更新 | 详细信息 |
|:---|:---|
|详细截断警告 | 数据截断错误消息默认包括表名和列名以及截断值。 请参阅 [VERBOSE_TRUNCATION_WARNINGS](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#verbose-truncation)。|
| &nbsp; | &nbsp; |

## <a name="mission-critical-security"></a>任务关键安全性
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供安全的体系结构，旨在使数据库管理员和开发人员能够创建安全的数据库应用程序并应对威胁。 每个版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 都在早期版本基础上进行了改进，并引入了新的特性和功能，[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 在此基础上继续进行构建。

|新增功能或更新 | 详细信息 |
|:---|:---|
|具有安全 Enclave 的 Always Encrypted|通过对服务器端安全隔离区中的纯文本数据启用计算，使用就地加密和丰富计算扩展 Always Encrypted。 就地加密可提高加密列、旋转列、加密密钥等加密操作的性能和可靠性，因为这样可以避免将数据移出数据库。<br><br> 对丰富计算（模式匹配和比较操作）的支持可将 Always Encrypted 解锁到一组更广泛的方案和应用程序，这些方案和应用程序需要敏感数据保护，同时还需要在 Transact-SQL 查询中使用更丰富的功能。 请参阅[包含安全 Enclave 的 Always Encrypted](../relational-databases/security/encryption/always-encrypted-enclaves.md)。|
|SQL Server 配置管理器中的证书管理|现在可以使用 SQL Server 配置管理器来执行查看和部署证书等证书管理任务。 请参阅[证书管理（SQL Server 配置管理器）](../database-engine/configure-windows/manage-certificates.md)。|
|数据发现和分类|数据发现和分类功能提供对用户表中的列进行分类和标记的功能。 对敏感数据（如商业、金融、医疗和 PII 等）进行分类，这在组织的信息保护中起到关键作用。 它可以充当基础结构，用于：<ul><li>帮助满足数据隐私标准和法规遵从性要求</li><li>各种安全方案，如监视（审核），以及对敏感数据异常访问的警报</li><li>可以更轻松地识别企业中敏感数据所在的位置，以便管理员采取保护数据库的正确措施</li></ul>|
|SQL Server 审核|另外，对[审核](../relational-databases/security/auditing/sql-server-audit-database-engine.md)进行了强化处理，在审核日志中包含了 `data_sensitivity_information` 新字段，其中包含查询返回的实际数据的敏感度分类（标签）。 有关详细信息和示例，请参阅 [`ADD SENSITIVITY CLASSIFICATION`](../t-sql/statements/add-sensitivity-classification-transact-sql.md)。|
| &nbsp; | &nbsp; |

## <a name="high-availability"></a>高可用性
每位用户在部署 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 时都需执行一项常见任务，即确保所有任务关键型 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例以及其中的数据库在企业和最终用户需要时随时可用。 可用性是 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 平台的关键支柱，并且 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 引入了许多新功能和增强功能，使企业能够确保其数据库环境高度可用。

### <a name="availability-groups"></a>可用性组

|新增功能或更新 | 详细信息 |
|:---|:---|
|最多五个同步副本|[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 将同步副本的最大数目从 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 中的 3 增加到了 5。 可以配置此组的 5 个副本在该组中进行自动故障转移。 有 1 个主要副本以及 4 个同步的次要副本。|
|次要副本到主要副本连接重定向| 允许客户端应用程序连接定向到主要副本，而不考虑在连接字符串中指定的目标服务器。 有关详细信息，请参阅[次要副本到主要副本读/写连接重定向（AlwaysOn 可用性组）](../database-engine/availability-groups/windows/secondary-replica-connection-redirection-always-on-availability-groups.md)。|
|HADR 权益| SQL Server 的每位软件保障客户都将能够对 Microsoft 仍支持的任何 SQL Server 版本使用三项增强权益。 有关详细信息，请参阅[此处的公告](https://cloudblogs.microsoft.com/sqlserver/2019/10/30/new-high-availability-and-disaster-recovery-benefits-for-sql-server/)。
| &nbsp; | &nbsp; |

### <a name="recovery"></a>恢复

|新增功能或更新 | 详细信息 |
|:---|:---|
|加速数据库恢复 | 通过加速数据库恢复 (ADR) 减少重启或长时间运行事务回滚后的恢复时间。 请参阅[加速数据库恢复](../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#adr)。|
| &nbsp; | &nbsp; |

### <a name="resumable-operations"></a>可恢复操作

|新增功能或更新 | 详细信息 |
|:---|:---|
|联机聚集列存储索引生成和重新生成 | 请参阅[联机执行索引操作](../relational-databases/indexes/perform-index-operations-online.md)。 |
|可恢复联机行存储索引生成 | 请参阅[联机执行索引操作](../relational-databases/indexes/perform-index-operations-online.md)。 |
|暂停和恢复透明数据加密 (TDE) 的初始扫描|请参阅[透明数据加密 (TDE) 扫描 - 暂停和继续](../relational-databases/security/encryption/transparent-data-encryption.md#scan-suspend-resume)。|
| &nbsp; | &nbsp; |

## <a name="platform-choice"></a>平台选择
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 在 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 中已引入的创新的基础上构建，旨在使你能够在所选平台上运行 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]，并获得比以往更多的功能和更高的安全性。

### <a name="linux"></a><a id="sql-server-on-linux"></a>Linux

| 新增功能或更新 | 详细信息 |
|:-----|:-----|
|复制支持 | 请参阅 [Linux 上的 SQL Server 复制](../linux/sql-server-linux-replication.md)。 |
|支持 Microsoft 分布式事务处理协调器 (MSDTC) | 请参阅[如何在 Linux 上配置 MSDTC](../linux/sql-server-linux-configure-msdtc.md)。 |
|OpenLDAP 支持第三方 AD 提供商 | 请参阅[教程：对 Linux 上的 SQL Server 使用 Active Directory 身份验证](../linux/sql-server-linux-active-directory-authentication.md)。 |
|Linux 上的机器学习服务 | 请参阅[在 Linux 上安装 SQL Server 机器学习服务（Python 和 R）](../linux/sql-server-linux-setup-machine-learning.md)。 |
|TempDB 改进 | 默认情况下，Linux 上的 SQL Server 新安装会根据逻辑核心数创建多个 TempDB 数据文件（最多 8 个数据文件）。 这不适用于就地次要版本或主版本升级。 每个 TempDB 文件的大小为 8 MB，且自动增长大小为 64 MB。 此行为类似于 Windows 上的默认 SQL Server 安装。 |
|Linux 上的 PolyBase | 请参阅在 Linux 上为非 Hadoop 连接器[安装 PolyBase](../relational-databases/polybase/polybase-linux-setup.md)。<br/><br/>请参阅 [PolyBase 类型映射](../relational-databases/polybase/polybase-type-mapping.md)。 |
| 变更数据捕获 (CDC) 支持 | Linux 上的 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 现在支持变更数据捕获 (CDC)。 |
| &nbsp; | &nbsp; |

### <a name="containers"></a>容器
开始使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的最简单方法是使用容器。 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 在早期版本中引入的创新的基础上构建，旨在使你能够以更安全的方式在新平台上部署 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 容器，并获得更多功能。

|新增功能或更新 | 详细信息 |
|:---|:---|
| Microsoft 容器注册表 | [Microsoft 容器注册表](https://azure.microsoft.com/blog/microsoft-syndicates-container-catalog/)现在将 Docker Hub 替换为新的官方 Microsoft 容器映像，其中包括 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]。 |
| 非根容器 | [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 引入了通过在默认情况下以非根用户身份启动 [!INCLUDE[sql-server](../includes/ssnoversion-md.md)] 进程来创建更安全容器的功能。 请参阅[以非根用户的身份构建并运行 SQL Server 容器](../linux/sql-server-linux-docker-container-deployment.md)。 |
| Red Hat 认证的容器映像 | 从 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 开始，可以在 Red Hat Enterprise Linux 上运行 SQL Server 容器。 |
| PolyBase 和机器学习支持| [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 引入了使用 SQL Server 容器的新方法，例如机器学习服务和 PolyBase。 查看[容器 GitHub 存储库中的 SQL Server](https://github.com/microsoft/mssql-docker/tree/master/linux/preview/examples) 中的一些示例。 |
| &nbsp; | &nbsp; |

## <a name="setup-options"></a>安装选项

|新增功能或更新 | 详细信息 |
|:---|:---| 
|新内存设置选项 | 在安装过程中设置“最小服务器内存(MB)”  和“最大服务器内存(MB)”  服务器配置。 请参阅[“数据库引擎配置 - 内存”页](./install/instance-configuration.md#memory)，以及[通过命令提示符安装 SQL Server](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Install) 中的 `USESQLRECOMMENDEDMEMORYLIMITS`、`SQLMINMEMORY` 和 `SQLMAXMEMORY` 参数。 建议值遵循[服务器内存配置选项](../database-engine/configure-windows/server-memory-server-configuration-options.md#manually)中的内存配置准则。| 
|新并行度设置选项 | 在安装过程中设置“最大并行度”  服务器配置。 有关详细信息，请参阅[“数据库引擎配置 - MaxDOP”页](./install/instance-configuration.md#maxdop)，以及[通过命令提示符安装 SQL Server](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Install) 中的 `SQLMAXDOP` 参数。 默认值遵循[配置服务器配置选项“最大并行度”](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines)中的最大并行度准则。| 
|服务器/CAL 许可证产品密钥的设置警告|如果输入了企业服务器/CAL 许可证产品密钥，且计算机上有 20 多个物理内核，或者在启用超线程时有 40 个逻辑内核，则安装过程中会显示警告。 用户仍然可以确认限制并继续设置，或者输入支持操作系统最大处理器数量的许可证密钥。|
| &nbsp; | &nbsp; |

## <a name="sql-server-machine-learning-services"></a><a id="ml"></a>SQL Server 机器学习服务

|新增功能或更新 | 详细信息 |
|:---|:---|
|基于分区的建模 | 可以使用添加到 `sp_execute_external_script` 的新参数来处理每个数据分区的外部脚本。 此功能支持训练多个小型模型（每个数据分区一个模型）而不是一个大型模型。 请参阅[创建基于分区的模型](../machine-learning/tutorials/r-tutorial-create-models-per-partition.md)。|
|Windows Server 故障转移群集| 可在 Windows Server 故障转移群集上配置机器学习服务的高可用性。|
| &nbsp; | &nbsp; |

## <a name="sql-server-analysis-services"></a>SQL Server Analysis Services

此版本引入了新功能和针对性能、资源管理和客户端支持的改进。

| 新增功能或更新 | 详细信息 |
|:---|:---|
|表格模型中的计算组| 通过将常见度量值表达式分组为“计算项”，计算组可显著减少冗余度量值的数量  。 要了解详细信息，请参阅[表格模型中的计算组](/analysis-services/tabular-models/calculation-groups)。 |
|查询交叉| 查询交叉是一种表格模型系统配置，可在高并发情况下改善用户查询响应时间。 要了解详细信息，请参阅[查询交叉](/analysis-services/tabular-models/query-interleaving)。 |
|表格模型中的多对多关系| 允许表之间存在多对多关系，两个表中的列都是非唯一的。 有关详细信息，请参阅[表格模型中的关系](/analysis-services/tabular-models/relationships-ssas-tabular)。|
|资源管理的属性设置| 此版本包含新的内存设置：针对资源管理的 Memory\QueryMemoryLimit、DbpropMsmdRequestMemoryLimit 和 OLAP\Query\RowsetSerializationLimit。 要了解详细信息，请参阅[内存设置](/analysis-services/server-properties/memory-properties)。|
|Power BI 缓存刷新的调控设置 | 此版本引入了 ClientCacheRefreshPolicy 属性，该属性将替代缓存的仪表板磁贴数据以及 Power BI 服务初始加载 Live Connect 报表时的报表数据。 有关详细信息，请参阅[常规属性](/analysis-services/server-properties/general-properties)。 |
| 联机附加  | 联机附加可用于本地查询横向扩展环境中只读副本的同步。 要了解详细信息，请参阅[联机附加](/analysis-services/what-s-new-in-sql-server-analysis-services#online-attach)。 |
| &nbsp; | &nbsp; |

## <a name="sql-server-integration-services"></a>SQL Server Integration Services

此版本引入了改进文件操作的新功能。

| 新增功能或更新 | 详细信息 |
|:---|:---|
|灵活的文件任务 |在本地文件系统、Azure Blob 存储和 Azure Data Lake Storage Gen2 上执行文件操作。 查看[灵活的文件任务](../integration-services/control-flow/flexible-file-task.md)。|
|灵活的文件源和目标 |对 Azure Blob 存储和 Azure Data Lake Storage Gen2 读写数据。 请参阅[灵活的文件源](../integration-services/data-flow/flexible-file-source.md)和[灵活的文件目标](../integration-services/data-flow/flexible-file-destination.md)。 |

## <a name="sql-server-master-data-services"></a>SQL Server [!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)]

| 新增功能或更新 | 详细信息 |
|:---|:---|
|支持 Azure SQL 托管实例数据库| 在 Azure SQL 托管实例上托管 [!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)]。 请参阅[[!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)]安装和配置](../master-data-services/master-data-services-installation-and-configuration.md#SetUpWeb)。|
|新 HTML 控件| HTML 控件替换了所有以前的 Silverlight 组件。 已删除 Silverlight 依赖项。|
| &nbsp; | &nbsp; |

## <a name="sql-server-reporting-services"></a>SQL Server Reporting Services

此版本的 SQL Server Reporting Services 功能支持 Azure SQL 托管实例、Power BI Premium 数据集、增强的可访问性、Azure Active Directory 应用程序代理以及透明数据库加密。 它还会更新 Microsoft 报表生成器。 有关详细信息，请参阅 [SQL Server Reporting Services 中的新增功能](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md)。

## <a name="see-also"></a>另请参阅

- [`SqlServer` PowerShell 模块](https://www.powershellgallery.com/packages/Sqlserver)
- [SQL Server PowerShell 文档](../powershell/sql-server-powershell.md)

## <a name="next-steps"></a>后续步骤

- [SQL Server 研讨会](https://aka.ms/sqlworkshops)
- [[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 发行说明](sql-server-version-15-release-notes.md)
- [Microsoft [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]：技术白皮书](https://aka.ms/sql2019whitepaper)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
