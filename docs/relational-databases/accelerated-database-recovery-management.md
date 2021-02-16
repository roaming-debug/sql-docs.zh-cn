---
description: 管理加速数据库恢复
title: 管理加速数据库恢复 | Microsoft Docs
ms.date: 02/02/2021
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- accelerated database recovery [SQL Server], recovery-only
- database recovery [SQL Server]
author: mashamsft
ms.author: mathoma
ms.reviewer: kfarlee
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 28ab5d23e4a2dc3168a9682424b3cb11a8e0b70e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062203"
---
# <a name="manage-accelerated-database-recovery"></a>管理加速数据库恢复

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver2019.md)]

## <a name="enabling-and-controlling-adr"></a>启用和控制 ADR

ADR 在 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 中默认处于禁用状态，并且可使用 DDL 语法进行控制：
```sql
ALTER DATABASE [DB] SET ACCELERATED_DATABASE_RECOVERY = {ON | OFF}
[(PERSISTENT_VERSION_STORE_FILEGROUP = { filegroup name }) ];

```

使用此语法控制该功能是启用还是禁用，并为永久版本存储 (PVS) 数据指定特定的文件组。 如果未指定文件组，则 PVS 将存储在 PRIMARY 文件组中。

## <a name="managing-the-persistent-version-store-filegroup"></a>管理永久版本存储文件组
ADR 功能的基础是使更改处于版本控制，不同版本的数据元素保存在 PVS 中。
查找 PVS 所在的位置时以及考虑如何管理 PVS 中数据的大小时，有一些注意事项。

### <a name="to-enable-adr-without-specifying-a-filegroup"></a>在不指定文件组的情况下启用 ADR

```sql
ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = ON;
GO
```

在这种情况下，如果未指定 PVS 文件组，则 `PRIMARY` 文件组将保存 PVS 数据。

### <a name="to-enable-adr-and-specify-that-the-pvs-should-be-stored-in-the-versionstorefg-filegroup"></a>启用 ADR 并指定 PVS 应存储在 [VersionStoreFG] 文件组中

在运行此脚本之前，请创建文件组。

```sql
ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = ON
(PERSISTENT_VERSION_STORE_FILEGROUP = [VersionStoreFG])
```

### <a name="to-disable-the-adr-feature"></a>禁用 ADR 功能

```sql
ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = OFF;
GO
```

即使在禁用 ADR 功能后，也会在永久版本存储中存储版本，系统仍需要这些版本来进行逻辑还原。

### <a name="change-the-location-of-the-pvs-to-a-different-filegroup"></a>将 PVS 的位置更改为其他文件组

出于各种原因，可能需要将 PVS 的位置移到另一个文件组。 例如，PVS 可能需要更多的空间或更快的存储。

更改 PVS 位置的过程分为三步。

1. 关闭 ADR 功能。

   ```sql
   ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = OFF;
   GO
   ```

2. 等待，直到可释放 PVS 中存储的所有版本

   为了能够打开具有永久版本存储新位置的 ADR，首先必须确保已从以前的 PVS 位置清除了所有版本信息。 若要强制执行清理操作，请运行以下命令：

   ```sql
   EXEC sys.sp_persistent_version_cleanup [database name]
   ```

   `sys.sp_persistent_version_cleanup` 存储过程是同步的，这意味着该过程将一直持续到当前 PVS 中的所有版本信息清除完成。  完成后，通过查询 DMV `sys.dm_persistent_version_store_stats` 并检查 `persistent_version_store_size_kb` 的值，可验证版本信息是否确实已删除。

   ```sql
   SELECT DB_Name(database_id), persistent_version_store_size_kb 
   FROM sys.dm_tran_persistent_version_store_stats where database_id = [MyDatabaseID]
   ```

   如果 persistent_version_store_size_kb 的值为 0，则可以重新启用 ADR 功能，将 PVS 配置为位于新文件组中。

1. 打开 ADR，为 PVS 指定新位置

   ```sql
   ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = ON
   (PERSISTENT_VERSION_STORE_FILEGROUP = [VersionStoreFG])
   ```

## <a name="troubleshooting"></a>疑难解答

> [!NOTE]
> 此部分也适用于 Azure SQL 数据库。

查询 `sys.dm_tran_persistent_version_store_stats`，检查 PVS 大小。

检查 `% of DB` 大小。 另请注意与典型大小的差异。

如果 PVS 明显比基线大，或者接近数据库大小的 50%，则认为 PVS 大。 

1. 基于事务 ID 查询 `sys.dm_tran_database_transactions`，检索 `oldest_active_transaction_id` 并检查此事务是否已长时间处于活动状态。

   活动事务会阻止清理 PVS 的操作。

2. 如果数据库属于可用性组，请检查 `secondary_low_water_mark`。 这与 `sys.dm_hadr_database_replica_states` 报告的 `low_water_mark_for_ghosts` 相同。 查询 `sys.dm_hadr_database_replica_states`，以查看其中一个副本是否包含此值，因为这也会阻止 PVS 清理操作。
3. 检查 `min_transaction_timestamp`（如果联机 PVS 被阻止，则为 `online_index_min_transaction_timestamp`），并根据对 `transaction_sequence_num` 列执行的 `sys.dm_tran_active_snapshot_database_transactions` 检查来查找包含阻止 PVS 清理的旧快照事务的会话。
4. 如果以上均不适用，则表示清理操作被中止事务控制。 检查 `aborted_version_cleaner_last_start_time` 和 `aborted_version_cleaner_last_end_time` 上次时间，查看中止的事务清理是否已完成。 中止事务清理完成后，`oldest_aborted_transaction_id` 应会移到更高的位置。
5. 如果中止事务最近未成功完成，请检查错误日志中是否存在报告 `VersionCleaner` 问题的消息。

使用下面的示例查询作为故障排除帮助：

```sql
SELECT pvss.persistent_version_store_size_kb / 1024. / 1024 AS persistent_version_store_size_gb,
       pvss.online_index_version_store_size_kb / 1024. / 1024 AS online_index_version_store_size_gb,
       pvss.current_aborted_transaction_count,
       pvss.aborted_version_cleaner_start_time,
       pvss.aborted_version_cleaner_end_time,
       dt.database_transaction_begin_time AS oldest_transaction_begin_time,
       asdt.session_id AS active_transaction_session_id,
       asdt.elapsed_time_seconds AS active_transaction_elapsed_time_seconds
FROM sys.dm_tran_persistent_version_store_stats AS pvss
LEFT JOIN sys.dm_tran_database_transactions AS dt
ON pvss.oldest_active_transaction_id = dt.transaction_id
   AND
   pvss.database_id = dt.database_id
LEFT JOIN sys.dm_tran_active_snapshot_database_transactions AS asdt
ON pvss.min_transaction_timestamp = asdt.transaction_sequence_num
   OR
   pvss.online_index_min_transaction_timestamp = asdt.transaction_sequence_num
WHERE pvss.database_id = DB_ID();
```
