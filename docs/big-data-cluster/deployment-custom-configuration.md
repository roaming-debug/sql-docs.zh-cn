---
title: 配置部署
titleSuffix: SQL Server big data clusters
description: 了解如何使用 azdata 管理工具中内置的配置文件自定义大数据群集部署。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d983b4d0d7cfb02a587675984fdc42c54bf9f0ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047187"
---
# <a name="configure-deployment-settings-for-cluster-resources-and-services"></a>为群集资源和服务配置部署设置

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

通过内置于 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 管理工具的一组预定义配置文件，你可以轻松地修改默认设置，以便更好地满足 BDC 工作负载要求。 借助配置文件的结构，可以精细地为资源的每项服务更新设置。

观看这段 13 分钟的视频，概览大数据群集配置：

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Big-Data-Cluster-Configuration/player?WT.mc_id=dataexposed-c9-niner]

> [!TIP]
> 请参考有关如何为任务关键组件（如 [SQL Server 主实例](deployment-high-availability.md)或 [HDFS 名称节点](deployment-high-availability-hdfs-spark.md)）配置高可用性的文章，以获取有关如何部署高可用性服务的详细信息  。

还可以设置资源级别配置或更新资源中所有服务的配置。 以下是 `bdc.json` 的结构摘要：

```json
{
    "apiVersion": "v1",
    "metadata": {
        "kind": "BigDataCluster",
        "name": "mssql-cluster"
    },
    "spec": {
        "resources": {
            "nmnode-0": {...
            },
            "sparkhead": {...
            },
            "zookeeper": {...
            },
            "gateway": {...
            },
            "appproxy": {...
            },
            "master": {...
            },
            "compute-0": {...
            },
            "data-0": {...
            },
            "storage-0": {...
        },
        "services": {
            "sql": {
                "resources": [
                    "master",
                    "compute-0",
                    "data-0",
                    "storage-0"
                ]
            },
            "hdfs": {
                "resources": [
                    "nmnode-0",
                    "zookeeper",
                    "storage-0",
                    "sparkhead"
                ],
                "settings": {...
            },
            "spark": {
                "resources": [
                    "sparkhead",
                    "storage-0"
                ],
                "settings": {...
            }
        }
    }
}
```

更新资源级别配置（如池中的实例）需要更新资源规格。例如，若要更新计算池中的实例数，则需要在 `bdc.json` 配置文件中修改此部分：

```json
"resources": {
    ...
    "compute-0": {
        "metadata": {
            "kind": "Pool",
            "name": "default"
        },
        "spec": {
            "type": "Compute",
            "replicas": 4
        }
    }
    ...
}
``` 

这与更改特定资源内单个服务的设置类似。 例如，如果只想更改存储池中 Spark 组件的 Spark 内存设置，则需要使用 `bdc.json` 配置文件中 `spark` 服务的 `settings` 部分来更新 `storage-0` 资源。

```json
"resources":{
    ...
     "storage-0": {
        "metadata": {
            "kind": "Pool",
            "name": "default"
        },
        "spec": {
            "type": "Storage",
            "replicas": 2,
            "settings": {
                "spark": {
                    "spark-defaults-conf.spark.driver.memory": "2g",
                    "spark-defaults-conf.spark.driver.cores": "1",
                    "spark-defaults-conf.spark.executor.instances": "3",
                    "spark-defaults-conf.spark.executor.memory": "1536m",
                    "spark-defaults-conf.spark.executor.cores": "1",
                    "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
                    "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
                    "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
                    "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
                    "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
                }
            }
        }
    }
    ...
}
```

若要对与多个资源关联的服务应用相同的配置，则需要更新 `services` 部分中的相应 `settings`。 例如，若要在存储池和 Spark 池中为 Spark 设置相同的设置，则需要更新 `bdc.json` 配置文件 `spark` 服务部分中的 `settings` 部分。

