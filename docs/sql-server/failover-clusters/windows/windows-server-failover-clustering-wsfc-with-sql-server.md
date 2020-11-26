---
title: Windows Server 故障转移群集与 SQL Server
description: 了解如何将 Windows Server 故障转移群集服务与 SQL Server 和故障转移群集实例一起使用。
ms.custom: seo-lt-2019
ms.date: 01/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- Windows Server Failover Clustering, with SQL Server
- WSFC, with SQL Server
- quorum [SQL Server]
- failover clustering [SQL Server], Always On Availability Groups
ms.assetid: 79d2ea5a-edd8-4b3b-9502-96202057b01a
author: cawrites
ms.author: chadam
ms.openlocfilehash: ac7272b1a8548f6265c9271a4f6bdb0799f02862
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127564"
---
# <a name="windows-server-failover-clustering-with-sql-server"></a>Windows Server 故障转移群集与 SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Windows Server 故障转移群集 (WSFC) 是一组独立的服务器，它们共同协作以提高应用程序和服务的可用性  。 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 利用 WSFC 服务和功能支持 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例。  
  
   
##  <a name="terms-and-definitions"></a><a name="TermsAndDefs"></a> 术语和定义  
 Windows Server 故障转移群集 (WSFC) WSFC 是一组独立的服务器，它们共同协作以提高应用程序和服务的可用性。  
  
 节点  
 参与 WSFC 的服务器。
  
 群集资源 (Cluster resource)  
 节点可以拥有的物理实体或逻辑实体，可联机和脱机、在节点间移动和作为群集对象进行管理。 在任何时间点，群集资源只能为单个节点所拥有。  
  
 角色  
 作为单个群集对象管理的群集资源集合，可提供特定功能。 对于 SQL Server，角色为 Always On 可用性组 (AG) 或 Always On 故障转移群集实例 (FCI)。 角色包含 AG 或 FCI 所需的所有群集资源。 故障转移和故障回复始终作用于角色上下文。 对于 FCI，角色包含 IP 地址资源、网络名称资源和 SQL Server 资源。 AG 角色包含 AG 资源，如果已配置侦听程序，则包含网络名称和 IP 资源。 

 网络名称资源 (Network name resource)  
 作为群集资源进行管理的逻辑服务器名称。 网络名称资源必须与一个 IP 地址资源结合使用。 这些项可能需要 Active Directory 域服务和/或 DNS 中的对象。 
  
 资源依赖项 (Resource dependency)  
 一个资源所依赖的另一个资源。 如果资源 A 依赖于资源 B，则 B 是 A 的依赖项。如果没有资源 B，则无法启动资源 A。  
  
  
 首选所有者 (Preferred owner)  
 资源组优先运行的节点。 每个资源组都按优先顺序与首选的所有者列表关联。 在自动故障转移过程中，资源组将移动到首选所有者列表中的下一个首选节点。  
  
 可能的所有者 (Possible owner)  
 可在其上运行资源的辅助节点。 每个资源组都与一系列可能的所有者关联。 角色仅可故障转移到作为可能的所有者列出的节点。   
  
 仲裁模式 (Quorum mode)  
 故障转移群集中的仲裁配置，用于确定群集可以承受的节点故障数。  
  
 强制仲裁  
 即使仅有仲裁所需的少数元素进行通信，该过程也会启动群集。  
  

##  <a name="overview-of-windows-server-failover-clustering"></a><a name="Overview"></a> Windows Server 故障转移群集概述  
 Windows Server 故障转移群集提供了各种基础结构功能来支持所承载的服务器应用程序（如 Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 和 Microsoft Exchange）的高可用性和灾难恢复方案。  如果一个群集节点或服务失败，则该节点上承载的服务可在一个称为“故障转移”的过程中自动或手动转移到另一个可用节点。  
  
 WSFC 中的节点协同工作，共同提供以下类型的功能：  
  
