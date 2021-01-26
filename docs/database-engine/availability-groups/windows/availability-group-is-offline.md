---
title: 可用性组处于脱机状态
description: 了解如何确定 Always On 可用性组可能脱机的时间和可能原因。
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.agp2online.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 093c5208-bf7a-49f4-a546-72b48197cadf
author: cawrites
ms.author: chadam
ms.openlocfilehash: a0c27e56044476217de8be84181115336a6ff015
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766046"
---
# <a name="always-on-availability-group-is-offline"></a>Always On 可用性组处于脱机状态
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>介绍  
  
|||  
|-|-|  
|**策略名称**|可用性组联机状态|  
|**问题**|可用性组处于脱机状态。|  
|**类别**|**严重**|  
|**方面**|可用性组 (availability group)|  
  
## <a name="description"></a>说明  
 此策略检查可用性组的联机或脱机状态。 当可用性组的群集资源处于脱机状态或可用性组不具有主副本时，此策略处于不正常状态并引发警报。  
  
 当可用性组的群集资源处于联机状态并且可用性组具有主副本时，此策略处于正常状态。
  
## <a name="possible-causes"></a>可能的原因  
 此问题可能由承载主副本的服务器实例中的失败或 Windows Server 故障转移群集 (WSFC) 可用性组资源脱机引起。 可用性组脱机可能有以下原因：  
  
-   可用性组未配置为自动故障转移模式。 主副本变为不可用且可用性组中所有副本的角色变为 RESOLVING。  
  
    -   主副本实例服务已关闭或停止响应。  
  
    -   可用性组与群集的连接有问题。  
  
-   可用性组配置为自动故障转移模式，但是未成功完成。  
  
    -   在自动故障转移期间，对目标副本的主副本就绪检查失败，并且没有副本可以变为新的主副本。  
  
-   群集中的可用性组资源变为脱机状态。  
  
    -   任何依赖的群集资源遇到严重问题并变为脱机状态。 可用性组资源还处于脱机状态，直到依赖的资源变为联机状态。  
  
    -   群集中的严重问题导致禁用了可用性组资源。  
  
-   对可用性组存在正在进行的自动、手动或强制故障转移。  
  
## <a name="possible-solutions"></a>可能的解决方法  
 以下是此问题的可能解决方法：  
  
-   如果主副本的 SQL Server 实例关闭，请重新启动服务器，然后验证可用性组恢复到正常状态。  
  
-   如果自动故障转移失败，请验证副本上的数据库与以前已知的主副本同步，然后故障转移到主副本。 如果数据库不同步，请选择数据丢失最少的副本，然后恢复到故障转移模式。  
  
-   如果群集中的资源在 SQL Server 实例处于正常状态时脱机，请使用故障转移群集管理器检查群集运行状况或服务器上的其他群集问题。 您还可以使用故障转移群集管理器尝试将可用性组资源联机。  
  
-   如果正在进行故障转移，请等待故障转移完成。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [使用 AlwaysOn 面板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
