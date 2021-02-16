---
title: 故障转移群集示例 - Linux 上的 SQL Server
description: Linux 上的 SQL Server 故障转移群集实例的概念包括群集层、实例数、IP 地址和名称以及共享存储。
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 1839442f20e2a48ed612e42e3c654d9e3255a477
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346401"
---
# <a name="failover-cluster-instances---sql-server-on-linux"></a>故障转移群集示例 - Linux 上的 SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍了与 Linux 上的 SQL Server 故障转移群集实例 (FCI) 相关的概念。 

要在 Linux 上创建 SQL Server FCI，请参阅[在 Linux 上配置 SQL Server FCI](sql-server-linux-shared-disk-cluster-configure.md)

## <a name="the-clustering-layer"></a>群集层

* 在 RHEL 中，群集层基于 Red Hat Enterprise Linux (RHEL) [HA 加载项](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf)。 

    > [!NOTE] 
    > 需拥有订阅才能访问 Red Hat HA 加载项和文档。 

* 在 SLES 中，群集层基于 SUSE Linux Enterprise [高可用性扩展 (HAE)](https://www.suse.com/products/highavailability)。

    有关群集配置、资源代理选项、管理、最佳实践和建议的详细信息，请参阅 [SUSE Linux Enterprise High Availability Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html)（SUSE Linux Enterprise 高可用性扩展 12 SP2）。

RHEL HA 加载项和 SUSE HAE 都是基于 [Pacemaker](https://clusterlabs.org/) 构建的。

如下图所示，存储将呈现给两个服务器。 群集组件（Corosync 和 Pacemaker）负责协调通信和资源管理。 其中一个服务器具有活动连接，可连接至存储资源以及 SQL Server。 当 Pacemaker 检测到故障时，群集组件会设法将资源移到另一个节点。  

![Red Hat Enterprise Linux 7 共享磁盘 SQL 群集](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 


> [!NOTE]
> 目前，SQL Server 在 Linux 上与 Pacemaker 集成不及在 Windows 上与 WSFC 集成的耦合性高。 在 SQL 内部，无法了解到群集是否存在，所有业务流程都是由外至内，并且 Pacemaker.将该服务作为一个独立实例控制。 此外，虚拟网络名称特定于 WSFC，Pacemaker 中无相同的等效项。 预计 @@servername 和 sys.servers 将返回节点名称，而群集 dmvs sys.dm_os_cluster_nodes 和 sys.dm_os_cluster_properties 将无记录。 为了使用指向字符串服务器名称的连接字符串，且不使用 IP，它们需要在其 DNS 服务器中注册用于创建使用选定服务器名称的虚拟 IP 资源的 IP（如以下部分所述）。

## <a name="number-of-instances-and-nodes"></a>实例数和节点数

与 Linux 上的 SQL Server 的一个关键区别在于，每个 Linux 服务器只能安装一个 SQL Server。 该安装称为实例。 这意味着，与 Windows Server 不同，每个 Windows Server 故障转移群集 (WSFC) 最多支持 25 个 FCI，基于 Linux 的 FCI 只有一个实例。 这个实例也是一个默认实例；Linux 上没有命名实例的概念。 

当涉及 Corosync 时，Pacemaker 群集最多只能有 16 个节点，因此单个 FCI 最多可以跨越 16 个服务器。 即使 Pacemaker 群集允许有 16 个节点，使用标准版 SQL Server 实现的 FCI 最多也只支持两个群集节点。

在 SQL Server FCI 中，SQL Server 实例在一个节点或另一个节点上处于活动状态。

## <a name="ip-address-and-name"></a>IP 地址和名称
在 Linux Pacemaker 群集上，每个 SQL Server FCI 都需要其自己唯一的 IP 地址和名称。 如果 FCI 配置跨越多个子网，则每个子网将需要一个 IP 地址。 唯一名称和 IP 地址用于访问 FCI，以便应用程序和最终用户无需知道 Pacemaker 群集的基础服务器。

DNS 中的 FCI 名称应与在 Pacemaker 群集中创建的 FCI 资源的名称相同。
名称和 IP 地址都必须在 DNS 中进行注册。

## <a name="shared-storage"></a>共享存储
所有 FCI，无论是在 Linux 上还是在 Windows Server 上，都需要某种形式的共享存储。 此存储将提供给可能托管 FCI 的所有服务器，但在任何给定时间，只有一台服务器可以将该存储用于 FCI。 Linux 下可用于共享存储的选项是：

- iSCSI
- 网络文件系统 (NFS)
- 在 Windows Server 下的服务器消息块 (SMB)，选项略有不同。 基于 Linux 的 FCI 目前不支持的一个选项是，使用 TempDB 节点的本地磁盘，这是 SQL Server 的临时工作区。

在跨越多个位置的配置中，存储在一个数据中心的内容必须与另一个数据中心同步。 如果发生故障转移，FCI 将能够联机并且存储也应能够联机。 实现这一目标需要一些外部存储复制方法，无论是通过基础存储硬件还是某些基于软件的实用程序。 

>[!NOTE]
>对于 SQL Server，基于 Linux 的部署（使用直接提供给服务器的磁盘）必须使用 XFS 或 EXT4 进行格式化。 目前不支持其他文件系统。 任何更改都将在此处反映出来。

对于不同的受支持方法，呈现共享存储的过程是相同的：

- 配置共享存储
- 将存储作为文件夹挂载到服务器，这些服务器将充当 FCI 的 Pacemaker 群集的节点
- 如果需要，请将 SQL Server 系统数据库移动到共享存储
- 测试 SQL Server 是否从连接到共享存储的每台服务器运行

与 Linux 上的 SQL Server 的一个主要区别是，虽然可以配置默认用户数据和日志文件位置，但系统数据库必须始终存在于 `/var/opt/mssql/data`。 在 Windows Server 上，可以移动包括 TempDB 在内的系统数据库。 这一事实涉及如何为 FCI 配置共享存储。

可以使用 `mssql-conf` 实用程序更改非系统数据库的默认路径。 有关如何更改默认值的信息，请参阅[更改默认数据或日志目录位置](sql-server-linux-configure-mssql-conf.md#datadir)。 还可以将 SQL Server 数据和事务存储在其他位置，即使不是默认位置，只要足够安全也可；该位置需要说明。

以下主题说明如何为基于 Linux 的 SQL Server FCI 配置支持的存储类型：

- [配置故障转移群集实例 - iSCSI - Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-configure-iscsi.md)
- [配置故障转移群集实例 - NFS - Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-configure-nfs.md)
- [配置故障转移群集实例 - SMB- Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-configure-smb.md)
