---
description: 'sys.pdw_nodes_columns (Transact-sql) '
title: sys.pdw_nodes_columns (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 268c77b7-1d71-4197-a2ed-5e2b2b8fc260
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 5e785b80629043772474eaa31a2cae5f93c742e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186518"
---
# <a name="syspdw_nodes_columns-transact-sql"></a>sys.pdw_nodes_columns (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  显示用户定义表和用户定义视图的列。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|object_id|**int**|此列所属对象的 ID。||  
|name|**sysname**|列的名称。 对象中唯一的。||  
|column_id|**int**|列的 ID。 对象中唯一的。||  
|system_type_id|**tinyint**|列的系统类型的 ID。||  
|user_type_id|**int**|用户定义的列类型的 ID。||  
|max_length|**smallint**|列的最大长度（字节）。|对于不支持的列类型，包含-1 (无效) 。|  
|精准率|**tinyint**|如果基于数值，则为该列的精度；否则为 0。||  
|scale|**tinyint**|如果基于数值，则为列的小数位数；否则为 0。||  
|collation_name|**sysname**|如果基于字符，则为该列排序规则的名称；否则为 NULL。||  
|is_nullable|**bit**|1 = 列可为空。||  
|is_ansi_padded|**bit**|1 = 如果列为字符、二进制或变量类型，则该列使用 ANSI_PADDING ON 行为。|始终为 0。|  
|is_rowguidcol|**bit**|1 = 列为声明的 ROWGUIDCOL。|始终为 0。|  
|is_identity|**bit**|1 = 列具有标识值。|始终为 0。|  
|is_computed|**bit**|1 = 列为计算列。|始终为 0。|  
|is_filestream|**bit**|1 = 列为 FILESTREAM 列。|始终为 0。|  
|is_replicated|**bit**|1 = 列已复制。|始终为 0。|  
|is_non_sql_subscribed|**bit**|1 = 列具有非 SQL 订阅服务器。|始终为 0。|  
|is_merge_published|**bit**|1 = 列已合并发布。|始终为 0。|  
|is_dts_replicated|**bit**|1 = 使用 SSIS 复制列。|始终为 0。|  
|is_xml_document|**bit**|1 = 内容为完整的 XML 文档。|始终为 0。|  
|xml_collection_id|**int**|0 = 没有 XML 架构集合。|始终为 0。|  
|default_object_id|**int**|默认对象的 ID;0 = 无默认值。|始终为 0。|  
|rule_object_id|**int**|绑定到列的独立规则的 ID。 <br />0 = 无独立规则。|始终为 0。|  
|is_sparse|**bit**|1 = 列为稀疏列。|始终为 0。|  
|is_column_set|**bit**|1 = 列为列集。|始终为 0。|  
|pdw_node_id|**int**|节点的唯一标识符 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。|NOT NULL|  
  
## <a name="permissions"></a>权限  
 需要 CONTROL SERVER 权限。  
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [sys.all_columns &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)  
  
  
