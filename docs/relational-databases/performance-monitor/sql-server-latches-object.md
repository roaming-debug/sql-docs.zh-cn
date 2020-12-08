---
title: SQL Server - Latches 对象 | Microsoft Docs
description: 了解 SQLServer:Latches 对象，该对象提供计数器以监视称为闩锁的内部 SQL Server 资源锁。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Latches object
- SQLServer:Latches
ms.assetid: 2393ea1c-2bf3-41c3-9f37-b9761144eeca
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 01d5eaa40f6e9deac5703b35b8859165218b40c3
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505668"
---
# <a name="sql-server-latches-object"></a>SQL Server Latches 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Microsoft **中的** SQLServer:Latches [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象提供计数器来监视称为闩锁的内部 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源锁。 通过监视闩锁来确定用户活动和资源使用情况，将有助于查明性能瓶颈。  
  
 下表介绍了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Latches** 计数器。  
  
|SQL Server Latches 计数器|说明|  
|---------------------------------|-----------------|  
|**Average Latch Wait Time (ms)**|必须等待授予的闩锁请求的平均等待时间（毫秒）。|  
|**Average Latch Wait Time Base**|仅供内部使用。| 
|**Latch Waits/sec**|未能立即授予的闩锁请求数。|  
|**Number of SuperLatches**|目前是 SuperLatch 的闩锁数。|  
|**SuperLatch Demotions/sec**|在上一秒钟内已降级为常规闩锁的 SuperLatch 数。|  
|**SuperLatch Promotions/sec**|在上一秒钟内已提升为 SuperLatch 的闩锁数。|  
|**Total Latch Wait Time (ms)**|上一秒钟内的闩锁请求的总等待时间（毫秒）。|  
  
## <a name="see-also"></a>另请参阅  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
