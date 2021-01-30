---
description: 'sys.pdw_column_distribution_properties (Transact-sql) '
title: 'sys.pdw_column_distribution_properties (Transact-sql) '
ms.custom: seo-dt-2019
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 46b74f99-2e22-4dbd-872a-533fce0e239c
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: d812be285cb2a89aaa6f83db2d08312dfed939af
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191220"
---
# <a name="syspdw_column_distribution_properties-transact-sql"></a>sys.pdw_column_distribution_properties (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  保存列的分发信息。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|object_id|**int**|列所属对象的 ID。||  
|column_id|**int**|列的 ID。||  
|**distribution_ordinal**|**tinyint**|分布中基于 (1 的) 顺序。|0 = 不是分布列。 1 = [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 使用此列来分布父表。|  
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