```json
"services": {
    ...
    "spark": {
        "resources": [
            "sparkhead",
            "storage-0"
        ],
        "settings": {
            "spark-defaults-conf.spark.driver.memory": "2g",
            "spark-defaults-conf.spark.driver.cores": "1",
            "spark-defaults-conf.spark.executor.instances": "3",
            "spark-defaults-conf.spark.executor.memory": "1536m",
            "spark-defaults-conf.spark.executor.cores": "1",
            "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
            "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
            "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
            "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
            "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
        }
    }
    ...
}
```

要自定义集群部署配置文件，可以使用任何 JSON 格式编辑器，例如 VSCode。 若是出于自动化目的为这些编辑内容编写脚本，请使用 `azdata bdc config` 命令。 本文介绍如何通过修改部署配置文件来配置大数据群集部署。 它提供了为不同方案更改配置的示例。 有关如何在部署中使用配置文件的详细信息，请参阅[部署指南](deployment-guidance.md#configfile)。

## <a name="prerequisites"></a>先决条件

- [安装 azdata](../azdata/install/deploy-install-azdata.md)。

- 本部分中的每个示例都假定已创建一个标准配置的副本。 有关详细信息，请参阅[创建自定义配置](deployment-guidance.md#customconfig)。 例如，以下命令根据默认的 `aks-dev-test` 配置，创建一个名为 `custom-bdc` 的目录，其中包含 `bdc.json` 和 `control.json` 两个 JSON 部署配置文件：

   ```bash
   azdata bdc config init --source aks-dev-test --target custom-bdc
   ```

## <a name="change-default-docker-registry-repository-and-images-tag"></a><a id="docker"></a> 更改默认 Docker 注册表、存储库和映像标记

内置配置文件（尤其是 control.json）包含 `docker` 部分，其中的容器注册表、存储库和映像标记已预先填充。 默认情况下，大数据群集所需的映像位于 `mssql/bdc` 存储库中的 Microsoft 容器注册表 (`mcr.microsoft.com`) 中：

```json
{
    "apiVersion": "v1",
    "metadata": {
        "kind": "Cluster",
        "name": "mssql-cluster"
    },
    "spec": {
        "docker": {
            "registry": "mcr.microsoft.com",
            "repository": "mssql/bdc",
            "imageTag": "2019-GDR1-ubuntu-16.04",
            "imagePullPolicy": "Always"
        },
        ...
    }
}
```

在部署之前，可以通过直接编辑 `control.json` 配置文件或使用 `azdata bdc config` 命令来自定义 `docker` 设置。 例如，以下命令使用不同的 `<registry>`、`<repository>` 和 `<image_tag>` 来更新 `custom-bdc` control.json 配置文件：

```bash
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.registry=<registry>"
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.repository=<repository>"
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.imageTag=<image_tag>"
```

> [!TIP]
> 最佳做法是，必须使用特定于版本的映像标记，并避免使用 `latest` 映像标记，因为这会导致版本不匹配，从而产生群集运行状况问题。

> [!TIP]
> 大数据群集部署必须有权访问从中拉取容器映像的容器注册表和存储库。 如果环境没有访问默认 Microsoft 容器注册表的权限，则可以执行脱机安装，其中所需的映像会首先放置在专用 Docker 存储库中。 有关脱机安装的详细信息，请参阅 [执行 SQL Server 大数据群集的脱机部署](deploy-offline.md)。 请注意，在签发部署前，必须先设置 `DOCKER_USERNAME` 和 `DOCKER_PASSWORD` [环境变量](deployment-guidance.md#env)，以确保部署工作流有权访问要从中拉取映像的专用存储库。

## <a name="change-cluster-name"></a><a id="clustername"></a> 更改群集名称

群集名称既是大数据群集的名称，也是将在部署时创建的 Kubernetes 命名空间的名称。 它在 `bdc.json` 部署配置文件的以下部分中指定：

```json
"metadata": {
    "kind": "BigDataCluster",
    "name": "mssql-cluster"
},
```

以下命令将键值对发送到 `--json-values` 参数，将大数据群集名称更改为 `test-cluster`：

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "metadata.name=test-cluster"
```

> [!IMPORTANT]
> 大数据群集的名称只能使用小写的字母数字字符，不能有空格。 将在与指定的群集名称同名的命名空间中创建群集的所有 Kubernetes 项目（容器、pod、状态集、服务）。

## <a name="update-endpoint-ports"></a><a id="ports"></a> 更新终结点端口

在 `control.json` 中为控制器定义终结点，在 `bdc.json` 的相应部分中为网关和 SQL Server 主实例定义终结点。 `control.json` 配置文件的以下部分显示了控制器的终结点定义：

```json
{
  "endpoints": [
    {
      "name": "Controller",
      "serviceType": "LoadBalancer",
      "port": 30080
    },
    {
      "name": "ServiceProxy",
      "serviceType": "LoadBalancer",
      "port": 30777
    }
  ]
}
```

以下示例使用内联 JSON 更改 `controller` 终结点的端口：

```bash
azdata bdc config replace --config-file custom-bdc/control.json --json-values "$.spec.endpoints[?(@.name==""Controller"")].port=30000"
```

## <a name="configure-scale"></a><a id="replicas"></a> 配置规模

每个资源（如存储池）的配置在 `bdc.json` 配置文件中定义。 例如，`bdc.json` 的以下部分显示 `storage-0` 资源定义：

```json
"storage-0": {
    "metadata": {
        "kind": "Pool",
        "name": "default"
    },
    "spec": {
        "type": "Storage",
        "replicas": 2,
        "settings": {
            "spark": {
                "spark-defaults-conf.spark.driver.memory": "2g",
                "spark-defaults-conf.spark.driver.cores": "1",
                "spark-defaults-conf.spark.executor.instances": "3",
                "spark-defaults-conf.spark.executor.memory": "1536m",
                "spark-defaults-conf.spark.executor.cores": "1",
                "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
                "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
                "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
                "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
                "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
            }
        }
    }
}
```

通过修改每个池的 `replicas` 值可以配置存储、计算和/或数据池中的实例数。 下面的示例使用内联 JSON 将存储、计算和数据池的这些值分别更改为 `10`、`4` 和 `4`：

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.storage-0.spec.replicas=10"
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.compute-0.spec.replicas=4"
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.data-0.spec.replicas=4"
```

> [!NOTE]
> 计算和数据池的有效实例上限为每个池 `8` 个实例。 虽然在部署时不会强制实施此限制，但不建议在生产部署中配置更高的规模。

## <a name="configure-storage"></a><a id="storage"></a> 配置存储

还可以更改用于每个池的存储类和特征。 以下示例将自定义存储类分配给存储池和数据池，并将用于存储数据的永久性卷声明的大小更新为 HDFS（存储池）500 Gb，master 和数据池 100 Gb。 

> [!TIP]
> 有关存储配置的详细信息，请参阅 [Kubernetes 上 SQL Server 大数据群集的数据暂留](concept-data-persistence.md)。

首先创建一个 patch.json 文件（如下所示）来调整存储设置

```json
{
        "patch": [
                {
                        "op": "add",
                        "path": "spec.resources.storage-0.spec.storage",
                        "value": {
                                "data": {
                                        "size": "500Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                },
        {
                        "op": "add",
                        "path": "spec.resources.master.spec.storage",
                        "value": {
                                "data": {
                                        "size": "100Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                },
                {
                        "op": "add",
                        "path": "spec.resources.data-0.spec.storage",
                        "value": {
                                "data": {
                                        "size": "100Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                }
        ]
}

```

然后，可以使用 `azdata bdc config patch` 命令更新 `bdc.json` 配置文件。
```bash
azdata bdc config patch --config-file custom-bdc/bdc.json --patch ./patch.json
```

> [!NOTE]
> 基于 `kubeadm-dev-test` 的配置文件不含每个池的存储定义，但可以根据需要使用上面的过程进行添加。

## <a name="configure-storage-pool-without-spark"></a><a id="sparkstorage"></a> 配置不使用 spark 的存储池

还可以将存储池配置为在没有 spark 的情况下运行，并创建单独的 spark 池。 利用此配置可以在扩展 spark 计算能力时不用考虑存储。 若要了解如何配置 Spark 池，请参阅本文中的 [创建 Spark 池](#sparkpool)部分。

> [!NOTE]
> 不支持在不使用 Spark 的情况下部署大数据群集。 因此，必须将 `includeSpark` 设置为 `true`，或者必须使用至少一个实例创建单独的 [Spark 池](#sparkpool)。 也可以在存储池中运行 Spark （`includeSpark` 为 `true`），同时创建一个单独的 Spark 池。

默认情况下，存储池资源的 `includeSpark` 设置设置为 true，因此必须将 `includeSpark` 字段编辑为存储配置才能进行更改。 以下命令显示如何使用内联 json 编辑此值。

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.storage-0.spec.settings.spark.includeSpark=false"
```

## <a name="create-a-spark-pool"></a><a id="sparkpool"></a> 创建 Spark 池

可以额外创建一个 Spark 池，而不是在存储池中运行的 Spark 实例。 以下示例显示如何通过修补 `bdc.json` 配置文件来创建具有两个实例的 Spark 池。 

首先，创建 `spark-pool-patch.json` 文件，如下所示：

```json
{
    "patch": [
        {
            "op": "add",
            "path": "spec.resources.spark-0",
            "value": {
                "metadata": {
                    "kind": "Pool",
                    "name": "default"
                },
                "spec": {
                    "type": "Spark",
                    "replicas": 2
                }
            }
        },
        {
            "op": "add",
            "path": "spec.services.spark.resources/-",
            "value": "spark-0"
        },
        {
            "op": "add",
            "path": "spec.services.hdfs.resources/-",
            "value": "spark-0"
        }
    ]
}
```

然后运行 `azdata bdc config patch` 命令：

```bash
azdata bdc config patch -c custom-bdc/bdc.json -p spark-pool-patch.json
```

## <a name="configure-pod-placement-using-kubernetes-labels"></a><a id="podplacement"></a> 使用 Kubernetes 标签配置 pod 布局

可以控制具有特定资源的 Kubernetes 节点上的 pod 放置，以适应各种类型的工作负载要求。 使用 Kubernetes 标签，可以自定义 Kubernetes 群集中的哪些节点将用于部署大数据群集资源，还可以限制哪些节点用于特定资源。
例如，你可能会想确保将存储池资源 pod 放置在具有更多存储的节点上，同时将 SQL Server 主实例放置在具有更高 CPU 和更多内存资源的节点上。 在这种情况下，需首先构建具有不同类型硬件的异类 Kubernetes 群集，然后相应[分配节点标签](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)。 部署大数据群集时，可使用 `control.json` 文件中的 `clusterLabel` 属性在群集级别指定相同的标签，以指示哪些节点用于大数据群集。 然后，将使用不同的标签用于池级别放置。 可使用 `nodeLabel` 属性在大数据群集部署配置文件中指定这些标签。 Kubernetes 在与指定标签匹配的节点上分配 pod。 需要添加到 kubernetes 群集中节点的特定标签键为 `mssql-cluster`（用于指示哪些节点用于大数据群集）和 `mssql-resource`（用于指示 pod 放置在哪些特定节点上以使用各种资源）。 这些标签的值可以是所选择的任何字符串。

> [!NOTE]
> 由于收集节点级指标的 pod 的性质，`metricsdc` pod 部署在所有带有 `mssql-cluster` 标签的节点上，而 `mssql-resource` 不适用于这些 pod。

以下示例展示如何编辑自定义配置文件，使其包含用于整个大数据群集的节点标签 `bdc`、用于在特定节点上放置 SQL Server 主实例 pod 的标签 `bdc-master`、用于存储池资源的 `bdc-storage-pool`、用于计算池和数据池 pod 的 `bdc-compute-pool` 以及用于资源其余部分的 `bdc-shared`。 

首先标记 Kubernetes 节点：

```bash
kubectl label node <kubernetesNodeName1> mssql-cluster=bdc mssql-resource=bdc-shared --overwrite=true
kubectl label node <kubernetesNodeName2> mssql-cluster=bdc mssql-resource=bdc-master --overwrite=true
kubectl label node <kubernetesNodeName3> mssql-cluster=bdc mssql-resource=bdc-compute-pool --overwrite=true
kubectl label node <kubernetesNodeName4> mssql-cluster=bdc mssql-resource=bdc-compute-pool --overwrite=true
kubectl label node <kubernetesNodeName5> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName6> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName7> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName8> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
```

然后更新群集部署配置文件以包括标签值。 本示例假设你要在 `custom-bdc` 配置文件中自定义配置文件。 默认情况下，内置配置中没有 `nodeLabel` 和 `clusterLabel` 键，因此需要手动编辑自定义配置文件或使用 `azdata bdc config add` 命令进行必要的编辑。

```bash
azdata bdc config add -c custom-bdc/control.json -j "$.spec.clusterLabel=bdc"
azdata bdc config add -c custom-bdc/control.json -j "$.spec.nodeLabel=bdc-shared"

azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.master.spec.nodeLabel=bdc-master"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.compute-0.spec.nodeLabel=bdc-compute-pool"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.data-0.spec.nodeLabel=bdc-compute-pool"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.storage-0.spec.nodeLabel=bdc-storage-pool"

# below can be omitted in which case we will take the node label default from the control.json
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.nmnode-0.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.sparkhead.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.zookeeper.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.gateway.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.appproxy.spec.nodeLabel=bdc-shared"
```
>[!NOTE]
> 最佳做法避免为 Kubernetes 主节点提供上述任何一个 BDC 角色。 如果仍计划将这些角色分配给 Kubernetes 主节点，则需要[删除其 ``master:NoSchedule`` 污点](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)。 请注意，这可能会使主节点过载，并导致无法在更大的群集上执行其 Kubernetes 管理任务。 在任何部署上向主节点计划一些 Pod 是正常的：它们已容忍 ``master:NoSchedule`` 污点，并且主要用于管理群集。 

## <a name="other-customizations-using-json-patch-files"></a><a id="jsonpatch"></a> 使用 JSON 修补程序文件的其他自定义

JSON 修补程序文件一次配置多个设置。 有关 JSON 修补程序的详细信息，请参阅 [python 中的 JSON 修补程序](https://github.com/stefankoegl/python-json-patch)和 [JSONPath 联机计算器](https://jsonpath.com/)。

下面的 `patch.json` 文件执行以下更改：

- 更新 `control.json` 中单个终结点的端口。

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "$.spec.endpoints[?(@.name=='Controller')].port",
      "value": 30000
    }
  ]
}
```

- 更新 `control.json` 中的所有终结点（`port` 和 `serviceType`）。

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.endpoints",
      "value": [
        {
          "serviceType": "LoadBalancer",
          "port": 30001,
          "name": "Controller"
        },
        {
          "serviceType": "LoadBalancer",
          "port": 30778,
          "name": "ServiceProxy"
        }
      ]
    }
  ]
}
```

- 更新 `control.json` 中的控制器存储设置。 除非在池级别重写，否则这些设置适用于所有群集组件。

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.storage",
      "value": {
        "data": {
          "className": "managed-premium",
          "accessMode": "ReadWriteOnce",
          "size": "100Gi"
        },
        "logs": {
          "className": "managed-premium",
          "accessMode": "ReadWriteOnce",
          "size": "32Gi"
        }
      }
    }
  ]
}
```

- 更新 `control.json` 中的存储类名称。

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.storage.data.className",
      "value": "managed-premium"
    }
  ]
}
```

- 更新 `bdc.json` 中存储池的池存储设置。

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.resources.storage-0.spec",
      "value": {
        "type": "Storage",
        "replicas": 2,
        "storage": {
          "data": {
            "size": "100Gi",
            "className": "myStorageClass",
            "accessMode": "ReadWriteOnce"
          },
          "logs": {
            "size": "32Gi",
            "className": "myStorageClass",
            "accessMode": "ReadWriteOnce"
          }
        }
      }
    }
  ]
}
```

