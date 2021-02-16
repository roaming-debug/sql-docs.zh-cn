---
title: 可用性组的可用性副本连接已断开
description: 确定在 AlwaysOn 可用性组中副本连接断开的可能原因。
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
f1_keywords:
- sql13.swb.agdashboard.arp2connected.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 1a2162d3-54fb-4356-b349-effbdc15a5a4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 371a4e1e76cfeb79eeb83f2dabd159927e044008
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343880"
---
# <a name="availability-replica-is-disconnected-within-an-always-on-availability-group"></a>AlwaysOn 可用性组中的可用性副本连接已断开
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>介绍  
  
|||  
|-|-|  
|**策略名称**|可用性副本连接状态|  
|**问题**|断开可用性副本的连接。|  
|**类别**|**严重**|  
|**方面**|可用性副本|  
  
## <a name="description"></a>说明  
 此策略检查可用性副本之间的连接状态。 当可用性副本的连接状态为 DISCONNECTED 时，此策略处于不正常状态。 否则，该策略处于正常状态。  
  
## <a name="possible-causes"></a>可能的原因  
 辅助副本未连接到主副本。 连接状态为 DISCONNECTED。 此问题可能由以下原因导致：  
  
-   连接端口可能与另一个应用程序冲突。  
  
-   加密类型或算法不匹配。  
  
-   连接端点已被删除或尚未启动。  
  
-   传输已断开连接。  
  
## <a name="possible-solutions"></a>可能的解决方法  
 以下是此问题的可能解决方法：  
  
-   检查主副本和辅助副本实例的数据库镜像端点配置，并更新不匹配的配置。  
  
-   检查端口是否冲突，如果冲突，请更改端口号。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [使用 AlwaysOn 面板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
