---
title: WSFC 仲裁模式和投票配置
description: 了解带有 SQL Server 故障转移群集实例或 Always On 可用性组的 Windows Server 故障转移群集所使用的仲裁模式和投票。
ms.custom: seo-lt-2019
ms.date: 10/03/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
- failover clustering [SQL Server], AlwaysOn Availability Groups
ms.assetid: ca0d59ef-25f0-4047-9130-e2282d058283
author: cawrites
ms.author: chadam
ms.openlocfilehash: d900bbc6b8183e4a903886878cf61ce68ac39573
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127524"
---
# <a name="wsfc-quorum-modes-and-voting-configuration-sql-server"></a>WSFC 仲裁模式和投票配置 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)][!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 和 AlwaysOn 故障转移群集实例 (FCI) 都利用 Windows Server 故障转移群集 (WSFC) 来作为平台技术。  WSFC 使用一种基于仲裁的方法来监视群集的整体运行状况，并且最大限度地提高节点级别的容错能力。 理解 WSFC 仲裁模式和节点投票配置对于 Always On 高可用性和灾难恢复解决方案的设计、操作和故障排除十分重要。  
  
  
##  <a name="cluster-health-detection-by-quorum"></a><a name="ClusterHealthDetectionbyQuorum"></a> 由仲裁进行的群集运行状况检测  
 WSFC 群集中的每个节点都参与周期性信号通信，以便与其他节点共享该节点的运行状况。 未响应的节点被认为是处于故障状态。  
  
  “仲裁”节点集是 WSFC 群集中的大多数投票节点和见证服务器。 WSFC 群集的总体运行状况和状态是由定期“仲裁投票”确定的。  仲裁的存在意味着群集运行状况正常，且能提供节点级别的容错能力。  
  
 没有仲裁并不指示群集未在正常状况下运行。  必须维护整体 WSFC 群集运行状况，以便确保运行状况辅助节点可用于要故障转移到的主节点。  如果仲裁投票失败，作为一项预防措施，WSFC 群集将被设为脱机。  这也将导致停止所有向群集注册的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。  
  
> [!IMPORTANT]  
>  如果 WSFC 群集因为仲裁失败而被设为脱机，则需要手动干预以便将其重新联机。  
>   
>  有关详细信息，请参阅：[通过强制仲裁进行 WSFC 灾难恢复 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)。  
  
##  <a name="quorum-modes"></a><a name="QuorumModes"></a> 仲裁模式  
  “仲裁模式”是在 WSFC 群集级别配置的，指示用于仲裁投票的方法。  故障转移群集管理器实用工具将会基于群集中的节点数来建议仲裁模式。  
  
 以下仲裁模式可用于确定构成投票仲裁的元素：  
  
-   **节点的大多数。** 群集中超过一半的投票节点必须投票赞成群集处于正常状态。  
  
-   **节点和文件共享的大多数。** 与节点的大多数仲裁模式相似，只有远程文件共享也配置为投票见证除外，并且从任何节点到该共享的连接也作为赞成投票计数。  超过一半的可能投票必须赞成群集处于正常状态。  
  
     作为最佳实践，见证文件共享不应驻留在该群集中的任何节点上，必须它应该对于该群集中的所有节点都是可见的。  
  
-   **节点和磁盘的大多数。** 与节点的大多数仲裁模式相似，只有共享磁盘群集资源也指定为投票见证除外，并且从任何节点到该共享磁盘的连接也作为赞成投票计数。  超过一半的可能投票必须赞成群集处于正常状态。  
  
-   **仅限磁盘。** 共享磁盘群集资源指定为见证，并且从任何节点到该共享磁盘的连接也作为赞成投票计数。  
  
> [!TIP]  
>  在将非对称存储配置用于 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]时，如果您具有奇数数目的投票节点，则通常应该使用节点的大多数仲裁模式；如果您具有偶数数目的投票节点，则通常应该使用节点和文件共享的大多数仲裁模式。  
  
##  <a name="voting-and-non-voting-nodes"></a><a name="VotingandNonVotingNodes"></a> 投票和非投票节点  
 默认情况下，WSFC 群集中的每个节点都作为群集仲裁的成员包括；每个节点都具有用于确定群集整体运行状况的单个投票，并且每个节点都将持续尝试建立仲裁。  到目前为止对仲裁的论述已谨慎地将对群集运行状况进行投票的一组 WSFC 群集节点划分为“投票节点”。  
  
 在一个 WSFC 群集中没有单独的节点可以明确确定该群集作为一个整体是正常运行还是非正常运行。  在任意给定时刻，从各节点的角度来说，其他一些节点可能好像脱机，或者好像处于故障转移中，或者好像由于网络通信失败而无法响应。  仲裁投票的一个关键功能是确定 WSFC 群集中每个节点的明显表现出来的状态是否真的就是这些节点的实际状态。  
  
 对于除了“仅限磁盘”之外的所有仲裁模式，仲裁投票的效力取决于群集中所有投票节点之间的可靠的通信。 同一物理子网上各节点之间的网络通信应被视为可靠的；应该信任仲裁投票。  
  
 但是，如果其他子网上的节点在仲裁投票中被视为不可响应的，但它实际上处于联机状态并且正常运行，则很可能是因为子网之间网络通信失败。  根据群集拓扑结构、仲裁模式和故障转移策略配置，该网络通信失败可能会有效地创建超过一组（或一个子组）的投票节点。  
  
 当多个子组的投票节点能够建立自己的仲裁时，这称作分区情况。  在这种情况下，单独仲裁中的节点可能具有不同的行为方式，并有互相冲突。  
  
