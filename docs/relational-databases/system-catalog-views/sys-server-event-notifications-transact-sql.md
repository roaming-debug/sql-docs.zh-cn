---
description: sys.server_event_notifications (Transact-SQL)
title: sys.server_event_notifications (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_event_notifications
- sys.server_event_notifications
- sys.server_event_notifications_TSQL
- server_event_notifications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_event_notifications catalog view
ms.assetid: 1a83a044-3130-4551-95ca-162525846ff5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0fa3a705df518fa57917da989540a8b97f849a9c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172682"
---
# <a name="sysserver_event_notifications-transact-sql"></a>sys.server_event_notifications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为每个服务器级事件通知对象返回一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|服务器事件通知名称。 在所有服务器级事件通知中是唯一的。|  
|object_id|**int**|对象标识号。 在 **master** 数据库中是唯一的。|  
|**parent_class**|**tinyint**|父级的类。 始终为 100 = Server。|  
|**parent_class_desc**|**nvarchar(60)**|父类的说明。 始终为 SERVER。|  
|**parent_id**|**int**|始终为0。|  
|create_date|**datetime**|创建日期。|  
|modify_date|**datetime**|上次使用 ALTER 语句修改对象的日期。|  
|service_name|**nvarchar(256)**|向其发送通知的目标服务的名称。|  
|**broker_instance**|**nvarchar(128)**|定义命名目标服务的 service broker。|  
|**creator_sid**|**varbinary(85)**|执行事件通知创建语句的登录名的 SID。 如果在事件通知定义中未指定 WITH FAN_IN，则为 NULL。|  
|principal_id|**int**|拥有此对象的服务器主体的 ID。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
