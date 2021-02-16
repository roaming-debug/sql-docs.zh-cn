---
title: 运行 Linux 上的可用性组 SQL Server
description: 本文介绍如何使用 Linux 上带可用性组的 SQL Server 实例来执行滚动升级。 升级之前，请查看最佳实践。
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 00e576b6fd2fc92cca591a5ffa3d6e97556bc1ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352560"
---
# <a name="operate-always-on-availability-groups-on-linux"></a>运行 Linux 上的 Always On 可用性组

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

## <a name="upgrade-availability-group"></a>升级可用性组

升级可用性组之前，请查看[升级可用性组副本实例](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md)中的模式和做法。

以下部分介绍如何使用 Linux 上带可用性组的 SQL Server 实例来执行滚动升级。 

### <a name="upgrade-steps-on-linux"></a>Linux 上的升级步骤

如果可用性组副本位于 Linux 上 SQL Server 的实例中，则可用性组的群集类型为 `EXTERNAL` 或 `NONE`。 除管理 Windows Server 故障转移群集 (WSFC) 外，群集管理器管理的可用性组是 `EXTERNAL`。 一个外部群集管理器的示例为具有 Corosync 的 Pacemaker。 不带群集管理器的可用性组具有群集类型 `NONE`。此处列出的升级步骤适用于群集类型为 `EXTERNAL` 或 `NONE` 的可用性组。

实例的升级顺序取决于其角色是否为次要角色，以及其是否承载同步或异步副本。 首先升级承载异步次要副本的 SQL Server 实例。 然后升级承载同步次要副本的实例。 

   >[!NOTE]
   >如果可用性组只有异步副本，为了避免丢失数据，请将一个副本更改为同步副本，并等待该副本完成同步。 然后升级此副本。
   
开始之前，请备份每个数据库。

1. 停用承载要升级的次要副本的节点上的资源。
   
   运行升级命令之前，请停用该资源，这样群集就不会监视它，从而避免执行不必要的故障转移。 以下示例在节点上添加位置约束，以停用资源。 使用资源名称更新 `ag_cluster-master`，使用承载要升级的副本的节点更新 `nodeName1`。

   ```bash
   pcs constraint location ag_cluster-master avoids nodeName1
   ```

1. 升级次要副本上的 SQL Server。

   以下示例升级 `mssql-server` 包和 `mssql-server-ha` 包。

   ```bash
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```
1. 删除位置约束。

   运行升级命令之前，请停用该资源，这样群集就不会监视它，从而避免执行不必要的故障转移。 以下示例在节点上添加位置约束，以停用资源。 使用资源名称更新 `ag_cluster-master`，使用承载要升级的副本的节点更新 `nodeName1`。

   ```bash
   pcs constraint remove location-ag_cluster-master-rhel1--INFINITY
   ```
   最佳做法是，确保资源已启动（使用 `pcs status` 命令），并且确保次要副本在升级后处于连接和已同步的状态。

1. 升级所有次要副本后，手动故障转移到其中一个同步次要副本。

   对于具有 `EXTERNAL` 群集类型的可用性组，请使用群集管理工具进行故障转移，具有 `NONE` 群集类型的可用性组应使用 Transact-SQL 进行故障转移。 
   以下示例使用群集管理工具对可用性组进行故障转移。 将 `<targetReplicaName>` 替换为即将成为主副本的同步次要副本的名称：

   ```bash
   sudo pcs resource move ag_cluster-master <targetReplicaName> --master  
   ``` 
   
   >[!IMPORTANT]
   >以下步骤仅适用于不带群集管理器的可用性组。

   如果可用性组群集类型为 `NONE`，则手动执行故障转移。 请按顺序完成下列步骤：

      a. 以下命令可以将主副本设置为次要副本。 将 `AG1` 替换为可用性组的名称。 在承载主副本的 SQL Server 实例上运行 Transact-SQL 命令。

      ```transact-sql
      ALTER AVAILABILITY GROUP [ag1] SET (ROLE = SECONDARY);
      ```

      b. 以下命令可以将同步次要副本设置为主副本。 在 SQL Server 的目标实例（承载同步次要副本的实例）上运行以下 Transact-SQL 命令。

      ```transact-sql
      ALTER AVAILABILITY GROUP [ag1] FAILOVER;
      ```

1. 故障转移之后，重复前面的过程，升级旧的主副本上的 SQL Server。

   以下示例升级 `mssql-server` 包和 `mssql-server-ha` 包。

   ```bash
   # add constraint for the resource to stop on the upgraded node
   # replace 'nodename2' with the name of the cluster node targeted for upgrade
   pcs constraint location ag_cluster-master avoids nodeName2
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```
   
   ```bash
   # upgrade mssql-server and mssql-server-ha packages
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```

   ```bash
   # remove the constraint; make sure the resource is started and replica is connected and synchronized
   pcs constraint remove location-ag_cluster-master-rhel1--INFINITY
   ```

1. 对于具有外部群集管理器的可用性组（群集类型为 "EXTERNAL"），请清除由手动故障转移产生的位置约束。 

   ```bash
   sudo pcs constraint remove cli-prefer-ag_cluster-master  
   ```

1. 恢复新升级的次要副本（以前的主副本）的数据移动。 当较高版本的 SQL Server 实例向某个可用性组中的较低版本实例传输日志块，则需要执行此步骤。 在新的次要副本（以前的主副本）上运行以下命令。

   ```transact-sql
   ALTER DATABASE database_name SET HADR RESUME;
   ```

升级所有服务器后即可执行故障回复。 若有必要，请故障转移回原始的主副本。 

## <a name="drop-an-availability-group"></a>删除可用性组

若要删除可用性组，请运行 [DROP AVAILABILITY GROUP](../t-sql/statements/drop-availability-group-transact-sql.md)。 如果群集类型为 `EXTERNAL` 或 `NONE`，请在承载副本的每个 SQL Server 实例上运行该命令。 例如，若要删除名为 `group_name` 的可用性组，请运行以下命令：

   ```transact-sql
   DROP AVAILABILITY GROUP group_name
   ```
 

## <a name="next-steps"></a>后续步骤

[为 SQL Server 可用性组群集资源配置 Red Hat Enterprise Linux 群集](sql-server-linux-availability-group-cluster-rhel.md)

[为 SQL Server 可用性组群集资源配置 SUSE Linux Enterprise Server 群集](sql-server-linux-availability-group-cluster-sles.md)

[为 SQL Server 可用性组群集资源配置 Ubuntu 群集](sql-server-linux-availability-group-cluster-ubuntu.md)
