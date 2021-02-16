---
title: 可用性组部署模式 - Linux 上的 SQL Server
description: 了解 Linux 服务器上的 SQL Server Always On 可用性组支持的部署配置。
ms.custom: seo-lt-2019
ms.date: 04/17/2019
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
ms.assetid: edd75f68-dc62-4479-a596-57ce8ad632e5
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.openlocfilehash: edeb7ab3de19b50f78f2b4b5bab9c0f8afe80780
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347782"
---
# <a name="high-availability-and-data-protection-for-availability-group-configurations"></a>可用性组配置的高可用性和数据保护

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍 Linux 服务器上的 SQL Server Always On 可用性组支持的部署配置。 可用性组支持高可用性和数据保护。 自动故障检测、自动故障转移和故障转移后的透明重新连接提供高可用性。 同步副本提供数据保护。 

在 Windows Server 故障转移群集 (WSFC) 上，常见的高可用性配置使用两个同步副本和第三个服务器或文件共享来提供仲裁。 文件共享见证验证可用性组配置 - 例如，同步状态和副本的角色。 此配置可确保选作故障转移目标的次要副本具有最新的数据和可用性组配置更改。 

WSFC 为可用性组副本和文件共享见证之间的故障转移仲裁同步配置元数据。 当可用性组不在 WSFC 上时，SQL Server 实例将配置元数据存储在 master 数据库中。

例如，Linux 群集上的某个可用性组具有 `CLUSTER_TYPE = EXTERNAL`。 没有用于仲裁故障转移的 WSFC。 在这种情况下，配置元数据由 SQL Server 实例进行管理和维护。 由于此群集中没有见证服务器，因此需要第三个 SQL Server 实例来存储配置状态元数据。 这三个 SQL Server 实例一起为群集提供分布式元数据存储。 

群集管理器可以查询可用性组中的 SQL Server 实例，并协调故障转移以维持高可用性。 在 Linux 群集中，Pacemaker 是群集管理器。 

SQL Server 2017 CU 1 可为具有两个同步副本和一个仅配置副本且 `CLUSTER_TYPE = EXTERNAL` 的可用性组实现高可用性。 仅配置副本可以托管在 SQL Server 2017 CU1 或更高版本的任意版本上 - 包括 SQL Server Express 版本。 仅配置副本在 master 数据库中维护有关可用性组的配置信息，但不包含可用性组中的用户数据库。 

## <a name="how-the-configuration-affects-default-resource-settings"></a>配置如何影响默认资源设置

