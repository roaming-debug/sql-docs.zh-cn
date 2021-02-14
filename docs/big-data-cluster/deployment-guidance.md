---
title: 部署指南
titleSuffix: SQL Server Big Data Clusters
description: 了解如何在 Kubernetes 上部署 SQL Server 大数据群集。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d97c5e9b8c508387298cf00a8fbbe96ded1a2eea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047147"
---
# <a name="how-to-deploy-big-data-clusters-2019-on-kubernetes"></a>如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

SQL Server 大数据群集在 Kubernetes 群集上部署为 docker 容器。 下面概述了设置和配置步骤：

- 在 Azure Kubernetes 服务 (AKS)、Red Hat OpenShift 或 Azure Red Hat OpenShift (ARO) 中的单个 VM、VM 群集上设置 Kubernetes 群集。
- 在自己的客户端计算机上安装群集配置工具 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]。
- 在 Kubernetes 群集中部署 SQL Server 大数据群集。

## <a name="supported-platforms"></a>支持的平台

有关经过验证可用于部署 SQL Server 大数据群集的各种 Kubernetes 平台的完整列表，请参阅[受支持的平台](release-notes-big-data-cluster.md#supported-platforms)。

### <a name="sql-server-editions"></a>SQL Server 版本

|版本|说明|
|---------|---------|
|Enterprise<br/>Standard<br/>开发人员| 大数据群集版本由 SQL Server 主实例的版本确定。 在部署时会默认部署开发人员版。 可在部署后更改版本。 请参阅[配置 SQL Server 主实例](./configure-sql-server-master-instance.md)。 |

## <a name="kubernetes"></a><a id="prereqs"></a> Kubernetes

### <a name="kubernetes-cluster-setup"></a><a id="kubernetes"></a> Kubernetes 群集设置

如果已拥有满足上述必备条件的 Kubernetes 群集，则可以直接跳至[部署步骤](#deploy)。 本节假定你对 Kubernetes 概念有基本的了解。  有关 Kubernetes 的详细信息，请参阅 [Kubernetes 文档](https://kubernetes.io/docs/home)。

可以选择通过以下方式部署 Kubernetes：

| 部署 Kubernetes 的位置： | 说明 | 链接 |
|---|---|---|
| **Azure Kubernetes 服务 (AKS)** | Azure 中的托管 Kubernetes 容器服务。 | [说明](deploy-on-aks.md) |
| 一台或多台计算机 (`kubeadm`) | 在使用 `kubeadm` 的物理计算机或虚拟机上部署的 Kubernetes 群集 | [说明](deploy-with-kubeadm.md) |
|**Azure Red Hat OpenShift** | 在 Azure 中运行的 OpenShift 托管产品/服务。 | [说明](deploy-openshift.md)|
|**Red Hat OpenShift**|混合云企业 Kubernetes 应用程序平台。| [说明](deploy-openshift.md)|

> [!TIP]
> 还可以将 AKS 和大数据群集的部署脚本编写成一个步骤。 有关详细信息，请参阅 [python 脚本](quickstart-big-data-cluster-deploy.md)或 Azure Data Studio [笔记本](notebooks-deploy.md)，了解如何实现此操作。

### <a name="verify-kubernetes-configuration"></a>验证 Kubernetes 配置

运行 `kubectl` 命令以查看群集配置。 确保 kubectl 指向正确的群集上下文。

```bash
kubectl config view
```

> [!Important] 
> 如果要在使用 `kubeadm` 启动的多节点 Kuberntes 群集上进行部署，在启动大数据群集部署之前，请确保部署所针对的所有 Kubernetes 节点上的时钟同步。 大数据群集具有各种时间敏感型服务的内置运行状况属性，并且时钟偏差可能导致不正确的状态。

配置 Kubernetes 群集后，可以继续部署新的 SQL Server 大数据群集。 如果要从以前的版本升级，请参阅[如何升级 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-upgrade.md)。

## <a name="ensure-you-have-storage-configured"></a>确保已配置存储

大多数大数据群集部署应具有永久存储。 此时，需要在在部署 BDC 之前制定计划，确定如何在 Kubernetes 群集上提供永久存储。

如果在 AKS 中进行部署，则无需进行任何存储设置。 AKS 通过动态预配提供内置的存储类。 可以在部署配置文件中自定义存储类（`default` 或 `managed-premium`）。 内置配置文件使用 `default` 存储类。 如果要在使用 `kubeadm` 部署的 Kubernetes 群集上进行部署，则需要确保有足够的存储来存储所需规模的群集，并对其进行配置以供使用。 如果想要自定义存储的使用方式，应该在继续操作之前执行此操作。 请参阅 [Kubernetes 上的 SQL Server 大数据群集的数据暂留](concept-data-persistence.md)。

## <a name="install-sql-server-2019-big-data-tools"></a>安装 SQL Server 2019 大数据工具

请先[安装大数据工具](deploy-big-data-tools.md)，然后再部署 SQL Server 2019 大数据群集：

- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]
- `kubectl`
- Azure Data Studio
- Azure Data Studio 的[数据虚拟化扩展](../azure-data-studio/extensions/data-virtualization-extension.md)


## <a name="deployment-overview"></a><a id="deploy"></a> 部署概述

大多数大数据群集设置都在 JSON 部署配置文件中定义。 在设置期间可以为使用 `kubeadm` 创建的 AKS 和 Kubernetes 群集使用默认部署配置文件，也可以自定义自己的部署配置文件。 出于安全原因，身份验证设置通过环境变量传递。

以下部分详细介绍了如何配置大数据群集部署并提供了常见自定义示例。 此外，你始终可以使用 VS Code 等编辑器编辑自定义部署配置文件。

## <a name="default-configurations"></a><a id="configfile"></a> 默认配置

JSON 配置文件中定义了大数据群集部署选项。 可以从 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 中提供的内置部署配置文件开始自定义群集部署。 

> [!NOTE]
> 大数据群集部署所需的容器映像在 `mssql/bdc` 存储库中的 Microsoft 容器注册表 (`mcr.microsoft.com`) 上托管。 默认情况下，这些设置已包含在 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 附带的每个部署配置文件的 `control.json` 配置文件中。 此外，每个版本的容器映像标记也在相同的配置文件中进行预填充。 如果需要将容器映像提取到自己的专用容器注册表中，或修改容器注册表/存储库设置，请按照[脱机安装](deploy-offline.md)一文中的说明进行操作

运行此命令以查找可用模板：

```
azdata bdc config list -o table 
```

从 SQL Server 2019 CU5 开始，以下模板可用： 

| 部署配置文件 | Kubernetes 环境 |
|---|---|
| `aks-dev-test` | 在 Azure Kubernetes 服务 (AKS) 上部署 SQL Server 大数据群集|
| `aks-dev-test-ha` | 在 Azure Kubernetes 服务 (AKS) 上部署 SQL Server 大数据群集。 配置任务关键型服务（如 SQL Server 主实例和 HDFS 名称节点）以实现高可用性。|
| `aro-dev-test`|在 Azure Red Hat OpenShift 上部署 SQL Server 大数据群集，以进行开发和测试。 <br/><br/>在 SQL Server 2019 CU5 中引入。|
| `aro-dev-test-ha`|在 Red Hat OpenShift 群集上部署具有高可用性的 SQL Server 大数据群集，以进行开发和测试。 <br/><br/>在 SQL Server 2019 CU5 中引入。|
| `kubeadm-dev-test` | 使用单个或多个物理计算机或虚拟机在使用 kubeadm 创建的 Kubernetes 群集上部署 SQL Server 大数据群集。|
| `kubeadm-prod`| 使用单个或多个物理计算机或虚拟机在使用 kubeadm 创建的 Kubernetes 群集上部署 SQL Server 大数据群集。 使用此模板使大数据群集服务与 Active Directory 集成。 任务关键型服务（如 SQL Server 主实例和 HDFS 名称节点）通过高可用配置部署。  |
| `openshift-dev-test`|在 Red Hat OpenShift 群集上部署 SQL Server 大数据群集，以进行开发和测试。 <br/><br/>在 SQL Server 2019 CU5 中引入。|
| `openshift-prod`|在 Red Hat OpenShift 群集上部署具有高可用性的 SQL Server 大数据群集。 <br/><br/>在 SQL Server 2019 CU5 中引入。|

可以通过运行 `azdata bdc create` 部署大数据群集。 此操作会提示你选择其中某个默认配置，然后指导你完成部署。

第一次运行 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 时，必须包含 `--accept-eula=yes` 才能接受最终用户许可协议 (EULA)。

```bash
azdata bdc create --accept-eula=yes
```

在这种情况下，系统会提示你输入不属于默认配置（如密码）的任何设置。 

> [!IMPORTANT]
> 大数据群集的默认名称为 `mssql-cluster`。 要运行任何使用 `-n` 参数指定 Kubernetes 命名空间的 `kubectl` 命令，必须了解这一点。

## <a name="custom-configurations"></a><a id="customconfig"></a> 自定义配置

还可以自定义部署以适应计划运行的工作负载。 不能在部署后更改大数据群集服务的规模（副本数）或存储设置，因此，必须仔细规划部署配置以避免容量问题。 要自定义部署，请按以下步骤操作：

1. 首先从与 Kubernetes 环境匹配的标准部署配置文件开始。 可以使用 `azdata bdc config list` 命令来列出它们：

   ```bash
   azdata bdc config list
   ```

1. 要自定义部署，请使用 `azdata bdc config init` 命令创建部署配置文件的副本。 例如，下列命令在名为 `custom` 的目标目录中创建 `aks-dev-test` 部署配置文件的副本：

   ```bash
   azdata bdc config init --source aks-dev-test --target custom
   ```

   >[!TIP]
   >`--target` 根据 `--source` 参数指定包含配置文件、`bdc.json` 和 `control.json` 的目录。

1. 若要自定义部署配置文件中的设置，可以在适用于编辑 VS Code 等 JSON 文件的工具中编辑部署配置文件。 对于脚本式自动化，也可以使用 `azdata bdc config` 命令编辑自定义部署配置文件。 例如，以下命令更改自定义部署配置文件，将部署群集的名称从默认 (`mssql-cluster`) 更改为 `test-cluster`：  

   ```bash
   azdata bdc config replace --config-file custom/bdc.json --json-values "metadata.name=test-cluster"
   ```

   > [!TIP]
   > 也可以通过在 azdata create bdc 命令中使用 --name 参数，在部署时传入群集名称 。 命令中的参数优先于配置文件中的值。
   >
   > 用于查找 JSON 路径的实用工具是 [JSONPath Online Evaluator](https://jsonpath.com/)。
   >
   除了传递键值对之外，还可以提供内联 JSON 值或传递 JSON 补丁文件。 有关详细信息，请参阅[配置大数据群集的部署设置](deployment-custom-configuration.md)。

1. 将自定义配置文件传递给 `azdata bdc create`。 请注意，必须设置所需的[环境变量](#env)，否则终端将提示输入相应值：

   ```bash
   azdata bdc create --config-profile custom --accept-eula yes
   ```

> 有关部署配置文件结构的详细信息，请参阅[部署配置文件参考](reference-deployment-config.md)。 有关更多配置示例，请参阅[配置大数据群集的部署设置](deployment-custom-configuration.md)。

## <a name="environment-variables"></a><a id="env"></a> 环境变量

以下环境变量用于未存储在部署配置文件中的安全设置。 请注意，可以在配置文件中设置除凭据之外的 Docker 设置。

| 环境变量 | 要求 |说明 |
|---|---|---|
| `AZDATA_USERNAME` | 必须 |SQL Server 大数据群集管理员的用户名。 具有相同名称的 sysadmin 登录名在 SQL Server 主实例中创建。 最佳安全做法是禁用 `sa` 帐户。 <br/><br/>[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]|
| `AZDATA_PASSWORD` | 必选 |上面创建的用户帐户的密码。 在 SQL Server 2019 CU5 之前部署的群集上，`root` 用户使用相同的密码来保护 Knox 网关和 HDFS。 |
| `ACCEPT_EULA`| 首次使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 时为必需项| 设置为“是”。 设置为环境变量时，它将 EULA 同时应用于 SQL Server 和 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]。 如果未设置为环境变量，则可以在第一次使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 命令时将 `--accept-eula=yes` 包含在内。|
| `DOCKER_USERNAME` | 可选 | 当容器映像存储在专用存储库中时，用于访问容器映像的用户名。 有关如何使用专用 Docker 存储库部署大数据群集的更多详细信息，请参阅[脱机部署](deploy-offline.md)主题。|
| `DOCKER_PASSWORD` | 可选 |用于访问上述专用存储库的密码。 |

必须先设置这些环境变量，才能调用 `azdata bdc create`。 如果未设置任何变量，则系统会提示输入变量。

以下示例介绍如何设置适用于 Linux (bash) 和 Windows (PowerShell) 的环境变量：

```bash
export AZDATA_USERNAME=admin
export AZDATA_PASSWORD=<password>
export ACCEPT_EULA=yes
```

```PowerShell
SET AZDATA_USERNAME=admin
SET AZDATA_PASSWORD=<password>
```

> [!NOTE]
> 在 SQL Server 2019 CU5 之前部署的群集上，必须以 `root` 用户身份使用上述密码来保护 Knox 网关。 `root` 是在此基本身份验证（用户名/密码）中唯一受支持的用户。
> [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]
> 若要通过基本身份验证连接到 SQL Server，请使用与 AZDATA_USERNAME 和 AZDATA_PASSWORD [环境变量](#env)相同的值。 

设置环境变量后，必须运行 `azdata bdc create` 才能触发部署。 本示例使用上面创建的群集配置文件：

```bash
azdata bdc create --config-profile custom --accept-eula yes
```

请注意以下原则：

- 如果密码中含有任何特殊字符，请确保使用双引号将密码括起来。 可以将 `AZDATA_PASSWORD` 设置为自己喜欢的任意形式，但请确保密码足够复杂，且请勿使用 `!`、`&` 或 `'` 字符。 请注意，双引号分隔符仅适用于 bash 命令。
- `AZDATA_USERNAME` 登录名是安装过程中在 SQL Server 主实例上创建的系统管理员。 创建 SQL Server 容器后，通过在容器中运行 `echo $AZDATA_PASSWORD`，可发现指定的 `AZDATA_PASSWORD` 环境变量。 出于安全目的，最好更改密码。

## <a name="unattended-install"></a><a id="unattended"></a> 无人参与安装

对于无人参与的部署，必须设置所有所需的环境变量，使用配置文件，并使用 `--accept-eula yes` 参数调用 `azdata bdc create` 命令。 上述部分中的示例介绍了无人参与安装的语法。

## <a name="monitor-the-deployment"></a><a id="monitor"></a> 监视部署

在群集启动过程中，客户端命令窗口返回部署状态。 在部署过程中，应该看到一系列消息正在等待控制器 Pod：

```output
Waiting for cluster controller to start.
```

15 到 30 分钟后，你应该会收到控制器 Pod 正在运行的消息：

```output
Cluster controller endpoint is available at 11.111.111.11:30080.
Cluster control plane is ready.
```

> [!IMPORTANT]
> 由于下载大数据群集组件的容器映像需要一定的时间，整个部署可能需要很长时间。 但是，这个过程应不会长达几个小时。 有关部署问题，请参阅[监视 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]并对其进行故障排除](cluster-troubleshooting-commands.md)。

部署完成后，输出会通知你成功：

```output
Cluster deployed successfully.
```

> [!TIP]
> 除非通过自定义配置进行修改，否则部署的大数据群集的默认名称为 `mssql-cluster`。

## <a name="retrieve-endpoints"></a><a id="endpoints"></a> 检索终结点

部署脚本成功完成后，可以使用以下步骤获取大数据群集的外部终结点的地址。

1. 部署完成后，可以从部署标准输出中查找控制器终结点的 IP 地址，也可以通过查看以下 `kubectl` 命令的 EXTERNAL-IP 输出来查找：

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

   > [!TIP]
   > 如果在部署过程中未更改默认名称，请在上一个命令中使用 `-n mssql-cluster`。 `mssql-cluster` 是大数据群集的默认名称。

1. 使用 [azdata login](../azdata/reference/reference-azdata.md) 登录大数据群集。 将 `--endpoint` 参数设置为控制器终结点的外部 IP 地址。

   ```bash
   azdata login --endpoint https://<ip-address-of-controller-svc-external>:30080 --username <user-name>
   ```

   指定在部署过程中为大数据群集管理员配置的用户名和密码（AZDATA_USERNAME 和 AZDATA_PASSWORD）。

   > [!TIP]
   > 如果你是 Kubernetes 群集管理员并且有权访问群集配置文件（kube 配置文件），则可以将当前上下文配置为指向目标 Kubernetes 群集。 在这种情况下，可以使用 `azdata login -n <namespaceName>` 登录，其中 `namespace` 是大数据群集名称。 如果登录命令中未指定凭据，则系统会提示输入凭据。
   
1. 运行 [azdata bdc endpoint list](../azdata/reference/reference-azdata-bdc-endpoint.md) 以获取一个列表，其中包含每个终结点的描述及其对应的 IP 地址和端口值。 

   ```bash
   azdata bdc endpoint list -o table
   ```

   以下列表显示了此命令的示例输出：

   ```output
   Description                                             Endpoint                                                   Ip              Name               Port    Protocol
   ------------------------------------------------------  ---------------------------------------------------------  --------------  -----------------  ------  ----------
   Gateway to access HDFS files, Spark                     https://11.111.111.111:30443                               11.111.111.111  gateway            30443   https
   Spark Jobs Management and Monitoring Dashboard          https://11.111.111.111:30443/gateway/default/sparkhistory  11.111.111.111  spark-history      30443   https
   Spark Diagnostics and Monitoring Dashboard              https://11.111.111.111:30443/gateway/default/yarn          11.111.111.111  yarn-ui            30443   https
   Application Proxy                                       https://11.111.111.111:30778                               11.111.111.111  app-proxy          30778   https
   Management Proxy                                        https://11.111.111.111:30777                               11.111.111.111  mgmtproxy          30777   https
   Log Search Dashboard                                    https://11.111.111.111:30777/kibana                        11.111.111.111  logsui             30777   https
   Metrics Dashboard                                       https://11.111.111.111:30777/grafana                       11.111.111.111  metricsui          30777   https
   Cluster Management Service                              https://11.111.111.111:30080                               11.111.111.111  controller         30080   https
   SQL Server Master Instance Front-End                    11.111.111.111,31433                                       11.111.111.111  sql-server-master  31433   tcp
   HDFS File System Proxy                                  https://11.111.111.111:30443/gateway/default/webhdfs/v1    11.111.111.111  webhdfs            30443   https
   Proxy for running Spark statements, jobs, applications  https://11.111.111.111:30443/gateway/default/livy/v1       11.111.111.111  livy               30443   https
   ```

也可以通过运行以下 `kubectl` 命令获取为群集部署的所有服务终结点：

```bash
kubectl get svc -n <your-big-data-cluster-name>
```

## <a name="verify-the-cluster-status"></a><a id="status"></a> 验证群集状态

部署完成后，可以通过 [azdata bdc status show](../azdata/reference/reference-azdata-bdc-status.md) 命令检查群集的状态。

```bash
azdata bdc status show
```

> [!TIP]
> 要运行状态命令，必须先用前面的终结点部分中所示的 `azdata login` 命令登录。

以下显示了此命令的示例输出：

```output
Bdc: ready                                                                                                                                                                                                          Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Services: ready                                                                                                                                                                                                     Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Servicename    State    Healthstatus    Details

 sql            ready    healthy         -
 hdfs           ready    healthy         -
 spark          ready    healthy         -
 control        ready    healthy         -
 gateway        ready    healthy         -
 app            ready    healthy         -


 Sql Services: ready                                                                                                                                                                                                 Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 master          ready    healthy         StatefulSet master is healthy
 compute-0       ready    healthy         StatefulSet compute-0 is healthy
 data-0          ready    healthy         StatefulSet data-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy


 Hdfs Services: ready                                                                                                                                                                                                Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 nmnode-0        ready    healthy         StatefulSet nmnode-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy
 sparkhead       ready    healthy         StatefulSet sparkhead is healthy


 Spark Services: ready                                                                                                                                                                                               Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 sparkhead       ready    healthy         StatefulSet sparkhead is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy


 Control Services: ready                                                                                                                                                                                             Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 controldb       ready    healthy         -
 control         ready    healthy         -
 metricsdc       ready    healthy         DaemonSet metricsdc is healthy
 metricsui       ready    healthy         ReplicaSet metricsui is healthy
 metricsdb       ready    healthy         StatefulSet metricsdb is healthy
 logsui          ready    healthy         ReplicaSet logsui is healthy
 logsdb          ready    healthy         StatefulSet logsdb is healthy
 mgmtproxy       ready    healthy         ReplicaSet mgmtproxy is healthy


 Gateway Services: ready                                                                                                                                                                                             Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 gateway         ready    healthy         StatefulSet gateway is healthy


 App Services: ready                                                                                                                                                                                                 Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 appproxy        ready    healthy         ReplicaSet appproxy is healthy
```

还可以使用以下命令获取更详细的状态：

- [azdata bdc control status show](../azdata/reference/reference-azdata-bdc-control-status.md) 返回与控制管理服务关联的所有组件的运行状况状态
```
azdata bdc control status show
```
示例输出：
```output
Control: ready                                                                                                                                                                                                      Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Resources: ready                                                                                                                                                                                                    Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 controldb       ready    healthy         -
 control         ready    healthy         -
 metricsdc       ready    healthy         DaemonSet metricsdc is healthy
 metricsui       ready    healthy         ReplicaSet metricsui is healthy
 metricsdb       ready    healthy         StatefulSet metricsdb is healthy
 logsui          ready    healthy         ReplicaSet logsui is healthy
 logsdb          ready    healthy         StatefulSet logsdb is healthy
 mgmtproxy       ready    healthy         ReplicaSet mgmtproxy is healthy
```

- `azdata bdc sql status show` 返回具有 SQL Server 服务的所有资源的运行状况状态
```
azdata bdc sql status show
```
示例输出：
```output
Sql: ready                                                                                                                                                                                                          Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Resources: ready                                                                                                                                                                                                    Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 master          ready    healthy         StatefulSet master is healthy
 compute-0       ready    healthy         StatefulSet compute-0 is healthy
 data-0          ready    healthy         StatefulSet data-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy
```

> [!IMPORTANT]
> 使用 `--all` 参数时，这些命令的输出包含 Kibana 和 Grafana 仪表板的 URL，可用于更详细的分析。

除了使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 之外，还可以使用 Azure Data Studio 查找终结点和状态信息。 有关通过 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 和 Azure Data Studio 查看群集状态的详细信息，请参阅[如何查看大数据群集的状态](view-cluster-status.md)。

## <a name="connect-to-the-cluster"></a><a id="connect"></a> 连接到群集

有关如何连接到大数据群集的详细信息，请参阅[使用 Azure Data Studio 连接到 SQL Server 大数据群集](connect-to-big-data-cluster.md)。

## <a name="next-steps"></a>后续步骤

若要详细了解大数据群集部署，请参阅下列资源：

- [配置大数据群集的部署设置](deployment-custom-configuration.md)
- [执行 SQL Server 大数据群集的脱机部署](deploy-offline.md)
- [Workshop:Microsoft [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 体系结构](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)