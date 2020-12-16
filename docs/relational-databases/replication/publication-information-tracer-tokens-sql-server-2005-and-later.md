---
title: 跟踪令牌（发布信息）
description: “发布信息”页的“跟踪令牌”选项卡的说明位于 SQL Server Management Studio (SSMS) 中的复制监视器中。
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.monitor.publicationinfo.tracertokens.f1
ms.assetid: a115ba95-17ae-45df-91bd-5a1a35f3745f
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a3b273e574b1a21c2f495b684063c525ccc7f362
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460180"
---
# <a name="publication-information-tracer-tokens-sql-server-2005-and-later"></a>发布信息，跟踪令牌（SQL Server 2005 和更高版本）
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  可以使用 **“跟踪令牌”** 选项卡验证连接，以及测量使用事务复制的系统的滞后时间。 将令牌（少量数据）写入发布数据库的事务日志中，就像标记典型的复制事务一样对其进行标记，使用令牌可以执行以下计算：  
  
-   计算在发布服务器上提交事务和在分发服务器上将相应命令插入分发数据库之间所间隔的时间。  
  
-   计算在分发数据库中插入命令和在订阅服务器上提交相应事务之间所间隔的时间。  
  
 通过这些计算可以回答许多问题，包括：  
  
-   哪个订阅服务器在从发布服务器接收更改时所需的时间最长？  
  
-   在应当收到跟踪令牌的订阅服务器中，哪些订阅服务器（如果有）没有收到跟踪令牌？  
  
## <a name="options"></a>选项  
 若要更改网格显示数据的方式，请右键单击网格，然后单击以下选项之一：  
  
-   **排序**：按 **“列排序”** 对话框中的一列或多列排序。  
  
-   **选择要显示的列**：选择要显示哪些列以及要在 **“选择列”** 对话框中以何种顺序显示它们。  
  
-   **筛选器**：根据 **“筛选设置”** 对话框中的列值筛选网格中的行。  
  
-   **清除筛选器**：清除网格的任何筛选设置。  
  
 筛选设置是特定于每个网格的。 列的选择和排序应用于同一类型的所有网格，如每个发布服务器的发布网格。  
  
 **插入跟踪器**  
 单击此项可以在发布服务器的事务日志中插入跟踪令牌。  
  
 **插入时间**  
 选择插入跟踪令牌的时间，以便从该时间开始显示滞后时间信息。 默认情况下，从当前时间开始显示信息。  
  
> [!NOTE]  
>  跟踪令牌信息的保留时间与其他历史数据的保留时间一样长，该时间由分发数据库的历史记录保持期来控制。 若要了解如何更改分发数据库属性，请参阅[查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)。  
  
 **订阅**  
 发布的每个订阅的名称。  
  
 **发布服务器到分发服务器**  
 在发布服务器上提交事务和在分发服务器上将相应命令插入分发数据库之间所间隔的时间。 值为 **“挂起”** 指示令牌尚未到达分发服务器。 如果持续显示挂起状态，请确保日志读取器代理正在运行。  
  
 **分发服务器到订阅服务器**  
 在分发数据库中插入命令和在订阅服务器上提交相应事务之间所间隔的时间。 值为 **“挂起”** 指示令牌尚未到达订阅服务器。 如果持续显示挂起状态，请确保分发代理正在运行。  
  
 **总滞后时间**  
 在发布服务器上提交事务和在订阅服务器上提交相应事务之间所间隔的时间。 该值表示对于此订阅服务器，此时复制系统的端对端滞后时间。 值为 **“挂起”** 指示令牌尚未到达订阅服务器。  
  
## <a name="see-also"></a>另请参阅  
 [启动和停止复制代理 (SQL Server Management Studio)](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)   
 [启动复制监视器](../../relational-databases/replication/monitor/start-the-replication-monitor.md)   
 [为事务复制测量滞后时间和验证连接](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)   
 [使用复制监视器监视性能](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)   
 [监视复制](../../relational-databases/replication/monitor/monitoring-replication.md)   
 [复制代理概述](../../relational-databases/replication/agents/replication-agents-overview.md)  
  
  
