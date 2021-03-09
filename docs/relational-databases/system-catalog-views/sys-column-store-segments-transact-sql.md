---
description: sys.column_store_segments (Transact-SQL)
title: sys.column_store_segments (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/24/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- column_store_segments
- sys.column_store_segments_TSQL
- sys.column_store_segments
- column_store_segments_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_store_segments catalog view
ms.assetid: 1253448c-2ec9-4900-ae9f-461d6b51b2ea
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 51c5e31a97a52e94a8a6d164c6d43425451494b1
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102463965"
---
# <a name="syscolumn_store_segments-transact-sql"></a>sys.column_store_segments (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

为列存储索引中的每个列段返回一行。 每个行组的每个列都有一个列段。 例如，具有10行组和34列的表将返回340行。 
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**partition_id**|**bigint**|指示分区 ID。 在数据库中是唯一的。|  
|**hobt_id**|**bigint**|具有此列存储索引的表的堆或B 树索引 (HoBT) 的 ID。|  
|column_id|**int**|列存储列的 ID。|  
|**segment_id**|**int**|行组的 ID。 为实现向后兼容性，即使这是行组 ID，列名仍将继续 segment_id 调用。 使用 <segment_id> 可以唯一地标识段 \<hobt_id, partition_id, column_id> 。|  
|**version**|**int**|列段格式的版本。|  
|**encoding_type**|**int**|用于该段的编码类型：<br /><br /> 1 = 不含字典的 VALUE_BASED 非字符串/二进制文件 (类似于4，具有一些内部变体) <br /><br /> 2 = 在字典中具有通用值的 VALUE_HASH_BASED 非字符串/二进制列<br /><br /> 3 = STRING_HASH_BASED 字典中包含通用值的字符串/二进制列<br /><br /> 4 = 不带字典的 STORE_BY_VALUE_BASED 非字符串/二进制<br /><br /> 5 = STRING_STORE_BY_VALUE_BASED 字符串/二进制，无字典<br /><br /> 有关详细信息，请参阅[备注](#remarks)部分。|  
|**row_count**|**int**|行组中的行数。|  
|**has_nulls**|**int**|1 如果列段具有 Null 值。|  
|**base_id**|**bigint**|如果正在使用编码类型1，则为基值 ID。 如果未使用编码类型1，则 base_id 设置为-1。|  
|**magnitude**|**float**|如果正在使用编码类型1，则为数量级。 如果未使用编码类型1，则数量级设置为-1。|  
|**primary_dictionary_id**|**int**|值0表示全局字典。 值-1 指示没有为此列创建全局字典。|  
|**secondary_dictionary_id**|**int**|如果为非零值，则指向当前段中此列的本地字典 (即行组) 。 值-1 指示此段没有本地字典。|  
|**min_data_id**|**bigint**|列段中的最小数据 ID。|  
|**max_data_id**|**bigint**|列段中的最大数据 ID。|  
|**null_value**|**bigint**|用于表示 Null 的值。|  
|**on_disk_size**|**bigint**|段大小（字节）。|  
  
## <a name="remarks"></a>备注  
的列存储段编码类型由选择，其 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 目标是通过分析分段数据来实现最低的存储成本。 如果数据大多不同，则 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 使用基于值的编码。 如果数据主要不是唯一的，则 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 使用基于哈希的编码。 基于字符串和基于值的编码之间的选择与要存储的数据的类型有关，无论字符串数据还是二进制数据。 如果可能，所有编码都利用位打包和长度为长度的编码。
 
## <a name="permissions"></a>权限  
 所有列都要求至少 `VIEW DEFINITION` 对表具有权限。 以下各列将返回 null，除非用户也具有 `SELECT` 权限： `has_nulls` 、 `base_id` 、、、 `magnitude` `min_data_id` `max_data_id` 和 `null_value` 。  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  

## <a name="examples"></a>示例

### <a name="the-following-query-returns-information-about-segments-of-a-columnstore-index"></a>以下查询返回有关列存储索引各段的信息。  
  
```sql  
SELECT i.name, p.object_id, p.index_id, i.type_desc,   
    COUNT(*) AS number_of_segments  
FROM sys.column_store_segments AS s   
INNER JOIN sys.partitions AS p   
    ON s.hobt_id = p.hobt_id   
INNER JOIN sys.indexes AS i   
    ON p.object_id = i.object_id  
WHERE i.type = 5 OR i.type = 6  
GROUP BY i.name, p.object_id, p.index_id, i.type_desc ;  
GO  
```  

## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)   
 [列存储索引指南](~/relational-databases/indexes/columnstore-indexes-overview.md)    
 [sys.column_store_dictionaries (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)  
  
 
