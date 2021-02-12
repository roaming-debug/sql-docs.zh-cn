---
title: 限制 Azure Kubernetes 服务 (AKS) 专用群集中大数据群集 (BDC) 的出口流量
titleSuffix: SQL Server Big Data Clusters
description: 了解如何限制 Azure Kubernetes 服务 (AKS) 专用群集中大数据群集 (BDC) 的出口流量。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 589031e40ba0f1e53afd5b4082d132358e9cda32
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046017"
---
# <a name="restrict-egress-traffic-of-big-data-clusters-bdc-clusters-in-azure-kubernetes-service-aks-private-cluster"></a>限制 Azure Kubernetes 服务 (AKS) 专用群集中大数据群集 (BDC) 的出口流量

AKS 预配了标准 SKU 负载均衡器，默认用于出口流量。 但是，如果禁用了公共 IP 或者出口需要额外的跃点，则默认设置可能不能满足所有方案的要求。 如果群集不允许使用公共 IP，并且位于网络虚拟设备 (NVA) 后面，则定义用户定义的路由表。

默认情况下，出于管理和操作目的，AKS 群集可以不受限制地对 Internet 进行出站（出口）访问。 AKS 群集中的工作器节点需要访问某些端口和完全限定的域名 (FQDN)，例如：

* 对于工作器节点 OS 安全更新程序，群集需要从 Microsoft 容器注册表 (MCR) 中拉取基础系统容器映像
* 启用了 GPU 的 AKS 工作器节点需要访问 Nvidia 中的一些终结点来安装驱动程序
* 在客户结合使用 AKS 与 Azure 服务的情况下，例如用于实现企业级合规性的 Azure Policy，以及包含容器见解的 Azure Monitor 
* 启用了 Dev Space 以及更多性质类似的方案

> [!NOTE]
> 目前，当你在 Azure Kubernetes 服务 (AKS) 专用群集中部署 BDC 时，除了本文中提到的依赖项外，此方案中没有入站依赖项（可以在[控制 Azure Kubernetes 服务 (AKS) 中群集节点的出口流量](/azure/aks/limit-egress-traffic)中找到所有出站依赖项）。

本文介绍了如何在使用高级网络和用户定义的路由 (UDR) 的 AKS 专用群集中部署 BDC，以及它与企业级网络环境的进一步集成。

## <a name="use-azure-firewall-to-restrict-egress-traffic"></a>使用 Azure 防火墙限制出口流量

Azure 防火墙提供了 Azure Kubernetes 服务 `(AzureKubernetesService)` FQDN 标记来简化此配置。

