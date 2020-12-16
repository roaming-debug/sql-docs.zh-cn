---
description: 用复制监视器监视性能
title: 使用复制监视器监视性能 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- monitoring performance [SQL Server replication], Replication Monitor
- Log Reader Agent, monitoring
- Replication Monitor, performance
- Merge Agent, monitoring
- Queue Reader Agent, monitoring
- Snapshot Agent, monitoring
- Distribution Agent, monitoring
- monitoring performance [SQL Server replication]
ms.assetid: f212397d-1bfd-496b-a246-668952891d09
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 909670345c94444ba4883422f5d02f13757106c5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477858"
---
# <a name="monitor-performance-with-replication-monitor"></a>用复制监视器监视性能
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  使用 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制监视器，可以通过以下列方式监视事务复制和合并复制性能：  
  
-   设置警告和阈值  
  
-   查看性能度量值  
  
-   使用跟踪令牌确定滞后时间（事务复制）  
  
-   查看详细的同步统计信息（合并复制）  
  
-   查看事务和传递时间（事务复制）  
  
## <a name="set-warnings-and-thresholds"></a>设置警告和阈值  
 复制监视器允许为大量性能条件启用警告。 启用警告时，需要指定阈值。 达到或超过该阈值时，在与其进行同步的订阅和发布的 **“状态”** 列中将显示警告（除非需要显示更高优先级的问题）。 除了在复制监视器中显示警告之外，达到阈值也可以触发警报。 可以为下列性能条件启用警告：  
  
-   超过指定的滞后时间（从事务在发布服务器上提交到相应的事务在订阅服务器上提交之间间隔的时间）。  
  
     这适用于事务复制。 如果达到或超过指定的阈值，状态将显示为 **“‘严重’状态下的性能”**。  
  
-   超出了指定的同步时间。  
  
     这适用于合并复制。 如果达到或超过指定的阈值，状态将显示为 **“长时间运行的合并”**。 您可以为拨号连接和局域网 (LAN) 连接指定不同的阈值。  
  
-   在给定时间内未处理完指定的行数。  
  
     这适用于合并复制。 如果达到或超过指定的阈值，状态将显示为 **“‘严重’状态下的性能”**。 您可以为拨号连接和 LAN 连接指定不同的阈值。  
  
 有关详细信息，请参阅 [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)。  
  
## <a name="view-performance-measurements"></a>查看性能度量值  
 对于发布，复制监视器在 **“当前平均性能”** 和 **“当前最差的性能”** 列中显示事务复制和合并复制的性能质量值；对于订阅，复制监视器在 **“性能”** 列中显示这些值。 有效值为：  
  
-   优秀  
  
-   好  
  
-   一般  
  
-   差  
  
-   严重（仅适用于事务复制）  
  
 这些值以下列方式确定：  
  
-   对于事务复制，性能质量由滞后时间阈值确定。 如果不设置阈值，则不显示任何值。 下表显示了阈值与性能质量值之间的相关性。 例如，如果将阈值设置为 60 秒而实际滞后时间为 30 秒（即滞后时间为阈值的 50%），则性能质量值为“好”。  
  
    |优秀|好|一般|差|严重|  
    |---------------|----------|----------|----------|--------------|  
    |0 – 34%|35 – 59%|60 – 84%|85 – 99%|100% +|  
  
-   对于合并复制，性能质量独立于任意一个阈值（行处理阈值确定是否在 **“状态”** 列中显示 **“‘严重’状态下的性能”** 值）。 性能质量通过将具有相同连接类型（拨号或 LAN）的发布的单个订阅性能与订阅的平均历史性能进行比较来确定。 如果通过同一类型的连接进行了五次同步，且每次同步都进行了 50 处或更多的更改，则复制监视器将在此列中显示一个值。 如果包含 50 或 50 多次更改的同步不到五次，或最新同步中的更改少于 50 次，复制监视器将不显示值。  
  
     下表显示了平均性能与性能质量值之间的相关性。 例如，如果在 LAN 连接上有 10 个订阅服务器以每秒 100 行的平均速率进行同步，而其中一个订阅以每秒 125 行的速率进行同步（即该订阅服务器的同步性能的平均值为 125%），则性能质量值为“好”。  
  
    |优秀|好|一般|差|  
    |---------------|----------|----------|----------|  
    |151+%|76 – 150%|26 – 75%|0 – 25%|  
  
 有关查看订阅信息的详细信息，请参阅[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。  
  
## <a name="determine-latency-with-tracer-tokens"></a>使用跟踪令牌确定滞后时间  
 事务复制使您可以通过在发布数据库的事务日志中插入一个令牌（少量数据）并记录到达分发服务器和订阅服务器所用的时间，来测量系统的滞后时间。 使用令牌还可以识别数据是否未到达分发服务器或订阅服务器。 有关详细信息，请参阅 [为事务复制测量滞后时间和验证连接](../../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)。  
  
## <a name="view-detailed-synchronization-performance-for-merge-replication"></a>查看合并复制的详细同步性能  
 对于合并复制，复制监视器会显示同步过程中所处理的每个项目的详细统计信息，其中包括每个处理阶段（如上载更改、下载更改等等）所用的时间。 它可帮助查明导致速度降低的特定表，是用来解决合并订阅性能问题的最佳途径。 有关查看详细统计信息详细信息，请参阅[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。  
  
## <a name="view-transactions-and-delivery-time-for-transactional-replication"></a>查看事务及事务复制的传递时间  
 对于事务复制，复制监视器显示分发数据库中尚未分发到订阅服务器的事务数以及分发这些事务的估计时间的信息。 有关详细信息，请参阅[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。  
  
## <a name="see-also"></a>另请参阅  
 [监视复制](../../../relational-databases/replication/monitor/monitoring-replication.md)   
 [在复制监视器中设置阈值和警告](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)  
  
  
