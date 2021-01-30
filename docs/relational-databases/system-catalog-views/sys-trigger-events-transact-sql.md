---
description: sys.trigger_events (Transact-SQL)
title: sys.trigger_events (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- trigger_events_TSQL
- trigger_events
- sys.trigger_events
- sys.trigger_events_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trigger_events catalog view
ms.assetid: 92540447-131c-491c-b033-c064c7d950e1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2c6e0067e7e92e0f375c4fc0d735a3bb3f466021
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188816"
---
# <a name="systrigger_events-transact-sql"></a>sys.trigger_events (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  激发触发器的每个事件对应一行。  
  
> [!NOTE]  
>  **sys.trigger_events** 不适用于事件通知。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.events>**|不适用|从 [sys.databases](../../relational-databases/system-catalog-views/sys-events-transact-sql.md)继承 **object_id**、**类型** **type_desc** 列。|  
|**is_first**|**bit**|触发器标记为第一个为此事件激发的触发器。|  
|**is_last**|**bit**|触发器标记为最后一个为此事件激发的触发器。|  
|**event_group_type**|**int**|要对其创建触发器的事件组，如果未对事件组创建触发器，则为 Null。|  
|**event_group_type_desc**|**nvarchar(60)**|要对其创建触发器的事件组的说明，如果未对事件组创建触发器，则为 Null。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  
  
  
