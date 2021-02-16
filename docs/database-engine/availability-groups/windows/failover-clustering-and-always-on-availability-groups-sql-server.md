---
title: 故障转移群集实例与可用性组
description: 通过合并 SQL Server 故障转移集群实例和 Always On 可用性组的功能，增强高可用性和灾难恢复能力。
ms.custom: seo-lt-2019
ms.date: 07/02/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- clustering [SQL Server]
- Availability Groups [SQL Server], WSFC clusters
- Failover Cluster Instances [SQL Server], see failover clustering [SQL Server]
- quorum [SQL Server]
- failover clustering [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], Failover Cluster Instances
ms.assetid: 613bfbf1-9958-477b-a6be-c6d4f18785c3
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 31cef60a33505278791d296aab1022be9b4bc06b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353882"
---
# <a name="failover-clustering-and-always-on-availability-groups-sql-server"></a>故障转移群集和 AlwaysOn 可用性组 (SQL Server)

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

   [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]是在 [!INCLUDE[sssql11](../../../includes/sssql11-md.md)] 中引入的高可用性和灾难恢复解决方案，它要求 Windows Server 故障转移群集 (WSFC)。 此外，尽管 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 不依赖于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集，但您可以使用故障转移群集实例 (FCI) 来为可用性组承载可用性副本。 因此，了解每种群集技术所扮演的角色以及设计您的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 环境所需的注意事项十分重要。  
  
> [!NOTE]  
>  有关 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 概念的详细信息，请参阅 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。  
  
  
##  <a name="windows-server-failover-clustering-and-availability-groups"></a><a name="WSFC"></a> Windows Server 故障转移群集和可用性组  
 部署 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 需要 Windows Server 故障转移群集 (WSFC)。 若要为 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 启用，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例必须驻留在某一 WSFC 节点上，并且该 WSFC 和节点必须处于联机状态。 此外，给定可用性组的每个可用性副本都必须位于相同 WSFC 的不同节点上。 唯一的例外是在迁移到另一个 WSFC 时，此时一个可用性组可能会暂时跨两个群集。  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 依赖 Windows Server 故障转移群集 (WSFC) 来监视和管理属于给定可用性组的可用性副本的当前角色，以及确定故障转移事件如何影响可用性副本。 为您创建的每个可用性组创建一个 WSFC 资源组。 WSFC 监视此资源组，以评估主要副本的运行状况。  
  
 针对 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的仲裁基于 WSFC 中的所有节点，而与给定群集节点是否托管任何可用性副本无关。 与数据库镜像相反，在 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]中没有见证服务器角色。  
  
 WSFC 的总体运行状况由群集中节点的仲裁投票决定。 如果 WSFC 因计划外灾难或由于持续的硬件或通信故障而脱机，则需要管理员手动干预。 Windows Server 或 WSFC 管理员将需要“强制仲裁”，然后在非容错配置中将仍有效的群集节点重新变为联机状态。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 注册表项是 WSFC 的子项。 如果删除后重新创建了 WSFC，则必须在其原始 WSFC 上托管可用性副本的每个 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 实例上都禁用然后重新启用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能。  
  
 有关在 WSFC 节点上运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的信息以及有关 WSFC 仲裁的信息，请参阅 [Windows Server 故障转移群集 (WSFC) 与 SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)。  
  
##  <a name="ssnoversion-failover-cluster-instances-fcis-and-availability-groups"></a><a name="SQLServerFC"></a> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例 (FCI) 和可用性组  
 可以通过将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 与 WSFC 一起实现，在服务器-实例级别设置第二层故障转移。 可用性副本可由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的独立实例或 FCI 实例承载。 对于某一给定可用性组，一个 FCI 伙伴只能承载一个副本。 当某一可用性副本正在一个 FCI 上运行时，可用性组的可能所有者列表将只包含活动的 FCI 节点。  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 不依赖于任何共享存储形式。 但是，如果使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例 (FCI) 来承载一个或多个可用性副本，每个 FCI 将需要标准 SQL Server 故障转移群集实例安装所要求的共享存储。  
  
 有关其它先决条件的详细信息，请参阅[针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md) 的“使用 SQL Server 故障转移群集实例 (FCI) 承载可用性副本的先决条件和限制”部分。  
  
### <a name="comparison-of-failover-cluster-instances-and-availability-groups"></a>比较故障转移群集实例和可用性组  
 无论 FCI 中的节点数是多少，整个 FCI 都只承载可用性组内的一个副本。 下表说明 FCI 中的节点和可用性组内的副本的概念区别。  
  
||FCI 内的节点|可用性组内的副本|  
|-|-------------------------|-------------------------------------------|  
|**使用 WSFC**|是|是|  
|**保护级别**|实例|数据库|  
|**存储类型**|共享|非共享<br /><br /> 尽管可用性组中的副本不共享存储，但是，由 FCI 承载的副本将使用该 FCI 所要求的共享存储解决方案。 该存储解决方案仅由 FCI 内的节点共享，不在可用性组的副本之间共享。|  
|**存储解决方案**|直连、SAN、装入点、SMB|取决于节点类型|  
|**可读辅助副本**|否*|是|  
|**适用的故障转移策略设置**|WSFC 仲裁<br /><br /> FCI 特有的<br /><br /> 可用性组设置**|WSFC 仲裁<br /><br /> 可用性组设置|  
|**故障转移资源**|服务器、实例和数据库|仅数据库|  
  
 *尽管可用性组中的同步次要副本始终在相应的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上运行，但 FCI 内的辅助节点实际未启动相应的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，因此不可读。 在 FCI 中，仅在 FCI 故障转移期间资源组所有权转移给辅助节点时，辅助节点才启动其 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 但是，在活动 FCI 节点上，当 FCI 承载的数据库属于可用性组时，如果本地可用性副本正在作为可读辅助副本运行，则数据库是可读的。  
  
 **可用性组的故障转移策略设置应用于所有副本，无论它位于独立实例还是 FCI 实例中。  
  
