---
description: sys.index_columns (Transact-SQL)
title: sys.index_columns (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.index_columns
- sys.index_columns_TSQL
- index_columns
- index_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.index_columns catalog view
ms.assetid: 211471aa-558a-475c-9b94-5913c143ed12
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cc5ce224b8010a5d49eef7f1be2f872c2685bd7a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094578"
---
# <a name="sysindex_columns-transact-sql"></a>sys.index_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  对于作为 **sys.databases** 索引或无序表的一部分的每个列，每列包含一行 (堆) 。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|定义索引所依据的对象的 ID。|  
|index_id|**int**|定义了列的索引的 ID。|  
|**index_column_id**|**int**|索引列的 ID。 **index_column_id** 仅在 **index_id** 中是唯一的。|  
|column_id|**int**|**Object_id** 中的列的 ID。<br /><br /> 0 = 非聚集索引中的行标识符 (RID)。<br /><br /> **column_id** 仅在 **object_id** 中是唯一的。|  
|**key_ordinal**|**tinyint**|键列集内的序数（从 1 开始）。<br /><br /> 0 = 不是键列，或者是 XML 索引、列存储索引或空间索引。<br /><br /> 注意： XML 索引或空间索引不能是键，因为基础列不是可比较的，这意味着不能对其值进行排序。|  
|**partition_ordinal**|**tinyint**|分区列集内的序数（从 1 开始）。 聚集列存储索引可以具有最多 1 个分区列。<br /><br /> 0 = 非分区列。|  
|**is_descending_key**|**bit**|1 = 索引键列采用降序排序。<br /><br /> 0 = 索引键列的排序方向为升序，或者列是列存储或哈希索引的一部分。|  
|**is_included_column**|**bit**|1 = 列是使用 CREATE INDEX INCLUDE 子句添加到索引的非键列，或者列是列存储索引的一部分。<br /><br /> 0 = 列不是包含列。<br /><br /> 因为列是聚集键的一部分而隐式添加的列未在 **sys.index_columns** 中列出。<br /><br /> 由于是分区列而隐式添加的列作为 0 返回。| 
|**column_store_order_ordinal**</br> 适用于： Azure Synapse Analytics (预览版) |**tinyint**|排序的聚集列存储索引中顺序列组内基于 1 (的) 。|
  
## <a name="permissions"></a>权限

 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例

 下例返回表 `Production.BillOfMaterials` 的所有索引和索引列。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT i.name AS index_name  
    ,COL_NAME(ic.object_id,ic.column_id) AS column_name  
    ,ic.index_column_id  
    ,ic.key_ordinal  
,ic.is_included_column  
FROM sys.indexes AS i  
INNER JOIN sys.index_columns AS ic
    ON i.object_id = ic.object_id AND i.index_id = ic.index_id  
WHERE i.object_id = OBJECT_ID('Production.BillOfMaterials');  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
  
index_name                                                 column_name        index_column_id key_ordinal is_included_column  
---------------------------------------------------------- -----------------  --------------- ----------- -------------  
AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate ProductAssemblyID  1               1           0  
AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate ComponentID        2               2           0  
AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate StartDate          3               3           0  
PK_BillOfMaterials_BillOfMaterialsID                       BillOfMaterialsID  1               1           0  
IX_BillOfMaterials_UnitMeasureCode                         UnitMeasureCode    1               1           0  
  
(5 row(s) affected)  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
