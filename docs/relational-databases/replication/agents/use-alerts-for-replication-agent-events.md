---
description: 对复制代理事件使用警报
title: 对复制代理事件使用警报 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing alerts
- Queue Reader Agent, alerts
- alerts [SQL Server replication]
- predefined replication alerts [SQL Server replication]
- Log Reader Agent, alerts
- Distribution Agent, alerts
- Merge Agent, alerts
- agents [SQL Server replication], alerts
- displaying alerts
- Snapshot Agent, alerts
ms.assetid: 8c42e523-7020-471d-8977-a0bd044b9471
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 86c78b601c8a6ac9b62c84c81ba695c61d7e4a81
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475908"
---
# <a name="use-alerts-for-replication-agent-events"></a>对复制代理事件使用警报
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 和 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理提供了使用警报监视事件（如复制代理事件）的方法。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理监视与警报相关联的事件的 Windows 应用程序日志。 如果发生此类事件， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理将通过执行已定义的任务和/或向指定操作员发送电子邮件或寻呼消息的方式自动进行响应。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 包括一组预定义的复制代理警报，您可以配置这些警报以执行某项任务和/或通知某个操作员。 有关如何定义要执行的任务的详细信息，请参阅本主题中的“自动响应警报”部分。  
  
 将计算机配置为分发服务器时，会安装下列警报：  
  
|消息 ID|预定义的警报|激发警报的条件|在 msdb..sysreplicationalerts 中输入其他信息|  
|----------------|----------------------|-----------------------------------------|-----------------------------------------------------------------|  
|14150|**复制: 代理成功**|代理成功关闭。|是|  
|14151|**复制: 代理失败**|代理关闭时出现错误。|是|  
|14152|**复制：代理重试**|代理在重试某项操作失败后关闭（代理遇到服务器不可用、死锁、连接失败或超时故障之类的错误）。|是|  
|14157|**复制：已删除过期的订阅**|已删除过期的订阅。|否|  
|20572|复制：验证失败后重新初始化了订阅|响应作业“数据验证失败时重新初始化订阅”成功重新初始化订阅。|否|  
|20574|复制：订阅服务器未通过数据验证|分发代理或合并代理未通过数据验证。|是|  
|20575|复制：订阅服务器已通过数据验证|分发代理或合并代理通过数据验证。|是|  
|20578|复制：代理自定义关闭|||  
|22815|**对等冲突检测警报**|当分发代理尝试在对等节点上应用更改时检测到冲突。|是|  
  
 除这些警报之外，复制监视器还提供了一组与状态和性能相关的警告和警报。 有关详细信息，请参阅 [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)。 您也可以使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 警报基础结构为其他复制事件定义警报。 有关详细信息，请参阅[创建用户定义事件](../../../ssms/agent/create-a-user-defined-event.md)。  
  
 **配置预定义的复制警报**  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]：[配置预定义的复制警报 (SQL Server Management Studio)](../../../relational-databases/replication/administration/configure-predefined-replication-alerts-sql-server-management-studio.md)  
  
## <a name="viewing-the-application-log-directly"></a>直接查看应用程序日志  
 若要查看 Windows 应用程序日志，请使用 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 事件查看器。 应用程序日志包含 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 错误消息以及计算机上其他许多活动的消息。 与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 错误日志不同，新的应用程序日志不是在每次启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时创建（每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 会话都会在现有应用程序日志中写入新事件）；但是，您可以指定已记录事件的保留时间。 查看 Windows 应用程序日志时，可以筛选特定事件的日志。 有关详细信息，请参阅 Windows 文档。  
  
## <a name="automating-a-response-to-an-alert"></a>自动生成警报响应  
 复制为未通过数据验证的订阅提供了响应作业，同时还提供了用于创建其他自动警报响应的框架。 响应作业的标题为 **“数据验证失败时重新初始化订阅”** ，它存储在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的 **代理的** “作业” [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]文件夹中。 有关启用此响应作业的信息，请参阅[配置预定义的复制警报 (SQL Server Management Studio)](../../../relational-databases/replication/administration/configure-predefined-replication-alerts-sql-server-management-studio.md)。 如果事务发布中的项目未通过验证，响应作业将只重新初始化那些未通过验证的项目。 如果合并发布中的项目未通过验证，响应作业将重新初始化发布中的所有项目。  
  
### <a name="framework-for-automating-responses"></a>用于自动生成响应的框架  
 通常，当发生警报时，能够帮助您理解引起警报的原因以及应采取的适当措施的唯一信息就包含在警报消息中。 分析此信息的过程是一个易出错且费时的过程。 复制在 **sysreplicationalerts** 系统表中提供了有关警报的其他信息，从而使自动生成响应变得更容易，因为系统表中提供的信息已按自定义程序易于使用的格式进行了分析。  
  
 例如，如果订阅服务器 A 上的 **Sales.SalesOrderHeader** 表中的数据未通过验证， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可以激发消息 20574，通知您未通过验证。 您收到的消息是：“订阅服务器‘A’上对发布‘MyPublication’中项目‘SalesOrderHeader’的订阅未通过数据验证”。  
  
 如果基于该消息创建响应，必须手动分析该消息中的订阅服务器名称、项目名称、发布名称和错误。 但是，因为分发代理和合并代理将相同的信息写入了 **sysreplicationalerts** （同时写入的还有代理类型、警报时间、发布数据库、订阅服务器数据库和发布类型等详细信息），所以响应作业可以直接从该表中查询相关的信息。 虽然无法将确切的行与警报的特定实例相关联，但是该表中有一个 **status** 列，可用于跟踪所服务的条目。 此表中的条目会一直保留到历史记录保持期结束。  
  
 例如，如果要在 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 中创建一个为警报消息 20574 服务的响应作业，则可以使用下列逻辑：  
  
```  
declare @publisher sysname, @publisher_db sysname, @publication sysname, @publication_type int, @article sysname, @subscriber sysname, @subscriber_db sysname, @alert_id int  
declare hc cursor local for select publisher, publisher_db, publication, publication_type, article, subscriber,   
  subscriber_db, alert_id from   
  msdb..sysreplicationalerts where  
  alert_error_code = 20574 and status = 0  
  for read only  
open hc  
fetch hc into  @publisher, @publisher_db, @publication, @publication_type, @article, @subscriber, @subscriber_db, @alert_id  
while (@@fetch_status <> -1)  
begin  
/* Do custom work  */  
/* Update status to 1, which means the alert has been serviced. This prevents subsequent runs of this job from doing this again */  
update msdb..sysreplicationalerts set status = 1 where alert_id = @alert_id  
 fetch hc into  @publisher, @publisher_db, @publication, @publication_type, @article, @subscriber, @subscriber_db, @alert_id  
end  
close hc  
deallocate hc  
```  
  
## <a name="see-also"></a>另请参阅  
 [复制代理管理](../../../relational-databases/replication/agents/replication-agent-administration.md)   
 [Best Practices for Replication Administration](../../../relational-databases/replication/administration/best-practices-for-replication-administration.md)   
 [监视（复制）](../../../relational-databases/replication/monitor/monitoring-replication.md)  
  
