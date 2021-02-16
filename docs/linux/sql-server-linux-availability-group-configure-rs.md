---
title: 配置读取扩展可用性组（Linux 上的 SQL Server）
description: 了解如何在 Linux 上配置 SQL Server AlwaysOn 可用性组 (AG) 用于“读取缩放”工作负载。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 01/09/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 5cad1c63817bf55231c065123b2456590dbfebb2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352741"
---
# <a name="configure-a-sql-server-availability-group-for-read-scale-on-linux"></a>在 Linux 上配置 SQL Server 可用性组用于读取缩放

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

可在 Linux 上配置 SQL Server AlwaysOn 可用性组 (AG) 用于读取缩放工作负载。 存在两种类型的 AG 体系结构。 高可用性 (HA) 体系结构利用群集管理器改善业务连续性。 此体系结构还可包括读取缩放副本。 若要创建高可用性体系结构，请参阅[配置 SQL Server Always On 可用性组以在 Linux 上实现高可用性](sql-server-linux-availability-group-configure-ha.md)。 其他体系结构只支持读取缩放工作负载。 本文介绍如何在不使用群集管理器的情况下创建 AG，用于读取缩放工作负载。 此体系结构仅提供读取缩放。 它不提供高可用性。

> [!NOTE]
> 设置有 `CLUSTER_TYPE = NONE` 的可用性组可包括不同操作系统平台上托管的副本。 它无法支持高可用性。 

[!INCLUDE [Create prerequisites](../includes/ss-linux-cluster-availability-group-create-prereq.md)]

## <a name="create-the-ag"></a>创建 AG

创建 AG。 设置 `CLUSTER_TYPE = NONE`。 此外，使用 `FAILOVER_MODE = MANUAL` 设置每个副本。 运行分析或报告工作负载的客户端应用程序可直接连接到辅助数据库。 还可以创建一个只读路由列表。 与主要副本的连接将读取连接请求循环转发到路由列表中的每个次要副本。

以下 Transact-SQL 脚本创建名为 `ag1` 的 AG。 脚本使用 `SEEDING_MODE = AUTOMATIC` 配置 AG 副本。 此设置会导致 SQL Server 在数据库添加到 AG 后自动在每个辅助服务器上创建数据库。 为环境更新以下脚本。 将 `<node1>` 和 `<node2>` 值替换为托管副本的 SQL Server 实例的名称。 使用为终结点设置的端口替换 `<5022>` 值。 在主 SQL Server 副本上运行以下 Transact-SQL 脚本：

```SQL
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = NONE)
    FOR REPLICA ON
        N'<node1>' WITH (
            ENDPOINT_URL = N'tcp://<node1>:<5022>',
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
                    SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            ),
        N'<node2>' WITH ( 
            ENDPOINT_URL = N'tcp://<node2>:<5022>', 
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
            SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            );
        
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### <a name="join-secondary-sql-servers-to-the-ag"></a>将辅助 SQL Server 加入 AG

以下 Transact-SQL 脚本将服务器加入名为 `ag1` 的 AG。 为环境更新脚本。 在每个辅助 SQL Server 副本上运行以下 Transact-SQL 脚本，从而加入 AG：

```SQL
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE);
         
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create post](../includes/ss-linux-cluster-availability-group-create-post.md)]

此 AG 不是高可用性配置。 如果需要高可用性，请遵循[为 Linux 上的 SQL Server 配置 AlwaysOn 可用性组](sql-server-linux-availability-group-configure-ha.md)中的说明。 具体而言，用 `CLUSTER_TYPE=WSFC`（在 Windows 中）或 `CLUSTER_TYPE=EXTERNAL`（在 Linux 中）创建 AG。 然后通过在 Windows 上使用 Windows Server 故障转移群集或在 Linux 上使用 Pacemaker 来集成一个群集管理器。

## <a name="connect-to-read-only-secondary-replicas"></a>连接到只读次要副本

连接只读次要副本有两种方法。 应用程序可直接连接到托管次要副本的 SQL Server 实例并查询数据库。 它们还可以使用只读路由，这需要一个侦听器。

* [可读次要副本](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)
* [只读路由](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md#ConnectToSecondary)

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>故障转移读取缩放可用性组上的主要副本

[!INCLUDE[Force failover](../includes/ss-force-failover-read-scale-out.md)]

## <a name="next-steps"></a>后续步骤

* [配置分布式可用性组](../database-engine/availability-groups/windows/distributed-availability-groups.md)
* [了解有关可用性组的详细信息](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
* [执行强制的手动故障转移](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)