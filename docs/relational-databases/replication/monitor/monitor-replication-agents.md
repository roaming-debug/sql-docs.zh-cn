---
title: 监视复制代理 | Microsoft Docs
description: SQL Server 复制监视器提供复制活动的系统视图，让你能够查找特定代理的相关信息。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- monitoring performance [SQL Server replication], agents
- Log Reader Agent, monitoring
- Replication Monitor, agents
- Merge Agent, monitoring
- Queue Reader Agent, monitoring
- Snapshot Agent, monitoring
- agents [SQL Server replication], monitoring
- Distribution Agent, monitoring
ms.assetid: d06ed24f-82d7-4b9e-9e40-cc9780476a71
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: ddaa4698033f06db589ca550f39d434173e012e7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477838"
---
# <a name="monitor-replication-agents"></a>监视复制代理
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制监视器提供复制活动的系统视图，并且还可以直观地查找有关特定代理的信息。 以下列表列出了每个代理、可以在其中找到代理的选项卡（位于复制监视器中）以及指向说明如何访问这些选项卡的主题的链接：  
  
-   以下代理与复制监视器中的发布相关联：  
  
    -   快照代理  
  
    -   日志读取器代理  
  
    -   队列读取器代理  
  
     通过以下选项卡访问与这些代理相关联的信息和任务：“代理”（可用于每个发布服务器和发布）和“警告”（可用于每个发布） 。 有关详细信息，请参阅[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。  
  
-   以下代理与复制监视器中的订阅相关联：  
  
    -   分发代理  
  
    -   合并代理  
  
     通过以下选项卡访问与这些代理相关联的信息和任务：“订阅监视列表”（所有发布服务器都提供）或“所有订阅”选项卡（所有发布都提供） 。 有关详细信息，请参阅[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。  
  
## <a name="using-sql-server-management-studio-to-monitor-replication-agents"></a>使用 SQL Server Management Studio 监视复制代理  
 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 为监视复制代理提供下列对话框：  
  
-   **“查看快照代理状态”** （针对所有发布）  
  
-   **“查看日志读取器代理状态”** （针对所有事务发布）  
  
-   **“查看同步状态”** （针对所有订阅；使用此对话框可以访问分发代理和合并代理）  
  
 复制监视器提供有关每个代理的补充信息，并且如果使用了队列读取器代理，它还会对其进行监视。 有关详细信息，请参阅[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md),
  
#### <a name="to-monitor-the-snapshot-agent-and-log-reader-agent"></a>监视快照代理和日志读取器代理  
  
1.  在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]中连接到发布服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。  
  
3.  右键单击发布，再单击 **“查看日志读取器代理状态”** 或 **“查看快照代理状态”** 。  
  
4.  在 **“查看日志读取器代理状态”** 或 **“查看快照代理状态”** 对话框中：  
  
    -   查看代理状态。  
  
    -   必要时启动或停止代理。  
  
    -   单击 **“监视器”** 启动 **复制监视器**。  
  
5.  单击“关闭”。  
  
#### <a name="to-monitor-the-distribution-agent-and-merge-agent-from-the-publisher"></a>监视分发代理和合并代理（从发布服务器）  
  
1.  在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]中连接到发布服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。  
  
3.  展开要监视的订阅的发布。  
  
4.  右键单击此订阅，再单击 **“查看同步状态”** 。  
  
5.  在 **“查看同步状态”** 对话框中：  
  
    -   查看代理状态。  
  
    -   必要时启动或停止代理。  
  
    -   对于推送订阅，单击 **“监视器”** 启动 **复制监视器**。  
  
    -   对于请求订阅，单击 **“查看作业历史记录”** 启动 **日志文件查看器**，此查看器可以显示代理日志的输出。  
  
6.  单击“关闭”。  
  
#### <a name="to-monitor-the-distribution-agent-and-merge-agent-from-the-subscriber"></a>监视分发代理和合并代理（从订阅服务器）  
  
1.  在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]中连接到订阅服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再展开 **“本地订阅”** 文件夹。  
  
3.  右键单击要监视的订阅，再单击 **“查看同步状态”** 。  
  
4.  在 **“查看同步状态”** 对话框中：  
  
    -   查看代理状态。  
  
    -   必要时启动或停止代理。  
  
    -   单击 **“查看作业历史记录”** 启动 **日志文件查看器**，此查看器可以显示代理日志的输出。  
  
5.  单击“关闭”。  
  
## <a name="see-also"></a>另请参阅  
 [复制代理概述](../../../relational-databases/replication/agents/replication-agents-overview.md)  
  
  
