---
title: 识别瓶颈 | Microsoft Docs
description: 了解导致瓶颈的原因，例如 SQL Server 中资源不足和资源故障/配置不正确。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- resource bottlenecks [SQL Server]
- database monitoring [SQL Server], bottlenecks
- performance [SQL Server], bottlenecks
- tuning databases [SQL Server], bottlenecks
- monitoring server performance [SQL Server], bottlenecks
- monitoring performance [SQL Server], bottlenecks
- database performance [SQL Server], bottlenecks
- server performance [SQL Server], bottlenecks
- bottlenecks [SQL Server]
- identifying bottlenecks [SQL Server]
ms.assetid: db079e65-ee80-4105-aec9-f8230d0d6635
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 66a29131862d0b7cb3972fd8a52b5fa21ab79ef3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463268"
---
# <a name="identify-bottlenecks"></a>识别瓶颈
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  对共享资源同时访问会导致瓶颈。 通常，每一软件系统都不可避免地存在瓶颈。 然而，对共享资源的过多需求将导致响应时间过长，因此必须进行识别和优化。  
  
 导致瓶颈的原因包括：  
  
-   资源不足，需要添加或升级组件。  
  
-   工作负荷在同类资源之间分布不均（例如，一个磁盘被独占）。  
  
-   资源发生故障。  
  
-   资源配置不正确。  
  
## <a name="analyzing-bottlenecks"></a>分析瓶颈  
 如果有多个事件的持续时间都过长，则表明存在能被优化的瓶颈。  
  
 例如：  
  
-   当某项工作试图访问某个组件时，某些其他组件可能加以妨碍，从而延长完成该工作所需的时间。  
  
-   客户端请求可能因网络阻塞而花费更长时间。  
  
 下面是跟踪服务器性能以识别瓶颈时应监视的五个主要方面。  
  
|可能的瓶颈方面|对服务器的影响|  
|------------------------------|---------------------------|  
|内存使用率|分配的内存不足或可由 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用的内存不足导致性能下降。 数据必须从磁盘读取而非直接从数据缓存读取。 当需要页时，Microsoft Windows 操作系统将通过与磁盘交换数据来执行大量分页操作。|  
|CPU 使用率|长期的高 CPU 使用率可能表明 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询需要优化或 CPU 需要升级。|  
|磁盘输入/输出 (I/O)|[!INCLUDE[tsql](../../includes/tsql-md.md)] 可以优化查询以减少不必要的 I/O（例如，使用索引）。|  
|用户连接|可能有太多用户同时访问服务器，从而导致性能下降。|  
|阻塞锁|应用程序设计不合理可能导致锁定或妨碍并发，因而导致更长的响应时间和更低的事务吞吐速度。|  
  
## <a name="see-also"></a>另请参阅  
 [监视 CPU 使用率](../../relational-databases/performance-monitor/monitor-cpu-usage.md)   
 [监视磁盘使用情况](../../relational-databases/performance-monitor/monitor-disk-usage.md)   
 [监视内存使用量](../../relational-databases/performance-monitor/monitor-memory-usage.md)   
 [SQL Server General Statistics 对象](../../relational-databases/performance-monitor/sql-server-general-statistics-object.md)   
 [SQL Server - Locks 对象](../../relational-databases/performance-monitor/sql-server-locks-object.md)  
  
  
