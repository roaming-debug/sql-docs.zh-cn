---
title: SQL Server - Batch Resp Statistics 对象 | Microsoft Docs
description: 了解 SQLServer:Batch Resp Statistics 性能对象，该对象提供计数器来跟踪 SQL Server 批响应时间。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Batch Resp Statistics
ms.assetid: a58e8733-6a8d-4b47-b5cb-042e813d808a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: acd7dc7b853c600b743d9791a3bade6ce8970a41
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505856"
---
# <a name="sql-server-batch-resp-statistics-object"></a>SQL Server, Batch Resp Statistics 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
**SQLServer:Batch Resp Statistics** 性能对象提供计数器来跟踪 SQL Server 批处理响应时间。

下表介绍了 SQL Server **Batch Resp Statistics** 性能对象。


|**SQL Server Batch Resp Statistics 计数器**|说明|  
|-------------|-----------------|  
|**Batches >=000000ms & \<000001ms**|其响应时间大于或等于 0ms 但小于 1ms 的 SQL 批处理的数目|
|**Batches >=000001ms & \<000002ms**|其响应时间大于或等于 1ms 但小于 2ms 的 SQL 批处理的数目|
|**Batches >=000002ms & \<000005ms**|其响应时间大于或等于 2ms 但小于 5ms 的 SQL 批处理的数目|
|**Batches >=000005ms & \<000010ms**|其响应时间大于或等于 5ms 但小于 10ms 的 SQL 批处理的数目|
|**Batches >=000010ms & \<000020ms**|其响应时间大于或等于 10ms 但小于 20ms 的 SQL 批处理的数目|
|**Batches >=000020ms & \<000050ms**|其响应时间大于或等于 20ms 但小于 50ms 的 SQL 批处理的数目|
|**Batches >=000050ms & \<000100ms**|其响应时间大于或等于 50ms 但小于 100ms 的 SQL 批处理的数目|
|**Batches >=000100ms & \<000200ms**|其响应时间大于或等于 100ms 但小于 200ms 的 SQL 批处理的数目|
|**Batches >=000200ms & \<000500ms**|其响应时间大于或等于 200ms 但小于 500ms 的 SQL 批处理的数目|
|**Batches >=000500ms & \<001000ms**|其响应时间大于或等于 500ms 但小于 1,000ms 的 SQL 批处理的数目|
|**Batches >=001000ms & \<002000ms**|其响应时间大于或等于 1,000ms 但小于 2,000ms 的 SQL 批处理的数目|
|**Batches >=002000ms & \<005000ms**|其响应时间大于或等于 2,000ms 但小于 5,000ms 的 SQL 批处理的数目|
|**Batches >=005000ms & \<010000ms**|其响应时间大于或等于 5,000ms 但小于 10,000ms 的 SQL 批处理的数目|
|**Batches >=010000ms & \<020000ms**|其响应时间大于或等于 10,000ms 但小于 20,000ms 的 SQL 批处理的数目|
|**Batches >=020000ms & \<050000ms**|其响应时间大于或等于 20,000ms 但小于 50,000ms 的 SQL 批处理的数目|
|**Batches >=050000ms & \<100000ms**|其响应时间大于或等于 50,000ms 但小于 100,000ms 的 SQL 批处理的数目| 
|**Batches >=100000ms**|其响应时间大于或等于 100,000ms 的 SQL 批处理的数目| 

对象中的每个计数器均包含以下实例：  
  
|项|说明|  
|----------|-----------------|  
|**CPU Time:Requests**|根据 CPU 时间得出的请求数。|  
|**CPU Time:Total(ms)**|CPU 在批处理上花费的总时间。|  
|**Elapsed Time:Requests**|根据占用时间得出的请求数。|  
|**Elapsed Time:Total(ms)**|批处理的占用时间。|  

## <a name="see-also"></a>另请参阅
[SQL Server - Plan Cache 对象](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md)  
[监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
