---
description: 'sys.dm_user_db_resource_governance (Transact-sql) '
title: sys.dm_user_db_resource_governance (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/17/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.resource_governance
- sys.resource_governance_TSQL
- resource_governance
- resource_governance_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governance catalog view
ms.assetid: ''
author: joesackmsft
ms.author: josack
monikerRange: =azuresqldb-current
ms.openlocfilehash: 8580ae3857b4c8c7f54599e70134879a076e89ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99131370"
---
# <a name="sysdm_user_db_resource_governance-transact-sql"></a>sys.dm_user_db_resource_governance (Transact-sql) 

[!INCLUDE[appliesto-xx-asdb-xxxx-xxx-md](../../includes/appliesto-xx-asdb-xxxx-xxx-md.md)]

返回当前数据库或弹性池中的资源调控机制所使用的实际配置和容量设置。
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|int|数据库的 ID，在 Azure SQL 数据库服务器中是唯一的。|
|**logical_database_guid**|uniqueidentifier|用户数据库的逻辑 GUID，用于保持用户数据库的使用。  重命名数据库或更改其服务级别目标将不会更改此值。|
|**physical_database_guid**|uniqueidentifier|用户数据库的物理 GUID，该 GUID 贯穿用户数据库的物理实例。 更改数据库服务级别目标将导致此值更改。|
|server_name|nvarchar|逻辑服务器名称。|
|**database_name**|nvarchar|逻辑数据库名称。|
|**slo_name**|nvarchar|服务级别目标，包括硬件生成。|
|**dtu_limit**|int|VCore) 的数据库的 DTU 限制 (NULL。|
|**cpu_limit**|int| (DTU 数据库) 的数据库 vCore 限制为 NULL。|
|**min_cpu**|tinyint|用户工作负荷资源池的 MIN_CPU_PERCENT 值。 请参阅 [资源池概念](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts)。|
|**max_cpu**|tinyint|用户工作负荷资源池的 MAX_CPU_PERCENT 值。 请参阅 [资源池概念](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts)。|
|**cap_cpu**|tinyint|用户工作负荷资源池的 CAP_CPU_PERCENT 值。 请参阅 [资源池概念](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts)。|
|**min_cores**|smallint|仅限内部使用。|
|**max_dop**|smallint|用户工作负荷组的 MAX_DOP 值。 请参阅 [创建工作负荷组](../../t-sql/statements/create-workload-group-transact-sql.md)。|
|**min_memory**|int|用户工作负荷资源池的 MIN_MEMORY_PERCENT 值。 请参阅 [资源池概念](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts)。|
|**max_memory**|int|用户工作负荷资源池的 MAX_MEMORY_PERCENT 值。 请参阅 [资源池概念](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts)。|
|**max_sessions**|int|用户工作负荷组中允许的最大会话数。|
|**max_memory_grant**|int|用户工作负荷组的 REQUEST_MAX_MEMORY_GRANT_PERCENT 值。 请参阅 [创建工作负荷组](../../t-sql/statements/create-workload-group-transact-sql.md)。|
|**max_db_memory**|int|仅限内部使用。|
|**govern_background_io**|bit|仅限内部使用。|
|**min_db_max_size_in_mb**|bigint|数据文件的最小 max_size 值（以 MB 为单位）。 请参阅 [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md)。|
|**max_db_max_size_in_mb**|bigint|数据文件的最大 max_size 值（以 MB 为单位）。 请参阅 [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md)。|
|**default_db_max_size_in_mb**|bigint|数据文件的默认 max_size 值（以 MB 为单位）。 请参阅 [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md)。|
|**db_file_growth_in_mb**|bigint|数据文件的默认增长增量（以 MB 为单位）。 请参阅 [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md)。|
|**initial_db_file_size_in_mb**|bigint|新数据文件的默认大小，以 MB 为单位。 请参阅 [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md)。|
|**log_size_in_mb**|bigint|新日志文件的默认大小，以 MB 为单位。 请参阅 [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md)。|
|**instance_cap_cpu**|int|仅限内部使用。|
|**instance_max_log_rate**|bigint|SQL Server 实例的日志生成速率限制（以字节/秒为单位）。 适用于由实例生成的所有日志，包括 `tempdb` 和其他系统数据库。 在弹性池中，适用于池中所有数据库生成的日志。|
|**instance_max_worker_threads**|int|SQL Server 实例的工作线程限制。|
|**replica_type**|int|副本类型，其中0是主要副本，1为辅助副本。|
|**max_transaction_size**|bigint|任何事务使用的最大日志空间，以 KB 为单位。|
|**checkpoint_rate_mbps**|int|仅限内部使用。|
|**checkpoint_rate_io**|int|仅限内部使用。|
|**last_updated_date_utc**|datetime|上次更改或重新配置的日期和时间（UTC）。|
|**primary_group_id**|int|主副本和辅助副本上的用户工作负荷的工作负荷组 ID。|
|**primary_group_max_workers**|int|用户工作负荷组的工作线程限制。|
|**primary_min_log_rate**|bigint|用户工作负荷组级别的最小日志速率（字节/秒）。 资源调控不会尝试将对数速率降低到此值以下。|
|**primary_max_log_rate**|bigint|用户工作负荷组级别的最大对数速率（字节/秒）。 资源调控不允许高于此值的日志速率。|
|**primary_group_min_io**|int|用户工作负荷组的最小 IOPS。 资源调控不会尝试降低低于此值的 IOPS。|
|**primary_group_max_io**|int|用户工作负荷组的最大 IOPS。 资源调控不允许高于此值的 IOPS。|
|**primary_group_min_cpu**|float|用户工作负荷组级别的最小 CPU 百分比。 资源调控不会尝试降低低于此值的 CPU 利用率。|
|**primary_group_max_cpu**|float|用户工作负荷组级别的最大 CPU 百分比。 资源调控不会允许 CPU 利用率高于此值。|
|**primary_log_commit_fee**|int|用户工作负荷组的日志费率调控提交费用（以字节为单位）。 提交费用会按固定值增加每个日志 IO 的大小，以仅用于日志速率记帐。 不会增加实际的日志 IO 到存储。|
|**primary_pool_max_workers**|int|用户工作负荷资源池的工作线程限制。|
|**pool_max_io**|int|用户工作负荷资源池的最大 IOPS 限制。|
|**govern_db_memory_in_resource_pool**|bit|仅限内部使用。|
|**volume_local_iops**|int|仅限内部使用。|
|**volume_managed_xstore_iops**|int|仅限内部使用。|
|**volume_external_xstore_iops**|int|仅限内部使用。|
|**volume_type_local_iops**|int|仅限内部使用。|
|**volume_type_managed_xstore_iops**|int|仅限内部使用。|
|**volume_type_external_xstore_iops**|int|仅限内部使用。|
|**volume_pfs_iops**|int|仅限内部使用。|
|**volume_type_pfs_iops**|int|仅限内部使用。|
|||

## <a name="permissions"></a>权限

此视图需要拥有 VIEW DATABASE STATE 权限。

## <a name="remarks"></a>备注

有关 Azure SQL 数据库中资源调控的说明，请参阅 [SQL 数据库资源限制](/azure/sql-database/sql-database-resource-limits-database-server)。

> [!IMPORTANT]
> 此 DMV 返回的大多数数据都适用于内部使用，随时可能更改。

## <a name="examples"></a>示例

以下查询在用户数据库的上下文中执行，返回用户工作负荷组和资源池级别的最大日志速率和最大 IOPS。 对于单一数据库，将返回一行。 对于弹性池中的数据库，将为池中的每个数据库返回一行。

```
SELECT database_name,
       primary_group_id,
       primary_max_log_rate,
       primary_group_max_io,
       pool_max_io
FROM sys.dm_user_db_resource_governance
ORDER BY database_name;  
```

## <a name="see-also"></a>另请参阅

- [资源调控器](../resource-governor/resource-governor.md)
- [sys.dm_resource_governor_resource_pools (Transact-SQL)](./sys-dm-resource-governor-resource-pools-transact-sql.md)
- [sys.dm_resource_governor_workload_groups (Transact-SQL)](./sys-dm-resource-governor-workload-groups-transact-sql.md)
- [sys.dm_resource_governor_resource_pools_history_ex (Transact-sql) ](./sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database.md)
- [sys.dm_resource_governor_workload_groups_history_ex（Azure SQL 数据库）](./sys-dm-resource-governor-workload-groups-history-ex-azure-sql-database.md)
- [事务日志速率调控](/azure/sql-database/sql-database-resource-limits-database-server#transaction-log-rate-governance)
- [单一数据库 DTU 资源限制](/azure/sql-database/sql-database-dtu-resource-limits-single-databases)
- [单一数据库 vCore 资源限制](/azure/sql-database/sql-database-vcore-resource-limits-single-databases)