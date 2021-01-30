---
description: MSdbms_datatype_mapping (Transact-SQL)
title: MSdbms_datatype_mapping (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSdbms_datatype_mapping_TSQL
- MSdbms_datatype_mapping
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_datatype_mapping system table
ms.assetid: 13289a0b-dfb0-4771-ad80-4c5f83cded99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8ea7ffaa872030b1a1d0d6ec20e17c69d64266b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160479"
---
# <a name="msdbms_datatype_mapping-transact-sql"></a>MSdbms_datatype_mapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSdbms_datatype_mapping** 表包含从源数据库管理)  (系统中的数据类型到目标 DBMS 中的一个或多个特定数据类型所允许的数据类型映射。 该表存储在 **msdb** 数据库中，用于异类数据库复制。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**datatype_mapping_id**|**int**|标识每个唯一的数据类型映射。|  
|**map_id**|**int**|标识源数据类型。|  
|**dest_datatype_id**|**int**|标识目标数据类型。|  
|**dest_precision**|**bigint**|定义目标数据类型的精度，其中，值为 NULL 表示不使用精度，值为 **-1** 表示使用源数据类型的精度。|  
|**dest_scale**|**int**|定义目标数据类型的小数位数，其中，值为 NULL 表示不使用小数位数，值为 **-1** 表示使用源数据类型的小数位数。|  
|**dest_length**|**bigint**|定义目标数据类型的长度，其中，值为 NULL 表示不使用该长度，值为 **-1** 表示使用源数据类型的长度。|  
|**dest_nullable**|**bit**|指示映射中的目标列是否允许 NULL 值，其中 NULL 值意味着此定义不是必需的。|  
|**dest_createparams**|**int**|位图，用于说明适用于每种数据类型的长度、精度和小数位数组合，其中包括：<br /><br /> **0x1** = 精度。<br /><br /> **0x2** = SCALE。<br /><br /> **0x4** = LENGTH。|  
  
## <a name="see-also"></a>另请参阅  
 [异类数据库复制](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [指定 Oracle 发布服务器的数据类型映射](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
