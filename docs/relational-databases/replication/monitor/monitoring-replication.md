---
title: 监视（复制）| Microsoft Docs
description: 了解用于跟踪 SQL Server 复制拓扑中的复制的活动和状态的监视工具。
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- monitoring performance [SQL Server replication], about monitoring replication
- transactional replication, monitoring
- monitoring [SQL Server replication]
- merge replication monitoring [SQL Server replication]
- snapshot replication [SQL Server], monitoring
- replication [SQL Server], monitoring
- administering replication, monitoring
ms.assetid: f182f43a-6af8-45bc-a708-08d5f7a6984a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 2d9b58373aa64a2e2ee377acace142803b13ba51
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477828"
---
# <a name="monitoring-replication"></a>监视（复制）
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  监视复制拓扑是部署复制的一个重要方面。 因为复制活动是分布式的，所以跟踪复制过程中涉及的所有计算机的活动和状态非常有必要。 使用各种监视工具，可以回答此类常见问题： 

-   我的复制系统是否正常？
-   哪些订阅比较慢？
-   我的事务订阅滞后程度如何？
-   在事务复制中，现在提交的事务要过多久才能到达订阅服务器？
-   为什么我的合并订阅比较慢？
-   为什么代理不运行？  
  

以下工具可用于监视复制：  
  
-   SQL Server 复制监视器是监视复制最重要的工具，它可以显示以发布服务器为中心的所有复制活动视图。 有关详细信息，请参阅 [Monitoring Replication](../../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)。 
-   SQL Server Management Studio - 提供对复制监视器的访问权限。 它还允许你查看当前状态以及由下列代理记录的最后一条消息，还允许你启动和停止每个代理：日志读取器代理、快照代理、合并代理和分发代理。 有关详细信息，请参阅 [Monitor Replication Agents](../../../relational-databases/replication/monitor/monitor-replication-agents.md)。  
  
-   **Transact-SQL (T-SQL) 和复制管理对象 (RMO)** - 可通过这两个接口监视来自分发服务器的所有类型的复制。 合并复制还提供从发布服务器监视复制的功能。  
  
-   **复制代理事件的警报** - 复制提供了许多复制代理事件的预定义警报，如有必要，还可创建其他警报。 警报用于触发对某个事件的自动响应和/或用于通知管理员。 有关详细信息，请参阅[对复制代理事件使用警报](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md)。  
  
-   **系统监视器** - 对性能监视很有用，它可为复制提供多个计数器。 有关详细信息，请参阅 [Monitoring Replication with System Monitor](../../../relational-databases/replication/monitor/monitoring-replication-with-system-monitor.md)。  
  

## <a name="see-also"></a>另请参阅  
 [Best Practices for Replication Administration](../../../relational-databases/replication/administration/best-practices-for-replication-administration.md)   

  
  
