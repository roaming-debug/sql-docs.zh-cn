---
description: sys.stats (Transact-SQL)
title: sys. stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.stats
- stats_TSQL
- sys.stats_TSQL
- stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.stats catalog view
ms.assetid: 42605c80-126f-460a-befb-a0b7482fae6a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 552c4d27444f1316bd38ead558e356025d57bd1b
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465115"
---
# <a name="sysstats-transact-sql"></a>sys.stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的数据库中的表、索引和索引视图对应的每个统计信息对象都包含一行。 每个索引都将具有相同名称和 ID (**index_id**  =  **stats_id**) 中的相应统计信息行，但并不是每个统计信息行都有相应的索引。  
  
 目录视图 [sys.stats_columns](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md) 提供数据库中每一列的统计信息。 有关统计信息的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|这些统计信息所属对象的 ID。|  
|name|**sysname**|统计信息的名称。 在对象中是唯一的。|  
|**stats_id**|**int**|统计信息 ID。 在对象中是唯一的。<br /><br />如果统计信息对应于索引，则 *stats_id* 值与 [sys.databases](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)目录视图中的 *index_id* 值相同。|  
|**auto_created**|**bit**|指示统计信息是否由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动创建。<br /><br /> 0 = 统计信息不是由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动创建。<br /><br /> 1 = 统计信息由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动创建。|  
|**user_created**|**bit**|指示统计信息是否由用户创建。<br /><br /> 0 = 统计信息不是用户创建的。<br /><br /> 1 = 统计信息是用户创建的。|  
|**no_recompute**|**bit**|指示是否已用 **NORECOMPUTE** 选项创建统计信息。<br /><br /> 0 = 统计信息不是用 **NORECOMPUTE** 选项创建的。<br /><br /> 1 = 统计信息是通过 **NORECOMPUTE** 选项创建的。|  
|**has_filter**|**bit**|0 = 统计信息不具有筛选器并且针对所有行进行计算。<br /><br /> 1 = 统计信息具有筛选器并且仅针对满足筛选器定义的行进行计算。|  
|**filter_definition**|**nvarchar(max)**|包含在筛选统计信息中的行子集的表达式。<br /><br /> NULL = 非筛选的统计信息。|  
|**is_temporary**|**bit**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 指示统计信息是否是临时的。 临时统计信息支持 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 辅助数据库（支持只读访问）。<br /><br /> 0 = 统计信息不是临时的。<br /><br /> 1 = 统计信息是临时的。|  
|**is_incremental**|**bit**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 指示统计信息是否作为增量统计信息创建。<br /><br /> 0 = 统计信息不是增量统计信息。<br /><br /> 1 = 统计信息是增量统计信息。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
 下面的示例返回 `HumanResources.Employee` 表的所有统计信息和统计信息列。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT s.name AS statistics_name  
      ,c.name AS column_name  
      ,sc.stats_column_id  
FROM sys.stats AS s  
INNER JOIN sys.stats_columns AS sc   
    ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id  
INNER JOIN sys.columns AS c   
    ON sc.object_id = c.object_id AND c.column_id = sc.column_id  
WHERE s.object_id = OBJECT_ID('HumanResources.Employee');  
```  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [统计信息](../../relational-databases/statistics/statistics.md)    
 [sys.dm_db_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.dm_db_stats_histogram &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)   
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)
 

 