> [!NOTE]  
>  有关不同版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中 FCI 内的 **节点数** 和 **Always On 可用性组** 的详细信息，请参阅 [SQL Server 2012 各个版本支持的功能](/previous-versions/sql/sql-server-2012/cc645993(v=sql.110)) (https://go.microsoft.com/fwlink/?linkid=232473))。  
  
### <a name="considerations-for-hosting-an-availability-replica-on-an-fci"></a>FCI 上承载可用性副本的注意事项  
  
> [!IMPORTANT]  
>  如果计划在 SQL Server 故障转移群集实例 (FCI) 上承载可用性副本，请确保 Windows Server 2008 主机节点满足故障转移群集实例 (FCI) 的 AlwaysOn 先决条件和限制。 有关详细信息，请参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)配置服务器实例时遇到的典型问题。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例 (FCI) 不支持通过可用性组来自动进行故障转移，因此只能为手动故障转移配置任何由 FCI 承载的可用性副本。  
  
 可能需要配置 WSFC 以包含并非在所有节点上可用的共享磁盘。 例如，考虑跨两个数据中心、包含三个节点的 WSFC。 其中两个节点在主数据中心托管 SQL Server 故障转移群集实例 (FCI)，并且有权访问相同的共享磁盘。 第三个节点在另一个数据中心中承载独立 SQL Server 实例，并且无权访问主数据中心的共享磁盘。 如果 FCI 托管主要副本，而独立实例托管次要副本，则此 WSFC 配置支持部署可用性组。  
  
 选择 FCI 承载给定可用性组的可用性副本时，请确保 FCI 故障转移不会导致单个 WSFC 节点尝试承载同一可用性组的两个可用性副本。  
  
 下面的示例方案说明此配置是如何导致问题的：  
  
 Marcel 配置具有两个节点（`NODE01` 和 `NODE02`）的 WSFC。 他在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 和 `fciInstance1`上安装一个 `NODE01` 故障转移群集实例 `NODE02` ，其中 `NODE01` 是 `fciInstance1`的当前所有者。  
 在 `NODE02`上，Marcel 安装了另一个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例 `Instance3`，该实例是一个独立实例。  
 在 `NODE01`上，Marcel 为 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]启用了 fciInstance1。 在 `NODE02`上，他为 `Instance3` 启用了 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]。 然后，他设置了一个可用性组，其中 `fciInstance1` 承载主副本， `Instance3` 承载辅助副本。  
 在某些时候，`fciInstance1` 会在 `NODE01` 上变得不可用，并且 WSFC 会导致 `fciInstance1` 故障转移到 `NODE02`。 在故障转移后， `fciInstance1` 将是启用了 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]的实例并且在 `NODE02`上以主角色运行。 但是， `Instance3` 现在驻留在与 `fciInstance1`相同的 WSFC 节点上。 这违反了 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 约束。  
 若要纠正此方案产生的问题，独立实例 `Instance3` 必须驻留在与 `NODE01` 和 `NODE02` 相同的 WSFC 中的另一个节点上。  
  
 有关 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 的详细信息，请参阅 [AlwaysOn 故障转移群集实例 (SQL Server)](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)。  
  
##  <a name="restrictions-on-using-the-wsfc-failover-cluster-manager-with-availability-groups"></a><a name="FCMrestrictions"></a> 将 WSFC 故障转移群集管理器用于可用性组的限制  
 不要使用故障转移群集管理器来操作可用性组，例如：  
  
-   不要在可用性组的群集服务（资源组）中添加或删除资源。  
  
-   不要更改任何可用性组属性，例如可能的所有者和首选所有者。 这些属性由可用性组自动设置。  
  
-   不要使用故障转移群集管理器将可用性组移到不同节点或者故障转移可用性组。  故障转移群集管理器不知道可用性副本的同步状态，因此，这样做可能会导致延长停机时间。 必须使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]。  

  >[!WARNING]
  > 使用故障转移群集管理器将托管可用性组的故障转移群集实例  移动到已  在托管同一个可用性组副本的节点，可能会导致可用性组副本丢失，使其无法在目标节点上重新联机。 故障转移群集的单个节点不能托管同一个可用性组的多个副本。 有关如何发生这种情况以及如何恢复的详细信息，请参阅博客[在可用性组中意外删除副本](/archive/blogs/alwaysonpro/issue-replica-unexpectedly-dropped-in-availability-group)。 
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   **博客：**  
  
     [将 SQL Server 的 Windows 故障转移群集（可用性组或 FCI）配置为有限安全](/archive/blogs/sqlalwayson/configure-windows-failover-clustering-for-sql-server-availability-group-or-fci-with-limited-security)  
  
     [SQL Server AlwaysOn 团队博客：SQL Server AlwayOn 团队官方博客](/archive/blogs/sqlalwayson/)  
  
     [CSS SQL Server 工程师博客](/archive/blogs/psssql/)  
  
-   **白皮书：**  
  
     [AlwaysOn 体系结构指南：使用故障转移群集实例和可用性组构建高可用性和灾难恢复解决方案](/previous-versions/sql/sql-server-2012/jj215886(v=msdn.10))  
  
     [用于高可用性和灾难恢复的 Microsoft SQL Server AlwaysOn 解决方案指南](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [针对 SQL Server 2012 的 Microsoft 白皮书](https://msdn.microsoft.com/library/hh403491.aspx)  
  
     [SQL Server 客户咨询团队白皮书](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [启用和禁用 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)   
 [监视可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [AlwaysOn 故障转移群集实例 (SQL Server)](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)  
  
