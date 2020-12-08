---
title: SQL Server - Broker TO Statistics 对象 | Microsoft Docs
description: 了解 SQLServer:Broker TO Statistics 性能对象，该对象报告有关 Service Broker 请求传输对象的信息。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Broker Transmission Object object
- 'SQL Server: Broker Transmission Object'
ms.assetid: b5bc5dde-e540-4848-8aa3-5735c51df2fb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: eabaa3854012b747e028babca44c749d1d51206e
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505825"
---
# <a name="sql-server-broker-to-statistics-object"></a>SQL Server Broker TO Statistics 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  SQLServer:Broker TO Statistics 性能对象报告有关 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 对话框请求传输对象的次数以及传输对象写入 tempdb 中的频率的信息。  
  
 传输对象记录 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 对话框的消息传输状态。 这些对象存储在内存中。 为了释放内存， [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会定期将各批非活动传输对象写入 **tempdb** 的工作表中。  
  
 下表列出了此对象包含的计数器。  
  
|SQL Server Broker TO Statistics 计数器|说明|  
|----------------------------------------------|-----------------|  
|**页的Length of Batched Writes**|保存在一个批次中的传输对象平均数。|  
|**页的Time To Write Batch (ms)**|保存一批传输对象所需的平均毫秒数。|  
|**页的Time to Write Batch Base**|仅供内部使用。|
|**页的Time Between Batches (ms)**|不同传输对象批次写入之间间隔的平均毫秒数。|  
|**页的Time Between Batches Base**|仅供内部使用。| 
|**Tran Object Gets/sec**|对话框每秒请求传输对象的次数。|  
|**Tran Objects Marked Dirty/sec**|传输对象每秒标记为“脏”的次数。 当第一次出现导致内存中的副本不同于存储在 **tempdb** 中的副本的修改时，传输对象会被其标记为“脏”。 当 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 必须在对话框的消息传输状态下记录更改时，将会修改传输对象。|  
|**Tran Object Writes/sec**|每秒将各批传输对象写入 **tempdb** 工作表的次数。 大量写入可表明 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存正在承受很大的压力。|  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Access Methods 对象](../../relational-databases/performance-monitor/sql-server-access-methods-object.md)   
 [SQL Server Memory Manager 对象](../../relational-databases/performance-monitor/sql-server-memory-manager-object.md)   
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
