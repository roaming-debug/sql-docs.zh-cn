---
title: 查看信息并执行任务（复制监视器）
description: 了解如何在 SQL Server Management Studio (SSMS) 中使用复制监视器查看信息和执行各种任务。
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing publication information
- publications [SQL Server replication], viewing information
- publications [SQL Server replication], Replication Monitor tasks
ms.assetid: 92e28a07-d6a7-461b-a0b3-bd9bc6afcbe5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 24eb21c5907e343ec433316fbdceebfc3864cedc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340749"
---
# <a name="view-information-and-perform-tasks-using-replication-monitor"></a>使用复制监视器查看信息和执行任务
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
复制监视器提供大量选项卡和选项，用于查看信息和执行各种任务。 本文介绍使用复制监视器时可以查看和完成的各种事项。 


## <a name="for-a-publication"></a>针对发布

### <a name="view-information"></a>查看信息
复制监视器提供下列选项卡，其中包括有关选定发布的信息：  
  
-   **所有订阅** - 此选项卡显示有关所选发布的所有订阅信息。  
  
-   **代理** - 此选项卡显示与发布使用的所有代理有关的信息：   
    -   快照代理，用于所有发布。   
    -   日志读取器代理，用于所有事务发布。   
    -   队列读取器代理，用于具有排队更新订阅的事务发布。  
  
-   **警告** - 此选项卡可以为代理指定警告和警报。  
  
