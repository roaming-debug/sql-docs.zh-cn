---
title: 用于可用性组的 SQL Server 强制故障转移
description: 对群集类型为 NONE 的可用性组强制进行故障转移
services: ''
author: MikeRayMSFT
ms.topic: include
ms.date: 02/05/2018
ms.author: mikeray
ms.custom: include file
ms.openlocfilehash: 6e4eb6f9cf7b58a18f42d0b99531084d7fb2cd59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343384"
---
每个可用性组仅有一个主要副本。 主要副本允许读取和写入操作。 若要更改哪个副本为主要副本，可进行故障转移。 在典型的可用性组中，群集管理器自动执行故障转移过程。 在群集类型为 NONE 的可用性组中，需手动执行故障转移过程。

在群集类型为 NONE 的可用性组中，有两种对主要副本进行故障转移的方法：

- 手动故障转移（无数据丢失）
- 强制手动故障转移（会丢失数据）


### <a name="manual-failover-without-data-loss"></a>手动故障转移（无数据丢失）

主要副本可用时使用此方法，但你需要暂时或永久更改托管主要副本的实例。
若要避免潜在的数据丢失，发出手动故障转移前，确保目标次要副本为最新版本。

手动故障转移（无数据丢失）：

1. 将当前的主要副本和目标次要副本设置为 `SYNCHRONOUS_COMMIT`。

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        MODIFY REPLICA ON N'<node2>' 
        WITH (AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);
   ```

1. 若要确定已将活动事务提交到主要副本和至少一个同步次要副本，请运行以下查询：

   ```SQL
   SELECT ag.name, 
      drs.database_id, 
      drs.group_id, 
      drs.replica_id, 
      drs.synchronization_state_desc, 
      ag.sequence_number
   FROM sys.dm_hadr_database_replica_states drs, sys.availability_groups ag
   WHERE drs.group_id = ag.group_id; 
   ```

   当 `synchronization_state_desc` 为 `SYNCHRONIZED` 时，会同步次要副本。

1. 将 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 更新为 1。

   以下脚本在名为 `ag1` 的可用性组上将 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 设置为 1。 运行以下脚本前，将 `ag1` 替换为可用性组的名称：

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        SET (REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = 1);
   ```

   此设置可确保将每个活动事务提交到主要副本和至少一个同步次要副本。
   >[!NOTE]
   >此设置并非特定于故障转移，应根据环境要求进行设置。

1. 将主要副本设置为脱机，准备进行角色更改： 

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] OFFLINE
   ```

1. 将目标次要副本升级为主要副本。

   ```SQL
   ALTER AVAILABILITY GROUP AGRScale FORCE_FAILOVER_ALLOW_DATA_LOSS; 
   ```

1. 将旧的主要副本的角色更新为 `SECONDARY`，在托管旧的主要副本的 SQL Server 实例上运行以下命令：

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        SET (ROLE = SECONDARY); 
   ```

   > [!NOTE]
   > 若要删除可用性组，请使用[删除可用性组](../t-sql/statements/drop-availability-group-transact-sql.md)。 对于使用群集类型为 NONE 或 EXTERNAL 创建的可用性组，请对可用性组的所有副本执行该命令。

1. 恢复数据移动，为托管主要副本的 SQL Server 实例上的可用性组中的每个数据库运行以下命令：

   ```SQL
   ALTER DATABASE [db1]
        SET HADR RESUME
   ```

1. 重新创建出于读取缩放目的创建且不受群集管理器管理的所有侦听器。 如果原始侦听器指向旧的主要副本，请将其删除，然后将其重新创建为指向新的主要副本。

### <a name="forced-manual-failover-with-data-loss"></a>强制手动故障转移（会丢失数据）

如果主要副本不可用且无法立即恢复，则需要强制执行向次要副本的故障转移（存在数据丢失）。 但是，如果原始主要副本在故障转移后恢复，它将承担主要角色。 若要避免每个副本处于不同的状态，在存在数据丢失的情况下进行强制故障转移后，从可用性组中删除原始主要副本。 原始主要副本重新联机后，从该副本完全删除该可用性组。 

若要强制执行从主要副本 N1 到次要副本 N2 的手动故障转移（存在数据丢失），请执行以下步骤： 

1. 在次要副本 (N2) 上，启动强制故障转移： 

    ```SQL
    ALTER AVAILABILITY GROUP [AGRScale] FORCE_FAILOVER_ALLOW_DATA_LOSS;
    ```
    
1. 在新的主要副本 (N2) 上，删除原始主要副本 (N1)： 

    ```SQL
    ALTER AVAILABILITY GROUP [AGRScale]
    REMOVE REPLICA ON N'N1';
    ```
    
1. 验证所有的应用程序流量均指向侦听器和/或新的主要副本。 
1. 如果原始主要副本 (N1) 进入联机状态，则立即在原始主要副本 (N1) 上使可用性组 AGRScale 脱机：

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] OFFLINE
   ```
1. 如果存在数据或未同步的更改，则通过备份或其他可满足业务需求的数据复制选项来保存这些数据。     
1. 接下来，从原始主要副本 (N1) 中删除可用性组：

    ```SQL
    DROP AVAILABILITY GROUP [AGRScale];
    ```
1. 删除原始主要副本 (N1) 上的可用性组数据库： 

    ```SQL
    USE [master]
    GO
    DROP DATABASE [AGDBRScale]
    GO
    ```
    
 1. （可选）如果需要，现可将 N1 作为新的次要副本添加回可用性组 AGRScale 中。
