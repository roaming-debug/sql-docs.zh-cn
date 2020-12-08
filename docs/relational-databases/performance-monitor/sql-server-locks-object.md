---
title: SQL Server - Locks 对象 | Microsoft Docs
description: 了解 SQLServer:Locks 对象，该对象提供了有关各种资源类型的 SQL Server 锁的信息。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Locks object
- SQLServer:Locks
ms.assetid: ace04f0d-3993-4444-8317-ca39d7087e49
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 94e726a108c78ffec8715e80c4b14f2274656be0
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505661"
---
# <a name="sql-server-locks-object"></a>SQL Server Locks 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Microsoft **中的** SQLServer:Locks [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象提供了有关各种资源类型的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 锁的信息。 锁加在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源上（如在一个事务中读取或修改的行），以防止各种事务并发使用资源。 例如，如果一个排它 (X) 锁被一个事务加在某一表的某一行上，在这个锁被释放前，其他事务都不可以修改这一行。 尽可能少使用锁可提高并发性，从而改善性能。 可以同时监视 **Locks** 对象的多个实例，每个实例代表一个资源类型上的一个锁。  
  
 下表介绍了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Locks 计数器。  
  
|SQL Server Locks 计数器|说明|  
|-------------------------------|-----------------|  
|**Average Wait Time (ms)**|每个导致等待的锁请求的平均等待时间（毫秒）。|  
|**Average Wait Time Base**|仅供内部使用。|
|**Lock Requests/sec**|锁管理器每秒请求的新锁和锁转换数。|  
|**Lock Timeouts (timeout > 0)/sec**|每秒超时的锁请求数，但不包括对 NOWAIT 锁的请求。|  
|**Lock Timeouts/sec**|每秒超时的锁请求数，包括对 NOWAIT 锁的请求。|  
|**Lock Wait Time (ms)**|锁在最后一秒内的总等待时间（毫秒）。|  
|**Lock Waits/sec**|每秒要求调用者等待的锁请求数。|  
|**Number of Deadlocks/sec**|每秒导致死锁的锁请求数。|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以锁定下列这些资源。  
  
|项|说明|  
|----------|-----------------|  
|**_Total**|所有锁的信息。|  
|**分配单元**|分配单元的锁。|  
|**应用程序**|锁定指定了应用程序的资源。|  
|**Database**|锁定数据库（包括数据库中的所有对象）。|  
|**扩展盘区**|锁定由连续的 8 个页构成的一组。|  
|**File**|锁定数据库文件。|  
|**堆/B 树**|堆或 B 树 (HOBT)。 锁定数据页堆，或索引的 B 树结构。|  
|**Key**|锁定索引中的某行。|  
|**元数据**|锁定一些目录信息（又称为元数据）。|  
|**Object**|锁定表、存储过程、视图等（包括所有数据和索引）。 该对象可以是包含 **sys.all_objects** 中某项的任何一个对象。|  
|**第**|锁定数据库中 8 KB 页。|  
|**RID**|行 ID。 锁定一个堆中的一行。|  
  
## <a name="see-also"></a>另请参阅  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
