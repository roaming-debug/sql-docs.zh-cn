---
title: SQL Server - Database Mirroring 对象 | Microsoft Docs
description: 了解 SQLServer:Database Mirroring 性能对象，该对象包含报告有关 SQL Server 数据库镜像的性能计数器。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Database Mirroring
- database mirroring [SQL Server], performance counters
- performance counters [SQL Server], database mirroring
- Database Mirroring object
ms.assetid: a27b51ee-7637-4525-9424-bcc16947dc13
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f88935ba5ef8cf7f42c844e2d9a02c21d68aee97
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505761"
---
# <a name="sql-server-database-mirroring-object"></a>SQL Server Database Mirroring 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  **SQLServer:Database Mirroring** 性能对象包含报告有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库镜像的性能计数器。 下表列出了此对象包含的计数器。  
  
|名称|说明|  
|----------|-----------------|  
|**Bytes Received/sec**|每秒收到的字节数。|  
|**Bytes Sent/sec**|每秒发送的字节数。|  
|**Log Bytes Received/sec**|每秒收到的日志字节数。|  
|**Log Bytes Redone from Cache/sec**|在上一秒钟内从镜像日志缓存中获得的重做日志字节数。<br /><br /> 此计数器只在镜像服务器上使用。 在主体服务器上此值始终是 0。|  
|**Log Bytes Sent from Cache/sec**|在上一秒钟内从镜像日志缓存中获得的发送日志字节数。<br /><br /> 此计数器只在主体服务器上使用。 在镜像服务器上此值始终是 0。|  
|**Log Bytes Sent/sec**|每秒发送的日志字节数。|  
|**Log Compressed Bytes Rcvd/sec**|在上一秒钟内所接收日志的压缩字节数。|  
|**Log Compressed Bytes Sent/sec**|在上一秒钟内所发送日志的压缩字节数。|  
|**Log Harden Time (ms)**|日志块在上一秒钟内等待强制写入磁盘的时间（毫秒）。|  
|**Log Remaining for Undo KB**|在故障转移之后等待由新的镜像服务器扫描的日志总字节数 (KB)。<br /><br /> 此计数器仅可在撤消阶段在镜像服务器上使用。 撤销阶段完成后，计数器会重置为 0。 在主体服务器上此值始终是 0。|  
|**Log Scanned for Undo KB**|自故障转移开始已由新的镜像服务器扫描的日志总字节数 (KB)。<br /><br /> 此计数器仅可在撤消阶段在镜像服务器上使用。 撤销阶段完成后，计数器会重置为 0。 在主体服务器上此值始终是 0。|  
|**Log Send Flow Control Time (ms)**|日志流消息在上一秒钟内等待发送流控制的时间（毫秒）。<br /><br /> 在数据库镜像中，将日志数据和元数据发送到镜像伙伴是数据量最密集的操作，并可能独占数据库镜像和 Service Broker 发送缓冲区。 使用此计数器可监视数据库镜像会话使用此缓冲区的情况。|  
|**Log Send Queue KB**|尚未发送到镜像服务器的日志总字节数 (KB)。|  
|**Mirrored Write Transactions/sec**|在上一秒钟内写入镜像数据库并等待日志发送到镜像数据库以进行提交的事务数。<br /><br /> 仅当主体服务器正在向镜像服务器发送日志记录时，此计数器才会增加。|  
|**Pages Sent/sec**|每秒发送的页数。|  
|**Receives/sec**|每秒收到的镜像消息数。|  
|**Redo Bytes/sec**|每秒在镜像数据库中前滚的日志字节数。|  
|**Redo Queue KB**|当前仍应用于镜像数据库以进行前滚操作的镜像日志的总字节数 (KB)。 此数据将从镜像数据库发送到主体数据库。|  
|**Send/Receive Ack Time**|在上一秒钟内消息等待伙伴确认的时间（毫秒）。<br /><br /> 在解决可能由网络瓶颈导致的问题（例如莫名其妙的故障转移、发送队列很大或事务滞后时间较长）时，此计数器非常有用。 在这些情况下，可以分析此计数器的值来确定是否是由于网络而导致出现上述问题。|  
|**Sends/sec**|每秒发送的镜像消息数。|  
|**Transaction Delay**|等待未终止的提交确认的延迟时间。|  
  
> [!NOTE]  
>  根据每个伙伴当前执行的角色不同，某些计时器显示零值。  
  
## <a name="remarks"></a>备注  
 使用性能计数器可以监视数据库镜像性能。 例如，可以检查 **Transaction Delay** 计数器以确定数据库镜像是否影响主体服务器的性能，可以检查 **Redo Queue** 和 **Log Send Queue** 计数器以确定镜像数据库与主体数据库之间保持同步的情况。 还可以检查 **Log Bytes Sent/sec** 计数器以监视每秒发送的日志量。  
  
## <a name="see-also"></a>另请参阅  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
