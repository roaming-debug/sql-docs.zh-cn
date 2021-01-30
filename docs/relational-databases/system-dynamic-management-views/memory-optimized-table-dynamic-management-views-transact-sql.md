---
title: " (Transact-sql) 的内存优化表动态管理视图"
description: 了解 SQL Server 中用于 In-Memory OLTP 的 SQL Server 动态管理视图和对象目录视图。
ms.custom: seo-dt-2019
ms.date: 02/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: ccd82fed-1a3f-47de-85c4-1c9bdd80b027
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 50b110b71cdbfc1d9f6775c0b046ff4cbdf69db6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196311"
---
# <a name="memory-optimized-table-dynamic-management-views-transact-sql"></a>内存优化表动态管理视图 (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  以下 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 动态管理视图 (dmv) 用于 In-Memory OLTP：  
  
 有关详细信息，请参阅[内存中 OLTP&#40;内存中优化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。  

:::row:::
    :::column:::
        [sys.dm_db_xtp_checkpoint_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-stats-transact-sql.md)

        [sys.dm_db_xtp_gc_cycle_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-gc-cycle-stats-transact-sql.md)

        [sys.dm_db_xtp_index_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md)

        [sys.dm_db_xtp_merge_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-merge-requests-transact-sql.md)

        [sys.dm_db_xtp_nonclustered_index_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-nonclustered-index-stats-transact-sql.md)

        [sys.dm_db_xtp_transactions &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-transactions-transact-sql.md)

        [sys.dm_xtp_gc_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-gc-stats-transact-sql.md)

        [sys.dm_xtp_transaction_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-transaction-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_checkpoint_files &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md)

        [sys.dm_db_xtp_hash_index_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-hash-index-stats-transact-sql.md)

        [sys.dm_db_xtp_memory_consumers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-memory-consumers-transact-sql.md)

        [sys.dm_db_xtp_object_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-object-stats-transact-sql.md)

        [sys.dm_db_xtp_table_memory_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md)

        [sys.dm_xtp_gc_queue_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-gc-queue-stats-transact-sql.md)

        [sys.dm_xtp_system_memory_consumers &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-system-memory-consumers-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="object-catalog-views"></a>对象目录视图

以下对象目录视图专门用于 In-Memory OLTP。

:::row:::
    :::column:::
        [sys.hash_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-hash-indexes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.memory_optimized_tables_internal_attributes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-memory-optimized-tables-internal-attributes-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="internal-dmvs"></a>内部 Dmv

还有一些仅供内部使用的 Dmv，我们不提供直接的文档。 在内存优化表的区域中，未记录的 Dmv 包括以下各项：

- sys.dm_xtp_threads
- sys.dm_xtp_transaction_recent_rows

