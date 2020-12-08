---
title: SQL Server - Buffer Manager 对象 | Microsoft Docs
description: 了解 Buffer Manager 对象，该对象提供用于监视页面内存的计数器、用于监视物理 I/O 的计数器以及缓冲池扩展。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Buffer Manager object
- SQLServer:Buffer Manager
ms.assetid: 9775ebde-111d-476c-9188-b77805f90e98
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2eae64110e8c359e424d6f7313f9949f28b46c9f
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505822"
---
# <a name="sql-server-buffer-manager-object"></a>SQL Server Buffer Manager 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  **Buffer Manager** 对象提供了计数器，用于监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如何使用以下项：  
  
-   存储数据页的内存。  
  
-   用于监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 读取和写入数据库页时的物理 I/O 的计数器。  
  
-   用于借助高速非易失性存储（如固态硬盘 (SSD)）扩展缓冲区高速缓存的缓冲池扩展。  
  
 监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用的内存和计数器有助于确定：  
  
-   是否存在物理内存不足的瓶颈。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法将经常访问的数据存储在缓存中，则必须从磁盘检索数据。   
  
-   是否可以通过添加更多内存或增加数据缓存或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部结构的可用内存来提高查询性能。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 需要从磁盘读取数据的频率。 与其他操作（例如内存访问）相比，物理 I/O 会消耗大量时间。 尽可能减少物理 I/O 可以提高查询性能。  
  
## <a name="buffer-manager-performance-objects"></a>缓冲区管理器性能对象  
 下表介绍 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Buffer Manager 性能对象。  
  
|SQL Server Buffer Manager 计数器|说明|  
|----------------------------------------|-----------------|  
|**Background writer pages/sec**|为强制恢复间隔设置而刷新的页数。| 
|**缓冲区缓存命中率**|指示在缓冲区高速缓存中找到而不需要从磁盘中读取的页的百分比。 该比率是缓存命中总次数与过去几千页访问以来的缓存查找总次数之比。 经过很长时间后，该比率的变化很小。 由于从缓存中读取数据比从磁盘中读取数据的开销小得多，一般希望该比率高一些。 通常，可以通过增加 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的可用内存量或使用缓冲池扩展功能来提高缓冲区缓存命中率。|  
|**Buffer cache hit ratio base**|仅供内部使用。|
|**Checkpoint pages/sec**|指示由要求刷新所有脏页的检查点或其他操作每秒刷新到磁盘的页数。|  
|**Database pages**|指示缓冲池中包含数据库内容的页数。|  
|**Extension allocated pages**|缓冲池扩展文件中非空闲高速缓存页的总数。|  
|**Extension free pages**|缓冲池扩展文件中空闲高速缓存页的总数。|  
|**Extension in use as percentage**|缓冲区管理器页占用的缓冲池扩展分页文件的百分比。|  
|**Extension outstanding IO counter**|缓冲池扩展文件的 I/O 队列长度。|  
|**Extension page evictions/sec**|每秒从缓冲池扩展文件中逐出的页数。|  
|**Extension page reads/sec**|每秒从缓冲池扩展文件中读取的页数。|  
|**Extension page unreferenced time**|未被引用时，页在缓冲池扩展中保留的平均时长（以秒计）。|  
|**Extension pages writes/sec**|每秒向缓冲池扩展文件中写入的页数。|  
|**Free list stalls/sec**|指示每秒必须等待空闲页面的请求数量。|  
|**Integral Controller Slope**|缓冲池的集成控制器上次使用的斜率，乘以 -100 亿。| 
|**Lazy writes/sec**|指示缓冲区管理器惰性编写器每秒写入的缓冲区数。 “惰性编写器”是一个系统进程，用于成批刷新过期的脏缓冲区（包含更改的缓冲区，必须将这些更改写回磁盘，才能将缓冲区重用于其他页），并使它们可用于用户进程。 惰性编写器不需要为创建可用缓冲区而频繁执行检查点。|  
|**Page life expectancy**|指示页面在没有引用的情况下，在此节点的缓冲池中停留的时间（以秒计）。|  
|**Page lookups/sec**|指示每秒要求在缓冲池中查找页的请求数。|  
|**Page reads/sec**|指示每秒发生的物理数据库页读取数。 此统计信息显示的是所有数据库间的物理页读取总数。 由于物理 I/O 的开销大，可以通过使用更大的数据缓存、智能索引、更有效的查询或更改数据库设计等方法，将开销降到最低。|  
|**Page writes/sec**|指示每秒发出的物理数据库页写入数。|  
|**Readahead pages/sec**|指示每秒为预期使用读取的页数。|  
|**Readahead time/sec**|发出预读所用的时间（微秒）。|
|**Target Pages**|缓冲池中理想的页数。|

  
## <a name="see-also"></a>另请参阅  
 [SQL Server:Buffer Node](../../relational-databases/performance-monitor/sql-server-buffer-node.md)   
 [“服务器内存”服务器配置选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md)   
 [SQL Server Plan Cache 对象](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md)   
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [sys.dm_os_performance_counters (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)   
 [缓冲池扩展](../../database-engine/configure-windows/buffer-pool-extension.md)  
  
  
