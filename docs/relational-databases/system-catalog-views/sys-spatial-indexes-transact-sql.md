---
description: sys.spatial_indexes (Transact-SQL)
title: sys.spatial_indexes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.spatial_indexes_TSQL
- spatial_indexes
- spatial_indexes_TSQL
- sys.spatial_indexes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_indexes catalog view
ms.assetid: 40e967d5-2e8d-45af-bf5e-5251493cf7cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 22d70c60c3234e57cff277e1ba298da9f55148ff
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093015"
---
# <a name="sysspatial_indexes-transact-sql"></a>sys.spatial_indexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  表示空间索引的主索引信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|\<inherited columns>||继承 [sys.databases](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)中的列。|  
|spatial_index_type|**tinyint**|空间索引的类型：<br /><br /> 1 = 几何空间索引<br /><br /> 2 = 地理空间索引|  
|spatial_index_type_desc|**nvarchar(60)**|空间索引的类型说明：<br /><br /> GEOMETRY = 几何空间索引<br /><br /> GEOGRAPHY = 地理空间索引|  
|tessellation_scheme|**sysname**|分割方案的名称：<br /><br /> GEOMETRY_GRID、GEOMETRY_AUTO_GRID、<br /><br /> GEOGRAPHY_GRID、GEOGRAPHY_AUTO_GRID<br /><br /> 注意：有关分割方案的信息，请参阅 [空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)。|  
|\<inherited columns>||继承 [sys.databases](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)中的列。<br /><br /> 继承的列 has_filter 和 filter_definition 显示在特定于空间索引的列之后。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.spatial_index_tessellations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)  
  
  
