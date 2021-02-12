---
title: 在 Azure Kubernetes 服务 (AKS) 专用群集中部署 BDC
titleSuffix: SQL Server Big Data Cluster
description: 了解如何通过具有高级网络 (CNI) 的 Azure Kubernetes Service (AKS) 专用群集部署 SQL Server 大数据群集。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d9197e936f3df8f0a98db672f69ac05fce07ebd8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043877"
---
# <a name="deploy-bdc-in-azure-kubernetes-service-aks-private-cluster"></a>在 Azure Kubernetes 服务 (AKS) 专用群集中部署 BDC

本文介绍了如何在 Azure Kubernetes 服务 (AKS) 专用群集上部署 SQL Server 大数据群集。 此配置支持在企业网络环境中限制使用公共 IP 地址。

专用部署具有以下优势：

* 限制使用公共 IP 地址
* 应用程序服务器和群集节点池之间的网络流量仅停留在专用网络上
* 可自定义必需的出口流量配置，来满足特定要求

本文介绍了如何在应用各自的安全字符串的同时使用 AKS 专用群集来限制使用公共 IP 地址。

## <a name="deploy-private-bdc-cluster-with-aks-private-cluster"></a>通过 AKS 专用群集部署专用 BDC 群集

首先，创建一个 [AKS 专用群集](/azure/aks/private-clusters)，确保 API 服务器和节点池之间的网络流量仅停留在专用网络上。 控制平面或 API 服务器具有 AKS 专用群集的内部 IP 地址。

本部分介绍了如何在具有高级网络 (CNI) 的 Azure Kubernetes 服务 (AKS) 专用群集中部署 BDC 群集。

## <a name="create-a-private-aks-cluster-with-advanced-networking"></a>创建具有高级网络的专用 AKS 群集

```bash

export REGION_NAME=<your Azure region >
export RESOURCE_GROUP=< your resource group name >
export SUBNET_NAME=aks-subnet
export VNET_NAME=bdc-vnet
export AKS_NAME=< your aks private cluster name >
 
az group create -n $RESOURCE_GROUP -l $REGION_NAME
 
az network vnet create \
    --resource-group $RESOURCE_GROUP \
    --location $REGION_NAME \
    --name $VNET_NAME \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name $SUBNET_NAME \
    --subnet-prefix 10.1.0.0/16
 

SUBNET_ID=$(az network vnet subnet show \
    --resource-group $RESOURCE_GROUP \
    --vnet-name $VNET_NAME \
    --name $SUBNET_NAME \
    --query id -o tsv)
 
echo $SUBNET_ID
## will be displayed something similar as the following: 
/subscriptions/xxxx-xxxx-xxx-xxxx-xxxxxxxx/resourceGroups/your-bdc-rg/providers/Microsoft.Network/virtualNetworks/your-aks-vnet/subnets/your-aks-subnet
```

### <a name="create-aks-private-cluster-with-advanced-networking-cni"></a>通过高级网络 (CNI) 创建 AKS 专用群集

若要进行下一步，需要在启用了专用群集功能的情况下使用标准负载均衡器来预配 AKS 群集。 命令将如下所示： 

```bash
az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

成功部署后，你可以前往 `<MC_yourakscluster>` 资源组，并将发现 `kube-apiserver` 是专用终结点。 有关示例，请参阅下文。

## <a name="connect-to-an-aks-cluster"></a>连接到 AKS 群集

```azurecli
az aks get-credentials -n $AKS_NAME -g $RESOURCE_GROUP
```

## <a name="build-big-data-cluster-bdc-deployment-profile"></a>生成大数据群集 (BDC) 部署配置文件

连接到 AKS 群集后，可以开始部署 BDC，还可以准备环境变量并启动部署： 

```azurecli
azdata bdc config init --source aks-dev-test --target private-bdc-aks --force
```

生成和配置 BDC 自定义部署配置文件：

```azurecli
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.docker.imageTag=2019-CU6-ubuntu-16.04"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.logs.className=default"

azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"

azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="deploy-private-sql-server-big-data-cluster-with-ha"></a>部署高可用性专用 SQL Server 大数据群集

如果[部署高可用性 (HA) SQL Server 大数据群集 (SQL-BDC)](deployment-high-availability.md)，则将使用 `aks-dev-test-ha` 部署配置文件。 成功部署后，你可以使用同一个 `kubectl get svc` 命令，你会看到生成了另一个 `master-secondary-svc` 服务。 需要将 `ServiceType` 配置为 `NodePort`。 其他步骤将与上一节中提到的步骤类似。

以下示例将 `ServiceType` 设置为 `NodePort`：

```azurecli
azdata bdc config replace -c private-bdc-aks /bdc.json -j "$.spec.resources.master.spec.endpoints[1].serviceType=NodePort"
```

## <a name="deploy-bdc-in-aks-private-cluster"></a>在 AKS 专用群集中部署 BDC

```azurecli
export AZDATA_USERNAME=<your bdcadmin username>
export AZDATA_PASSWORD=< your bdcadmin password>

azdata bdc create --config-profile private-bdc-aks --accept-eula yes
```

## <a name="check-deployment-status"></a>检查部署状态

部署需要几分钟的时间，你可以使用以下命令来检查部署状态： 

```console
kubectl get pods -n mssql-cluster -w
```

## <a name="check-the-service-status"></a>检查服务状态

使用以下命令检查服务。 验证它们是否都正常并且没有使用任何外部 IP：

```console
kubectl get services -n mssql-cluster
```

请参阅如何[管理 AKS 专用群集中的 BDC](private-manage.md)，下一步是[连接到 BDC 群集](connect-to-big-data-cluster.md)。

有关此方案的自动化脚本，请参阅 [GitHub 上的 SQL Server 示例存储库](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/private-aks)。

## <a name="next-steps"></a>后续步骤

[管理专用群集](private-manage.md)

[限制专用 BDC 群集的出口流量](private-restrict-egress-traffic.md)

[使用 Azure Data Studio 连接到 SQL Server 大数据群集](connect-to-big-data-cluster.md)
