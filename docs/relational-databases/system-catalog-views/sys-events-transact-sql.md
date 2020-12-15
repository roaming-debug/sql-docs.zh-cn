---
description: sys.events (Transact-SQL)
title: sys. events (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.events_TSQL
- sys.events
- events_TSQL
- events
dev_langs:
- TSQL
helpviewer_keywords:
- sys.events catalog view
ms.assetid: f245a97a-80fc-43fb-a6e4-139420c9a47a
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3383217a22167b5255f748792952949396138fe4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97413048"
---
# <a name="sysevents-transact-sql"></a>sys.events (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  导致触发器或事件通知激发的每个事件对应一行。 这些事件表示使用 [CREATE trigger](../../t-sql/statements/create-trigger-transact-sql.md) 或 [create event notification](../../t-sql/statements/create-event-notification-transact-sql.md)创建触发器或事件通知时指定的事件类型。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|触发器或事件通知的 ID。 此值与 **类型** 一起唯一标识行。|  
|**type**|**int**|导致触发器激发的事件。|  
|**type_desc**|**nvarchar(60)**|导致触发器激发的事件的说明。|  
|**is_trigger_event**|**bit**|1 = 触发器事件。<br /><br /> 0 = 通知事件。|  
|**event_group_type**|**int**|要对其创建触发器或事件通知的事件组，如果未对事件组中创建触发器或事件通知，则为 Null。|  
|**event_group_type_desc**|**nvarchar(60)**|创建触发器或事件通知的事件组的说明，如果未在事件组中创建触发器或事件通知，则为 Null。|  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
