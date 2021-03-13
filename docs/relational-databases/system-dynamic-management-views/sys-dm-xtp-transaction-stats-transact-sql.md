---
description: sys.dm_xtp_transaction_stats (Transact-SQL)
title: sys.dm_xtp_transaction_stats (Transact-SQL)
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_xtp_transaction_stats_TSQL
- dm_xtp_transaction_stats
- sys.dm_xtp_transaction_stats_TSQL
- sys.dm_xtp_transaction_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xtp_transaction_stats dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 37a98ee4a73e7bcc0ce06cef44d3a142e7fba772
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421859"
---
# <a name="sysdm_xtp_transaction_stats-transact-sql"></a>sys.dm_xtp_transaction_stats (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  报告自服务器启动以来有关运行的事务的统计信息。  
  
 有关详细信息，请参阅[内存中 OLTP&#40;内存中优化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|total_count|**bigint**|内存中 OLTP 数据库引擎中已运行的事务总数。|  
|read_only_count|**bigint**|只读事务数。|  
|total_aborts|**bigint**|通过用户或系统中止而中止的事务总数。|  
|system_aborts|**bigint**|系统启动的中止数。 例如，原因为写入冲突、验证失败或依赖关系失败。|  
|validation_failures|**bigint**|因验证失败造成的事务中止的次数。|  
|dependencies_taken|**bigint**|仅限内部使用。|  
|dependencies_failed|**bigint**|因所依赖的事务中止的事务中止次数。|  
|savepoint_create|**bigint**|创建的保存点的数量。 为每个原子块创建新的保存点。|  
|savepoint_rollbacks|**bigint**|回滚到前一个保存点的次数。|  
|savepoint_refreshes|**bigint**|仅限内部使用。|  
|log_bytes_written|**bigint**|写入内存中 OLTP 日志记录的总字节数。|  
|log_IO_count|**bigint**|需要日志 IO 的事务总数。 只考虑针对持久表的事务。|  
|phantom_scans_started|**bigint**|仅限内部使用。|  
|phatom_scans_retries|**bigint**|仅限内部使用。|  
|phantom_rows_touched|**bigint**|仅限内部使用。|  
|phantom_rows_expiring|**bigint**|仅限内部使用。|  
|phantom_rows_expired|**bigint**|仅限内部使用。|  
|phantom_rows_expired_removed|**bigint**|仅限内部使用。|  
|scans_started|**bigint**|仅限内部使用。|  
|scans_retried|**bigint**|仅限内部使用。|  
|rows_returned|**bigint**|仅限内部使用。|  
|rows_touched|**bigint**|仅限内部使用。|  
|rows_expiring|**bigint**|仅限内部使用。|  
|rows_expired|**bigint**|仅限内部使用。|  
|rows_expired_removed|**bigint**|仅限内部使用。|  
|rows_inserted|**bigint**|仅限内部使用。|  
|rows_updated|**bigint**|仅限内部使用。|  
|rows_deleted|**bigint**|仅限内部使用。|  
|write_conflicts|**bigint**|仅限内部使用。|  
|unique_constraint_violations|**bigint**|唯一约束冲突的总数。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [内存优化表动态管理视图 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
