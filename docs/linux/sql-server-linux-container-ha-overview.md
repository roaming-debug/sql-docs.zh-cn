---
title: SQL Server 容器的高可用性
description: 了解 SQL Server 容器的高可用性。 此外了解如何在 Kubernetes 上使用 SQL Server 部署容器。
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/09/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: b0d384b2d75b6eedc431b4b352de8915b7e8097d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471528"
---
# <a name="high-availability-for-sql-server-containers"></a>SQL Server 容器的高可用性

在 Kubernetes 中本机创建和管理 SQL Server 实例。

将 SQL Server 部署到由 [Kubernetes](https://kubernetes.io/) 管理的 Docker 容器。 在 Kubernetes 中，具有 SQL Server 实例的容器可以在群集节点出现故障时自动进行恢复。

SQL Server 2017 引入了可以在 Kubernetes 上部署的 Docker 映像。 可以使用 Kubernetes 持久卷声明 (PVC) 来配置映像。 Kubernetes 监视容器中的 SQL Server 进程。 如果进程、Pod、容器或节点发生故障，Kubernetes 将自动启动另一个实例，并重新连接到存储。

## <a name="container-with-sql-server-instance-on-kubernetes"></a>Kubernetes 上存在具有 SQL Server 实例的容器

Kubernetes 1.6 及更高版本支持[存储类](https://kubernetes.io/docs/concepts/storage/storage-classes/)、[持久卷声明](https://kubernetes.io/docs/concepts/storage/storage-classes/#persistentvolumeclaims)和 [Azure 磁盘卷类型](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_disk)  。 

在此配置中，Kubernetes 扮演容器业务流程协调程序一角。 

![Kubernetes SQL Server 群集示意图](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql.png)

在上图中，`mssql-server` 是 [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/) 中的 SQL Server 实例（容器）。 [副本集](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)可确保在节点发生故障后自动恢复 Pod。 应用程序会连接到服务。 在这种情况下，该服务表示负载均衡器，承载着 `mssql-server` 发生故障后保持不变的 IP 地址。

Kubernetes 协调群集中的资源。 如果承载 SQL Server 实例容器的节点发生故障，该节点会使用 SQL Server 实例来启动新容器，并将其附加到同一个持久存储上。

SQL Server 2017 及更高版本支持 Kubernetes 上的容器。

若要在 Kubernetes 中创建容器，请查看[在 Kubernetes 中部署 SQL Server 容器](tutorial-sql-server-containers-kubernetes.md)

## <a name="next-steps"></a>后续步骤

若要在 Azure Kubernetes Service (AKS) 中部署 SQL Server 容器，请参阅以下示例：
* [在 Docker 容器中部署 SQL Server](./sql-server-linux-docker-container-deployment.md)
* [在 Kubernetes 中部署 SQL Server 容器](tutorial-sql-server-containers-kubernetes.md)