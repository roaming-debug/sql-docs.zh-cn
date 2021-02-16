---
title: 为可用性组配置 Ubuntu 群集
titleSuffix: SQL Server on Linux
description: 了解如何在 Ubuntu 上创建三节点群集，以及如何将先前创建的可用性组资源添加到该群集。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: dd0d6fb9-df0a-41b9-9f22-9b558b2b2233
ms.openlocfilehash: 13b719848e9cbced670e58ae3a747967a8901ada
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338315"
---
# <a name="configure-ubuntu-cluster-and-availability-group-resource"></a>配置 Ubuntu 群集和可用性组资源

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文档介绍如何在 Ubuntu 上创建三节点群集，以及如何将先前创建的可用性组添加为群集中的资源。 若要实现高可用性，Linux 上的可用性组需要三个节点，请参阅[可用性组配置的高可用性和数据保护](sql-server-linux-availability-group-ha.md)。

> [!NOTE] 
> 目前，SQL Server 在 Linux 上与 Pacemaker 集成不及在 Windows 上与 WSFC 集成的耦合性高。 在 SQL 内部，无法了解到群集是否存在，所有业务流程都是由外至内，且 Pacemaker 将该服务作为一个独立实例控制。 此外，虚拟网络名称特定于 WSFC，Pacemaker 中无相同的等效项。 查询群集信息的 Always On 动态管理视图会返回空行。 仍可创建一个侦听器，将其用于故障转移后的透明重新连接，但需要使用创建虚拟 IP 资源所用的 IP 在 DNS 服务器中手动注册侦听器名称（如以下部分所述）。

以下部分介绍了设置故障转移群集解决方案的步骤。 

## <a name="roadmap"></a>路线图

在 Linux 服务器上创建可用性组以获得高可用性的步骤与 Windows Server 故障转移群集上的步骤不同。 下面的列表对高级步骤进行了说明： 

1. [在群集节点上配置 SQL Server](sql-server-linux-setup.md)。

2. [创建可用性组](sql-server-linux-availability-group-configure-ha.md)。 

