---
title: 入门
titleSuffix: SQL Server Big Data Clusters
description: 了解用于部署 SQL Server 大数据群集的步骤和资源。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 1a6a064ad4a6077bffcd1642569d63ee0e086a7f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038807"
---
# <a name="get-started-with-big-data-clusters-2019-deployment"></a>开始 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 部署

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文概述了如何部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]。 本文向你介绍概念并提供用于理解部署方案的框架。 具体部署步骤因客户端和服务器的平台选择而异。 有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的简介，请参阅 [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md)

有关 SQL Server 方案，请参阅：

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Linux](../linux/sql-server-linux-setup.md)
- [Docker 容器](../linux/sql-server-linux-docker-container-deployment.md)

## <a name="quick-introduction"></a>快速简介 

观看此 9 分钟的视频，大致了解如何部署大数据群集：

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Big-Data-Clusters-deployment-overview/player?WT.mc_id=dataexposed-c9-niner]


> [!TIP]
> 要快速获得一个部署了 Kubernetes 和大数据群集的环境以协助增强其功能，请使用[脚本部分](#scripts)中指向的示例脚本之一。 部署后，可使用以下部分中的[客户端工具](#tools)来管理群集。


## <a name="client-tools"></a><a id="tools"></a> 客户端工具

大数据群集需要一组特定的客户端工具。 将大数据群集部署到 Kubernetes 之前，应安装部署所需的工具。 对于不同方案，需要使用特定的工具。 每篇文章都应说明用于执行特定任务的必需工具。 有关工具和安装链接的完整列表，请参阅[安装 SQL Server 2019 大数据工具](deploy-big-data-tools.md)。

## <a name="kubernetes"></a>Kubernetes

大数据群集被部署为在 [Kubernetes](https://kubernetes.io/docs/home) 中进行管理的一系列相关容器。 可通过多种方式托管 Kubernetes。 即使已有现有的 Kubernetes 环境，也应查看大数据群集的相关要求。

- **Azure Kubernetes 服务 (AKS)** ：AKS 支持在 Azure 中部署托管的 Kubernetes 群集。 用户仅管理和维护代理节点。 使用 AKS，无需为群集预配硬件。 还可轻松使用 [python 脚本](quickstart-big-data-cluster-deploy.md)或[部署笔记本](notebooks-deploy.md)创建 AKS 群集和部署大数据群集，只需一步即可完成。 有关为大数据群集部署配置 AKS 的详细信息，请参阅[为 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 配置 Azure Kubernetes 服务](deploy-on-aks.md)。

- **Azure Red Hat OpenShift (ARO)** ：ARO 允许在 Azure 中部署托管的 Red Hat OpenShift 群集。 用户仅管理和维护代理节点。 使用 ARO，无需为群集预配自己的硬件。 还可轻松使用 [python 脚本](quickstart-big-data-cluster-deploy-aro.md)来创建 ARO 群集和部署大数据群集，只需一步即可完成。 此部署模型在 SQL Server 2019 CU5 中引入。 

- **多台计算机**：还可以将 Kubernetes 部署到多台 Linux 计算机，这些计算机可以是物理服务器或虚拟机。 可使用 [Kubeadm](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/) 工具创建 Kubernetes 群集。 可使用 [bash 脚本](deployment-script-single-node-kubeadm.md)自动执行此类部署。 如果已有想要用于大数据群集的现有基础结构，此方法会很有效。 有关将 kubeadm 部署用于大数据群集的详细信息，请参阅[在多台计算机上为 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 部署配置 Kubernetes](deploy-with-kubeadm.md)  。

- **Red Hat OpenShift**：部署到自己的 Red Hat OpenShift 群集。 相关信息请参阅[在 OpenShift 本地和 Azure Red Hat OpenShift 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deploy-openshift.md)。 此部署模型在 SQL Server 2019 CU5 中引入。

## <a name="deploy-a-big-data-cluster"></a>部署大数据群集

配置 Kubernetes 后，可使用 `azdata bdc create` 命令部署大数据群集。 部署时，可采用多种不同的方法。

- 若要部署到开发测试环境，则可选择使用 azdata 提供的[默认配置](deployment-guidance.md#deploy)之一  。

- 若要自定义部署，可以创建并使用自己的[部署配置文件](deployment-guidance.md#configfile)。

- 对于完全无人参与的安装，可以传递环境变量中的所有其他设置。 有关详细信息，请参阅[无人参与的部署](deployment-guidance.md#unattended)。


## <a name="deployment-scripts"></a><a id="scripts"></a> 部署脚本

通过部署脚本，只需一个步骤即可部署 Kubernetes 和大数据群集。 部署脚本还经常为大数据群集设置提供默认值。 可以通过创建自己的版本来自定义任何部署脚本，以便以不同的方式配置大数据群集部署。

以下部署脚本当前可用：

- [Python 脚本 - 部署 Azure Kubernetes 服务 (AKS) 上的大数据群集](quickstart-big-data-cluster-deploy.md)
- [Bash 脚本 - 将大数据群集部署到单个节点 kubeadm 群集](deployment-script-single-node-kubeadm.md)

## <a name="deployment-notebooks"></a>部署笔记本

还可以通过运行 Azure Data Studio 笔记本部署大数据群集。 有关如何在 AKS 上使用笔记本进行部署的详细信息，请参阅以下文章：

- [使用 Azure Data Studio 笔记本部署大数据群集](notebooks-deploy.md)。

## <a name="next-steps"></a>后续步骤

成功部署大数据群集后，[连接到该群集](connect-to-big-data-cluster.md)并考虑[加载示例数据](tutorial-load-sample-data.md)以便与多个演练配合使用。