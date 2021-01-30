---
description: 'sys.pdw_nodes_column_store_row_groups (Transact-sql) '
title: 'sys.pdw_nodes_column_store_row_groups (Transact-sql) '
ms.custom: seo-dt-2019
ms.date: 08/05/2020
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 17a4c925-d4b5-46ee-9cd6-044f714e6f0e
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: d4b9ea267b0dbf23ff304dc7632a2fdc742d37dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186575"
---
# <a name="syspdw_nodes_column_store_row_groups-transact-sql"></a>sys.pdw_nodes_column_store_row_groups (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  按段提供聚集列存储索引信息，以帮助管理员在中做出系统管理决策 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。 **sys.pdw_nodes_column_store_row_groups** 包含了物理存储的总行数的列 (包括标记为已删除的行) 以及标记为已删除的行数的列。 使用 **sys.pdw_nodes_column_store_row_groups** 确定哪些行组的已删除行的百分比很高，应重新生成这些行组。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|基础表的 ID。 这是计算节点上的物理表，而不是控制节点上逻辑表的 object_id。 例如，object_id 与 sys.databases 中的 object_id 不匹配。<br /><br /> 若要与 sys.databases 联接，请使用 sys.pdw_index_mappings。|  
|index_id|**int**|*Object_id* 表上的聚集列存储索引的 ID。|  
|**partition_number**|**int**|保存行组 *row_group_id* 的表分区的 ID。 你可以使用 *partition_number* 将此 DMV 加入 sys.databases。|  
|**row_group_id**|**int**|此行组的 ID。 这在分区中是唯一的。|  
|**dellta_store_hobt_id**|**bigint**|delta 行组的 hobt_id；或如果行组类型不是 delta，则为 NULL。 delta 行组是正在接受新记录的读/写行组。 增量行组具有 **打开** 状态。 delta 行组仍采用行存储格式，并且尚未压缩成列存储格式。|  
|**state**|**tinyint**|与 state_description 关联的 ID 号。<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED|  
|**state_desccription**|**nvarchar(60)**|行组的持久状态的说明：<br /><br /> OPEN-正在接受新记录的读/写行组。 开放的行组仍采用行存储格式，并且尚未压缩成列存储格式。<br /><br /> 已关闭-已填充但尚未由元组移动器进程压缩的行组。<br /><br /> 压缩-已填充和压缩的行组。|  
|**total_rows**|**bigint**|行组中物理存储的总行数。 一些行可能已删除，但它们仍被存储。 一个行组中的最大行数为 1,048,576（十六进制 FFFFF）。|  
|**deleted_rows**|**bigint**|在标记为删除的行组中物理存储的行数。<br /><br /> 对于增量行组，始终为0。|  
|**size_in_bytes**|**int**|此行组中所有页面的组合大小（以字节为单位）。 此大小不包括存储元数据或共享字典所需的大小。|  
|pdw_node_id|**int**|节点的唯一 ID [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。|  
|distribution_id|**int**|分布的唯一 ID。|
  
## <a name="remarks"></a>备注  
 针对每个表中具有聚合或非聚合列存储索引的每个列存储行组返回一行。  
  
 使用 **sys.pdw_nodes_column_store_row_groups** 来确定行组中包含的行数和行组的大小。  
  
 当行组中的已删除行数量增长到占总行数的较大百分比时，该表的效率将下降。 重新生成列存储索引以减少表的大小，同时减少读取该表所需的磁盘 I/O。 若要重新生成列存储索引，请使用 **ALTER index** 语句的 **rebuild** 选项。  
  
 可更新的列存储首先将新数据插入到处于行存储格式的 **打开** 行组中，有时也称为增量表。  打开的行组已满后，其状态将更改为 " **已关闭**"。 由元组移动器将关闭的行组压缩为列存储格式，并将状态更改为已 **压缩**。  元组搬运者是一个后台进程，它定期唤醒并检查是否有任何关闭的行组正准备要压缩成列存储行组。  元组搬运者还取消分配其中已删除每个行的行组。 解除分配的行组标记为已 **停** 用。 若要立即运行元组移动器，请使用 **ALTER INDEX** 语句的 "重新 **组织**" 选项。  
  
 如果列存储行组已填充，它将进行压缩并停止接受新行。 当从压缩组中删除行时，这些行将保留但标记为已删除。 对压缩组的更新将实现为压缩组中的删除以及对打开组的插入。  
  
## <a name="permissions"></a>权限  
 需要 VIEW SERVER STATE 权限  。  
  
## <a name="examples-sssdw-and-sspdw"></a>示例：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 下面的示例将 **sys.pdw_nodes_column_store_row_groups** 表联接到其他系统表，以返回有关特定表的信息。 计算的 `PercentFull` 列是对行组效率的估计。 若要查找单个表的信息，请删除 WHERE 子句前面的注释连字符，并提供表名称。  
  
```sql
SELECT IndexMap.object_id,   
  object_name(IndexMap.object_id) AS LogicalTableName,   
  i.name AS LogicalIndexName, IndexMap.index_id, NI.type_desc,   
  IndexMap.physical_name AS PhyIndexNameFromIMap,   
  CSRowGroups.*,  
  100*(ISNULL(deleted_rows,0))/total_rows AS PercentDeletedRows   
FROM sys.tables AS t  
JOIN sys.indexes AS i  
    ON t.object_id = i.object_id  
JOIN sys.pdw_index_mappings AS IndexMap  
    ON i.object_id = IndexMap.object_id  
    AND i.index_id = IndexMap.index_id  
JOIN sys.pdw_nodes_indexes AS NI  
    ON IndexMap.physical_name = NI.name  
    AND IndexMap.index_id = NI.index_id  
JOIN sys.pdw_nodes_column_store_row_groups AS CSRowGroups  
    ON CSRowGroups.object_id = NI.object_id   
    AND CSRowGroups.pdw_node_id = NI.pdw_node_id  
    AND CSRowGroups.distribution_id = NI.distribution_id
    AND CSRowGroups.index_id = NI.index_id      
WHERE total_rows > 0
--WHERE t.name = '<table_name>'   
ORDER BY object_name(i.object_id), i.name, IndexMap.physical_name, pdw_node_id;  
```  

下面的 [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] 示例对聚集列存储的每个分区的行以及打开、关闭或压缩的行组中的行数进行计数：  

```sql
SELECT
    s.name AS [Schema Name]
    ,t.name AS [Table Name]
    ,rg.partition_number AS [Partition Number]
    ,SUM(rg.total_rows) AS [Total Rows]
    ,SUM(CASE WHEN rg.State = 1 THEN rg.Total_rows Else 0 END) AS [Rows in OPEN Row Groups]
    ,SUM(CASE WHEN rg.State = 2 THEN rg.Total_Rows ELSE 0 END) AS [Rows in Closed Row Groups]
    ,SUM(CASE WHEN rg.State = 3 THEN rg.Total_Rows ELSE 0 END) AS [Rows in COMPRESSED Row Groups]
FROM sys.pdw_nodes_column_store_row_groups rg
  JOIN sys.pdw_nodes_tables pt
    ON rg.object_id = pt.object_id
    AND rg.pdw_node_id = pt.pdw_node_id
    AND pt.distribution_id = rg.distribution_id
  JOIN sys.pdw_table_mappings tm
    ON pt.name = tm.physical_name
  INNER JOIN sys.tables t
    ON tm.object_id = t.object_id
  INNER JOIN sys.schemas s
    ON t.schema_id = s.schema_id
GROUP BY s.name, t.name, rg.partition_number
ORDER BY 1, 2
```
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [&#40;Transact-sql&#41;创建列存储索引 ](../../t-sql/statements/create-columnstore-index-transact-sql.md)   
 [sys.pdw_nodes_column_store_segments &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-segments-transact-sql.md)   
 [sys.pdw_nodes_column_store_dictionaries &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-dictionaries-transact-sql.md)  
  
  
