---
title: SQL Server 多子网群集
description: 了解如何在多子网环境中配置 SQL Server 故障转移群集实例，该实例除了提供高可用性之外还提供灾难恢复。
ms.custom: seo-lt-2019
ms.date: 09/01/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- stretch cluster
- Availability Groups [SQL Server], WSFC clusters
- failover clustering [SQL Server], AlwaysOn Availability Groups
- multi-site failover cluster
- failover clustering [SQL Server]
ms.assetid: cd909612-99cc-4962-a8fb-e9a5b918e221
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4c230a793b81960b29e66813ba392eeb6395d5ee
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642722"
---
# <a name="sql-server-multi-subnet-clustering-sql-server"></a>SQL Server 多子网群集 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 多子网故障转移群集是一种配置，其中，每个故障转移群集节点都连接到其他子网或其他子网组。 这些子网可位于同一位置或在地理上分散的地点中。 跨地理上分散的站点进行群集有时称为拉伸群集。 因为没有所有节点都可以访问的共享存储，所以在多个子网上的数据存储之间应该复制数据。 对于数据复制，有多个可用数据的副本。 因此，多子网故障转移群集除了具备高可用性之外，还提供了灾难恢复解决方案。  
  
   
##  <a name="sql-server-multi-subnet-failover-cluster-two-nodes-two-subnets"></a><a name="VisualElement"></a> SQL Server 多子网故障转移群集（两个节点，两个子网）  
 下图表示 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]中的一个两节点、两子网的故障转移群集实例 (FCI)。  
  
 ![包含 MultiSubnetFailover 的多子网体系结构](../../../sql-server/failover-clusters/windows/media/multi-subnet-architecture-withmultisubnetfailoverparam.png "包含 MultiSubnetFailover 的多子网体系结构")  
  
  
##  <a name="multi-subnet-failover-cluster-instance-configurations"></a><a name="Configurations"></a> 多子网故障转移群集实例配置  
 以下是使用多个子网的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 的一些示例：  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI SQLCLUST1 包括 Node1 和 Node2。 节点 1 连接到 Subnet1。 Node2 连接到 Subnet2。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序将此配置视作一个多子网群集，并且将 IP 地址资源依赖关系设置为 **OR**。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI SQLCLUST1 包括 Node1、Node2 和 Node3。 Node1 和 Node2 连接到 Subnet1。 Node3 连接到 Subnet2。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序将此配置视作一个多子网群集，并且将 IP 地址资源依赖关系设置为 **OR**。 因为 Node1 和 Node2 位于同一子网上，所以此配置还提供本地高可用性。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI SQLCLUST1 包括 Node1 和 Node2。 Node1 位于 Subnet1 上。 Node2 位于 Subnet1 和 Subnet2 上。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序将此配置视作一个多子网群集，并且将 IP 地址资源依赖关系设置为 **OR**。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI SQLCLUST1 包括 Node1 和 Node2。 Node1 连接到 Subnet1 和 Subnet2。 Node2 也连接到 Subnet1 和 Subnet2。 **安装程序将 IP 地址资源依赖关系设置为** AND [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。  
  
    > **注意：** 此配置不视作多子网故障转移群集配置，因为群集的节点位于同一组子网中。  
  
##  <a name="ip-address-resource-considerations"></a><a name="ComponentsAndConcepts"></a> IP 地址资源注意事项  
 在一个多子网故障转移群集配置中，IP 地址不由该故障转移群集中的所有节点所拥有，并且在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 启动期间可能不是全都处于联机状态。 从 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]开始，您可以将 IP 地址资源依赖关系设置为 **OR**。 这使得 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可以在存在至少一个它可以绑定到的有效 IP 地址时处于联机状态。  
  
  > [!NOTE] 
  > - 在早于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]版本中，在多站点群集配置中使用了拉伸 V-LAN 技术，以便为跨站点的故障转移公开单个 IP 地址。 通过 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 跨不同子网对节点建立群集的新功能，您现在无需实现拉伸 V-LAN 技术，便可以跨多个站点配置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集。  

  
