---
description: Audit Broker Conversation 事件类
title: Audit Broker Conversation 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Audit Broker Conversation event class
ms.assetid: d58e3577-e297-42e5-b8fe-206665a75d13
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 41813ebefcdf014045b343719249009a2908584f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179801"
---
# <a name="audit-broker-conversation-event-class"></a>Audit Broker Conversation 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 创建了 **Audit Broker Conversation** 事件以报告与 Service Broker 对话安全有关的审核消息。  
  
## <a name="audit-broker-conversation-event-class-data-columns"></a>Audit Broker Conversation 事件类的数据列  
  
|数据列|类型|说明|列号|可筛选|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|**BigintData1**|**bigint**|消息序列号。|52|否|  
|**ClientProcessID**|**int**|由主机分配给正在运行客户端应用程序的进程的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|**DatabaseID**|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**错误**|**int**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误号（如果此事件报告了错误）。|31|否|  
|**EventClass**|**int**|捕获的事件类的类型。 对于 **Audit Broker Conversation** ，始终为 **158**。|27|否|  
|**EventSubClass**|**int**|事件子类的类型，提供有关每个事件类的进一步信息。 下表列出了此事件的事件子类值。|21|是|  
|**FileName**|**nvarchar**|登录失败的原因。 如果登录成功，则此列为空。|36|否|  
|**GUID**|**uniqueidentifier**|对话的会话 ID。 此标识符将作为消息的一部分进行传输，并在会话双方之间共享。|54|否|  
|**HostName**|**nvarchar**|正在运行客户端程序的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 **HOST_NAME** 函数。|8|是|  
|**IntegerData**|**int**|消息片段数。|25|否|  
|**NTDomainName**|**nvarchar**|用户所属的 Windows 域。|7|是|  
|**NTUserName**|**nvarchar**|拥有生成此事件的连接的用户的名称。|6|是|  
|**ObjectId**|**int**|目标服务的用户 ID。|22|否|  
|**RoleName**|**nvarchar**|会话句柄的角色。 这可以是 **initiator** 或 **target**。|38|否|  
|**ServerName**|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|**严重性**|**int**|在此事件报告错误时表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误的严重级别。|29|否|  
|**SPID**|**int**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为客户端所关联的进程分配的服务器进程 ID。|12|是|  
|**StartTime**|**datetime**|事件（如果有）的开始时间。|14|是|  
|**State**|**int**|指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 源代码中生成该事件的位置。 可能生成此事件的每个位置都有不同的状态代码。 Microsoft 支持工程师可使用此状态代码查找生成该事件的位置。|30|否|  
|**TextData**|**ntext**|如果发生错误，将显示一条用来说明失败原因的消息。 以下值之一：<br /><br /> <br /><br /> **Cert not found**。 针对对话协议安全指定的用户没有证书。<br /><br /> **Not in valid time period**。 针对对话协议安全指定的用户有一份证书，但该证书已过期。<br /><br /> **Cert too large for memory allocation**。 针对对话协议安全指定的用户有一份证书，但该证书过大。 Service Broker 支持的最大证书大小为 32,768 字节。<br /><br /> **Private key not found**。 针对对话协议安全指定的用户有一份证书，但没有与该证书相关联的私钥。<br /><br /> **The cert's private key size is incompatible with the crypto provider**。 无法成功处理证书的私钥大小。 私钥大小必须是 64 字节的倍数。<br /><br /> **The cert's public key size is incompatible with the crypto provider**。 无法成功处理证书的公钥大小。 公钥大小必须是 64 字节的倍数。<br /><br /> **The cert's private key size is incompatible with the encrypted key exchange key**。 密钥交换密钥中指定的密钥大小与证书的私钥大小不匹配。 这通常表示远程计算机上的证书与数据库中的证书不匹配。<br /><br /> **The cert's public key size is incompatible with the security header's signature**。 无法通过证书的公钥来验证安全标头的签名。 这通常表示远程计算机上的证书与数据库中的证书不匹配。|1|是|  
  
 下表列出了此事件类的子类值。  
  
|ID|子类|说明|  
|--------|--------------|-----------------|  
|1|No Security Header|安全会话期间，Service Broker 接收到不包含会话密钥的消息。 建立了安全会话后，对话协议要求会话中的所有消息都包含会话密钥。|  
|2|No Certificate|Service Broker 无法为会话中的某一名参与者找到可用的证书。 为了保证会话安全，数据库必须同时包含会话发送方和接收方的证书。|  
|3|Invalid Signature|Broker 无法使用发送方证书中的公钥来验证发送方所提供的消息签名。 这可能表示消息已损坏、消息被篡改、远程服务和本地服务没有配置相同的用户证书的或者证书已过期。|  
|4|Run As Target Failure|目标用户不具有对目标队列的接收权限。 为了防止未经授权的用户接收消息，如果目标用户无法接收消息队列，则无论初始用户是否有权将消息排队，Service Broker 都不会将消息排队。|  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
