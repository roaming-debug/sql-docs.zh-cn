---
title: 卸载次要可用性组副本的工作负荷
description: 了解如何在 SQL Server 上卸载对 AlwaysOn 可用性组的次要副本的只读查询和报表。
ms.custom: seo-lt-2019
ms.date: 06/06/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- readable secondary replicas
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 78f3f81a-066a-4fff-b023-7725ff874fdf
author: cawrites
ms.author: chadam
ms.openlocfilehash: 82a8d9f4e787fd419e31e637775e33a4cf71f36d
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584859"
---
# <a name="offload-read-only-workload-to-secondary-replica-of-an-always-on-availability-group"></a>卸载对 AlwaysOn 可用性组的次要副本的只读工作负荷
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 活动辅助功能包括支持对一个或多个次要副本的只读访问（可读次要副本）。 可读辅助副本可以处于异步提交可用性模式或同步提交可用性模式。 可读辅助副本允许对其所有辅助数据库的只读访问。 但是，可读辅助数据库并非设置为只读。 它们是动态的。 当对相应主数据库的更改应用到某一给定的辅助数据库时，该辅助数据库将更改。 对于典型的辅助副本，包括持久内存优化表，辅助数据库中的数据接近实时。 此外，全文检索与辅助数据库同步。 在许多情况下，主数据库和相应的辅助数据库之间的数据滞后时间只有几秒钟。  
  
 在主数据库中进行的安全设置会对辅助数据库永久保留。 这包括用户、数据库角色和应用程序角色及其各自的权限；如果对主数据库启用了透明数据加密 (TDE)，还将包括 TDE。  
  
