---
title: 适用于 Linux 上的 SQL Server 的可用性组
description: 了解适用于 Linux 上的 SQL Server 的 Always On 可用性组的特征。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/17/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: e37742d4-541c-4d43-9ec7-a5f9b2c0e5d1
ms.openlocfilehash: 7573e1faa9d6cb4bbf75dc4e516da22344523bff
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172019"
---
# <a name="always-on-availability-groups-on-linux"></a>Linux 上的 Always On 可用性组

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍在安装了基于 Linux 的 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 情况下的 Always On 可用性组 (AG) 的特征。 还介绍了基于 Linux 和基于 Windows Server 故障转移群集 (WSFC) 的 AG 之间的差异。 请参阅[基于 Windows 的文档](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)以了解关于 AG 的基础知识，因为其在 Windows 上和 Linux 上的工作方式相同，但 WSFC 除外。

从较高角度来看，Linux 上的 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 下的可用性组和基于 WSFC 的实现中的可用性组相同。 也就是说，所有限制和功能都相同，但存在一些例外情况。 主要区别包括：

-   从 SQL Server 2017 CU16 开始，Microsoft 分布式事务处理协调器 (DTC) 受 Linux 支持。 但是，Linux 上的可用性组尚不支持 DTC。 如果应用程序需要使用分布式事务，并且需要 AG，请在 Windows 上部署 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]。
-   需要高可用性的基于 Linux 的部署使用 Pacemaker 进行群集，而不使用 WSFC。
-   与 Windows 上的大多数 AG 配置（工作组群集方案除外）不同，Pacemaker 从不需要 Active Directory 域服务 (AD DS)。
-   对于 Linux 和 Windows 来说，将 AG 从一个节点故障转移到另一个节点的方式不同。
-   某些设置（例如 `required_synchronized_secondaries_to_commit`）在 Linux 上只能通过 Pacemaker 进行更改，而基于 WSFC 的安装则使用 Transact-SQL。

## <a name="number-of-replicas-and-cluster-nodes"></a>副本和群集节点的数量

[!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)] 中的 AG 可拥有的副本总数为两个：一个主要副本以及一个仅能用作可用性目的的次要副本。 次要副本不能用于任何其他目的，例如可读查询。 [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)] 中的 AG 最多可拥有的副本总数为九个：一个主要副本，以及最多八个次要副本，并且最多可以对其中三个副本（包括主要副本）进行同步。 如果使用基础群集，涉及 Corosync 时，总共最多可具有 16 个节点。 对于 [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)]可用性组最多可以跨越 16 个节点的 9 个，而对于 [!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)]则最多可以跨越 2 个节点。

需要能自动故障转移到其他副本的双副本配置需要使用仅配置副本，如[仅配置副本和仲裁](#configuration-only-replica-and-quorum)中所述。 [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] Cumulative Update 1 (CU1) 中已引入仅配置副本，因此，此版本应该是为此配置部署的最低版本。

如果使用了 Pacemaker，则必须对其进行正确配置以保持正常运行。 也就是说，除了满足所有 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 要求（如仅配置副本）以外，还必须从 Pacemaker 的角度正确实现仲裁和 STONITH。

仅 [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)] 支持可读次要副本。

## <a name="cluster-type-and-failover-mode"></a>群集类型和故障转移模式

[!INCLUDE[sssql17-md](../includes/sssql17-md.md)] 中的新增内容是引入了 AG 的群集类型。 对于 Linux，有两个有效值：External 和 None。 若群集类型为 External，则表示将在 AG 下使用 Pacemaker。 若要使用 External 群集类型，则需要将故障转移模式也设置为 External（这同样是 [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] 中的新增内容）。 在使用 Pacemaker 时，支持自动故障转移，但与 WSFC 不同，故障转移模式会设置为 External，而不是自动。 与 WSFC 不同，AG 的 Pacemaker 部分是在配置 AG 之后创建的。

若群集类型为 None，则表示 AG 不会使用 Pacemaker，也没有这方面的要求。 即使在已配置 Pacemaker 的服务器上，如果为 AG 配置了 None 群集类型，Pacemaker 也不会看到或管理该 AG。 None 群集类型仅支持从主要副本到次要副本的手动故障转移。 采用 None 创建的 AG 主要针对读取横向扩展方案和升级。 虽然这可以在不需要自动故障转移的方案（如灾难恢复或本地可用性）中运行，但不建议这样做。 若不使用 Pacemaker，侦听器的使用也会更加复杂。

