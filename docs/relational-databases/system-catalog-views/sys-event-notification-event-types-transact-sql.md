---
description: sys.event_notification_event_types (Transact-SQL)
title: sys.event_notification_event_types (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.event_notification_event_types_TSQL
- sys.event_notification_event_types
- event_notification_event_types_TSQL
- event_notification_event_types
dev_langs:
- TSQL
helpviewer_keywords:
- sys.event_notification_event_types catalog view
ms.assetid: 73dae456-7044-4b00-b0bd-990ef810b356
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 23f843b7996f527ac55a2bc05416c7a3ca8d9ca0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203874"
---
# <a name="sysevent_notification_event_types-transact-sql"></a>sys.event_notification_event_types (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  为可以激发事件通知的每个事件或事件组返回一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|type |**int**|导致激发事件通知的事件或事件组的类型。|  
|type_name|**nvarchar(128)**|事件或事件组的名称。 这可以在 [CREATE EVENT NOTIFICATION](../../t-sql/statements/create-event-notification-transact-sql.md) 语句的 FOR 子句中指定。|  
|**parent_type**|**int**|作为事件或事件组父级的事件组的类型。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
