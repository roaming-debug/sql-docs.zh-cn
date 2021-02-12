---
title: 在 Azure Kubernetes 服务 (AKS) 专用群集中管理大数据群集 (BDC)
titleSuffix: SQL Server Big Data Cluster
description: 了解如何在 Azure Kubernetes 服务 (AKS) 专用群集中管理 SQL Server 大数据群集。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 768818a9e034961aa0460515e3db84ce3a4a8092
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043867"
---
# <a name="manage-big-data-cluster-in-aks-private-cluster"></a>在 AKS 专用群集中管理大数据群集

本文介绍如何使用在 Azure 中部署的 SQL Server 大数据群集 (BDC) 来管理 Azure Kubernetes Service (AKS) 专用群集。

如[创建专用群集](/azure/aks/private-clusters/)中所述，AKS 专用群集 API 服务器终结点没有公共 IP 地址。 若要管理 API 服务器，请使用有权访问 AKS 群集的 Azure 虚拟网络 (VNet) 的 VM。

## <a name="azure-vm---same-vnet"></a>Azure VM - 同一 VNet

最简单的方法是在与 AKS 群集所在的同一 VNet 中部署 Azure VM。

1. 在 AKS 群集所在的同一 VNET 中部署 Azure VM。 这有时称为 jumpbox。
1. 连接到该 VM，并[安装 SQL Server 2019 大数据工具](deployment-guidance.md#install-sql-server-2019-big-data-tools)。

出于安全考虑，可以为 API 服务器授权 IP 范围使用 AKS 功能来限制对 API 服务器的访问（在 AKS 控制平面上）。 受限访问允许使用特定的 IP 地址（例如 jumpbox VM 或管理 VM），或一组开发人员的 IP 地址范围和防火墙公共前端 IP 地址。

## <a name="other-options"></a>其他选项

使用 jumpbox 的替代方法包括：

* 在单独的网络中使用 VM 并设置[虚拟网络对等互联](/azure/virtual-network/virtual-network-peering-overview)到 VNet。

* [Azure ExpressRoute](/azure/expressroute/expressroute-introduction) 或 [VPN 网关](/azure/vpn-gateway/vpn-gateway-about-vpngateways)连接。

   [用于连接到专用群集的选项](/azure/aks/private-clusters#options-for-connecting-to-the-private-cluster)讨论了上述每种方法。

* 如果服务在 [Azure 标准负载均衡器](/azure/aks/load-balancer-standard)后运行，则可为 [Azure 专用链接](/azure/private-link/private-link-service-overview#limitations)启用此服务。 使用 Azure 专用链接，你可以从其他 Azure Vnet 启用专用访问。

* 在混合方案中，还可以设置 [Azure ExpressRoute](/azure/expressroute/expressroute-introduction) 或 [VPN 网关](/azure/vpn-gateway/vpn-gateway-about-vpngateways)连接。

## <a name="next-steps"></a>后续步骤

[使用 Azure Data Studio 连接到 SQL Server 大数据群集](connect-to-big-data-cluster.md)