群集类型存储在 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 动态管理视图 (DMV) `sys.availability_groups` 的 `cluster_type` 和 `cluster_type_desc` 列中。

## <a name="required_synchronized_secondaries_to_commit"></a>required\_synchronized\_secondaries\_to\_commit

[!INCLUDE[sssql17-md](../includes/sssql17-md.md)] 中新增了一项由 AG 使用的设置，名为 `required_synchronized_secondaries_to_commit`。 该设置会告知 AG 必须与主要副本数量一致的次要副本数量。 这样可实现自动故障转移（仅当通过 External 群集类型与 Pacemaker 集成时）等功能，并可控制多方面的行为，例如正确数量的次要副本联机或脱机时的主要副本可用性。 若要详细了解其中的工作原理，请参阅[可用性组配置的高可用性和数据保护](sql-server-linux-availability-group-ha.md)。 `required_synchronized_secondaries_to_commit` 值是默认设置的，并由 Pacemaker/ [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 维护。 可以手动替代此值。

将 `required_synchronized_secondaries_to_commit` 和新序列号（存储在 `sys.availability_groups` 中）组合，可以对 Pacemaker 和 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 进行通知，例如通知其可能会发生自动故障转移。 在这种情况下，次要副本的序列号会与主要副本的相同，表示其已具备所有最新配置信息。

可以为 `required_synchronized_secondaries_to_commit` 设置三种值：0、1 或 2。 这些值控制着副本变得不可用时出现的行为。 这些数字对应的是必须与主要副本同步的次要副本数量。 在 Linux 环境中，行为如下所示：

-   0 - 次要副本不需要与主要副本同步。 但是，如果不同步次要副本，则不会进行自动故障转移。 
-   1 - 必须将一个次要副本与主要副本同步；可实现自动故障转移。 当同步的次要副本可用时，主数据库才可用。
-   2 - 必须将三个或更多节点的 AG 配置中的两个次要副本都与主要副本同步；可实现自动故障转移。

`required_synchronized_secondaries_to_commit` 不仅控制着使用同步副本的故障转移行为，还控制数据丢失。 若值为 1 或 2，则始终需要同步次要副本，因此始终都存在数据冗余。 这意味着不存在数据丢失。

若要更改 `required_synchronized_secondaries_to_commit` 的值，请使用以下语法：

>[!NOTE]
>更改此值会导致资源重启，也就是说会出现短暂的故障。 要避免这种情况，唯一的方法是暂时将资源设置为不由群集托管。

**Red Hat Enterprise Linux (RHEL) 和 Ubuntu**

```bash
sudo pcs resource update <AGResourceName> required_synchronized_secondaries_to_commit=<Value>
```

**SUSE Linux Enterprise Server (SLES)**

```bash
sudo crm resource param ms-<AGResourceName> set required_synchronized_secondaries_to_commit <value>
```

其中“AGResourceName”是为 AG 配置的资源名称，Value 为“0”、“1”或“2”   。 若要将其改回默认值，即由 Pacemaker 管理参数，请在不使用任何值的情况下执行同一语句。

当满足以下条件时，可以进行 AG 的自动故障转移：

-   主要副本和次要副本设置为同步数据移动。
-   次要副本的状态为“已同步”（而不是“正在同步”），表示这两个副本处于同一数据点。
-   群集类型设置为 External。 若群集类型为 None，则无法进行自动故障转移。
-   要成为主要副本的次要副本的 `sequence_number` 是最大序列号，也就是说，该次要副本的 `sequence_number` 与原始主要副本的序列号匹配。

如果满足这些条件，并且承载主要副本的服务器发生故障，AG 便会将所有权更改为同步副本。 可以通过 `required_synchronized_secondaries_to_commit` 进一步控制同步副本（总共可以有三个：一个主要副本和两个次要副本）的行为。 这对 Windows 和 Linux 上的 AG 均适用，但是两者的配置方式完全不同。 在 Linux 上，该值由群集在 AG 资源本身上自动配置。

## <a name="configuration-only-replica-and-quorum"></a>仅配置副本和仲裁

仅配置副本也是 [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] CU1 中的新增内容。 由于 Pacemaker 不同于 WSFC，尤其是涉及仲裁和需要使用 STONITH 时，只有两个节点的配置对 AG 不起作用。 对于 FCI，Pacemaker 提供的仲裁机制可以正常运行，因为所有 FCI 故障转移仲裁都发生在群集层上。 对于 AG，Linux 下的仲裁发生在 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 中，其中存储了所有元数据。 这时仅配置副本便可发挥作用。

如果没有任何其他内容，则需要第三个节点并至少需要一个同步副本。 仅配置副本将 AG 配置存储在 master 数据库中，与 AG 配置中的其他副本一样。 仅配置副本不会让用户数据库参与 AG。 配置数据从主要副本同步发送。 在自动或手动故障转移期间，都会使用此配置数据。

若要让 AG 维护仲裁，并启用群集类型为 External 的自动故障转移，则必须满足以下某一条件：

-   具有三个同步副本（仅 [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)]）；或者
-   具有两个副本（主要副本和次要副本）以及一个仅配置副本。

