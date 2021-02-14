---
description: sys.availability_read_only_routing_lists (Transact-SQL)
title: sys.availability_read_only_routing_lists (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- availability_read_only_routing_lists_TSQL
- availability_read_only_routing_lists
- sys.availability_read_only_routing_lists
- sys.availability_read_only_routing_lists_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- read-only routing
- Availability Groups [SQL Server], readable secondary replicas
- Availability Groups [SQL Server], read-only routing
- readable secondary replicas
- sys.availability_read_only_routing_lists dynamic management view
ms.assetid: 0686bc5a-c206-41ef-b40a-79a8259d51d2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: caee2c4d248b9cdba9a5f7539e375ce8379a23b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338942"
---
# <a name="sysavailability_read_only_routing_lists-transact-sql"></a>sys.availability_read_only_routing_lists (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为 WSFC 故障转移群集中 AlwaysOn 可用性组的每个可用性副本的只读路由列表返回一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|拥有路由列表的可用性副本的唯一 ID。|  
|**routing_priority**|**int**|路由的优先级顺序（1 表示第一级，2 表示第二级，依此类推）。|  
|**read_only_replica_id**|**uniqueidentifier**|只读工作负荷将路由到的可用性副本的唯一 ID。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [Always On 可用性组动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [AlwaysOn 可用性组目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [&#40;Transact-sql 监视可用性组&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
