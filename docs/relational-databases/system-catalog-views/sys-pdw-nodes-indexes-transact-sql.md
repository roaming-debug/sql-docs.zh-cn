---
description: 'sys.pdw_nodes_indexes (Transact-sql) '
title: sys.pdw_nodes_indexes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 261bcb7f-a906-4979-b274-bc5f1aa66426
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: a9fee9ec599444091e0543b0ed7e4fd326fdaa02
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186504"
---
# <a name="syspdw_nodes_indexes-transact-sql"></a>sys.pdw_nodes_indexes (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  返回的索引 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|object_id|**int**|此索引所属的对象的 id。||  
|name|**sysname**|索引的名称。 名称仅在对象中是唯一的。 NULL = 堆||  
|index_id|**int**|索引的 id。 index_id 只在该对象中是唯一的。<br /><br /> 0 = 堆<br /><br /> 1 = 聚集索引<br /><br /> > 1 = 非聚集索引||  
|type|**tinyint**|索引的类型：<br /><br /> 0 = 堆<br /><br /> 1 = 聚集<br /><br /> 2 = 非聚集<br /><br /> 5 = 聚集 xVelocity 内存优化的列存储索引|  
|type_desc|**nvarchar(60)**|索引类型的说明：<br /><br /> HEAP<br /><br /> CLUSTERED<br /><br /> NONCLUSTERED<br /><br /> 聚集列存储||  
|is_unique|**bit**|0 = 索引不是唯一的。|始终为 0。|  
|data_space_id|**int**|此索引的数据空间的 id。 数据空间是文件组或分区方案。<br /><br /> 0 = object_id 是表值函数。||  
|ignore_dup_key|**bit**|0 = IGNORE_DUP_KEY 是 OFF。|始终为 0。|  
|is_primary_key|**bit**|1 = 索引是 PRIMARY KEY 约束的一部分。|始终为 0。|  
|is_unique_constraint|**bit**|1 = 索引是 UNIQUE 约束的一部分。|始终为 0。|  
|fill_factor|**tinyint**|> 0 = 创建或重新生成索引时使用的 FILLFACTOR 百分比。<br /><br /> 0 = 默认值|始终为 0。|  
|is_padded|**bit**|0 = PADINDEX 是 OFF。|始终为 0。|  
|is_disabled|**bit**|1 = 禁用索引。<br /><br /> 0 = 不禁用索引。||  
|is_hypothetical|**bit**|0 = 索引不是假设的。|始终为 0。|  
|allow_row_locks|**bit**|1 = 索引允许行锁。|始终为1。|  
|allow_page_locks|**bit**|1 = 索引允许页锁。|始终为1。|  
|has_filter|**bit**|0 = 索引不具有筛选器。|始终为 0。|  
|filter_definition|**nvarchar(max)**|包含在筛选索引中的行子集的表达式。|始终为 NULL。|  
|pdw_node_id|**int**|节点的唯一标识符 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。|NOT NULL|  
  
## <a name="permissions"></a>权限  
 需要 CONTROL SERVER 权限。  
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
