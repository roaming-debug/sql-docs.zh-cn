---
description: sys.computed_columns (Transact-SQL)
title: sys.computed_columns (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.computed_columns_TSQL
- sys.computed_columns
- computed_columns_TSQL
- computed_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.computed_columns catalog view
ms.assetid: c962c619-e18f-4315-9251-8d9862462299
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 05eec2fbe78d63cbfdc57d9c93c34b2874b33084
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104739577"
---
# <a name="syscomputed_columns-transact-sql"></a>sys.computed_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  对于作为计算列的 **sys.databases** 列中的每一列都包含一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**\<Inherited columns>**||**Sys.computed_columns** 视图返回 **sys.databases** 视图中的所有列。 它还返回如下所述的其他列。 有关 **sys.computed_columns** 视图继承自 **sys.databases** 的列的说明，请参阅 [&#40;transact-sql&#41;的 sys.databases。](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) 在 **sys.computed_columns** 视图中， **is_computed** 列的值始终设置为1。|  
|**definition**|**nvarchar(max)**|定义该计算列的 SQL 文本。|  
|**uses_database_collation**|**bit**|1 = 列定义依赖数据库的默认排序规则进行正确计算；否则为 0。 这种依赖关系可防止更改数据库的默认排序规则。|  
|**is_persisted**|**bit**|计算列是持久化的。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
