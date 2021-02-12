---
title: 部署单节点 kubeadm 群集
titleSuffix: SQL Server Big Data Clusters
description: 使用 bash 部署脚本将 SQL Server 2019 大数据群集部署到单节点 kubeadm 群集。
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 63e1e3776c62033280287e55c288791901a2d15f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046917"
---
# <a name="deploy-with-a-bash-script-to-a-single-node-kubeadm-cluster"></a>使用 bash 脚本部署到单个节点 kubeadm 群集

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

在本教程中，你将使用示例 bash 部署脚本部署使用 kubeadm 的单节点 Kubernetes 群集，并在其上部署 SQL Server 大数据群集。

## <a name="prerequisites"></a>先决条件

- Vanilla Ubuntu 18.04 或 16.04 服务器虚拟机或物理计算机。 所有依赖项都由脚本设置，你可以从 VM 内部运行该脚本。

  > [!NOTE]
  > 尚不支持使用 Azure Linux VM。

- VM 应至少具有 8 个 CPU、64 GB 的 RAM 和 100 GB 磁盘空间。 在拉取所有大数据群集 Docker 映像后，将剩下 50 GB 用于在所有组件中使用的数据和日志。

- 使用以下命令更新现有包，以便确保 OS 映像是最新的。

   ``` bash
   sudo apt update && sudo apt upgrade -y
   sudo systemctl reboot
   ```

## <a name="recommended-virtual-machine-settings"></a>建议的虚拟机设置

1. 为虚拟机使用静态内存配置。 例如，在 Hyper-V 安装中，不使用动态内存分配，而是分配建议的 64 GB 或更多空间。

1. 在虚拟机监控程序中使用检查点或快照功能，以便将虚拟机回滚到干净状态。


## <a name="instructions-to-deploy-sql-server-big-data-cluster"></a>部署 SQL Server 大数据群集的说明

1. 在 VM 上下载计划用于部署的脚本。

   ```bash
   curl --output setup-bdc.sh https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm/setup-bdc.sh
   ```

2. 使用以下命令使该脚本可执行。

   ```bash
   chmod +x setup-bdc.sh
   ```

3. 运行该脚本（请确保使用 sudo 运行）

   ```bash
   sudo ./setup-bdc.sh
   ```

   出现提示时，请输入供以下外部终结点使用的密码：控制器、SQL Server master 和网关。 根据现有的 SQL Server 密码规则，密码应足够复杂。 控制器用户名默认为 admin。

4. 设置 azdata 工具的别名。

   ```bash
   source ~/.bashrc
   ```

5. 刷新 azdata 的别名设置。

   ```bash
   azdata --version
   ```

## <a name="cleanup"></a>清理

提供了 [cleanup-bdc.sh](https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm/cleanup-bdc.sh) 脚本，以便在必要时重置环境。 但是，我们建议将虚拟机用于测试目的，并使用虚拟机监控程序中的快照功能将虚拟机回滚到干净状态。

## <a name="next-steps"></a>后续步骤

要开始使用大数据群集，请参阅[教程：将示例数据加载到 SQL Server 大数据群集中](tutorial-load-sample-data.md)。
