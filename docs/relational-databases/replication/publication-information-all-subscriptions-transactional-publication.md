---
title: 所有订阅（事务 - SSMS）
description: 介绍 SQL Server Management Studio 中所选事务发布的“所有订阅”选项卡。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.monitor.publicationinfo.allsubscriptions.tran.f1
ms.assetid: 7073350c-f667-4f70-88e9-152c9a1b08dd
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 338775b139a2a1b91182261a0502f4f5ac72ec2d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439858"
---
# <a name="publication-information-all-subscriptions-transactional-publication"></a>发布信息，所有订阅（事务发布）
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  **“所有订阅”** 选项卡显示所选发布的所有订阅的相关信息。  
  
## <a name="options"></a>选项  
 有关订阅的详细信息及相关任务，请右键单击相应订阅所在的行，再单击快捷菜单上的选项。 若要更改网格显示数据的方式，请右键单击网格，然后单击以下选项之一：  
  
-   **排序**：按 **“列排序”** 对话框中的一列或多列排序。  
  
-   **选择要显示的列**：选择要显示哪些列以及要在 **“选择列”** 对话框中以何种顺序显示它们。  
  
-   **筛选器**：根据 **“筛选设置”** 对话框中的列值筛选网格中的行。  
  
-   **清除筛选器**：清除网格的任何筛选设置。  
  
 筛选设置是特定于每个网格的。 列的选择和排序应用于同一类型的所有网格，如每个发布服务器的发布网格。  
  
 **显示**  
 仅限 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本。 为所选订阅类型选择要显示的订阅状态。 例如，可以选择仅显示包含错误的订阅。  
  
 **Status**  
 每个订阅的状态，由分发代理或日志读取器代理的状态决定（显示优先级较高的状态；如果使用排队更新订阅，则状态还可以由队列读取器代理决定）。  
  
 默认情况下，包含订阅信息的网格按 **“状态”** 列排序（对于具有相同状态的订阅，再按 **“性能”** 列排序）。 下面的列表显示了可能的状态值以及这些值的排序顺序（例如，错误项始终显示在该网格的顶部）：  
  
-   错误  
  
-   “严重”状态下的性能（仅限[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本）  
  
-   即将过期/已过期（仅限[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本）  
  
-   未初始化的订阅（仅限[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本）  
  
-   正在重试失败的命令  
  
-   未运行  
  
-   正在运行  
  
 如果给定的订阅处于多种状态，该排序顺序还决定将要显示哪个值。 例如，如果某个订阅包含错误并且即将过期， **“状态”** 列将显示 **“错误”** 。  
  
 状态值 **“‘严重’状态下的性能”** 、 **“即将过期/已过期”** 和 **“未初始化的订阅”** 都是警告。 当显示警告时， **“状态”** 列还可显示是否正在运行代理。 例如，状态可能为 **“正在运行，‘严重’状态下的性能”** 。  
  
 只有在设置了阈值时，才会显示状态值 **“‘严重’状态下的性能”** 和 **“即将过期/已过期”** 。 有关性能度量和设置阈值的信息，请参阅[使用复制监视器监视性能](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)和[在复制监视器中设置阈值和警告](../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)。  
  
 **订阅**  
 每个订阅的名称，格式为： *SubscriberName: SubscriptionDatabaseName*。  
  
 **“性能”**  
 仅限[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本。 每个订阅的性能等级都是基于复制监视器的最新度量值，并不反映历史性能。 对于为发布定义了性能阈值的订阅，均会度量其性能；如果没有为发布定义性能阈值，此列将显示 **“未启用”** 。 性能等级可以为以下值之一：  
  
-   很好  
  
-   好  
  
-   一般  
  
-   较差  
  
-   严重  
  
 如果性能状态为“严重”，则 **“状态”** 列中将显示 **“‘严重’状态下的性能”** 。 有关如何定义性能等级以及如何设置性能阈值的详细信息，请参阅[使用复制监视器监视性能](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)。  
  
 **滞后时间**  
 仅限[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本。 事务在发布服务器上提交和相应的事务在订阅服务器上提交之间间隔的平均时间。 所显示的滞后时间基于复制监视器的最新度量值。 有关测量滞后时间的详细信息，请参阅[为事务复制测量滞后时间和验证连接](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)。  
  
## <a name="see-also"></a>另请参阅  
 [启动复制监视器](../../relational-databases/replication/monitor/start-the-replication-monitor.md)   
 [使用复制监视器查看信息和执行任务](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)   
 [监视复制](../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
