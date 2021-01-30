---
description: sys.server_event_session_targets (Transact-SQL)
title: sys.server_event_session_targets (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_event_session_targets_TSQL
- sys.server_event_session_targets_TSQL
- sys.server_event_session_targets
- server_event_session_targets
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_event_session_targets catalog view
- xe
ms.assetid: dda4879d-57ae-4267-b410-1ef5c37404c7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d69b9273cbcf67484568300793ece0cb8f0c05e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172637"
---
# <a name="sysserver_event_session_targets-transact-sql"></a>sys.server_event_session_targets (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  返回事件会话的每个事件目标所对应的行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|事件会话的 ID。 不可为 null。|  
|target_id|**int**|目标的 ID。 ID 在事件会话对象中是唯一的。 不可为 null。|  
|name|**sysname**|事件目标的名称。 不可为 null。|  
|包|**sysname**|包含事件目标的事件包的名称。 不可为 null。|  
|name|**sysname**|包含事件目标的模块的名称。 不可为 null。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="remarks"></a>备注  
 此视图具有下列关系基数。  
  
| From | 功能 | 关系 |
| ---- | -- | ------------ |
|sys.server_event_session_targets.event_session_id|sys.server_event_sessions sys.server_event_sessions.event_session_id|多对一|  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [&#40;Transact-sql&#41;的扩展事件目录视图 ](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [扩展事件](../../relational-databases/extended-events/extended-events.md)  
  
  
