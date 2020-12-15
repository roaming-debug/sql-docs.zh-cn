---
description: 'sys.pdw_nodes_column_store_dictionaries (Transact-sql) '
title: 'sys.pdw_nodes_column_store_dictionaries (Transact-sql) '
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.custom: seo-dt-2019
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 7ae1c2e4-45c0-4880-a692-1f299fbcfd19
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: c64f087631b6104af14fc793c06946515220a51a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475158"
---
# <a name="syspdw_nodes_column_store_dictionaries-transact-sql"></a>sys.pdw_nodes_column_store_dictionaries (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  列存储索引中使用的每个字典都包含一行。 字典用于对某些而非全部数据类型进行编码，因此并非列存储索引中的所有列都有字典。 字典可以作为主字典存在（对于所有段），也可能作为用于部分列段的其他辅助字典存在。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**partition_id**|**bigint**|指示分区 ID。 在数据库中是唯一的。|  
|**hobt_id**|**bigint**|具有此列存储索引的表的堆或B 树索引 (HoBT) 的 ID。|  
|column_id|**int**|列存储列的 ID。|  
|**dictionary_id**|**int**|字典的 ID。|  
|**version**|**int**|字典格式的版本。|  
|**type**|**int**|字典类型：<br /><br /> 1-包含 **int** 值的哈希字典<br /><br /> 2-未使用<br /><br /> 3-包含字符串值的哈希字典<br /><br /> 包含 **浮点** 值的4哈希字典|  
|**last_id**|**int**|字典中的最后一个数据 ID。|  
|**entry_count**|**bigint**|字典中的条目数。|  
|**on_disc_size**|**bigint**|字典大小（以字节为单位）。|  
|pdw_node_id|**int**|节点的唯一标识符 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。|  
  
## <a name="permissions"></a>权限  
 需要 `VIEW SERVER STATE` 权限。  
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [&#40;Transact-sql&#41;创建列存储索引 ](../../t-sql/statements/create-columnstore-index-transact-sql.md)   
 [sys.pdw_nodes_column_store_segments &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-segments-transact-sql.md)   
 [sys.pdw_nodes_column_store_row_groups &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-row-groups-transact-sql.md)  
  
  
