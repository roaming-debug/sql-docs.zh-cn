---
description: sys.dm_hadr_availability_replica_cluster_states (Transact-SQL)
title: sys.dm_hadr_availability_replica_cluster_states (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_availability_replica_cluster_states_TSQL
- dm_hadr_availability_replica_cluster_states
- sys.dm_hadr_availability_replica_cluster_states
- dm_hadr_availability_replica_cluster_states_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_availability_replica_cluster_states dynamic management view
ms.assetid: 2e0dd780-6a71-4f4b-b7f7-6e063bec71d6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 44469591b42b29166c11cf431a58371387e67e53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354190"
---
# <a name="sysdm_hadr_availability_replica_cluster_states-transact-sql"></a>sys.dm_hadr_availability_replica_cluster_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为 Windows Server 故障转移群集 (WSFC) 群集中所有 AlwaysOn 可用性组（无论副本位置如何）的每个 AlwaysOn 可用性副本（无论加入状态如何）都返回一行。  
  
##  <a name="connected_state"></a>  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|可用性副本的唯一标识符。|  
|**replica_server_name**|**nvarchar(256)**|承载副本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|  
|**group_id**|**uniqueidentifier**|可用性组的唯一标识符。|  
|**join_state**|**tinyint**|0 = 未联接<br /><br /> 1 = 已联接，独立<br /><br /> 2 = 已联接，故障转移群集实例|  
|**join_state_desc**|**nvarchar(60)**|NOT_JOINED<br /><br /> JOINED_STANDALONE<br /><br /> JOINED_FAILOVER_CLUSTER_INSTANCE|  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [监视可用性组 (Transact-SQL)](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
