---
title: 配置 max degree of parallelism 服务器配置选项 | Microsoft Docs
description: 了解最大并行度 (MAXDOP) 选项。 查看如何使用它来限制 SQL Server 在执行并行计划时使用的处理器数量。
ms.date: 02/12/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- parallel queries [SQL Server]
- processors [SQL Server], parallel queries
- number of processors for parallel queries
- max degree of parallelism option
- MaxDop
ms.assetid: 86b65bf1-a6a1-4670-afc0-cdfad1558032
author: markingmyname
ms.author: maghan
ms.custom: contperf-fy20q4
ms.openlocfilehash: 4006ad1707d30d0a9147056ddfff8b71ae7643e1
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236967"
---
# <a name="configure-the-max-degree-of-parallelism-server-configuration-option"></a>配置 max degree of parallelism 服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 SQL Server 中配置最大并行度 (MAXDOP) 服务器配置选项。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例在具有多个微处理器或 CPU 的计算机上运行时，[!INCLUDE[ssde_md](../../includes/ssde_md.md)] 会检测是否可以使用并行。 并行度为每次并行计划的执行设置运行单个语句时要使用的处理器数。 您可以使用 **max degree of parallelism** 选项来限制并行计划执行时所用的处理器数。 有关最大并行度 (MAXDOP) 所设置的限制的详细信息，请参阅此页中的[注意事项](#Considerations)部分。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 考虑为查询、索引数据定义语言 (DDL) 操作、并行插入、联机更改列、并行统计信息集合以及静态的和由键集驱动的游标填充实施并行执行计划。

