---
description: CREATE EVENT NOTIFICATION (Transact-SQL)
title: CREATE EVENT NOTIFICATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_EVENT_NOTIFICATION_TSQL
- NOTIFICATION_TSQL
- EVENT
- NOTIFICATION
- CREATE EVENT NOTIFICATION
- EVENT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EVENT NOTIFICATION statement
- events [SQL Server], notifications
- event notifications [SQL Server], creating
ms.assetid: dbbff0e8-9e25-4f12-a1ba-e12221d16ac2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2f4f4863a468f5f7d0da39cc578c8f4f865f7828
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186870"
---
# <a name="create-event-notification-transact-sql"></a>CREATE EVENT NOTIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  创建可向 Service Broker 服务发送有关数据库或服务器事件的信息的对象。 只能使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句创建事件通知。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE EVENT NOTIFICATION event_notification_name   
ON { SERVER | DATABASE | QUEUE queue_name }   
[ WITH FAN_IN ]  
FOR { event_type | event_group } [ ,...n ]  
TO SERVICE 'broker_service' , { 'broker_instance_specifier' | 'current database' }  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 event_notification_name  
 事件通知名。 事件通知名必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则，且必须在创建事件通知的作用域内唯一：SERVER、DATABASE 或 object_name。  
  
 SERVER  
 将事件通知的作用域应用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的当前实例。 如果已指定，则只要 FOR 子句中的指定事件在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例中发生，便会激发通知。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
 DATABASE  
 将事件通知的作用域应用于当前数据库。 如果已指定，则只要 FOR 子句中的指定事件在当前数据库中发生，便会激发通知。  
  
 QUEUE  
 将通知的作用域应用于当前数据库中的特定队列。 仅已指定 FOR QUEUE_ACTIVATION 或 FOR BROKER_QUEUE_DISABLED 时，才能指定 QUEUE。  
  
 queue_name  
 应用事件通知的队列的名称。 仅在指定了 QUEUE 时才能指定 queue_name。  
  
 WITH FAN_IN  
 指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对于下列所有事件通知，仅为每个事件向所有指定服务发送一条消息：  
  
-   为同一事件创建。  
  
-   由同一主体事件（由相同 SID 标识）创建。  
  
-   指定的相同服务和 broker_instance_specifier。  
  
-   指定 WITH FAN_IN。  
  
 例如，创建了三个事件通知。 所有事件通知指定 FOR ALTER_TABLE、WITH FAN_IN、相同的 TO SERVICE 子句，并且使用相同的 SID 创建。 运行 ALTER TABLE 语句时，这三个事件通知创建的消息将合并为一条消息。 因此，目标服务只接收一条事件消息。  
  
 event_type  
 导致执行事件通知的事件类型的名称。 event_type 可以为 [!INCLUDE[tsql](../../includes/tsql-md.md)] DDL 事件类型、SQL 跟踪事件类型或 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 事件类型。 有关限定 [!INCLUDE[tsql](../../includes/tsql-md.md)] DDL 事件类型的列表，请参阅 [DDL 事件](../../relational-databases/triggers/ddl-events.md)。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 事件类型为 QUEUE_ACTIVATION 和 BROKER_QUEUE_DISABLED。 有关详细信息，请参阅 [Event Notifications](../../relational-databases/service-broker/event-notifications.md)。  
  
 event_group  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 或 SQL 跟踪事件类型的预定义组的名称。 可在执行属于事件组的任何事件后激发事件通知。 有关 DDL 事件组、事件组包含的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 事件以及可定义事件组的作用域的列表，请参阅 [DDL 事件组](../../relational-databases/triggers/ddl-event-groups.md)。  
  
 当 CREATE EVENT NOTIFICATION 语句完成时，event_group 通过将其涵盖的事件类型添加到 sys.events 目录视图中，还可作为宏使用。  
  
 **'** *broker_service* **'**  
 指定接收事件实例数据的目标服务。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为事件通知打开一个或多个与目标服务的会话。 该服务必须具有用于发送消息的相同 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件消息类型和约定。  
  
 在删除事件通知前，会话一直保持打开状态。 某些错误可能导致会话提前关闭。 显式结束部分或全部会话可能导致目标服务无法接收更多消息。  
  
 { **'** _broker\_instance\_specifier_ **'**  |  **'current database'** }  
 指定解析 broker_service 所依据的 *Service Broker* 实例。 特定 Service Broker 的值可通过查询 sys.databases 目录视图的 service_broker_guid 列来获取。 使用 'current database' 在当前数据库中指定 Service Broker 实例。 'current database' 是不区分大小写的文字字符串。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 包括事件通知专用的消息类型和约定。 所以，无需创建 Service Broker 启动服务，因为已存在指定以下约定名称的启动服务：`https://schemas.microsoft.com/SQL/Notifications/PostEventNotification`  
  
 接收事件通知的目标服务必须使用此预先存在的约定。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssSB](../../includes/sssb-md.md)] 对话安全模式。 必须根据完全安全模式手动配置对话安全设置。 有关详细信息，请参阅[配置事件通知的对话安全模式](../../relational-databases/service-broker/configure-dialog-security-for-event-notifications.md)。  
  
 如果激活通知的事件事务被回滚，则事件通知的发送也被回滚。 如果在触发器内提交或回滚事务，则触发器中定义的操作不会激发事件通知。 由于跟踪事件不与事务绑定，所以无论激活事件通知的事务是否被回滚，都会发送基于跟踪事件的事件通知。  
  
 如果服务器与目标服务之间的会话在激发事件通知后中断，则将报告一个错误并删除该事件通知。  
  
 事件通知是否成功发送不会影响最初启动该通知的事件事务。  
  
 将记录发送事件通知时的所有失败。  
  
