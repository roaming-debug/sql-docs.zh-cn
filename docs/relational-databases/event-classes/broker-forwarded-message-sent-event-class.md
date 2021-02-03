---
description: Broker:Forwarded Message Sent 事件类
title: Broker:Forwarded Message Sent 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Broker:Forwarded Message Sent event class
ms.assetid: d0ef74d9-a4ef-4918-aa21-6b267e85569f
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eb714e530973b719cf89a860534af7799ad31d31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205686"
---
# <a name="brokerforwarded-message-sent-event-class"></a>Broker:Forwarded Message Sent 事件类

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 当 Service Broker 转发消息时，会生成 Broker:Forwarded Message Sent 事件。  
  
## <a name="brokerforwarded-message-sent-event-class-data-columns"></a>Broker:Forwarded Message Sent 事件类的数据列  
  
|数据列|类型|说明|列号|可筛选|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|BigintData1|**bigint**|消息序列号。|52|否|  
|ClientProcessID|**int**|由主机分配给正在运行客户端应用程序的进程的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|DatabaseID|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|DBUserName|**nvarchar**|消息来源服务的 Broker 实例 ID。|40|否|  
|EventClass|**int**|捕获的事件类的类型。 Broker:Forwarded Message Sent 始终是 139。|27|否|  
|EventSequence|**int**|此事件的序列号。|51|否|  
|FileName|**nvarchar**|作为消息发送目标的服务的名称。|36|否|  
|GUID|**uniqueidentifier**|对话的会话 ID。 此标识符将作为消息的一部分进行传输，并在会话双方之间共享。|54|否|  
|HostName|**nvarchar**|正在运行客户端程序的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|IndexID|**int**|所转发消息的剩余跃点数。|24|否|  
|IntegerData|**int**|所转发消息的片段数。|25|否|  
|IsSystem|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|否|  
|LoginSid|**图像**|已登录用户的安全标识号 (SID)。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|NTDomainName|**nvarchar**|用户所属的 Windows 域。|7|是|  
|NTUserName|**nvarchar**|拥有生成此事件的连接的用户的名称。|6|是|  
|ObjectId|**int**|转发消息后转发的消息的生存时间值。|22|否|  
|ObjectName|**nvarchar**|所转发消息的消息 ID。|34|否|  
|OwnerName|**nvarchar**|消息要定向到的 Broker 标识符。|37|否|  
|RoleName|**nvarchar**|会话句柄的角色。 有效值是：<br /><br /> Initiator。 此 Broker 发起了该会话。<br /><br /> Target。 此 Broker 是会话的目标。|38|否|  
|ServerName|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|SPID|**int**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为客户端所关联的进程分配的服务器进程 ID。|12|是|  
|StartTime|**datetime**|事件（如果有）的开始时间。|14|是|  
|成功|**int**|转发过程所用的时间。|23|否|  
|TargetLoginName|**nvarchar**|此实例将消息发送到的网络地址。 注意，这可能与消息的最终目的地不同。|42|否|  
|TargetUserName|**nvarchar**|启动消息的服务的名称。|39|否|  
|TransactionID|**bigint**|系统为事务分配的 ID。|4|否|  
  
  
