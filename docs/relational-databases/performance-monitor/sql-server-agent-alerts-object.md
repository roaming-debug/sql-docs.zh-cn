---
title: SQL Server 代理 - Alerts 对象 | Microsoft Docs
description: 了解 SQL Server 代理中的 Alerts 性能对象，该对象包含性能计数器，可报告有关 SQL Server 代理警报的信息。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Alerts object
- SQLAgent:Alerts
ms.assetid: e5e37f74-ee88-46d0-ad8f-71fd1b1fa64a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 180224d5be7a49f031b3ff37741c90a9647fd89b
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505928"
---
# <a name="sql-server-agent-alerts-object"></a>SQL Server 代理中的 Alerts 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  SQL Server 代理中的 **Alerts** 性能对象包含性能计数器，可报告有关 SQL Server 代理警报的信息。 下表列出了此对象包含的计数器。  
  
 下表介绍了 **SQLAgent:Alerts** 计数器。  
  
|名称|说明|  
|----------|-----------------|  
|**Activated alerts**|此计数器可报告自上次 SQL Server 代理重新启动以来 SQL Server 代理已经激活的警报总数。|  
|**Alerts activated/minute**|此计数器可报告上一分钟内 SQL Server 代理激活的警报数。|  
  
> [!NOTE]  
>  用户必须是 **sysadmin** 固定服务器角色的成员，才能使用此 SQL Server 代理对象。  
  
## <a name="see-also"></a>另请参阅  
 [Alerts](../../ssms/agent/alerts.md)   
 [使用性能对象](../../ssms/agent/use-performance-objects.md)   
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
