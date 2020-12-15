---
description: 'sys.pdw_materialized_view_mappings (Transact-sql) '
title: sys.pdw_materialized_view_mappings (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: d62b0e25-3226-4f87-a10a-b3a0d9555e19
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 645d4d722cadfb25453dd51400c05a8ca67c3c71
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472898"
---
# <a name="syspdw_materialized_view_mappings-transact-sql"></a>sys.pdw_materialized_view_mappings (Transact-sql)   

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

通过 object_id 将具体化视图与内部对象名称进行绑定。

Physical_name 和 object_id 的列构成此目录视图的键。
  
|列名|数据类型|说明|  
|-----------------|---------------|-----------------|  
|physical_name |**nvarchar (36)**|具体化视图的物理名称。|  
|object_id  |**int**|具体化视图的对象 ID。 请参阅 [sys.databases (transact-sql) ](./sys-objects-transact-sql.md?view=azure-sqldw-latest)。| 

## <a name="permissions"></a>权限

要求拥有 VIEW DATABASE STATE 权限。
  
## <a name="see-also"></a>另请参阅

[利用具体化视图进行性能优化](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[CREATE MATERIALIZED VIEW AS SELECT &#40;Transact-SQL&#41;](../../t-sql/statements/create-materialized-view-as-select-transact-sql.md?view=azure-sqldw-latest)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/alter-materialized-view-transact-sql.md?view=azure-sqldw-latest)   
[EXPLAIN &#40;Transact-SQL&#41;](../../t-sql/queries/explain-transact-sql.md?view=azure-sqldw-latest)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](./sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](./sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest)   
[DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql.md?view=azure-sqldw-latest)   
[Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Azure Synapse Analytics 支持的系统视图](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[Azure Synapse Analytics 支持的 T-SQL 语句](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)