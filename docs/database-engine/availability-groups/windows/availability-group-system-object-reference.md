---
title: 可用性组系统对象引用
description: 介绍使用 Always On 可用性组时可使用的各种系统对象的引用。
ms.custom: seo-lt-2019
ms.date: 04/03/2010
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: reference
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 99a600e29cf8b1f8bda8b093e9b51c572d86ad9b
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584659"
---
# <a name="always-on-availability-group-system-object-reference"></a>AlwaysOn 可用性组系统对象参考

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]
    
本主题是使用 AlwaysOn 可用性组时可使用的所有各种系统对象的参考页。 

## <a name="system-catalog-views"></a>系统目录视图

| 系统目录视图 | 说明|
| :------ | :----------------------------- |
| [监视可用性数据库](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)   | 为 SQL Server 实例上的每个可用性数据库都包含一行，实例在托管 Windows Server 故障转移群集 (WSFC) 中任何 AlwaysOn 可用性组的可用性副本，无论本地副本数据库是否已加入可用性组。 |
| [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  | 为 Windows Server 故障转移群集 (WSFC) 中任何 AlwaysOn 可用性组侦听程序的每个关联 IP 地址都返回一行。 |
| [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)    | 对于每个 AlwaysOn 可用性组，返回零行（指明没有与可用性组关联的网络名称），或为 Windows Server 故障转移群集 (WSFC) 中的每个可用性组侦听程序配置都返回一行。  |
| [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   | 为 SQL Server 本地实例为其托管可用性副本的每个可用性组都返回一行。 每一行都包含可用性组元数据的缓存的副本。 |
| [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)    | 为 Windows Server 故障转移群集 (WSFC) 中的每个 AlwaysOn 可用性组都返回一行。 每一行都包含 WSFC 群集的可用性组元数据。 |
| [sys.availability_read_only_routing_lists](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)    | 为 WSFC 故障转移群集中 AlwaysOn 可用性组的每个可用性副本的只读路由列表返回一行。 |
| [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)    | 为属于 WSFC 故障转移群集中任何 AlwaysOn 可用性组的每个可用性副本都返回一行。 |
| &nbsp; | &nbsp; |

## <a name="system-dynamic-management-views"></a>系统动态管理视图


| 系统动态管理视图 | 说明|
| :------ | :----------------------------- |
| [sys.dm_hadr_auto_page_repair](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)   | 为针对任何可用性数据库（位于服务器实例为任何可用性组承载的可用性副本上）的每一个自动页修复尝试都返回一行。  |
| [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)    | 为在 SQL Server 的本地实例上拥有可用性副本的每个 AlwaysOn 可用性组都返回一行。 每行显示定义给定可用性组的运行状况的状态。 |
| [sys.dm_hadr_availability_replica_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)     | 为 Windows Server 故障转移群集 (WSFC) 中 AlwaysOn 可用性组的每个可用性副本（无论加入状态如何）都返回一行 |
| [sys.dm_hadr_availability_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)     | 为 Windows Server 故障转移群集 (WSFC) 群集中所有 AlwaysOn 可用性组（无论副本位置如何）的每个 AlwaysOn 可用性副本（无论加入状态如何）都返回一行。 |
| [sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)    | 为每个本地副本都返回一行，并为与本地副本位于同一 AlwaysOn 可用性组的每个远程副本都返回一行。 每一行都包含给定副本的状态信息。 |
| [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)    | 返回一行，其中公开与仲裁有关的群集名称和信息 |
| [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)    | 为构成仲裁的每个成员及其状态都返回一行 |
| [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)    | 为每个参与可用性组子网配置的 WSFC 群集成员都返回一行。  |
| [sys.availability_databases_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)     | 返回一行信息，这些信息可便于洞察 Windows Server 故障转移群集 (WSFC) 上每个 AlwaysOn 可用性组中的可用性数据库的运行状况。  |
| [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)    | 为参与 AlwaysOn 可用性组（SQL Server 的本地实例在托管其可用性副本）的每个数据库都返回一行。 |
| [sys.dm_hadr_instance_node_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)    | 对于托管加入 AlwaysOn 可用性组的可用性副本的每个 SQL Server 实例，返回托管服务器实例的 Windows Server 故障转移群集 (WSFC) 节点的名称。 |
| [sys.dm_hadr_name_id_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)    | 显示 AlwaysOn 可用性组的映射，即当前的 SQL Server 实例已联接到三个唯一 ID：一个可用性组 ID、一个 WSFC 资源 ID 和一个 WSFC 组 ID。 |
| [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)    | 返回包含各个 TCP 侦听器的动态信息的行。 |
| &nbsp; | &nbsp; |

## <a name="system-functions"></a>系统函数


| 系统函数 | 说明|
| :------ | :----------------------------- |
| [sys.fn_hadr_is_primary_replica](../../../relational-databases/system-functions/sys-fn-hadr-is-primary-replica-transact-sql.md)  | 用于确定当前副本是否为主副本。 |
| [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)    | 用于确定当前副本是否为首选备份副本。 |
| [sys.fn_hadr_distributed_ag_replica](../../../relational-databases/system-functions/sys-fn-hadr-distributed-ag-replica-transact-sql.md)    | 用于将分布式可用性组中的副本映射到本地可用性组。 |
| &nbsp; | &nbsp; |


  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   

  
