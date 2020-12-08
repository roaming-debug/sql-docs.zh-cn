---
title: SQL Server XTP 数据库 | Microsoft Docs
description: 了解 SQL Server XTP 数据库性能对象，该对象提供内存中 OLTP 特定于数据库的计数器。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQL Server 2016 XTP Databases
ms.assetid: 488ff55e-173f-43f6-9bdb-67b35e7cebfe
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9b4e75e2033e0e73c592eb0e565038d514f73229
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505495"
---
# <a name="sql-server-xtp-databases"></a>SQL Server XTP 数据库
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

**SQL Server XTP 数据库** 性能对象提供内存中 OLTP 特定于数据库的计数器。

> [!NOTE]
>  SQL Server XTP 数据库计数器当前从 sys.dm_os_performance_counters 中不可见。  可以通过 [系统监视器](../../relational-databases/performance/start-system-monitor-windows.md)查看这些计数器。

下表说明了 **SQL Server XTP 数据库** 计数器。

|计数器|说明| 
|-------------|-----------------|  
|**事务片段大型数据的平均大小**|事务片段大型数据负载的平均大小。 此为非常低级的计数器，不适合客户使用。|
|**事务片段的平均大小**|事务片段负载的平均大小。 如果此值为零，则将从后端分配器分配更多页。 此为非常低级的计数器，不适合客户使用。|
|**刷新线程 256K 队列深度**|256K IO 请求的刷新线程队列深度。|
|**刷新线程 4K 队列深度**|4K IO 请求的刷新线程队列深度。|
|**刷新线程 64K 队列深度**|64K IO 请求的刷新线程队列深度。|
|**刷新线程冻结 IO 数/秒 (256K)**|处于冻结阈值之上（因此无法发出）的刷新页面处理期间遇到的 256K IO 请求数。|
|**刷新线程冻结 IO 数/秒 (4K)**|处于冻结阈值之上（因此无法发出）的刷新页面处理期间遇到的 4K IO 请求数。|
|**刷新线程冻结 IO 数/秒 (64K)**|处于冻结阈值之上（因此无法发出）的刷新页面处理期间遇到的 64K IO 请求数。|
|**IoPagePool256K 可用列表计数**|256K IO 页池可用列表中的页数。 如果此值为零，则将从后端分配器分配更多页。 此为非常低级的计数器，不适合客户使用。|
|**已分配的 IoPagePool256K 总数**|由 256K IO 页池从后端分配器中分配和保留的总页数。 此为非常低级的计数器，不适合客户使用。|
|**IoPagePool4K 可用列表计数**|4K IO 页池可用列表中的页数。 如果此值为零，则将从后端分配器分配更多页。 此为非常低级的计数器，不适合客户使用。|
|**已分配的 IoPagePool4K 总数**|由 4K IO 页池从后端分配器中分配和保留的总页数。 此为非常低级的计数器，不适合客户使用。|
|**IoPagePool64K 可用列表计数**|64K IO 页池可用列表中的页数。 如果此值为零，则将从后端分配器分配更多页。 此为非常低级的计数器，不适合客户使用。|
|**已分配的 IoPagePool64K 总数**|由 64K IO 页池从后端分配器中分配和持有的总页数。 此为非常低级的计数器，不适合客户使用。|
|**MtLog 256K 展开计数**|256K MtLog 展开的次数。 此为非常低级的计数器，不适合客户使用。|
|**未完成的 MtLog 256K IO 数**|由 MtLog 发出的未完成的 256K IO 请求数。|
|**MtLog 256K 页面填充百分比/刷新页面**|每个刷新的 256K MtLog 页面的平均填充百分比。 此为非常低级的计数器，不适合客户使用。|
|**MtLog 256K 写入字节数/秒**|每秒在 256K MtLog 对象上写入的字节数。 此为非常低级的计数器，不适合客户使用。|
|**MtLog 4K 展开计数**|4K MtLog 展开的次数。 此为非常低级的计数器，不适合客户使用。|
|**未完成的 MtLog 4K IO 数**|由 MtLog 发出的未完成的 4K IO 请求数。|
|**MtLog 4K 页面填充百分比/刷新页面**|每个刷新的 4K MtLog 页面的平均填充百分比。 此为非常低级的计数器，不适合客户使用。|
|**MtLog 4K 写入字节数/秒**|每秒在 4K MtLog 对象上写入的字节数。 此为非常低级的计数器，不适合客户使用。|
|**MtLog 64K 展开计数**|64K MtLog 展开的次数。 此为非常低级的计数器，不适合客户使用。|
|**未完成的 MtLog 64K IO 数**|由 MtLog 发出的未完成的 64K IO 请求数。|
|**MtLog 64K 页面填充百分比/刷新页面**|每个刷新的 64K MtLog 页面的平均填充百分比。 此为非常低级的计数器，不适合客户使用。|
|**MtLog 64K 写入字节数/秒**|每秒在 64K MtLog 对象上写入的字节数。 此为非常低级的计数器，不适合客户使用。|
|**合并数量**|正在进行的合并的数量。|
|**合并数量/秒**|每秒创建的合并数（平均值）。|
|**数字序列化**|正在进行的序列化的数量。|
|**数字序列化数/秒**|每秒创建的序列化数（平均值）。|
|**结尾缓存页面计数**|在结尾缓存中分配的页数。 此为非常低级的计数器，不适合客户使用。|
|**结尾缓存页面计数峰值**|在结尾缓存中分配的最大页数。 此为非常低级的计数器，不适合客户使用。|


## <a name="see-also"></a>另请参阅  
[SQL Server XTP（内存中 OLTP）性能计数器](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)
