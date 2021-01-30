---
description: sys.system_views (Transact-SQL)
title: sys.system_views (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.system_views_TSQL
- system_views
- system_views_TSQL
- sys.system_views
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_views catalog view
ms.assetid: a526c410-e7b5-4075-8103-e1f3c6837c3c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e0dcaebcf38e4aa3b1cbee41d5528f8d10a991cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189275"
---
# <a name="syssystem_views-transact-sql"></a>sys.system_views (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]附带的每个系统视图都在表中对应一行。 所有系统视图都包含在名为 **sys** 或 **INFORMATION_SCHEMA** 的架构中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|\<inherited columns>||有关此视图所继承的列的列表，请参阅 [sys.databases &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)。|  
|**is_replicated**|**bit**|1 = 视图已复制。|  
|**has_replication_filter**|**bit**|1 = 视图具有复制筛选器。|  
|**has_opaque_metadata**|**bit**|1 = 为视图指定了 VIEW_METADATA 选项。 有关详细信息，请参阅 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)。|  
|**has_unchecked_assembly_data**|**bit**|1 = 表包含依赖于上次 ALTER ASSEMBLY 期间定义发生更改的程序集的持久化数据。 在下一次成功执行 DBCC CHECKDB 或 DBCC CHECKTABLE 后将重置为 0。|  
|**with_check_option**|**bit**|1 = 在视图定义中指定了 WITH CHECK OPTION。|  
|**is_date_correlation_view**|**bit**|1 = 系统自动创建视图，以存储 **日期时间** 列之间的相关信息。 通过将 DATE_CORRELATION_OPTIMIZATION 设置为 ON，启用创建此视图。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [DBCC CHECKDB &#40;Transact-sql&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)   
 [DBCC CHECKTABLE &#40;Transact-sql&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)   
 [ALTER ASSEMBLY (Transact-SQL)](../../t-sql/statements/alter-assembly-transact-sql.md)  
  
  
