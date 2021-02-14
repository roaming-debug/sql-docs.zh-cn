---
title: 使用 Jupyter 笔记本和 Azure Data Studio 监视群集
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 2019 大数据群集上，使用 Jupyter 笔记本和 Azure Data Studio 监视群集。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bc370b42f247c4b8e57c5628feaae7385ba1502d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039370"
---
# <a name="monitoring-cluster-with-notebooks"></a>使用笔记本监视群集

此页是适用于 SQL Server 大数据群集的笔记本的索引。 这些可执行的笔记本 (.ipynb) 旨在用于 SQL Server 2019，以帮助监视大数据群集。

每个笔记本均可检查其自身的依赖项。 “运行所有单元格”要么成功完成，要么抛出异常，并显示指向另一个笔记本的超链接提示，以解决缺少依赖项的问题。 单击提示超链接转到后续的笔记本，按“运行所有单元格”，成功后返回到原始笔记本，再按“运行所有单元格”。

一旦安装了所有依赖项，但“运行所有单元格”失败，每个笔记本就会分析结果，并尽可能生成指向另一个笔记本的超链接提示，以进一步帮助解决问题。


## <a name="monitoring-kubernetes"></a>监视 Kubernetes

此部分包含一组笔记本，用于使用 `azdata` 命令行接口 (CLI) 获取 SQL Server 大数据群集的相关信息和状态。

|名称 |说明 |
|---|---|---|---|
|TSG006 - 获取系统 Pod 状态|查看所有系统 Pod 的状态。 |
|TSG007 - 获取 BDC Pod 状态|查看大数据群集 Pod 状态。|
|TSG008 - 获取版本信息 (Kubernetes)|获取 Kubernetes 群集信息。|
|TSG009 - 获取节点 (Kubernetes)|获取 Kubernetes 上下文。 |
|TSG010 - 获取配置上下文|使用 DMV 查询来获取内部查询处理器错误的详细信息|
|TSG015 - 查看 BDC 服务 (Kubernetes)|获取部署在 Kubernetes 群集中的 BDC 群集的服务状态。 |
|TSG016- 描述 BDC Pod|获取部署在 Kubernetes 群集中的 BDC 的 Pod 状态。 |
|TSG020- 描述节点 (Kubernetes)|使用 kubectl 命令行接口来获取 BDC 群集的节点信息。 |
|TSG021 - 获取群集信息 (Kubernetes)|获取 Kubernetes 群集信息。 |
|TSG022 - 获取 kubeadm 主机的外部 IP 地址|获取 kubeadm 主机的外部 IP 地址。 |
|TSG023 - 获取所有 BDC 对象 (Kubernetes)|获取系统命名空间和大数据群集命名空间的所有 Kubernetes 资源的摘要。 |
|TSG042 - 获取节点名称，以及“数据和日志”PVC 的外部装载|获取托管 Pod 的节点名称，以及“数据和日志”外部装载。 |
|TSG063 - 获取存储类 (Kubernetes)|使用此笔记本来获取 Kubernetes 存储类。 |
|TSG064 - 获取 BDC 永久性卷声明|显示大数据群集的永久性卷声明 (PVC)。 |
|TSG065 - 获取 BDC 机密 (Kubernetes)|查看大数据群集机密。 |
|TSG066 - 获取 BDC 事件 (Kubernetes)|查看大数据群集事件。|
|TSG072 - 获取永久性卷 (Kubernetes)|显示 Kubernetes 群集的永久卷 (PV)。 永久卷是非命名空间对象。 |
|TSG081 - 获取命名空间 (Kubernetes)|获取 Kubernetes 命名空间。 |
|TSG089 - 描述 BDC 非运行 Pod|显示 Kubernetes 群集的非运行 BDC Pod。 |
|TSG097 - 获取 BDC StatefulSet (Kubernetes)|显示 Kubernetes 群集的 BDC StatefulSet。 |
|TSG098 - 获取 BDC ReplicaSet (Kubernetes)|显示 Kubernetes 群集的 BDC ReplicaSet。 |
|TSG099 - 获取 BDC DaemonSet (Kubernetes)|显示 Kubernetes 群集的 BDC DaemonSet。 |


## <a name="monitor-big-data-cluster-bdc"></a>监视大数据群集 (BDC)

此部分包含一组笔记本，用于获取托管 SQL Server 大数据群集 (BDC) 的 Kubernetes 群集的相关信息和状态。

|名称 |说明 |
|---|---|---|---|
|TSG003 - 显示 BDC Spark 会话|查看 BDC Spark 会话。 |
|TSG004 - 显示 BDC 应用|查看在 BDC 群集中启动并运行的应用。|
|TSG012 - 显示 BDC 状态|获取 BDC 群集的不同组件的当前状态。|
|TSG013 - 显示存储池 (HDFS) 中的文件列表|获取存储池 (HDFS) 中的文件列表。 |
|TSG014 - 显示 BDC 终结点|获取 BDC 群集的所有可用终结点。|
|TSG017 - 显示 BDC 配置|获取 BDC 配置。 |
|TSG033 - 显示 BDC SQL 状态|获取部署在 Kubernetes 群集中的 BDC 的 SQL Server 状态。 |
|TSG049 - 显示 BDC 控制器状态|获取部署在 Kubernetes 群集中的 BDC 的控制器状态。 |
|TSG068 - 显示 BDC HDFS 状态|获取部署在 Kubernetes 群集中的 BDC 的 HDFS 状态。 |
|TSG069 - 显示大数据群集网关状态|获取部署在 Kubernetes 群集中的 BDC 的 BDC 网关状态。 |
|TSG070 - 查询 SQL 主池| 在主实例上执行 SQL 查询。 |

## <a name="next-steps"></a>后续步骤

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。
