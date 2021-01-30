---
description: 'sys.pdw_nodes_column_store_segments (Transact-sql) '
title: 'sys.pdw_nodes_column_store_segments (Transact-sql) '
ms.custom: seo-dt-2019
ms.date: 03/28/2018
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: e2fdf8e9-1b74-4682-b2d4-c62aca053d7f
author: julieMSFT
ms.author: jrasnick
manager: jrj
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 08e99e71c4086f60b869b9d25c6b0d0f4deec35d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186565"
---
# <a name="syspdw_nodes_column_store_segments-transact-sql"></a>sys.pdw_nodes_column_store_segments (Transact-sql) 

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

对于 columnstore 索引中的每列包括一行。

| 列名称                 | 数据类型  | 说明                                                  |
| :-------------------------- | :--------- | :----------------------------------------------------------- |
| **partition_id**            | **bigint** | 指示分区 ID。 在数据库中是唯一的。     |
| **hobt_id**                 | **bigint** | 具有此 columnstore 索引的表的堆或 B 树 (hobt) 的 ID。 |
| column_id               | **int**    | 列存储列的 ID。                                |
| **segment_id**              | **int**    | 列段的 ID。 为实现向后兼容性，即使这是行组 ID，列名仍将继续 segment_id 调用。 您可以使用 <hobt_id，partition_id column_id> <segment_id> 来唯一标识段。 |
| **version**                 | **int**    | 列段格式的版本。                        |
| **encoding_type**           | **int**    | 用于该段的编码类型：<br /><br /> 1 = 不含字典的 VALUE_BASED 非字符串/二进制文件 (类似于4，具有一些内部变体) <br /><br /> 2 = 在字典中具有通用值的 VALUE_HASH_BASED 非字符串/二进制列<br /><br /> 3 = STRING_HASH_BASED 字典中包含通用值的字符串/二进制列<br /><br /> 4 = 不带字典的 STORE_BY_VALUE_BASED 非字符串/二进制<br /><br /> 5 = STRING_STORE_BY_VALUE_BASED 字符串/二进制，无字典<br /><br /> 如果可能，所有编码都利用位打包和长度为长度的编码。 |
| **row_count**               | **int**    | 行组中的行数。                             |
| **has_nulls**               | **int**    | 1 如果列段具有 Null 值。                     |
| **base_id**                 | **bigint** | 如果正在使用编码类型1，则为基值 ID。  如果未使用编码类型1，则 base_id 设置为1。 |
| **magnitude**               | **float**  | 如果正在使用编码类型1，则为数量级。  如果未使用编码类型1，则数量级设置为1。 |
| **primary__dictionary_id**  | **int**    | 主字典的 ID。 如果为非零值，则指向当前段中此列的本地字典 (即行组) 。 值-1 指示此段没有本地字典。 |
| **secondary_dictionary_id** | **int**    | 辅助字典的 ID。 如果为非零值，则指向当前段中此列的本地字典 (即行组) 。 值-1 指示此段没有本地字典。 |
| **min_data_id**             | **bigint** | 列段中的最小数据 ID。                       |
| **max_data_id**             | **bigint** | 列段中的最大数据 ID。                       |
| **null_value**              | **bigint** | 用于表示 Null 的值。                               |
| **on_disk_size**            | **bigint** | 段大小（字节）。                                    |
| pdw_node_id             | **int**    | 节点的唯一标识符 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。 |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

联接 sys.pdw_nodes_column_store_segments 与其他系统表，以确定每个逻辑表的列存储段数。

```sql
SELECT  sm.name           as schema_nm
,       tb.name           as table_nm
,       nc.name           as col_nm
,       nc.column_id
,       COUNT(*)          as segment_count
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb                   ON  sm.[schema_id]          = tb.[schema_id]
JOIN    sys.[pdw_table_mappings] mp       ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt         ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[pdw_nodes_partitions] np     ON  np.[object_id]          = nt.[object_id]
                                          AND np.[pdw_node_id]        = nt.[pdw_node_id]
                                          AND np.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_nodes_columns] nc        ON  np.[object_id]          = nc.[object_id]
                                          AND np.[pdw_node_id]        = nc.[pdw_node_id]
                                          AND np.[distribution_id]    = nc.[distribution_id]
JOIN    sys.[pdw_nodes_column_store_segments] rg  ON  rg.[partition_id]         = np.[partition_id]
                                                      AND rg.[pdw_node_id]      = np.[pdw_node_id]
                                                      AND rg.[distribution_id]  = np.[distribution_id]
                                                      AND rg.[column_id]        = nc.[column_id]
GROUP BY    sm.name
,           tb.name
,           nc.name
,           nc.column_id  
ORDER BY    table_nm
,           nc.column_id
,           sm.name ;
```

## <a name="permissions"></a>权限

需要 VIEW SERVER STATE 权限  。

## <a name="see-also"></a>另请参阅

[Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
[CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md)  
[sys.pdw_nodes_column_store_row_groups &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-row-groups-transact-sql.md)  
[sys.pdw_nodes_column_store_dictionaries &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-dictionaries-transact-sql.md)
