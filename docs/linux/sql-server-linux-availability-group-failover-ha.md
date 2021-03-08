---
title: 管理可用性组故障转移 - Linux 上的 SQL Server
description: 本文介绍故障转移的类型：自动故障转移、计划的手动故障转移和强制手动故障转移。 自动故障转移和计划的手动故障转移会保留所有数据。
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 03/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 863278eacebc4b405a4a44e72c4318e950d0cc1d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837660"
---
# <a name="always-on-availability-group-failover-on-linux"></a>Linux 上的 Always on 可用性组故障转移

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

在可用性组 (AG) 的上下文中，可用性副本的主要角色和次要角色通常在称为故障转移的过程中可互换。 存在三种故障转移形式：自动故障转移（无数据丢失）、计划的手动故障转移（无数据丢失）和强制手动故障转移（可能丢失数据）。最后一种形式通常称为“强制故障转移”。 自动故障转移和计划的手动故障转移会保留所有数据。 AG 在可用性副本级别进行故障转移。 也就是说，AG 故障转移到其辅助副本之一（当前故障转移目标）。 

有关故障转移的背景信息，请参阅[故障转移和故障转移模式](../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)。

## <a name="manual-failover"></a><a name="failover"></a>手动故障转移

使用群集管理工具对由外部群集管理器管理的 AG 进行故障转移。 例如，如果解决方案使用 Pacemaker 管理 Linux 集群，则使用 `pcs` 在 RHEL 或 Ubuntu 上执行手动故障转移。 在 SLES 中使用 `crm`。 

