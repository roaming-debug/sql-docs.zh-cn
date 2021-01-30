---
description: 'sys.dm_resource_governor_external_resource_pool_affinity (Transact-sql) '
title: sys.dm_resource_governor_external_resource_pool_affinity (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_resource_governor_external_resource_pool_affinity
- sys.dm_resource_governor_external_resource_pool_affinity_TSQL
- dm_resource_governor_external_resource_pool_affinity
- dm_resource_governor_external_resource_pool_affinity_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_external_resource_pool_affinity
- dm_resource_governor_external_resource_pool_affinity
ms.assetid: e32fac49-5161-47c0-8540-af3fe730c00c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1dbbbbfacabb47fc1c18bfce9930ae93bc0f266b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203377"
---
# <a name="sysdm_resource_governor_external_resource_pool_affinity-transact-sql"></a>sys.dm_resource_governor_external_resource_pool_affinity (Transact-sql) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]
**适用于：** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)][!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)]和 [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)][!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)]

返回有关当前外部资源池配置的 CPU 关联信息。
  
|列名称|数据类型|说明|
|----------------|---------------|-----------------|
|pool_id|**int**|外部资源池的 ID。 不可为 null。|
|processor_group|**smallint**|Windows 逻辑处理器组的 ID。 不可为 null。|
|cpu_mask|**bigint**|表示与此池关联的 Cpu 的二进制掩码。 不可为 null。|
  
## <a name="remarks"></a>备注

使用关联的创建的池 `AUTO` 不会出现在此视图中，因为它们没有关联。 有关详细信息，请参阅 [CREATE EXTERNAL RESOURCE pool &#40;transact-sql&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md) 和 [ALTER external Resource pool &#40;transact-sql&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md) 语句。

## <a name="permissions"></a>权限

需要 `VIEW SERVER STATE` 权限。

## <a name="see-also"></a>另请参阅

[SQL Server 中机器学习的资源调控](../../machine-learning/administration/resource-governor.md)

[sys.dm_resource_governor_resource_pool_affinity &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pool-affinity-transact-sql.md)

[“已启用外部脚本”服务器配置选项](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)

[ALTER EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)
