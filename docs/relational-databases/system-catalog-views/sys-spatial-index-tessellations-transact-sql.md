---
description: sys.spatial_index_tessellations (Transact-SQL)
title: sys.spatial_index_tessellations (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- spatial_index_tessellations
- sys.spatial_index_tessellations_TSQL
- spatial_index_tessellations_TSQL
- sys.spatial_index_tessellations
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_index_tessellations catalog view
ms.assetid: 8b17a9a4-b57f-4220-8138-fc73581b1670
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ddf7b9cd75999da6cc48f6e7a83bab7a733f822b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095409"
---
# <a name="sysspatial_index_tessellations-transact-sql"></a>sys.spatial_index_tessellations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

 表示每个空间索引的分割方案和参数的相关信息。  
  
> [!NOTE]  
>  有关分割的信息，请参阅[空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)。  
  

|列名称|数据类型|描述|  
|-----------------|---------------|-----------------|  
|object_id|**int**|定义了索引的对象的 ID。 每个 (object_id，index_id) 对在 [sys.spatial_indexes](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)中都有相应的条目。|  
|index_id|**int**|定义了索引列的空间索引的 ID。|  
|tessellation_scheme|**sysname**|分割方案的名称，如下所示： GEOMETRY_GRID、GEOGRAPHY_GRID|  
|bounding_box_xmin|**float(53)**|边界框左下角的 X 坐标，其中之一： NULL = 不适用于给定的分割方案 (例如 GEOGRAPHY_GRID) *n* = 如果 tessellation_scheme 为 GEOMETRY_GRID，则为最小 x 坐标值。                     **注意：** 边界框参数定义的坐标将根据其 [空间引用标识符 (SRID)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md)解释为每个对象。|  
|bounding_box_ymin|**float(53)**|边界框左下角的 Y 坐标，其中之一： NULL = 不适用于给定分割方案 (例如 GEOGRAPHY_GRID) *n* = 如果 tessellation_scheme 为 GEOMETRY_GRID，则为最小 y 坐标值|  
|bounding_box_xmax|**float(53)**|边界框右上角的 X 坐标，其中之一： NULL = 不适用于给定分割方案 (例如 GEOGRAPHY_GRID) *n* = 如果 tessellation_scheme 为 GEOMETRY_GRID，则为最大 x 坐标值|  
|bounding_box_ymax|**float(53)**|边界框右上角的 Y 坐标，其中之一： NULL = 不适用于给定分割方案 (例如 GEOGRAPHY_GRID) *n* = 如果 tessellation_scheme 为 GEOMETRY_GRID，则为最大 y 坐标值|  
|level_1_grid|**smallint**|顶级网格的网格密度。 如果 tessellation_scheme 是 GEOMETRY_GRID 或 GEOGRAPHY_GRID，则为以下其中一个： 16 = 4 x 4 个网格 (低) 64 = 8 x 8 个网格 (中等) 256 = 16 x 16 个网格 (高) NULL = 不适用于给定的空间索引类型或分割方案。  在使用新的 SQL Server 11 分割时，将返回 NULL。|  
|level_1_grid_desc|**nvarchar(60)**|顶级网格的网格密度，其中一种： LOW 中高 NULL = 不适用于给定的空间索引类型或分割方案。  在使用新的 SQL Server 11 分割时，将返回 NULL。|  
|level_2_grid|**smallint**|第二级网格的网格密度。 如果 tessellation_scheme 是 GEOMETRY_GRID 或 GEOGRAPHY_GRID，则为以下其中一个： 16 = 4 x 4 个网格 (低) 64 = 8 x 8 个网格 (中等) 256 = 16 x 16 个网格 (高) NULL = 不适用于给定的空间索引类型或分割方案。  在使用新的 SQL Server 11 分割时，将返回 NULL。|  
|level_2_grid_desc|**nvarchar(60)**|第二级网格的网格密度，其中一个： LOW 中高 NULL = 不适用于给定的空间索引类型或分割方案。  在使用新的 SQL Server 11 分割时，将返回 NULL。|  
|level_3_grid|**smallint**|第三级网格的网格密度。   如果 tessellation_scheme 是 GEOMETRY_GRID 或 GEOGRAPHY_GRID，则为以下其中一个： 16 = 4 x 4 个网格 (低) 64 = 8 x 8 个网格 (中等) 256 = 16 x 16 个网格 (高) NULL = 不适用于给定的空间索引类型或分割方案。  在使用新的 SQL Server 11 分割时，将返回 NULL。|  
|level_3_grid_desc|**nvarchar(60)**|第三级网格的网格密度，其中一个： LOW 中高 NULL = 不适用于给定的空间索引类型或分割方案。  在使用新的 SQL Server 11 分割时，将返回 NULL。|  
|level_4_grid|**smallint**|第四级网格的网格密度。 如果 tessellation_scheme 是 GEOMETRY_GRID 或 GEOGRAPHY_GRID，则为以下其中一个： 16 = 4 x 4 个网格 (低) 64 = 8 x 8 个网格 (中等) 256 = 16 x 16 个网格 (高) NULL = 不适用于给定的空间索引类型或分割方案。  在使用新的 SQL Server 11 分割时，将返回 NULL。|  
|level_4_grid_desc|**nvarchar(60)**|第四级网格的网格密度，其中之一为： < LOW 中高 NULL = 不适用于给定的空间索引类型或分割方案。  在使用新的 SQL Server 11 分割时，将返回 NULL。|  
|cells_per_object|**int**|每个空间对象的单元格数，其中之一：如果 tessellation_scheme GEOMETRY_GRID 或 GEOGRAPHY_GRID， *n* = 每个对象的单元格数 NULL = 不适用于给定的空间索引类型或分割方案|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.spatial_indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
