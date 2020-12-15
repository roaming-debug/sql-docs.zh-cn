---
description: 'sys.pdw_health_component_status_mappings (Transact-sql) '
title: sys.pdw_health_component_status_mappings (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
ms.assetid: 4272cfad-5ad7-493d-9edd-d9111619bda0
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: d0d5bfea5d43315fed6b74f8767cea6cb89904a9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466938"
---
# <a name="syspdw_health_component_status_mappings-transact-sql"></a>sys.pdw_health_component_status_mappings (Transact-sql) 
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  定义 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 组件状态与制造商定义的组件名称之间的映射。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|property_id|**int**|属性的唯一标识符。<br /><br /> property_id、component_id 和 physical_name 构成此视图的键。|NOT NULL|  
|component_id|**int**|组件的 ID。 请参阅 [&#40;transact-sql&#41;sys.pdw_health_components ](../../relational-databases/system-catalog-views/sys-pdw-health-components-transact-sql.md)。<br /><br /> property_id、component_id 和 physical_name 构成此视图的键。|NOT NULL|  
|physical_name|**nvarchar(32)**|制造商定义的属性名称。<br /><br /> property_id、component_id 和 physical_name 构成此视图的键。|NOT NULL|  
|logical_name|**nvarchar(255)**|由定义的属性名称 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 。|NOT NULL<br /><br /> 0-设备实例是唯一的。<br /><br /> 1-设备实例不唯一。|  
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
