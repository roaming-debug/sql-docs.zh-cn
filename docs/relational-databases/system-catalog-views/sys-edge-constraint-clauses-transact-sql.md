---
description: 'sys.edge_constraint_clauses (Transact-sql) '
title: sys.edge_constraint_clauses (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/17/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.edge_constraint_clauses
- edge_constraint_clauses
- SQL Graph
- edge_constraints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.edge_constraint_clauses catalog view
ms.assetid: 0f782d2f-7126-46ab-85b7-bcba44862231
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 043378ceafd2e3fba906c38e421afd2e18cba7a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065062"
---
# <a name="sysedge_constraint_clauses-transact-sql"></a>sys.edge_constraint_clauses (Transact-sql) 
[!INCLUDE[sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

对于一个边缘约束，每个子句包含一行。
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|边缘约束的 object_id。|  
|**from_object_id**|**int**|FROM 节点表的 object_id。|  
|**to_object_id**|**int**|到节点表的 object_id。|  
|**clause_number**|**int**|内部生成的子句的整数索引。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
