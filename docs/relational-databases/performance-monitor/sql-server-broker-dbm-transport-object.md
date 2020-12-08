---
title: SQL Server - Broker - DBM Transport 对象 | Microsoft Docs
description: 了解 Broker/DBM Transport 性能对象，该对象包含报告 Service Broker 和数据库镜像的网络信息的性能计数器。
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Broker / DBM Transport object
- SQLServer:Broker/DBM Transport
ms.assetid: eddb60b6-20a9-416c-adf3-4bc1687944fa
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 89eb1e6e2ab2f030a189054e32dbe0c96f43687b
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505846"
---
# <a name="sql-server-broker---dbm-transport-object"></a>SQL Server Broker - DBM Transport 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  **Broker / DBM Transport** 性能对象包含报告 Service Broker 和数据库镜像的网络信息的性能计数器。 下表列出了此对象包含的计数器。  
  
|SQL Server Broker/DBM Transport 计数器|说明|  
|------------------------------------------------|-----------------|  
|**Current Bytes for Recv I/O**|此计数器报告当前运行的传输接收操作读取的字节数。|  
|**Current Bytes for Send I/O**|此计数器报告当前通过网络正被发送的消息片段中的字节数。|  
|**Current Msg Frags for Send I/O**|此计数器报告通过网络正被发送的消息片段的总数。|  
|**Message Fragment P1 Sends/sec**|此计数器报告每秒通过网络发送的优先级为 1 的消息片段数。|  
|**Message Fragment P2 Sends/sec**|此计数器报告每秒通过网络发送的优先级为 2 的消息片段数。|  
|**Message Fragment P3 Sends/sec**|此计数器报告每秒通过网络发送的优先级为 3 的消息片段数。|  
|**Message Fragment P4 Sends/sec**|此计数器报告每秒通过网络发送的优先级为 4 的消息片段数。|  
|**Message Fragment P5 Sends/sec**|此计数器报告每秒通过网络发送的优先级为 5 的消息片段数。|  
|**Message Fragment P6 Sends/sec**|此计数器报告每秒通过网络发送的优先级为 6 的消息片段数。|  
|**Message Fragment P7 Sends/sec**|此计数器报告每秒通过网络发送的优先级为 7 的消息片段数。|  
|**Message Fragment P8 Sends/sec**|此计数器报告每秒通过网络发送的优先级为 8 的消息片段数。|  
|**Message Fragment P9 Sends/sec**|此计数器报告每秒通过网络发送的优先级为 9 的消息片段数。|  
|**Message Fragment P10 Sends/sec**|此计数器报告每秒通过网络发送的优先级为 10 的消息片段数。|  
|**Message Fragment Receives/sec**|此计数器报告每秒通过网络接收的消息片段数。|   
|**Message Fragment Sends/sec**|此计数器报告每秒通过网络发送的所有优先级的消息片段数。|  
|**Msg Fragment Recv Size Avg**|此计数器报告通过网络接收的消息片段的平均大小。|  
|**Msg Fragment Recv Size Avg Base**|仅供内部使用。| 
|**Msg Fragment Send Size Avg**|此计数器报告通过网络发送的消息片段的平均大小。|  
|**Msg Fragment Send Size Avg Base**|仅供内部使用。|
|**Open Connection Count**|此计数器报告 Service Broker 当前已经打开的网络连接数。|  
|**Pending Bytes for Recv I/O**|此计数器报告已经从网络接收但尚未放置到队列中或尚未放弃的消息片段中包含的字节数。|  
|**Pending Bytes for Send I/O**|此计数器报告准备好通过网络发送的消息片段中的字节总数。|  
|**Pending Msg Frags for Recv I/O**|此计数器报告已经从网络接收但尚未放置到队列中或尚未放弃的消息片段数。|  
|**Pending Msg Frags for Send I/O**|此计数器报告准备好通过网络发送的消息片段的总数。|  
|**Receive I/O bytes/sec**|此计数器报告每秒通过网络由 Service Broker 端点和数据库镜像端点接收的字节数。|  
|**Receive I/O Bytes Total**|此计数器报告通过网络由 Service Broker 端点和数据库镜像端点接收的字节总数。|  
|**Receive I/O Len Avg**|此计数器报告传输接收操作的字节平均数。|  
|**Receive I/O Len Avg Base**|仅供内部使用。|
|**Receive I/Os/sec**|此计数器报告每秒 Service Broker / DBM transport 层已完成的传输接收 I/O 操作数。 注意，传输接收操作可能包含多个消息片段。|  
|**Recv I/O Buffer Copies bytes/sec**|传输接收 I/O 操作必须将缓冲区片段移入内存时的速率。|
|**Recv I/O Buffer Copies Count**|传输接收 I/O 操作必须将缓冲区片段移入内存的次数。| 
|**Send I/O bytes/sec**|此计数器报告 Service Broker 端点和数据库镜像端点每秒通过网络传送的字节数。|   
|**Send I/O Bytes Total**|此计数器报告 Service Broker 端点和数据库镜像端点通过网络传送的总字节数。| 
|**Send I/O Len Avg**|此计数器报告每个传输发送操作的平均大小（以字节为单位）。 注意，传输发送操作可能包含多个消息片段。|  
|**Send I/O Len Avg Base**|仅供内部使用。|
|**Send I/Os/sec**|此计数器报告每秒已完成的传输发送 I/O 操作数。 注意，传输发送操作可能包含多个消息片段。|  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_broker_forwarded_messages (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-broker-forwarded-messages-transact-sql.md)   
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
