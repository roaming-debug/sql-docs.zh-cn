---
title: 在 Azure Kubernetes 服务 (AKS) 上的 Active Directory 中部署
titleSuffix: SQL Server Big Data Cluster
description: 介绍有关如何在 Azure Kubernetes 服务 (AKS) 上，在 AD 模式下部署 SQL Server 大数据群集的概念和计划信息。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 11/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a0407589eefab4d5e997d582fee900a20dfcb0a3
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837008"
---
# <a name="deploy-sql-server-big-data-clusters-in-ad-mode-on-azure-kubernetes-services-aks"></a>在 Azure Kubernetes 服务 (AKS) 上的 AD 模式下部署 SQL Server 大数据群集

SQL Server 大数据群集支持 [Active Directory (AD) 部署模式](./active-directory-prerequisites.md)用于标识和访问管理 (IAM)。 Azure Kubernetes 服务 (AKS) 的 IAM 难度大，因为 SQL Server 不支持受 Microsoft 标识平台广泛支持的行业标准协议（如 OAuth 2.0 和 OpenID Connect）。  

本文介绍如何在 [Azure Kubernetes 服务 (AKS)](/azure/aks/intro-kubernetes) 中进行部署的同时，在 AD 模式下部署大数据群集 (BDC)。 

## <a name="architecture-topologies"></a>体系结构拓扑

Active Directory 域服务 (AD DS) 在 Azure 虚拟机 (VM) 上运行的方式[与](/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm)它在许多本地实例上运行的方式相同。  在 Azure 中提升新域控制器后，设置虚拟网络的主 DNS 服务器和辅助 DNS 服务器，将需要降级为第三级或更低级别的本地 DNS 服务器降级。 通过 AD 身份验证，[Linux 上的加入域的客户端能够使用其域凭据和 Kerberos 协议向 SQL Server 进行身份验证](../linux/sql-server-linux-active-directory-auth-overview.md)。

可以通过多种方式在 AKS 中的 AD 模式下部署 BDC。  本文介绍两种方法，使用它们更易于实现和集成现有企业级体系结构：

* **将本地 Active Directory 域扩展到 Azure。** 此方法在 Azure 上使用 Active Directory 域服务 (AD DS) [支持 Active Directory 环境](/azure/architecture/reference-architectures/identity/adds-extend-domain)提供分布式身份验证服务。 需要复制本地 Active Directory 域服务 (AD DS)，以减少由于从云向本地 AD DS 发送身份验证请求而导致的延迟。 此解决方案的典型用例是：应用程序部分在本地托管，部分在 Azure 托管，身份验证请求需要来回传送。

   请通过[此参考体系结构](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-extend-domain)了解如何分步部署此解决方案。

* **将 Active Directory 域服务 (AD DS) 资源林扩展到 Azure。** 在此体系结构中，需要在 Azure 中创建受本地 AD 林信任的新域。 此体系结构显示了[ Azure 中从域到本地林的单向信任](/azure/architecture/reference-architectures/identity/adds-forest)。

   信任支持本地用户访问 Azure 的域中的资源。 请通过[此参考体系结构](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-forest)了解如何分步部署此解决方案。

通过上述参考体系结构，可以创建一个登陆区域，其中包含从头开始部署的所有资源或基于现有体系结构的任何其他变通方法。 除了这些参考体系结构外，还应在保留在目标 VNet 或对等互连 VNet 中的单独子网上的 AKS 群集中部署 BDC。

下图表示典型的体系结构：

:::image type="content" source="media/active-directory-deployment-aks/ad-in-aks-diagram.png" alt-text="具有 AD 的 AKS 群集和 SQL Server 大数据群集":::

## <a name="recommendations"></a>建议

以下建议适用于 AKS 上 AD 模式下的大多数 BDC 部署。 每个组件中将提到可用选项，以提供更好地集成企业级体系结构的指南。

### <a name="networking-recommendations"></a>网络建议

可以使用一些关键组件将本地环境连接到 Azure：

* **Azure VPN 网关**：VPN 网关是特定类型的虚拟网关，用于跨公共 Internet 在 Azure 虚拟网络和本地位置之间发送加密的流量。 你将使用 Azure 虚拟网络网关和本地虚拟网络网关。 有关如何配置这些应用程序的信息，请参阅[什么是 VPN 网关](/azure/vpn-gateway/vpn-gateway-about-vpngateways)。
* **Azure ExpressRoute**：ExpressRoute 连接不通过公共 Internet，与通过 Internet 的典型连接相比，提供更高的安全性、可靠性、速度和更低的延迟。 所选的连接选项会影响解决方案的延迟、性能和 SLA 级别，具体取决于 SKU。 有关具体信息，请参阅 [ExpressRoute 虚拟网络网关简介](/azure/expressroute/expressroute-about-virtual-network-gateways)。

大多数客户使用跳转框或 [Azure Bastion](/azure/bastion/bastion-overview) 访问其他 Azure 基础结构。 利用 Azure专用链接，可以安全地访问 Azure PaaS 服务（包括此场景中的 AKS），以及虚拟网络中的专用终结点上的其他 Azure 托管服务。 虚拟网络与服务之间的流量将通过 Microsoft 主干网络，因此不会向公共 Internet 泄露。 你还可以在虚拟网络中创建自己的专用链接服务，并将其专门提供给自己的客户。

### <a name="active-directory-and-azure-recommendation"></a>Active Directory 和 Azure 建议

本地 AD DS 存储有关用户帐户的信息，并支持同一网络上的其他授权用户通过对与用户、计算机、应用程序或安全边界中包含的其他资源关联的标识进行身份验证来访问此信息。 在大多数混合场景中，用户身份验证通过与本地 AD DS 环境建立的 VPN 网关或 ExpressRoute 连接运行。  

对于 AD 模式下的 BDC 部署，用于[将本地 Active Directory 与 Azure 集成](/azure/architecture/reference-architectures/identity/)的解决方案必须具有以下先决条件：

* [AD 帐户具有特定权限](active-directory-prerequisites.md)可以在本地 Active directory 中提供的组织单位 (OU) 内创建用户、组和计算机帐户。
* 用于[解析内部 DNS](active-directory-dns-reconciliation.md) 的 DNS 服务器。 它必须在 DNS 服务器中包含 A（正向查找）和 PTR（反向查找）记录，并使用该域中的名称 。 指定 BDC 部署配置文件中的域 DNS 设置。  

## <a name="next-steps"></a>后续步骤

[教程：在 Azure Kubernetes 服务 (AKS) 上的 AD 模式下部署 SQL Server 大数据群集](active-directory-deployment-aks-tutorial.md)