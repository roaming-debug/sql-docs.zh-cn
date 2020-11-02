---
title: 性能中心
description: 查找有关 SQL Server 数据库引擎和 Azure SQL 数据库中的性能的必要信息。
ms.custom: seo-dt-2019
ms.date: 12/11/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- Performance (SQL Server)
- Performance (SQL Database)
helpviewer_keywords:
- SQL Server, performance
- performance (SQL Server)
- database performance (SQL Server)
- SQL Database (Performance)
- performance (SQL Database)
- database performance (SQL Database)
ms.assetid: 301204b2-140d-4495-98ed-021a9b5025f5
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: f12143e297b5cbf102afec030d19106ece71427a
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92678983"
---
# <a name="performance-center-for-sql-server-database-engine-and-azure-sql-database"></a>SQL Server 数据库引擎和 Azure SQL 数据库的性能中心
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本页提供的链接可帮助你找到有关 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 和 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]中的性能的必要信息。  
  
 **图例**  
  
 ![说明功能可用性图标的图例的屏幕截图。](../../relational-databases/performance/media/security-center-legend.PNG "security-center-legend")  
  
## <a name="configuration-options-for-performance"></a>性能的配置选项  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过许多 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 级别的配置选项，提供了可影响数据库引擎性能的功能。 通过 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]，Microsoft 可为你执行这些优化中的大多数（不是全部）。  
  
