---
title: RHEL：为 Linux 中的 SQL Server 配置可用性组
description: 了解如何在运行 Red Hat Enterprise Linux (RHEL) 时为 SQL Server 配置可用性组。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 01/23/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: b7102919-878b-4c08-a8c3-8500b7b42397
ms.openlocfilehash: 2790bbe9eb648a2d599d1b0bad8f514ab72eda50
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338384"
---
# <a name="configure-rhel-cluster-for-sql-server-availability-group"></a>为 SQL Server 可用性组配置 RHEL 群集

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文档介绍如何在 Red Hat Enterprise Linux 上为 SQL Server 创建三节点可用性组群集。 若要实现高可用性，Linux 上的可用性组需要三个节点，请参阅[可用性组配置的高可用性和数据保护](sql-server-linux-availability-group-ha.md)。 集群层基于在 [Pacemaker](https://clusterlabs.org/) 的基础上构建的 Red Hat Enterprise Linux (RHEL) [HA 加载项](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf)。 

> [!NOTE] 
> 访问 Red Hat 完整文档需要有效订阅。 

有关群集配置、资源代理选项和管理的详细信息，请访问 [RHEL 参考文档](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html)。

> [!NOTE] 
> SQL Server 与 Linux 上的 Pacemaker 没有像它与 Windows Server 故障转移群集那样紧密集成。 SQL Server 实例不能识别群集。 Pacemaker 提供群集资源业务流程。 此外，虚拟网络名称特定于 Windows Server 故障转移群集 - Pacemaker 中没有等效项。 查询集群信息的可用性组动态管理视图 (DMV) 在 Pacemaker 集群上返回空行。 要在故障转移后创建透明重新连接的侦听器，请使用用于创建虚拟 IP 资源的 IP 在 DNS 中手动注册侦听器名称。 

以下各节介绍了设置 Pacemaker 集群并在集群中添加可用性组作为资源以实现高可用性的步骤。

## <a name="roadmap"></a>路线图

在 Linux 服务器上创建可用性组以获得高可用性的步骤与 Windows Server 故障转移群集上的步骤不同。 下面的列表对高级步骤进行了说明： 

1. [在群集节点上配置 SQL Server](sql-server-linux-setup.md)。

2. [创建可用性组](sql-server-linux-availability-group-configure-ha.md)。 

3. 配置 Pacemaker 等群集资源管理器。 本文档中包含这些说明。
   
   配置群集资源管理器的方式取决于特定的 Linux 分发版。 

   >[!IMPORTANT]
   >生产环境需要 STONITH 等隔离代理，以实现高可用性。 本文档中的演示不使用隔离代理。 演示仅用于测试和验证目的。
   
   >Linux 群集使用隔离将群集返回到某个已知状态。 配置隔离的方式取决于分发版和环境。 目前，在某些云环境中无法使用隔离。 有关详细信息，请参阅 [RHEL 高可用性群集的支持策略 - 虚拟化平台](https://access.redhat.com/articles/29440)。

4. [将可用性组添加为群集中的资源](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource)。  

## <a name="configure-high-availability-for-rhel"></a>为 RHEL 配置高可用性

若要为 RHEL 配置高可用性，请启用高可用性订阅，然后配置 Pacemaker。

### <a name="enable-the-high-availability-subscription-for-rhel"></a>为 RHEL 启用高可用性订阅

群集中的每个节点都必须具有适当的 RHEL 订阅和高可用性添加项。 在[如何在 Red Hat Enterprise Linux 中安装高可用性群集包](https://access.redhat.com/solutions/45930)中查看要求。 请按照以下步骤配置订阅和存储库：

1. 注册系统。

   ```bash
   sudo subscription-manager register
   ```

   提供用户名和密码。   

1. 列出可用的注册池。

   ```bash
   sudo subscription-manager list --available
   ```

   从可用池列表中，请注意高可用性订阅的池 ID。

1. 更新以下脚本。 使用池 ID 替换 `<pool id>` 以获得上一步中的高可用性。 运行脚本以附加订阅。

   ```bash
   sudo subscription-manager attach --pool=<pool id>
   ```

1. 启用存储库。

   **RHEL 7**

   ```bash
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-rpms
   ```

   **RHEL 8**

   ```bash
   sudo subscription-manager repos --enable=rhel-8-for-x86_64-highavailability-rpms
   ```

有关详细信息，请参阅 [Pacemaker - 开源、高可用性群集](https://clusterlabs.org/pacemaker/)。 

配置订阅后，请完成以下步骤以配置 Pacemaker：

### <a name="configure-pacemaker"></a>配置 Pacemaker

注册订阅后，请完成以下步骤以配置 Pacemaker：
   
[!INCLUDE [RHEL-Configure-Pacemaker](../includes/ss-linux-cluster-pacemaker-configure-rhel.md)]

配置 Pacemaker 后，请使用 `pcs` 与群集进行交互。 在群集中的一个节点上执行所有命令。 

## <a name="configure-fencing-stonith"></a>配置隔离 (STONITH)

Pacemaker 群集供应商需要启用 STONITH，并为受支持的群集安装程序配置隔离设备。 STONITH 表示“shoot the other node in the head”（将其他节点爆头）。 群集资源管理器无法确定节点的状态或节点上资源的状态时，通过隔离使群集再次处于已知状态。

STONITH 设备提供隔离代理。 有关如何在 Azure 中为此群集创建 STONITH 设备的示例，请参阅[在 Azure 中的 Red Hat Enterprise Linux 上设置 Pacemaker](/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker/#1-create-the-stonith-devices)。 修改环境的说明。

资源级别隔离确保在因配置资源引起服务中断时，不会发生数据损坏。 例如，可以使用资源级别隔离在通信链接出现故障时将节点上的磁盘标记为过时。 

节点级别隔离确保节点不会运行任何资源。 这是通过重置节点来完成的。 Pacemaker 支持各种隔离设备。 示例包括不间断电源或服务器的管理接口卡。

有关 STONITH 和隔离的信息，请参阅以下文章：

* [Pacemaker 从头开始群集](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/index.html)
* [隔离和 STONITH](https://clusterlabs.org/doc/crm_fencing.html)
* [Pacemaker 的 Red Hat 高可用性附加项：隔离](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/ch-fencing-HAAR.html)

>[!NOTE]
>由于节点级别隔离配置在很大程度上取决于你的环境，因此请在本教程中禁用它（稍后可以对其进行配置）。 以下脚本禁用节点级别隔离：
>
>```bash
>sudo pcs property set stonith-enabled=false
>``` 
>
>禁用 STONITH 仅出于测试目的。 如果计划在生产环境中使用 Pacemaker，则应根据环境来计划 STONITH 实现，并使其处于启用状态。

## <a name="set-cluster-property-cluster-recheck-interval"></a>设置群集属性 cluster-recheck-interval

`cluster-recheck-interval` 表示群集检查资源参数、约束或其他群集选项中的更改的轮询间隔。 如果副本发生故障，群集将尝试按 `failure-timeout` 值和 `cluster-recheck-interval` 值所绑定的间隔重启副本。 例如，如果 `failure-timeout` 设置为 60 秒且 `cluster-recheck-interval` 设置为 120 秒，则会在 60 秒至 120 秒的间隔内尝试重启。 建议将 failure-timeout 设置为 60 秒，将 cluster-recheck-interval 设置为大于 60 秒的值。 建议不要将 cluster-recheck-interval 设置为较小的值。

若要将属性值更新为 `2 minutes`，请运行：

```bash
sudo pcs property set cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> 当 start-failure-is-fatal 群集设置的值为 false 时，所有使用最新的可用 Pacemaker 包 1.1.18-11.el7 的分发版（包括 RHEL 7.3和 7.4）都会为该设置引入行为更改。 此更改会影响故障转移工作流。 如果主要副本发生中断，群集本应将故障转移到其中某个可用的次要副本。 然而，用户会注意到群集一直尝试启动发生故障的主要副本。 如果该主要副本由于永久性中断而永远不联机，群集就永远不会将故障转移到另一个可用的次要副本。 由于此更改，先前建议使用的用于设置 start-failure-is-fatal 的配置不再有效，并且必须将该设置还原为其默认值 `true`。 此外，必须更新 AG 资源以包含 `failover-timeout` 属性。 

若要将属性值更新为 `true`，请运行：

```bash
sudo pcs property set start-failure-is-fatal=true
```

若要将 `ag_cluster` 资源属性 `failure-timeout` 更新为 `60s`，请运行：

```bash
pcs resource update ag_cluster meta failure-timeout=60s
```


有关 Pacemaker 群集属性的信息，请参阅 [ Pacemaker 群集属性](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/ch-clusteropts-HAAR.html)。

## <a name="create-a-sql-server-login-for-pacemaker"></a>为 Pacemaker 创建 SQL Server 登录名

[!INCLUDE [SQL-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="create-availability-group-resource"></a>创建可用性组资源

若要创建可用性组资源，请使用 `pcs resource create` 命令并设置资源属性。 以下命令将为可用性组创建一个名为 `ag1` 的 `ocf:mssql:ag` master/subordinate 类型的资源。

**RHEL 7**

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=60s master notify=true
```

**RHEL 8**

随着 RHEL 8 可用，create 语法已发生了更改。 如果使用的是 RHEL 8，术语 `master` 已更改为`promotable`。 使用以下 create 命令，而不是上面的命令： 

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=60s promotable notify=true
```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

<a name="createIP"></a>

## <a name="create-virtual-ip-resource"></a>创建虚拟 IP 资源

若要创建虚拟 IP 地址资源，请在一个节点上运行以下命令。 使用网络中的可用静态 IP 地址。 将 `<10.128.16.240>` 之间的 IP 地址替换为一个有效的 IP 地址。

```bash
sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<10.128.16.240>
```

Pacemaker 中不存在等效的虚拟服务器名称。 若要使用指向字符串服务器名称而不是 IP 地址的连接字符串，请在 DNS 中注册虚拟 IP 资源地址和所需的虚拟服务器名称。 对于 DR 配置，请在主站点和 DR 站点上使用 DNS 服务器注册所需的虚拟服务器名称和 IP 地址。

## <a name="add-colocation-constraint"></a>加主机托管约束

Pacemaker 群集中的几乎所有决策（例如，选择资源运行的位置）都通过比较分数来完成。 分数按资源计算。 群集资源管理器选择特定资源分数最高的节点。 如果节点的某一资源分数为负，则该资源无法在此节点上运行。

在 pacemaker 群集上，可以使用约束来控制群集的决定。 约束具有一个分数。 如果约束的分数低于 `INFINITY`，则 Pacemaker 将其视为建议项。 `INFINITY` 的分数是必需项。

要确保主副本和虚拟 IP 资源在同一主机上运行，请定义分数为 INFINITY 的主机托管约束。 若要添加主机托管约束，请在一个节点上运行以下命令。

### <a name="rhel-7"></a>RHEL 7

在 RHEL 7 中创建 `ag_cluster` 资源时，它会将资源创建为 `ag_cluster-master`。 对于 RHEL 7，请使用以下命令：

```bash
sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
```

### <a name="rhel-8"></a>RHEL 8

在 RHEL 8 中创建 `ag_cluster` 资源时，它会将资源创建为 `ag_cluster-clone`。 对于 RHEL 8，请使用以下命令：

```bash
sudo pcs constraint colocation add virtualip with master ag_cluster-clone INFINITY with-rsc-role=Master
```

## <a name="add-ordering-constraint"></a>添加排序约束

主机托管约束具有隐式排序约束。 在移动可用性组资源前，它会移动虚拟 IP 资源。 默认情况下的事件序列为：

1. 用户发出 `pcs resource move`，将资源从节点 1 的可用性组主要副本移动到节点 2。
1. 虚拟 IP 资源在节点 1 上停止。
1. 虚拟 IP 资源在节点 2 上启动。
  
   >[!NOTE]
   >此时，IP 地址暂时指向节点 2，同时节点 2 仍为故障转移前的次要副本。 
   
1. 节点 1 上的可用性组主要副本降级为次要副本。
1. 节点 2 上的可用性组次要副本升级为主要副本。 

若要防止 IP 地址暂时指向具有故障转移前的次要副本的节点，请添加排序约束。 

若要添加排序约束，请在一个节点上运行以下命令：

### <a name="rhel-7"></a>RHEL 7

```bash
sudo pcs constraint order promote ag_cluster-master then start virtualip
```

### <a name="rhel-8"></a>RHEL 8

```bash
sudo pcs constraint order promote ag_cluster-clone then start virtualip
```

>[!IMPORTANT]
>配置群集并将可用性组添加为群集资源后，无法使用 Transact-SQL 对可用性组资源进行故障转移。 Linux 上的 SQL Server 群集资源与此操作系统的耦合性不及在 Windows Server 故障转移群集 (WSFC) 上的耦合性高。 SQL Server 服务无法识别此群集是否存在。 所有业务流程都通过群集管理工具完成。 在 RHEL 或 Ubuntu 中使用 `pcs`，在 SLES 中使用 `crm` 工具。 

使用 `pcs` 手动对可用性组进行故障转移。 请勿使用 Transact-SQL 启动故障转移。 有关说明，请参阅[故障转移](sql-server-linux-availability-group-failover-ha.md#failover)。

## <a name="next-steps"></a>后续步骤

[操作 HA 可用性组](sql-server-linux-availability-group-failover-ha.md)
