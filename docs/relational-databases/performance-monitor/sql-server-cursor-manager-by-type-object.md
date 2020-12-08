---
title: SQL Server - Cursor Manager by Type 对象 | Microsoft Docs
description: 了解 SQLServer:Cursor Manager by Type 对象，该对象提供计数器（按类型分组）来监视游标。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Cursor Manager by Type object
- SQLServer:Cursor Manager by Type
ms.assetid: d67fbd8a-7554-4a16-96f1-d9ee857a95e3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 95f88849070d95703b611a00c5c1e5abef94fffb
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505783"
---
# <a name="sql-server-cursor-manager-by-type-object"></a>SQL Server Cursor Manager by Type 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  **SQLServer:Cursor Manager by Type** 对象提供计数器（按类型分组）来监视游标。  
  
 此表介绍了 SQL Server **Cursor Manager by Type** 计数器。  
  
|Cursor Manager by Type 计数器|说明|  
|-------------------------------------|-----------------|  
|**Active cursors**|活动游标数。|  
|**Cache Hit Ratio**|高速缓存命中次数和查找次数的比率。|  
|**Cache Hit Ratio Base**|仅供内部使用。| 
|**Cached Cursor Counts**|缓存中给定类型的游标数。|  
|**Cursor Cache Use Count/sec**|每种缓存的游标的使用次数。|  
|**Cursor memory usage**|游标占用的内存量 (KB)。|  
|**Cursor Requests/sec**|服务器收到的 SQL 游标请求数。|  
|**Cursor worktable usage**|游标使用的工作表数。|  
|**Number of active cursor plans**|游标计划数。|  
  
 对象中的每个计数器均包含以下实例：  
  
|Cursor Manager 实例|说明|  
|-----------------------------|-----------------|  
|**_Total**|所有游标的信息。|  
|**API 游标**|仅 API 游标信息。|  
|**TSQL 全局游标**|仅 [!INCLUDE[tsql](../../includes/tsql-md.md)] 全局游标信息。|  
|**TSQL 局部游标**|仅 [!INCLUDE[tsql](../../includes/tsql-md.md)] 局部游标信息。|  
  
## <a name="see-also"></a>另请参阅  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
