---
title: 复制代理管理 | Microsoft Docs
description: 了解如何管理执行复制任务（例如创建架构和数据的副本以及在服务器之间传播更改）的复制代理。
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Snapshot Agent, administering
- Log Reader Agent, administering
- Queue Reader Agent, administering
- shared agents [SQL Server replication]
- Merge Agent, administering
- Distribution Agent, administering
- agents [SQL Server replication], administering
- replication cleanup jobs [SQL Server]
- administering replication, agents
- replication [SQL Server], administering
- independent agents [SQL Server replication]
ms.assetid: f27186b8-b1b2-4da0-8b2b-91f632c2ab7e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 76c793758fa9886f0f8246c5d6c4aa7ec27fa707
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467328"
---
# <a name="replication-agent-administration"></a>复制代理管理
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  复制代理执行许多与复制有关的任务，其中包括创建架构和数据副本、检测发布服务器或订阅服务器上的更新以及在服务器之间传播更改。 默认情况下，复制代理在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理作业步骤下运行。 由于这些代理完全是可执行文件，因此可以从命令行和批处理脚本直接调用它们。 每个复制代理支持一组运行时参数，用于控制代理的运行方式；这些参数在代理配置文件或命令行中指定。  
  
> [!IMPORTANT]  
>  默认情况下，安装完 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之后， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理服务处于禁用状态，除非在安装过程中明确选择自动启动该服务。  
  
 复制代理文件位于 [!INCLUDE[ssInstallPathVar](../../../includes/ssinstallpathvar-md.md)]\COM 目录下。 下表列出了可执行复制的名称和文件名。 单击代理链接可查看其参数引用。  
  
|可执行代理|文件名|  
|----------------------|---------------|  
|[Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)|snapshot.exe|  
|[Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)|distrib.exe|  
|[复制日志读取器代理](../../../relational-databases/replication/agents/replication-log-reader-agent.md)|logread.exe|  
|[复制队列读取器代理](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)|qrdrsvc.exe|  
|[Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)|replmerg.exe|  
  
 除了复制代理之外，复制还有大量执行计划维护和按需维护的作业。  
  
 **运行代理和运行维护作业**  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 和复制监视器：[启动和停止复制代理 (SQL Server Management Studio)](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)  
  
-   复制编程：[Replication Agent Executables Concepts](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  
## <a name="agent-profiles"></a>代理配置文件  
 在配置复制时，将在分发服务器上安装一组代理配置文件。 代理配置文件包含一组在代理每次运行时都要使用的参数：在代理启动过程中，每个代理都会登录到分发服务器，并查询其配置文件中的参数。 复制为每个代理提供一个默认的配置文件，同时还为日志读取器代理、分发代理和合并代理提供附加的预定义配置文件。 除了所提供的配置文件之外，您还可以创建符合自己应用程序要求的配置文件。 有关详细信息，请参阅 [Replication Agent Profiles](../../../relational-databases/replication/agents/replication-agent-profiles.md)。  
  
 有关如何直接指定命令行参数的信息，请参阅[复制代理可执行文件概念](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)。  
  
## <a name="monitoring-replication-agents"></a>监视复制代理  
 复制监视器使您可以查看信息和执行与每个复制代理关联的任务。 以下列表列出了每个代理、可以在其中找到代理的选项卡（位于复制监视器中）以及指向说明如何访问这些选项卡的主题的链接：  
  
-   以下代理与复制监视器中的发布相关联：  
  
    -   快照代理  
  
    -   日志读取器代理  
  
    -   队列读取器代理  
  
     通过 **“代理”** 选项卡访问与这些代理有关的信息和任务。有关详细信息，请参阅“使用 [复制监视器](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)查看信息和执行任务”。  
  
-   以下代理与复制监视器中的订阅相关联：  
  
    -   分发代理  
  
    -   合并代理  
  
     通过以下选项卡访问与这些代理相关联的信息和任务：“订阅监视列表”（所有发布服务器都提供）或“所有订阅”选项卡（所有发布都提供） 。 有关详细信息，请参阅[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。  
  
## <a name="independent-and-shared-agents"></a>独立代理和共享代理  
 独立代理是为一个订阅提供服务的代理。 共享代理为多个订阅提供服务；如果使用同一共享代理的多个订阅需要同步，则在默认情况下，它们会排队等候，共享代理一次为一个订阅提供服务。 使用独立代理会减少滞后时间，因为只要订阅需要同步，独立代理就可以为其提供服务。 合并复制始终使用独立代理，而事务复制在默认情况下为在新建发布向导中创建的发布使用独立代理（在早期的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]版本中，事务复制在默认情况下使用共享代理）。  
  
## <a name="replication-maintenance-jobs"></a>复制维护作业  
 复制使用下列作业来执行计划维护和按需维护。  
  
|清除作业|说明|默认计划|  
|------------------|-----------------|----------------------|  
|清除代理历史记录：分发|从分发数据库中删除复制代理历史记录。|每十分钟运行一次|  
|清除分发：分发|从分发数据库中删除复制的事务。 |每十分钟运行一次|  
|过期订阅清除|从发布数据库检测和删除过期的订阅。 在分发服务器上停用在最大分发保持期内尚未同步的订阅。|每天凌晨 1:00 运行| 
|重新初始化数据验证失败的订阅|检测所有未通过数据验证的订阅并标记它们以进行重新初始化。 下次合并代理或分发代理运行时，订阅服务器上将应用新快照。|无默认调度（默认情况下未启用）。|  
|复制代理检查|检测未积极记录历史信息的复制代理。 如果作业步骤失败，它将写入 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 事件日志。|每十分钟运行一次。|  
|分发的复制监视刷新器|刷新复制监视器所使用的缓存的查询。|连续运行。|  
  
## <a name="see-also"></a>另请参阅  
 [监视复制](../../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
