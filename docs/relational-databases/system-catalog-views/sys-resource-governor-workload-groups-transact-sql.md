---
description: sys.resource_governor_workload_groups (Transact-SQL)
title: sys.resource_governor_workload_groups (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.resource_governor_workload_groups
- resource_governor_workload_groups_TSQL
- sys.resource_governor_workload_groups_TSQL
- resource_governor_workload_groups
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_workload_groups catalog view
ms.assetid: 619ba4b7-868f-4784-b527-ec1dfd703c4f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d89b9210d0da27b3b4d45e8f4d2652960cf0c1d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183632"
---
# <a name="sysresource_governor_workload_groups-transact-sql"></a>sys.resource_governor_workload_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的存储工作负荷组配置。 每个工作负荷组都可以订阅一个且只能订阅一个资源池。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|group_id|**int**|工作负荷组的唯一 ID。 不可为 null。|  
|name|**sysname**|工作负荷组的名称。 不可为 null。|  
|importance|**sysname**|**注意：** 重要性仅适用于同一资源池中的工作负荷组。<br /><br /> 此工作负荷组中的请求的相对重要性。 重要性为下列值之一，默认值为：低、中、高。<br /><br /> 不可为 null。|  
|request_max_memory_grant_percent|**int**|授予单个请求的最大内存量（以百分比表示）。 默认值为 25。 不可为 null。<br /><br /> **注意：** 如果此设置大于50%，则大型查询一次运行一个。 因此，在查询正在运行时出现内存不足错误的风险更高。|  
|request_max_cpu_time_sec|**int**|针对单个请求的最大 CPU 使用限制（以秒为单位）。 默认值为 0，指定没有限制。 不可为 null。<br /><br /> **注意：** 有关详细信息，请参阅 [CPU 阈值超出事件类别](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md)。|  
|request_memory_grant_timeout_sec|**int**|针对单个请求的内存授予超时（以秒为单位）。 默认值为 0，表示使用基于查询开销的内部计算。 不可为 null。|  
|max_dop|**int**|工作负荷组的最大并行度。 默认值为 0，表示使用全局设置。 不可为 null。<br /><br /> **Node：** 此设置将替代查询选项 **maxdop**。|  
|group_max_requests|**int**|并发请求的最大数目。 默认值为 0，指定没有限制。 不可为 null。|  
|pool_id|**int**|此工作负荷组使用的资源池的 ID。|  
|external_pool_id|**int**|**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本。<br /><br /> 此工作负荷组使用的外部资源池的 ID。|  
  
## <a name="remarks"></a>备注  
 目录视图显示存储的元数据。 若要查看内存中的配置，请使用对应的动态管理视图， [sys.dm_resource_governor_workload_groups &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)。  
  
 如果资源调控器配置已发生更改，但尚未应用 ALTER RESOURCE GOVERNOR RECONFIGURE 语句，则存储的配置和内存中的配置可能会不同。  
  
## <a name="permissions"></a>权限  
 若要查看内容，则需要拥有 VIEW ANY DEFINITION 权限；若要更改内容，则需要拥有 CONTROL SERVER 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_resource_governor_workload_groups (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Resource Governor 目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)  
  
  
