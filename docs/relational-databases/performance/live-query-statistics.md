---
title: 实时查询统计信息 | Microsoft Docs
description: 了解如何在 SQL Server Management Studio 中查看活动查询的实时执行计划。 使用执行统计信息调试查询性能问题。
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- query statistics [SQL Server] live query stats
- live query statistics
- debugging [SQL Server], live query stats
- statistics [SQL Server], live query statistics
- query profiling
- lightweight query profiling
- lightweight profiling
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2c45216331d940398c0d89e319c9f847d70faba0
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505160"
---
# <a name="live-query-statistics"></a>实时查询统计信息
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 能够查看活动查询的实时执行计划。 此实时查询计划作为控制流，能够实时了解从一个[查询计划操作员](../../relational-databases/showplan-logical-and-physical-operators-reference.md)到另一个操作员的查询执行过程。 实时查询计划显示总体查询进度和操作员级运行时执行统计信息（例如处理的行数、经过的时间、操作员进度等）。由于此数据是实时可用的，无需等待完成查询，因此这些执行统计信息对于调试查询性能问题非常有用。 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 开始支持此功能，但它可以与 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 配合使用。  

> [!NOTE]
> 在内部，实时查询统计信息利用 [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md) DMV。
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。  
  
> [!WARNING]  
> 此功能主要用于故障排除。 使用此功能会明显降低整体查询性能，尤其是在 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中。 有关详细信息，请参阅[查询分析基础结构](../../relational-databases/performance/query-profiling-infrastructure.md)。  
> 此功能可与 [Transact-SQL 调试器](../../ssms/scripting/configure-firewall-rules-before-running-the-tsql-debugger.md)配合使用。  
  
## <a name="to-view-live-query-statistics-for-one-query"></a>查看某查询的实时查询统计信息 
  
1.  若要查看实时查询执行计划，请在工具菜单上单击“添加实时查询统计信息”图标。  
  
     ![工具栏上的“实时查询统计信息”按钮](../../relational-databases/performance/media/livequerystatstoolbar.png "工具栏上的“实时查询统计信息”按钮")  
  
     还可以查看实时查询执行计划，方法是在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中右键单击所选查询，然后单击“包含实时查询统计信息”。  
  
     ![弹出菜单上的“实时查询统计信息”按钮](../../relational-databases/performance/media/livequerystatsmenu.png "弹出菜单上的“实时查询统计信息”按钮")  
  
2.  现在执行查询。 实时查询计划显示查询计划操作员的总体查询进度和运行时执行统计信息（例如，经过的时间、进度等）。 查询进度信息和执行统计信息会在查询执行的同时定期更新。 使用此信息可了解整个查询执行过程，以及调试长时间运行的查询、无限期运行的查询、导致 tempdb 溢出和超时问题的查询。  
  
     ![显示计划中的“实时查询统计信息”按钮](../../relational-databases/performance/media/livequerystatsplan.png "显示计划中的“实时查询统计信息”按钮")  
  
## <a name="to-view-live-query-statistics-for-any-query"></a>查看任何查询的实时查询统计信息 

此外，可以通过右键单击“进程”表或“活动的耗费大量资源的查询”表中的任何查询，从[活动监视器](../../relational-databases/performance-monitor/activity-monitor.md)中访问实时执行计划  。  
  
 ![活动监视器中的“实时查询统计信息”按钮](../../relational-databases/performance/media/livequerystatsactmon.png "活动监视器中的“实时查询统计信息”按钮")  
  
## <a name="remarks"></a>备注  
 必须启用统计信息配置文件基础结构，实时查询统计信息才能捕获查询进度的相关信息。 开销有可能较大，具体取决于使用的版本。 有关此开销的详细信息，请参阅[查询分析基础结构](../../relational-databases/performance/query-profiling-infrastructure.md)。
  
## <a name="permissions"></a>权限  
需要数据库级别 `SHOWPLAN` 权限才能填充“实时查询统计信息”结果页，还需要执行查询所需的所有权限。
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上，需要服务器级别 `VIEW SERVER STATE` 权限才能查看实时统计信息。  
对于 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 高级层，需要数据库的 `VIEW DATABASE STATE` 权限才能查看实时统计信息。 在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 标准层和基本层上，需要“服务器管理员”或“Azure Active Directory 管理员”帐户才能查看实时统计信息 。
  
## <a name="see-also"></a>另请参阅  
 [执行计划](../../relational-databases/performance/execution-plans.md)    
 [查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md)    
 [监视和优化性能](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [性能监视和优化工具](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [打开活动监视器 (SQL Server Management Studio)](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [活动监视器](../../relational-databases/performance-monitor/activity-monitor.md)     
 [相关视图、函数和过程](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md)     
 [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)     
 [跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)    
 [Showplan 逻辑运算符和物理运算符参考](../../relational-databases/showplan-logical-and-physical-operators-reference.md)     
 [查询分析基础结构](../../relational-databases/performance/query-profiling-infrastructure.md)