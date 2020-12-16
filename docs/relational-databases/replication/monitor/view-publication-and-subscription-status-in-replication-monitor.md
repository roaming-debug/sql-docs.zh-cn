---
title: 查看发布和订阅状态（复制监视器）
description: 了解如何在 SQL Server Management Studio (SSMS) 中使用复制监视器查看发布和订阅状态。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Log Reader Agent, monitoring
- Merge Agent, monitoring
- Queue Reader Agent, monitoring
- publications [SQL Server replication], viewing information
- Snapshot Agent, monitoring
- Distribution Agent, monitoring
- monitoring performance [SQL Server replication], publication status
- monitoring performance [SQL Server replication], subscription status
- subscriptions [SQL Server replication], viewing status
- Replication Monitor, publication and subscription status
ms.assetid: 16590771-9867-463e-a973-36a5c145ac16
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: b716a4cd679e0cb252fb5805c0d3a8e74c6fb60a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460240"
---
# <a name="view-publication-and-subscription-status-in-replication-monitor"></a>在复制监视器中查看发布和订阅状态
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制监视器显示发布和订阅的状态信息：  
  
-   发布的状态由其订阅的最高优先级状态决定。 例如，如果对发布的一个订阅有错误，另一个订阅有性能问题，则显示发布的错误状态。  
  
-   订阅的状态由处理该订阅的代理的状态决定。 对于合并复制，由合并代理的状态决定。 对于事务复制，由日志读取器代理或分发代理（显示较高优先级状态；如果使用的是排队更新订阅，则状态还可以由队列读取器代理的状态决定）的状态决定。 对于快照复制，由快照代理或分发代理（显示较高优先级状态）的状态决定。  
  
 下列表列出了发布和订阅的可能状态值。 只有在达到或超过阈值时，才显示三个状态值。  
  
-   过期订阅  
  
     本状态值适用于所有类型的复制。 有关详细信息，请参阅 [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)。  
  
-   “严重”状态下的性能  
  
     此状态值适用于事务复制和合并复制。 有关详细信息，请参阅[使用复制监视器监视性能](../../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)。  
  
-   长时间运行的合并  
  
     此状态值适用于合并复制。 有关详细信息，请参阅[使用复制监视器监视性能](../../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)。  
  
 除提供发布和订阅状态外，合并复制还提供项目级统计信息，其中包括关于以下内容的详细信息：完成某个合并阶段还需要多长时间；处理某个给定项目花费的时间；订阅服务器正在使用的连接类型以及其他重要信息。 统计信息显示在复制监视器的合并代理窗口中。 快照复制和事务复制提供有关分发代理处理的详细信息。  
  
 **查看发布和订阅状态**  
  
-   复制监视器：[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md) 
  
 **查看代理的详细信息**  
  
-   复制监视器：[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。
  
## <a name="publication-status-values"></a>发布状态值  
 下表按优先级顺序显示了发布状态值及其对应的图标。  
  
|状态|图标|  
|------------|----------|  
|错误|![UI 图标：错误](../../../database-engine/availability-groups/windows/media/repl-icon-error.gif "UI 图标：错误")|  
|“严重”状态下的性能|![UI 图标：警告](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "UI 图标：警告")|  
|正在重试失败的命令|![UI 图标：复制代理重试](../../../relational-databases/replication/monitor/media/repl-icon-retry.gif "UI 图标：复制代理重试")|  
|OK|none|  
  
## <a name="subscription-status-values"></a>订阅状态值  
 下列表按优先级顺序显示了订阅状态值及其对应的图标。 一个订阅可以同时处于两种状态，如“即将过期/已过期”  和“正在重试失败的命令”  ；将显示最高优先级状态。  
  
 状态值“‘严重’状态下的性能”  、“即将过期/已过期”  和“未初始化”  都是警告。 显示警告时，复制监视器还显示是否有代理在运行。 例如，状态可能为 **“正在运行，‘严重’状态下的性能”** 。  
  
### <a name="transactional-subscriptions"></a>事务订阅  
  
|状态|图标|  
|------------|----------|  
|错误|![UI 图标：错误](../../../database-engine/availability-groups/windows/media/repl-icon-error.gif "UI 图标：错误")|  
|“严重”状态下的性能|![UI 图标：警告](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "UI 图标：警告")|  
|即将过期/已过期|![UI 图标：警告](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "UI 图标：警告")|  
|未初始化的订阅|![UI 图标：警告](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "UI 图标：警告")|  
|正在重试失败的命令|![UI 图标：复制代理重试](../../../relational-databases/replication/monitor/media/repl-icon-retry.gif "UI 图标：复制代理重试")|  
|未运行|![UI 图标：复制代理已停止](../../../relational-databases/replication/monitor/media/repl-icon-stopped.gif "UI 图标：复制代理已停止")|  
|正在运行|![UI 图标：复制代理正在运行](../../../relational-databases/replication/monitor/media/repl-icon-running.gif "UI 图标：复制代理正在运行")|  
  
### <a name="merge-subscriptions"></a>合并订阅  
  
|状态|图标|  
|------------|----------|  
|错误|![UI 图标：错误](../../../database-engine/availability-groups/windows/media/repl-icon-error.gif "UI 图标：错误")|  
|“严重”状态下的性能|![UI 图标：警告](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "UI 图标：警告")|  
|长时间运行的合并|![UI 图标：警告](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "UI 图标：警告")|  
|即将过期/已过期|![UI 图标：警告](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "UI 图标：警告")|  
|未初始化的订阅|![UI 图标：警告](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "UI 图标：警告")|  
|正在重试失败的命令|![UI 图标：复制代理重试](../../../relational-databases/replication/monitor/media/repl-icon-retry.gif "UI 图标：复制代理重试")|  
|正在同步|![UI 图标：复制代理正在运行](../../../relational-databases/replication/monitor/media/repl-icon-running.gif "UI 图标：复制代理正在运行")|  
|未同步|![UI 图标：复制代理已停止](../../../relational-databases/replication/monitor/media/repl-icon-stopped.gif "UI 图标：复制代理已停止")|  
  
### <a name="snapshot-subscriptions"></a>快照订阅  
  
|状态|图标|  
|------------|----------|  
|错误|![UI 图标：错误](../../../database-engine/availability-groups/windows/media/repl-icon-error.gif "UI 图标：错误")|  
|即将过期/已过期|![UI 图标：警告](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "UI 图标：警告")|  
|未初始化的订阅|![UI 图标：警告](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "UI 图标：警告")|  
|正在重试失败的命令|![UI 图标：复制代理重试](../../../relational-databases/replication/monitor/media/repl-icon-retry.gif "UI 图标：复制代理重试")|  
|正在同步|![UI 图标：复制代理正在运行](../../../relational-databases/replication/monitor/media/repl-icon-running.gif "UI 图标：复制代理正在运行")|  
|未同步|![UI 图标：复制代理已停止](../../../relational-databases/replication/monitor/media/repl-icon-stopped.gif "UI 图标：复制代理已停止")|  
  
## <a name="see-also"></a>另请参阅  
 [监视复制](../../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