3. 配置 Pacemaker 等群集资源管理器。 本文档中包含这些说明。
   
   配置群集资源管理器的方式取决于特定的 Linux 分发版。 

   >[!IMPORTANT]
   >生产环境需要 STONITH 等隔离代理，以实现高可用性。 本文档中的演示不使用隔离代理。 演示仅用于测试和验证目的。 
   >
   >Linux 群集使用隔离将群集返回到某个已知状态。 配置隔离的方式取决于分发版和环境。 目前，隔离在某些云环境中不可用。 有关详细信息，请参阅 [RHEL 高可用性群集的支持策略 - 虚拟化平台](https://access.redhat.com/articles/29440)。
   >
   >通常在操作系统上实现隔离，并取决于环境。 在操作系统分销商文档中查找有关隔离的说明。

5.  [将可用性组添加为群集中的资源](sql-server-linux-availability-group-cluster-ubuntu.md#create-availability-group-resource)。 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>在每个群集节点上安装和配置 Pacemaker

1. 在所有节点上打开防火墙端口。 对 Pacemaker 高可用性服务、SQL Server 实例和可用性组终结点打开此端口。 运行 SQL Server 的服务器的默认 TCP 端口是 1433。  

   ```bash
   sudo ufw allow 2224/tcp
   sudo ufw allow 3121/tcp
   sudo ufw allow 21064/tcp
   sudo ufw allow 5405/udp
        
   sudo ufw allow 1433/tcp # Replace with TDS endpoint
   sudo ufw allow 5022/tcp # Replace with DATA_MIRRORING endpoint
        
   sudo ufw reload
   ```
   
   或者，可以直接禁用防火墙：
        
   ```bash
   sudo ufw disable
   ```

1. 安装 Pacemaker 包。 在所有节点上运行以下命令：

   ```bash
   sudo apt-get install pacemaker pcs fence-agents resource-agents
   ```

2. 为安装 Pacemaker 和 Corosync 包时创建的默认用户设置密码。 在所有节点上使用相同的密码。 

   ```bash
   sudo passwd hacluster
   ```

## <a name="enable-and-start-pcsd-service-and-pacemaker"></a>启用并启动 pcsd 服务和 Pacemaker

以下命令将启用并启动 pcsd 服务和 Pacemaker。 在所有节点上运行。 这样，节点就可以在重新启动后重新加入群集。 

```bash
sudo systemctl enable pcsd
sudo systemctl start pcsd
sudo systemctl enable pacemaker
```
>[!NOTE]
>“启用 Pacemaker”命令完成时可能会出现错误“Pacemaker 默认启动不包含运行级别，正在中止”。 这无关紧要，群集配置可继续进行。 

## <a name="create-the-cluster"></a>创建群集

1. 从所有节点中删除所有现有群集配置。 

   运行“sudo apt-get install pcs”将同时安装 pacemaker、corosync 和 pcs，并开始运行这 3 个服务。  启动 corosync 将生成“/etc/cluster/corosync.conf”模板文件。  若要成功执行后续步骤，不应存在此文件 - 因此解决方法是停止 pacemaker/corosync 并删除“/etc/cluster/corosync.conf”，之后，后续步骤便可成功完成。 “pcs cluster destroy”的作用相同，可将其用作一次性初始群集设置步骤。
   
   以下命令将删除所有现有群集配置文件并停止所有群集服务。 这会永久性销毁群集。 请在预生产环境中将其作为第一个步骤运行。 请注意，“pcs cluster destroy”已禁用 Pacemaker 服务，需重新启用。 在所有节点上运行以下命令。
   
   >[!WARNING]
   >该命令会销毁所有现有群集资源。

   ```bash
   sudo pcs cluster destroy 
   sudo systemctl enable pacemaker
   ```

1. 创建群集。 

   >[!WARNING]
   >由于出现一个已知问题（群集供应商正在对其进行调查），群集启动（“pcs cluster start”）将失败，并出现以下错误。 这是因为在运行群集设置命令时创建的 /etc/corosync/corosync.conf 中配置的日志文件是错误的。 若要解决此问题，请将日志文件更改为 /var/log/corosync/corosync.log。 或者可创建 /var/log/cluster/corosync.log 文件。
 
   ```Error
   Job for corosync.service failed because the control process exited with error code. 
   See "systemctl status corosync.service" and "journalctl -xe" for details.
   ```
  
以下命令将创建一个三节点群集。 在运行该脚本之前，替换 `< ... >` 之内的值。 在主要节点上运行以下命令。 

   ```bash
   sudo pcs cluster auth <node1> <node2> <node3> -u hacluster -p <password for hacluster>
   sudo pcs cluster setup --name <clusterName> <node1> <node2...> <node3>
   sudo pcs cluster start --all
   sudo pcs cluster enable --all
   ```
   
   >[!NOTE]
   >如果以前已在相同节点上配置了群集，则需要在运行“pcs cluster setup”时使用“--force”选项。 请注意，这相当于运行“pcs cluster destroy”，需要使用“sudo systemctl enable pacemaker”重新启用 Pacemaker 服务。


## <a name="configure-fencing-stonith"></a>配置隔离 (STONITH)

Pacemaker 群集供应商需要启用 STONITH，并为受支持的群集安装程序配置隔离设备。 群集资源管理器无法确定节点的状态或节点上资源的状态时，隔离用于使群集再次处于已知状态。 资源级别隔离主要确保在因配置资源引起服务中断时，不会发生数据损坏。 例如，可将资源级别隔离用于 DRBD（分布式复制块设备），从而在通信链接出现故障时将节点上的磁盘标记为过时。 节点级别隔离确保节点不会运行任何资源。 重置节点可实现此目的，并且其 Pacemaker 实现被称为 STONITH（即“关闭头中的其他节点”）。 Pacemaker 支持多种隔离设备，例如不间断电源或服务器的管理接口卡。 有关详细信息，请参阅 [Pacemaker Clusters from Scratch](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/)（从头构建 Pacemaker 群集）和 [Fencing and Stonith](https://clusterlabs.org/doc/crm_fencing.html)（隔离和 Stonith） 

由于节点级别隔离配置在很大程度上取决于你的环境，因此我们在本教程中禁用它（稍后可以对其进行配置）。 在主要节点上运行以下脚本： 

```bash
sudo pcs property set stonith-enabled=false
```

>[!IMPORTANT]
>禁用 STONITH 仅出于测试目的。 如果计划在生产环境中使用 Pacemaker，则应根据环境来计划 STONITH 实现，并使其处于启用状态。 有关针对任何特定分发隔离代理的详细信息，请与操作系统供应商联系。 

## <a name="set-cluster-property-cluster-recheck-interval"></a>设置群集属性 cluster-recheck-interval

`cluster-recheck-interval` 表示群集检查资源参数、约束或其他群集选项中的更改的轮询间隔。 如果副本发生故障，群集将尝试按 `failure-timeout` 值和 `cluster-recheck-interval` 值所绑定的间隔重启副本。 例如，如果 `failure-timeout` 设置为 60 秒且 `cluster-recheck-interval` 设置为 120 秒，则会在 60 秒至 120 秒的间隔内尝试重启。 建议将 failure-timeout 设置为 60 秒，将 cluster-recheck-interval 设置为大于 60 秒的值。 建议不要将 cluster-recheck-interval 设置为较小的值。

若要将属性值更新为 `2 minutes`，请运行：

```bash
sudo pcs property set cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> 如果已拥有由 Pacemaker 群集管理的可用性组资源，请注意，当 start-failure-is-fatal 群集设置的值为 false 时，所有使用最新的可用 Pacemaker 包 1.1.18-11.el7 的分发版都会为该设置引入行为更改。 此更改会影响故障转移工作流。 如果主要副本发生中断，群集本应将故障转移到其中某个可用的次要副本。 然而，用户会注意到群集一直尝试启动发生故障的主要副本。 如果该主要副本由于永久性中断而永远不联机，群集就永远不会将故障转移到另一个可用的次要副本。 由于此更改，先前建议使用的用于设置 start-failure-is-fatal 的配置不再有效，并且必须将该设置还原为其默认值 `true`。 此外，必须更新 AG 资源以包含 `failover-timeout` 属性。 
>
>若要将属性值更新为 `true`，请运行：
>
>```bash
>sudo pcs property set start-failure-is-fatal=true
>```
>
>若要将现有 AG 资源属性 `failure-timeout` 更新为 `60s`，请运行以下命令（将 `ag1` 替换为你的可用性组资源的名称）：
>
>```bash
>pcs resource update ag1 meta failure-timeout=60s
>```

## <a name="install-sql-server-resource-agent-for-integration-with-pacemaker"></a>安装 SQL Server 资源代理以与 Pacemaker 集成

在所有节点上运行以下命令。 

```bash
sudo apt-get install mssql-server-ha
```

## <a name="create-a-sql-server-login-for-pacemaker"></a>为 Pacemaker 创建 SQL Server 登录名

[!INCLUDE [SLES-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="create-availability-group-resource"></a>创建可用性组资源

若要创建可用性组资源，请使用 `pcs resource create` 命令并设置资源属性。 以下命令将为可用性组创建一个名为 `ag1` 的 `ocf:mssql:ag` master/subordinate 类型的资源。 

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=30s master meta notify=true

```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

## <a name="create-virtual-ip-resource"></a>创建虚拟 IP 资源

若要创建虚拟 IP 地址资源，请在一个节点上运行以下命令。 使用网络中的可用静态 IP 地址。 运行此脚本前，请使用有效的 IP 地址替换 `< ... >` 之内的值。

```bash
sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<10.128.16.240>
```

Pacemaker 中不存在等效的虚拟服务器名称。 若要使用指向字符串服务器名称的连接字符串，而不是使用 IP 地址，请在 DNS 中注册 IP 资源地址和所需的虚拟服务器名称。 对于 DR 配置，请在主站点和 DR 站点上使用 DNS 服务器注册所需的虚拟服务器名称和 IP 地址。

## <a name="add-colocation-constraint"></a>加主机托管约束

Pacemaker 群集中的几乎所有决策（例如，选择资源运行的位置）都通过比较分数来完成。 分数按资源计算，群集资源管理器选择特定资源分数最高的节点。 （如果节点的某一资源分数为负，则该资源无法在此节点上运行。）使用约束来配置群集的决策。 约束具有一个分数。 如果约束的分数低于 INFINITY，则它仅作为建议项。 分数为 INFINITY 则表明它是必需项。 若要确保主要副本和虚拟 IP 资源位于同一主机上，请定义一个分数为 INFINITY 的主机托管约束。 若要添加主机托管约束，请在一个节点上运行以下命令。 

```bash
sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
```

## <a name="add-ordering-constraint"></a>添加排序约束

主机托管约束具有隐式排序约束。 在移动可用性组资源前，它会移动虚拟 IP 资源。 默认情况下的事件序列为：

1. 用户发出 `pcs resource move`，将资源从节点 1 的可用性组主要副本移动到节点 2。
1. 虚拟 IP 资源在节点 1 上停止。
1. 虚拟 IP 资源在节点 2 上启动。

   >[!NOTE]
   >此时，IP 地址暂时指向节点 2，同时节点 2 仍为故障转移前的次要副本。 
   
1. 节点 1 上的可用性组主要副本降级为次要副本。
1. 节点 2 上的可用性组次要副本提升为主要副本。 

若要防止 IP 地址暂时指向具有故障转移前的次要副本的节点，请添加排序约束。 

若要添加排序约束，请在一个节点上运行以下命令：

```bash
sudo pcs constraint order promote ag_cluster-master then start virtualip
```

>[!IMPORTANT]
>配置群集并将可用性组添加为群集资源后，无法使用 Transact-SQL 对可用性组资源进行故障转移。 Linux 上的 SQL Server 群集资源与此操作系统的耦合性不及在 Windows Server 故障转移群集 (WSFC) 上的耦合性高。 SQL Server 服务无法识别此群集是否存在。 所有业务流程都通过群集管理工具完成。 在 RHEL 或 Ubuntu 中，使用 `pcs`。 

<!---[!INCLUDE [Pacemaker Concepts](..\includes\ss-linux-cluster-pacemaker-concepts.md)]--->

## <a name="next-steps"></a>后续步骤

[操作 HA 可用性组](sql-server-linux-availability-group-failover-ha.md)