> [!NOTE]  
>  此裂脑情况仅在系统管理员手动执行强制的仲裁操作时或者在非常罕见的情况（如强制故障转移）时才可能出现；并且显式细分仲裁节点组。  
  
 为了简化仲裁配置和增加正常运行时间，可能需要调整每个节点的“NodeWeight”设置，以便为进行仲裁而计算票数时不包括该节点的投票。  
  
> [!IMPORTANT]  
>  为了使用 NodeWeight 设置，必须将以下修补程序应用到 WSFC 群集中的所有服务器：  
>   
>  [KB2494036](https://support.microsoft.com/kb/2494036)：该修补程序用于配置在 [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] 和 [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] 中没有仲裁投票的群集节点  
  
##  <a name="recommended-adjustments-to-quorum-voting"></a><a name="RecommendedAdjustmentstoQuorumVoting"></a> 建议的仲裁投票调整  
 在启用或禁用某一给定 WSFC 节点的投票时，应遵循以下准则：  
  
-   **默认为不投票。** 假定在没有明确理由的情况下每个节点都不应投票。  
  
-   **包括所有的主副本。** 承载可用性组主副本或作为 FCI 的首选所有者的每个 WSFC 节点都应具有一票。  
  
-   **包括可能的自动故障转移所有者。** 可作为自动可用性组故障转移或 FCI 故障转移的结果承载主副本的每个节点都应具有一票。 如果在 WSFC 群集中只有一个可用性组并且可用性副本仅由独立实例承载，则此规则仅包括作为自动故障转移目标的辅助副本。  
  
-   **不包括辅助站点节点。** 通常，不要向驻留在辅助灾难恢复站点的 WSFC 节点投票。  在主站点不存在任何问题时，您不会希望辅助站点中的节点参与到令群集脱机的决策中来。  
  
-   **奇数数目的投票。** 如果需要，将见证文件共享、见证节点或见证磁盘添加到群集并且调整仲裁模式以便防止群集仲裁中出现可能的等同值。  
  
-   **故障转移后重新评估投票分配。** 您不希望故障转移到不支持运行状况仲裁的群集配置。  
  
> [!IMPORTANT]
>  在验证 WSFC 仲裁投票配置时，如果以下任何条件成立，Always On 可用性组向导将显示一个警告。  
> 
>  -   承载主副本的群集节点不具有投票  
> -   辅助副本配置用于自动故障转移并且其群集节点不具有投票。  
> -   [KB2494036](https://support.microsoft.com/kb/2494036) 未安装在承载可用性副本的所有群集节点上。 此修补程序是在多站点部署中为群集节点添加或删除投票所必需的。 但在单站点部署中，此修补程序通常不是必需的并且您可以放心地忽略该警告。  
> 
> [!TIP]
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 公开若干系统动态管理视图 (DMV)，可帮助你管理 WSFC 群集配置和节点仲裁投票相关的设置。  
> 
>  有关详细信息，请参阅：[sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)[sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)[sys.dm_os_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md) 和 [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [查看群集仲裁 NodeWeight 设置](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [配置群集仲裁 NodeWeight 设置](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [用于高可用性和灾难恢复的 Microsoft SQL Server AlwaysOn 解决方案指南](https://go.microsoft.com/fwlink/?LinkId=227600)  
  
-   [Always On 可用性组向导中的仲裁投票配置检查](https://blogs.msdn.microsoft.com/sqlalwayson/2012/03/13/quorum-vote-configuration-check-in-alwayson-availability-group-wizards-andy-jing/)  
  
-   [Windows Server 技术：故障转移群集](https://technet.microsoft.com/library/cc732488\(v=WS.10\).aspx)  
  
-   [故障转移群集分步指南：配置故障转移群集中的仲裁](https://technet.microsoft.com/library/cc770620\(WS.10\).aspx)  
  
## <a name="see-also"></a>另请参阅  
 [通过强制仲裁进行 WSFC 灾难恢复 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)   
 [Windows Server 故障转移群集 (WSFC) 与 SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)  
  
  
