---
description: Locks 事件类别
title: “锁定”事件类别 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Locks event category [SQL Server]
- SQL Server event classes, Locks event category
- event classes [SQL Server], Locks event category
- lock escalation [SQL Server], locks event category
ms.assetid: 27d6afa2-7dab-4fe7-a1ad-064b879dc654
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ed5d792cd88f067ae014cb5191a363c51f336b7c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467988"
---
# <a name="locks-event-category"></a>Locks 事件类别
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用 Locks 事件类别中的事件类可监视 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例中的锁定活动。 这些事件类有助于调查由于多个用户同时读取和修改数据而引起的锁定问题。  
  
 由于 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 通常会处理很多锁，因此在跟踪期间捕获 **Locks** 事件类别会带来很大的开销并生成大型跟踪文件或表。  
  
## <a name="in-this-section"></a>本节内容  
  
|主题|说明|  
|-----------|-----------------|  
|[Deadlock Graph 事件类](../../relational-databases/event-classes/deadlock-graph-event-class.md)|提供死锁的 XML 说明。|  
|[Lock:Acquired 事件类](../../relational-databases/event-classes/lock-acquired-event-class.md)|指示已获取资源（例如表中的行）的锁。|  
|[Lock:Cancel 事件类](../../relational-databases/event-classes/lock-cancel-event-class.md)|跟踪在获取锁之前取消的锁请求（例如，为防止死锁）。|  
|[Lock:Deadlock Chain 事件类](../../relational-databases/event-classes/lock-deadlock-chain-event-class.md)|监视出现死锁条件的时间和涉及的对象。|  
|[Lock:Deadlock 事件类](../../relational-databases/event-classes/lock-deadlock-event-class.md)|跟踪事务何时针对已由另一个事务锁定（导致死锁）的资源请求锁。|  
|[Lock:Escalation 事件类](../../relational-databases/event-classes/lock-escalation-event-class.md)|指示较细粒度的锁已转换为较粗粒度的锁。|  
|[Lock:Released 事件类](../../relational-databases/event-classes/lock-released-event-class.md)|跟踪何时释放锁。|  
|[Lock:Timeout (timeout > 0) 事件类](../../relational-databases/event-classes/lock-timeout-timeout-0-event-class.md)|由于另一个事务持有所需资源的阻塞锁而使锁请求无法完成时，跟踪该锁请求。 仅在锁超时值大于零的情况下才会发生此类事件。|  
|[Lock:Timeout 事件类](../../relational-databases/event-classes/lock-timeout-event-class.md)|由于另一个事务持有所需资源的阻塞锁而使锁请求无法完成时，跟踪该锁请求。|  
  
  