SQL Server 2017 引入了 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 群集资源设置。 此设置可保证在主要副本提交每个事务之前，指定数量的次要副本将事务数据写入日志。 使用外部群集管理器时，此设置会影响高可用性和数据保护。 此设置的默认值取决于创建群集资源时的体系结构。 安装 SQL Server 资源代理 `mssql-server-ha` 并为可用性组创建群集资源时，群集管理器会检测可用性组配置并相应地设置 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT`。 

如果配置支持，则资源代理参数 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 将设置为提供高可用性和数据保护的值。 有关详细信息，请参阅[了解 Pacemaker 的 SQL Server 资源代理](#pacemakerNotify)。

以下部分解释了群集资源的默认行为。 

选择一种可用性组设计，以满足高可用性、数据保护和读取扩展的特定业务需求。

以下配置描述了可用性组设计模式以及每个模式的功能。 这些设计模式适用于具有高可用性解决方案且 `CLUSTER_TYPE = EXTERNAL` 的可用性组。 

- **三个同步副本**
- **两个同步副本**
- **两个同步副本和一个仅配置副本**

<a name="threeSynch"></a>

## <a name="three-synchronous-replicas"></a>三个同步副本

此配置包含三个同步副本。 默认情况下，它可提供高可用性和数据保护。 它还可以提供读取扩展。

![三个副本][3]

具有三个同步副本的可用性组可以提供读取扩展、高可用性和数据保护。 下表对可用性行为进行了说明。 

|可用性行为 |读取扩展|高可用性和 </br> 数据保护 | 数据保护|
|:---|---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 |1<sup>\*</sup>|2|
|主要副本中断 |自动故障转移。 新的主要副本为 R/W 副本。 |自动故障转移。 新的主要副本为 R/W 副本。 |自动故障转移。 在先前的主要副本恢复并加入可用性组作为次要副本之前，新的主要副本不可用于用户事务。 |
|一个次要副本中断  | 主要副本为 R/W 副本。 如果主要副本发生故障，则不进行自动故障转移。 |主要副本为 R/W 副本。 如果主要副本也发生故障，则不进行自动故障转移。 | 主要副本不可用于用户事务。 |

<sup>\*</sup> 默认值

<a name="twoSynch"></a>

## <a name="two-synchronous-replicas"></a>两个同步副本

此配置可实现数据保护。 与其他可用性组配置一样，它可以实现读取扩展。 两个同步副本配置不提供自动高可用性。 两个副本配置仅适用于 SQL Server 2017 RTM，不再受 SQL Server 2017 的更高版本（CU1 及更高版本）的支持。

![两个同步副本][1]

具有两个同步副本的可用性组可提供读取扩展和数据保护。 下表对可用性行为进行了说明。 

|可用性行为 |读取扩展 |数据保护|
|:---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 <sup>\*</sup>|1|
|主要副本中断 | 手动故障转移。 可能导致数据丢失。 新的主要副本为 R/W 副本。| 自动故障转移。 在先前的主要副本恢复并加入可用性组作为次要副本之前，新的主要副本不可用于用户事务。|
|一个次要副本中断  |主要副本为 R/W 副本，运行可能导致数据丢失。 |在次要副本恢复之前，主要副本不可用于用户事务。|

<sup>\*</sup> 默认值

<a name = "configOnly"></a>

## <a name="two-synchronous-replicas-and-a-configuration-only-replica"></a>两个同步副本和一个仅配置副本

具有两个（或更多）同步副本和一个仅配置副本的可用性组除了提供数据保护，还可以提供高可用性。 下图呈现了此体系结构：

![仅配置可用性组][2]

1. 用户数据到次要副本的同步复制。 它还包括可用性组配置元数据。
2. 可用性组配置元数据的同步复制。 它不包括用户数据。

在可用性组关系图中，主要副本将配置数据推送到次要副本和仅配置副本。 次要副本还接收用户数据。 仅配置副本不接收用户数据。 次要副本处于同步可用性模式。 仅配置副本不包含可用性组中的数据库 - 仅包含有关可用性组的元数据。 仅配置副本上的配置数据是同步提交的。

> [!NOTE]
> 具有仅配置副本的可用性组是 SQL Server 2017 CU1 的新增功能。 可用性组中的所有 SQL Server 实例都必须为 SQL Server 2017 CU1 或更高版本。 

`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 的默认值为 0。 下表对可用性行为进行了说明。 

|可用性行为 |高可用性和 </br> 数据保护 | 数据保护|
|:---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 <sup>\*</sup>|1|
|主要副本中断 | 自动故障转移。 新的主要副本为 R/W 副本。 | 自动故障转移。 新的主要副本不可用于用户事务。 |
|次要副本中断 | 主要副本为 R/W 副本，运行可能导致数据丢失（如果主要副本发生故障且无法恢复）。 如果主要副本也发生故障，则不进行自动故障转移。 | 主要副本不可用于用户事务。 如果主要副本也发生故障，则没有可以将故障转移到的副本。 |
|仅配置副本中断 | 主要副本为 R/W 副本。 如果主要副本也发生故障，则不进行自动故障转移。 | 主要副本为 R/W 副本。 如果主要副本也发生故障，则不进行自动故障转移。 |
|同步次要副本 + 仅配置副本中断| 主要副本不可用于用户事务。 不进行自动故障转移。 | 主要副本不可用于用户事务。 如果主要副本也发生故障，则没有可以将故障转移到的副本。 |

<sup>\*</sup> 默认值

> [!NOTE]
> 托管仅配置副本的 SQL Server 实例也可以托管其他数据库。 它还可以作为仅配置数据库参与到多个可用性组中。 

## <a name="requirements"></a>要求

- 具有仅配置副本的可用性组中的所有副本都必须为 SQL Server 2017 CU 1 或更高版本。
- 任何版本的 SQL Server 都可以托管仅配置副本，包括 SQL Server Express。 
- 除主要副本外，可用性组还需要至少一个次要副本。
- 仅配置副本不计入每个 SQL Server 实例的最大副本数。 SQL Server Standard Edition 最多允许 3 个副本，SQL Server Enterprise Edition 最多允许 9 个副本。

## <a name="considerations"></a>注意事项

