---
title: SQL Server Service Broker | Microsoft Docs
description: 了解 Service Broker。 了解它如何为 SQL Server 数据库引擎和 Azure SQL 托管实例中的消息提供本机支持。
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.SSBMSGTYPEPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBCONTRACTPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBQUEUEPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBREMSVCBINDPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBROUTEPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBPRIORITYPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBSERVICEPROPERTIES.GENERAL.F1
helpviewer_keywords:
- Broker See Service Broker
- SQL Server Service Broker
- Service Broker
ms.assetid: 8b8b3b57-fd46-44de-9a4e-e3a8e3999c1e
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: cf37305f773f4b417ed3cac1bc5a31ad8d910505
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465698"
---
# <a name="service-broker"></a>Service Broker
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSB](../../includes/sssb-md.md)] 为 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 和 [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance-index)中的消息传递和队列提供本机支持。 开发人员可轻松创建通过 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 组件在不同数据库之间通信的复杂应用程序，也可构建分发式应用程序和可靠的应用程序。  
  
## <a name="when-to-use-service-broker"></a>何时使用 Service Broker

 使用 Service Broker 组件实现本机数据库内异步消息处理功能。 使用 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 的应用程序开发人员无需编写复杂的内部通信和消息，即可跨多个数据库分发数据工作负荷。 由 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 处理对话上下文中的通信路径，因此 Service Broker 可减少开发和测试工作。 同时还提高了性能。 例如，支持网站的前端数据库可以记录信息，并发送处理密集型任务以便在后端数据库中进行排队。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 确保在事务上下文中管理所有任务，以确保可靠性和技术一致性。  
  
## <a name="overview"></a>概述

  Service Broker 是一种消息传递框架，可用于创建本机数据库内面向服务的应用程序。 与在查询生命周期期间不断从表中读取数据的经典查询处理功能不同，面向服务的应用程序中提供可交换消息的数据库服务。 每个服务都有一个队列，消息在处理之前都排在队列中。
  
![服务代理](media/service-broker.png)
  
  可使用 Transact-SQL `RECEIVE` 命令或通过每次消息到达队列时都要调用的激活过程来提取队列中的消息。
  
### <a name="creating-services"></a>创建服务
 
  通过使用 [CREATE SERVICE](../../t-sql/statements/create-service-transact-sql.md) Transact SQL 语句创建数据库服务。 可将服务与通过 [CREATE QUEUE](../../t-sql/statements/create-queue-transact-sql.md) 语句创建的消息队列相关联：
  
```sql
CREATE QUEUE dbo.ExpenseQueue;
GO
CREATE SERVICE ExpensesService
    ON QUEUE dbo.ExpenseQueue; 
```

### <a name="sending-messages"></a>发送消息
  
  使用 [SEND](../../t-sql/statements/send-transact-sql.md) Transact-SQL 语句在服务之间的对话上发送消息。 对话是通过 `BEGIN DIALOG` Transact-SQL 语句在服务之间建立的信道。 
  
```sql
DECLARE @dialog_handle UNIQUEIDENTIFIER;

BEGIN DIALOG @dialog_handle  
FROM SERVICE ExpensesClient  
TO SERVICE 'ExpensesService';  
  
SEND ON CONVERSATION @dialog_handle (@Message) ;  
```
   消息将发送到 `ExpenssesService` 并置于 `dbo.ExpenseQueue` 中。 由于没有与此队列关联的激活过程，因此在有人读取消息之前，该消息将保留在队列中。

### <a name="processing-messages"></a>处理消息

   可使用标准 `SELECT` 查询选择放置在队列中的消息。 `SELECT` 语句将不修改队列，也不删除消息。 要读取和拉取队列中的消息，可使用 [RECEIVE](../../t-sql/statements/receive-transact-sql.md) Transact-SQL 语句。

