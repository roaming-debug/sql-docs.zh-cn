---
title: 在 Azure Kubernetes 服务 (AKS) 上的 Active Directory 中部署 - 教程
titleSuffix: SQL Server Big Data Cluster
description: 了解如何在 Azure Kubernetes 服务 (AKS) 上的 AD 模式下部署 SQL Server 大数据群集。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 11/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 760fc7070f0673f61a28cd126be0c6542ac4ee93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039859"
---
# <a name="tutorial-deploy-sql-server-big-data-clusters-in-ad-mode-on-azure-kubernetes-services-aks"></a>教程：在 Azure Kubernetes 服务 (AKS) 上的 AD 模式下部署 SQL Server 大数据群集

本文介绍如何使用参考体系结构在 Active Directory 身份验证模式下部署 SQL Server 大数据群集 (BDC)。 参考体系结构将本地 Active Directory 域服务 (AD DS) 扩展到 Azure。 可以使用 [Azure 构建基块](https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks)通过 [Azure 体系结构中心](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-extend-domain)进行部署。

## <a name="prerequisites"></a>先决条件

在部署 SQL Server 大数据群集之前，需要执行以下操作：

* 访问用于管理的 Azure VM。 此 VM 需要访问你将在其中部署 BDC 的 Azure 虚拟网络 (VNet)。 它必须位于同一 VNet 中，或位于[对等互连 VNet](/azure/virtual-network/virtual-network-manage-peering) 上。
* 在管理 VM 上[安装大数据工具](deploy-big-data-tools.md)。
* 准备在本地 AD 控制器中的 [Active Directory 身份验证模式](active-directory-prerequisites.md)下部署群集。

## <a name="create-aks-subnet"></a>创建 AKS 子网

1. 设置环境变量

   ```console
   export REGION_NAME=< your Azure Region >
   export RESOURCE_GROUP=<your resource group >
   export SUBNET_NAME=aks-subnet
   export VNet_NAME= adds-vnet
   export AKS_NAME= <your aks cluster name>
   ```

1. 创建 AKS 子网

   ```console
   SUBNET_ID=$(az network vnet subnet show \
    --resource-group $RESOURCE_GROUP \
    --vnet-name $VNet_NAME \
    --name $SUBNET_NAME \
    --query id -o tsv)
   ```

以下屏幕截图显示我们如何计划子网驻留在体系结构的 VNet 中。

:::image type="content" source="media/active-directory-deployment-aks/ad-in-aks-diagram.png" alt-text="具有 AD 的 AKS 群集和 SQL Server 大数据群集":::

## <a name="create-an-aks-private-cluster"></a>创建 AKS 专用群集

可以使用以下命令部署 AKS 专用群集。 如果不需要专用群集，请在命令中删除 `--enable-private-cluster` 参数。 如需了解其他要求，请参阅[如何部署 Azure Kubernetes 群集 (AKS)](/azure/aks/tutorial-kubernetes-deploy-cluster)。

```azurecli
az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.3.0.10 \
    --service-cidr 10.3.0.0/24 \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

部署 AKS 群集后，[连接到 AKS 群集](/azure/aks/tutorial-kubernetes-deploy-cluster#connect-to-cluster-using-kubectl)。

## <a name="verify-reverse-dns-entry-for-domain-controller"></a>验证域控制器的反向 DNS 条目

在 AKS 群集中的 AD 模式下启动 BDC 部署之前，请验证域控制器本身是否已在 DNS 服务器中注册 A 记录和 PTR 记录（反向 DNS 条目） 。

如需验证此设置，可运行 `nslookup` 命令或运行 [PowerShell 脚本](troubleshoot-ad-reverse-lookup-zone.md)以确认你已配置反向 DNS 条目（PTR 记录）。

## <a name="create-bdc-deployment-profile"></a>创建 BDC 部署配置文件

以下命令创建部署配置文件：

```console
azdata bdc config init --source kubeadm-prod  --target bdc-ad-aks
```

使用以下命令可为部署配置文件设置参数。

### <a name="controljson"></a>control.json

```console
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.storage.logs.className=default"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[0].dnsName=controller.contoso.com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[1].dnsName=proxys.contoso.com"

# security settings 
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.dnsIpAddresses=[\"192.168.0.4\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.domainControllerFullyQualifiedDns=[\"ad1.contoso.com\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.domainDnsName=contoso.com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.clusterUsers=[\"bdcusersgroup\"]"
```

### <a name="bdcjson"></a>bdc.json

```console
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].dnsName=master.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[1].dnsName=mastersec.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[1].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].dnsName=gateway.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].dnsName=approxy.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="initiate-deployment"></a>启动部署

以下命令将启动 BDC 部署：

```console
azdata bdc create --config-profile bdc-ad-aks --accept-eula yes
```

## <a name="next-steps"></a>后续步骤

[使用 Azure Data Studio 连接到 SQL Server 大数据群集](connect-to-big-data-cluster.md)

[教程：将示例数据加载到 SQL Server 大数据群集中](tutorial-load-sample-data.md)