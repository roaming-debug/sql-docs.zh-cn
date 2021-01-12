---
description: sys.transmission_queue (Transact-SQL)
title: sys.transmission_queue (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- transmission_queue
- sys.transmission_queue_TSQL
- sys.transmission_queue
- transmission_queue_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.transmission_queue catalog view
ms.assetid: f3515d1a-be8f-4a27-8058-8865f0919838
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8d3b3b9662feefbc14c4ee12fbdd17f4e10ba25c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094332"
---
# <a name="systransmission_queue-transact-sql"></a>sys.transmission_queue (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  此目录视图中的每一行对应于传输队列中的一条消息，如下表所示：  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**conversation_handle**|**uniqueidentifier**|此消息所属会话的标识符。 不可为 NULL。|  
|**to_service_name**|**nvarchar(256)**|此消息要发送到的服务的名称。 可以为 null.|  
|**to_broker_instance**|**nvarchar(128)**|承载此消息要发送到的服务的 Broker 名称。 可以为 null.|  
|**from_service_name**|**nvarchar(256)**|发送此消息的服务的名称。 可以为 null.|  
|**service_contract_name**|**nvarchar(256)**|此消息的会话所遵循的约定名称。 可以为 null.|  
|**enqueue_time**|**datetime**|消息进入队列的时间。 无论实例的本地时区如何，该值都使用 UTC。 不可为 NULL。|  
|**message_sequence_number**|**bigint**|消息的序列号。 不可为 NULL。|  
|**message_type_name**|**nvarchar(256)**|消息的消息类型名称。 可以为 null.|  
|**is_conversation_error**|**bit**|此消息是否是错误消息。<br /><br /> 0 = 不是错误消息。<br /><br /> 1 = 是错误消息。<br /><br /> 不可为 NULL。|  
|**is_end_of_dialog**|**bit**|此消息是否是会话消息的结尾。 不可为 NULL。<br /><br /> 0 = 不是会话消息的结尾。<br /><br /> 1 = 是会话消息的结尾。<br /><br /> 不可为 NULL。|  
|**message_body**|**varbinary(max)**|此消息的正文。 可以为 null.|  
|**transmission_status**|**nvarchar(4000)**|此消息位于队列中的原因。 这通常是一条错误消息，说明未能发送该消息的原因。 如果为空白，则尚未发送此消息。 可以为 null.|  
|**priority**|**tinyint**|为此消息指定的优先级。 不可为 NULL。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
  
