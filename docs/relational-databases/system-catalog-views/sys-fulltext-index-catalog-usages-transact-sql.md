---
description: sys.fulltext_index_catalog_usages (Transact-SQL)
title: sys.fulltext_index_catalog_usages (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_index_catalog_usages
- sys.fulltext_index_catalog_usages_TSQL
- fulltext_index_catalog_usages
- fulltext_index_catalog_usages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fulltext_index_catalog_usages catalog view
ms.assetid: d095ab62-270b-484b-a541-9f9e7c951cf0
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a3c132904c985ddf2e46985668caa75838f3bb3c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191506"
---
# <a name="sysfulltext_index_catalog_usages-transact-sql"></a>sys.fulltext_index_catalog_usages (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  对于全文索引引用的每个全文目录，返回与其对应的一行。    
 
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|全文索引表的 ID。 在该数据库中是唯一的。|  
|index_id|**int**|全文索引的 ID。|  
|**fulltext_catalog_id**|**int**|全文目录的 ID。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [数据空间 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/data-spaces-transact-sql.md)  
  
  
