---
title: SQL Server，数据库副本 | Microsoft Docs
description: 了解 SQLServer:Database Replica 性能对象，该对象包含有关 Always On 可用性组辅助数据库的性能计数器。
ms.custom: ''
ms.date: 01/13/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- SQLServer:Database Replica
- performance counters [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], performance counters
ms.assetid: a5f6bdce-2b13-4924-aaeb-b50b57d624d8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3dff6cb9b3a65ea5f38a97786e10701bc1f31749
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170299"
---
# <a name="sql-server-database-replica"></a>SQL Server，数据库副本

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  **SQLServer:Database Replica** 性能对象包含的性能计数器报告有关 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中 Always On 可用性组的辅助数据库的信息。 此对象仅在承载辅助副本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上有效。  
  
|计数器名称|说明|有关视图…|  
|------------------|-----------------|--------------|  
|**File Bytes Received/sec**|辅助副本在最后一秒为辅助数据库接收的 FILESTREAM 数据量。|辅助副本|  
|**Log Apply Pending Queue**|正在等待应用于数据库副本的日志块数。|辅助副本|
|**Log Apply Ready Queue**|正在等待且已准备好应用于数据库副本的日志块数。|辅助副本|
|**Log Bytes Received/sec**|辅助副本在最后一秒为数据库接收的日志记录量。|辅助副本|  
|**Log remaining for undo**|为完成撤消阶段尚剩余的日志量 (KB)。|辅助副本|  
|**Log Send Queue**|主数据库的日志文件中尚未发送到次要副本的日志记录量 (KB)。 此值从主副本发送到辅助副本。 队列大小不包括发送到次要副本的文件流文件。|辅助副本|  
|**Mirrored Write Transaction/sec**|在最后一秒钟内写入主数据库，然后等待提交直到将日志发送到辅助数据库的事务数。|主副本|  
|**恢复队列**|次要副本的日志文件中尚未重做的日志记录量。|辅助副本|  
|**Redo blocked/sec**|数据库读取器持有的锁阻止的重做线程数。|辅助副本|  
|**Redo Bytes Remaining**|为完成还原阶段而要重做的剩余的日志量 (KB)。|辅助副本|  
|**Redone Bytes/sec**|在最后一秒在辅助数据库上重做的日志记录量。|辅助副本|  
|**Total Log requiring undo**|必须撤消的日志总字节数 (KB)。|辅助副本|  
|**Transaction Delay**|等待所有当前事务的未终止的提交确认的总延迟时间（以毫秒为单位）。 除以“镜像写入事务/秒”，获得“平均事务延迟时间” 。 有关详细信息，请参阅 [SQL Server 2012 AlwaysOn – Part 12 – Performance Aspects and Performance Monitoring II](/archive/blogs/saponsqlserver/sql-server-2012-alwayson-part-12-performance-aspects-and-performance-monitoring-ii)（SQL Server 2012 AlwaysOn - 第 12 节 - 性能方面和性能监视 II）|主副本|  
  
## <a name="see-also"></a>另请参阅
  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server，可用性副本](../../relational-databases/performance-monitor/sql-server-availability-replica.md)   
 [SQL Server，Databases 对象](../../relational-databases/performance-monitor/sql-server-databases-object.md)   
 [AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
