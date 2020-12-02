---
description: BEGIN CONVERSATION TIMER (Transact-SQL)
title: BEGIN CONVERSATION TIMER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CONVERSATION
- BEGIN_CONVERSATION_TSQL
- TIMER_TSQL
- TIMER
- CONVERSATION TIMER
- CONVERSATION_TIMER_TSQL
- BEGIN CONVERSATION TIMER
- BEGIN_CONVERSATION_TIMER_TSQL
- CONVERSATION_TSQL
- BEGIN CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN CONVERSATION TIMER statement
- DialogTimer message
- dialogs [Service Broker], beginning
- timeouts [Service Broker]
- timer messages [Service Broker]
- conversations [Service Broker], timers
- starting timers [Service Broker]
- https://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer message
ms.assetid: 98e49b3f-a38f-4180-8171-fa9cb30db4cb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dc2b02a96911dc0ff6e7be86c5cbd36708ddf348
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124170"
---
# <a name="begin-conversation-timer-transact-sql"></a>BEGIN CONVERSATION TIMER (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  启动计时器。 当超时到期时，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 为对话在本地队列上放入 `https://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer` 类型的消息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
BEGIN CONVERSATION TIMER ( conversation_handle )  
   TIMEOUT = timeout   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 BEGIN CONVERSATION TIMER (conversation\_handle)  
 指定要计时的会话。 conversation_handle 的类型必须为 uniqueidentifier。  
  
 TIMEOUT  
 指定在将消息放入队列之前要等待的时间，以秒为单位。  
  
## <a name="remarks"></a>备注  
 会话计时器为应用程序提供了一种方式，可以在指定时间后接收与某个会话有关的消息。 如果在计时器过期前对会话调用 BEGIN CONVERSATION TIMER，可将超时设置为新值。 与会话生存期不同，会话双方的会话计时器彼此独立。 DialogTimer 消息到达本地队列时，不会影响会话的远程端。 因此，应用程序可以将计时器消息用于任何目的。  
  
 例如，可以使用会话计时器避免应用程序过久地等待过期响应。 如果希望应用程序在 30 秒内完成对话，则可将对话的会话计时器设置为 60 秒（30 秒加 30 秒的宽限期）。 如果对话 60 秒后仍处于打开状态，则应用程序将收到一条关于该对话队列的超时消息。  
  
 此外，应用程序可以使用会话计时器请求在特定时间进行激活。 例如，可以创建一个服务，每隔几分钟报告活动连接数；或者创建一种服务，每天晚上报告未清的采购订单。 该服务将会话计时器设置为在所需时间过期；计时器过期时，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 将发送一条 DialogTimer 消息。 DialogTimer 消息导致 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 启动队列的激活存储过程。 该存储过程将向远程服务发送一条消息，并重新启动会话计时器。  
  
 BEGIN CONVERSATION TIMER 在用户定义函数中无效。  
  
## <a name="permissions"></a>权限  
 在默认情况下，对会话服务具有 SEND 权限的用户、sysadmin 固定服务器角色的成员和 db_owner 固定数据库角色的成员具有设置会话计时器的权限。  
  
## <a name="examples"></a>示例  
 以下示例为由 `@dialog_handle` 标识的对话设置两分钟的超时。  
  
```sql 
-- @dialog_handle is of type uniqueidentifier and  
-- contains a valid conversation handle.  
  
BEGIN CONVERSATION TIMER (@dialog_handle)  
TIMEOUT = 120 ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [BEGIN DIALOG CONVERSATION (Transact-SQL)](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [END CONVERSATION (Transact-SQL)](../../t-sql/statements/end-conversation-transact-sql.md)   
 [RECEIVE (Transact-SQL)](../../t-sql/statements/receive-transact-sql.md)  
  
  
