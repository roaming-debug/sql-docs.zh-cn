---
title: 可用性组的可用性数据库挂起
description: 确定 Always On 可用性组中的数据库挂起的可能原因。
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.drp1notsuspended.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 6baee70f-848c-4e86-b20d-78875c0f82cb
author: cawrites
ms.author: chadam
ms.openlocfilehash: 36e9640011bb273fee05a1eefa899e10bb595eff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340915"
---
# <a name="availability-database-is-suspended-for-an-availability-group"></a>可用性组的可用性数据库挂起
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>介绍  
  
|||  
|-|-|  
|**策略名称**|可用性数据库挂起状态|  
|**问题**|可用性数据库挂起。|  
|**类别**|**警告**|  
|**方面**|可用性数据库|  
  
## <a name="description"></a>说明  
 此策略检查辅助数据库（也称为“辅助数据库副本”）的数据移动状态。 数据移动挂起时，此策略处于不正常状态。 否则，该策略处于正常状态。  
  
## <a name="possible-causes"></a>可能的原因  
 由于下列原因，此可用性数据库上的数据同步可能已挂起：  
  
-   由于错误，系统可能已挂起数据同步。  
  
-   数据库管理员出于维护目的可能已挂起数据同步。  
  
## <a name="possible-solution"></a>可能的解决方法  
 继续数据同步。 如果问题仍然存在，请在事件日志中查看该可用性组，然后诊断系统挂起数据移动的原因。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [使用 AlwaysOn 面板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
