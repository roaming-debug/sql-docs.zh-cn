---
title: Kubernetes 疑难解答
titleSuffix: SQL Server big data clusters
description: 本文提供了用于监视 SQL Server 2019 大数据群集并对其进行故障排除的有用命令。
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3f0d8ba02c56ca625094829770771c053e31802e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039617"
---
# <a name="troubleshoot-big-data-clusters-2019-kubernetes"></a>[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Kubernetes 疑难解答

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍了几个用于监视 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 并对其进行故障排除的实用 Kubernetes 命令。 其中介绍了如何深入了解位于大数据群集中的 Pod 或其他 Kubernetes 项目的详细信息。 本文也涵盖了一些常见任务，例如将文件复制到运行某个 SQL Server 大数据群集服务的容器中或从中进行复制。

> [!TIP]
> 为了监视大数据群集组件的状态，可以使用 [azdata bdc status](deployment-guidance.md#status) 命令或 Azure Data Studio 附带的内置[排除故障笔记本](notebooks-manage-bdc.md)。

> [!TIP]
> 在 Windows（cmd 或 PS）或 Linux (bash) 客户端计算机上运行以下 kubectl 命令。 它们需要该群集中的上一身份验证以及一个用于运行该身份验证的群集上下文。 例如，对于先前创建的 AKS 群集，可以运行 `az aks get-credentials --name <aks_cluster_name> --resource-group <azure_resource_group_name>` 以下载 Kubernetes 群集配置文件并设置群集上下文。

## <a name="get-status-of-pods"></a>获取 Pod 的状态

可以使用 `kubectl get pods` 命令获取群集中 Pod 的状态，以获取所有命名空间或大数据群集命名空间。 以下部分演示了这两者的示例。

### <a name="show-status-of-all-pods-in-the-kubernetes-cluster"></a>显示 Kubernetes 群集中所有 Pod 的状态

运行以下命令以获取所有 Pod 及其状态，包括作为在其中创建 SQL Server 大数据群集 Pod 的命名空间的一部分的 Pod：

```bash
kubectl get pods --all-namespaces
```

### <a name="show-status-of-all-pods-in-the-sql-server-big-data-cluster"></a>显示 SQL Server 大数据群集中所有 Pod 的状态

`-n` 参数用于指定特定的命名空间。 请注意，SQL Server 大数据群集 Pod 是在群集启动时创建的新命名空间中创建的，该命名空间基于部署配置文件中指定的群集名称。 此处使用默认名称 `mssql-cluster`。

```bash
kubectl get pods -n mssql-cluster
```

对于正在运行的大数据群集，你应该看到类似于以下列表的输出：

```output
PS C:\> kubectl get pods -n mssql-cluster
NAME              READY   STATUS    RESTARTS   AGE
appproxy-f2qqt    2/2     Running   0          110m
compute-0-0       3/3     Running   0          110m
control-zlncl     4/4     Running   0          118m
data-0-0          3/3     Running   0          110m
data-0-1          3/3     Running   0          110m
gateway-0         2/2     Running   0          109m
logsdb-0          1/1     Running   0          112m
logsui-jtdnv      1/1     Running   0          112m
master-0          7/7     Running   0          110m
metricsdb-0       1/1     Running   0          112m
metricsdc-shv2f   1/1     Running   0          112m
metricsui-9bcj7   1/1     Running   0          112m
mgmtproxy-x6gcs   2/2     Running   0          112m
nmnode-0-0        1/1     Running   0          110m
storage-0-0       7/7     Running   0          110m
storage-0-1       7/7     Running   0          110m
```

> [!NOTE]
> 在部署期间，仍会出现状态为 ContainerCreating 的 Pod 。 如果部署因任何原因而挂起，可通过此状态了解问题所在。 另请参阅“READY”列。 这将告诉你已在 Pod 中启动的容器数量。 请注意，部署可能需要 30 分钟或更长时间，具体取决于你的配置和网络。 大部分时间花在了下载不同组件的容器映像上。

## <a name="get-pod-details"></a>获取 Pod 详细信息

运行以下命令以获取 JSON 格式输出中特定 Pod 的详细说明。 其中包括以下详细信息，例如，Pod 所在位置的当前 Kubernetes 节点、Pod 中运行的容器以及用于启动容器的映像。 其中也显示其他详细信息，例如，与 Pod 关联的标签、状态和持久性卷声明。

```bash
kubectl describe pod  <pod_name> -n <namespace_name>
```

下面的示例显示名为 `mssql-cluster` 的大数据群集中的 `master-0` Pod 的详细信息：

```bash
kubectl describe pod  master-0 -n mssql-cluster
```

如果发生任何错误，有时可以在该 Pod 的最近事件中看到错误。

## <a name="get-pod-logs"></a>获取 Pod 日志

可以检索在 Pod 中运行的容器的日志。 以下命令检索在名为 `master-0` 的 Pod 中运行的所有容器的日志，并将其输出到文件名 `master-0-pod-logs.txt`：

```bash
kubectl logs master-0 --all-containers=true -n mssql-cluster > master-0-pod-logs.txt
```

## <a name="get-status-of-services"></a><a id="services"></a> 获取服务的状态

运行以下命令以获取有关大数据群集服务的详细信息。 这些详细信息包括服务类型以及与相应服务和端口关联的 IP。 请注意，SQL Server 大数据群集服务是在群集启动时创建的新命名空间中创建的，该命名空间基于部署配置文件中指定的群集名称。

```bash
kubectl get svc -n <namespace_name>
```

下面的示例显示名为 `mssql-cluster` 的大数据群集中的服务状态：

```bash
kubectl get svc -n mssql-cluster
```

以下服务支持与大数据群集的外部建立连接：

| 服务 | 说明 |
|---|---|
| **master-svc-external** | 提供对主实例的访问权限。<br/>（EXTERNAL-IP、31433 和 SA 用户 ） |
| **controller-svc-external** | 支持管理群集的工具和客户端。 |
| **gateway-svc-external** | 提供对 HDFS/Spark 网关的访问权限。<br/>（EXTERNAL-IP 和 `<AZDATA_USERNAME>` 用户）<sup>1</sup>|
| **appproxy-svc-external** | 支持应用程序部署方案。 |

<sup>1</sup> [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

> [!TIP]
> 这是使用 kubectl 查看服务的一种方法，但也可以使用 `azdata bdc endpoint list` 命令查看这些终结点。 有关详细信息，请参阅[获取大数据群集终结点](deployment-guidance.md#endpoints)。

## <a name="get-service-details"></a>获取服务详细信息

运行此命令以获取 JSON 格式输出中的服务的详细说明。 其中包括标签、选择器、IP、外部 IP（如果服务为 LoadBalancer 类型）、端口等详细信息。

```bash
kubectl describe service <service_name> -n <namespace_name>
```

下面的示例检索 master-svc-external 服务的详细信息：

```bash
kubectl describe service master-svc-external -n mssql-cluster
```

## <a name="copy-files"></a><a id="copy"></a> 复制文件

如果需要将文件从容器复制到本地计算机，请使用带以下语法的命令 `kubectl cp`：

```bash
kubectl cp <pod_name>:<source_file_path> -c <container_name> -n <namespace_name> <target_local_file_path>
```

同样，可以使用 `kubectl cp` 以将文件从本地计算机复制到特定容器中。

```bash
kubectl cp <source_local_file_path> <pod_name>:<target_container_path> -c <container_name>  -n <namespace_name>
```

### <a name="copy-files-from-a-container"></a><a id="copyfrom"></a> 从容器复制文件

下面的示例将 SQL Server 日志文件从容器复制到本地计算机上的 `~/temp/sqlserverlogs` 路径（在本示例中，本地计算机是 Linux 客户端）：

```bash
kubectl cp master-0:/var/opt/mssql/log -c mssql-server -n mssql-cluster ~/tmp/sqlserverlogs
```

### <a name="copy-files-into-container"></a><a id="copyinto"></a> 将文件复制到容器中

下面的示例将“AdventureWorks2016CTP3.bak”文件从本地计算机复制到 `master-0` Pod 中的 SQL Server 主实例容器 (`mssql-server`)。 该文件将被复制到容器的 `/tmp` 目录中。 

```bash
kubectl cp ~/Downloads/AdventureWorks2016CTP3.bak master-0:/tmp -c mssql-server -n mssql-cluster
```

## <a name="force-delete-a-pod"></a><a id="forcedelete"></a> 强制删除 Pod
 
建议不要强制删除 Pod。 但是，为了测试可用性、复原能力或数据持久性，可以使用 `kubectl delete pods` 命令删除 Pod 以模拟 Pod 发生故障。

```bash
kubectl delete pods <pod_name> -n <namespace_name> --grace-period=0 --force
```

以下示例删除存储池 Pod `storage-0-0`：

```bash
kubectl delete pods storage-0-0 -n mssql-cluster --grace-period=0 --force
```

## <a name="get-pod-ip"></a><a id="getip"></a> 获取 Pod IP
 
出于故障排除目的，可能需要获取当前正在运行 Pod 的节点的 IP。 若要获取 IP 地址，请使用带有以下语法的 `kubectl get pods` 命令：

```bash
kubectl get pods <pod_name> -o yaml -n <namespace_name> | grep hostIP
```

下面的示例获取正在运行 `master-0` Pod 的节点的 IP 地址：

```bash
kubectl get pods master-0 -o yaml -n mssql-cluster | grep hostIP
```

## <a name="kubernetes-dashboard"></a>Kubernetes 仪表板

可以启动 Kubernetes 仪表板以获取有关群集的其他信息。 以下部分介绍如何使用 kubeadm 在 AKS 上启动 Kubernetes 仪表板以及启动 Kubernetes。
 
### <a name="start-dashboard-when-cluster-is-running-in-aks"></a>在 AKS 中运行群集时启动仪表板

要启动 Kubernetes 仪表板，请运行：

```bash
az aks browse --resource-group <azure_resource_group> --name <aks_cluster_name>
```

> [!Note]
> 如果收到以下错误信息：无法侦听端口 8001：由于以下错误，无法创建所有侦听器：无法创建侦听器：错误侦听 tcp4 127.0.0.1:8001:> 绑定：通常每个套接字地址只允许使用一次（协议/网络地址/端口）。无法创建侦听器：错误侦听 tcp6：地址 [[::1]]:8001：其中缺少端口 > 地址错误：无法侦听任何请求的端口 [{8001 9090}]，请确保没有从其他窗口中启动该仪表板。

在浏览器上启动仪表板时，由于默认情况下在 AKS 群集中启用 RBAC，因此你可能会收到权限警告，并且仪表板使用的服务帐户权限不足，无法访问所有资源（例如，已禁止 Pod：用户“system:serviceaccount:kube-system:kubernetes-dashboard”无法在命名空间“默认”中列出 Pod）。 运行以下命令，为 `kubernetes-dashboard` 提供所需的权限，然后重启仪表板：

```bash
kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

### <a name="start-dashboard-when-kubernetes-cluster-is-bootstrapped--using-kubeadm"></a>使用 kubeadm 启动 Kubernetes 群集时启动仪表板

有关如何在 Kubernetes 群集中部署和配置仪表板的详细说明，请参阅 [Kubernetes 文档](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)。 要启动 Kubernetes 仪表板，请运行此命令：

```bash
kubectl proxy
```

## <a name="next-steps"></a>后续步骤

有关大数据群集的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md)。
