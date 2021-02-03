---
description: MSSQLSERVER_1222
title: MSSQLSERVER_1222 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1222 (Database Engine error)
ms.assetid: c5b1c2f4-f591-4cc1-aa17-204636a27f29
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9e77d9799a8d78c34ee47b13fc728f20361c3cb2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197160"
---
# <a name="mssqlserver_1222"></a>MSSQLSERVER_1222
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|1222|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|LK_TIMEOUT|  
|消息正文|已超过了锁请求超时时段。|  
  
## <a name="explanation"></a>说明  
另一个事务持有必需资源的锁的时间比此查询可以等待该资源的时间长。  
  
## <a name="user-action"></a>用户操作  
执行以下任务以缓解该问题：  
  
1.  如有可能，请找出持有必需资源的锁的事务。 使用 **sys.dm_os_waiting_tasks** 和 **sys.dm_tran_locks** 动态管理视图。  
  
2.  如果事务仍持有该锁，请终止该事务（如何适合）。  
  
3.  重新执行查询。  

如果频繁出现此错误，请更改锁超时期限，或者修改有问题的事务以便它们持有锁的时间减少。  
  