有关此标记的完整信息，请参阅[使用 Azure 防火墙限制出口流量](/azure/aks/limit-egress-traffic#restrict-egress-traffic-using-azure-firewall)。

下图展示了如何在 AKS 专用群集上限制流量。 

:::image type="content" source="media/private-cluster-restrict-egress-traffic/aks-azure-firewall-egress.png" alt-text="AKS 专用群集防火墙出口流量":::

使用 Azure 防火墙创建基本体系结构的具体步骤：

1. 创建资源组和 VNet
2. 创建和设置 Azure 防火墙
3. 创建用户定义的路由表
4. 设置防火墙规则
5. 创建服务主体 (SP)
6. 创建 AKS 专用群集
7. 创建 BDC 部署配置文件
8. 部署 BDC

下面是详细步骤。

## <a name="create-the-resource-group-and-vnet"></a>创建资源组和 VNet

1. 定义一组用于创建资源的环境变量。

   ```console
   export REGION_NAME=<region>
   export RESOURCE_GROUP=private-bdc-aksudr-rg
   export SUBNET_NAME=aks-subnet
   export VNET_NAME=bdc-vnet
   export AKS_NAME=bdcaksprivatecluster
   ```

1. 创建资源组

   ```azurecli
   az group create -n $RESOURCE_GROUP -l $REGION_NAME
   ```

1. 创建 VNET

   ```azurecli
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
   ```

## <a name="create-and-set-up-azure-firewall"></a>创建和设置 Azure 防火墙

1. 定义一组用于创建资源的环境变量。

   ```console
   export FWNAME=bdcaksazfw
   export FWPUBIP=$FWNAME-ip
   export FWIPCONFIG_NAME=$FWNAME-config

   az extension add --name azure-firewall
   ```

1. 为防火墙创建专用子网

   > [!NOTE]
   > 创建后，就无法更改防火墙名称

   ```azurecli
   az network vnet subnet create \
     --resource-group $RESOURCE_GROUP \
     --vnet-name $VNET_NAME \
     --name AzureFirewallSubnet \
     --address-prefix 10.3.0.0/24

    az network firewall create -g $RESOURCE_GROUP -n $FWNAME -l $REGION_NAME --enable-dns-proxy true

    az network public-ip create -g $RESOURCE_GROUP -n $FWPUBIP -l $REGION_NAME --sku "Standard"

    az network firewall ip-config create -g $RESOURCE_GROUP -f $FWNAME -n $FWIPCONFIG_NAME --public-ip-address $FWPUBIP --vnet-name $VNET_NAME
   ```

默认情况下，Azure 自动在 Azure 子网、虚拟网络和本地网络之间路由流量。 

## <a name="create-user-defined-route-table"></a>创建用户定义的路由表

创建用户定义的路由 (UDR) 表，其中包含跳转到 Azure 防火墙的跃点。

```azurecli

export SUBID= <your Azure subscription ID>
export FWROUTE_TABLE_NAME=bdcaks-rt
export FWROUTE_NAME=bdcaksroute
export FWROUTE_NAME_INTERNET=bdcaksrouteinet

export FWPUBLIC_IP=$(az network public-ip show -g $RESOURCE_GROUP -n $FWPUBIP --query "ipAddress" -o tsv)
export FWPRIVATE_IP=$(az network firewall show -g $RESOURCE_GROUP -n $FWNAME --query "ipConfigurations[0].privateIpAddress" -o tsv)

# Create UDR and add a route for Azure Firewall

az network route-table create -g $RESOURCE_GROUP --name $FWROUTE_TABLE_NAME

az network route-table route create -g $RESOURCE_GROUP --name $FWROUTE_NAME --route-table-name $FWROUTE_TABLE_NAME --address-prefix 0.0.0.0/0 --next-hop-type VirtualAppliance --next-hop-ip-address $FWPRIVATE_IP --subscription $SUBID

az network route-table route create -g $RESOURCE_GROUP --name $FWROUTE_NAME_INTERNET --route-table-name $FWROUTE_TABLE_NAME --address-prefix $FWPUBLIC_IP/32 --next-hop-type Internet
```

## <a name="set-up-firewall-rules"></a>设置防火墙规则 

```azurecli
# Add FW Network Rules

az network firewall network-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwnr' -n 'apiudp' --protocols 'UDP' --source-addresses '*' --destination-addresses "AzureCloud.$REGION_NAME" --destination-ports 1194 --action allow --priority 100
az network firewall network-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwnr' -n 'apitcp' --protocols 'TCP' --source-addresses '*' --destination-addresses "AzureCloud.$REGION_NAME" --destination-ports 9000
az network firewall network-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwnr' -n 'time' --protocols 'UDP' --source-addresses '*' --destination-fqdns 'ntp.ubuntu.com' --destination-ports 123

# Add FW Application Rules

az network firewall application-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwar' -n 'fqdn' --source-addresses '*' --protocols 'http=80' 'https=443' --fqdn-tags "AzureKubernetesService" --action allow --priority 100
```

可以运行下面的命令，将用户定义的路由 (UDR) 表关联到之前部署了BDC 的 AKS 群集：

```azurecli
az network vnet subnet update -g $RESOURCE_GROUP --vnet-name $VNET_NAME --name $SUBNET_NAME --route-table $FWROUTE_TABLE_NAME
```

## <a name="create--configure-service-principal-sp"></a>创建和配置服务主体 (SP)

在这一步中，需要创建服务主体，并向虚拟网络分配权限。

请参阅以下示例： 

```azurecli
# Create SP and Assign Permission to Virtual Network

az ad sp create-for-rbac -n "bdcaks-sp" --skip-assignment

APPID=<your service principal ID >
PASSWORD=< your service principal password >
VNETID=$(az network vnet show -g $RESOURCE_GROUP --name $VNET_NAME --query id -o tsv)

# Assign SP Permission to VNET

az role assignment create --assignee $APPID --scope $VNETID --role "Network Contributor"


RTID=$(az network route-table show -g $RESOURCE_GROUP -n $FWROUTE_TABLE_NAME --query id -o tsv)
az role assignment create --assignee $APPID --scope $RTID --role "Network Contributor"
```

## <a name="create-aks-cluster"></a>创建 AKS 群集

然后，创建以 `userDefinedRouting` 作为出站类型的 AKS 群集。

```azurecli
az aks create \
    --resource-group $RESOURCE_GROUP \
    --location $REGION_NAME \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --outbound-type userDefinedRouting \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --service-principal $APPID \
    --client-secret $PASSWORD \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

## <a name="build-big-data-cluster-bdc-deployment-profile"></a>生成大数据群集 (BDC) 部署配置文件

可以使用自定义配置文件创建 BDC 群集： 

```console
azdata bdc config init --source aks-dev-test --target private-bdc-aks --force
```

## <a name="generate-and-config-bdc-custom-deployment-profile"></a>生成和配置 BDC 自定义部署配置文件： 
```console
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.docker.imageTag=2019-CU6-ubuntu-16.04"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.logs.className=default"

azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"

azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="deploy-bdc-in-aks-private-cluster"></a>在 AKS 专用群集中部署 BDC

```console
export AZDATA_USERNAME=<your bdcadmin username>
export AZDATA_PASSWORD=< your bdcadmin password>

azdata bdc create --config-profile private-bdc-aks --accept-eula yes
```

## <a name="use-third-party-firewall-instead-of-azure-firewall"></a>使用第三方防火墙，而不是 Azure 防火墙

在使用 AKS 专用群集部署 BDC 时，使用第三方防火墙限制出口流量。 有关示例，请参阅 [Azure 市场防火墙](https://azuremarketplace.microsoft.com/marketplace/apps?search=firewall&page=1)。 第三方防火墙可用于所含配置更兼容的专用部署解决方案。 防火墙应提供以下网络规则：

* 所有[适用于 AKS 群集的必需出站网络规则和 FQDN](/azure/aks/limit-egress-traffic#required-outbound-network-rules-and-fqdns-for-aks-clusters)，以及所有通配符 HTTP/HTTPS 终结点和依赖项，这些可能会根据一些限定符和实际需求随 AKS 群集而变化。
* Azure Global 必需的网络规则/FQDN/应用程序规则（[此处](/azure/aks/limit-egress-traffic#azure-global-required-network-rules)有所提及）。
* 适用于 AKS 群集的建议的可选 FQDN/应用程序规则（[此处](/azure/aks/limit-egress-traffic#optional-recommended-fqdn--application-rules-for-aks-clusters)有所提及）。 

请查看如何[管理 AKS 专用群集中的 BDC](private-manage.md)，下一步是[连接到 BDC 群集](connect-to-big-data-cluster.md)。

有关此方案的自动化脚本，请参阅 [GitHub 上的 SQL Server 示例存储库](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/private-aks)。

## <a name="next-steps"></a>后续步骤

[管理 AKS 专用群集中的大数据群集](private-manage.md)

[使用 Azure Data Studio 连接到 SQL Server 大数据群集](connect-to-big-data-cluster.md)
