---
title: AlwaysOn 故障转移群集实例
description: 了解 Always On 故障转移实例如何通过 SQL Server 中服务器实例级别上的冗余来实现本地高可用性。
ms.custom: seo-lt-2019
ms.date: 01/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- clustering [SQL Server]
- high availability [SQL Server], failover clustering
- virtual servers [SQL Server], about virtual servers
- clusters [SQL Server]
- servers [SQL Server], failover clustering
- resource groups [SQL Server]
- availability [SQL Server]
- failover clustering [SQL Server]
- AlwaysOn [SQL Server], see failover clustering [SQL Server]
ms.assetid: 86a15b33-4d03-4549-8ea2-b45e4f1baad7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 570380cd0a02eb1e05d6c953da112d78f25afeb1
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96121113"
---
# <a name="always-on-failover-cluster-instances-sql-server"></a>AlwaysOn 故障转移群集实例 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  作为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Always On 产品/服务的一部分，Always On 故障转移群集实例利用 Windows Server 故障转移群集 (WSFC) 功能通过冗余在实例级别（故障转移群集实例 (FCI)）提供了本地高可用性  。 FCI 是在 Windows Server 故障转移群集 (WSFC) 节点上和（可能）多个子网中安装的单个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 在网络中，FCI 显示为在单台计算机上运行的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，不过它提供了从一个 WSFC 节点到另一个 WSFC 节点的故障转移（如果当前节点不可用）。  
  
 FCI 可利用[可用性组](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)提供数据库级别的远程灾难恢复。 有关详细信息，请参阅[故障转移群集和可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)。  
 
 > [!NOTE]  
 > Windows Server 2016 Datacenter 版引入了对存储空间直通 (S2D) 的支持。 SQL Server 故障转移群集实例支持将 S2D 用于群集存储资源。 有关详细信息，请参阅 [Windows Server 2016 中的存储空间直通](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)。
 > 
 >故障转移群集实例还支持群集共享卷 (CSV)。 有关详细信息，请参阅 [了解故障转移群集中的群集共享卷](https://technet.microsoft.com/library/dd759255.aspx)。 
   
 **本主题内容：**  
  
-   [优点](#Benefits)  
  
-   [建议](#Recommendations)  
  
-   [故障转移群集实例概述](#Overview)  
  
-   [故障转移群集实例的元素](#FCIelements)  
  
-   [SQL Server 故障转移的概念和任务](#ConceptsAndTasks)  
  
-   [相关主题](#RelatedTopics)  
  
##  <a name="benefits-of-a-failover-cluster-instance"></a><a name="Benefits"></a> 故障转移群集实例的优点  
 当服务器上出现硬件或软件故障时，连接到该服务器的应用程序或客户端将会停机。 在将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例配置为 FCI（而非独立实例）时，该 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的高可用性受到 FCI 中提供的冗余节点的保护。 在 FCI 中，一次只能有一个节点拥有 WSFC 资源组。 在出现故障（硬件故障、操作系统故障、应用程序或服务故障）或进行计划的升级时，该资源组的所有权就会转移至另一个 WSFC 节点。 此过程对于连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的客户端或应用程序是透明的，可以最大限度地缩短出现故障时应用程序或客户端的停机时间。 以下列出了 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例提供的一些主要优点：  
  
-   通过冗余提供实例级的保护  
  
-   在出现故障（硬件故障、操作系统故障、应用程序或服务故障）时自动进行故障转移  
  
    > [!IMPORTANT]  
    >  在可用性组中，不支持从 FCI 到可用性组中其他节点的自动故障转移。 这意味着，如果自动故障转移是高可用性解决方案的一个重要组成部分，则 FCI 和独立节点不应在某一可用性组中结合在一起使用。  不过，对于灾难恢复解决方案而言，可以进行此类结合使用。  
  
-   支持多种存储解决方案，包括 WSFC 群集磁盘（iSCSI、光纤信道等）和服务器消息块 (SMB) 文件共享。  
  
-   使用多子网 FCI 或在可用性组中运行 FCI 托管数据库的灾难恢复解决方案。 利用 [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]中的新的多子网支持功能，多子网 FCI 不再需要虚拟 LAN，因此可提高多子网 FCI 的可管理性和安全性。  
  
-   故障转移过程中无需重新配置应用程序和客户端  
  
-   用于实现自动故障转移的针对具体触发器事件的灵活的故障转移策略  
  
-   通过使用专用和持久的连接执行定期的详细运行状况检测，实现可靠的故障转移  
  
-   通过间接后台检查点在故障转移期间实现可配置性和可预测性  
  
-   故障转移期间限制对资源的使用  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
 在生产环境中，我们建议将静态 IP 地址与故障转移群集实例的虚拟 IP 地址结合使用。  我们不建议在生产环境中使用 DHCP。 在停机情况下，如果 DHCP IP 租期已到，则它需要额外的时间重新注册与 DNS 名称关联的新 DHCP IP 地址。  
  
##  <a name="failover-cluster-instance-overview"></a><a name="Overview"></a> 故障转移群集实例概述  
 FCI 会在具有一个或多个 WSFC 节点的 WSFC 资源组中运行。 当 FCI 启动时，这些节点中的某个节点将获取该资源组的所有权并使其 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例处于联机状态。 此节点拥有的资源包括：  
  
-   网络名称  
  
-   IP 地址  
  
-   共享磁盘  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库引擎服务  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理服务  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Analysis Services 服务（如果已安装）  
  
-   一个文件共享资源（如果安装了 FILESTREAM 功能）  
  
 任何时候，只有资源组所有者（而非 FCI 中的任何其他节点）将在资源组中运行各自的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务。 在出现故障转移（无论是自动故障转移还是计划的故障转移）时，将发生以下事件序列：  
  
1.  除非出现硬件或系统故障，否则会将缓冲区缓存中的所有脏页写入磁盘。  
  
2.  资源组中所有相应的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务都将在活动节点上停止。  
  
3.  资源组所有权将转移到 FCI 中的另一个节点。  
  
4.  新资源组所有者将启动其 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务。  
  
5.  客户端应用程序连接请求将自动定向到使用相同虚拟网络名称 (VNN) 的新活动节点。  
  
 只要 FCI 的基础 WSFC 群集处于良好的仲裁运行状况（大多数仲裁 WSFC 节点可用作自动故障转移目标），FCI 就将处于联机状态。 当 WSFC 群集丢失其仲裁时（无论此情况是因硬件、软件、网络故障还是不适当的仲裁配置导致的），整个 WSCF 群集以及 FCI 将脱机。 在此计划外故障转移方案中，需要手动干预以在剩余可用节点中重新建立仲裁，以使 WSFC 群集和 FCI 重新联机。 有关详细信息，请参阅 [WSFC 仲裁模式和投票配置 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md)。  
  
### <a name="predictable-failover-time"></a>可预测的故障转移时间  
 根据 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上次执行检查点操作的时间，缓冲区缓存中可能存在大量脏页。 因此，故障转移持续的时间取决于将剩余脏页写入磁盘的时间，这会导致不可预测的较长的故障转移时间。 从 [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]开始，FCI 可以使用间接检查点来限制保存在缓冲区缓存中的脏页数。 虽然这样做会导致在常规工作负荷下占用额外资源，但将提高故障转移时间的可预测性和可配置性。 这在组织中的服务级协议指定高可用性解决方案的还原时间目标 (RPO) 时很有用。 有关间接检查点的详细信息，请参阅 [Indirect Checkpoints](../../../relational-databases/logs/database-checkpoints-sql-server.md#IndirectChkpt)。  
  
### <a name="reliable-health-monitoring-and-flexible-failover-policy"></a>可靠的运行状况监视和灵活的故障转移策略  
 成功启动 FCI 后，WSFC 服务将监视基础 WSFC 群集的运行状况和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的运行状况。 从 [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]开始，WSFC 服务使用专用连接来轮询活动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，以便通过系统存储过程获取详细的组件诊断信息。 这蕴含了三方面的含义：  
  
-   利用与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的专用连接，始终能够对组件诊断信息进行可靠轮询，即使在 FCI 负荷较重时也是如此。 这样一来，便能够区分负荷较重的系统与实际具有故障条件的系统，从而阻止出现诸如错误故障转移这样的问题。  
  
-   利用详细组件诊断信息，可以配置更灵活的故障转移策略，由此您便能选择哪些故障条件将触发故障转移以及哪些故障条件将不触发故障转移。  
  
-   利用详细组件诊断信息，还可以通过追溯方式更好地对自动故障转移进行故障排除。 诊断信息将存储到与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 错误日志并置的日志文件中。 可以将这些日志文件加载到日志文件查看器中以检查导致出现故障转移的组件状态，从而确定导致该故障转移的原因。  
  
 有关详细信息，请参阅 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
##  <a name="elements-of-a-failover-cluster-instance"></a><a name="FCIelements"></a> 故障转移群集实例的元素  
 FCI 由一组物理服务器（节点）构成，这些服务器包含类似的硬件配置以及相同的软件配置，其中包括操作系统版本和修补程序级别，以及 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本、修补程序级别、组件和实例名称。 相同的软件配置是确保 FCI 在节点间进行故障转移时能够正常运行所必需的。  
  
 WSFC 资源组  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 在 WSFC 资源组中运行。 该资源组中的每个节点均维护配置设置和检查点注册表项的同步副本，以确保 FCI 在故障转移后可完全正常运行，并且群集中一次只有一个节点（活动节点）拥有该资源组。 WSFC 服务可管理服务器群集、仲裁配置、故障转移策略和故障转移操作以及 FCI 的 VNN 和虚拟 IP 地址。 在出现故障（硬件故障、操作系统故障、应用程序或服务故障）或进行计划的升级时，资源组所有权就转移至 FCI 中的另一个节点。WSFC 资源组中支持的节点数取决于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本。 另外，同一个 WSFC 群集可运行多个 FCI（多个资源组），具体取决于您的硬件能力（如 CPU、内存和磁盘数）。  
  
 SQL Server 二进制文件  
 产品二进制文件本地安装在 FCI 的每个节点上，此过程类似于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 独立安装。 但是，在启动过程中，服务将不会自动启动，而是由 WSFC 管理。  
  
 存储  
 与可用性组相反，对于数据库和日志存储，FCI 必须在 FCI 的所有节点之间使用共享存储。 共享存储的形式可以为 WSFC 群集磁盘、SAN 上的磁盘、存储空间直通 (S2D) 或 SMB 上的文件共享。 这样一来，当发生故障转移时，FCI 中的所有节点都会具有相同的实例数据视图。 但这意味着，共享存储有可能成为单个故障点，并且 FCI 依赖于基本存储解决方案来确保数据保护。  
  
 网络名称  
 FCI 的 VNN 为 FCI 提供了一个统一连接点。 这将允许应用程序连接到 VNN，而无需知道当前活动节点。 当发生故障转移时，VNN 会在新的活动节点启动后注册到该节点。 此过程对于连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的客户端或应用程序是透明的，可以最大限度地缩短出现故障时应用程序或客户端的停机时间。  
  
 虚拟 IP  
 对于多子网 FCI，将为 FCI 中的每个子网分配一个虚拟 IP 地址。 在故障转移期间，将更新 DNS 服务器上的 VNN 以指向各自子网的虚拟 IP 地址。 在发生多子网故障转移后，应用程序和客户端可使用同一 VNN 连接到 FCI。  
  
##  <a name="sql-server-failover-concepts-and-tasks"></a><a name="ConceptsAndTasks"></a> SQL Server 故障转移的概念和任务  
  
|概念和任务|主题|  
|------------------------|-----------|  
|介绍故障检测机制和灵活的故障转移策略。|[Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)|  
|介绍 FCI 管理和维护概念。|[故障转移群集实例管理和维护](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)|  
|介绍多子网配置和概念|[SQL Server 多子网群集 (SQL Server)](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md)|  
  
##  <a name="related-topics"></a><a name="RelatedTopics"></a> 相关主题  
  
|**主题说明**|**主题**|  
|----------------------------|---------------|  
|介绍如何安装新的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI。|[创建新的 SQL Server 故障转移群集（安装程序）](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)|  
|介绍如何升级到 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 故障转移群集。|[升级 SQL Server 故障转移群集实例](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)|  
|介绍 Windows 故障转移群集的概念并提供指向 Windows 故障转移群集相关任务的链接|[!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)]: [故障转移群集的概述](https://go.microsoft.com/fwlink/?LinkId=177878)<br /><br /> [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)] R2： [故障转移群集的概述](https://go.microsoft.com/fwlink/?LinkId=177879)|  
|介绍 FCI 中的节点和可用性组中的副本的概念区别以及有关使用 FCI 承载可用性组的副本的注意事项。|[故障转移群集和可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)|  
  
  
