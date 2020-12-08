---
title: 监视和优化性能 | Microsoft Docs
description: 了解如何监视数据库以评估服务器性能、使用定期快照和连续收集数据以跟踪性能趋势。
ms.custom: ''
ms.date: 07/18/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- instances of SQL Server, monitoring performance
- monitoring server performance [SQL Server]
- Database Engine [SQL Server], performance
- monitoring performance [SQL Server], about performance
- server performance [SQL Server]
- monitoring performance [SQL Server]
- database performance [SQL Server], about performance
- tuning databases [SQL Server], about performance
- status information [SQL Server], performance monitoring
- database monitoring [SQL Server], about performance
- monitoring [SQL Server], queries performance
- server performance [SQL Server], about performance
- tuning databases [SQL Server]
- database performance [SQL Server]
- monitoring [SQL Server], server performance
- database monitoring [SQL Server]
- monitoring server performance [SQL Server], about monitoring server performance
ms.assetid: 87f23f03-0f19-4b2e-bfae-efa378f7a0d4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e7f7c49e0324f8b615e17e4b2db06fbc6bf58f21
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505156"
---
# <a name="monitor-and-tune-for-performance"></a>监视和优化性能
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  监视数据库的目的是评估服务器的性能。 有效监视包括定期拍摄当前性能的快照来隔离导致问题的进程，以及连续收集数据来跟踪性能趋势。  
  
 日常数据库性能评估有助于使响应时间最小化并使吞吐量最大化，从而实现最佳性能。 有效网络流量、磁盘 I/O 和 CPU 使用率是实现最佳性能的关键。 您需要透彻地分析应用程序要求，了解数据的逻辑结构和物理结构，评估数据库使用情况，并协商使用 [如联机事务处理 (OLTP) 与决策支持] 冲突之间的平衡措施。  
  
## <a name="monitoring-and-tuning-databases-for-performance"></a>监视和优化数据库性能  
 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 Microsoft Windows 操作系统提供实用工具，以便查看数据库的当前状态并跟踪性能的状态变化。 可使用多种工具和技术来监视 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可帮助你实现以下操作：  
  
-   确定是否可以提高性能。 例如，通过监视常用查询的响应时间，可以确定是否需要更改表的查询或索引。  
  
-   评估用户活动。 例如，通过监视尝试连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的用户，可以确定安全设置是否充分以及是否需要测试应用程序或开发系统。 例如，通过在执行 SQL 查询时对其进行监视，可以确定这些查询是否编写正确并生成预期的结果。  
  
-   解决问题或调试应用程序组件（如存储过程）。  
  
## <a name="monitoring-in-a-dynamic-environment"></a>动态环境中的监视  
更改条件会导致性能发生变化。 在评估中，您可以看到性能会随着用户数量增加、用户访问和连接方法改变、数据库内容增加、客户端应用程序改变、应用程序中的数据变化、查询变得更加复杂以及网络流量上升而变化。 通过使用工具来监视性能，帮助你将性能的变化与条件和复杂查询的变化相关联。 **示例：**  
  
-   通过监视常用查询的响应时间，可以确定是否需要更改查询或执行查询的表上的索引。  
  
-   通过在执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询时对其进行监视，可以确定这些查询是否编写正确并生成预期的结果。  
  
-   通过监视试图连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的用户，可以确定安全设置是否得当并测试应用程序或开发系统。  
  
响应时间是指以可视确认信息（指出正在处理查询）的形式将结果集的首行返回给用户所需的时间。 吞吐量是指在一段给定时间内，服务器处理的查询总数。  
  
随着用户数量的增加，对服务器资源的竞争也会更激烈，进而导致响应时间增加和总体吞吐量减少。  
  
## <a name="monitoring-and-performance-tuning-tasks"></a>监视和性能优化任务  
  
|主题| 任务|  
|-----------|----------------------|  
|[监视 SQL Server 组件](../../relational-databases/performance/monitor-sql-server-components.md)|监视任何 SQL Server 组件所需的步骤，如活动监视器、扩展事件、动态管理视图和函数等。|  
|[性能监视和优化工具](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)|列出随 SQL Server 提供的监视和优化工具，如实时查询统计信息和数据库引擎优化顾问。|  
|[使用查询优化助手升级数据库](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)|升级到更新的数据库兼容级别期间保持工作负荷性能稳定性。|  
|[使用查询存储来监视性能](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)|使用查询存储将自动捕获查询、计划和运行时统计信息的历史记录，并保留它们以供查阅。|  
|[建立性能基线](../../relational-databases/performance/establish-a-performance-baseline.md)|如何建立性能基线。|  
|[隔离性能问题](../../relational-databases/performance/isolate-performance-problems.md)|隔离数据库性能问题。|  
|[识别瓶颈](../../relational-databases/performance/identify-bottlenecks.md)|监视和跟踪服务器性能，以发现瓶颈。|  
|[使用 DMV 来确定视图的使用情况统计信息和性能](../../relational-databases/performance/use-dmvs-determine-usage-performance-views.md)|本文介绍了一些方法和脚本，用于获取查询性能的相关信息。|  
|[服务器性能和活动监视](../../relational-databases/performance/server-performance-and-activity-monitoring.md)|使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 Windows 性能和活动监视工具。|  
|[监视资源使用情况](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)|使用系统监视器（也称为 perfmon）通过性能计数器来测量 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的性能。|  

  
## <a name="see-also"></a>另请参阅  
 [企业范围的自动化管理](../../ssms/agent/automated-administration-across-an-enterprise.md)    
 [比较和分析执行计划](../../relational-databases/performance/compare-and-analyze-execution-plans.md)    
 [显示和保存执行计划](../../relational-databases/performance/display-and-save-execution-plans.md)    
  
  