```sql
RECEIVE conversation_handle, message_type_name, message_body  
FROM ExpenseQueue; 
```

  处理队列中的所有消息之后，应使用 [END CONVERSATION](../../t-sql/statements/end-conversation-transact-sql.md) Transact-SQL 语句关闭对话。

## <a name="where-is-the-documentation-for-service-broker"></a>Service Broker 文档在哪里？  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 的参考文档位于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 文档中。 本参考文档包含以下各节：  
  
-   适用于 CREATE, ALTER 和 DROP 语句的[数据定义语言 (DDL) 语句 (Transact-SQL)](../../t-sql/statements/statements.md)  
  
-   [Service Broker 语句](../../t-sql/statements/statements.md)  
  
-   [Service Broker 目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/service-broker-catalog-views-transact-sql.md)  
  
-   [与 Service Broker 有关的动态管理视图 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
-   [ssbdiagnose 实用工具 (Service Broker)](../../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md)  
  
 有关 [概念以及开发和管理任务，请参阅](/previous-versions/sql/sql-server-2008-r2/bb522893(v=sql.105)) 以前发布的文档 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 。 由于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 中的更改数量少，因此未在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]文档中重新生成该文档。  
  
## <a name="whats-new-in-service-broker"></a>Service Broker 新增功能  
 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]未引入任何重大更改。  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]中引入了以下更改。  

### <a name="service-broker-and-azure-sql-managed-instance"></a>Service Broker 和 Azure SQL 托管实例

- 不支持跨实例 Service Broker 
 - `sys.routes` -先决条件：通过 sys.routes 选择地址。 在每个路由上，地址必须是本地的。 请参阅 [sys.routes](../../relational-databases/system-catalog-views/sys-routes-transact-sql.md)。
 - `CREATE ROUTE` - 不能使用除 `LOCAL` 以外的 `ADDRESS` 执行 `CREATE ROUTE`。 请参阅 [CREATE ROUTE](../../t-sql/statements/create-route-transact-sql.md)。
 - `ALTER ROUTE` 不能结合使用 `ALTER ROUTE` 和 `ADDRESS`（`LOCAL` 除外）。 请参阅 [ALTER ROUTE](../../t-sql/statements/alter-route-transact-sql.md)。  
  
### <a name="messages-can-be-sent-to-multiple-target-services-multicast"></a>可以将消息发送到多个目标服务（多播）  
 通过支持多个会话句柄，扩展了 [SEND (Transact-SQL)](../../t-sql/statements/send-transact-sql.md) 语句的语法以启用多播。  
  
### <a name="queues-expose-the-message-enqueued-time"></a>队列将公开此消息排队时间  
 队列具有一个新列 **message_enqueue_time**，用于显示消息已在队列中待了多少时间。  
  
### <a name="poison-message-handling-can-be-disabled"></a>可以禁用有害消息处理  
 现在，[CREATE QUEUE (Transact-SQL)](../../t-sql/statements/create-queue-transact-sql.md) 和 [ALTER QUEUE (Transact-SQL)](../../t-sql/statements/alter-queue-transact-sql.md) 语句可以通过添加子句 `POISON_MESSAGE_HANDLING (STATUS = ON | OFF)` 来启用或禁用有害消息处理。 目录视图 **sys.service_queues** 现在具有列 **is_poison_message_handling_enabled** ，以指示是启用还是禁用有害消息。  
  
### <a name="always-on-support-in-service-broker"></a>Service Broker 中的 AlwaysOn 支持  
 有关详细信息，请参阅 [Service Broker 与 AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/service-broker-with-always-on-availability-groups-sql-server.md)。  
  
  
## <a name="next-steps"></a>后续步骤

Service Broker 的最常见用途是[事件通知](../../relational-databases/service-broker/event-notifications.md)。 了解如何[实现事件通知](../../relational-databases/service-broker/implement-event-notifications.md)、[配置对话框安全性](../../relational-databases/service-broker/configure-dialog-security-for-event-notifications.md)或[获取详细信息](../../relational-databases/service-broker/get-information-about-event-notifications.md)。