-   **跟踪令牌**（仅限事务复制） - 此选项卡可以测量滞后时间，即在发布服务器上提交的事务与在订阅服务器上提交的相应事务之间间隔的时长。  
  
 有关每个选项卡上各选项的详细信息，请选择右窗格中的选项卡，然后在菜单栏上选择“帮助”。 有关启动复制监视器的信息，请参阅[启动复制监视器](../../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
### <a name="perform-tasks"></a>执行任务
  
1.  在左窗格中展开发布服务器组，再展开其中的一个发布服务器，然后选择其中的一个发布。   
2.  要查看和修改发布属性，请右键单击该发布，然后选择“属性”。    
3.  要查看有关订阅的信息，请选择“所有订阅”选项卡，右键单击该订阅，然后选择“属性” 。 您还可以通过该选项卡访问更详细的信息和执行任务。 
4.  要查看有关代理的信息，请选择“代理”选项卡。您还可以通过该选项卡访问更详细的信息和执行任务。 
5.  要查看有关代理警告和阈值的信息，请选择“警告”选项卡。有关详细信息，请参阅 [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)。  
6.  要查看有关跟踪令牌的信息，请选择“跟踪令牌”选项卡。有关如何使用跟踪令牌的详细信息，请参阅 [为事务复制测量滞后时间和验证连接](../../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)。  
  
## <a name="for-a-publisher"></a>针对发布服务器 

### <a name="view-information"></a>查看信息
复制监视器提供了下列选项卡，以显示有关选定发布服务器的信息：   
-   **发布** - 显示选定发布服务器上所有发布的相关信息。   
-   **订阅监视列表** - 显示有关所选发布服务器上所有可用发布中包含错误、警告或最差性能的订阅的信息。 对于运行 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 以前版本的分发服务器，不显示此选项卡。    
-   **代理** 选项卡 - 显示所有类型的复制所使用的代理和作业的详细信息。 使用该选项卡，还可以启动和停止每个代理和作业。 若要查看每个选项卡上各个选项的详细信息，请在右窗格中单击该选项卡，再单击菜单栏上的 **“帮助”** 。 有关启动复制监视器的信息，请参阅[启动复制监视器](../../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
### <a name="perform-tasks"></a>执行任务
  
1.  在左窗格中，展开发布服务器组，然后单击一个发布服务器。   
2.  若要查看所有发布的信息，请单击 **“发布”** 选项卡。    
3.  若要查看有关订阅的信息，请单击 **“订阅监视列表”** 选项卡。还可以从此访问更详细的信息以及执行任务：   
    -   若要查看与订阅相关联的代理的详细信息，请右键单击该订阅，再单击 **“查看详细信息”** 。    
    -   若要查看订阅的属性，请右键单击该订阅，然后单击 **“属性”** 。    
    -   若要同步推送订阅，请右键单击该订阅，然后单击 **“开始同步”** 。   
    -   若要重新初始化订阅，请右键单击该订阅，然后单击 **“重新初始化订阅”** 。   
4.  若要查看有关代理的信息，请单击 **“代理”** 选项卡。您还可以通过此选项卡访问更详细的信息并执行任务：    
    -   若要查看有关代理的详细信息（如信息性消息以及任何错误消息），请右键单击代理，然后单击 **“查看详细信息”** 。  
    -   若要查看有关运行代理的作业的详细信息（如计划、作业步骤详细信息等等），请右键单击代理，然后单击 **“属性”** 。    
    -   若要管理代理的配置文件，请右键单击代理，然后单击 **“代理配置文件”** 。 有关详细信息，请参阅[处理复制代理配置文件](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)。    
    -   若要启动未运行的代理，请右键单击代理，然后单击 **“启动代理”** 。  
    -   若要停止运行中的代理，请右键单击代理，然后单击 **“停止代理”** 。  


## <a name="for-a-subscription"></a>针对订阅 

### <a name="view-information"></a>查看信息
  复制监视器提供下列包含订阅相关信息的选项卡：    
-   **所有订阅** - 显示有关所选发布的所有订阅的信息。   
-   **订阅监视列表** - 显示所选发布服务器上所有可用发布中包含错误、警告或最差性能的订阅的信息。 对于运行 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 以前版本的分发服务器，不显示此选项卡。 有关每个选项卡上各选项的详细信息，请在右窗格中单击相应选项卡，然后在菜单栏上单击 **“帮助”** 。 有关启动复制监视器的信息，请参阅[启动复制监视器](../../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
### <a name="perform-tasks"></a>执行任务
  
1.  在左窗格中展开发布服务器组，再展开其中的一个发布服务器，然后单击其中的一个发布。   
2.  若要查看有关订阅的信息，请单击 **“所有订阅”** 选项卡。若要只查看处于给定状态（如同步）的订阅，请从 **“显示”** 下拉列表中选择一个选项。    
3.  若要查看和修改订阅属性，请右键单击该订阅，然后单击 **“属性”** 。 您还可以通过该选项卡访问更详细的信息和执行任务。 
  
### <a name="to-view-information-and-perform-tasks-for-subscriptions-in-the-subscription-watch-list-tab"></a>在“订阅监视列表”选项卡中查看订阅信息和执行订阅任务  
  
1.  在左窗格中，展开发布服务器组，然后单击一个发布服务器。    
2.  若要查看有关订阅的信息，请单击 **“订阅监视列表”** 选项卡。    
3.  从“显示 \<SubscriptionType> 订阅”下拉列表中选择要显示的订阅类型。 若要只查看处于给定状态（如同步）的订阅，请从 **“显示”** 下拉列表中选择一个选项。    
4.  若要查看和修改订阅属性，请右键单击该订阅，然后单击 **“属性”** 。 您还可以通过该选项卡访问更详细的信息和执行任务。 
  
  
## <a name="for-publication-agents"></a>针对发布代理
复制监视器提供了 **“代理”** 选项卡，其中包含与所选发布关联的代理的相关信息。 分发代理和合并代理均与订阅相关联。 
  
### <a name="view-information"></a>查看信息
 此选项卡显示有关下列代理的信息：  
  
-   快照代理，用于所有发布。  
-   日志读取器代理，用于所有事务发布。  
-   队列读取器代理，用于为排队更新订阅启用的事务发布。 
  
 若要查看有关此选项卡上的选项的详细信息，请在菜单栏上单击 **“帮助”** 。 有关启动复制监视器的信息，请参阅[启动复制监视器](../../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
### <a name="to-view-information-and-perform-tasks-for-the-agents-associated-with-a-publication"></a>查看与发布相关的代理的信息并执行此代理的任务  
  
1.  在左窗格中展开发布服务器组，再展开其中的一个发布服务器，然后单击其中的一个发布。    
2.  单击 **“代理”** 选项卡来查看有关代理的信息。 您还可以通过此选项卡访问更详细的信息并执行任务：    
    -   若要查看有关代理的详细信息（如信息性消息以及任何错误消息），请右键单击代理，然后单击 **“查看详细信息”** 。  
    -   若要查看有关运行代理的作业的详细信息（如计划、作业步骤详细信息等等），请右键单击代理，然后单击 **“属性”** 。    
    -   若要管理代理的配置文件，请右键单击代理，然后单击 **“代理配置文件”** 。 有关详细信息，请参阅[处理复制代理配置文件](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)。   
    -   若要启动未运行的代理，请右键单击代理，然后单击 **“启动代理”** 。   
    -   若要停止运行中的代理，请右键单击代理，然后单击 **“停止代理”** 。  
  
## <a name="for-subscription-agents"></a>针对订阅代理

### <a name="view-information"></a>查看信息
-   **所有订阅** - 显示有关所选发布的所有订阅的信息。  
  
-   **订阅监视列表** - 旨在显示有关所选发布服务器上所有可用发布中包含错误、警告或最差性能的订阅的信息。 对于运行 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 以前版本的分发服务器，不显示此选项卡。 有关每个选项卡上各选项的详细信息，请在右窗格中单击相应选项卡，然后在菜单栏上单击 **“帮助”** 。 有关启动复制监视器的信息，请参阅[启动复制监视器](../../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
### <a name="perform-tasks"></a>执行任务
  
1.  在左窗格中展开发布服务器组，再展开其中的一个发布服务器，然后单击其中的一个发布。    
2.  单击 **“所有订阅”** 选项卡以查看有关订阅的信息。 您还可以通过此选项卡访问更详细的信息并执行任务：   
    -   若要查看与订阅相关联的代理的详细信息，请右键单击该订阅，再单击 **“查看详细信息”** 。 详细信息包括：代理历史记录和错误消息；事务复制的性能统计信息；合并复制的项目级同步统计信息。  
  
         启动的详细信息窗口上显示的选项卡取决于订阅的类型：对于快照订阅，显示的选项卡是 **“分发服务器到订阅服务器的历史记录”** ；对于事务订阅，显示的选项卡有 **“发布服务器到分发服务器的历史记录”** 、 **“发布服务器到订阅服务器的历史记录”** 和 **“未分发的命令”** ；对于合并订阅，显示的选项卡是 **“同步历史记录”** 。  
  
    -   若要同步推送订阅，请右键单击该订阅，然后单击 **“开始同步”** 。    
    -   若要重新初始化订阅，请右键单击该订阅，然后单击 **“重新初始化订阅”** 。    
    -   若要验证单个合并订阅，请右键单击该订阅，再单击 **“验证单个订阅”** 。 若要验证合并发布的所有订阅，请右键单击该发布，再单击 **“验证所有订阅”** ；若要验证事务发布的所有订阅，请右键单击该发布，再单击 **“验证多个订阅”** 。    
    -   若要管理代理的配置文件，请右键单击代理，然后单击 **“代理配置文件”** 。 有关详细信息，请参阅[处理复制代理配置文件](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)。  
  
### <a name="subscription-watch-list-tab"></a>“订阅监视列表”选项卡 
  
1.  在左窗格中，展开发布服务器组，然后单击一个发布服务器。    
2.  单击 **“订阅监视列表”** 选项卡以查看有关订阅的信息。 您还可以通过此选项卡访问更详细的信息并执行任务：   
    -   若要查看与订阅相关联的代理的详细信息，请右键单击该订阅，再单击 **“查看详细信息”** 。 详细信息包括：代理历史记录和错误消息；事务复制的性能统计信息；合并复制的项目级同步统计信息。    
         启动的详细信息窗口上显示的选项卡取决于订阅的类型：对于快照订阅，显示的选项卡是 **“分发服务器到订阅服务器的历史记录”** ；对于事务订阅，显示的选项卡是 **“发布服务器到分发服务器的历史记录”** 、 **“发布服务器到订阅服务器的历史记录”** 和 **“未分发的命令”** ；对于合并订阅，显示的选项卡是 **“同步历史记录”** 。  
  
    -   若要同步推送订阅，请右键单击该订阅，然后单击 **“开始同步”** 。    
    -   若要重新初始化订阅，请右键单击该订阅，然后单击 **“重新初始化订阅”** 。    
    -   若要验证单个合并订阅，请右键单击该订阅，再单击 **“验证单个订阅”** 。 若要验证合并发布的所有订阅，请右键单击该发布，再单击 **“验证所有订阅”** ；若要验证事务发布的所有订阅，请右键单击该发布，再单击 **“验证多个订阅”** 。    
    -   若要管理代理的配置文件，请右键单击代理，然后单击 **“代理配置文件”** 。 有关详细信息，请参阅[处理复制代理配置文件](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)。  

    


## <a name="see-also"></a>另请参阅  
 [查看和修改发布属性](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [查看和修改推送订阅属性](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [查看和修改请求订阅属性](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)  
 [在复制监视器中设置阈值和警告](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)   
 [复制代理管理](../../../relational-databases/replication/agents/replication-agent-administration.md)    
  
  
