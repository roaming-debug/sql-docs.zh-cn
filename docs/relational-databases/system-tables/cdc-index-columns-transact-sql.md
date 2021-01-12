---
description: cdc.index_columns (Transact-SQL)
title: cdc.index_columns (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cdc.index_columns_TSQL
- cdc.index_columns
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.index_columns
ms.assetid: 256ec8a5-3031-40a8-9fdb-99db42ea453d
author: cawrites
ms.author: chadam
ms.openlocfilehash: d52e7a8ff70bb8bea173f9bd86ab7aff0137a931
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093793"
---
# <a name="cdcindex_columns-transact-sql"></a>cdc.index_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为与更改表关联的每个索引列返回一行。 变更数据捕获使用这些索引列来唯一标识源表中的行。 默认情况下，将包括源表的主键列。 但是，如果在对源表启用变更数据捕获时指定了源表的唯一索引，则将改用该索引中的列。 如果启用净更改跟踪，则该源表需要主键或唯一索引。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.sp_cdc_enable_table ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)。  
  
 我们建议您不要直接查询系统表， 请改为执行 [sys.sp_cdc_help_change_data_capture](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md) 存储过程。  

  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|更改表的 ID。|  
|column_name|**sysname**|索引列的名称。|  
|**index_ordinal**|**tinyint**|索引中的列序号（从 1 开始）。|  
|column_id|**int**|源表中的列 ID。|  
  
## <a name="see-also"></a>另请参阅  
 [cdc.change_tables &#40;Transact-sql&#41;](../../relational-databases/system-tables/cdc-change-tables-transact-sql.md)  
  
  
