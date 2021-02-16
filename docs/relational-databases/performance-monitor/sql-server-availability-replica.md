---
title: SQL Server，可用性副本 | Microsoft Docs
description: 了解 SQLServer:Availability Replica 性能对象，该对象包含的性能计数器与 AlwaysOn 可用性组中的可用性副本有关。
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- performance counters [SQL Server], AlwaysOn Availability Groups
- SQLServer:Availability Replica
- Availability Groups [SQL Server], performance counters
ms.assetid: e402f996-c1fb-484a-b804-45c49972f2e0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 39df44f69e8a4cd05b5cefda27aefcd20d2100e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338150"
---
# <a name="sql-server-availability-replica"></a>SQL Server，可用性副本

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  在 **中，** SQLServer:Availability Replica [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]性能对象包含的性能计数器报告有关 AlwaysOn 可用性组中可用性副本的信息。 所有可用性副本性能计数器都适用于主副本和辅助副本，并且具有反映本地副本的发送/接收计数器。 大多数情况下，主副本发送大部分数据，而辅助副本将接收这些数据。 但是，辅助副本会将 ACK 和其他一些后台流量发送到主副本。 请注意，在某个给定可用性副本上，某些计数器将显示零值，具体取决于该本地副本的当前角色，即是主副本还是辅助副本。  
  
|计数器名称|说明|  
|------------------|-----------------|  
|**Bytes Received from Replica/sec**|**在 SQL Server 2012 和 2014 中：** 每秒从可用性副本接收的实际字节（压缩）数（同步或异步）。 Ping 和状态更新将生成网络流量，甚至在没有用户更新的数据库上也是如此。 <BR/> <BR/> **SQL Server 2016 及更高版本：** 每秒从可用性副本接收的实际字节数（异步压缩，同步不压缩）。|  
|**Bytes Sent to Replica/sec**|**在 SQL Server 2012 和 2014 中：** 每秒通过网络发送到远程可用性副本的实际字节（压缩）数（同步或异步）。 默认情况下会同时为同步副本和异步副本启用压缩。 <BR/> <BR/> **在 SQL Server 2016 及更高版本中：** 每秒发送到远程可用性副本的字节数。 在同步副本的压缩之前。 （未压缩的同步副本的实际字节数）|  
|**Bytes Sent to Transport/sec**|**在 SQL Server 2012 和 2014 中：** 每秒通过网络发送到远程可用性副本的实际字节（压缩）数（同步或异步）。 默认情况下会同时为同步副本和异步副本启用压缩。 <BR/> <BR/> **在 SQL Server 2016 及更高版本中：** 在同步副本压缩之前每秒发送到远程可用性副本的字节数。 （未压缩的同步副本的实际字节数）|  
|**Flow Control Time (ms/sec)**|日志流消息在最后一秒钟内等待发送流控制的时间（毫秒）。|  
|**Flow Control/sec**|在最后一秒钟内启动的流控制次数。 **Flow Control Time (ms/sec)** 除以 **Flow Control/sec** 为每次等待的平均时间。|  
|**Receives from Replica/sec**|每秒从副本接收的 Always On 消息的数量。|  
|**Resent Messages/sec**|在最后一秒钟内重新发送的 Always On 消息的数量。|  
|**Sends to Replica/sec**|每秒发送到此可用性副本的 Always On 消息的数量。|  
|**Sends to Transport/sec**|每秒通过网络发送到远程可用性副本的 Always On 消息的实际数量。 在主副本上，这是发送到辅助副本的消息数。 在复制副本上，这是发送到主副本的消息数。|  
  
## <a name="see-also"></a>另请参阅 
 
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server，数据库副本](../../relational-databases/performance-monitor/sql-server-database-replica.md)   
 [AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