- 更新 `bdc.json` 中存储池的 Spark 设置。

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.services.spark.settings",
      "value": {
            "spark-defaults-conf.spark.driver.memory": "2g",
            "spark-defaults-conf.spark.driver.cores": "1",
            "spark-defaults-conf.spark.executor.instances": "3",
            "spark-defaults-conf.spark.executor.memory": "1536m",
            "spark-defaults-conf.spark.executor.cores": "1",
            "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
            "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
            "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
            "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
            "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
      }
    }
  ]
}
```

> [!TIP]
> 有关部署配置文件的结构和用于更改该文件的选项的更多信息，请参阅[大数据群集的部署配置文件参考](reference-deployment-config.md)。

使用 `azdata bdc config` 命令应用 JSON 修补程序文件中的更改。 下面的示例将 `patch.json` 文件应用到目标部署配置文件 `custom-bdc/bdc.json`。

```bash
azdata bdc config patch --config-file custom-bdc/bdc.json --patch-file ./patch.json
```

## <a name="disable-elasticsearch-to-run-in-privileged-mode"></a>禁止 ElasticSearch 在特权模式下运行

默认情况下，ElasticSearch 容器在大数据群集的特权模式下运行。 此设置可确保在容器初始化时，当 ElasticSearch 处理较大数量的日志时，容器具有足够的权限在所需主机上更新设置。 在[本文](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html)中可以找到有关此主题的详细信息。 

若要禁止运行 ElasticSearch 的容器在特权模式下运行，则必须更新 `control.json` 中的 `settings` 部分，并将 `vm.max_map_count` 的值指定为 `-1`。 下面是此部分代码的示例：

```json
{
    "apiVersion": "v1",
    "metadata": {...},
    "spec": {
        "docker": {...},
        "storage": {...},
        "endpoints": [...],
        "settings": {
            "ElasticSearch": {
                "vm.max_map_count": "-1"
            }
        }
    }
}
```

可以手动编辑 `control.json`，并将上述部分添加到 `spec`，也可以创建如下所示的补丁文件 `elasticsearch-patch.json`，并使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 来修补 `control.json` 文件：

```json
{
  "patch": [
    {
      "op": "add",
      "path": "spec.settings",
      "value": {
            "ElasticSearch": {
                "vm.max_map_count": "-1"
        }
      }
    }
  ]
}
```

运行此命令可修补配置文件：

```bash
azdata bdc config patch --config-file custom-bdc/control.json --patch-file elasticsearch-patch.json
```

> [!IMPORTANT]
> 建议的最佳做法是按照[本文](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html)中的说明在 Kubernetes 集群中的每个主机上手动更新 `max_map_count` 设置。

## <a name="turn-pods-and-nodes-metrics-collection-onoff"></a>打开/关闭 pod 和节点指标集合

SQL Server 2019 CU5 启用两个功能开关来控制 pod 和节点指标的集合。 如果使用不同的解决方案来监视 Kubernetes 基础架构，可以通过在 control.json 部署配置文件中将 allowNodeMetricsCollection 和 allowPodMetricsCollection 设置为 false，来关闭 pod 和主机节点的内置指标集合   。 对于 OpenShift 环境，这些设置在内置部署配置文件中默认设置为 false，因为收集 pod 和节点指标需要特权功能。
运行此命令，以使用 azdata CLI 在自定义配置文件中更新这些设置的值：

```bash
 azdata bdc config replace -c custom-bdc/control.json -j "$.security.allowNodeMetricsCollection=false"
 azdata bdc config replace -c custom-bdc/control.json -j "$.security.allowPodMetricsCollection=false"
 ```

## <a name="next-steps"></a>后续步骤

有关在大数据群集部署中使用配置文件的详细信息，请参阅[如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md#configfile)。