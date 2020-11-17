---
title: WSFC 群集服务处于脱机状态 | Microsoft Docs
description: WSFC 群集状态检查 Windows Server 故障转移群集的状态。 当群集处于脱机状态或强制仲裁状态时，此策略处于不正常状态。
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp1WSFCquorum.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: d502548d-ece6-4a42-9ded-2157d33e3d21
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 13bf3e27aba6ea559ad5c1757b00367108685a9f
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583277"
---
# <a name="wsfc-cluster-service-is-offline"></a>WSFC 群集服务处于脱机状态

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]
    
## <a name="introduction"></a>简介  
  
|||  
|-|-|  
|**策略名称**|WSFC 群集状态|  
|**问题**|WSFC 群集服务处于脱机状态。|  
|**类别**|**严重**|  
|**方面**|SQL Server 实例|  
  
## <a name="description"></a>说明  
 此策略检查 Windows Server 故障转移群集 (WSFC) 的状态。 此策略处于不正常状态，在 WSFC 群集处于脱机状态或者处于强制仲裁状态时将引发警报。 此群集中承载的所有可用性组处于脱机状态或者需要灾难恢复操作。  
  
 在群集状态为正常仲裁时，此群集状态是正常的。  
  
> [!NOTE]  
>  对于此版本的 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]，与可能原因和解决方法有关的信息位于 TechNet Wiki 上的 [WSFC 群集服务处于脱机状态](https://go.microsoft.com/fwlink/p/?LinkId=220849) 中。  
  
## <a name="possible-causes"></a>可能的原因  
 此问题可能是由于群集服务问题或在群集中缺少仲裁导致的。  
  
## <a name="possible-solution"></a>可能的解决方法  
 使用群集管理器工具可执行强制仲裁或灾难恢复工作流。 如果您通过执行强制仲裁或灾难恢复无法解决该问题，请与您的群集管理员联系以便帮助解决此问题。 有关详细信息，请参阅 [联机丛书中的](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md) 在无仲裁情况下强制启动 WSFC 群集 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [使用 AlwaysOn 面板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