- 每个可用性组只能有一个仅配置副本。 
- 仅配置副本不能是主要副本。
- 无法修改仅配置副本的可用性模式。 若要将仅配置副本更改为同步或异步次要副本，请删除仅配置副本，并添加具有所需可用性模式的次要副本。 
- 仅配置副本与可用性组元数据同步。 没有用户数据。 
- 具有一个主要副本和一个仅配置副本但没有次要副本的可用性组是无效的。 
- 无法在 SQL Server Express 版本的实例上创建可用性组。 

<a name="pacemakerNotify"></a>

## <a name="understand-sql-server-resource-agent-for-pacemaker"></a>了解 Pacemaker 的 SQL Server 资源代理

SQL Server 2017 CTP 1.4 向 `sys.availability_groups` 添加了 `sequence_number`，以允许 Pacemaker 通过主要副本识别次要副本的最新状态。 `sequence_number` 是一个单调递增的 BIGINT，表示本地可用性组副本的最新状态。 Pacemaker 会在每个可用性组配置更改时更新 `sequence_number`。 配置更改示例包括故障转移、副本添加或删除。 该数字将在主要副本上更新，然后复制到次要副本。 因此，具有最新配置的次要副本的序列号与主要副本相同。 

当 Pacemaker 决定将某个副本提升为主要副本时，会先向所有副本发送 *预提升* 通知。 各副本将返回序列号。 接下来，当 Pacemaker 实际尝试将某个副本提升为主要副本时，只有当该副本的序列号在所有序列号中最高时，该副本才会提升自身。 如果其自己的序列号与最高序列号不匹配，则该副本会拒绝提升操作。 这样，就只有序列号最高的副本才会升级为主副本，确保不会丢失数据。 

此过程需要至少一个可用于提升的副本，其序列号与上一个主要副本相同。 Pacemaker 资源代理设置 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT`，使得至少一个同步次要副本处于最新状态，并且默认情况下可用作自动故障转移的目标。 对于每个监视操作，计算 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 的值（并在必要时更新）。 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 值为“同步副本数”除以 2。 在故障转移时，资源代理需要（`total number of replicas` - `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 个副本）来响应预提升通知。 `sequence_number` 最高的副本提升为主要副本。 

例如，某个可用性组具有三个同步副本：一个主要副本和两个同步次要副本。

- `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 为 1；(3 / 2 -> 1)。

- 需要响应预提升操作的副本数为 2；(3 - 1 = 2)。 

在此方案中，必须有两个副本响应才能触发故障转移。 为了在主要副本中断后成功进行自动故障转移，两个次要副本都需要保持最新状态并响应预提升通知。 如果它们处于联机状态且同步，则它们具有相同的序列号。 可用性组将提升其中一个。 如果只有一个次要副本响应预提升操作，则资源代理无法确保进行响应的次要副本具有最高的 sequence_number，从而不会触发故障转移。

> [!IMPORTANT]
> `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 为 0 时有数据丢失风险。 在主要副本中断期间，资源代理不会自动触发故障转移。 你可以等主要副本恢复，也可以使用 `FORCE_FAILOVER_ALLOW_DATA_LOSS` 手动进行故障转移。

你可以选择覆盖默认行为，并阻止可用性组资源自动设置 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT`。

以下脚本在名为 `<**ag1**>` 的可用性组上将 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 设置为 0。 运行前，将 `<**ag1**>` 替换为可用性组的名称。

```bash
sudo pcs resource update <**ag1**> required_synchronized_secondaries_to_commit=0
```

若要还原为默认值，请根据可用性组配置运行以下命令：

```bash
sudo pcs resource update <**ag1**> required_synchronized_secondaries_to_commit=
```

> [!NOTE]
> 运行上述命令时，主要副本会暂时降级为次要副本，然后再次提升。 资源更新会导致所有副本停止并重新启动。 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 的新值仅在重新启动副本后设置，而不是立即设置。

## <a name="see-also"></a>另请参阅

[Linux 上的可用性组](sql-server-linux-availability-group-overview.md)

<!--Image references-->
[1]: ./media/sql-server-linux-availability-group-ha/1-read-scale-out.png
[2]: ./media/sql-server-linux-availability-group-ha/2-configuration-only.png
[3]: ./media/sql-server-linux-availability-group-ha/3-three-replica.png
[4]: ./media/sql-server-linux-availability-group-ha/configuration-only-example.png
