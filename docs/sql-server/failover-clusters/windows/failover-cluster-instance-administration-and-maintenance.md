---
title: 故障转移群集实例管理和维护
description: 了解如何通过 SQL Server 安装程序或故障转移群集管理器管理单元来维护和管理 SQL Server 故障转移群集实例。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- user accounts [SQL Server], failover clustering
- clusters [SQL Server], maintaining
- nodes [Faillover Clustering]
- failover clustering [SQL Server], maintaining
- adding nodes
- virtual servers [SQL Server], removing nodes
- clustered instance of SQL Server
- nodes [Faillover Clustering], removing
- nodes [Faillover Clustering], adding
- service accounts [SQL Server]
- removing nodes
- virtual servers [SQL Server], adding nodes
ms.assetid: 2d5c63e9-8061-45c3-94db-8dd3100b8a91
author: cawrites
ms.author: chadam
ms.openlocfilehash: e2f519ae0fec2ba6e24e5e17832d7d6a33782133
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350872"
---
# <a name="failover-cluster-instance-administration-and-maintenance"></a>故障转移群集实例管理和维护
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  诸如从现有 Always On 故障转移群集实例 (FCI) 中添加或删除节点等此类维护任务均使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序来完成。 其他管理任务（如更改 IP 地址资源、从某些 FCI 情形中恢复）则使用故障转移群集管理器管理单元来完成，该管理单元专用于 Windows Server 故障转移群集 (WSFC) 服务。  
  
## <a name="maintaining-a-failover-cluster-instance"></a>维护故障转移群集实例  
 安装 FCI 后，您可以使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序对其进行更改或修复。 例如，您可以向 FCI 添加其他节点、将 FCI 作为独立实例运行或从 FCI 配置中删除节点。  
  
### <a name="adding-a-node-to-an-existing-failover-cluster-instance"></a>向现有故障转移群集实例添加节点  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序提供了用于维护现有 FCI 的选项。 如果选择此选项，则可通过在要添加到 FCI 的计算机上运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序，向 FCI 中添加其他节点。 有关详细信息，请参阅[创建新的 SQL Server 故障转移群集（安装程序）](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)和[在 SQL Server 故障转移群集中添加或删除节点（安装程序）](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。  
  
### <a name="removing-a-node-from-an-existing-failover-cluster-instance"></a>从现有故障转移群集实例删除节点  
 通过在要从 FCI 删除的计算机上运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序，可以从 FCI 中删除节点。 FCI 中的每个节点都可以看作是一个对等方，不依赖于 FCI 中的其他节点，因此，您可以删除其中的任何节点。 已损坏的节点不一定能被删除，并且删除过程将不会从不可用的节点中卸载 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 二进制文件。 可以随时将已删除的节点重新添加到 FCI。 有关详细信息，请参阅[在 SQL Server 故障转移群集中添加或删除节点（安装程序）](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。  
  
### <a name="changing-service-accounts"></a>更改服务帐户  
 在 FCI 节点关闭或脱机时，不得更改任何 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户的密码。 如果一定要这么做，则必须在所有节点都重新联机后再使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器重置密码。  
  
 如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的服务帐户不是群集中的管理员，则无法在该群集的任何节点上删除管理共享。 管理共享必须在群集中可用， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 才能运行。  
  
> [!IMPORTANT]  
>  请勿对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户和 WSFC 服务帐户使用同一帐户。 否则，如果更改了 WSFC 服务帐户的密码， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装将失败。  
  
 在 [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)]中，服务 SID 可用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户。 有关详细信息，请参阅 [配置 Windows 服务帐户和权限](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)预览版本升级问题的解答。  
  
## <a name="administering-a-failover-cluster-instance"></a>管理故障转移群集实例  
  
|任务说明|主题链接|  
|----------------------|----------------|  
|说明如何为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源添加依赖项。|[向 SQL Server 资源添加依赖项](../../../sql-server/failover-clusters/windows/add-dependencies-to-a-sql-server-resource.md)|  
|Kerberos 是一种网络身份验证协议，旨在为客户端/服务器应用程序提供严格的身份验证。 Kerberos 为互操作性奠定了基础，同时有助于增强企业范围的网络身份验证的安全性。 可以将 Kerberos 身份验证用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 独立实例或 Always On FCI。|[为 Kerberos 连接注册服务主体名称](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)。|  
|提供指向描述如何启用 Kerberos 身份验证的内容的链接||  
|介绍用于从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集故障中恢复的过程。|[从故障转移群集实例故障中恢复](../../../sql-server/failover-clusters/windows/recover-from-failover-cluster-instance-failure.md)|  
|介绍用于更改 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的 IP 地址资源的过程。|[更改故障转移群集实例的 IP 地址](../../../sql-server/failover-clusters/windows/change-the-ip-address-of-a-failover-cluster-instance.md)|  
  
## <a name="see-also"></a>另请参阅  
 [配置 HealthCheckTimeout 属性设置](../../../sql-server/failover-clusters/windows/configure-healthchecktimeout-property-settings.md)   
 [配置 FailureConditionLevel 属性设置](../../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md)   
 [查看和读取故障转移群集实例诊断日志](../../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md)  
  
  
