---
title: 通过强制仲裁进行灾难恢复
description: 从仲裁故障恢复需要手动干预。 了解如何在 SQL Server 故障转移群集实例发生灾难的情况下如何强制仲裁。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
- failover clustering [SQL Server], AlwaysOn Availability Groups
ms.assetid: 6cefdc18-899e-410c-9ae4-d6080f724046
author: cawrites
ms.author: chadam
ms.openlocfilehash: fcdd0f1d34ed4107cbe3c90708abe286efbf0837
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642647"
---
# <a name="wsfc-disaster-recovery-through-forced-quorum-sql-server"></a>通过强制仲裁进行 WSFC 灾难恢复 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  仲裁故障通常由涉及 WSFC 群集中的多个节点的系统性灾难、持久性通信故障或配置错误引起的。  从仲裁故障恢复需要手动干预。  
  
-   **开始之前：** [先决条件](#Prerequisites)、[安全性](#Security)  
  
-   通过强制仲裁过程进行 WSFC 灾难恢复  [通过强制仲裁过程进行 WSFC 灾难恢复](#Main)  
  
-   [相关任务](#RelatedTasks)  
  
-   [相关内容](#RelatedContent)  
  
##  <a name="before-you-start"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 强制仲裁过程假定在仲裁失败前存在运行状况正常的仲裁。  
  
> [!WARNING]  
>  用户应熟悉 Windows Server 故障转移群集、WSFC 仲裁模型、 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的概念和交互方式，以及环境特定的部署配置。  
>   
>  有关详细信息，请参阅：[Windows Server 故障转移群集 (WSFC) 与 SQL Server](https://msdn.microsoft.com/library/hh270278\(v=SQL.110\).aspx) 和 [WSFC 仲裁模式和投票配置 (SQL Server)](https://msdn.microsoft.com/library/hh270280\(v=SQL.110\).aspx)  
  
###  <a name="security"></a><a name="Security"></a> Security  
 用户必须是一个域帐户，该帐户是每个 WSFC 群集节点上本地 Administrators 组的成员。  
  
##  <a name="wsfc-disaster-recovery-through-the-forced-quorum-procedure"></a><a name="Main"></a> 通过强制仲裁过程进行 WSFC 灾难恢复  
 请注意，仲裁故障将会使 WSFC 群集中的所有群集服务、SQL Server 实例和 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]设为脱机，这是因为该群集的配置无法确保节点级容错。  仲裁故障意味着 WSFC 群集中的运行状况投票节点不再满足仲裁模型要求。 一些节点可能已完全失败，而另一些节点可能只是关闭了 WSFC 服务并且除失去与仲裁通信的能力之外其他方面运行状况良好。  
  
 若要使 WSFC 群集重新联机，您必须消除现有配置下仲裁故障的根源，根据需要恢复受影响的数据库，并且您可能需要在 WSFC 群集中重新配置其余的节点以反映现存的群集拓扑。  
  
  您可以在 WSFC 群集节点上使用“强制仲裁”过程来覆盖使该群集脱机的安全控制。  这样做可有效地通知 WSFC 群集挂起仲裁投票检查，并使您能够在该群集中的任意节点上将 WSFC 群集资源和 SQL Server 重新联机。  
  
 此类型的灾难恢复过程应包含以下步骤：  
  
#### <a name="to-recover-from-quorum-failure"></a>从仲裁故障中恢复：  
  
1.  **确定故障的范围。** 确定哪些可用性组或 SQL Server 实例是不响应的，哪些群集节点处于联机状态且可在灾后使用，并检查 Windows 事件日志和 SQL Server 系统日志。  在可行的情况下，您应保留取证数据和系统日志以供未来分析使用。  
  
    > [!TIP]  
    >  在 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]的响应实例上，可以通过查询 [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md) 动态管理视图 (DMV)，获取有关在本地服务器实例上拥有可用性副本的可用性组的运行状况信息。  
  
2.  **在单一节点上使用强制仲裁来启动 WSFC 群集。** 确定组件故障数最少的一个节点（已关闭 WSFC 群集服务的节点除外）。  确认此节点能够与其他大多数节点进行通信。  
  
     在此节点上，使用强制仲裁过程来手动强制群集联机。  为了最大程度地减少可能丢失的数据，应选择一个最后承载可用性组主副本的节点。  
  
     有关详细信息，请参阅：[在无仲裁情况下强制启动 WSFC 群集](https://msdn.microsoft.com/library/hh270275\(v=SQL.110\).aspx)  
  
    > [!NOTE]  
    >  在逻辑 WSFC 群集获得大多数投票并自动转换到操作的常规仲裁模式之前，强制仲裁设置会在群集范围内阻止仲裁检查。  
  
3.  **逐一在每个其他方面运行正常的节点上正常启动 WSFC 服务。** 当您在其他节点上启动该群集服务时，您无需指定强制仲裁选项。  
  
     随着每个节点上的 WSFC 服务重新联机，该服务会与其他运行状态正常的节点进行协商以同步新的群集配置状态。  记住一次只能在一个节点上执行此操作，以避免在解析群集的上一个已知状态时出现潜在的争用情况。  
  
    > [!WARNING]  
    >  确保您启动的每个节点都能够与其他刚刚联机的节点通信。  应考虑在其他节点上禁用 WSFC 服务。  否则，您将会面临创建多个仲裁节点集（即“裂脑”情形）的风险。 如果您在步骤 1 中的调查结果很准确，则应该不会发生这种情况。  
  
4.  **应用新的仲裁模式和节点投票配置。** 如果强制仲裁成功地重新启动了群集中的所有节点并且仲裁故障的根源得到了消除，则更改为原始仲裁模式并且不需要节点投票配置。  
  
     否则，您应评估新恢复的群集节点和可用性副本拓扑，并相应地更改每个节点的仲裁模式和投票分配。 应将未恢复的节点设置为脱机，或将其节点投票设置为零。  
  
    > [!TIP]  
    >  此时，群集中的节点和 SQL Server 实例可能看起来已恢复回正常操作。  但是，可能仍然不存在运行状况正常的仲裁。  使用故障转移群集管理器或 SQL Server Management Studio 中的 AlwaysOn 面板或适当的 DMV 来确认仲裁已恢复正常。  
  
5.  **根据需要恢复可用性组数据库副本。** 非可用性组数据库应会作为常规 SQL Server 启动过程的一部分自行恢复和联机。  
  
     通过按照以下顺序将可用性组副本重新联机可以最大程度地减少可能丢失的数据并缩短恢复时间：主副本、同步辅助副本和异步辅助副本。  
  
6.  **修复或替换失败的组件并重新验证群集。** 从最初的灾难和仲裁故障中恢复之后，应修复或替换失败的节点并对相关的 WSFC 和 AlwaysOn 配置进行相应地调整。  这可以包括删除可用性组副本、使节点从群集中退出或者在节点上平展并重新安装软件。  
  
     您必须修复或删除所有失败的可用性副本。  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将不会截断超过最后一个可用性副本的上一个已知点的事务日志。   如果没有在可用性组中修复或删除某个失败的副本，则事务日志将会增长，因而您将面临其他副本的事务日志空间不足的风险。  
  
    > [!NOTE]  
    >  如果您在某一可用性组侦听器在 WSFC 群集上存在时运行 WSFC 验证配置向导，该向导将生成以下不正确的警告消息：  
    >   
    >  “网络名称 ‘Name:<network_name>’ 的 RegisterAllProviderIP 属性设为 1。对于当前群集配置，该值应设为 0。”  
    >   
    >  请忽略此消息。  
  
7.  **根据需要，重复步骤 4。** 目标是重新建立适当级别的容错和高可用性以实现正常的操作。  
  
8.  **进行 RPO/RTO 分析。** 您应分析 SQL Server 系统日志、数据库时间戳和 Windows 事件日志，以确定故障的根源，并记录实际的恢复点和恢复时间经验。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [在无仲裁情况下强制启动 WSFC 群集](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
-   [执行可用性组的强制手动故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [查看群集仲裁 NodeWeight 设置](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [配置群集仲裁 NodeWeight 设置](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
-   [使用 AlwaysOn 面板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [查看故障转移群集的事件和日志](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Get-ClusterLog 故障转移群集 Cmdlet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
## <a name="see-also"></a>另请参阅  
 [Windows Server 故障转移群集 (WSFC) 与 SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)  
  
