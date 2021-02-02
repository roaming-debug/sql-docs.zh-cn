---
description: DROP EVENT NOTIFICATION (Transact-SQL)
title: DROP EVENT NOTIFICATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP EVENT NOTIFICATION
- DROP_EVENT_NOTIFICATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- event notifications [SQL Server], removing
- deleting event notifications
- dropping event notifications
- DROP EVENT NOTIFICATION statement
- removing event notifications
ms.assetid: 0ffd8f47-4ea3-4238-9e73-c318df710cf7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 475349d9b2cf6222bc469fbcab8f4fbef756632e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158617"
---
# <a name="drop-event-notification-transact-sql"></a>DROP EVENT NOTIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从当前数据库中删除事件通知触发器。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
DROP EVENT NOTIFICATION notification_name [ ,...n ]  
ON { SERVER | DATABASE | QUEUE queue_name }  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *notification_name*  
 要删除的事件通知名称。 可以指定多个事件通知。 若要查看当前创建的事件通知列表，请使用 [sys.event_notifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-event-notifications-transact-sql.md)。  
  
 SERVER  
 指示应用于当前服务器的事件通知的作用域。 如果在创建事件通知时指定了 SERVER，则必须指定 SERVER。  
  
 DATABASE  
 指示将事件通知的作用域应用于当前数据库。 如果在创建事件通知时指定了 DATABASE，则必须指定 DATABASE。  
  
 QUEUE queue_name  
 指示将事件通知的作用域应用于由 queue_name 指定的队列。 如果在创建事件通知时指定了 QUEUE，则必须指定 QUEUE。 queue_name 为队列名称，该参数也必须指定。  
  
## <a name="remarks"></a>注解  
 如果事件通知在事务中激发并在同一事务中删除，则发送事件通知实例，然后删除事件通知。  
  
## <a name="permissions"></a>权限  
 若要删除作用域为数据库级别的事件通知，用户至少必须是事件通知的所有者，或必须在当前数据库中拥有 ALTER ANY DATABASE EVENT NOTIFICATION 权限。  
  
 若要删除作用域为服务器级别的事件通知，用户至少必须是事件通知的所有者，或必须在服务器中拥有 ALTER ANY EVENT NOTIFICATION 权限。  
  
 若要删除针对特定队列的事件通知，用户至少必须是事件通知的所有者或必须对父队列拥有 ALTER 权限。  
  
## <a name="examples"></a>示例  
 以下示例创建具有数据库范围内的事件通知，然后删除该通知：  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE EVENT NOTIFICATION NotifyALTER_T1  
ON DATABASE  
FOR ALTER_TABLE  
TO SERVICE 'NotifyService',  
    '8140a771-3c4b-4479-8ac0-81008ab17984';  
GO  
DROP EVENT NOTIFICATION NotifyALTER_T1  
ON DATABASE;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE EVENT NOTIFICATION (Transact-SQL)](../../t-sql/statements/create-event-notification-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.event_notifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-event-notifications-transact-sql.md)   
 [sys.events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-events-transact-sql.md)  
  
  
