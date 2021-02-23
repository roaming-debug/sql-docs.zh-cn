---
title: 已部署的资源
titleSuffix: SQL Server Big Data Clusters
description: 通常部署在 SQL Server 大数据群集中的 Pod 的说明。
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 20f31d6cbf4bcdbbdc666b50dea626f79e298db1
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343937"
---
# <a name="resources-deployed-with-big-data-cluster"></a>使用大数据群集部署的资源

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍了 SQL Server 大数据集部署的资源。

大数据群集根据部署配置文件部署 Pod。 有关详细信息，请参阅[默认配置](deployment-guidance.md#configfile)。 

本文介绍了使用 `aks-dev-test-ha` 配置文件部署的 Pod，并包括 Spark 池。 查询 Kubernetes，以查看群集中部署的 Pod。 下面的示例返回特定命名空间下的 Pod 列表。

```bash
kubectl get pods -n <namespace>
```

将 `<namespace>` 替换为大数据群集的名称。 

有关详细信息，请参阅[如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md#configfile)。

下图显示了大数据集中部署的组件：

:::image type="content" source="media/big-data-cluster-overview/architecture-diagram-overview.png" alt-text="big-data-cluster-diagram":::

有关体系结构的信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]？](big-data-cluster-overview.md)。

## <a name="deployed-pods"></a>已部署的 Pod

下表列出了部署在大数据群集中的 Pod。