## <a name="permissions"></a>权限  
 若要创建以数据库为作用域 (ON DATABASE) 的事件通知，需要在当前数据库中具有 CREATE DATABASE DDL EVENT NOTIFICATION 权限。  
  
 若要对以服务器为作用域 (ON SERVER) 的 DDL 语句创建事件通知，需要在该服务器中具有 CREATE DDL EVENT NOTIFICATION 权限。  
  
 若要对跟踪事件创建事件通知，需要在服务器中具有 CREATE TRACE EVENT NOTIFICATION 权限。  
  
 若要创建以队列为作用域的事件通知，需要对该队列具有 ALTER 权限。  
  
## <a name="examples"></a>示例  
  
> [!NOTE]  
>  在下面的示例 A 和 B 中，`TO SERVICE 'NotifyService'` 子句中的 GUID ('8140a771-3c4b-4479-8ac0-81008ab17984') 特定于设置相应示例的计算机。 对于该实例，它是 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 GUID。  
>   
>  若要复制和运行这些示例，需要将此 GUID 替换为您的计算机和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的 GUID。 如上面的“参数”部分所述，可以通过查询 sys.databases 目录视图的 service_broker_guid 列获取 'broker\_instance\_specifier'。  
  
### <a name="a-creating-an-event-notification-that-is-server-scoped"></a>A. 创建服务器范围的事件通知  
 以下示例创建使用 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 设置目标服务时所需的对象。 该目标服务引用专用于事件通知的启动服务的消息类型和约定。 然后对该目标服务创建一个事件通知，只要 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例发生 `Object_Created` 跟踪事件，便会发送一个通知。  
  
```sql  
--Create a queue to receive messages.  
CREATE QUEUE NotifyQueue ;  
GO  

--Create a service on the queue that references  
--the event notifications contract.  
CREATE SERVICE NotifyService  
ON QUEUE NotifyQueue  
([https://schemas.microsoft.com/SQL/Notifications/PostEventNotification]);  
GO  

--Create a route on the service to define the address   
--to which Service Broker sends messages for the service.  
CREATE ROUTE NotifyRoute  
WITH SERVICE_NAME = 'NotifyService',  
ADDRESS = 'LOCAL';  
GO 

--Create the event notification.  
CREATE EVENT NOTIFICATION log_ddl1   
ON SERVER   
FOR Object_Created   
TO SERVICE 'NotifyService',  
    '8140a771-3c4b-4479-8ac0-81008ab17984' ;  
```  
  
### <a name="b-creating-an-event-notification-that-is-database-scoped"></a>B. 创建以数据库为作用域的事件通知  
 以下示例将在上例所涉及的同一目标服务上创建事件通知。 在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 示例数据库上发生 `ALTER_TABLE` 事件后会激发该事件通知。  
  
```sql  
CREATE EVENT NOTIFICATION Notify_ALTER_T1  
ON DATABASE  
FOR ALTER_TABLE  
TO SERVICE 'NotifyService',  
    '8140a771-3c4b-4479-8ac0-81008ab17984';  
```  
  
### <a name="c-getting-information-about-an-event-notification-that-is-server-scoped"></a>C. 获取有关服务器范围的事件通知的信息  
 以下示例查询 `sys.server_event_notifications` 目录视图，以获取以服务器为作用域创建的事件通知 `log_ddl1` 的元数据。  
  
```sql  
SELECT * FROM sys.server_event_notifications  
WHERE name = 'log_ddl1';  
```  
  
### <a name="d-getting-information-about-an-event-notification-that-is-database-scoped"></a>D. 获取有关以数据库为作用域的事件通知的信息  
 以下示例查询 `sys.event_notifications` 目录视图，以获取以数据库为作用域创建的事件通知 `Notify_ALTER_T1` 的元数据。  
  
```sql  
SELECT * FROM sys.event_notifications  
WHERE name = 'Notify_ALTER_T1';  
```  
  
## <a name="see-also"></a>另请参阅  
 [事件通知](../../relational-databases/service-broker/event-notifications.md)   
 [DROP EVENT NOTIFICATION (Transact-SQL)](../../t-sql/statements/drop-event-notification-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.event_notifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-event-notifications-transact-sql.md)   
 [sys.server_event_notifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-event-notifications-transact-sql.md)   
 [sys.events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-events-transact-sql.md)   
 [sys.server_events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-events-transact-sql.md)  
  
  
