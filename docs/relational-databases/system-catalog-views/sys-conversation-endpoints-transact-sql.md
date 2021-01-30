---
description: sys.conversation_endpoints (Transact-SQL)
title: sys.conversation_endpoints (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- conversation_endpoints_TSQL
- conversation_endpoints
- sys.conversation_endpoints
- sys.conversation_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.conversation_endpoints catalog view
ms.assetid: 2ed758bc-2a9d-4831-8da2-4b80e218f3ea
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2a06a028bc7ab6bfebb64e6b54f32c7d8d0a991b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200867"
---
# <a name="sysconversation_endpoints-transact-sql"></a>sys.conversation_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会话的每一端由会话端点表示。 对于数据库中的每个会话端点，此目录视图相应地包含一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|conversation_handle|**uniqueidentifier**|此会话端点的标识符。 不可为 NULL。|  
|conversation_id|**uniqueidentifier**|会话的标识符。 此标识符由会话的两个参与方共享。 它与 is_initiator 列在数据库中是唯一的。 不可为 NULL。|  
|is_initiator|**tinyint**|此端点是会话的发起方还是它的目标。  不可为 NULL。<br /><br /> 1 = 发起方<br /><br /> 0 = 目标|  
|service_contract_id|**int**|此会话的约定的标识符。 不可为 NULL。|  
|conversation_group_id|**uniqueidentifier**|此会话所属的会话组的标识符。 不可为 NULL。|  
|service_id|**int**|会话的这一端的服务的标识符。 不可为 NULL。|  
|lifetime|**datetime**|此会话的过期日期/时间。 不可为 NULL。|  
|state|**char(2)**|会话的当前状态。 不可为 NULL。 下列其中一项：<br /><br /> 这样就开始出站。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已处理此会话的 BEGIN 会话，但尚未发送消息。<br /><br /> SI 已开始入站。 另一个实例已开始与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的新会话，但 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 尚未完全收到第一条消息。 如果第一条消息含有碎片或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 收到消息的顺序不对，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以在此状态下创建会话。 但是，如果会话接收的第一次传输包含完整的第一条消息，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以在 CO（正在转换）状态下创建会话。<br /><br /> 合作。 会话已建立，会话的双方都可以发送消息。 典型服务的大部分通信都在会话处于此状态时发生。<br /><br /> DI   已断开入站。 会话的远程端已发出 END CONVERSATION。 会话将保持此状态，直到会话的本地端发出 END CONVERSATION。 应用程序可能仍会接收会话消息。 由于会话的远程端已经结束了会话，因此应用程序无法通过此会话发送消息。 当应用程序发出 END CONVERSATION 时，会话将转为 CD（关闭）状态。<br /><br /> DO   已断开出站。 会话的本地端已发出 END CONVERSATION。 会话将保持此状态，直到会话的远程端确认 END CONVERSATION。 应用程序将无法发送或接收会话消息。 当会话的远程端确认 END CONVERSATION 之后，会话将转为 CD（关闭）状态。<br /><br /> ER 错误。 此端点发生错误。 此错误消息放入应用程序队列中。 如果应用程序队列为空，则表示应用程序已使用此错误消息。<br /><br /> CD   关闭。 会话端点不再使用。|  
|state_desc|**nvarchar(60)**|终结点对话状态的说明。 此列可以为 NULL。 下列其中一项：<br /><br /> **STARTED_OUTBOUND**<br /><br /> **STARTED_INBOUND**<br /><br /> **工作组**<br /><br /> **DISCONNECTED_INBOUND**<br /><br /> **DISCONNECTED_OUTBOUND**<br /><br /> **闭**<br /><br /> **ERROR**|  
|far_service|**nvarchar(256)**|会话的远程端上的服务的名称。 不可为 NULL。|  
|far_broker_instance|**nvarchar(128)**|会话的远程端的 Broker 实例。 可以为 null.|  
|principal_id|**int**|对话的本地端所使用的证书所属的主体的标识符。 不可为 NULL。|  
|far_principal_id|**int**|对话的远程端所使用的证书所属的用户的标识符。 不可为 NULL。|  
|outbound_session_key_identifier|**uniqueidentifier**|此对话的出站加密密钥的标识符。 不可为 NULL。|  
|inbound_session_key_identifier|**uniqueidentifier**|此对话的入站加密密钥的标识符。 不可为 NULL。|  
|security_timestamp|**datetime**|创建本地会话密钥的时间。 不可为 NULL。|  
|dialog_timer|**datetime**|此对话的会话计时器发送 DialogTimer 消息的时间。 不可为 NULL。|  
|send_sequence|**bigint**|发送序列中的下一个消息号。 不可为 NULL。|  
|last_send_tran_id|**binary(6)**|要发送消息的上一个事务的内部事务 ID。 不可为 NULL。|  
|end_dialog_sequence|**bigint**|End Dialog 消息的序号。 不可为 NULL。|  
|receive_sequence|**bigint**|在消息接收序列中预期的下一个消息号。 不可为 NULL。|  
|receive_sequence_frag|**int**|在消息接收序列中预期的下一个消息碎片号。 不可为 NULL。|  
|system_sequence|**bigint**|此对话的最后一个系统消息的序号。 不可为 NULL。|  
|first_out_of_order_sequence|**bigint**|此对话的无序消息中的第一个消息的序号。 不可为 NULL。|  
|last_out_of_order_sequence|**bigint**|此对话的无序消息中的最后一个消息的序号。 不可为 NULL。|  
|last_out_of_order_frag|**int**|此对话的无序碎片中的最后一个消息的序号。 不可为 NULL。|  
|is_system|**bit**|如果这是系统对话，则为 1。 不可为 NULL。|  
|priority|**tinyint**|分配给此会话端点的会话优先级。 不可为 NULL。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
  
