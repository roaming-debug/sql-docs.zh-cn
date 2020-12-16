---
description: 分发服务器信息，发布
title: 分发服务器信息，发布 | Microsoft Docs
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.monitor.Distributor.Publications.f1
- sql13.rep.monitor.Distributor.commonjobs..f1
- sql13.rep.monitor.Distributor.SubscriptionSummary.merge.f1
- sql13.rep.monitor.Distributor.SubscriptionSummary.snapshot.f1
- sql13.rep.monitor.Distributor.SubscriptionSummary.tran.f1
ms.assetid: 1f499277-7f12-42ba-8cf4-52b683434944
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: b5fb4f1788ed3812490ca0e730f854b18c64e6ad
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464788"
---
# <a name="distributor-information-publications"></a>分发服务器信息，发布
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  **“发布”** 选项卡提供左窗格中所选分发服务器的所有发布的摘要信息。  
  
显示的有关分发服务器支持的发布信息包括一个列，该列包含发布服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 否则，发布信息与您在复制监视器的发布服务器视图中查看发布时提供的发布信息相同。 有关 **“发布”** 选项卡中的列的详细信息，请参阅 [Publisher Information, Publications](../../relational-databases/replication/publisher-information-publications.md)。  

## <a name="subscription-watch-list"></a>订阅监视列表

### <a name="transactional-replication"></a>事务复制
  用于事务订阅的信息包括发布服务器的名称。 否则，在此对话框中提供的功能和信息与发布服务器视图中的功能和信息相同。 有关如何使用此对话框的详细信息，请参阅[发布服务器信息，订阅监视列表（事务发布，SQL Server 2005 和更高版本）](../../relational-databases/replication/publisher-information-subscription-watch-list-transactional.md)。 

### <a name="merge-replication"></a>合并复制
  用于合并发布订阅的信息包括发布服务器的名称。 否则，在此对话框中提供的功能和信息与发布服务器视图中的功能和信息相同。 有关如何使用此对话框的详细信息，请参阅[发布服务器信息，订阅监视列表（合并发布，SQL Server 2005 和更高版本）](../../relational-databases/replication/publisher-information-subscription-watch-list-merge-publication.md)。  

### <a name="snapshot-replication"></a>快照复制 
  用于快照发布订阅的信息包括发布服务器的名称。 否则，在此对话框中提供的功能和信息与发布服务器视图中的功能和信息相同。 有关如何使用此对话框的详细信息，请参阅[发布服务器信息，订阅监视列表（快照发布，SQL Server 2005 和更高版本）](../../relational-databases/replication/publisher-information-subscription-watch-list-snapshot.md)。  

## <a name="agents"></a>代理
“代理”选项卡显示与发布服务器和订阅服务器关联的代理和维护作业的相关信息。  
  
 在分发服务器视图中分发服务器的 **“代理”** 选项卡中可用的代理包括在发布服务器的 **“代理”** 选项卡中可用的所有代理。 不过，分发服务器视图中分发服务器的 **“代理”** 选项卡还包括一个分发服务器代理和一个合并代理。  
  
 有关快照代理、日志读取器代理和队列读取器代理的详细信息，请参阅 [Publisher Information, Agents](../../relational-databases/replication/publisher-information-agents.md)。 请注意，当您查看分发服务器的 **“代理”** 选项卡上的代理信息时，将提供快照代理和日志读取器代理的发布服务器信息。 不过，在分发服务器视图中分发服务器的 **“代理”** 选项卡中，您还可以选择 **“分发服务器代理”** 和 **“合并代理”**。  
  
### <a name="options"></a>选项  
 以下各节说明了此选项卡上为分发服务器代理和合并代理显示的数据。  
  
### <a name="distributor-agent"></a>“分发服务器代理”  
 **Status**  
 此代理的状态。 下面列出了可能的状态值：  
  
-   错误    
-   重试    
-   运行    
-   未运行   
-   从未启动  
  
 **发布者**  
 发布服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
 **发布**  
 与此代理关联的发布的名称。  
  
 **订阅**  
 订阅的名称，格式为：[*SubscriberName*].[*Database*]。  
  
 **类型**  
 复制的类型：推送、请求或匿名。  
  
 **上次启动时间**  
 上次启动此代理的时间。  
  
 **Duration**  
 此代理已经运行的时间长度。 如果此代理当前正在运行，该时间表示已运行的时间；如果此代理之前已运行完毕，则该时间表示总时间。  
  
 **上一操作**  
 在此代理最近一次运行的过程中最后执行的操作。  
  
 **传送速率**  
 在此代理最近一次运行期间分发数据库中提交初始化命令的速率，以每秒的命令数表示。  
  
 **延迟**  
 从在发布数据库中提交最近的更改到在分发数据库中提交对应的命令经过的时间，以秒为单位。  
  
 **事务数**  
 在此代理最近一次运行期间分发数据库中提交的事务数。  
  
 **命令数**  
 在此代理最近一次运行期间分发数据库中提交的命令数。 一个命令与一次数据更改（如一次更新）相同。  
  
 **平均命令数**  
 在此代理最近一次运行期间平均每个事务的命令数。  
  
### <a name="merge-agent"></a>合并代理  
 **Status**  
 此代理的状态。 下面列出了可能的状态值：  
  
-   错误  
  
-   重试  
  
-   运行  
  
-   未运行  
  
-   从未启动  
  
 **发布者**  
 发布服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
 **发布**  
 与此代理关联的发布的名称。  
  
 **订阅**  
 订阅的名称，格式为：[*SubscriberName*].[*Database*]。  
  
 **类型**  
 复制的类型：推送、请求或匿名。  
  
 **上次启动时间**  
 上次启动此代理的时间。  
  
 **Duration**  
 此代理已经运行的时间长度。 如果此代理当前正在运行，该时间表示已运行的时间；如果此代理之前已运行完毕，则该时间表示总时间。  
  
 **上一操作**  
 在此代理最近一次运行的过程中最后执行的操作。  
  
 **传送速率**  
 在分发数据库中提交更改的速率，以每秒的命令数为单位。  
  
 **发布服务器插入**  
 发布服务器上应用的 INSERT 命令数。  
  
 **发布服务器更新**  
 发布服务器上应用的 UPDATE 命令数。  
  
 **发布服务器删除**  
 发布服务器上应用的 DELETE 命令数。  
  
 **发布服务器冲突**  
 合并过程中发布服务器上发生的冲突数。  
  
 **订阅服务器插入**  
 订阅服务器上应用的 INSERT 命令数。  
  
 **订阅服务器更新**  
 订阅服务器上应用的 UPDATE 命令数。  
  
 **订阅服务器删除**  
 订阅服务器上应用的 DELETE 命令数。  
  
 **订阅服务器冲突**  
 合并过程中订阅服务器上发生的冲突数。  

  
## <a name="see-also"></a>另请参阅  
 [启动复制监视器](../../relational-databases/replication/monitor/start-the-replication-monitor.md)   
 [监视复制](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)  
  
  
