---
title: SQL Server，内存节点 | Microsoft Docs
description: 了解“内存节点”对象，该对象在 SQL Server 中的 NUMA 节点上提供监视服务器内存使用情况的计数器。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: 55b28ba9-b6d5-4ea9-8103-db8a72f42982
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7f1c3f161d59e3bbf08036cc4c15d3380f2484dd
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505621"
---
# <a name="sql-server-memory-node"></a>SQL Server、内存节点
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Microsoft **中的** “内存节点” [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象在 NUMA 节点上提供监视服务器内存使用情况的计数器。  
  
## <a name="memory-node-counters"></a>内存节点计数  
 下表描述 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]“内存节点”计数器。  
  
|SQL Server Memory Manager 计数器|说明|  
|----------------------------------------|-----------------|  
|**Database Node Memory (KB)**|指定服务器当前在此节点上用于数据库页面的内存量。|  
|**Free Node Memory (KB)**|指定服务器在此节点上未使用的内存量。|  
|**Foreign Node Memory (KB)**|指定此节点上非 NUMA 本地内存的量。|  
|**Stolen Memory Node (KB)**|指定服务器在此节点上出于数据库页面以外的目的使用的内存量。|  
|**Target Node Memory**|指定此节点的理想内存量。|  
|**Total Node Memory**|指示服务器在此节点上已提交的总内存量。|  
  
## <a name="see-also"></a>另请参阅  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server Buffer Manager 对象](../../relational-databases/performance-monitor/sql-server-buffer-manager-object.md)   
 [sys.dm_os_performance_counters (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
  
