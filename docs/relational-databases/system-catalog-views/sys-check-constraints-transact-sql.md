---
description: sys.check_constraints (Transact-SQL)
title: sys.check_constraints (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/28/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.check_constraints
- sys.check_constraints_TSQL
- check_constraints_TSQL
- check_constraints
dev_langs:
- TSQL
helpviewer_keywords:
- sys.check_constraints catalog view
ms.assetid: 940ebc5e-44ba-4dae-8b29-da94f2d1d6c4
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 73b76b420c06b27b61fb1adeb469d09cf6899310
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477518"
---
# <a name="syscheck_constraints-transact-sql"></a>sys.check_constraints (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  对于作为检查约束的每个对象，都包含一个对应的行，其 **sys.databases** 为 "C"。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.objects>**||有关此视图所继承的列的列表，请参阅 [sys.databases &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)。|  
|**is_disabled**|**bit**|禁用 CHECK 约束。|  
|**is_not_for_replication**|**bit**|创建 CHECK 约束且使用 NOT FOR REPLICATION 选项。|  
|**is_not_trusted**|**bit**|系统未针对所有行验证 CHECK 约束。|  
|**parent_column_id**|**int**|0 表示表级 CHECK 约束。<br /><br /> 非零值表示这是针对具有指定 ID 值的列定义的列级 CHECK 约束。|  
|**definition**|**nvarchar(max)**|定义该 CHECK 约束的 SQL 表达式。|  
|**uses_database_collation**|**bit**|1 = 约束定义依赖数据库的默认排序规则进行正确计算；否则为 0。 这种依赖关系可防止更改数据库的默认排序规则。|  
|**is_system_named**|**bit**|1 = 名称由系统生成。<br /><br /> 0 = 名称由用户提供。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
