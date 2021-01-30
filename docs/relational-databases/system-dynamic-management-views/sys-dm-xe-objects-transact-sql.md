---
description: sys.dm_xe_objects (Transact-SQL)
title: sys.dm_xe_objects (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_xe_objects
- sys.dm_xe_objects
- sys.dm_xe_objects_TSQL
- dm_xe_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_objects dynamic management view
- extended events [SQL Server], views
ms.assetid: 5d944b99-b097-491b-8cbd-b0e42b459ec0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9e797a7010a23bfc8b13673d1efa1a90d849e6ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99129341"
---
# <a name="sysdm_xe_objects-transact-sql"></a>sys.dm_xe_objects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对事件包显示的每个对象都返回一行。 对象可以是以下项之一：  
  
-   事件。 事件指示执行路径中的相关点。 所有事件都包含与相关点有关的信息。  
  
-   操作。 激发事件时将同步执行操作。 操作可以将运行时数据追加到事件中。  
  
-   攻击. 目标在激发事件的线程中同步使用事件或在系统提供的线程中异步使用事件。  
  
-   谓词。 谓词源从事件源中检索值以在比较运算中使用。 谓词比较用于比较特定的数据类型并返回一个布尔值。  
  
-   类型。 类型封装了字节集合的长度和特征，在解释数据时需要用到这些内容。  

 |列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(60)**|对象的名称。 名称在特定对象类型的包中是唯一的。 不可为 null。|  
|object_type|**nvarchar(60)**|对象的类型。 object_type 是以下项之一：<br /><br /> event<br /><br /> 操作<br /><br /> 目标<br /><br /> pred_source<br /><br /> pred_compare<br /><br /> type<br /><br /> 不可为 null。|  
|package_guid|**uniqueidentifier**|公开此操作的包的 GUID。 与 sys.dm_xe_packages.package_id 存在多对一关系。 不可为 null。|  
|description|**nvarchar(256)**|操作的说明。 说明由包作者设置。 不可为 null。|  
|capabilities|**int**|说明对象功能的位图。 可以为 Null。|  
|capabilities_desc|**nvarchar(256)**|列出对象的所有功能。 可以为 Null。<br /><br /> **适用于所有对象类型的功能**<br /><br /> -<br />                                **私有**。 可供内部使用并且无法通过 CREATE/ALTER EVENT SESSION DDL 访问的唯一对象。 审核事件和目标属于此类别，此外，在内部使用的少量对象也属于此类别。<br /><br /> ===============<br /><br /> **事件功能**<br /><br /> -<br />                                **No_block**。 事件位于无论任何原因都无法阻塞的关键代码路径中。 具有此功能的事件无法添加到指定 NO_EVENT_LOSS 的任何事件会话中。<br /><br /> ===============<br /><br /> **适用于所有对象类型的功能**<br /><br /> -<br />                                **Process_whole_buffers**。 目标一次使用多个事件的缓冲区，而不是逐个使用事件。<br /><br /> -<br />                        **Singleton**。 在某一进程中只能存在目标的一个实例。 尽管多个事件会话可以引用相同的单个目标，但实际上只有一个实例，并且该实例将仅看到一次各唯一的事件。 如果将目标添加到全都收集同一个事件的多个会话，这一点非常重要。<br /><br /> -<br />                                **同步**。 在控制权返回到调用代码行之前，将对正在生成事件的线程执行目标。|  
|type_name|**nvarchar(60)**|pred_source 和 pred_compare 对象的名称。 可以为 Null。|  
|type_package_guid|**uniqueidentifier**|包的 GUID，此包公开此对象所操作的类型。 可以为 Null。|  
|type_size|**int**|数据类型的大小（单位为字节）。 仅限于有效的对象类型。 可以为 Null。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
### <a name="relationship-cardinalities"></a>关系基数  
  
|From|功能|关系|  
|----------|--------|------------------|  
|sys.dm_xe_objects.package_guid|sys.dm_xe_packages.guid|多对一|  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