-   **分布式元数据和通知。** 群集中的每个节点上维护着 WSFC 服务和承载的应用程序元数据。 除了承载的应用程序设置之外，此元数据还包括 WSFC 配置和状态。 对一个节点的元数据或状态进行的更改会自动传播到 WSFC 中的其他节点。  
  
-   **资源管理。** WSFC 中的各节点可能提供物理资源，如直接连接存储、网络接口和对共享磁盘存储的访问。 承载的应用程序将其本身注册为群集资源，并可配置启动和运行状况对于其他资源的依赖关系。  
  
-   运行状况监视。  节点间和主节点运行状况检测是通过结合使用信号样式的网络通信和资源监视来实现的。 WSFC 的总体运行状况是由 WSFC 中节点仲裁的投票决定。  
  
-   **故障转移协调。** 每个资源都配置为由主节点承载，并且每个资源均可自动或手动转移到一个或多个辅助节点。 基于运行状况的故障转移策略控制节点之间资源所有权的自动转移。 在发生故障转移时通知节点和承载的应用程序，以便其做出适当的响应。  
  
 有关详细信息，请参阅： [故障转移群集概述 - Windows Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831579(v=ws.11))  
  
##  <a name="sql-server-always-on-technologies-and-wsfc"></a><a name="AlwaysOnWsfcTech"></a> SQL Server AlwaysOn 技术和 WSFC  
 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] Always On  是一个充分利用 WSFC 的高可用性和灾难恢复解决方案。 Always On 功能提供一个灵活的集成解决方案，用于提高应用程序可用性，提供更好的硬件投资回报，并简化高可用性部署和管理。  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 和 AlwaysOn 故障转移群集实例将 WSFC 用作一种平台技术，将组件注册为 WSFC 群集资源。   相关的资源合并为一个“角色”，这些资源可能依赖于其他 WSFC 群集资源。 这样，WSFC 就可以感测并标明是否需要重启 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，或自动将其故障转移到 WSFC 中的不同服务器节点上。  
  
