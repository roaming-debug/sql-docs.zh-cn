---
description: Broker:Conversation 事件类
title: Broker:Conversation 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Broker:Conversation event class
ms.assetid: 784707b5-cc67-46a3-8ae6-8f8ecf4b27c0
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f77b2cff9793f7e6be200423dd6bc23377814635
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468018"
---
# <a name="brokerconversation-event-class"></a>Broker:Conversation 事件类

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成 **Broker:Conversation** 事件来报告 Service Broker 会话的进度。  
  
## <a name="brokerconversation-event-class-data-columns"></a>Broker:Conversation 事件类的数据列  
  
|数据列|类型|说明|列号|可筛选|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 该列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|**ClientProcessID**|**int**|由主机分配给正在运行客户端应用程序的进程的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|**DatabaseID**|**int**|由 USE *database* 语句指定的数据库的 ID。 如果未发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 **DB_ID** 函数来确定数据库的值。|3|是|  
|**EventClass**|**int**|捕获的事件类的类型。 对于 **Broker:Conversation** ，始终为 **124**。|27|否|  
|**EventSequence**|**int**|此事件的序列号。|51|否|  
|**EventSubClass**|**nvarchar**|事件子类的类型。 它提供了有关每个事件类的详细信息。|21|是|  
|**GUID**|**uniqueidentifier**|对话的会话 ID。 此标识符将作为消息的一部分进行传输，并在会话双方之间共享。|54|否|  
|**HostName**|**nvarchar**|正在运行客户端程序的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 **HOST_NAME** 函数。|8|是|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。<br /><br /> 0 = 用户<br /><br /> 1 = 系统|60|否|  
|**LoginSid**|**图像**|已登录用户的安全标识号 (SID)。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|**MethodName**|**nvarchar**|会话所属的会话组。|47|否|  
|**NTDomainName**|**nvarchar**|用户所属的 Windows 域。|7|是|  
|**NTUserName**|**nvarchar**|拥有生成此事件的连接的用户的名称。|6|是|  
|**ObjectName**|**nvarchar**|对话的会话句柄。|34|否|  
|**Priority**|**int**|会话的优先级|5|是|  
|**RoleName**|**nvarchar**|会话句柄的角色。 这可以是 **initiator** 或 **target**。|38|否|  
|**ServerName**|**nvarchar**|正在跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|**严重性**|**int**|在此事件报告错误时表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误的严重级别。|29|否|  
|**SPID**|**int**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为客户端所关联的进程分配的服务器进程 ID。|12|是|  
|**StartTime**|**datetime**|该事件的启动时间（如果可用）。|14|是|  
|**TextData**|**ntext**|会话的当前状态。 可以是下列值之一：|1|是|  
|||**SO**。 已开始出站。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已处理了此会话的 BEGIN CONVERSATION，但尚未发送消息。|||  
|||**SI**。 已开始入站。 另一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例开始了与当前实例的新会话，但当前实例尚未完成第一条消息的接收工作。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不按照顺序接收消息，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能会在此状态下创建会话。 但是，如果为会话接收的第一个传输包含了完整的第一条消息，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能会在 CO 状态下创建会话。|||  
|||**CO**。 正在进行会话。 会话已建立，会话的双方都可以发送消息。 典型服务的大部分通信都在会话处于此状态时发生。|||  
|||**DI**。 已断开连接的入站。 会话的远程端已发出 END CONVERSATION。 会话将保持此状态，直到会话的本地端发出 END CONVERSATION。 应用程序仍然可以接收会话消息。 由于会话的远程端已经结束了会话，因此应用程序无法通过此会话发送消息。 当应用程序发出 END CONVERSATION 时，会话将转为“关闭”(CD) 状态。|||  
|||**DO**。 已断开连接的出站。 会话的本地端已发出 END CONVERSATION。 会话将保持此状态，直到会话的远程端确认 END CONVERSATION。 应用程序将无法发送或接收会话消息。 当会话的远程端确认 END CONVERSATION 之后，会话将转为“关闭”(CD) 状态。|||  
|||**ER**。 错误。 此端点发生错误。 Error、Severity 和 State 列中包含与发生的具体错误有关的信息。|||  
|||**CD**。 已关闭。 会话端点不再使用。|||  
|**Transaction ID**|**bigint**|系统为事务分配的 ID。|4|否|  
  
 下表列出了此事件类的子类值。  
  
