---
description: sys.dm_broker_forwarded_messages (Transact-SQL)
title: sys.dm_broker_forwarded_messages (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_broker_forwarded_messages
- dm_broker_forwarded_messages
- sys.dm_broker_forwarded_messages_TSQL
- dm_broker_forwarded_messages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_forwarded_messages dynamic management view
ms.assetid: 5576376d-6364-417a-8475-aa770e060845
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 089fb1aaf7cca8f3d8d3d57c49e11cf803f3329e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095288"
---
# <a name="sysdm_broker_forwarded_messages-transact-sql"></a>sys.dm_broker_forwarded_messages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  针对每个 Service Broker 消息都返回一行，此消息表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例正在转发中。  
  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**conversation_id**|**uniqueidentifier**|此消息所属会话的 ID。 可以为 null.|  
|**is_initiator**|**bit**|指示此消息是否来自会话的发起方。  可以为 null.<br /><br /> 0 = 不来自发起方<br /><br /> 1 = 来自发起方|  
|**to_service_name**|**nvarchar(512)**|此消息所发往的服务的名称。 可以为 null.|  
|**to_broker_instance**|**nvarchar(512)**|Broker 的标识符，该 Broker 承载此消息所发往的服务。 可以为 null.|  
|**from_service_name**|**nvarchar(512)**|发送此消息的服务的名称。 可以为 null.|  
|**from_broker_instance**|**nvarchar(512)**|承载发送此消息的服务的 Broker 的标识符。 可以为 null.|  
|**adjacent_broker_address**|**nvarchar(512)**|此消息发往的网络地址。 可以为 null.|  
|**message_sequence_number**|**bigint**|对话框中的消息的序列号。 可以为 null.|  
|**message_fragment_number**|**int**|如果对话消息分为多个片段，则它表示此传输消息包含的片段数。 可以为 null.|  
|**hops_remaining**|**tinyint**|消息在到达最终目标地址之前可能被重新传送的次数。 每次转发消息时，此数字都会减少 1。 可以为 null.|  
|**time_to_live**|**int**|消息保持活动状态的最长时间。 当该值为 0 时，表示放弃了消息。 可以为 null.|  
|**time_consumed**|**int**|消息已保持活动状态的时间。 每次转发消息时，该数字都会按照它转发消息所用的时间增加。 不可为 NULL。|  
|**message_id**|**uniqueidentifier**|消息的 ID。 可以为 null.|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与 Service Broker 有关的动态管理视图 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  