|||  
|-|-|  
|**磁盘配置选项**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [磁盘条带化和 RAID](https://technet.microsoft.com/library/ms190764\(v=sql.105\).aspx)|  
|**数据和日志文件配置选项**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [将数据和日志文件放到不同的驱动器上](../../relational-databases/policy-based-management/place-data-and-log-files-on-separate-drives.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [查看或更改数据文件和日志文件的默认位置 (SQL Server Management Studio)](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md)|  
|**TempDB 配置选项**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [TempDB 的性能改进](../databases/tempdb-database.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [数据库引擎配置 - TempDB](../../database-engine/install-windows/install-sql-server.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [在 Azure VM 中使用 SSD 来存储 SQL Server TempDB 和缓冲池扩展](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [Azure 虚拟机中 SQL Server 的临时磁盘的磁盘和性能最佳实践](/aspnet/core/performance/performance-best-practices)|  
|**服务器配置选项**|**处理器配置选项**<br /><br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [关联掩码 - 服务器配置选项](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [关联输入-输出掩码 - 服务器配置选项](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [affinity64 mask 服务器配置选项](../../database-engine/configure-windows/affinity64-mask-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [affinity64 I/O mask 服务器配置选项](../../database-engine/configure-windows/affinity64-input-output-mask-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [配置 max worker threads 服务器配置选项](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md)<br /><br />**内存配置选项**<br /><br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [服务器内存 - 服务器配置选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md)<br /><br />**索引配置选项**<br /><br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [配置填充因子服务器配置选项](../../database-engine/configure-windows/configure-the-fill-factor-server-configuration-option.md)<br /><br />**查询配置选项**<br /><br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [配置“每次查询占用的最小内存”服务器配置选项](../../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [配置查询调控器开销限制服务器配置选项](../../database-engine/configure-windows/configure-the-query-governor-cost-limit-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [配置并行的开销阈值服务器配置选项](../../database-engine/configure-windows/configure-the-cost-threshold-for-parallelism-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [“针对即席工作负荷进行优化”服务器配置选项](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md)<br /><br />**备份配置选项**<br /><br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [查看或配置 backup compression default 服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)|  
|**数据库配置优化选项**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [数据压缩](../../relational-databases/data-compression/data-compression.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: [查看或更改数据库的兼容级别](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)|  
|**表配置优化**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [已分区表和已分区索引](../../relational-databases/partitions/partitioned-tables-and-indexes.md)|  
|**Azure 虚拟机中的数据库引擎性能**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [快速检查列表](/aspnet/core/performance/performance-best-practices)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [虚拟机大小和存储帐户注意事项](/aspnet/core/performance/performance-best-practices)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [磁盘和性能注意事项](/aspnet/core/performance/performance-best-practices)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [I/O 性能注意事项](/aspnet/core/performance/performance-best-practices)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [功能特定的性能注意事项](/aspnet/core/performance/performance-best-practices)| 
|**性能最佳做法和 Linux 上的 SQL Server 的配置准则**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [SQL Server 配置](../../linux/sql-server-linux-performance-best-practices.md#sql-server-configuration)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [Linux OS 配置](../../linux/sql-server-linux-performance-best-practices.md#linux-os-configuration)|

> [!IMPORTANT]
> 其他注意事项请参阅：    
> -  [适用于具有高性能工作负载的 SQL Server 2012 和 SQL Server 2014 的推荐更新和配置选项](https://support.microsoft.com/help/2964518)
> -  [适用于具有高性能工作负载的 SQL Server 2017 和 SQL Server 2016 的推荐更新和配置选项](https://support.microsoft.com/help/4465518)

## <a name="query-performance-options"></a>查询性能选项  
  
|||  
|-|-|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[索引](../../relational-databases/indexes/indexes.md)**|[重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)<br />[为索引指定填充因子](../../relational-databases/indexes/specify-fill-factor-for-an-index.md)<br />[配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)<br />[用于索引的 SORT_IN_TEMPDB 选项](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md)<br />[改进全文索引的性能](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)<br />[配置“每次查询占用的最小内存”服务器配置选项](../../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md)<br />[配置 index create memory 服务器配置选项](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[已分区表和已分区索引](../../relational-databases/partitions/partitioned-tables-and-indexes.md)**|[分区的优点](../partitions/partitioned-tables-and-indexes.md)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[联接](../../relational-databases/performance/joins.md)**|[联接基础知识](../../relational-databases/performance/joins.md#fundamentals)<br />[嵌套循环联接](../../relational-databases/performance/joins.md#nested_loops)<br />[合并联接](../../relational-databases/performance/joins.md#merge)<br />[联接](../../relational-databases/performance/joins.md#hash)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[子查询](../../relational-databases/performance/subqueries.md)**|[子查询基础知识](../../relational-databases/performance/subqueries.md#fundamentals)<br />[相关子查询](../../relational-databases/performance/subqueries.md#correlated)<br />[子查询类型](../../relational-databases/performance/subqueries.md#types)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[存储过程](../stored-procedures/stored-procedures-database-engine.md)**|[CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md#best-practices)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[用户定义的函数](../user-defined-functions/user-defined-functions.md)**|[CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md#best-practices)<br />[创建用户定义函数（数据库引擎）](../user-defined-functions/create-user-defined-functions-database-engine.md)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **并行优化**|[配置 max worker threads 服务器配置选项](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md)<br />[ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **查询优化器优化**|[ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)<br />[USE HINT 查询提示](../../t-sql/queries/hints-transact-sql-query.md#use_hint)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[统计信息](../../relational-databases/statistics/statistics.md)**|[何时更新统计信息](../statistics/statistics.md)<br />[更新统计信息](../../relational-databases/statistics/update-statistics.md)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png":::  **[内存中 OLTP（内存中优化）](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)**|[Memory-Optimized Tables](../in-memory-oltp/sample-database-for-in-memory-oltp.md)<br />[本机编译的存储过程](../in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md)<br />[通过本机编译的存储过程创建和访问 TempDB 中的表](../../relational-databases/in-memory-oltp/create-and-access-tables-in-tempdb-from-stored-procedures.md)<br />[对内存优化哈希索引的常见性能问题进行故障排除](/previous-versions/sql/sql-server-2016/dn589805(v=sql.130))<br />[演示：内存中 OLTP 的性能改进](../../relational-databases/in-memory-oltp/demonstration-performance-improvement-of-in-memory-oltp.md)|
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png":::  **[智能查询处理](../../relational-databases/performance/intelligent-query-processing.md)**|[智能查询处理](../../relational-databases/performance/intelligent-query-processing.md)|
  
## <a name="see-also"></a>另请参阅  
 [监视和优化性能](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [相关视图、函数和过程](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [单一数据库的 Azure SQL 数据库性能指南](/azure/azure-sql/database/performance-guidance)   
 [使用弹性池优化 Azure SQL 数据库性能](/azure/azure-sql/database/elastic-pool-overview)   
 [适用于 Azure SQL 数据库的 Query Performance Insight](/azure/azure-sql/database/query-performance-insight-use)  
 [索引设计指南](../../relational-databases/sql-server-index-design-guide.md)  
 [内存管理体系结构指南](../../relational-databases/memory-management-architecture-guide.md)  
 [页和区体系结构指南](../../relational-databases/pages-and-extents-architecture-guide.md)  
 [迁移后验证和优化指南](../../relational-databases/post-migration-validation-and-optimization-guide.md)  
 [查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md)  
 [SQL Server 事务锁定和行版本控制指南](../sql-server-transaction-locking-and-row-versioning-guide.md)  
 [SQL Server 事务日志体系结构和管理指南](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)  
 [线程和任务体系结构指南](../../relational-databases/thread-and-task-architecture-guide.md) 
