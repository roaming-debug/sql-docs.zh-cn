---
title: 使用 kubeadm 配置 Kubernetes
titleSuffix: SQL Server Big Data Clusters
description: 了解如何在多台 Ubuntu 16.04 或 18.04 计算机（物理或虚拟）上为 SQL Server 2019 大数据群集部署配置 Kubernetes。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8c189f3a77f68dc212ead363b935016931b1dd6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038859"
---
# <a name="configure-kubernetes-on-multiple-machines-for-sql-server-big-data-cluster-deployments"></a>在多台计算机上为 SQL Server 大数据群集部署配置 Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文提供示例，说明如何使用 kubeadm 在多台计算机上为 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 部署配置 Kubernetes。 在此示例中，目标为多台 Ubuntu 16.04 或 18.04 LTS 计算机（物理或虚拟）。 如果要部署到其他 Linux 平台，则必须更改部分命令以匹配你的系统。  

> [!TIP] 
> 有关配置 Kubernetes 的示例脚本，请参阅[在 Ubuntu 16.04 LTS 或 18.04 LTS 上使用 Kubeadm 创建 Kubernetes 群集](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm)。
另请参阅[此](deployment-script-single-node-kubeadm.md)主题，以获取自动在 VM 上部署单节点 kubeadm 部署，然后在其上部署大数据群集默认配置的示例脚本。

## <a name="prerequisites"></a>先决条件

- 至少 3 台 Linux 物理计算机或虚拟机
- 每台计算机的建议配置：
   - 8 个 CPU
   - 64 GB 内存
   - 100 GB 存储
 
> [!Important] 
> 在启动大数据群集部署之前，请确保部署所针对的所有 Kubernetes 节点上的时钟同步。 大数据群集具有各种时间敏感型服务的内置运行状况属性，并且时钟偏差可能导致不正确的状态。

## <a name="prepare-the-machines"></a>准备计算机

在每台计算机上，必须满足多个先决条件。 在 bash 终端中，在每台计算机上运行以下命令：

1. 将当前计算机添加到 `/etc/hosts` 文件：

   ```bash
   echo $(hostname -i) $(hostname) | sudo tee -a /etc/hosts
   ```

1. 禁用所有设备上的交换功能。

   ```bash
   sudo sed -i "/ swap / s/^/#/" /etc/fstab
   sudo swapoff -a
   ```

1. 导入密钥并注册 Kubernetes 的存储库。

   ```bash
   sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
   echo 'deb http://apt.kubernetes.io/ kubernetes-xenial main' | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
   ```

1. 在计算机上配置 docker 和 Kubernetes 必备组件。

   ```bash
   KUBE_DPKG_VERSION=1.15.0-00 #or your other target K8s version, which should be at least 1.13.
   sudo apt-get update && \
   sudo apt-get install -y ebtables ethtool && \
   sudo apt-get install -y docker.io && \
   sudo apt-get install -y apt-transport-https && \
   sudo apt-get install -y kubelet=$KUBE_DPKG_VERSION kubeadm=$KUBE_DPKG_VERSION kubectl=$KUBE_DPKG_VERSION && \
   curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash
   ```
 
1. 设置 `net.bridge.bridge-nf-call-iptables=1`。 在 Ubuntu 18.04 上，以下命令首先启用 `br_netfilter`。

   ```bash
   . /etc/os-release
   if [ "$VERSION_CODENAME" == "bionic" ]; then sudo modprobe br_netfilter; fi
   sudo sysctl net.bridge.bridge-nf-call-iptables=1
   ```

## <a name="configure-the-kubernetes-master"></a>配置 Kubernetes 主计算机

在每台计算机上运行上述命令后，选择其中一台计算机作为 Kubernetes 主计算机。 然后在该计算机上运行以下命令。

1. 首先，使用以下命令在当前目录中创建 rbac.yaml 文件。 

   ```bash
   cat <<EOF > rbac.yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: default-rbac
   subjects:
   - kind: ServiceAccount
     name: default
     namespace: default
   roleRef:
     kind: ClusterRole
     name: cluster-admin
     apiGroup: rbac.authorization.k8s.io
   EOF
   ```

1. 在此计算机上初始化 Kubernetes 主计算机。 下面的示例脚本指定 Kubernetes 版本 `1.15.0`。 你使用的版本取决于 Kubernetes 群集。

   ```bash
   KUBE_VERSION=1.15.0
   sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --kubernetes-version=$KUBE_VERSION
   ```

   应看到 Kubernetes 主计算机已成功初始化的输出。

1. 请注意 `kubeadm join` 命令，在其他服务器上加入 Kubernetes 群集时需要用到它。 复制此命令以供将来使用。

   ![kubeadm join](./media/deploy-with-kubeadm/kubeadm-join.png)

1. 在主目录中设置 Kubernetes 配置文件。

   ```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```

1. 配置群集和 Kubernetes 仪表板。

   ```bash
   kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
   helm init
   kubectl apply -f rbac.yaml
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
   kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
   ```

## <a name="configure-the-kubernetes-agents"></a>配置 Kubernetes 代理

其他计算机将在群集中充当 Kubernetes 代理。 

在其他每台计算机上，运行在上一部分中复制的 `kubeadm join` 命令。

![kubeadm join agents](./media/deploy-with-kubeadm/kubeadm-join-agents.png)

## <a name="view-the-cluster-status"></a>查看群集状态

若要验证到群集的连接，请使用 [kubectl get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands) 命令返回群集节点列表。

```bash
kubectl get nodes
```

## <a name="next-steps"></a>后续步骤

本文中的步骤在多台 Ubuntu 计算机上配置了 Kubernetes 群集。 下一步是部署 SQL Server 2019 大数据群集。 有关说明，请参阅以下文章：

[在 Kubernetes 上部署 SQL Server](deployment-guidance.md#deploy)
