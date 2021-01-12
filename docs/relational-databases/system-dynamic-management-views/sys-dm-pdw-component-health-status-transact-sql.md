---
description: 'sys.dm_pdw_component_health_status (Transact-sql) '
title: sys.dm_pdw_component_health_status (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 68cc3f7a-693c-4d5d-a76b-455352af8d7f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 51883c9ea851e25cdf3e58b1d4c47c8112440b43
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097520"
---
# <a name="sysdm_pdw_component_health_status-transact-sql"></a>sys.dm_pdw_component_health_status (Transact-sql) 
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  保存有关设备组件的当前运行状况的信息。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**||不为 NULL|  
|component_id|int|组件的 ID。 请参阅 [&#40;transact-sql&#41;sys.pdw_health_components ](../../relational-databases/system-catalog-views/sys-pdw-health-components-transact-sql.md)。<br /><br /> pdw_node_id、component_id、property_id 和 component_instance_id 构成此视图的键。|不为 NULL|  
|property_id|**int**|属性的 ID。 请参阅 [&#40;transact-sql&#41;sys.pdw_health_component_properties ](../../relational-databases/system-catalog-views/sys-pdw-health-component-properties-transact-sql.md)。|NOT NULL|  
|component_instance_id|**nvarchar(255)**|标识组件的实例。 例如，可以 component_instance_id = "CPU1" 标识 CPU 的实例。<br /><br /> pdw_node_id、component_id、property_id 和 component_instance_id 构成此视图的键。|NOT NULL|  
|property_value|**nvarchar(255)**|当前属性值。|Null|  
|update_time|**datetime**|上次更新指标的时间。|NOT NULL|  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
