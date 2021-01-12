---
description: Extended Events Tables - trace_xe_action_map
title: trace_xe_action_map (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- trace_xe_action_map_TSQL
- trace_xe_action_map
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], tables
- trace_xe_action_map
ms.assetid: 208a1413-ce7f-4521-b765-d74723627302
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5eeb22a3617ffb2510c4d9d768cf2cde5e281ee0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100590"
---
# <a name="extended-events-tables---trace_xe_action_map"></a>Extended Events Tables - trace_xe_action_map
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  映射到 SQL 跟踪列 ID 的每个扩展事件操作各占一行。 此表存储在 sys 架构中的 master 数据库中。  
  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|trace_column_id|**smallint**|正在映射的 SQL 跟踪列的 ID。|  
|package_name|**nvarchar(60)**|映射操作所在的扩展事件包的名称。|  
|xe_action_name|**nvarchar(60)**|映射到 SQL 跟踪列的扩展事件操作的名称。|  
  
## <a name="remarks"></a>备注  
 您可以使用以下查询确定与 SQL 跟踪列等效的扩展事件操作：  
  
```  
SELECT tc.name AS trace_column, am.package_name, am.xe_action_name  
FROM sys.trace_columns AS tc  
INNER JOIN sys.trace_xe_action_map AS am  
   ON tc.trace_column_id = am.trace_column_id  
```  
  
 不映射到表中不包括的操作的 SQL 跟踪列。  
  
## <a name="see-also"></a>另请参阅  
 [trace_xe_event_map (Transact-SQL)](../../relational-databases/system-tables/extended-events-tables-trace-xe-event-map.md)  
  
  
