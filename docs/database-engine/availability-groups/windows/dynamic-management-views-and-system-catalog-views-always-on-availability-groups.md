---
title: 可用性组的 DMV 和系统目录视图
description: 可帮助监视和诊断 Always On 可用性组运行状况的一系列动态管理视图和目录视图。
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
ms.assetid: 4320a4a4-6183-462b-8bda-e7424e7cb706
author: rothja
ms.author: jroth
ms.openlocfilehash: a2a69aa016ae258375c6095bbf38e58ddd75b981
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643579"
---
# <a name="dynamic-management-views-and-system-catalog-views-always-on-availability-groups"></a>动态管理视图和系统目录视图（Always On 可用性组）
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题介绍 Always On 动态管理视图 (DMV) 上的一些常见查询，可使用这些查询来监视可用性组并对其进行故障排除。  
  
> [!TIP]  
>  在 Always On 仪表板中，可通过右键单击相应的表头并选择想要显示或隐藏的 DMV，轻松配置 GUI 以显示可用性副本和可用性数据库的许多 DMV。  
  
 有关可用性组 DMV 的详细信息，请参阅 [Always On 可用性组动态管理视图和函数 &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)。 有关可用性组目录视图的详细信息，请参阅 [Always On 可用性组目录视图 &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)。  
  
## <a name="check-the-wsfc-cluster-node-configuration"></a>检查 WSFC 群集节点配置  
 以下 Transact-SQL (T-SQL) 查询会检索当前 Windows Server 故障转移群集 (WSFC) 群集中所有节点的状态。  
  
```sql  
use master  
go  
select * from sys.dm_hadr_cluster_members  
go  
```  
  
 此结果集会报告当前 WSFC 群集每个成员节点的状态。 如果仲裁定义为“节点和文件共享的大多数”，则甚至会报告文件共享  。 可以查看每个节点的状态，包括每个节点的投票权重（[number_of_quorum_votes](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md) 值）。  
  
## <a name="explore-the-cluster-network"></a>探索群集网络  
 以下查询会检索当前 WSFC 群集的网络配置。  
  
```sql  
select * from sys.dm_hadr_cluster_networks  
```  
  
 结果集为 WSFC 集群中的每个网络适配器包含一行。 例如，在每个节点上包含两个网络适配器的双节点群集中，此查询会返回四行。  
  
## <a name="explore-the-availability-groups"></a>探索可用性组  
 以下查询会检索有关可用性组的信息。  
  
```sql  
select primary_replica, primary_recovery_health_desc, synchronization_health_desc from sys.dm_hadr_availability_group_states  
go  
select * from sys.availability_groups  
go  
select * from sys.availability_groups_cluster  
go  
```  
  
 DMV [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)、[sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) 和 [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) 都可返回有关当前 WSFC 群集中可用性组的信息。 事实上，[sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) 和 [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) 似乎会返回相同的信息。  
  
 但是，[sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) 报告存储在 WSFC 群集中的可用性组元数据，而 [sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) 则报告缓存在 SQL Server 进程空间中的可用性组元数据。 此外，这两个 DMV 报告配置信息，而 [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md) 报告可用性组的当前运行状态。  
  
> [!IMPORTANT]  
>  此命名法沿用了记录可用性副本和可用性数据库的 DMV。  
  
## <a name="explore-the-availability-replicas"></a>探索可用性副本  
 以下查询会检索有关可用性组中定义的可用性副本的信息。  
  
```sql  
select replica_id, role_desc, connected_state_desc, synchronization_health_desc from sys.dm_hadr_availability_replica_states  
go  
select replica_server_name, replica_id, availability_mode_desc, endpoint_url from sys.availability_replicas  
go  
select replica_server_name, join_state_desc from sys.dm_hadr_availability_replica_cluster_states  
go  
```  
  
 与可用性组 DMV 类似，可发现三个报告可用性副本的 DMV。 [sys.dm_hadr_availability_replica_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md) 报告有关 SQL Server 中本地缓存的可用性副本的状态信息，而 [sys.dm_hadr_availability_replica_cluster_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md) 报告有关 WSFC 群集中可用性副本的状态信息。 最后，[sys.availability_replicas](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md) 报告 SQL Server 中本地缓存的可用性副本的配置数据。  
  
## <a name="explore-availability-replica-health"></a>探索可用性副本运行状况  
 以下查询会检索有关可用性副本当前运行状况的信息。  
  
```sql  
select replica_id, role_desc, recovery_health_desc, synchronization_health_desc from sys.dm_hadr_availability_replica_states  
go  
```  
  
 比较针对主要副本和次要副本的查询结果，并请注意，对于次要副本，仅会报告该副本的运行状况信息，而不会报告可用性组中任何其他副本的信息。  
  
## <a name="explore-the-availability-databases"></a>探索可用性数据库  
 以下查询会检索有关可用性组中定义的可用性副本的信息。 可观察挂起可用性数据库的数据移动前后，查询结果的更改。  
  
```sql
select * from sys.availability_databases_cluster  
go  
select group_database_id, database_name, is_failover_ready  from sys.dm_hadr_database_replica_cluster_states  
go  
select database_id, synchronization_state_desc, synchronization_health_desc, last_hardened_lsn, redo_queue_size, log_send_queue_size from sys.dm_hadr_database_replica_states  
go  
```  
  
 此处同样有三个有关可用性数据库的 Always On DMV 报告。 [sys.availability_databases_cluster](~/relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md) 报告有关 WSFC 群集中的可用性数据库的配置信息。 [sys.dm_hadr_database_replica_cluster_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md) 报告 SQL Server 中本地缓存的有关数据库副本的状态信息。 它包含一些重要状态信息，如可用性副本的故障转移就绪。 最后，[sys.dm_hadr_database_replica_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) 是非常详细的结果集，它报告每个可用性数据库上的标识和状态信息，如主要和次要数据库副本日志的 LSN 进度信息。  
  
## <a name="explore-availability-database-health"></a>探索可用性数据库运行状况  
 以下查询会检索有关副本上每个可用性数据库运行状况的信息。 可观察挂起可用性数据库的数据移动前后，查询结果的更改。  
  
```sql  
select dc.database_name, dr.database_id, dr.synchronization_state_desc,   
dr.suspend_reason_desc, dr.synchronization_health_desc  
from sys.dm_hadr_database_replica_states dr  join sys.availability_databases_cluster dc  
on dr.group_database_id=dc.group_database_id   
where is_local=1  
go  
```  
  
  
