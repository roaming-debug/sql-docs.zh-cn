---
title: Kubernetes 上的数据暂留
titleSuffix: SQL Server big data clusters
description: 了解永久性卷如何提供插件模型，来用于 Kubernetes 中的存储。 还可了解数据暂留如何在 SQL Server 2019 大数据群集中运行。
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a2836a6966727c0c2d27b363048f2e1dcd6afa49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052069"
---
# <a name="data-persistence-with-sql-server-big-data-cluster-in-kubernetes"></a>使用 Kubernetes 上的 SQL Server 大数据群集进行数据暂留

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

[永久性卷](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)提供了插件模型，可用于 Kubernetes 中的存储。 在此模型中，存储的提供方式是根据它的使用方式抽象而来。 因此，你可以提供自己的高可用性存储并将其插入到 SQL Server 大数据群集中。 这样一来，你就能完全控制所需的存储类型、可用性和性能。 Kubernetes 支持[各种存储解决方案](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner)，包括 Azure 磁盘和文件、网络文件系统 (NFS) 和本地存储。

## <a name="configure-persistent-volumes"></a>配置持久卷

SQL Server 大数据群集通过使用[存储类](https://kubernetes.io/docs/concepts/storage/storage-classes/)来使用这些永久性卷。 可以为不同类型的存储创建不同的存储类，并在部署大数据群集时指定它们。 可以在池一级配置存储类和永久性卷声明大小，以用于特定目的。 SQL Server 大数据群集通过对每个需要永久性卷的组件使用指定存储类名，创建[永久性卷声明](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims)。 然后，它将相应的永久性卷（或卷）装载到 Pod 中。 

在规划大数据群集的存储配置时，需要考虑以下几个重要方面：

- 为了实现成功的大数据群集部署，请确保你拥有所需数量的可用永久性卷。 若要在 Azure Kubernetes 服务 (AKS) 群集上部署，且使用的是内置存储类（`default` 或 `managed-premium`），那么此类支持对永久性卷进行动态预配。 因此，无需预先创建永久性卷，但必须确保 AKS 群集中可用的工作器节点可以附加与部署所需的永久性卷数量相同的磁盘。 根据为工作器节点指定的 [VM 大小](/azure/virtual-machines/linux/sizes)，每个节点可以附加一定数量的磁盘。 对于默认大小的群集（无高可用性），至少必须有 24 个磁盘。 若要启用高可用性或纵向扩展任何池，请确保每个附加副本至少有两个永久性卷，无论要纵向扩展的资源是什么。

- 如果在配置中提供的存储类的存储预配程序不支持动态预配，必须预先创建永久性卷。 例如，`local-storage` 预配程序不启用动态预配。 若要了解如何在使用 `kubeadm` 部署的 Kubernetes 群集中继续操作，请参阅此[示例脚本](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu)。

- 部署大数据群集时，可以配置相同的存储类，以供群集中的所有组件使用。 但生产部署的最佳做法是，各种组件需要不同的存储配置，以适应各种大小或吞吐量的工作负载。 可以覆盖控制器中为每个 SQL Server 主实例、数据集和存储池指定的默认存储配置。 本文提供了具体操作示例。

- 在计算存储池大小要求时，必须考虑 HDFS 配置的复制因子。  复制因子在部署时可在群集部署配置文件中配置。 开发测试配置文件（即 `aks-dev-test`或`kubeadm-dev-test`）的默认值是 2，对于我们为生产部署推荐的配置文件（即 `kubeadm-prod`），默认值是 3。 作为最佳做法，我们建议你将大数据集群的生产部署配置为至少 3 个 HDFS 的复制因子。 复制因子的值将影响存储池中的实例数量：至少必须部署与复制因子的值一样多的存储池实例。 此外，还必须相应地调整存储空间的大小，并将在 HDFS 中复制数据的次数调整为复制因子的值。 在[此处](https://hadoop.apache.org/docs/r3.2.1/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Data_Replication)可以找到更多关于 HDFS 数据复制的信息。 

- 从 SQL Server 2019 CU1 版本开始，BDC 不会在部署后更新存储配置设置。 有了此约束，你将无法使用 BDC 操作来修改每个实例永久性卷声明的大小，也无法在部署后缩放任何池。 因此，在部署大数据群集之前，请务必先计划存储布局。 但你可以直接使用 Kubernetes API 来扩展永久卷的大小。 在这种情况下，不会更新 BDC 元数据，并且你将在配置群集元数据中看到不一致的卷大小。

- 通过在 Kubernetes 上部署为容器化应用程序，并使用有状态集和永久性存储等功能，Kubernetes 确保在出现运行状况问题时重启 Pod，并将它们附加到同一永久性存储。 不过，如果出现节点故障，且必须在另一节点上重启 Pod，那么如果存储是故障节点的本地存储，就会增加不可用性风险。 为了降低这一风险，要么必须配置额外的冗余，并启用[高可用性功能](deployment-high-availability.md)，要么必须使用远程冗余存储。 下面概述了大数据群集中各种组件适用的存储选项：

| 资源 | 数据存储类型 | 日志存储类型 |  说明 |
|---|---|---|--|
| SQL Server 主实例 | 本地存储（副本＞＝3）或远程冗余存储（副本＝1） | 本地存储 | 基于有状态集的实现（其中 Pod 固定在节点上）将确保重启，暂时故障不会导致数据丢失。 |
| 计算池 | 本地存储 | 本地存储 | 未存储任何用户数据。 |
| 数据池 | 本地存储/远程冗余存储 | 本地存储 | 如果无法从其他数据源重建数据池，请使用远程冗余存储。  |
| 存储池 (HDFS) | 本地存储/远程冗余存储 | 本地存储 | 已启用 HDFS 复制。 |
| Spark 池 | 本地存储 | 本地存储 | 未存储任何用户数据。 |
| 控件（controldb、metricsd、logsdb）| 远程冗余存储 | 本地存储 | 对大数据群集元数据使用存储级冗余至关重要。 |

> [!IMPORTANT]
> 请确保与控制相关的组件位于远程冗余存储设备上。 `controldb-0` Pod 托管 SQL Server 实例，其中的数据库存储所有与群集服务状态、各种设置和其他相关信息相关的元数据。 如果此实例不可用，就会导致群集中的其他依赖服务出现运行状况问题。

## <a name="configure-big-data-cluster-storage-settings"></a>配置大数据群集存储设置

与其他自定义一样，可以在部署时为 `bdc.json` 配置文件中的每个池和 `control.json` 文件中的控制服务在群集配置文件中指定存储设置。 如果池规范中没有存储配置设置，控制存储设置将用于其他所有组件，包括 SQL Server 主实例（`master` 资源）、HDFS（`storage-0` 资源）和数据池组件。 下面的代码示例表示可以在规范中添加的存储配置部分：

```json
    "storage": 
    {
      "data": {
        "className": "default",
        "accessMode": "ReadWriteOnce",
        "size": "15Gi"
      },
      "logs": {
        "className": "default",
        "accessMode": "ReadWriteOnce",
        "size": "10Gi"
    }
```

大数据群集部署使用永久性存储来存储各种组件的数据、元数据和日志。 你可以自定义作为部署的一部分创建的持久卷声明的大小。 根据最佳做法，建议结合使用存储类和 Retain  [回收策略](https://kubernetes.io/docs/concepts/storage/storage-classes/#reclaim-policy)。

> [!WARNING]
> 在没有持久存储的情况下运行时可在测试环境中运行，但可能导致群集无法正常运行。 在 Pod 重启后，群集元数据和/或用户数据就会永久丢失。 建议不要在此配置下运行。

[配置存储](#config-samples)部分提供了更多有关如何为 SQL Server 大数据群集部署来配置存储设置的示例。

## <a name="aks-storage-classes"></a>AKS 存储类

AKS 随附[两个内置存储类](/azure/aks/azure-disks-dynamic-pv/)（`default` 和 `managed-premium`），以及这两个类的动态预配程序。 可以指定这两个类中的一个，也可以创建你自己的存储类，从而部署已启用永久性存储的大数据群集。 默认情况下，用于AKS 的内置群集配置文件 `aks-dev-test` 随附使用 `default` 存储类的永久性存储配置。

> [!WARNING]
> 使用内置存储类 `default` 和 `managed-premium` 创建的持久卷包含回收策略“删除”。 因此，如果你删除 SQL Server 大数据群集，永久性卷声明和永久性卷都会遭删除。 可通过结合使用 `azure-disk` 预配程序和 `Retain` 回收策略，创建自定义存储类，如[存储概念](/azure/aks/concepts-storage/#storage-classes)所述。

## <a name="storage-classes-for-kubeadm-clusters"></a>`kubeadm` 群集的存储类 

使用 `kubeadm` 部署的 Kubernetes 群集没有内置的存储类。 必须使用本地存储或[首选存储预配程序](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner)来创建你自己的存储类和永久性卷。 在这种情况下，将 `className` 设置为你配置的存储类。

> [!IMPORTANT]
>  在 kubeadm 的内置部署配置文件（`kubeadm-dev-test` 和 `kubeadm-prod`）中，没有为数据和日志存储指定存储类名。 部署前，必须先自定义配置文件，并设置 `className` 的值。 否则，预先部署验证就会失败。 部署还有一个验证步骤，用于检查存储类是否存在，但不是必需的持久卷。 请务必根据群集规模来创建足够的卷。 对于默认的最小群集大小（默认规模，无高可用性），必须创建至少 24 个持久卷。 有关如何使用本地预配程序来创建永久性卷的示例，请参阅[使用 Kubeadm 创建 Kubernetes 群集](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu)。

## <a name="customize-storage-configurations-for-each-pool"></a>自定义每个池的存储配置

对于所有自定义项，必须先创建要使用的内置配置文件的副本。 例如，下列命令在名为 `custom` 的子目录中创建 `aks-dev-test` 部署配置文件的副本：

```bash
azdata bdc config init --source aks-dev-test --target custom
```

此过程会创建两个文件（`bdc.json` 和 `control.json`），你可以自定义它们，具体方法是手动编辑它们，或使用 `azdata bdc config` 命令。 可以结合使用 jsonpath 和 jsonpatch 库以提供编辑配置文件的方法。


### <a name="configure-storage-class-name-andor-claims-size"></a><a id="config-samples"></a> 配置存储类名称和/或声明大小

默认情况下，对于群集中预配的每个 Pod，为之预配的永久性卷声明的大小为 10GB。 可以在群集部署之前更新此值，以适应要在自定义配置文件中运行的工作负载。

在下面的示例中，永久性卷声明的大小在 `control.json` 中更新为 32GB。 如果未在池一级被替代，此设置应用于所有池。

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.storage.data.size=100Gi"
```

下面的示例展示了如何修改 `control.json` 文件的存储类：

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.storage.data.className=<yourStorageClassName>"
```

也可以手动编辑自定义配置文件，或使用更改存储池的存储类的 .json 修补程序，如下面的示例所示：

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "$.spec.resources.storage-0.spec",
      "value": {
        "type":"Storage",
        "replicas":2,
        "storage":{
            "data":{
                    "size": "100Gi",
                    "className": "myStorageClass",
                    "accessMode":"ReadWriteOnce"
                    },
            "logs":{
                    "size":"32Gi",
                    "className":"myStorageClass",
                    "accessMode":"ReadWriteOnce"
                    }
                }
            }
        }
    ]
}
```

应用补丁文件。 使用 `azdata bdc config patch` 命令来应用 .json 修补程序文件中的更改。 下面的示例将 `patch.json` 文件应用于目标部署配置文件 `custom.json`：

```bash
azdata bdc config patch --config-file custom/bdc.json --patch-file ./patch.json
```

## <a name="next-steps"></a>后续步骤

- 有关 Kubernetes 中卷的完整文档，请参阅[有关卷的 Kubernetes文档](https://kubernetes.io/docs/concepts/storage/volumes/)。

- 有关部署 SQL Server 大数据群集的详细信息，请参阅[如何在 Kubernetes 上部署 SQL Server 大数据群集](deployment-guidance.md)。
