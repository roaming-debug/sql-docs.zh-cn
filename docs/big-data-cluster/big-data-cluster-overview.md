---
title: 什么是大数据群集？
titleSuffix: SQL Server Big Data Clusters
description: 了解在 Kubernetes 上运行并为关系数据和 HDFS 数据提供横向扩展选项的 SQL Server 大数据群集。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 01/07/2020
ms.topic: overview
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bd3a092906bf2a7d46c7f343b7edf913bdd4d9cf
ms.sourcegitcommit: ab9ddcc16fdfc245cf9a49d1e90bb1ffe3958c38
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92914333"
---
# <a name="what-are-big-data-clusters-2019"></a>什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]？

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

从 [!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] 开始，借助 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 可部署在 Kubernetes 上运行的 SQL Server、Spark 和 HDFS 容器的可缩放群集。 这些组件并行运行以确保可读取、写入和处理 Transact-SQL 或 Spark 中的大数据，这样你就可以借助大量大数据轻松合并并分析高价值关系数据。

使用 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 可以：

- [部署](./deploy-get-started.md) SQL Server、Spark 和在 Kubernetes 上运行的 HDFS 容器的可缩放群集。 
- 在 Transact-SQL 或 Spark 中读取、写入和处理大数据。
- 通过大容量大数据轻松合并和分析高价值关系数据。
- 查询外部数据源。
- 在由 SQL Server 管理的 HDFS 中存储大数据。
- 通过群集查询多个外部数据源的数据。
- 将数据用于 AI、机器学习和其他分析任务。
- 在 [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] 中[部署和运行应用程序](./concept-application-deployment.md)。
- 使用 [PolyBase](../relational-databases/polybase/polybase-guide.md) 虚拟化数据。 使用外部表从外部 SQL Server、Oracle、Teradata、MongoDB 和 ODBC 数据源查询数据。
- 使用 Always On 可用性组技术为 SQL Server 主实例和所有数据库提供高可用性。

有关最新版本的新功能和已知问题的详细信息，请参阅[发行说明](release-notes-big-data-cluster.md)。

## <a name="scenarios"></a>方案

使用 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 可灵活处理大数据。 可查询外部数据源，存储通过 SQL Server 管理的 HDFS 中的大数据，或通过群集查询来自多个外部数据源的数据。 然后，可以将数据用于 AI，机器学习和其他分析任务。 下列各部分提供了有关这些方案的详细信息。

### <a name="data-virtualization"></a>数据虚拟化

通过利用 [PolyBase](../relational-databases/polybase/polybase-guide.md)，[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 可以查询外部数据源，而无需移动或复制数据。 [!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] 引入了数据源的新连接器。

![数据虚拟化](media/big-data-cluster-overview/data-virtualization.png)

### <a name="data-lake"></a>Data Lake

SQL Server 大数据群集包括可缩放的 HDFS 存储池  。 这可用于存储可能来自多个外部源的大数据。 大数据存储在大数据群集中的 HDFS 中后，便可分析和查询数据并将其与关系数据相结合。

![Data Lake](media/big-data-cluster-overview/data-lake.png)

### <a name="scale-out-data-mart"></a>横向扩展数据市场

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 提供了横向扩展计算和存储以提高分析任何数据的性能。 来自各种源的数据可作为缓存跨数据池节点进行引入和分布以供进一步分析  。

![数据市场](media/big-data-cluster-overview/data-mart.png)

### <a name="integrated-ai-and-machine-learning"></a>集成的 AI 和机器学习

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 可对 HDFS 存储池和数据池中的数据启用 AI 和机器学习任务。 使用 R、Python、Scala 或者 Java，可在 SQL Server 中使用 Spark 以及内置的 AI 工具。

![AI 和 ML](media/big-data-cluster-overview/ai-ml-spark.png)

### <a name="management-and-monitoring"></a>管理和监视

通过命令行工具、API、门户和动态管理视图的组合提供管理和监视。

可以使用 [Azure Data Studio](../azure-data-studio/what-is.md) 在大数据群集上执行各种任务：
- 用于常见管理任务的内置片段。
- 浏览 HDFS、上传文件、预览文件和创建目录的能力。
- 创建、打开和运行与 Jupyter 兼容的笔记本的能力。
- 用于简化外部数据源的创建的数据虚拟化向导（由数据虚拟化扩展启用  ）。

## <a name="architecture"></a><a id="architecture"></a>体系结构

SQL Server 大数据群集是由 [Kubernetes](https://kubernetes.io/docs/concepts/) 编排的 Linux 容器群集。

### <a name="kubernetes-concepts"></a>Kubernetes 的概念

Kubernetes 是一个开放源代码容器业务流程协调程序，可以根据需要缩放容器部署。 下表定义了一些重要的 Kubernetes 术语：

|术语|描述|
|:--|:--|
| **Cluster** | Kubernetes 群集是一组称为节点的计算机。 一个节点控制群集并被指定为主节点；其余节点是工作器节点。 Kubernetes 主节点负责在工作器节点之间分配工作，并负责监视群集的运行状况。 |
| **Node** | 节点运行容器化应用程序。 它可以是物理计算机或虚拟机。 Kubernetes 群集可以混合包含物理计算机节点和虚拟机节点。 |
| **Pod** | Pod 是 Kubernetes 的原子部署单元。 Pod 是运行应用程序所需的一个或多个容器和相关资源的逻辑组。 一个 Pod 只能在一个节点上运行；一个节点可以运行一个或多个 Pod。 Kubernetes 主节点自动将 Pod 分配给群集中的其余节点。 |
| &nbsp; ||

在 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 中，Kubernetes 负责 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的状态；Kubernetes 生成和配置群集节点、将 Pod 分配给节点，并监视群集的运行状况。

### <a name="big-data-clusters-architecture"></a>大数据群集体系结构

下图显示了 SQL Server 大数据群集的组件：

![体系结构概述](media/big-data-cluster-overview/architecture-diagram-overview.png)

### <a name="controller"></a><a id="controlplane"></a>控制器

控制器为群集提供管理和安全性。 它包含控制服务、配置存储和其他群集级服务，例如 Kibana、Grafana 和 Elastic Search。

### <a name="compute-pool"></a><a id="computeplane"></a> 计算池

计算池为群集提供计算资源。 它包含在 Linux 上的 SQL Server Pod 上运行的节点。 计算池中的 Pod 分为用于特定处理任务的 SQL Compute实例。 

### <a name="data-pool"></a><a id="dataplane"></a> 数据池

数据池用于数据暂留和缓存。 数据池由一个或多个运行 Linux 上的 SQL Server 的 Pod 组成。 它用于从 SQL 查询或 Spark 作业中提取数据。 SQL Server 大数据群集数据市场持久保留在数据池中。 

### <a name="storage-pool"></a>存储池

存储池由 Linux 上的 SQL Server、Spark 和 HDFS 组成的存储池 Pod 组成。 SQL Server 大数据群集中的所有存储节点都是 HDFS 群集的成员。

> [!TIP]
> 如需深入了解大数据群集体系结构和安装，请参阅[研讨会：Microsoft [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 体系结构](https://github.com/microsoft/sqlworkshops-bdc)。

## <a name="next-steps"></a>后续步骤

有关部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅 [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 入门](deploy-get-started.md)。