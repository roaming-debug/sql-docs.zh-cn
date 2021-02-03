---
description: Audit Broker Login 事件类
title: Audit Broker Login 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Audit Broker Login event class
ms.assetid: af9b1153-2791-40ef-a95c-50923cd0cc97
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d4b719da934ca31cb75be296ffb4dbb4a3578f53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186236"
---
# <a name="audit-broker-login-event-class"></a>Audit Broker Login 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 创建 **Audit Broker Login** 事件以报告与 Service Broker 传输安全相关的审核消息。  
  
## <a name="audit-broker-login-event-class-data-columns"></a>Audit Broker Login 事件类的数据列  
  
|数据列|类型|说明|列号|可筛选|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|在此事件类中未使用。|10|是|  
|**ClientProcessID**|**int**|在此事件类中未使用。|9|是|  
|**DatabaseID**|**int**|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**EventClass**|**int**|捕获的事件类的类型。 对于 **Audit Broker Login** 始终为 **159**。|27|否|  
|**EventSequence**|**int**|此事件的序列号。|51|否|  
|**EventSubClass**|**int**|事件子类的类型，提供有关每个事件类的进一步信息。 下表列出了此事件的事件子类值。|21|是|  
|**FileName**|**nvarchar**|远程 broker 身份验证级别。 在远程 broker 端点上配置的支持的身份验证方法。 如果有多种可用方法，则接受（目标）端点将确定先试用哪种方法。 可能的值包括：<br /><br /> **无**。 未配置任何身份验证方法。<br /><br /> **NTLM**。 要求使用 NTLM 身份验证。<br /><br /> **KERBEROS**。 要求使用 Kerberos 身份验证。<br /><br /> **NEGOTIATE**。 由 Windows 协商身份验证方法。<br /><br /> **CERTIFICATE**。 要求使用为端点配置的证书，该证书存储在 **master** 数据库中。<br /><br /> **NTLM、CERTIFICATE**。 接受 NTLM 或 TLS/SSL 证书身份验证。<br /><br /> **KERBEROS、CERTIFICATE**。 接受 Kerberos 或端点证书身份验证。<br /><br /> **NEGOTIATE、CERTIFICATE**。 由 Windows 协商身份验证方法，或者使用端点证书进行身份验证。<br /><br /> **CERTIFICATE、NTLM**。 使用端点证书或 NTLM 进行身份验证。<br /><br /> **CERTIFICATE、KERBEROS**。 使用端点证书或 Kerberos 进行身份验证。<br /><br /> **CERTIFICATE、NEGOTIATE**。 接受端点证书进行身份验证，或由 Windows 协商身份验证方法。|36|否|  
|**HostName**|**nvarchar**|在此事件类中未使用。|8|是|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|否|  
|**LoginSid**|**图像**|已登录用户的安全标识号 (SID)。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|**NTDomainName**|**nvarchar**|用户所属的 Windows 域。|7|是|  
|**NTUserName**|**nvarchar**|拥有生成此事件的连接的用户的名称。|6|是|  
|**ObjectName**|**nvarchar**|用于此连接的连接字符串。|34|否|  
|**OwnerName**|**nvarchar**|在本地 broker 端点上配置的所支持的身份验证方法。 如果有多种可用方法，则接受（目标）端点将确定先试用哪种方法。 可能的值包括：<br /><br /> **无**。 未配置任何身份验证方法。<br /><br /> **NTLM**。 要求使用 NTLM 身份验证。<br /><br /> **KERBEROS**。 要求使用 Kerberos 身份验证。<br /><br /> **NEGOTIATE**。 由 Windows 协商身份验证方法。<br /><br /> **CERTIFICATE**。 要求使用为端点配置的证书，该证书存储在 **master** 数据库中。<br /><br /> **NTLM、CERTIFICATE**。 接受 NTLM 或 TLS/SSL 证书身份验证。<br /><br /> **KERBEROS、CERTIFICATE**。 接受 Kerberos 或端点证书身份验证。<br /><br /> **NEGOTIATE、CERTIFICATE**。 由 Windows 协商身份验证方法，或者使用端点证书进行身份验证。<br /><br /> **CERTIFICATE、NTLM**。 接受端点证书或 NTLM 身份验证。<br /><br /> **CERTIFICATE、KERBEROS**。 使用端点证书或 Kerberos 进行身份验证。<br /><br /> **CERTIFICATE、NEGOTIATE**。 接受端点证书进行身份验证，或由 Windows 协商身份验证方法。|37|否|  
|**ProviderName**|**nvarchar**|用于此连接的身份验证方法。|46|否|  
|**RoleName**|**nvarchar**|连接的角色。 这可以是 **initiator** 或 **target**。|38|否|  
|**ServerName**|**nvarchar**|正被跟踪的 SQL Server 实例的名称。|26|否|  
|**SPID**|**int**|SQL Server 为与客户端相关联的进程分配的服务器进程 ID。|12|是|  
|**StartTime**|**datetime**|事件（如果有）的开始时间。|14|是|  
|**State**|**int**|指示 SQL Server 源代码中生成该事件的位置。 可能生成此事件的每个位置都有不同的状态代码。 Microsoft 支持工程师可使用此状态代码查找生成该事件的位置。|30|否|  
|**TargetUserName**|**nvarchar**|登录状态。 即以下函数之一：<br /><br /> INITIAL<br /><br /> WAIT LOGIN NEGOTIATE<br /><br /> ONE ISC<br /><br /> ONE ASC<br /><br /> TWO ISC<br /><br /> TWO ASC<br /><br /> WAIT ISC Confirm<br /><br /> WAIT ASC Confirm<br /><br /> WAIT REJECT<br /><br /> WAIT PRE-MASTER SECRET<br /><br /> WAIT VALIDATION<br /><br /> WAIT ARBITRATION<br /><br /> ONLINE<br /><br /> ERROR<br /><br /> <br /><br /> **注意**：ISC = 启动安全上下文。 ASC = 接受安全上下文|39|否|  
|**TransactionID**|**bigint**|系统为事务分配的 ID。|4|否|  
  
 下表列出了此事件类的子类值。  
  
|ID|子类|说明|  
|--------|--------------|-----------------|  
|1|Login Success|Login Success 事件报告相邻的 broker 登录进程已经成功完成。|  
|2|Login Protocol Error|Login Protocol Error 事件报告 broker 接收到一个消息，该消息格式正确但对于登录进程的当前状态无效。 消息可能已丢失，或未按顺序发送。|  
|3|Message Format Error|Message Format Error 事件报告 broker 收到一条与所需格式不匹配的消息。 该消息可能已损坏，或者 SQL Server 之外的某个程序可能正将消息发送到 Service Broker 使用的端口。|  
|4|Negotiate Failure|Negotiate Failure 事件报告本地 broker 和远程 broker 支持身份验证的互斥级别。|  
|5|Authentication Failure|Authentication Failure 事件报告由于错误 Service Broker 无法对连接执行身份验证。 对于 Windows 身份验证，此事件报告 Service Broker 无法使用 Windows 身份验证。 对于基于证书的身份验证，此事件报告 Service Broker 无法访问证书。|  
|6|授权失败|Authorization Failure 事件报告 Service Broker 已拒绝连接的身份验证。 对于 Windows 身份验证，此事件报告连接的安全标识符与数据库用户不匹配。 对于基于证书的身份验证，此事件报告在消息中传递的公钥并不响应数据库中的证书。|  
  
## <a name="see-also"></a>另请参阅  
 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
