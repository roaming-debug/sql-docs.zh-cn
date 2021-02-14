---
title: 大数据群集部署中的 Active Directory 和 Kubernetes DNS 协调
description: 在 Active Directory 模式下配置用于 SQL Server 大数据群集的 DNS 协调
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a20afc81637eba30427b7b1062738b2d81ae67a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047877"
---
# <a name="active-directory-and-kubernetes-dns-reconciliation-in-big-data-clusters-deployments"></a>大数据群集部署中的 Active Directory 和 Kubernetes DNS 协调

本文介绍在部署大数据群集 (BDC) 时为适应 Active Directory 集成而需要解决的一些难题和相关解决方案。

## <a name="overview"></a>概述

如果部署大数据群集时没有进行 Active Directory 集成，则依靠 [Kubernetes CoreDNS](https://kubernetes.io/docs/tasks/administer-cluster/coredns/) 服务来实现内部 DNS 解析。 Kubernetes 使用内部域，例如 `<namespace>.svc.cluster.local`。 它在 DNS 服务器中创建 A（正向查找）和 PTR（反向查找）记录，并使用该域中的名称。

但是，如果启用 Active Directory 模式，会出现一个新域及其自己的 DNS 服务器集。 在内部名称解析期间，这可能会导致无法确定在正向和反向查找时要转到哪一组 DNS 服务器。

## <a name="challenges"></a>挑战

* 当部署新的 Kubernetes Pod 时，需要在两组 DNS 服务器中都添加 DNS 条目。 Kubernetes 负责在其 CoreDNS 中记录条目，但是 BDC 部署工作流负责在 Active Directory 域控制器 DNS 服务器中添加所需的条目。 同样，在删除 BDC 群集时，工作流必须确保删除这些条目。
* Active Directory DNS 服务器位于 Kubernetes 群集外部。 但是 BDC 在 Kubernetes 中有自己的 IP 空间，并且无法在位于外部的 DNS 服务器中为此 IP 空间创建记录，因为此 IP 空间在群集边界之外是不可见的。
* 当 Kubernetes 群集内发生故障转移事件时，必须同时更新 AD DNS 服务器中的记录。
* 除了 Pod 名称之外，还必须能够通过 AD 域名查找来寻址 Kubernetes 服务名称。 这在 Active Directory DNS 中会存在困难，因为一个服务名称可以映射到多个 Pod IP 地址。
* 组织性的 Active Directory DNS 服务器中的记录更新传播和复制延迟情况可能会非常严重，并超出 BDC 管理工作流的控制范围。 这会在部署和故障转移后立即影响 BDC 功能。 相反，Kubernetes CoreDNS 得益于其局部性，速度更快、效率更高。

## <a name="solution"></a>解决方案

为了克服上述困难，在 BDC 中实现的解决方案涉及了一个新的内部 CoreDNS 服务，该服务在 BDC 命名空间内进行管理。 这是 BDC 命名空间中的 Pod 出于名称解析目的要访问的唯一 DNS 服务。 新的 CoreDNS 服务将多个域的复杂性隐藏到“幕后”。

例如，在下图中可以看到，Pod 使用 BDC CoreDNS 服务器来解析名称。 Pod 不直接与 Kubernetes CoreDNS 服务器或 AD DNS 服务器交互。 

:::image type="content" source="media/active-directory-dns-reconciliation/bdc-ad-dns-reconciliation.png" alt-text="Pod 在自己的命名空间中连接到 CoreDNS 服务器":::

以下是一些实现细节，详尽展示了这种设计在 BDC 中的工作原理：

### <a name="centralized-management-of-multiple-domains"></a>集中管理多个域

名称查找的复杂性也通过集中处理的方式隐藏到内部 DNS 服务的“幕后”。 这避免了将管理多个域的负担转嫁到单个 Pod 上，并简化了设计。

### <a name="no-records-for-internal-pods-in-external-dns-servers"></a>外部 DNS 服务器中没有内部 Pod 的记录

由于这一设计原则，BDC 不必在外部 DNS 服务器的 Kubernetes IP 空间中为 Pod 创建和管理 A 和 PTR 记录。

### <a name="no-duplication-of-records"></a>无重复记录

多个位置的内部 DNS 记录。 这些记录的唯一存储是 Kubernetes CoreDNS。 BDC 内部 CoreDNS 将执行 DNS 查询的计算性重写并将其转发给 Kubernetes CoreDNS。

### <a name="computational-rewriting"></a>计算性重写

由于 BDC 不存储任何记录，BDC 负责将具有 AD 域名称的“传入正向查找查询”转换为具有 Kubernetes 域的名称，并将此查询转发给 Kubernetes CoreDNS。
例如，会将 `compute-0-0.contoso.local` 的传入查询重写为 `compute-0-0.compute-0-svc.contoso.svc.cluster.local`，并将此请求转发给 Kubernetes CoreDNS。
在反向查找的情况下，会使用内部 IP 将请求转发到 Kubernetes CoreDNS，并在响应客户端之前在那里将响应重写为 AD 域名。

### <a name="simplicity-in-pod-configurations"></a>Pod 配置的简易性

由于所有 BDC Pod 的 /etc/resolv.conf 中只引用了内部BDC CoreDNS，这简化了 Pod 的网络视图。 其中的复杂性被隐藏到内部 CoreDNS 的“幕后”。

### <a name="static-and-reliable-ip-address-for-dns-service"></a>DNS 服务的静态和可靠 IP 地址

BDC 部署的 CoreDNS 服务将具有可从所有 Pod 访问的经过注册的静态内部 IP。 这可确保无需更新 /etc/resolv.conf 中的值。

### <a name="service-load-balance-management-is-retained-by-kubernetes"></a>通过 Kubernetes 保留了服务负载平衡管理

当查找服务而不是单个 Pod 时，它们仍将转到 Kubernetes CoreDNS，因此 BDC 不负责专门实现 AD 域的负载平衡。

例如，如果正向查找请求是针对 `compute-0-svc.contoso.local`，则会将其重写为 ` compute-0-svc.contoso.svc.cluster`.local。 此请求将转发到 Kubernetes CoreDNS，并在该处实现负载平衡。 一个响应将是多个计算池实例（Pod 副本）之一的 IP 地址。

### <a name="scalability"></a>可伸缩性

由于 BDC 不存储任何记录，内部 BDC CoreDNS 可以在不需要跨多个副本实现状态保留和记录复制的情况下进行扩展。 如果 DNS 记录将存储在 BDC 中，则 BDC 还需要负责跨所有 Pod 复制状态。

### <a name="externally-visible-service-entries-stay-in-ad-dns"></a>外部可见的服务条目保留在 AD DNS 中

对于需要让 Kubernetes 群集外部的客户端能够访问的服务终结点，将在部署 BDC 时在 AD DNS 服务器中创建 DNS 条目。 用户通过部署配置的配置文件输入要从其进行注册的 DNS 名称。

### <a name="self-deprovisioning"></a>自行取消预配

删除 BDC 后，在取消预配群集时，就没有其他需要删除 DNS 条目的动态工作了。 远程 Active Directory DNS 中唯一需要清理的条目是用于外部服务的条目，它们的数量是静态的。 系统会自动将内部 DNS 条目与群集一并删除。

## <a name="next-steps"></a>后续步骤

- [在 Active Directory 模式下部署 SQL Server 大数据群集](active-directory-deploy.md)
- [在 Active Directory 模式下管理大数据群集](active-directory-objects.md)
- [在同一 Active Directory 域中部署多个 SQL Server 大数据群集](active-directory-deployment-background.md)