### <a name="ip-address-resource-or-dependency-considerations"></a>IP 地址资源 OR 依赖关系注意事项  
 如果您将 IP 地址资源依赖关系设置为 **OR**，则可能要考虑以下故障转移行为：  
  
-   在当前拥有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集资源组的节点上的 IP 地址之一出现失败时，在该节点上有效的所有 IP 地址都失败前，将不自动触发故障转移。  
  
-   当发生故障转移时，如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可以绑定到在当前节点上有效的至少一个 IP 地址，则它将进入联机状态。 在启动时未绑定到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的 IP 地址将在错误日志中列出。  
  
   
 并行安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 与 [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)]的独立实例时，请注意避免 IP 地址上的 TCP 端口号冲突。 当 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 的两个实例都配置为使用默认 TCP 端口 (1433) 时，通常会发生冲突。 要避免冲突，请将一个实例配置为使用非默认的固定端口。 在独立实例上配置固定端口通常是最简单的。 若将 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 配置为使用不同的端口，则在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 未能连接到备用节点时，将防止出现会阻止实例启动的意外 IP 地址/TCP 端口冲突。  
  
##  <a name="client-recovery-latency-during-failover"></a><a name="DNS"></a> 故障转移期间的客户端恢复延迟  
 默认情况下，多子网 FCI 会针对其网络名称启用 RegisterAllProvidersIP 群集资源。 在多子网配置中，将在 DNS 服务器上注册网络名称的联机和脱机 IP 地址。 之后，客户端应用程序会从 DNS 服务器检索所有已注册的 IP 地址，并尝试按顺序或并行连接到这些地址。 这意味着，多子网故障转移中的客户端恢复时间不再依赖 DNS 更新延迟。 默认情况下，客户端会按顺序尝试 IP 地址。 当客户端在其连接字符串中使用新的可选 **MultiSubnetFailover=True** 参数时，它将改为同时尝试 IP 地址并连接到第一台响应的服务器。 这有助于在发生故障转移时最大程度地减少客户端恢复延迟。 有关详细信息，请参阅 [AlwaysOn 客户端连接 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md) 和 [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)。  
  
 对于旧版客户端库或第三方数据访问接口，您不能在连接字符串中使用 **MultiSubnetFailover** 参数。 为了帮助确保您的客户端应用程序在 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]中以最佳方式使用多子网 FCI，请尝试按照 21 秒的间隔为其他每个 IP 地址调整客户端连接字符串中的连接超时。 这将确保客户端的重新连接尝试在它能够循环访问多子网 FCI 中的所有 IP 地址之前不会超时。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Management Studio 和 **sqlcmd** 的默认客户端连接超时期限为 15 秒。  
 
 > [!NOTE]
 > - 若要使用多个子网且有静态 DNS，必须先制定用于更新与侦听器关联的 DNS 记录的流程，再执行故障转移，否则网络名称将无法联机。
  
   
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
|内容说明|主题|  
|-------------------------|-----------|  
|安装 SQL Server 故障转移群集|[创建新的 SQL Server 故障转移群集（安装程序）](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)|  
|您的现有 SQL Server 故障转移群集的就地升级|[升级 SQL Server 故障转移群集实例（安装程序）](./upgrade-a-sql-server-failover-cluster-instance.md)|  
|维护您的现有 SQL Server 故障转移群集|[在 SQL Server 故障转移群集中添加或删除节点（安装程序）](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)|  
|使用“故障转移群集管理”管理单元来查看 WSFC 事件和日志|[查看故障转移群集的事件和日志](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)|  
|使用 Windows PowerShell 为 WSFC 故障转移群集中的所有节点（或特定节点）创建日志文件|[Get-ClusterLog 故障转移群集 Cmdlet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))|  
  

  
