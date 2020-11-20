---
title: 性能仪表板 |Microsoft Docs
description: 了解 SQL Server Management Studio 性能仪表板，你可通过它快速了解 SQL Server 和 Azure SQL 托管实例。
ms.custom: ''
ms.date: 11/13/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- performance dashboard [SQL Server]
- performance dashboard reports
- perf dashboard
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: pelopes
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 45b657be3cef9267431892eeced327e966b97b7d
ms.sourcegitcommit: 2bf83972036bdbe6a039fb2d1fc7b5f9ca9589d3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94674172"
---
# <a name="performance-dashboard"></a>性能仪表板
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 版本 17.2 及更高版本包括性能仪表板。 此仪表板旨在让你直观、快速地了解 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 开始）和 [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] 托管实例的性能状态。 

性能仪表板有助于快速识别 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] 是否遇到性能瓶颈。 如果找到瓶颈，则可轻松地捕获可能解决该问题所需的附加诊断数据。 性能仪表板可以帮助识别的一些常见性能问题，包括：
-  CPU 瓶颈（以及哪些查询正占用最多的 CPU）
-  I/O 瓶颈（以及哪些查询正执行最多的 I/O）
-  查询优化器生成的索引建议（缺失索引）
-  阻塞
-  资源争用（包括闩锁争用）

性能仪表板还有助于识别之前可能已执行的耗费大量资源的查询，并且有多个指标可用于定义高成本：CPU、逻辑写入数，逻辑读取数、持续时间、物理读取数和 CLR 时间。

性能仪表板分为以下各部分和子报表：
-  系统 CPU 使用率
-  当前等待请求
-  当前活动
   -  用户请求
   -  用户会话
   -  缓存命中率
-  历史信息
   -  等待
   -  闩锁
   -  I/O 统计信息
   -  耗费大量资源的查询
- 其他信息
  -  活动跟踪
  -  活动 xEvent 会话
  -  数据库
  -  缺失索引

> [!NOTE] 
> 性能仪表板在内部使用与动态管理视图 (DMV) 和函数 (DMF) 相关的[执行](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)、[索引](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)和 [I/O](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)。

## <a name="to-view-the-performance-dashboard"></a>查看性能仪表板 
  
若要查看性能仪表板，请右键单击对象资源管理器中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例名称，选择“报表”，“标准报表”，然后单击“性能仪表板”  。  
  
![菜单中的性能仪表板](../../relational-databases/performance/media/perf_dashboard_ssms.png "菜单中的性能仪表板")  
  
性能仪表板将作为新选项卡显示。下面的示例清楚地显示了 CPU 瓶颈所在：  
  
![主屏幕中的性能仪表板](../../relational-databases/performance/media/perf_dashboard.png "主屏幕中的性能仪表板")  
  
## <a name="remarks"></a>备注
“缺失索引”报表显示查询编译期间查询优化程序识别的可能缺少的索引。 但是，不应根据表面判断而执行这些建议。 Microsoft 建议评估分数大于 100,000 的索引用于创建，因为这些索引可对用户查询实现最大的预计改进。 

> [!TIP]
> 始终评估新索引建议是否与同一表中的现有索引相当，只需更改现有索引而不是创建新索引即可实现相同的实际结果。 例如，给定列 C1、C2 和 C3 上的新建议索引，首先评估列 C1 和 C2 上是否存在现有索引。 如果存在，可能只将列 C3 添加到现有索引（保留预先存在的列的顺序）以避免创建新索引会更可取。
> 有关详细信息，请参阅[索引体系结构和设计指南](../../relational-databases/sql-server-index-design-guide.md)。

“等待”报表可筛选出所有空闲和睡眠的等待。 有关等待的详细信息，请参阅 [sys.dm_os_wait_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) 和[使用等待和队列优化 SQL Server 2005 性能](https://download.microsoft.com/download/4/7/a/47a548b9-249e-484c-abd7-29f31282b04d/performance_tuning_waits_queues.doc)。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 重启时“耗费大量资源的查询”报表将重置，因为基础 DMV 中的数据已被清除。 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，耗费大量资源的查询的详细信息可以在查询存储中找到。 


> [!NOTE]
> 性能仪表板首次作为 [SQL Server 2005](https://techcommunity.microsoft.com/t5/SQL-Server-Support/SQL-Server-2005-Performance-Dashboard-Reports/ba-p/315415) 的独立下载发布，之后又针对 [SQL Server 2012](https://www.microsoft.com/download/details.aspx?id=29063) 进行了更新。 虽然 SQL Server Management Studio 报表呈现器不支持剪贴板访问报表中包含的文本，但可通过独立报表访问这些文本。  如果需要复制报表中的查询文本，请下载独立报表。

## <a name="permissions"></a>权限  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上，需要 `VIEW SERVER STATE` 和 `ALTER TRACE` 权限。 在 [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] 上，需要在数据库中拥有 `VIEW DATABASE STATE` 权限。

## <a name="see-also"></a>另请参阅  
 [监视和优化性能](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [性能监视和优化工具](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [打开活动监视器 (SQL Server Management Studio)](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [活动监视器](../../relational-databases/performance-monitor/activity-monitor.md)     
 [使用查询存储来监视性能](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
