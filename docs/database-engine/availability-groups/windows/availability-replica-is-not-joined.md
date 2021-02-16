---
title: 可用性副本未联接到可用性组
description: 了解如何确定可能导致副本未联接到 Always On 可用性组的原因。
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
f1_keywords:
- sql13.swb.agdashboard.arp4joined.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 9c0d10b1-9e12-430c-83b9-ca2bd0a3afc4
author: cawrites
ms.author: chadam
ms.openlocfilehash: d80170723101e1e78e70d06f91653e4592b3560d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343825"
---
# <a name="availability-replica-is-not-joined-to-an-always-on-availability-group"></a>可用性副本未联接到 Always On 可用性组
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>介绍  
  
|||  
|-|-|  
|**策略名称**|可用性副本联接状态|  
|**问题**|可用性副本未联接。|  
|**类别**|**警告**|  
|**方面**|可用性副本|  
  
## <a name="description"></a>说明  
 此策略检查可用性副本的联接状态。 当可用性副本添加到可用性组但未正确联接时，该策略将处于不正常状态。 否则，该策略处于正常状态。  
  
## <a name="possible-causes"></a>可能的原因  
 辅助副本未联接到可用性组。 要将某一可用性副本成功联接到可用性组，联接状态必须为“已联接独立实例”(1) 或“已联接故障转移群集”(2)。  
  
## <a name="possible-solution"></a>可能的解决方法  
 使用 Transact-SQL、PowerShell 或 SQL Server Management Studio 将辅助副本联接到可用性组。 有关将辅助副本联接到可用性组的详细信息，请参阅 [将辅助副本联接到可用性组 (SQL Server)](https://msdn.microsoft.com/library/ff878473\(SQL.110\).aspx)。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [使用 AlwaysOn 面板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