无论 AG 配置采用的群集类型是 External 还是 None，都可以进行手动故障转移。 虽然可以使用群集类型为 None 的 AG 配置仅配置副本，但不建议这样做，因为这会使部署工作复杂化。 对于这些配置，请手动修改 `required_synchronized_secondaries_to_commit` 的值，该值至少为 1，以便至少存在一个同步副本。

可以在 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 的任何版本（包括 [!INCLUDE[ssexpress-md](../includes/ssexpress-md.md)]）上托管仅配置副本。 这会将许可成本降至最低，并确保其适用于 [!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)] 中的 AG。 这意味着所需的第三个服务器只需满足 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 的最低规范，因为它不接收 AG 的用户事务流量。

使用仅配置副本时，该副本具有以下行为：

-   默认情况下，`required_synchronized_secondaries_to_commit` 设置为 0。 如有需要，可手动将其修改为 1。
-   如果主要副本出现故障并且 `required_synchronized_secondaries_to_commit` 为 0，次要副本将成为新的主要副本，并且可以对其进行读取和写入操作。 如果该值为 1，则将进行自动故障转移，但在另一个副本处于联机状态之前将不接受新的事务。
-   如果次要副本出现故障且 `required_synchronized_secondaries_to_commit` 为 0，那么主要副本仍然会接受事务，但如果此时主要副本出现故障，数据便会失去保护，并且手动和自动故障转移都无法进行，因为没有可用的次要副本。
-   如果仅配置副本出现故障，AG 将正常运行，但是无法进行自动故障转移。
-   如果同步次要副本和仅配置副本都出现故障，主要副本便无法接受事务，并且无法将故障转移到任何副本。

在 CU1 中，通过 `mssql-server-ha` 生成的 corosync.log 文件的日志记录中存在已知 bug。 如果次要副本因所需的可用副本数量而无法成为主要副本，当前消息便会显示“预计接收 1 个序列号，但是仅接收到 2 个。 处于联机状态的副本不足，无法安全升级本地副本。” 这些数字应该对调位置，应该显示为“预计接收到 2 个序列号，但是仅接收到 1个。 处于联机状态的副本不足，无法安全升级本地副本。” 

## <a name="multiple-availability-groups"></a>多个可用性组 

每个 Pacemaker 群集或一组服务器可创建多个 AG。 唯一的限制是系统资源。 AG 所有权由主机显示。 不同的 AG 可属于不同的节点；它们不需要全部都在同一节点上运行。

## <a name="drive-and-folder-location-for-databases"></a>数据库的驱动器和文件夹位置

