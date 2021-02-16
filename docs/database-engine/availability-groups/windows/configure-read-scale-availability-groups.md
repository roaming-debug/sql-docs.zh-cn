---
title: 为可用性组配置读取缩放
description: 了解如何在 Windows 上配置 SQL Server Always On 可用性组用于读取扩展工作负载。
ms.custom: seodec18
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.date: 05/24/2018
ms.topic: how-to
ms.prod: sql
ms.technology: availability-groups
ms.openlocfilehash: 995d8d2238ef9ded767544423093c8352e02bb1c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343388"
---
# <a name="configure-read-scale-for-an-always-on-availability-group"></a>为 AlwaysOn 可用性组配置读取缩放

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

可在 Windows 上配置 SQL Server AlwaysOn 可用性组用于读取缩放工作负载。 可用性组有两类体系结构：
* 使用群集管理器提供改善的业务连续性并且可包含可读次要副本的高可用性体系结构。 若要创建此高可用性体系结构，请参阅[在 Windows 上创建和配置可用性组](creation-and-configuration-of-availability-groups-sql-server.md)。 
* 只支持读取缩放工作负载的体系机构。 

本文介绍如何在不使用群集管理器的情况下创建可用性组，用于读取缩放工作负载。 此体系结构仅提供读取缩放。 它不提供高可用性。

>[!NOTE]
>带 `CLUSTER_TYPE = NONE` 的可用性组可包括各种操作系统平台上托管的副本。 它无法支持高可用性。 对于 Linux 操作系统，请参阅[在 Linux 上配置 SQL Server 可用性组用于读取缩放](../../../linux/sql-server-linux-availability-group-configure-rs.md)。

[!INCLUDE [Create prerequisites](../../../includes/ss-availability-group-rs-prereq.md)]

## <a name="create-an-availability-group"></a>创建可用性组

创建可用性组。 设置 `CLUSTER_TYPE = NONE`。 此外，使用 `FAILOVER_MODE = NONE` 设置每个副本。 运行分析或报告工作负荷的客户端应用程序可直接连接到辅助数据库。 还可以创建一个只读路由列表。 与主要副本的连接将读取连接请求循环转发到路由列表中的每个次要副本。

以下 Transact-SQL 脚本创建名为 `ag1` 的可用性组。 该脚本使用 `SEEDING_MODE = AUTOMATIC` 配置可用性组副本。 此设置会导致 SQL Server 在数据库添加到可用性组后自动在每个辅助服务器上创建数据库。 

为环境更新以下脚本。 将 `<node1>` 和 `<node2>` 值替换为托管副本的 SQL Server 实例的名称。 使用为终结点设置的端口替换 `<5022>` 值。 在主 SQL Server 副本上运行以下 Transact-SQL 脚本：

```sql
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

### <a name="join-secondary-sql-server-instances-to-the-availability-group"></a>将辅助 SQL Server 实例加入可用性组

以下 Transact-SQL 脚本将服务器加入名为 `ag1` 的可用性组。 为环境更新脚本。 若要加入可用性组，请在每个辅助 SQL Server 副本上运行以下 Transact-SQL 脚本：

```sql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE);

ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create post](../../../includes/ss-availability-group-rs-postactivity.md)]

此可用性组不是高可用性配置。 如果需要高可用性，请遵循[为 Linux 上的 SQL Server 配置 AlwaysOn 可用性组](../../../linux/sql-server-linux-availability-group-configure-ha.md)或[在 Windows 上创建和配置可用性组](creation-and-configuration-of-availability-groups-sql-server.md)中的说明。

## <a name="connect-to-read-only-secondary-replicas"></a>连接到只读次要副本

可根据以下两种方法之一连接到只读次要副本：
* 应用程序可直接连接到托管次要副本的 SQL Server 实例并查询数据库。 有关详细信息，请参阅[可读次要副本](active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)。
* 应用程序还可以使用只读路由，这需要一个侦听器。 如果在不使用群集管理器的情况下部署读取缩放方案，你仍然可以创建一个侦听器，该侦听器指向当前主要副本的 IP 地址和与 SQL Server 所侦听端口不同的端口。 故障转移后，需要重新创建指向新的主 IP 地址的侦听器。 有关详细信息，请参阅[只读路由](listeners-client-connectivity-application-failover.md#ConnectToSecondary)。

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>故障转移读取缩放可用性组上的主要副本

[!INCLUDE[Force failover](../../../includes/ss-force-failover-read-scale-out.md)]

## <a name="next-steps"></a>后续步骤

* [配置分布式可用性组](./distributed-availability-groups.md)
* [了解有关可用性组的详细信息](overview-of-always-on-availability-groups-sql-server.md)
* [执行强制的手动故障转移](perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)