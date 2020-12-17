---
title: 配置 Azure Kubernetes 服务
titleSuffix: SQL Server Big Data Clusters
description: 了解如何为 SQL Server 2019 大数据群集部署配置 Azure Kubernetes 服务 (AKS)。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 27e2596894e6d36742472ad1d3ae192fc37787e6
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489667"
---
# <a name="configure-azure-kubernetes-service-for-sql-server-big-data-cluster-deployments"></a>为 SQL Server 大数据群集部署配置 Azure Kubernetes 服务

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍如何为 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 部署配置 Azure Kubernetes 服务 (AKS)。

通过 AKS，可轻松创建、配置和管理虚拟机群集，这些虚拟机已使用 Kubernetes 群集进行了预配置，可运行容器化应用程序。 这使用户能够使用现有技能或利用社区不断分享的丰富专业知识和经验在 Microsoft Azure 上部署和管理基于容器的应用程序。

本文介绍使用 Azure CLI 在 AKS 上部署 Kubernetes 的步骤。 如果没有 Azure 订阅，请在开始操作前先创建一个免费帐户。

> [!TIP]
> 还可以将 AKS 和大数据群集的部署脚本编写成一个步骤。 有关详细信息，请参阅 [python 脚本](quickstart-big-data-cluster-deploy.md)或 Azure Data Studio [笔记本](notebooks-deploy.md)，了解如何实现此操作。

## <a name="prerequisites"></a>先决条件

- [部署 SQL Server 2019 大数据工具](deploy-big-data-tools.md)：
   - **Kubectl**
   - **Azure Data Studio**
   - **SQL Server 2019 扩展**
   - **Azure CLI**

- Kubernetes 服务器要求的最低版本为 1.13。 对于 AKS，需要使用 `--kubernetes-version` 参数指定与默认版本不同的版本。

- 若要确保成功部署并获得最佳体验，同时验证 AKS 上的基本方案，可以使用单节点或多节点 AKS 群集，并提供以下资源：
   - 8 个跨所有节点的 vCPU
   - 每个 VM 具有 64 GB 内存
   - 跨所有节点的 24 个或更多附加磁盘

   > [!TIP]
   > Azure 基础结构提供多种 VM 容量选项，请参阅[此处](/azure/virtual-machines/windows/sizes)，了解计划要部署的区域中的选项。

## <a name="create-a-resource-group"></a>创建资源组

Azure 资源组是一个逻辑组，用于部署和管理 Azure 资源。 通过以下步骤登录到 Azure 并为 AKS 群集创建资源组。

1. 在命令提示符处，运行以下命令并按照提示登录到 Azure 订阅：

    ```azurecli
    az login
    ```

1. 如有多个订阅，可通过运行以下命令来查看所有订阅：

   ```azurecli
   az account list
   ```

1. 若要更改为其他订阅，可运行以下命令：

   ```azurecli
   az account set --subscription <subscription id>
   ```

1. 使用此命令来标识要在其中部署群集和资源的 Azure 区域：

   ```azurecli
   az account list-locations -o table
   ```

1. 使用“**az group create**”命令创建资源组。 以下示例在 `westus2` 位置创建名为 `sqlbdcgroup` 的资源组。

   ```azurecli
   az group create --name sqlbdcgroup --location westus2
   ```

## <a name="verify-available-kubernetes-versions"></a>验证可用的 Kubernetes 版本

使用 Kubernetes 的最新可用版本。 最新可用版本取决于要部署群集的位置。 以下命令返回特定位置中可用的 Kubernetes 版本。

运行该命令之前，请更新脚本。 将 `<Azure data center>` 替换为群集的位置。

   **bash**

   ```bash
   az aks get-versions \
   --location <Azure data center> \
   --query orchestrators \
   --o table
   ```

   **PowerShell**

   ```powershell
   az aks get-versions `
   --location <Azure data center> `
   --query orchestrators `
   --o table
   ```

选择群集的最新可用版本。 记录版本号。 将在下一步中使用它。

## <a name="create-a-kubernetes-cluster"></a>创建 Kubernetes 群集

1. 使用 [az aks create](/cli/azure/aks) 命令在 AKS 中创建 Kubernetes 群集。 以下示例创建一个名为“kubcluster”的 Kubernetes 群集，该群集具有一个 Standard_L8s 大小的 Linux 代理节点。

   在运行该脚本之前，请将 `<version number>` 替换为在上一步中标识的版本号。

   请确保在前面部分中使用的资源组中创建 AKS 群集。

   **bash：**

   ```bash
   az aks create --name kubcluster \
   --resource-group sqlbdcgroup \
   --generate-ssh-keys \
   --node-vm-size Standard_L8s \
   --node-count 1 \
   --kubernetes-version <version number>
   ```

   **PowerShell：**

   ```powershell
   az aks create --name kubcluster `
   --resource-group sqlbdcgroup `
   --generate-ssh-keys `
   --node-vm-size Standard_L8s `
   --node-count 1 `
   --kubernetes-version <version number>
   ```

   可通过更改 `--node-count <n>` 来增加或减少 Kubernetes 代理节点数，其中 `<n>` 为要使用的代理节点数。 其中不包括主 Kubernetes 节点，该节点由 AKS 在后台进行管理。 上一个示例出于评估目的仅使用单个节点。 还可以更改 `--node-vm-size`，以选择符合工作负载要求的合适的虚拟机大小。 使用 `az vm list-sizes --location westus2 -o table` 命令可列出你所在地区的可用虚拟机大小。

   几分钟后，该命令完成并返回有关群集的 JSON 格式的信息。

   > [!TIP]
   > 如果在 AKS 中创建群集时出现任何错误，请参阅本文的[故障排除部分](#troubleshoot)。

1. 保存前一个命令的 JSON 输出供稍后使用。

## <a name="connect-to-the-cluster"></a>连接到群集

1. 若要配置 kubectl 以连接到 Kubernetes 群集，请运行 [az aks get-credentials](/cli/azure/aks#az-aks-get-credentials) 命令。 此步骤下载凭据并配置 kubectl CLI 以使用这些凭据。

   ```azurecli
   az aks get-credentials --resource-group=sqlbdcgroup --name kubcluster
   ```

1. 若要验证到群集的连接，请使用 [kubectl get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands) 命令返回群集节点列表。  以下示例显示配置了 1 个主节点和 3 个代理节点时的输出。

   ```bash
   kubectl get nodes
   ```

## <a name="troubleshooting"></a><a id="troubleshoot"></a> 故障排除

如果使用上述命令创建 Azure Kubernetes 服务时遇到任何问题，请尝试以下解决方法：

- 请确保已安装[最新 Azure CLI](/cli/azure/install-azure-cli)。
- 使用另一个资源组和群集名称来测试上述步骤。
- 请参阅详细的[有关 AKS 的故障排除文档](/azure/aks/troubleshooting)。

## <a name="next-steps"></a>后续步骤

本文中的步骤在 AKS 中配置了 Kubernetes 群集。 下一步是在 AKS Kubernetes 群集上部署 SQL Server 2019 大数据群集。 有关如何部署大数据群集的详细信息，请参阅以下文章：

[如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)