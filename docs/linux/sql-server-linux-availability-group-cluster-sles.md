---
title: SUSE：为 Linux 上的 SQL Server 配置可用性组
titleSuffix: SQL Server
description: 了解如何在 SUSE Linux Enterprise Server (SLES) 上为 SQL Server 创建可用性组群集。
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 85180155-6726-4f42-ba57-200bf1e15f4d
ms.openlocfilehash: 232235e1e7b8718b83c4826b4a019cb6c52e437c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338411"
---
# <a name="configure-sles-cluster-for-sql-server-availability-group"></a>为 SQL Server 可用性组配置 SLES 群集

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本指南介绍如何在 SUSE Linux Enterprise Server (SLES) 12 SP2 上为 SQL Server 创建三节点群集。 若要实现高可用性，Linux 上的可用性组需要三个节点，请参阅[可用性组配置的高可用性和数据保护](sql-server-linux-availability-group-ha.md)。 此群集层基于在 [Pacemaker](https://clusterlabs.org/) 之上生成的 SUSE [High Availability Extension (HAE)](https://www.suse.com/products/highavailability)。 

有关群集配置、资源代理选项、管理、最佳实践和建议的详细信息，请参阅 [SUSE Linux Enterprise High Availability Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html)（SUSE Linux Enterprise 高可用性扩展 12 SP2）。

>[!NOTE]
>目前，SQL Server 在 Linux 上与 Pacemaker 集成不及在 Windows 上与 WSFC 集成的耦合性高。 Linux 上的 SQL Server 服务无法识别群集。 Pacemaker 控制群集资源的所有业务流程，包括可用性组资源。 在 Linux 上，不应该依赖提供 sys.dm_hadr_cluster 等群集信息的 Always On 可用性组动态管理视图 (DMV)。 此外，虚拟网络名称特定于 WSFC，Pacemaker 中无相同的等效项。 仍可创建一个侦听器，将其用于故障转移后的透明重新连接，但需要使用创建虚拟 IP 资源所用的 IP 在 DNS 服务器中手动注册侦听器名称（如以下部分所述）。

[!INCLUDE [bias-sensitive-term-t](../includes/bias-sensitive-term-t.md)]

## <a name="roadmap"></a>路线图

为实现高可用性而创建可用性组的过程因 Linux 服务器和 Windows Server 故障转移群集而异。 下面的列表对高级步骤进行了说明： 

1. [在群集节点上配置 SQL Server](sql-server-linux-setup.md)。

2. [创建可用性组](sql-server-linux-availability-group-failover-ha.md)。 

3. 配置 Pacemaker 等群集资源管理器。 本文档中包含这些说明。
   
   配置群集资源管理器的方式取决于特定的 Linux 分发版。 

   >[!IMPORTANT]
   >生产环境需要 STONITH 等隔离代理，以实现高可用性。 本文中的示例不使用隔离代理。 它们仅用于测试和验证目的。 
   
   >Pacemaker 群集使用隔离将群集恢复到已知状态。 配置隔离的方式取决于分发版和环境。 目前，隔离在某些云环境中不可用。 请参阅 [SUSE Linux Enterprise High Availability Extension](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.fencing)。

5. [将可用性组添加为群集中的资源](sql-server-linux-availability-group-cluster-sles.md#configure-the-cluster-resources-for-sql-server)。 

## <a name="prerequisites"></a>先决条件

要完成下面的端到端方案，需要三台计算机来部署三个节点群集。 下面的步骤概述了配置这些服务器的方法。

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>在每个群集节点上安装和配置操作系统 

第一步是在群集节点上配置操作系统。 对于此演练，使用具有适用于 HA 加载项的有效订阅的 SLES 12 SP2。

### <a name="install-and-configure-sql-server-service-on-each-cluster-node"></a>在每个群集节点上安装和配置 SQL Server 服务

1. 在所有节点上安装和设置 SQL Server 服务。 有关详细说明，请参阅[安装 Linux 上的 SQL Server](sql-server-linux-setup.md)。

1. 将一个节点指定为主节点，将其他节点指定为辅助节点。 本指南中将使用这些术语。

1. 确保将成为群集一部分的节点可互相通信。

   下面的示例显示了 `/etc/hosts`，以及名为 SLES1、SLES2 和 SLES3 的三个节点。

   ```
   127.0.0.1   localhost
   10.128.16.33 SLES1
   10.128.16.77 SLES2
   10.128.16.22 SLES3
   ```

   所有群集节点都必须能够通过 SSH 相互访问。 `hb_report` 或 `crm_report`（用于故障排除）和 Hawk 的历史记录浏览器等工具需要在节点之间进行无密码的 SSH 访问，否则它们只能收集当前节点的数据。 如果使用非标准 SSH 端口，请使用 -X 选项（请参阅 `man` 页）。 例如，如果 SSH 端口为 3479，请使用以下命令调用 `crm_report`：

   ```bash
   sudo crm_report -X "-p 3479" [...]
   ```

   有关详细信息，请参阅 [SLES 管理指南 - 杂项部分](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.misc)。


## <a name="create-a-sql-server-login-for-pacemaker"></a>为 Pacemaker 创建 SQL Server 登录名

[!INCLUDE [SLES-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="configure-an-always-on-availability-group"></a>配置 Always On 可用性组

在 Linux 服务器上配置可用性组，然后配置群集资源。 若要配置可用性组，请参阅[在 Linux 上为 SQL Server 配置 Always On 可用性组](sql-server-linux-availability-group-configure-ha.md)

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>在每个群集节点上安装和配置 Pacemaker

1. 安装 High Availability Extension

   有关参考，请参阅[安装 SUSE Linux Enterprise Server 和 High Availability Extension](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.installation)

1. 在这两个节点上安装 SQL Server 资源代理包。

   ```bash
   sudo zypper install mssql-server-ha
   ```

## <a name="set-up-the-first-node"></a>设置第一个节点

   请参阅 [SLES 安装说明](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.1st-node)

1. 以 `root` 用户身份登录到要将其用作群集节点的物理计算机或虚拟机。
2. 执行如下命令，启动该启动脚本：
   ```bash
   sudo ha-cluster-init
   ```

   如果 NTP 尚未配置为在启动时启动，则会显示一条消息。 

   如果仍要继续，脚本会自动生成用于 SSH 访问和用于 Csync2 同步工具的密钥，然后启动这二者所需的服务。 

3. 配置群集通信层 (Corosync)： 

   a. 输入要绑定到的网络地址。 默认情况下，该脚本建议使用 eth0 网络地址。 或者，输入其他网络地址（例如，bond0 地址）。 

   b. 输入一个多播地址。 该脚本建议使用可用作默认地址的随机地址。 

   c. 输入一个多播端口。 该脚本建议将 5405 用作默认端口。 

   d. 若要配置 `SBD ()`，请为要用于 SBD 的块设备的分区输入一个持久路径。 该路径在群集中所有节点之间必须一致。 
   最后，该脚本将启动 Pacemaker 服务，从而使此单节点群集处于联机状态并启用 Web 管理接口 Hawk2。 要用于 Hawk2 的 URL 将显示在屏幕上。 

4. 有关安装过程的任何详细信息，请查看 `/var/log/sleha-bootstrap.log`。 现在已有一个正在运行的单节点群集。 使用 crm status 检查群集状态：

   ```bash
   sudo crm status
   ```

   也可以通过 `crm configure show xml` 或 `crm configure show` 查看群集配置。

5. 此启动过程创建一个名为 hacluster 的 Linux 用户，其密码为 linux。 请尽快使用安全密码替换此默认密码： 

   ```bash
   sudo passwd hacluster
   ```

## <a name="add-nodes-to-the-existing-cluster"></a>将节点添加到现有群集

如果运行的群集包含一个或多个节点，可使用 ha-cluster-join 启动脚本添加更多群集节点。 该脚本只需要访问现有的群集节点，并且将自动完成当前计算机上的基本设置。 请使用以下步骤：

如果已使用 `YaST` 群集模块配置现有群集节点，在运行 `ha-cluster-join` 之前，请确保满足以下先决条件：
- 现有节点上的根用户拥有用于无密码登录的 SSH 密钥。 
- 现有节点上配置了 `Csync2`。 有关详细信息，请参阅通过 YaST 配置 Csync2。 

1. 以根用户身份登录到要加入群集的物理计算机或虚拟机。 
2. 执行如下命令，启动该启动脚本： 

   ```bash
   sudo ha-cluster-join
   ```

   如果 NTP 尚未配置为在启动时启动，则会显示一条消息。 

3. 如果仍要继续，系统将提示输入现有节点的 IP 地址。 输入 IP 地址。 

4. 如果尚未配置这两个计算机之间的无密码 SSH 访问，系统还将提示输入现有节点的根密码。 

   登录到指定节点后，该脚本将复制 Corosync 配置，配置 SSH 和 `Csync2`，以及将当前联机的计算机作为新群集节点。 此外，该脚本还会启用 Hawk 所需的服务。 如果已配置 `OCFS2` 的共享存储，该脚本也自动为 `OCFS2` 文件系统创建装入点目录。 

5. 对要添加到群集中的所有计算机重复上述步骤。 

6. 有关该过程的详细信息，请查看 `/var/log/ha-cluster-bootstrap.log`。 

1. 使用 `sudo crm status` 检查群集状态。 如果已成功添加第二个节点，则输出将如下所示：

   ```bash
   sudo crm status
   
   3 nodes configured
   1 resource configured
   Online: [ SLES1 SLES2 SLES3]
   Full list of resources:   
   admin_addr     (ocf::heartbeat:IPaddr2):       Started node1
   ```

   >[!NOTE]
   >`admin_addr` 是在最初单节点群集设置期间配置的虚拟 IP 群集资源。

添加所有节点后，检查是否需要调整全局群集选项中的 no-quorum-policy。 这对双节点群集尤为重要。 有关详细信息，请参阅 4.1.2 部分（no-quorum-policy 选项）。 

## <a name="set-cluster-property-cluster-recheck-interval"></a>设置群集属性 cluster-recheck-interval

`cluster-recheck-interval` 表示群集检查资源参数、约束或其他群集选项中的更改的轮询间隔。 如果副本发生故障，群集将尝试按 `failure-timeout` 值和 `cluster-recheck-interval` 值所绑定的间隔重启副本。 例如，如果 `failure-timeout` 设置为 60 秒且 `cluster-recheck-interval` 设置为 120 秒，则会在 60 秒至 120 秒的间隔内尝试重启。 建议将 failure-timeout 设置为 60 秒，将 cluster-recheck-interval 设置为大于 60 秒的值。 建议不要将 cluster-recheck-interval 设置为较小的值。

若要将属性值更新为 `2 minutes`，请运行：

```bash
crm configure property cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> 如果已拥有由 Pacemaker 群集管理的可用性组资源，请注意，当 start-failure-is-fatal 群集设置的值为 false 时，所有使用最新的可用 Pacemaker 包 1.1.18-11.el7 的分发版都会为该设置引入行为更改。 此更改会影响故障转移工作流。 如果主要副本发生中断，群集本应将故障转移到其中某个可用的次要副本。 然而，用户会注意到群集一直尝试启动发生故障的主要副本。 如果该主要副本由于永久性中断而永远不联机，群集就永远不会将故障转移到另一个可用的次要副本。 由于此更改，先前建议使用的用于设置 start-failure-is-fatal 的配置不再有效，并且必须将该设置还原为其默认值 `true`。 此外，必须更新 AG 资源以包含 `failover-timeout` 属性。 
>
>若要将属性值更新为 `true`，请运行：
>
>```bash
>crm configure property start-failure-is-fatal=true
>```
>
>若要将现有 AG 资源属性 `failure-timeout` 更新为 `60s`，请运行以下命令（将 `ag1` 替换为你的可用性组资源的名称）： 
>
>```bash
>crm configure edit ag1
># In the text editor, add `meta failure-timeout=60s` after any `param`s and before any `op`s
>```

有关 Pacemaker 群集属性的详细信息，请参阅[配置群集资源](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_config_crm_resources.html)。

## <a name="configure-fencing-stonith"></a>配置隔离 (STONITH)
Pacemaker 群集供应商需要启用 STONITH，并为受支持的群集安装程序配置隔离设备。 群集资源管理器无法确定节点的状态或节点上资源的状态时，隔离用于使群集再次处于已知状态。

资源级别隔离主要通过配置资源来确保在中断期间不会发生数据损坏。 例如，可将资源级别隔离用于 DRBD（分布式复制块设备），从而在通信链接出现故障时将节点上的磁盘标记为过时。

节点级别隔离确保节点不会运行任何资源。 重置节点可实现此目的，并且其 Pacemaker 实现被称为 STONITH（即“关闭头中的其他节点”）。 Pacemaker 支持多种隔离设备，例如不间断电源或服务器的管理接口卡。

有关详细信息，请参阅：

- [Pacemaker 从头开始群集](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/)
- [隔离和 Stonith](https://clusterlabs.org/doc/crm_fencing.html)
- [SUSE HA 文档：隔离和 STONITH](https://www.suse.com/documentation/sle_ha/book_sleha/data/cha_ha_fencing.html)

在群集初始化时，如果未检测到任何配置，则禁用 STONITH。 以后可通过运行以下命令将其启用：

```bash
sudo crm configure property stonith-enabled=true
```
  
>[!IMPORTANT]
>禁用 STONITH 仅出于测试目的。 如果计划在生产环境中使用 Pacemaker，则应根据环境来计划 STONITH 实现，并使其处于启用状态。 SUSE 不为任何云环境（包括 Azure）或 Hyper-V 提供隔离代理。 因此，群集供应商不提供在这些环境中运行生产群集相关的支持。 我们正在研究解决方案以弥补该漏洞，并在将来版本中推出。

## <a name="configure-the-cluster-resources-for-sql-server"></a>为 SQL Server 配置群集资源

请参阅 [SLES 管理指南](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.manual_config)

## <a name="enable-pacemaker"></a>启用 Pacemaker

启用 Pacemaker 以使其自动启动。

请在群集中的每个节点上运行以下命令。

```bash
systemctl enable pacemaker
```

### <a name="create-availability-group-resource"></a>创建可用性组资源

以下命令为可用性组 [ag1] 的三个副本创建并配置可用性组资源。 监视操作和超时必须在 SLES 中显式指定，这是因为超时与工作负荷高度相关，因此必须针对每个部署进行仔细调整。
在群集中的某个节点上运行命令：

1. 请运行 `crm configure` 以打开 crm 提示符：

   ```bash
   sudo crm configure 
   ```

1. 在 crm 提示符中，运行以下命令以配置资源属性。

   ```bash
   primitive ag_cluster \
      ocf:mssql:ag \
      params ag_name="ag1" \
      meta failure-timeout=60s \
      op start timeout=60s \
      op stop timeout=60s \
      op promote timeout=60s \
      op demote timeout=10s \
      op monitor timeout=60s interval=10s \
      op monitor timeout=60s interval=11s role="Master" \
      op monitor timeout=60s interval=12s role="Slave" \
      op notify timeout=60s
   ms ms-ag_cluster ag_cluster \
      meta master-max="1" master-node-max="1" clone-max="3" \
     clone-node-max="1" notify="true" \
   commit
      ```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

### <a name="create-virtual-ip-resource"></a>创建虚拟 IP 资源

如果在运行 `ha-cluster-init` 时未创建虚拟 IP 资源，则可以现在创建此资源。 以下命令创建一个虚拟 IP 资源。 使用自己的网络中的可用地址替换 `<**0.0.0.0**>`，使用 CIDR 子网掩码中的位数替换 `<**24**>`。 在一个节点上运行。

```bash
crm configure \
primitive admin_addr \
   ocf:heartbeat:IPaddr2 \
   params ip=<**0.0.0.0**> \
      cidr_netmask=<**24**>
```

### <a name="add-colocation-constraint"></a>加主机托管约束
Pacemaker 群集中的几乎所有决策（例如，选择资源运行的位置）都通过比较分数来完成。 分数按资源计算，群集资源管理器选择特定资源分数最高的节点。 （如果节点的某一资源分数为负，则该资源无法在此节点上运行。）我们可以使用约束来控制群集的决策。 约束具有一个分数。 如果约束的分数低于 INFINITY，则它仅作为建议项。 分数为 INFINITY 则表明它是必需项。 我们需要确保可用性组主要副本和虚拟 IP 资源在同一主机上运行，因此我们定义一个分数为 INFINITY 的主机托管约束。 

要将虚拟 IP 的主机托管约束设置为在与主节点相同的节点上运行，请在一个节点上运行以下命令：

```bash
crm configure
colocation vip_on_master inf: \
    admin_addr ms-ag_cluster:Master
commit
```

### <a name="add-ordering-constraint"></a>添加排序约束
主机托管约束具有隐式排序约束。 在移动可用性组资源前，它会移动虚拟 IP 资源。 默认情况下的事件序列为： 

1. 用户发出 resource migrate，将资源迁移到可用性组主要副本 - 从节点 1 到节点 2。
2. 虚拟 IP 资源在节点 1 上停止。
3. 虚拟 IP 资源在节点 2 上启动。 此时，IP 地址暂时指向节点 2，同时节点 2 仍为故障转移前的次要副本。 
4. 节点 1 上的可用性组主要副本降级。
5. 节点 2 上的可用性组升级为主要副本。 

若要防止 IP 地址暂时指向具有故障转移前的次要副本的节点，请添加排序约束。 若要添加排序约束，请在一个节点上运行以下命令： 

```bash
sudo crm configure \
   order ag_first inf: ms-ag_cluster:promote admin_addr:start
```


>[!IMPORTANT]
>配置群集并将可用性组添加为群集资源后，无法使用 Transact-SQL 对可用性组资源进行故障转移。 Linux 上的 SQL Server 群集资源与此操作系统的耦合性不及在 Windows Server 故障转移群集 (WSFC) 上的耦合性高。 SQL Server 服务无法识别此群集是否存在。 所有业务流程都通过群集管理工具完成。 在 SLES 中使用 `crm`。 

使用 `crm` 手动对可用性组进行故障转移。 请勿使用 Transact-SQL 启动故障转移。 有关详细信息，请参阅[故障转移](sql-server-linux-availability-group-failover-ha.md#failover)。


有关详细信息，请参阅：
- [管理群集资源](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.config.crm).   
- [HA 概念](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.concepts)
- [Pacemaker 快速参考](https://github.com/ClusterLabs/pacemaker/blob/master/doc/pcs-crmsh-quick-ref.md) 

<!---[!INCLUDE [Pacemaker Concepts](..\includes\ss-linux-cluster-pacemaker-concepts.md)]--->

## <a name="next-steps"></a>后续步骤

[操作 HA 可用性组](sql-server-linux-availability-group-failover-ha.md)