> [!NOTE]  
>  尽管您无法将数据写入辅助数据库，但可以在承载辅助副本的服务器实例上写入读写数据库，包括用户数据库和 **tempdb** 之类的系统数据库。  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 还支持对可读辅助副本（*只读路由*）的读意向连接请求的重新路由。 有关只读路由的详细信息，请参阅 [使用侦听程序连接到只读次要副本（只读路由）](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md#ConnectToSecondary)。  
  
##  <a name="benefits"></a><a name="bkmk_Benefits"></a> 优势  
 指定与可读辅助副本的只读连接具有以下优点：  
  
-   从主副本卸下辅助副本只读工作负荷，以便将资源用于关键任务工作负荷。 如果具有关键任务读工作负荷或不能滞后的工作负荷，应在主副本上运行它。  
  
-   提高承载可读辅助副本的系统的投资回报率。  
  
 此外，可读辅助副本对只读操作提供稳定的支持，如下所示：  
  
-   有关可读辅助数据库的自动临时统计信息可优化对基于磁盘的表的只读查询。 对于内存优化表，将自动创建缺少的统计信息。 但不会自动更新旧统计信息。 您需要手动更新主副本的统计信息。 有关详细信息，请参阅本主题后面的 [只读访问数据库的统计信息](#Read-OnlyStats)。  
  
-   基于磁盘的表的只读工作负荷使用行版本控制来消除辅助数据库上的阻塞争用。 对辅助数据库运行的所有查询都自动映射到快照隔离事务级别，即使在显式设置了其他事务隔离级别时也是如此。 此外，所有锁定提示都将被忽略。 这消除了读取器/编写器的争用。  
  
-   基于持久的内存优化表的只读工作负荷访问数据的方式与主数据库中的访问方式相同，使用具有相同事务隔离级别限制的本机存储过程或 SQL 互操作性（请参阅 [数据库引擎中的隔离级别](/previous-versions/sql/sql-server-2008-r2/ms189122(v=sql.105))）。 对主副本运行的报表工作负荷或只读查询可以直接用于辅助副本，无需任何更改。 同样，对辅助副本运行的报表工作负荷或只读查询也可以直接用于主副本，无需任何更改。  与基于磁盘的表相似，对辅助数据库运行的所有查询都自动映射到快照隔离事务级别，即使在显式设置了其他事务隔离级别时也是如此。  
  
-   对于辅助副本上的基于磁盘的表和内存优化表类型，都可以对表变量执行 DML 操作。  
  
##  <a name="prerequisites-for-the-availability-group"></a><a name="bkmk_Prerequisites"></a> 可用性组先决条件  
  
-   **可读辅助副本（必需）**  
  
     数据库管理员需要配置一个或多个副本，这样，在辅助角色下运行时，可允许所有连接（仅针对只读访问）或只允许读意向连接。  
  
    > [!NOTE]  
    >  或者，数据库管理员可以配置任何可用性副本，以便在主角色下运行时排除只读连接。
  
     有关详细信息，请参阅本主题后面的 [关于对可用性副本的客户端连接访问 (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)之类的系统数据库。  
  
    >[!WARNING]
    >  只有在同一 SQL Server 主要版本上的副本才可读。 有关详细信息，请参阅[滚动升级基础知识](upgrading-always-on-availability-group-replica-instances.md#rolling-upgrade-basics-for-always-on-ags)。
  
  
-   **可用性组侦听器**  
  
     为支持只读路由，可用性组必须具备 [可用性组侦听程序](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)。 只读客户端必须将其连接请求定向到此侦听器，并且客户端的连接字符串必须将应用程序意向指定为“只读”。 也就是说，它们必须是 *读意向连接请求*。  
  
-   **只读路由**  
  
     “只读路由” 指的是 SQL Server 将定向到可用性组侦听器的传入的读意向连接请求路由到可用的可读辅助副本的能力。 只读路由的先决条件如下：  
  
    -   为支持只读路由，可读辅助副本需要一个只读路由 URL。 此 URL 仅在本地副本在辅助角色下运行时起作用。 必须根据需要在逐个副本的基础上指定只读路由 URL。 每个只读路由 URL 都用于将读意向请求路由到一个特定的可读辅助副本。 通常，向每个可读辅助副本分配一个只读路由 URL。  
  
    -   要在其作为主副本时支持只读路由的每个可用性副本都要求一个只读路由列表。 一个给定的只读路由列表仅在本地副本在主角色下运行时才起作用。 必须根据需要在逐个副本的基础上指定此列表。 通常，每个只读路由列表中将包含各只读路由 URL，并且在列表的末尾具有本地副本的 URL。  
  
        > [!NOTE]  
        >  读意向连接请求可在不同副本中达到负载平衡。 有关详细信息，请参阅 [在只读副本间配置负载平衡](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing)。  
  
     有关详细信息，请参阅 [为可用性组配置只读路由 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)。  
  
> [!NOTE]  
>  有关可用性组侦听程序的信息，以及只读路由的详细信息，请参阅 [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)。  
  
##  <a name="limitations-and-restrictions"></a><a name="bkmk_LimitationsRestrictions"></a> 限制和局限  
 不完全支持某些操作，如下所示：  
  
-   当启用可读副本以便读取时，它便开始接收与其辅助数据库的连接。 但是，如果在主数据库上有活动事务，行版本将不会在相应的辅助数据库上完全可用。 必须提交或回滚在配置辅助副本时主副本上存在的所有活动事务。 在此过程完成前，对辅助数据库的事务隔离级别映射将不完整，并且查询被暂时阻塞。  
  
    > [!WARNING]  
    >  运行长时间运行的事务将影响保存的版本控制行数，对于基于磁盘的表和内存优化表都是如此。  
  
-   在具有内存优化表的辅助数据库上，即使始终对内存优化表生成行版本，也会阻塞查询，除非在启用辅助副本以便读取时主副本上的所有活动事务都已完成。 这将确保基于磁盘的表和内存优化表可同时用于报告工作负荷和只读查询。  
  
-   属于可读辅助副本的辅助数据库不支持更改跟踪和变更数据捕获：  
  
    -   在辅助数据库上显式禁用更改跟踪。  
  
    -   不能仅对次要副本数据库启用变更数据捕获。 可以在主要副本数据库上启用变更数据捕获，并且可以从次要副本数据库上使用函数的 CDC 表读取所做的更改。  
  
-   由于读操作会映射到快照隔离事务级别，因此，一个或多个辅助副本上的事务会阻止在主副本上清除虚影记录。 当所有辅助副本都不再需要虚影记录时，虚影记录清除任务将自动清除主副本上基于磁盘的表的虚影记录。  这类似于您在主副本上运行事务时执行的操作。 在辅助数据库上的极端情况下，需要终止正在阻塞虚影清除的长时间运行的读查询。 请注意，如果辅助副本断开连接或数据移动在辅助数据库上挂起，可能阻塞虚影清除。 此状态还会阻止日志截断，因此，如果此状态持续，则我们建议您从可用性组中删除此辅助数据库。 内存优化表没有虚影记录清除问题，因为行版本保存在内存中，独立于主副本上的行版本。  
  
-   如果文件包含辅助副本上仍需要的虚影记录，则主副本上对包含基于磁盘的表的文件的 DBCC SHRINKFILE 操作可能失败。  
  
-   从 [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] 开始，即使主要副本由于用户操作或故障（例如，由于用户命令或故障而导致同步暂停，或者由于 WSFC 脱机而导致副本正在解析状态）而处于脱机状态，可读次要副本也能保持联机状态。 但是，只读路由并不在此情况下工作，因为可用性组侦听器也处于脱机状态。 对于只读工作负荷，客户端必须直接连接到只读辅助副本。  
  
> [!NOTE]  
>  如果你在托管可读次要副本的服务器实例上查询 [sys.dm_db_index_physical_stats](../../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) 动态管理视图，则可能会遇到 REDO 阻塞问题。 这是因为此动态管理视图获取指定用户表或视图的 IS 锁，而该锁可能阻止 REDO 线程对该用户表或视图的 X 锁请求。  
  
##  <a name="performance-considerations"></a><a name="bkmk_Performance"></a> 性能注意事项  
 此节讨论可读辅助数据库的几个性能注意事项  
  
 **本节内容：**  
  
-   [数据滞后时间](#DataLatency)  
  
-   [只读工作负荷的影响](#ReadOnlyWorkloadImpact)  
  
-   [索引](#bkmk_Indexing)  
  
-   [只读访问数据库的统计信息](#Read-OnlyStats)  
  
###  <a name="data-latency"></a><a name="DataLatency"></a> 数据滞后时间  
 如果您的只读工作负荷可以容忍一定程度的数据滞后，则实现对辅助副本的只读访问很有用。 在数据滞后不可接受的情况下，请考虑对主副本运行只读工作负荷。  
  
 主副本将主数据库上的更改日志记录发送到辅助副本。 在每个辅助数据库上，专用重做线程应用这些日志记录。 在读访问权限的辅助数据库上，给定的数据更改不显示在查询结果中，直到包含更改的日志记录已应用到辅助数据库并且已在主数据库上提交事务。  
  
 这意味着在主副本和辅助副本之间将会存在一定程度的滞后时间，通常只需几秒钟。 但是，在极少数情况下，例如在网络问题降低了网络吞吐量的情况下，滞后时间可能会较长。 在存在 I/O 瓶颈和数据移动操作处于挂起状态时，将增加滞后时间。 若要监视挂起的数据移动，可以使用 [AlwaysOn 面板](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md) 或 [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) 动态管理视图。  
  
####  <a name="data-latency-on-databases-with-memory-optimized-tables"></a><a name="bkmk_LatencyWithInMemOLTP"></a> 具有内存优化表的数据库上的数据延迟  
 在 [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] 中，存在关于活动次要副本上数据延迟的特殊注意事项 - 请参阅[[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)]活动次要副本：可读次要副本](https://technet.microsoft.com/library/ff878253(v=sql.120).aspx)。 启动 [!INCLUDE[ssSQL15](../../../includes/sssql15-md.md)] 在内存优化表的数据延迟方面没有特殊注意事项。 内存优化表的预期数据延迟相当于基于磁盘的表的延迟。  
  
###  <a name="read-only-workload-impact"></a><a name="ReadOnlyWorkloadImpact"></a> 只读工作负荷的影响  
 为辅助副本配置只读访问时，辅助数据库上的只读工作负荷占用来自重做线程的系统资源，如 CPU 和 I/O（对于基于磁盘的表），特别是在基于磁盘的表的只读工作负荷大量占用 I/O 的情况下。 访问内存优化表时没有 IO 影响，因为所有行都驻留在内存中。  
  
 此外，辅助副本上的只读工作负荷还会阻止通过日志记录应用的数据定义语言 (DDL) 发生更改。  
  
-   虽然读取操作由于行版本控制而不会占用共享锁，但这些操作会占用架构稳定性 (Sch-S) 锁，这些锁可能会阻塞正在应用 DDL 更改的重做操作。 DDL 操作包括 ALTER/DROP 表和视图，但不包括存储过程的 DROP 或 ALTER。 因此，如果删除基于磁盘的表或内存优化表，请对主副本执行。 当 REDO 线程处理日志记录以删除表时，必须获取表的 SCH_M 锁，并且必须可通过运行查询访问表阻塞。  对于主副本也是如此，只是删除表是用户会话的一部分，而不是 REDO 线程的一部份。  
  
-   还有其他阻塞内存优化表。 如果辅助副本上存在本机存储过程的并发执行，删除本机存储过程可以导致 REDO 线程阻塞。 对于主副本也是如此，只是删除存储过程是用户会话的一部分，而不是 REDO 线程的一部份。  
  
 应了解与生成查询有关的最佳实践，并且在辅助数据库中应用这些最佳实践。 例如，将需要长时间运行的查询（如数据聚合）安排在低活动期间进行。  
  
> [!NOTE]  
>  如果 REDO 线程被次要副本上的查询阻塞，将引发 **sqlserver.lock_redo_blocked** XEvent。  
  
###  <a name="indexing"></a><a name="bkmk_Indexing"></a> 索引  
 若要优化可读辅助副本上的只读工作负荷，您可能需要对辅助数据库中的表创建索引。 因为您无法在辅助数据库上进行架构或数据更改，所以应在主数据库中创建索引，并且允许更改通过重做进程传输到辅助数据库。  
  
 若要监视辅助副本上的索引使用活动，请查询 **sys.dm_db_index_usage_stats** 动态管理视图的 **user_seeks**、 **user_scans** 和 [user_lookups](../../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md) 列。  
  
###  <a name="statistics-for-read-only-access-databases"></a><a name="Read-OnlyStats"></a> 只读访问数据库的统计信息  
 表和索引视图的列的统计信息用于优化查询计划。 对于可用性组，作为应用事务日志记录操作的一部分，在主数据库上创建和维护的统计信息将自动保留在辅助数据库中。 但是，辅助数据库上的只读工作负荷需要的统计信息可能与在主数据库上创建的统计信息不同。 但是，因为辅助数据库被限制为只读访问，所以无法在辅助数据库上创建统计信息。  
  
 为了解决此问题，辅助副本在 **tempdb** 中创建和维护辅助数据库的临时统计信息。 将在临时统计信息名称后追加后缀 The suffix _readonly_database_statistic，以便将临时统计信息与主数据库永久保存的永久统计信息加以区分。  
  
 只有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可以创建和更新临时统计信息。 但是，您可以借助用于永久统计信息的相同工具来删除临时统计信息和监视其属性：  
  
-   使用 [DROP STATISTICS](../../../t-sql/statements/drop-statistics-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句删除临时统计信息。  
  
-   使用 **sys.stats** 和 **sys.stats_columns** 目录视图监视统计信息。 **sys_stats** 包含一个 **is_temporary** 列，用于指示哪些统计信息是永久的，哪些统计信息是临时的。  
  
 不支持自动更新主副本或辅助副本上内存优化表的统计信息。 必须监视辅助副本上的查询性能和计划，在需要时手动更新主副本上的统计信息。 不过，会在主副本和辅助副本上自动创建缺少的统计信息。  
  
 有关 SQL Server 统计信息的详细信息，请参阅 [统计信息](../../../relational-databases/statistics/statistics.md)。  
  
 **本节内容：**  
  
-   [辅助数据库上陈旧的永久统计信息](#StalePermStats)  
  
-   [限制和局限](#StatsLimitationsRestrictions)  
  
####  <a name="stale-permanent-statistics-on-secondary-databases"></a><a name="StalePermStats"></a> 辅助数据库上陈旧的永久统计信息  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可检测辅助数据库上的永久统计信息的过时时间。 但是，除了通过主数据库进行更改外，不能对永久统计信息进行更改。 为了进行查询优化， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 在辅助数据库上为基于磁盘的表创建临时统计信息并使用它们来替代过时的永久统计信息。  
  
 永久统计信息在主数据库上进行更新后，自动将它们永久保存到辅助数据库。 然后， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用更新的永久统计信息，该信息比临时统计信息要新。  
  
 如果可用性组进行故障转移，则在所有辅助副本上删除临时统计信息。  
  
####  <a name="limitations-and-restrictions"></a><a name="StatsLimitationsRestrictions"></a> 限制和局限  
  
-   因为临时统计信息存储于 **tempdb** 中，所以重新启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务将导致所有临时统计信息消失。  
  
-   后缀 suffix _readonly_database_statistic 是为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]生成的统计信息预留的。 在主数据库上创建统计信息时不能使用此后缀。 有关详细信息，请参阅[统计信息](../../../relational-databases/statistics/statistics.md)。  
  
##  <a name="accessing-memory-optimized-tables-on-a-secondary-replica"></a><a name="bkmk_AccessInMemTables"></a> 访问辅助副本上的内存优化表  
 次要副本上的事务隔离级别与主要副本上的事务隔离级别相同，都可以与内存优化表结合使用。 建议你将会话级的隔离级别设置为 READ COMMITTED，将数据库级选项 MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT 设置为 ON。 例如：  
  
```sql  
ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON  
GO  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED  
GO  
SELECT SUM(UnitPrice*OrderQty)   
FROM Sales.SalesOrderDetail_inmem  
GO  
  
```  
  
##  <a name="capacity-planning-considerations"></a><a name="bkmk_CapacityPlanning"></a> 容量规划注意事项  
  
-   对于基于磁盘的表，可读次要副本出于以下两个原因需要占用 **tempdb** 中的空间：  
  
    -   快照隔离级别会将行版本复制到 **tempdb** 中。  
  
    -   在 **tempdb** 中创建和维护辅助数据库的临时统计信息。 临时统计信息会促使略微增大 **tempdb** 的大小。 有关详细信息，请参阅本节后面的 [只读访问数据库的统计信息](#Read-OnlyStats)。  
  
-   在配置对一个或多个辅助副本的读访问时，主数据库将对已删除、修改或插入的数据行添加 14 个字节的系统开销，以便为基于磁盘的表存储指向辅助数据库上的行版本的指针。 这 14 个字节开销将转入辅助数据库。 在向数据行添加 14 个字节的系统开销时，可能会发生页拆分。  
  
     行版本数据不由主数据库生成。 而是辅助数据库生成行版本。 但是，行版本控制在主数据库和辅助数据库中都会增加数据存储。  
  
     行版本数据的增加依赖于主数据库上的快照隔离或读提交快照隔离 (RCSI) 级别设置。 下表介绍可读辅助数据库上基于磁盘的表的不同设置下的版本控制行为。  
  
    |可读辅助副本？|启用了快照隔离或 RCSI 级别隔离？|主数据库|辅助数据库|  
    |---------------------------------|-----------------------------------------------|----------------------|------------------------|  
    |否|否|无行版本或 14 个字节的系统开销|无行版本或 14 个字节的系统开销|  
    |否|是|行版本和 14 个字节的系统开销|无行版本但有 14 个字节的系统开销|  
    |是|否|无行版本但有 14 个字节的系统开销|行版本和 14 个字节的系统开销|  
    |是|是|行版本和 14 个字节的系统开销|行版本和 14 个字节的系统开销|  
  
##  <a name="related-tasks"></a><a name="bkmk_RelatedTasks"></a> 相关任务  
  
-   [配置对可用性副本的只读访问 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [为可用性组配置只读路由 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [监视可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
-   [查看可用性副本属性 (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [使用“新建可用性组”对话框 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [SQL Server Always On 团队博客：SQL Server Always On 团队官方博客](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [关于对可用性副本的客户端连接访问 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 [可用性组侦听程序、客户端连接和应用程序故障转移 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [统计信息](../../../relational-databases/statistics/statistics.md)  
  