与基于 Windows 的 AG 一样，参与 AG 的用户数据库的驱动器和文件夹结构应该相同。 例如，如果用户数据库位于服务器 A 上的 `/var/opt/mssql/userdata` 中，则服务器 B 上应该存在同样的文件夹。[基于 Windows 的可用性组和副本的互操作性](#interoperability-with-windows-based-availability-groups-and-replicas)部分中介绍了这一点的唯一一种例外情况。

## <a name="the-listener-under-linux"></a>Linux 下的侦听器

侦听器是 AG 的一项可选功能。 侦听器为所有连接（对主要副本的读取/写入和/或对次要副本的只读连接）提供单个入口点，这样应用程序和最终用户不需要了解哪个服务器在托管数据。 在 WSFC 中，这是网络名称资源和 IP 资源的组合，随后会在 AD DS（如果需要）以及 DNS 中注册。 结合 AG 资源本身，它提供了抽象。 有关侦听器的详细信息，请参阅[侦听器、客户端连接和应用程序故障转移](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)。

Linux 下的侦听器的配置方式不同，但其功能相同。 Pacemaker 中没有网络名称资源的概念，也没有在 AD DS 中创建的对象；Pacemaker 中仅创建了一个可在任何节点上运行的 IP 地址资源。 需要创建一个与 DNS 中的侦听器的 IP 资源关联的条目，并采用“易记名称”。 只有在承载该可用性组的主要副本的服务器上，侦听器的 IP 资源才处于活动状态。

如果使用了 Pacemaker 并创建了与侦听器关联的 IP 地址资源，则会出现短暂的故障，因为无论是自动还是手动故障转移，IP 地址都会在一台服务器上停止，并在另一台服务器上启动。 虽然这样通过单个名称和 IP 地址的组合提供抽象，但也不会掩盖此故障。 要处理该中断，应用程序必须能够通过某种功能检测到此情况并进行重新连接。

但是，DNS 名称和 IP 地址的组合仍不足以提供 WSFC 上的侦听器提供的所有功能，例如次要副本的只读路由。 在配置 AG 时，仍需要在 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 中配置“侦听器”。 可以在向导以及 Transact-SQL 语法中查看此项。 可以通过两种方式对其进行配置，以便达到与 Windows 上一样的运行效果：

-   对于群集类型为 External 的 AG，与在 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 中创建的“侦听器”关联的 IP 地址应该是在 Pacemaker 中创建的资源的 IP 地址。
-   对于群集类型为 None 的 AG，请使用与主要副本关联的 IP 地址。

与提供的 IP 地址关联的实例随后会成为协调器，用于处理诸如应用程序发出的只读路由请求之类的事件。

## <a name="interoperability-with-windows-based-availability-groups-and-replicas"></a>基于 Windows 的可用性组和副本的互操作性 

群集类型为 External 或属于 WSFC 的 AG 的副本不能跨平台。 无论 AG 为 [!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)] 还是 [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)] 都是如此。 即，对于使用基础群集的传统 AG 配置来说，不能是一个副本在 WSFC 上，而另一个副本在具有 Pacemaker 的 Linux 上。

群集类型为 None 的 AG 的副本可以跨越操作系统的界限，因此同一 AG 中可能同时存在基于 Linux 和 Windows 的副本。 此处展示了一个示例，其中的主要副本是基于 Windows 的，而次要副本基于其中一个 Linux 的分发版。

![混合 None](./media/sql-server-linux-availability-group-overview/image1.png)

分布式 AG 也可以跨越操作系统的界限。 基础 AG 按照其配置规则进行绑定，例如配置为 External 的只能用于 Linux，但它要加入的 AG 可以使用 WSFC 配置。 请考虑以下示例：

![混合分发式 AG](./media/sql-server-linux-availability-group-overview/image2.png)

<!-- Distributed AGs are also supported for upgrades from [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] to [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]. For more information on how to achieve this, see [the article "x"].

If using automatic seeding with a distributed availability group that crosses OSes, it can handle the differences in folder structure. How this works is described in [the documentation for automatic seeding].
-->
 
## <a name="next-steps"></a>后续步骤
[为 Linux 上的 SQL Server 配置可用性组](sql-server-linux-availability-group-configure-ha.md)

[为 Linux 上的 SQL Server 配置读取缩放可用性](sql-server-linux-availability-group-configure-rs.md)

[在 RHEL 上添加可用性组群集资源](sql-server-linux-availability-group-cluster-rhel.md)

[在 SLES 上添加可用性组群集资源](sql-server-linux-availability-group-cluster-sles.md)

[在 Ubuntu 上添加可用性组群集资源](sql-server-linux-availability-group-cluster-ubuntu.md)

[配置分布式可用性组](sql-server-linux-availability-group-cross-platform.md)

