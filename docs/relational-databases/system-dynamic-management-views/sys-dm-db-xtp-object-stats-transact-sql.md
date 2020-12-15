---
description: sys.dm_db_xtp_object_stats (Transact-SQL)
title: sys.dm_db_xtp_object_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_object_stats_TSQL
- sys.dm_db_xtp_object_stats
- dm_db_xtp_object_stats
- sys.dm_db_xtp_object_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_object_stats dynamic management view
ms.assetid: 07300b59-3cab-4d3e-8138-5ea8f584f88f
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a082e789e05a5ca59afb9db74790a5cfb54232d7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468468"
---
# <a name="sysdm_db_xtp_object_stats-transact-sql"></a>sys.dm_db_xtp_object_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  报告自上次数据库重新启动以来对每个 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 对象进行的操作所影响的行数。 统计信息会在操作执行时更新（无论事务提交还是回滚）。  
  
 sys.dm_db_xtp_object_stats 可以帮助您标识更改最多的内存优化表。 您可以决定删除表中未使用或很少使用的索引，因为每个索引都会影响性能。 如果存在哈希索引，则您应定期重新计算桶计数。 有关详细信息，请参阅 [Determining the Correct Bucket Count for Hash Indexes](/previous-versions/sql/)。  
  
 sys.dm_db_xtp_object_stats 可以帮助您标识引发写/写冲突（这可能会影响应用程序性能）的内存优化表。 例如，如果您有事务重试逻辑，则相同语句可能需要执行多次。 您还可以使用此信息标识需要写/写错误处理的表（以及因此得到的业务逻辑）。  
  
 该视图为数据库中的每个内存优化表包含一行。  
  
 有关详细信息，请参阅[内存中 OLTP&#40;内存中优化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**bigint**|对象的 ID。|  
|row_insert_attempts|**bigint**|自上次数据库重新启动以来由已提交和中止的事务插入表中的行数。|  
|row_update_attempts|**bigint**|自上次数据库重新启动以来由已提交和中止的事务在表中更新的行数。|  
|row_delete_attempts|**bigint**|自上次数据库重新启动以来由已提交和中止的事务从表中删除的行数。|  
|write_conflicts|**bigint**|自上次数据库重新启动以来发生的写入冲突数。|  
|unique_constraint_violations|**bigint**|自上次数据库重新启动以来发生的唯一约束冲突数。|  
|object_address|**varbinary(8)**|仅限内部使用。|  
  
## <a name="permissions"></a>权限  
 要求对当前数据库拥有 VIEW DATABASE STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [内存优化表动态管理视图 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
