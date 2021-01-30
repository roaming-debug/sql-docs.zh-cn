---
description: syscollector_collection_items (Transact-SQL)
title: syscollector_collection_items (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syscollector_collection_items_TSQL
- syscollector_collection_items
dev_langs:
- TSQL
helpviewer_keywords:
- syscollector_collection_items view
- add data collector view
ms.assetid: a279ecd1-a59c-4315-9f08-bf221f00a465
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 52f7bb7471b0b8c8d38fc2f4fa1790fd5c12647a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199788"
---
# <a name="syscollector_collection_items-transact-sql"></a>syscollector_collection_items (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关收集组中的项的信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**collection_set_id**|**int**|标识收集组。 不可为 null。|  
|**collection_item_id**|**int**|标识收集组中的项。 不可为 null。|  
|**collector_type_uid**|**uniqueidentifier**|用于标识收集器类型的 GUID。 不可为 null。|  
|name |**nvarchar(4000)**|收集组的名称。 可以为 Null。|  
|**frequency**|**int**|收集项收集数据的频率。 不可为 null。|  
|**parameters**|**xml**|介绍与相应收集项关联的收集器类型的参数化。 将使用存储在特定收集器类型的 **parameter_schema** 中的 xml 架构 (XSD) 来验证此收集项的 xml 架构。 可以为 Null。 有关详细信息，请参阅 [&#40;transact-sql&#41;syscollector_collector_types ](../../relational-databases/system-catalog-views/syscollector-collector-types-transact-sql.md)。|  
  
## <a name="permissions"></a>权限  
 需要 **dc_operator** **dc_proxy** 选择。  
  
## <a name="see-also"></a>另请参阅  
 [数据收集器存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [数据收集器视图 (Transact-SQL)](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [“数据收集”](../../relational-databases/data-collection/data-collection.md)  
  
  
