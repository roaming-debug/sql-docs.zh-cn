---
title: 启动和停止复制代理 (SSMS)
description: 了解如何启动和停止 SQL Server Management Studio 和复制监视器中的复制代理。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- agents [SQL Server replication], stopping
- agents [SQL Server replication], starting
ms.assetid: 97977c4a-8c7c-4a22-9480-69aa812bd1e5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 1cddb10655acce42c621a8032edd5e75226c1c5e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467308"
---
# <a name="start-and-stop-a-replication-agent-sql-server-management-studio"></a>启动和停止复制代理 (SQL Server Management Studio)
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  可以从 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 中的“作业”和“复制”文件夹以及复制监视器启动和停止代理 。 可启动和停止以下代理和作业：  
  
-   快照代理，用于所有发布。  
  
-   日志读取器代理，用于所有事务发布。  
  
-   队列读取器代理，用于为排队更新订阅启用的事务发布。  
  
-   分发代理，用于同步对事务发布和快照发布的订阅。  
  
-   合并代理，用于同步对合并发布的订阅。  
  
-   复制维护作业。  
  
 有关启动合并代理和分发代理的详细信息，请参阅[同步推送订阅](../../../relational-databases/replication/synchronize-a-push-subscription.md)和[同步请求订阅](../../../relational-databases/replication/synchronize-a-pull-subscription.md)。 有关维护作业的详细信息，请参阅[运行复制维护作业 (SQL Server Management Studio)](../../../relational-databases/replication/administration/run-replication-maintenance-jobs-sql-server-management-studio.md)。  
  
 有关启动复制监视器的信息，请参阅[启动复制监视器](../../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
### <a name="to-start-and-stop-a-snapshot-agent-or-log-reader-agent-from-management-studio"></a>从 Management Studio 启动和停止快照代理或日志读取器代理  
  
1.  在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]中连接到发布服务器，然后展开服务器节点和 **“复制”** 文件夹。  
  
2.  展开 **“本地发布”** 文件夹，然后右键单击发布。  
  
3.  单击 **“查看快照代理状态”** 或 **“查看日志读取器代理状态”** 。  
  
4.  单击 **“启动”** 或 **“停止”** 。  
  
### <a name="to-start-and-stop-a-queue-reader-agent-from-management-studio"></a>从 Management Studio 启动和停止队列读取器代理  
  
1.  在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]中连接到分发服务器，然后展开服务器节点。  
  
2.  展开 **“SQL Server 代理”** 文件夹，再展开 **“作业”** 文件夹。  
  
3.  右键单击代理的作业，再单击 **“启动作业”** 或 **“停止作业”** 。 队列读取器代理的作业名称的格式为 [\<Distributor>].\<integer>。  
  
### <a name="to-start-and-stop-a-snapshot-agent-log-reader-agent-or-queue-reader-agent-from-replication-monitor"></a>从复制监视器启动和停止快照代理、日志读取器代理或队列读取器代理  
  
1.  在左窗格中展开发布服务器组，再展开其中的一个发布服务器，然后单击其中的一个发布。  
  
2.  单击 **“代理”** 选项卡。  
  
3.  右键单击代理，再单击 **“启动代理”** 或 **“停止代理”** 。  
  
## <a name="see-also"></a>另请参阅  
 [监视复制](../../../relational-databases/replication/monitor/monitoring-replication.md)   
 [复制代理可执行文件概念](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)   
 [复制代理概述](../../../relational-databases/replication/agents/replication-agents-overview.md)  
  
  
