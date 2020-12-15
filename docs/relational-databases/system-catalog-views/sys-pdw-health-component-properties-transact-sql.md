---
description: 'sys.pdw_health_component_properties (Transact-sql) '
title: sys.pdw_health_component_properties (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
ms.assetid: 66999c0c-dc43-4327-99fb-8366f465e69d
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: a6446cb81a33fef53ac398f9e23bd080b310ede4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439472"
---
# <a name="syspdw_health_component_properties-transact-sql"></a>sys.pdw_health_component_properties (Transact-sql) 
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  存储用于描述设备的属性。 某些属性显示设备状态，一些属性描述设备本身。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|property_id|**int**|组件的属性的唯一标识符。<br /><br /> property_id 和 component_id 构成此视图的键。|NOT NULL|  
|component_id|**int**|组件的 ID。 请参阅 [&#40;transact-sql&#41;sys.pdw_health_components ](../../relational-databases/system-catalog-views/sys-pdw-health-components-transact-sql.md)。<br /><br /> property_id 和 component_id 构成此视图的键。|NOT NULL|  
|property_name|**nvarchar(255)**|属性的名称。|NOT NULL|  
|physical_name|**nvarchar(32)**|制造商定义的属性名称。|NOT NULL|  
|is_key|**bit**|确定设备实例是否唯一。|NOT NULL<br /><br /> 0-设备实例是唯一的。<br /><br /> 1-设备实例不唯一。|  
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