> **重要说明!!** 若要充分利用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] AlwaysOn 技术，应该应用多个与 WSFC 相关的先决条件。  
>   
>  有关详细信息，请参阅：[针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
### <a name="instance-level-high-availability-with-always-on-failover-cluster-instances"></a>实例级高可用性以及 AlwaysOn 故障转移群集实例  
 Always On 故障转移群集实例  (FCI) 是安装在 WSFC 中的节点上的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 这种类型的实例依赖于存储和虚拟网络名称资源。 存储可以使用光纤通道、iSCSI、FCoE 或 SAS 进行共享磁盘存储，也可以将本地附加存储与[存储空间直通 (S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-overview) 配合使用。 虚拟网络名称资源依赖于一个或多个虚拟 IP 地址（每个地址位于不同子网中）。 SQL Server 服务和 SQL Server 代理服务也是资源，且都依赖于存储和虚拟网络名称资源。  
  
 发生故障转移时，WSFC 服务将实例的资源所有权转移到指定的故障转移节点。 然后， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例在故障转移节点上重新启动，数据库恢复如常。 在任何给定时刻，群集中只有一个节点可以承载 FCI 和基础资源。  
  
> **注意：**  AlwaysOn 故障转移群集实例要求使用对称共享磁盘存储，如存储区域网络 (SAN) 或 SMB 文件共享。  共享磁盘存储卷必须可用于 WSFC 群集中所有可能的故障转移节点。  
  
 有关详细信息，请参阅：[AlwaysOn 故障转移群集实例 (SQL Server)](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)  
  
### <a name="database-level-high-availability-with-sshadr"></a>数据库级高可用性与 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]  
  Always On“可用性组”(AG) 是一个或多个一同故障转移的用户数据库。 一个可用性组包含一个主“可用性副本”  和一至四个次要副本，这些副本通过基于 SQL Server 日志的数据移动来实现数据保护以进行维护，无需共享存储。 每个副本均由 WSFC 的不同节点上的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例承载。 可用性组和相应的虚拟网络名称注册为 WSFC 群集中的资源。  
  
  主副本节点上的“可用性组侦听器” [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 响应要求连接到虚拟网络名称的传入客户端请求，侦听器基于连接字符串中的属性将每个请求重定向到相应的  实例。  
  
 当发生故障转移时，不是将共享物理资源的所有权转移到另一个节点，而是利用 WSFC 重新配置另一个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上的辅助副本，使其成为可用性组的主副本。 然后，将可用性组的虚拟网络名称资源转移到该实例。  
  
 在任何给定时刻，只有单个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例可承载可用性组数据库的主副本，而所有关联的辅助副本都必须分别驻留在单独的实例上，并且每个实例必须驻留在单独的物理节点上。  
  
> **注意：** [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 不要求部署故障转移群集实例或使用异步共享存储（SAN 或 SMB）。  
>   
>  故障转移群集实例 (FCI) 可与可用性组结合使用，以提高可用性副本的可用性。 但是，为了防止 WSFC 群集中出现潜在的争用情况，不支持可用性组自动故障转移到驻留在 FCI 上的副本，也不支持从驻留在 FCI 上的副本自动故障转移到可用性组。  
  
 有关详细信息，请参阅： [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
##  <a name="wsfc-health-monitoring-and-failover"></a><a name="AlwaysOnWsfcHealth"></a> WSFC 运行状况监视和故障转移  
 AlwaysOn 解决方案的高可用性是通过积极主动地监视物理和逻辑 WSFC 群集资源的运行状况，以及自动故障转移到冗余硬件和重新配置冗余硬件来实现的。   系统管理员还可以对可用性组或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例启动从一个节点到另一个节点的“手动故障转移”。  
  
### <a name="failover-policies-for-nodes-failover-cluster-instances-and-availability-groups"></a>节点、故障转移群集实例和可用性组的故障转移策略  
 “故障转移策略”  是在 WSFC 节点、[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障群集实例 (FCI) 以及可用性组级别配置的。   这些策略基于非正常运行的群集资源状态和节点响应的严重性、持续时间和频率，它们可以触发服务重新启动或将群集资源从一个节点“自动故障转移” [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 到另一个节点，或者可以触发将可用性组主副本从一个  实例移到另一个此类实例。  
  
 可用性组副本的故障转移不影响基础 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。  FCI 的故障转移将随实例一起移动所承载的可用性组副本。  
  
 有关详细信息，请参阅： [故障转移群集实例的故障转移策略](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
### <a name="wsfc-resource-health-detection"></a>WSFC 资源运行状况检测  
 WSFC 中的每个资源都可以定期或按需报告其状态和运行状况。 很多情况可以指示资源故障；例如，电源故障、磁盘或内存错误、网络通信错误或服务不响应。  
  
 WSFC 资源（如网络、存储或服务）可能彼此依赖。 资源的累计运行状况由持续汇总其运行状况和其每个资源依赖项的运行状况来确定。  
  
### <a name="wsfc-inter-node-health-detection-and-quorum-voting"></a>WSFC 节点间运行状况检测和仲裁投票  
 WSFC 中的每个节点都参与周期性信号通信，以便与其他节点共享该节点的运行状况。 未响应的节点被认为是处于故障状态。  
  
  “仲裁”是通过确保 WSFC 中有足够的资源处于联机状态，来帮助确保 WSFC 启动和运行的一种机制。 如果 WSFC 的投票足够，则它很正常并且能够提供节点级别的容错。  
  
  “仲裁模式”在 WSFC 中配置，用于指示用于仲裁投票的方法以及执行自动故障转移或使群集脱机的时间。 
  
> **提示！！** WSFC 中最好始终有奇数数目的仲裁投票。  为进行仲裁投票，不必在群集的所有节点上安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 其他服务器可以充当仲裁成员，或者可以将 WSFC 仲裁模式配置为将远程文件共享用作补救措施。  
>   
>  有关详细信息，请参阅： [WSFC 仲裁模式和投票配置 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md)  
  
### <a name="disaster-recovery-through-forcing-quorum"></a>通过强制仲裁进行灾难恢复  
 根据操作实践和 WSFC 配置，可以引发自动故障转移和手动故障转移，同时仍保持可靠、容错的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Always On 解决方案。 但是，如果 WSFC 中合格投票节点的仲裁彼此之间无法通信，或者 WSFC 群集无法执行运行状况验证，则 WSFC 可能会脱机。  
  
 如果 WSFC 由于计划外灾难或由于持续的硬件或通信故障而导致脱机，则需要管理员手动干预才能“强制仲裁”  ，并在非容错配置中将仍有效的群集节点变为联机状态。  
  
 之后，还必须执行一系列步骤来重新配置 WSFC，恢复受影响的数据库副本，并重新建立一个新仲裁。  
  
 有关详细信息，请参阅： [通过强制仲裁进行 WSFC 灾难恢复 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)  
  
##  <a name="relationship-of-sql-server-alwayson-components-to-wsfc"></a><a name="AlwaysOnWsfcRelationship"></a> SQL Server AlwaysOn 组件与 WSFC 的关系  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] AlwaysOn 和 WSFC 功能和组件之间存在多层关系。  
  
 AlwaysOn 可用性组承载于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上。  
 指定将连接到主数据库或辅助数据库的逻辑可用性组侦听程序名称的客户端请求将重定向至基础 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 的相应实例网络名称。  
  
 “SQL Server 实例”当前承载于单个节点上。  
 如果存在，则独立的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例始终驻留在具有静态实例网络名称的单个“节点”上。  如果存在，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 在两个或多个具有单个虚拟“实例网络名称”的可能的故障转移节点之一上处于活动状态。  
  
 “节点”为 WSFC 群集的成员。  
 每个节点上存储了所有节点的WSFC 配置元数据和状态。 每个服务器都为用户或系统数据库提供非对称存储或共享存储 (SAN) 卷。 在一个或多个 IP 子网上，每个服务器都至少具有一个物理网络接口。  
  
 WSFC 监视一组服务器的运行状况并管理它们的配置。  
 WSFC 机制将对 WSFC 配置元数据和状态的更改传播到 WSFC 中的所有节点。 如果使用磁盘见证，则还会将元数据存储在此处。 默认情况下，WSFC 的每个节点都会获得仲裁投票，并且如果已配置见证，可在必要时使用见证。
 
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 注册表项是 WSFC 群集的子项。  
 如果先删除然后重新创建了 WSFC，则必须在原始 WSFC 上启用了 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的每个服务器实例上都禁用然后重新启用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 功能。 有关详细信息，请参阅[启用和禁用 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)。  
  
 ![SQL Server AlwaysOn 组件上下文关系图](../../../sql-server/failover-clusters/windows/media/alwaysoncomponentcontextdiagram.gif "SQL Server AlwaysOn 组件上下文关系图")  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [查看群集仲裁 NodeWeight 设置](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [配置群集仲裁 NodeWeight 设置](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
-   [在无仲裁情况下强制启动 WSFC 群集](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [Windows Server 技术：故障转移群集](https://technet.microsoft.com/library/cc732488\(v=WS.10\).aspx)  

-   [存储空间直通 \(S2D\) 概述](/windows-server/storage/storage-spaces/storage-spaces-direct-overview)

-   [Windows Server 2008 R2 中的故障转移群集](https://technet.microsoft.com/library/ff182338\(WS.10\).aspx)  
  
-   [查看故障转移群集的事件和日志](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Get-ClusterLog 故障转移群集 Cmdlet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 故障转移群集实例 (SQL Server)](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [WSFC 仲裁模式和投票配置 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md)   
 [故障转移群集实例的故障转移策略](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)   
 [通过强制仲裁进行 WSFC 灾难恢复 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)  
 [SQL Server 2016 支持 Windows Server 2016 存储空间直通](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