> [!IMPORTANT]
> 在正常操作中，请勿使用 Transact-SQL 或 SQL Server 管理工具（如 SSMS 或 PowerShell）进行故障转移。 当 `CLUSTER_TYPE = EXTERNAL` 时，`FAILOVER_MODE` 唯一可接受的值为 `EXTERNAL`。 通过这些设置，所有手动或自动故障转移操作都由外部集群管理器执行。 有关强制故障转移（可能发生数据丢失）的说明，请参阅[强制故障转移](#forceFailover)。

### <a name="a-namemanualfailovermanual-failover-steps"></a><a name="manualFailover">手动故障转移步骤

要进行故障转移，将成为主副本的次要副本必须为同步副本。 如果次要副本是异步的，需要[更改可用性模式](../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)。

通过两个步骤手动进行故障转移。

   首先，通过从拥有资源的群集节点[将 AG 资源移动到新节点来手动进行故障转移](#manualMove)。

   群集故障转移 AG 资源并添加位置约束。 此约束将资源配置为在新节点上运行。 删除此约束以便将来能顺利进行故障转移。

   其次，[删除位置约束](#removeLocConstraint)。

#### <a name="step-1-manually-fail-over-by-moving-availability-group-resource"></a><a name="manualMove"></a> 步骤 1. 通过移动可用性组资源手动进行故障转移

要手动将名为 ag_cluster 的 AG 资源故障转移到名为 nodeName2 的群集节点，请运行适用于你的分发的命令 ：

- RHEL/Ubuntu 示例

   ```bash
   sudo pcs resource move ag_cluster-master nodeName2 --master --lifetime=30S
   ```

- SLES 示例

   ```bash
   crm resource migrate ag_cluster nodeName2 --lifetime=30S
   ```

>[!IMPORTANT]
>使用 --lifetime 选项时，为移动资源而创建的位置约束本质上是临时的，并且在前面的示例中在 30 秒内有效。
>请注意，临时约束不会自动清除，并可能会显示在约束列表中，但作为过期约束显示。 过期约束不会影响 pacemaker 群集的故障转移行为。 如果移动资源时不使用 --lifetime 选项，则应删除自动添加的位置约束，如下所述。

#### <a name="step-2-remove-the-location-constraint"></a><a name="removeLocConstraint"> </a> 步骤 2. 删除位置约束

在手动故障转移期间，`pcs` 命令 `move` 或 `crm` 命令 `migrate` 会为要放置在新目标节点上的资源添加位置约束。 若要查看新约束，请在手动移动资源后运行以下命令：

- RHEL/Ubuntu 示例

   ```bash
   sudo pcs constraint list --full
   ```

- SLES 示例

   ```bash
   crm config show
   ```

因手动故障转移而创建的约束示例。 
 `Enabled on: Node1 (score:INFINITY) (role: Master) (id:cli-prefer-ag_cluster-master)`

   > [!NOTE]
   > Red Hat Enterprise Linux 8.x 和 Ubuntu 18.04 上 pacemaker 群集中的 AG 资源名称可能类似于 ag_cluster-clone，因为有关资源的命名法正在发展为使用“可升级克隆” 。 

- RHEL/Ubuntu 示例

   在以下命令中，`cli-prefer-ag_cluster-master` 是需要删除的约束的 ID。 `sudo pcs constraint list --full` 会返回此 ID。 
   
   ```bash
   sudo pcs resource clear ag_cluster-master  
   ```
   或
   
   ```bash
   sudo pcs constraint remove cli-prefer-ag_cluster-master  
   ```
  
   或者，你可以在单个行中移动和清除自动生成的约束，如下所示。 下面的示例根据 Red Hat Enterprise Linux 8.x 使用“克隆”术语。 
  
   ```bash
   sudo pcs resource move ag_cluster-clone --master nodeName2 && sleep 30 && sudo pcs resource clear ag_cluster-clone

   ```
  
- SLES 示例

   在以下命令中，`cli-prefer-ms-ag_cluster` 是约束的 ID。 `crm config show` 会返回此 ID。 
   
   ```bash
   crm configure
   delete cli-prefer-ms-ag_cluster 
   commit
   ```

>[!NOTE]
>自动故障转移不会添加位置约束，因此不需要进行清除。 

更多相关信息：
- [Red Hat - 管理群集资源](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/ch-manageresource-HAAR.html)
- [Pacemaker - 手动移动资源](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/_move_resources_manually.html)
 [SLES 管理指南 - 资源](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.resource) 
 
## <a name="force-failover"></a><a name="forceFailover"></a> 强制故障转移 

强制故障转移只用于灾难恢复。 在这种情况下，无法使用群集管理工具进行故障转移，因为主数据中心已关闭。 如果您强制故障转移到某一未同步的辅助副本，则可能会丢失一些数据。 仅在后述情况下才执行强制故障转移：需要立即恢复对 AG 的服务且愿意冒失去数据的风险。

如果无法使用群集管理工具与群集进行交互 - 例如，如果群集由于主数据中心中的灾难事件而无响应，则可能不得不执行强制故障转移来绕过外部群集管理器。 建议不要将此过程用于常规操作，因为存在数据丢失的风险。 仅在群集管理工具无法执行故障转移操作时使用它。 从功能上讲，此过程类似于在 Windows 中对 AG [执行强制手动故障转移](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)。
 
此强制故障转移的过程特定于 Linux 上的 SQL Server。

1. 验证 AG 资源不再由群集管理。 

      - 在目标群集节点上将资源设置为非托管模式。 此命令指示资源代理停止资源监视和管理。 例如： 
      
      ```bash
      sudo pcs resource unmanage <resourceName>
      ```

      - 如果未能将资源模式设置为非托管模式，请删除该资源。 例如：

      ```bash
      sudo pcs resource delete <resourceName>
      ```

      >[!NOTE]
      >删除某个资源时，还将删除所有关联的约束。 

1. 在承载次要副本的 SQL Server 实例上，设置会话上下文变量 `external_cluster`。

   ```Transact-SQL
   EXEC sp_set_session_context @key = N'external_cluster', @value = N'yes';
   ```

1. 使用 Transact-SQL 对 AG 进行故障转移。 在以下示例中，将 `<MyAg>` 替换为 AG 的名称。 连接到承载目标次要副本的 SQL Server 实例，并运行以下命令：

   ```Transact-SQL
   ALTER AVAILABILITY GROUP <MyAg> FORCE_FAILOVER_ALLOW_DATA_LOSS;
   ```

1.  强制故障转移后，先让 AG 恢复正常运行状态，再重新启动群集资源监视和管理，或重新创建 AG 资源。 查看[强制故障转移后的重要任务](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md#FollowUp)。

1.  重启群集资源监视和管理：

   要重新启动群集资源监视和管理，请运行以下命令：

   ```bash
   sudo pcs resource manage <resourceName>
   sudo pcs resource cleanup <resourceName>
   ```

   如果删除了群集资源，请重新创建它。 要重新创建群集资源，请按照[创建可用性组资源](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource)中的说明进行操作。

>[!Important]
>不要将上述步骤用于灾难恢复演练，因为可能会导致数据丢失。 而是将异步副本更改为同步，并安装[常规手动故障转移](#manualFailover)的说明操作。

## <a name="database-level-monitoring-and-failover-trigger"></a>数据库级别监视和故障转移触发器

对于 `CLUSTER_TYPE=EXTERNAL`，故障转移触发器语义不同于 WSFC。 AG 位于 WSFC 中的 SQL Server 实例上时，转换数据库的 `ONLINE` 状态会导致 AG 运行状况报告错误。 在响应中，群集管理器触发故障转移操作。 在 Linux 上，SQL Server 实例不能与群集通信。 “由外而内”监视数据库运行状况。 如果用户选择了数据库级故障转移监视和故障转移（通过在创建 AG 时设置选项 `DB_FAILOVER=ON`），则每次群集运行监视操作时，都会检查数据库状态是否为 `ONLINE`。 群集在 `sys.databases` 中查询状态。 对于不同于 `ONLINE` 的任何状态，群集将自动触发故障转移（如满足自动故障转移条件）。 故障转移的实际时间取决于监视操作的频率以及 sys.databases 中正在更新的数据库状态。

自动故障转移至少需要一个同步副本。

## <a name="next-steps"></a>后续步骤

[为 SQL Server 可用性组群集资源配置 Red Hat Enterprise Linux 群集](sql-server-linux-availability-group-cluster-rhel.md)

[为 SQL Server 可用性组群集资源配置 SUSE Linux Enterprise Server 群集](sql-server-linux-availability-group-cluster-sles.md)

[为 SQL Server 可用性组群集资源配置 Ubuntu 群集](sql-server-linux-availability-group-cluster-ubuntu.md)
