---
title: 为 Linux 上的 SQL Server 配置可用性组
description: 了解如何在 Linux 上创建 SQL Server Always On 可用性组 (AG) 以实现高可用性。
author: VanMSFT
ms.custom: seo-lt-2019
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/26/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 7313fbc789b37cf326d55566c1f087f80b4929df
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351941"
---
# <a name="configure-sql-server-always-on-availability-group-for-high-availability-on-linux"></a>配置 SQL Server Always On 可用性组以在 Linux 上实现高可用性

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍如何在 Linux 上创建 SQL Server Always On 可用性组 (AG) 以实现高可用性。 AG 有两种配置类型。 高可用性配置使用群集管理器提供业务连续性  。 此配置还可包括读取缩放副本。 本文档介绍了如何创建 AG 以实现高可用性。

还可以在不使用群集管理器的情况下创建 AG 来实现读取缩放  。 用于读取缩放的 AG 仅提供用于性能横向扩展的只读副本。它不提供高可用性。 若要创建用于读取缩放的 AG，请参阅[在 Linux 上配置 SQL Server 可用性组用于读取缩放](sql-server-linux-availability-group-configure-rs.md)。

用于保证高可用性和数据保护的配置需要两个或三个同步提交副本。 如果使用三个同步副本，即使一个服务器不可用，AG 也可以自动恢复。 有关详细信息，请参阅[可用性组配置的高可用性和数据保护](sql-server-linux-availability-group-ha.md)。 

