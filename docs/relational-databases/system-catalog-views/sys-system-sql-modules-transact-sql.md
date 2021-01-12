---
description: sys.system_sql_modules (Transact-SQL)
title: sys.system_sql_modules (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- system_sql_modules_TSQL
- sys.system_sql_modules
- sys.system_sql_modules_TSQL
- system_sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_sql_modules catalog view
ms.assetid: ad3548bc-4780-4821-b962-b421d52daed9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d08a1f60ae706cf0327189ba596609e2640c9e59
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094481"
---
# <a name="syssystem_sql_modules-transact-sql"></a>sys.system_sql_modules (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为每个包含 SQL 语言定义模块的系统对象返回一行。 类型为 FN、IF、P、PC、TF 和 V 的系统对象具有关联的 SQL 模块。 若要标识包含对象，可以将此视图加入到 [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|该包含对象的对象标识号，在数据库中是唯一的。|  
|**definition**|**nvarchar(max)**|定义此模块的 SQL 文本。|  
|uses_ansi_nulls|**bit**|1 = 创建模块时 SET ANSI_NULLS 数据库选项的设置为 ON。<br /><br /> 始终返回1。|  
|**uses_quoted_identifier**|**bit**|1 = 创建模块时 SET QUOTED_IDENTIFIER 选项的设置为 ON。<br /><br /> 始终返回1。|  
|**is_schema_bound**|**bit**|0 = 创建模块时未使用 SCHEMABINDING 选项。<br /><br /> 始终返回 0。|  
|**uses_database_collation**|**bit**|0 = 模块不依赖于数据库的默认排序规则。<br /><br /> 始终返回 0。|  
|**is_recompiled**|**bit**|0 = 创建过程时未使用 WITH RECOMPILE 选项。<br /><br /> 始终返回 0。|  
|**null_on_null_input**|**bit**|0 = 创建的模块不对任意 NULL 输入生成 NULL 输出。<br /><br /> 始终返回 0。|  
|**execute_as_principal_id**|**int**|始终返回 NULL|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.all_sql_modules &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-all-sql-modules-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  
  
  
