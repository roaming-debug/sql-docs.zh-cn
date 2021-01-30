---
description: MSdbms_map (Transact-SQL)
title: MSdbms_map (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSdbms_map
- MSdbms_map_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_map system table
ms.assetid: df67e691-3a50-450a-99c5-8c4a041749ae
author: cawrites
ms.author: chadam
ms.openlocfilehash: 41d0283235329d286f3ed3d6e92403c0fa20ae66
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160466"
---
# <a name="msdbms_map-transact-sql"></a>MSdbms_map (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSdbms_map** 表包含源数据类型信息，以及指向源和目标 DBMS 对的默认目标数据类型信息的链接。 该表存储在 **msdb** 数据库中，用于异类发布。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**map_id**|**int**|唯一标识数据类型映射。|  
|**src_dbms_id**|**int**|通过在 [MSdbms](../../relational-databases/system-tables/msdbms-transact-sql.md)表中指定源 DBMS **dbms_id** 来标识该源 DBMS。|  
|**dest_dbms_id**|**int**|通过在 [MSdbms](../../relational-databases/system-tables/msdbms-transact-sql.md)表中指定其 **dbms_id** 来标识目标 DBMS。|  
|**src_datatype_id**|**int**|标识源数据类型 [MSdbms_datatype](../../relational-databases/system-tables/msdbms-datatype-transact-sql.md)表中的 **datatype_id** 。|  
|**src_len_min**|**bigint**|源 DBMS 中的数据类型的最小长度，值为 NULL 表示不使用该长度。|  
|**src_len_max**|**bigint**|源 DBMS 中的数据类型的最大长度，值为 NULL 表示不使用该长度。|  
|**src_prec_min**|**bigint**|源 DBMS 中的数据类型的最小精度，值为 NULL 表示不使用该精度。|  
|**src_prec_max**|**bigint**|源 DBMS 中的数据类型的最大精度，值为 NULL 表示不使用该精度。|  
|**src_scale_min**|**int**|源 DBMS 中的数据类型的最小小数位数，值为 NULL 表示不使用该小数位数。|  
|**src_scale_max**|**int**|源 DBMS 中的数据类型的最大小数位数，值为 NULL 表示不使用该小数位数。|  
|**src_nullable**|**bit**|指示映射中的目标列是否允许 NULL 值，其中 NULL 值意味着此定义不是必需的。|  
|**default_datatype_mapping_id**|**int**|通过在表 [MSdbms_datatype_mapping](../../relational-databases/system-tables/msdbms-datatype-mapping-transact-sql.md)中指定默认的数据类型 **map_id** 来标识该映射。|  
  
## <a name="see-also"></a>另请参阅  
 [异类数据库复制](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [指定 Oracle 发布服务器的数据类型映射](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