|ID|子类|说明|  
|--------|--------------|-----------------|  
|1|SEND Message|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成 **SEND Message** SEND Message [!INCLUDE[ssDE](../../includes/ssde-md.md)] 事件。|  
|2|END CONVERSATION|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 执行不包含 WITH ERROR 子句的 END CONVERSATION 语句时 **END CONVERSATION** SEND Message [!INCLUDE[ssDE](../../includes/ssde-md.md)] 事件。|  
|3|END CONVERSATION WITH ERROR|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 执行不包含 WITH ERROR 子句的 END CONVERSATION 语句时 **END CONVERSATION WITH ERROR** SEND Message [!INCLUDE[ssDE](../../includes/ssde-md.md)] 事件。|  
|4|Broker Initiated Error|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成 **Broker Initiated Error** Broker Initiated Error [!INCLUDE[ssSB](../../includes/sssb-md.md)] 事件。 例如，当 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 无法成功为对话发送消息时，将为该对话创建错误消息并生成此事件。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不生成此事件。|  
|5|Terminate Dialog|[!INCLUDE[ssSB](../../includes/sssb-md.md)] 终止了对话。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 为了响应阻止对话继续进行的条件而终止了对话，但这不同于会话错误或会话正常结束。 例如，删除一个服务将导致 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 终止该服务的所有对话。|  
|6|Received Sequenced Message|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成 **Received Sequenced Message** Received Sequenced Message [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件类。 所有用户定义的消息类型都是已编序的消息。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会在两种情况下生成未编序的消息：<br /><br /> [!INCLUDE[ssSB](../../includes/sssb-md.md)] 生成的错误消息是未编序的。<br /><br /> 消息确认可能是未编序的。 为提高效率， [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会在已编序的消息中包含任何可用的消息确认。 不过，如果应用程序没有在特定时间段内将已编序的消息发送到远程端点， [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将为消息确认创建一个未编序的消息。|  
|7|Received END CONVERSATION|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从会话的另一端收到 End Dialog 消息时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成 Received END CONVERSATION 事件。|  
|8|Received END CONVERSATION WITH ERROR|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 当 **从会话的另一端收到用户定义的错误时，将生成** Received END CONVERSATION WITH ERROR [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 收到 Broker 定义的错误时，不会生成此事件。|  
|9|Received Broker Error Message|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 当 **从会话的另一端收到 Broker 定义的错误消息时，将生成** Received Broker Error Message [!INCLUDE[ssSB](../../includes/sssb-md.md)] 事件。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 收到 Broker 定义的错误时， [!INCLUDE[ssSB](../../includes/sssb-md.md)] 不会生成此事件。<br /><br /> 例如，如果当前数据库包含一个指向转发数据库的默认路由，则 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将把包含未知服务名称的消息发送到转发数据库。 如果该数据库不能发送此消息，则该数据库中的 Broker 将创建一个错误消息并将该错误消息返回到当前数据库。 当前数据库从转发数据库收到 Broker 生成的错误后，将生成 **Received Broker Error Message** 事件。|  
|10|Received END CONVERSATION Ack|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成 **Received END CONVERSATION Ack** 事件类。|  
|11|BEGIN DIALOG|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成 **BEGIN DIALOG** 事件。|  
|12|Dialog Created|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成 **Dialog Created** Received END CONVERSATION WITH ERROR [!INCLUDE[ssSB](../../includes/sssb-md.md)] 事件。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 都将创建一个端点，而不管当前数据库是对话的发起方还是对话的目标。|  
|13|END CONVERSATION WITH CLEANUP|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 执行包含 WITH CLEANUP 子句的 END CONVERSATION 语句时 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 生成 END CONVERSATION WITH CLEANUP 事件。|  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
