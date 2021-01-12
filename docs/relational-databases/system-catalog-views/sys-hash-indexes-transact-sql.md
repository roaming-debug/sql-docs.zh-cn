---
description: sys.hash_indexes (Transact-SQL)
title: sys.hash_indexes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.hash_indexes_TSQL
- hash_indexes
- sys.hash_indexes
- hash_indexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.hash_indexes catalog view
ms.assetid: d9e230fb-d3ff-486f-86ef-44898f0a703e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b422523074806b6cac30fc56859006561fc0c828
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095523"
---
# <a name="syshash_indexes-transact-sql"></a>sys.hash_indexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  显示当前哈希索引和哈希索引属性。 仅 [内存中 OLTP &#40;In-Memory 优化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)支持哈希索引。  
  
 Sys.hash_indexes 视图包含与 sys. 索引视图相同的列和一个名为 **bucket_count** 的附加列。 有关 sys.hash_indexes 视图中的其他列的详细信息，请参阅[transact-sql&#41;&#40;sys.databases。 ](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||[&#40;transact-sql&#41;继承 sys.databases](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)中的列。|  
|**bucket_count**|**int**|哈希索引的哈希存储桶计数。<br /><br /> 有关 bucket_count 值的详细信息，包括设置值的准则，请参阅 [CREATE TABLE &#40;transact-sql&#41;](../../t-sql/statements/create-table-transact-sql.md)。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]. 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
  
```  
SELECT object_name([object_id]) AS 'table_name', [object_id],  
     [name] AS 'index_name', [type_desc], [bucket_count]   
FROM sys.hash_indexes   
WHERE OBJECT_NAME([object_id]) = 'T1';  
```  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
