---
title: 虚影清除进程指南 | Microsoft Docs
description: 了解虚影清除进程，这是一个后台进程，用于在 SQL Server 中删除页面中已标记为删除的记录。
ms.custom: ''
ms.date: 05/02/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- ghost cleanup
- ghost records
- ghost clean up process
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 05113205c693de928321880f1586996f81b76a66
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750357"
---
# <a name="ghost-cleanup-process-guide"></a>虚影清除进程指南

虚影清除进程是一个单线程后台进程，用于删除页面中已标记为删除的记录。 下文简要介绍了这一进程。

## <a name="ghost-records"></a>虚影记录

从索引页的叶级别删除的记录不会从页面中物理删除，而会将该记录标记为“待删除”或“虚影”。 这意味着该行保留在页面上，但行标题中的发生了一些变化，表明该行实际上是虚影。 这是为了在删除操作期间优化性能。 虚影对于行级锁定和快照隔离（此时需要维护旧版的行）都是必需的。

## <a name="ghost-record-cleanup-task"></a>虚影记录清除任务

标记为删除或虚影的记录由后台虚影清除进程清除。 此后台进程在删除事务提交后的某个时间运行，并从页面上物理删除虚影记录。 虚影清除进程会以一个时间间隔自动运行（SQL Server 2012+ 每 5 秒钟运行一次、SQL Server 2008/2008R2 每 10 秒钟运行一次），并检查是否有任何页面已标记有虚影记录。 如果找到，它将转到相应位置并删除标记为删除或虚影的记录，每次执行时最多处理 10 个页面。

记录被虚影化时，会将数据库标记为“具有虚影条目”，虚影清除进程只会扫描这些数据库。 删除所有虚影记录后，虚影清除进程也会将数据库标记为“没有虚影记录”，并在下次运行时跳过该数据库。 该进程还会跳过它无法加上共享锁的所有数据库，并在下次运行时再次尝试。

以下查询可以确定单个数据库中存在的虚影记录数。 

 ```sql
 SELECT sum(ghost_record_count) total_ghost_records, db_name(database_id) 
 FROM sys.dm_db_index_physical_stats (NULL, NULL, NULL, NULL, 'SAMPLED')
 group by database_id
 order by total_ghost_records desc
```

## <a name="disable-the-ghost-cleanup"></a>禁用虚影清除

在具有大量删除记录的高负载系统上，由于页面保留在缓冲池中并生成 IO，虚影清除进程可能会导致性能问题。 在这种情况下，可使用[跟踪标志 661](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 来禁用该进程。 但是，禁用该进程会对性能产生影响。

禁用虚影清除进程可能会导致数据库不必要地增大，并可能导致性能问题。 由于虚影清除进程会删除标记为“虚影”的记录，因此禁用该进程会将这些记录留在页面上，从而阻止 SQL Server 重用此空间。 这转而会强制 SQL Server 将数据添加到新页面，导致数据库文件膨胀，还可能导致[页面拆分](indexes/specify-fill-factor-for-an-index.md)。 在创建执行计划和执行扫描操作时，页面拆分会导致性能问题。 

一旦禁用虚影清除进程，需采取一些行动来删除虚影记录。 一种选择是执行索引重新生成，这将在页面上移动数据。 另一种选择是手动运行 [sp_clean_db_free_space](system-stored-procedures/sp-clean-db-free-space-transact-sql.md)（清除所有数据库数据文件）或 [sp_clean_db_file_free_space](system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md)（清除单个数据库数据文件），这将删除虚影记录。

 >[!warning]
 > 通常建议不要禁用虚影清除进程。 执行此操作前应在受控环境中进行彻底测试，然后才能在生产环境中永久实施。


## <a name="next-steps"></a>后续步骤  
[禁用虚影清除进程](https://support.microsoft.com/help/920093/tuning-options-for-sql-server-when-running-in-high-performance-workloa)
<br>[从单个数据库文件中删除虚影记录](system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md)
<br>[从所有数据库数据文件中删除虚影记录](system-stored-procedures/sp-clean-db-free-space-transact-sql.md)


