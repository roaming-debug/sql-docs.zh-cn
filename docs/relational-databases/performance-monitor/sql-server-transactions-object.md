---
title: SQL Server - Transactions 对象 | Microsoft Docs
description: 了解事务对象，该对象提供计数器以监视数据库引擎中的活动事务以及 SQL Server 中事务的影响。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Transactions
- Transactions object
ms.assetid: 85240267-78fd-476a-9ef6-010d6cf32dd8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2f86c13a6f4cc6672b457653f6d923b63245fbc7
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505541"
---
# <a name="sql-server-transactions-object"></a>SQL Server Transactions 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Microsoft **中的** Transactions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象提供的计数器用于监视 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例中处于活动状态的事务的数量，以及这些事务对资源（如 **tempdb** 中的快照隔离行版本存储区）的影响。 事务是工作的逻辑单位，是一组要么必须全部成功要么必须全部从数据库中清除（以便保持数据的逻辑完整性）的操作。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中，对数据的所有修改都在事务中进行。  
  
 当数据库设置为允许快照隔离级别时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须保留对数据库中每行进行修改的记录。 每次修改行时，由于在修改前行已存在，所以行的副本将记录在 **tempdb** 中的行版本存储区。 **Transaction** 对象中的许多计数器可用来监视 **tempdb** 中的行版本存储区的大小和增长速率。  
  
 **Transactions** 对象计数器报告一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的所有事务。  
  
 下表说明了 **SQLServer:Transactions** 计数器。  
  
|SQL Server Transactions 计数器|说明|  
|--------------------------------------|-----------------|  
|**Free Space in tempdb (KB)**|**tempdb** 中的可用空间量 (KB)。 必须具有足够的可用空间以保存快照隔离级别版本存储区和在 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例中创建的所有新临时对象。|  
|**Longest Transaction Running Time**|比任何其他当前事务活动时间都长的事务启动后运行的时间长度（秒）。 在数据库处于读取已提交读快照隔离级别之下时，该计数器只显示活动。 如果数据库处于任何其他隔离级别中，该计数器将不会记录任何活动。|  
|**NonSnapshot Version Transactions**|未使用快照隔离级别并进行了数据修改（这些修改在 **tempdb** 版本存储区中生成行版本）的当前活动事务的数目。|  
|**Snapshot Transactions**|使用快照隔离级别的当前活动事务的数目。<br /><br /> 注意：Snapshot Transactions 对象计数器在进行第一次数据访问时响应，而不是在发出 `BEGIN TRANSACTION` 语句时响应。|  
|**中的**|当前活动的所有类型的事务的数目。|  
|**Update conflict ratio**|使用快照隔离级别的、在最后一秒内遇到更新冲突的事务的百分比。 更新冲突在以下情况下发生：快照隔离级别事务尝试修改一行，但该行最近一次修改由在快照隔离级别事务启动时未提交的其他事务执行。|  
|**Update conflict ratio base**|仅供内部使用。|
|**Update Snapshot Transactions**|使用快照隔离级别并已修改数据的当前活动事务的数目。|  
|**Version Cleanup rate (KB/s)**|行版本从 **tempdb** 中的快照隔离版本存储区中删除的速率（KB/秒）。|  
|**Version Generation rate (KB/s)**|向 **tempdb** 中的快照隔离版本存储区中添加新行版本的速率（KB/秒）。|  
|**Version Store Size (KB)**|**tempdb** 中用来存储快照隔离级别行版本的空间量 (KB)。|  
|**Version Store unit count**|**tempdb** 中的快照隔离版本存储区中的活动分配单元的数目。|  
|**Version Store unit creation**|[!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例启动后，在快照隔离存储区中创建的分配单元的数目。|  
|**Version Store unit truncation**|[!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例启动后，从快照隔离存储区中删除的分配单元的数目。|  
  
## <a name="see-also"></a>另请参阅  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
