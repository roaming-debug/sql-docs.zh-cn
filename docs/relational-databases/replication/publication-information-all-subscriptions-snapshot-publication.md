---
title: 所有订阅（快照 - SSMS）
description: 介绍 SQL Server Management Studio (SSMS) 中所选快照发布的“所有订阅”选项卡。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.monitor.publicationinfo.allsubscriptions.snapshot.f1
ms.assetid: 7ce656c2-6e60-4625-8955-1daff641070c
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 823e7715e5fa3fc12fc1194e9f91536eae768d42
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477788"
---
# <a name="publication-information-all-subscriptions-snapshot-publication"></a>发布信息，所有订阅（快照发布）
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  **“所有订阅”** 选项卡显示针对所选快照发布的所有订阅的相关信息。  
  
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
 每一个订阅的状态，该状态由快照代理或分发代理的状态决定（显示优先级较高的状态）。  
  
 默认情况下，包含订阅信息的网格按 **“状态”** 列排序。 下面的列表显示了可能的状态值以及这些值的排序顺序（例如，错误项始终显示在该网格的顶部）：  
  
-   错误  
  
-   即将过期/已过期（仅限[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本）  
  
-   未初始化的订阅（仅限[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本）  
  
-   正在重试失败的命令  
  
-   正在同步  
  
-   未同步  
  
 如果给定的订阅处于多种状态，该排序顺序还决定将要显示哪个值。 例如，如果某个订阅包含错误并且即将过期， **“状态”** 列将显示 **“错误”** 。  
  
 状态值 **“即将过期/已过期”** 和 **“未初始化的订阅”** 属于警告。 当显示警告时， **“状态”** 列还可显示是否正在运行代理。 例如，状态可以为 **“正在运行，即将过期/已过期”** 。  
  
 只有在设置了阈值时，才会显示状态值 **“即将过期/已过期”** 。 有关设置阈值的信息，请参阅[在复制监视器中设置阈值和警告](../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)。  
  
 **订阅**  
 每个订阅的名称，格式为： *SubscriberName: SubscriptionDatabaseName*。  
  
 **上次同步**  
 分发代理上次运行的时间。 如果正在进行同步，则显示 **“正在进行”** 。  
  
## <a name="see-also"></a>另请参阅  
 [启动复制监视器](../../relational-databases/replication/monitor/start-the-replication-monitor.md)   
 [使用复制监视器查看信息和执行任务](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)   
 [监视复制](../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