> [!NOTE]
> [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] 介绍有关在安装过程中如何基于可用处理器数量设置 MAXDOP 服务器配置的自动建议。 安装程序用户界面允许接受建议的设置或输入自己的值。 有关详细信息，请参阅[“数据库引擎配置 - MaxDOP”页](../../sql-server/install/instance-configuration.md#maxdop)。

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="considerations"></a><a name="Considerations"></a> 注意事项  
-   此选项是一个高级选项，仅应由有经验的数据库管理员或认证的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 专业人员更改。  

-   如果 affinity mask 选项不设置为默认值，则可能会限制可用于对称多处理 (SMP) 系统上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的处理器数。 
  
-   若将最大并行度 (MAXDOP) 设置为 0，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将能够使用至多 64 个可用的处理器。 但在大多数情况下，不推荐使用此值。 有关最大并行度的建议值的详细信息，请参阅此页中的[建议](#Recommendations)部分。

-   若要取消生成并行计划，请将 **max degree of parallelism** 设置为 1。 将该值设置为 1 到 32,767 之间的数值来指定执行单个查询所使用的最大处理器核数。 如果指定的值比可用的处理器数大，则使用实际可用数量的处理器。 如果计算机只有一个处理器，将忽略 **max degree of parallelism** 值。  

-   将按[任务](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)设置最大并行度限制。 它不是按[请求](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)限制或按查询限制。 这意味着在并行查询执行期间，一个请求可生成多个任务（不超过 MAXDOP 限制），并且每个任务将使用一个工作线程和一个计划程序。 有关详细信息，请参阅[线程和任务体系结构指南](../../relational-databases/thread-and-task-architecture-guide.md)中的“计划并行任务”部分。 
  
-   可重写“最大并行度”服务器配置值：
    -   在查询级别，使用 MAXDOP [查询提示](../../t-sql/queries/hints-transact-sql-query.md)。     
    -   在数据库级别，请使用 MAXDOP [数据库范围的配置](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)。
    -   在工作负载级别，请使用 MAX_DOP [Resource Governor 工作负载组配置选项](../../t-sql/statements/create-workload-group-transact-sql.md)。

-   索引操作（如创建或重新生成索引、或删除聚集索引）可能会大量占用资源。 您可以通过在索引语句中指定 MAXDOP 索引选项来覆盖索引操作的 max degree of parallelism 值。 MAXDOP 值在执行时应用于语句，但不存储在索引元数据中。 有关详细信息，请参阅 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)。  
  
-   除了查询和索引操作之外，此选项还控制 DBCC CHECKTABLE、DBCC CHECKDB 和 DBCC CHECKFILEGROUP 的并行。 使用跟踪标志 2528，可以禁用为这些语句所做的并行执行计划。 有关详细信息，请参阅[跟踪标志 (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。

###  <a name="recommendations"></a><a name="Recommendations"></a> <a name="Guidelines"></a> 建议  
从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始，在服务启动期间，如果 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 在启动时检测到每个 NUMA 节点或插槽内的物理内核数目超过 8 个，在默认情况下就会自动创建 soft-NUMA 节点。 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 将相同物理内核中的逻辑处理器放入不同的 soft-NUMA 节点中。 下表中的建议旨在将并行查询的所有工作线程保持在相同 soft-NUMA 节点中。 这将提高跨工作负荷 NUMA 节点查询和分布工作线程的性能。 有关详细信息，请参阅 [Soft-NUMA](../../database-engine/configure-windows/soft-numa-sql-server.md)。

从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始，请使用以下准则配置“最大并行度”服务器配置值：

|服务器配置|处理器数目|指南|
|----------------|-----------------|-----------------|
|具有单个 NUMA 节点的服务器|小于或等于 8 个逻辑处理器|将 MAXDOP 保持为小于或等于逻辑处理器的数量|
|具有单个 NUMA 节点的服务器|大于 8 个逻辑处理器|将 MAXDOP 保持为 8 个|
|具有多个 NUMA 节点的服务器|每个 NUMA 节点拥有小于或等于 16 个逻辑处理器|将 MAXDOP 保持为小于或等于每个 NUMA 节点的逻辑处理器的数量|
|具有多个 NUMA 节点的服务器|每个 NUMA 节点大于 16 个逻辑处理器|将 MAXDOP 保持为每个 NUMA 节点逻辑处理器数量的一半，最大值为 16|
  
> [!NOTE]
> 上表中的 NUMA 节点是指，由 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 及更高版本自动创建的 soft-NUMA 节点，或基于硬件的 NUMA 节点（如果 soft-NUMA 已遭禁用的话）。   
>  为 Resource Governor 工作负荷组设置“最大并行度”选项时，请使用这些相同的准则。 有关详细信息，请参阅 [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md)。
  
从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 到 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]，请使用以下准则配置“最大并行度”服务器配置值：

|服务器配置|处理器数目|指南|
|----------------|-----------------|-----------------|
|具有单个 NUMA 节点的服务器|小于或等于 8 个逻辑处理器|将 MAXDOP 保持为小于或等于逻辑处理器的数量|
|具有单个 NUMA 节点的服务器|大于 8 个逻辑处理器|将 MAXDOP 保持为 8 个|
|具有多个 NUMA 节点的服务器|每个NUMA 节点拥有小于或等于 8 个逻辑处理器|将 MAXDOP 保持为小于或等于每个 NUMA 节点的逻辑处理器的数量|
|具有多个 NUMA 节点的服务器|每个 NUMA 节点大于 8 个逻辑处理器|将 MAXDOP 保持为 8 个|
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，所有用户都具备不带参数或仅带第一个参数的 **sp_configure** 的执行权限。 若要执行带两个参数的 **sp_configure** 以更改配置选项或运行 RECONFIGURE 语句，则用户必须具备 ALTER SETTINGS 服务器级别的权限。 ALTER SETTINGS 权限由 **sysadmin** 和 **serveradmin** 固定服务器角色隐式持有。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-configure-the-max-degree-of-parallelism-option"></a>配置 max degree of parallelism 选项  
  
1.  在“对象资源管理器”中，右键单击服务器并选择“属性”。  
  
2.  单击 **“高级”** 节点。  
  
3.  在 **“最大并行度”** 框中，选择执行并行计划时所使用的最大处理器数。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-the-max-degree-of-parallelism-option"></a>配置 max degree of parallelism 选项  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例说明如何使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 将 `max degree of parallelism` 选项的值配置为 `16`。  
  
```sql  
USE AdventureWorks2012 ;  
GO   
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE WITH OVERRIDE;  
GO  
EXEC sp_configure 'max degree of parallelism', 16;  
GO  
RECONFIGURE WITH OVERRIDE;  
GO  
```  
  
 有关详细信息，请参阅 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)版本的组合自动配置的最大工作线程数。  
  
##  <a name="follow-up-after-you-configure-the-max-degree-of-parallelism-option"></a><a name="FollowUp"></a> 跟进：在配置“最大并行度”选项之后  
 该设置将立即生效，无需重新启动服务器。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)        
 [affinity mask 服务器配置选项](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md)      
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md#DOP)       
 [线程和任务体系结构指南](../../relational-databases/thread-and-task-architecture-guide.md)    
 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)    
 [查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)     
 [设置索引选项](../../relational-databases/indexes/set-index-options.md)     

## <a name="next-steps"></a>后续步骤

[重新配置 &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)
[监视和优化性能](../../relational-databases/performance/monitor-and-tune-for-performance.md)