所有服务器必须是物理服务器或虚拟服务器，并且虚拟服务器必须位于同一虚拟化平台上。 此要求是因为隔离代理是特定于平台的。 请参阅[来宾群集策略](https://access.redhat.com/articles/29440#guest_policies)。

## <a name="roadmap"></a>路线图

在 Linux 服务器上创建 AG 以获得高可用性的步骤与 Windows Server 故障转移群集上的步骤不同。 下面的列表对高级步骤进行了说明： 

1. [在三个群集服务器上配置 SQL Server](sql-server-linux-setup.md)。

   >[!IMPORTANT]
   >AG 中的所有三个服务器都需要在同一个平台上（物理或虚拟），因为 Linux 高可用性使用隔离代理来隔离服务器上的资源。 隔离代理特定于每个平台。

2. 创建 AG。 本文介绍了此步骤。 

3. 配置 Pacemaker 等群集资源管理器。
   
   配置群集资源管理器的方式取决于特定的 Linux 分发版。 有关特定于分发的说明，请参阅以下链接： 

   * [RHEL](sql-server-linux-availability-group-cluster-rhel.md)
   * [SUSE](sql-server-linux-availability-group-cluster-sles.md)
   * [Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md)

   >[!IMPORTANT]
   >生产环境需要 STONITH 等隔离代理，以实现高可用性。 本文档中的演示不使用隔离代理。 演示仅用于测试和验证目的。 
   
   >Linux 群集使用隔离将群集返回到某个已知状态。 配置隔离的方式取决于分发版和环境。 目前，在某些云环境中无法使用隔离。 有关详细信息，请参阅 [RHEL 高可用性群集的支持策略 - 虚拟化平台](https://access.redhat.com/articles/29440)。
   
   >关于 SLES，请参阅 [SUSE Linux Enterprise High Availability Extension](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.fencing)（SUSE Linux 企业高可用性扩展）。

5. 将 AG 添加为群集中的资源。  

   将 AG 添加为群集中的资源的方式取决于 Linux 分发。 有关特定于分发的说明，请参阅以下链接： 

   * [RHEL](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource)
   * [SLES](sql-server-linux-availability-group-cluster-sles.md#configure-the-cluster-resources-for-sql-server)
   * [Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md#create-availability-group-resource)

[!INCLUDE [Create Prerequisites](../includes/ss-linux-cluster-availability-group-create-prereq.md)]

## <a name="create-the-ag"></a>创建 AG

本节中的示例说明如何使用 Transact-SQL 创建可用性组。 还可以使用 SQL Server Management Studio 可用性组向导。 使用向导创建 AG 时，在将副本加入 AG 时，会返回错误。 若要解决此问题，请向 pacemaker 授予对所有副本上 AG 的 `ALTER`、`CONTROL` 和 `VIEW DEFINITIONS` 权限。 在主副本上授予权限后，通过向导将节点加入 AG，但为了使 HA 正常运行，请为所有副本授予权限。

若要实现可确保自动故障转移的高可用性配置，AG 至少需要三个副本。 以下任一配置均支持高可用性：

- [三个同步副本](sql-server-linux-availability-group-ha.md#threeSynch)

- [两个同步副本和一个配置副本](sql-server-linux-availability-group-ha.md#twoSynch)

有关详细信息，请参阅[可用性组配置的高可用性和数据保护](sql-server-linux-availability-group-ha.md)。

>[!NOTE]
>可用性组可以包括其他同步或异步副本。 

在 Linux 上创建 AG 以实现高可用性。 配合使用 [CREATE AVAILABILITY GROUP](../t-sql/statements/create-availability-group-transact-sql.md) 与 `CLUSTER_TYPE = EXTERNAL`。 

* 可用性组 - `CLUSTER_TYPE = EXTERNAL` 指定：由外部群集实体管理 AG。 Pacemaker 是外部群集实体的一个示例。 当 AG 群集类型为外部时， 

* 设置主副本和次要副本 `FAILOVER_MODE = EXTERNAL`。 
   指定：副本与外部群集管理器（如 Pacemaker）交互。 

以下 Transact-SQL 脚本为名为 `ag1` 的高可用性创建 AG。 脚本使用 `SEEDING_MODE = AUTOMATIC` 配置 AG 副本。 此设置会导致 SQL Server 在每个辅助服务器上自动创建数据库。 为环境更新以下脚本。 将 `<node1>`、`<node2>` 或 `<node3>` 值替换为托管副本的 SQL Server 实例的名称。 将 `<5022>` 替换为为数据镜像终结点设置的端口。 若要创建 AG，请在承载主副本的 SQL Server 实例上运行以下 Transact-SQL。

仅运行以下脚本之一  ： 

- [创建具有三个同步副本的可用性组](#threeSynch)。
- [创建具有两个同步副本和一个配置副本的可用性组](#configOnly)
- [创建具有两个同步副本的可用性组](#readScale)。

<a name="threeSynch"></a>

- 创建具有三个同步副本的 AG

   ```SQL
   CREATE AVAILABILITY GROUP [ag1]
       WITH (DB_FAILOVER = ON, CLUSTER_TYPE = EXTERNAL)
       FOR REPLICA ON
           N'<node1>' 
            WITH (
               ENDPOINT_URL = N'tcp://<node1>:<5022>',
               AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
               FAILOVER_MODE = EXTERNAL,
               SEEDING_MODE = AUTOMATIC
               ),
           N'<node2>' 
            WITH ( 
               ENDPOINT_URL = N'tcp://<node2>:<5022>', 
               AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
               FAILOVER_MODE = EXTERNAL,
               SEEDING_MODE = AUTOMATIC
               ),
           N'<node3>'
           WITH( 
              ENDPOINT_URL = N'tcp://<node3>:<5022>', 
              AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
              FAILOVER_MODE = EXTERNAL,
              SEEDING_MODE = AUTOMATIC
              );
        
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```

   >[!IMPORTANT]
   >运行前一个脚本创建具有三个同步副本的 AG 后，请不要运行以下脚本：

<a name="configOnly"></a>
- 创建具有两个同步副本和一个配置副本的 AG：

   >[!IMPORTANT]
   >此体系结构允许任何版本的 SQL Server 承载第三个副本。 例如，第三个副本可以托管在 SQL Server Enterprise Edition 上。 在 Enterprise Edition 上，唯一有效的终结点类型是 `WITNESS`。 

   ```SQL
   CREATE AVAILABILITY GROUP [ag1] 
      WITH (CLUSTER_TYPE = EXTERNAL) 
      FOR REPLICA ON 
       N'<node1>' WITH ( 
          ENDPOINT_URL = N'tcp://<node1>:<5022>', 
          AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
          FAILOVER_MODE = EXTERNAL, 
          SEEDING_MODE = AUTOMATIC 
          ), 
       N'<node2>' WITH (  
          ENDPOINT_URL = N'tcp://<node2>:<5022>',  
          AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
          FAILOVER_MODE = EXTERNAL, 
          SEEDING_MODE = AUTOMATIC 
          ), 
       N'<node3>' WITH ( 
          ENDPOINT_URL = N'tcp://<node3>:<5022>', 
          AVAILABILITY_MODE = CONFIGURATION_ONLY  
          );
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```
<a name="readScale"></a>

- 创建具有两个同步副本的 AG

   包括具有同步可用性模式的两个副本。 例如，以下脚本创建一个名为 `ag1` 的 AG。 `node1` 和 `node2` 以同步模式承载副本，具有自动种子设定和自动故障转移功能。

   >[!IMPORTANT]
   >仅运行以下脚本以创建具有两个同步副本的 AG。 如果运行了前面任一脚本，则不要运行以下脚本。 

   ```SQL
   CREATE AVAILABILITY GROUP [ag1]
      WITH (CLUSTER_TYPE = EXTERNAL)
      FOR REPLICA ON
      N'node1' WITH (
         ENDPOINT_URL = N'tcp://node1:5022',
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
         FAILOVER_MODE = EXTERNAL,
         SEEDING_MODE = AUTOMATIC
      ),
      N'node2' WITH ( 
         ENDPOINT_URL = N'tcp://node2:5022', 
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
         FAILOVER_MODE = EXTERNAL,
         SEEDING_MODE = AUTOMATIC
      );
        
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```


还可以使用 SQL Server Management Studio 或 PowerShell 配置带有 `CLUSTER_TYPE=EXTERNAL` 的 AG。 

### <a name="join-secondary-replicas-to-the-ag"></a>将次要副本联接到 AG

Pacemaker 用户需要具有对所有副本上可用性组的 `ALTER`、`CONTROL` 和 `VIEW DEFINITION` 权限。 若要授予权限，在创建可用性组且将主副本和每个次要副本添加到可用性组之后，请在这些副本上立即运行以下 Transact-SQL 脚本。 在运行脚本之前，请将 `<pacemakerLogin>` 替换为 pacemaker 用户帐户的名称。 如果没有 pacemaker 的登录名，请[为 pacemaker 创建 SQL Server 登录名](sql-server-linux-availability-group-cluster-ubuntu.md#create-a-sql-server-login-for-pacemaker)。

```sql
GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::ag1 TO <pacemakerLogin>
GRANT VIEW SERVER STATE TO <pacemakerLogin>
```

以下 Transact-SQL 脚本将 SQL Server 实例加入名为 `ag1` 的 AG。 为环境更新脚本。 在托管次要副本的每个 SQL Server 实例上运行以下 Transact-SQL，以加入 AG。

```sql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
         
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create Post](../includes/ss-linux-cluster-availability-group-create-post.md)]

>[!IMPORTANT]
>创建 AG 后，必须配置与 Pacemaker 等群集技术的集成，以实现高可用性。 对于使用 AG 的读取缩放配置，从 [!INCLUDE [SQL Server version](../includes/sssql17-md.md)] 开始，不再需要设置群集。

如果执行本文档中的步骤，会得到尚未群集化的 AG。 下一步是添加群集。 此配置对于读取缩放/负载均衡方案有效，但对于高可用性而言并不完整。 为实现高可用性，需要将 AG 添加为群集资源。 有关说明，请参阅[后续步骤](#next-steps)。 

## <a name="notes"></a>说明

>[!IMPORTANT]
>配置群集并将 AG 添加为群集资源后，无法使用 Transact-SQL 对 AG 资源进行故障转移。 Linux 上的 SQL Server 群集资源与此操作系统的耦合性不及在 Windows Server 故障转移群集 (WSFC) 上的耦合性高。 SQL Server 服务无法识别此群集是否存在。 所有业务流程都通过群集管理工具完成。 在 RHEL 或 Ubuntu 中，使用 `pcs`。 在 SLES 中使用 `crm`。 

>[!IMPORTANT]
>如果 AG 是群集资源，则当前版本中存在一个已知问题，即在数据丢失时强制故障转移到异步副本不起作用。 此问题将在即将发布的版本中得到解决。 手动或自动故障转移到同步副本成功。


## <a name="next-steps"></a>后续步骤

[为 SQL Server 可用性组群集资源配置 Red Hat Enterprise Linux 群集](sql-server-linux-availability-group-cluster-rhel.md)

[为 SQL Server 可用性组群集资源配置 SUSE Linux Enterprise Server 群集](sql-server-linux-availability-group-cluster-sles.md)

[为 SQL Server 可用性组群集资源配置 Ubuntu 群集](sql-server-linux-availability-group-cluster-ubuntu.md)