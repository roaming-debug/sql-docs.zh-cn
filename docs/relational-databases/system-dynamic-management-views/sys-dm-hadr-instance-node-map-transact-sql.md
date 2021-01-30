---
description: sys.dm_hadr_instance_node_map (Transact-SQL)
title: sys.dm_hadr_instance_node_map (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sys.dm_hadr_instance_node_map_TSQL
- sys.sys.dm_hadr_instance_node_map
- sys.dm_hadr_instance_node_map_TSQL
- sys.dm_hadr_instance_node_map
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC
- sys.sys.dm_hadr_instance_node_map dynamic management view
ms.assetid: ccfaf62c-9f87-43cf-a5e7-8942e91dd041
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7632bf8f343c1dd906e574e3ded3688fa2a812ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188674"
---
# <a name="sysdm_hadr_instance_node_map-transact-sql"></a>sys.dm_hadr_instance_node_map (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 承载联接到其 Always On 可用性组的可用性副本的每个实例，将返回承载该服务器实例的 Windows Server 故障转移群集 (WSFC) 节点的名称。 此动态管理视图具有以下用法：  
  
-   此动态管理视图对于检测包含承载于同一 WSFC 节点上的多个可用性副本的可用性组很有用，这是一个不受支持的配置，如果可用性组的配置不正确，则在进行 FCI 故障转移后可能出现此配置。 有关详细信息，请参阅[故障转移群集和 AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)。  
  
-   当多个 SQL Server 实例承载于同一 WSFC 节点上时，资源 DLL 将使用此动态管理视图来确定要连接到的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
   
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**ag_resource_id**|**nvarchar(256)**|作为 WSFC 中的资源的可用性组的唯一 ID。|  
|**instance_name**|**nvarchar(256)**|名称-*服务器* / *实例*-承载可用性组的副本的服务器实例。|  
|**node_name**|**nvarchar(256)**|WSFC 节点的名称。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [Always On 可用性组动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [AlwaysOn 可用性组目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [&#40;Transact-sql 监视可用性组&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