|名称  |区域|
|---------|---------|
|`control-<nnnn>`        |[控制](#control)|
|`controldb-<#>`         |[控制](#control)|
|`controlwd-<nnnn>`      |[控制](#control)|
|`logsdb-<#>`            |[控制](#control)|
|`logsui-<nnnn>`         |[控制](#control)|
|`metricsdb-<#>`         |[控制](#control)|
|`metricsdc-<nnnn>`      |[控制](#control)|
|`metricsui-<nnnn>`      |[控制](#control)|
|`mgmtproxy-<nnnn>`      |[控制](#control)|
|`zookeeper-<#>`         |[控制](#control)|
|`dns-<nnnn>`            |[控制](#control)|
|`master-<#n>`           |[主实例](#master-instance)|
|`operator-<nnnn>`       |[主实例](#master-instance)
|`compute-<#n>-<#m>`     |[计算池](#compute-pool)|
|`data-<#>-<#>`          |[数据池](#data-pool) |
|`storage-<#>-<#>`       |[存储池](#storage-pool)|
|`nmnode-<#>-<#>`        |[存储池](#storage-pool)|
|`sparkhead-<#>`         |[存储池](#storage-pool)|
|`appproxy-<#m>`         |[应用程序池](#application-pool)|
|`gateway-<#>`           |[网关服务](#gateway-service)|

并非每个 BDC 群集中都包含所有 Pod。 具有高可用性的部署或 Active Directory 集成包含特定的 Pod。 

### <a name="high-availability-specific-pods"></a>高可用性特定的 Pod：

- `operator-<nnnn>`
- `zookeeper-<#>`

### <a name="active-directory-specific-pods"></a>Active Directory 特定的 Pod：

- `dns-<nnnn>`

以下部分介绍了 Pod，并列出每个 Pod 中的容器。

## <a name="control"></a>控制

控制 Pod 提供控制服务。

|Pod 名称 |Count| Kubernetes 控制器类型 | 容器 |
|--------|----|------|--------|-------|
|`control-#`|1| ReplicaSet |- `controller`<br><br>- `security-support`<br><br>- `fluentbit`
|`controldb`|1| StatefulSet |- `mssql-server`<br><br>- `fluentbit`
|`controlwd`|1|  ReplicaSet |- `controlwatchdog`
|`logsdb-#` |1| StatefulSet |- `elasticsearch`
|`logsui`   |1| ReplicaSet |- `kibana`
|`metricsdb-#`|1| StatefulSet |- `influxdb`
|`metricsdc`|每个 Kubernetes 节点 1 个。 | DaemonSet |- `telegraf` |
|`metricsui-nnnn`|1| ReplicaSet |- `grafana` |
|`mgmtproxy-nnnn`|1| ReplicaSet |- `service-proxy`<br><br>- `fluentbit`|
|`dns-nnnn`|Active Directory 集成 0 或 1 个| ReplicaSet |- `dns`<br><br>- `fluentbit`|

## <a name="master-instance"></a>主实例

`master-<#n>` 是 SQL Server 主实例。

- 通过 DDL 管理数据池
- 通过 DML 操作数据池中的数据
- 将分析查询执行卸载到数据池

|Pod 名称 |Count| Kubernetes 控制器类型 | 容器 |
|--------|----|------|--------|
|`master-<#n>`|高可用性 1 个或更多。| StatefulSet|- `mssql-server`<br><br>- `fluentbit`<br><br>- `collectd`<br><br>- `mssql-ha-supervisor` <sup>*</sup>|
|`operator`<sup>*</sup>| 高可用性 0 或 1 个 | ReplicaSet |- `mssql-ha-operator`

<sup>*</sup> 仅限高可用性部署。 运算符实现并注册 SQL Server 和可用性组资源的自定义资源定义。 部署运算符时，它将自身注册为侦听器，以接收有关在 Kubernetes 群集中部署 SQL Server 资源的通知。 `mssql-ha-supervisor` 支持可用性组。

每个 `master` Pod 都包含一个 SQL Server 实例。 一个高可用性部署包括 3 个 Pod。 每个 Pod 包含一个 SQL Server 实例，其中包含 SQL Server Always On 可用性组中的数据库。

根据工作负载，在部署时包括其他 Pod。 

## <a name="compute-pool"></a>计算池

计算池提供用于计算的 SQL Server 实例。

|Pod 名称 |Count| Kubernetes 控制器类型 | 容器 |
|--------|----|------|--------|
|`compute-<#n>-<#m>`|1 个或更多。| StatefulSet |- `mssql-server`<br><br>- `fluentbit`<br><br>- `collectd`

- `#n` 标识计算池。
- `#m` 标识池中的实例 ID。

计算池 SQL Server 实例是无状态的。 它们只需要 `tempdb` 的存储。

根据工作负载，在部署时包括其他 Pod。 

## <a name="data-pool"></a>数据池

数据池提供用于存储和计算的 SQL Server 实例。

|Pod 名称 |Count| Kubernetes 控制器类型 | 容器 |
|--------|----|------|--------|-------|
|`data-<#n>-<#m>` | 0 个或更多 | StatefulSet | -` mssql-server` <br><br>- `fluentbit`<br><br>- `collectd`|

- `#n` 标识数据池。
- `#m` 标识池中的实例 ID。

根据工作负载，在部署时包括其他 Pod。

## <a name="storage-pool"></a>存储池

存储池通过 Spark 提供数据引入、在 HDFS 中存储、通过 HDFS 和 SQL Server 终结点提供数据访问。

|Pod 名称 |Count| Kubernetes 控制器类型 | 容器 |
|--------|----|------|--------|
|`storage-0-#`|1 个或更多。 根据工作负载，在部署时包括其他 Pod。 | StatefulSet |- `hadoop`<br><br>- `mssql-server`<br><br>- `fluentbit`<br><br>
|`nmnode-0-#`|高可用性 1 个或更多| StatefulSet |- `hadoop`<br><br>- `fluentbit`
|`sparkehead-#`|高可用性 1 个或更多| StatefulSet |- `hadoop-yarn-jobhistory`<br><br>- `hadoop-livy-sparkhistory`<br><br>- `hadoop-hivemetastore`<br><br>-- `fluentbit`
|`zookeeper`|高可用性 0 或 3 个。 | StatefulSet |- `zookeeper`<br><br>- `fluentbit`

## <a name="application-pool"></a>应用程序池

应用程序池包含在一些测试配置文件中。 应用程序池承载应用程序服务代理，这些代理在部署大数据群集的应用程序时定义。 

`appproxy` 是位于应用程序池应用程序前面的 Web API。 它对用户进行身份验证，然后将请求路由到应用程序。

|Pod 名称 | Kubernetes 控制器类型 | 容器  |
|--------|----|------|
|`appproxy`| ReplicaSet |- `app-service-proxy`<br><br>- `fluentbit`

有关详细信息，请参阅[什么是大数据集上的应用程序部署？](concept-application-deployment.md)。

根据工作负载，在部署时包括其他 Pod。 

## <a name="gateway-service"></a>网关服务

网关服务提供到 Spark、HDFS、[Yarn](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html)、Yarn UI 和 Spark UI 的 Knox 网关。

|Pod 名称 | Kubernetes 控制器类型 | 容器 |
|--------|-----|-----|
|`gateway-<#>`| StatefulSet |- `knox`<br><br>- `fluentbit`

仅支持一个网关。

## <a name="open-source-container-references"></a>开放源代码容器引用

有关特定开源项目和版本，请参阅[开源软件参考](reference-open-source-software.md)。

## <a name="next-steps"></a>后续步骤

若要了解有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅以下资源：

- [什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)
- [Workshop:Microsoft [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 体系结构](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
- [如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md#configfile)
