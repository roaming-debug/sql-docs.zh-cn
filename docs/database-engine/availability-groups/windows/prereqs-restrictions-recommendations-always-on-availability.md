---
title: 可用性组：先决条件、限制和建议
description: 介绍将 Always On 可用性组部署到 SQL Server 的先决条件、限制和建议。
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], WSFC clusters
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], prerequisites and restrictions
- Availability Groups [SQL Server], Failover Cluster Instances
- Availability Groups [SQL Server], databases
- Availability Groups [SQL Server]
ms.assetid: edbab896-42bb-4d17-8d75-e92ca11f7abb
author: cawrites
ms.author: chadam
ms.openlocfilehash: ec3a84dc54dcaf373f8fd817c259602c7901410d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642511"
---
# <a name="prerequisites-restrictions-and-recommendations-for-always-on-availability-groups"></a>针对 AlwaysOn 可用性组的先决条件、限制和建议
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  本文介绍部署 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 时的注意事项，包括针对主机、Windows Server 故障转移群集 (WSFC)、服务器实例和可用性组的先决条件、限制和建议。 对于上述各组件，还指出了安全注意事项和所需权限（如果有）。  
  
> [!IMPORTANT]  
>  在部署 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]之前，强烈建议您阅读本主题的每个章节。  
    
##  <a name="net-hotfixes-that-support-availability-groups"></a><a name="DotNetHotfixes"></a> 支持可用性组的 .NET 修补程序  
 根据你将与 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 一起使用的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 组件和功能，你可能需要安装在下表中标识的其他 .NET 修补程序。 可以按任意顺序安装这些修补程序。  
  
|依赖功能|修补程序|链接|  
|-----------------------|------------|----------|  
|[!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]|.NET 3.5 SP1 修补程序添加对读意向、只读和多子网故障转移的 SQL Client for AlwaysOn 功能的支持。 此修补程序需要安装在每个 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 报表服务器上。|KB 2654347：[.NET 3.5 SP1 修补程序添加对 AlwaysOn 功能的支持](https://go.microsoft.com/fwlink/?LinkId=242896)|  
  

###  <a name="checklist-requirements-windows-system"></a><a name="SystemRequirements"></a>清单：要求（Windows 系统）  
 为了支持 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 功能，请确保要参与一个或多个可用性组的每台计算机都满足以下基本要求：  
  
|要求|链接|  
|-----------------|----------|  
|确保此系统不是域控制器。|域控制器上不支持可用性组。|  
|确保每台计算机正在运行 Windows Server 2012 或更高版本。|[安装 SQL Server 2016 的硬件和软件要求](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)|  
|确保每台计算机都是 WSFC 中的一个节点。|[Windows Server 故障转移群集 (WSFC) 与 SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)|  
|确保 WSFC 包含足够的节点来支持可用性组配置。|群集节点可以为一个可用性组托管一个副本。 同一个节点不能托管来自同一可用性组的两个副本。 群集节点可以加入多个可用性组，每个组包含一个副本。 <br /><br /> 咨询数据库管理员，了解需要多少个群集节点才能支持计划的可用性组的可用性副本。<br /><br /> [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。|  

  
> [!IMPORTANT]  
>  另外，确保已正确配置了您的环境以连接到可用性组。 有关详细信息，请参阅 [AlwaysOn 客户端连接 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)。  
  
##  <a name="recommendations-for-computers-that-host-availability-replicas-windows-system"></a><a name="ComputerRecommendations"></a> 针对承载可用性副本的计算机的建议（Windows 系统）  
  
-   **的系统：** 对于给定的可用性组，所有可用性副本都应在可处理同样的工作负荷的相当的系统上运行。  
  
-   **专用网络适配器：** 为获得最佳性能，请将专用的网络适配器（网络接口卡）用于 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]。  
  
-   **足够的磁盘空间：** 服务器实例在其上承载可用性副本的每个计算机都必须为该可用性组中的所有数据库拥有足够的磁盘空间。 请记住，在主数据库增长时，其相应的辅助数据库也增长相同量。  
  
###  <a name="permissions-windows-system"></a><a name="PermissionsWindows"></a> 权限（Windows 系统）  
 若要管理 WSFC，用户必须是每个群集节点上的系统管理员。  
  
 有关用于管理群集的帐户的详细信息，请参阅[附录 A：故障转移群集要求](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197454(v=ws.10))。  
  
###  <a name="related-tasks-windows-system"></a><a name="RelatedTasksWindows"></a> 相关任务（Windows 系统）  
  
|任务|链接|  
|----------|----------|  
|设置 HostRecordTTL 值。|[更改 HostRecordTTL（使用 Windows PowerShell）](#ChangeHostRecordTTLps)|  
  
####  <a name="change-the-hostrecordttl-using-windows-powershell"></a><a name="ChangeHostRecordTTLps"></a> 更改 HostRecordTTL（使用 Windows PowerShell）  
  
1.  通过 **“以管理员身份运行”** 打开 PowerShell 窗口。  
  
2.  导入 FailoverClusters 模块。  
  
3.  使用 **Get-ClusterResource** cmdlet 查找网络名称资源，然后使用 **Set-ClusterParameter** cmdlet 设置 **HostRecordTTL** 值，如下所示：  
  
     Get-ClusterResource "\<NetworkResourceName>" | Set-ClusterParameter HostRecordTTL \<TimeInSeconds>  
  
     下面的 PowerShell 示例为名为 `SQL Network Name (SQL35)` 的网络名称资源将 HostRecordTTL 设置为 300 秒。  
  
    ```powershell
    Import-Module FailoverClusters  
  
    $nameResource = "SQL Network Name (SQL35)"  
    Get-ClusterResource $nameResource | Set-ClusterParameter ClusterParameter HostRecordTTL 300  
    ```  
  
    > [!TIP]  
    >  每次打开新的 PowerShell 窗口时，都需要导入 **FailoverClusters** 模块。  
  
##### <a name="related-content-powershell"></a>相关内容 (PowerShell)  
  
-   [群集和高可用性](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering) （故障转移群集和网络负载平衡团队博客）  
  
-   [故障转移群集上的 Windows PowerShell 入门](https://technet.microsoft.com/library/ee619762\(WS.10\).aspx)  
  
-   [群集资源命令和等效的 Windows PowerShell cmdlet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619744(v=ws.10)#BKMK_resource)  
  
###  <a name="related-content-windows-system"></a><a name="RelatedContentWS"></a> 相关内容（Windows 系统）  
  
-   [在多站点故障转移群集中配置 DNS 设置](https://technet.microsoft.com/library/dd197562\(WS.10\).aspx)  
  
-   [向网络名称资源注册 DNS](https://techcommunity.microsoft.com/t5/failover-clustering/dns-registration-with-the-network-name-resource/ba-p/371482)  
  

##  <a name="sql-server-instance-prerequisites-and-restrictions"></a><a name="ServerInstance"></a> SQL Server 实例先决条件和限制  
 每个可用性组均要求称作“可用性副本” [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的一组故障转移伙伴，它们由  的实例承载。 给定的服务器实例可以是独立实例或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例 (FCI)。  
  
 **本节内容：**  
  
-   [清单：先决条件](#PrerequisitesSI)  
  
-   [可用性组的线程使用情况](#ThreadUsage)  
  
-   [权限](#PermissionsSI)  
  
-   [相关任务](#RelatedTasksSI)  
  
-   [相关内容](#RelatedContentSI)  
  
###  <a name="checklist-prerequisites-server-instance"></a><a name="PrerequisitesSI"></a>清单：先决条件（服务器实例）  
  
|先决条件|链接|  
|------------------|-----------|  
|主机必须是一个 WSFC 节点。 托管给定可用性组的可用性副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例驻留在单独的群集节点上。 迁移到其他群集时，一个可用性组可能会暂时跨两个群集。 SQL Server 2016 引入了分布式可用性组。 在分布式可用性组中，两个可用性组驻留在不同的群集上。|[Windows Server 故障转移群集 (WSFC) 与 SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)<br /><br /> [故障转移群集和 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)<br/> <br/> [分布式可用性组（AlwaysOn 可用性组）](./distributed-availability-groups.md)|  
|如果您希望将可用性组与 Kerberos 一起使用：<br /><br /> 承载可用性组的可用性副本的所有服务器实例都必须使用相同的 SQL Server 服务帐户。<br /><br /> 域管理员需要在可用性组侦听器的虚拟网络名称 (VNN) 的 SQL Server 服务帐户上使用 Active Directory 手动注册服务主体名称 (SPN)。 如果在 SQL Server 服务帐户之外的帐户上注册 SPN，则身份验证将失败。<br /><br /> <br /><br /> <b>\*\* 重要提示 \*\*</b> 如果你更改 SQL Server 服务帐户，则域管理员必须重新手动注册 SPN。|[为 Kerberos 连接注册服务主体名称](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)<br /><br /> **简要说明：**<br /><br /> Kerberos 和 SPN 强制实施相互身份验证。 SPN 将映射到启动 SQL Server 服务的 Windows 帐户。 如果未正确注册 SPN 或注册失败，则 Windows 安全层将无法确定与 SPN 关联的帐户，因而无法使用 Kerberos 身份验证。<br /><br /> <br /><br /> 注意：NTLM 没有此要求。|  
|如果您计划使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例 (FCI) 承载可用性副本，则请确保您理解 FCI 限制并且满足 FCI 要求。|[使用 SQL Server 故障转移群集实例 (FCI) 承载可用性副本的先决条件和要求](#FciArLimitations)（本文后面将作介绍）|  
|每个服务器实例必须运行相同版本的 SQL Server 才能参加 AlwaysOn 可用性组。|[SQL 2014](/previous-versions/sql/2014/getting-started/features-supported-by-the-editions-of-sql-server-2014?view=sql-server-2014&preserve-view=true)、[SQL 2016](../../../sql-server/editions-and-components-of-sql-server-2016.md?view=sql-server-2016&preserve-view=true) 和 [SQL 2017](../../../sql-server/editions-and-components-of-sql-server-2017.md?view=sql-server-2017&preserve-view=true) 的各版本和支持的功能。|  
|为某一可用性组承载可用性副本的所有服务器实例必须都使用相同的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 排序规则。|[设置或更改服务器排序规则](../../../relational-databases/collations/set-or-change-the-server-collation.md)|  
|对将为任何可用性组承载可用性副本的每个服务器实例都启用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 功能。 在某一给定计算机上，您可为 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 启用您的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装支持的任意多的服务器实例。|[启用和禁用 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)<br /><br /> <br /><br /> <b>\*\*重要提示\*\*</b> 如果销毁并重新创建了 WSFC，则必须在每个服务器实例上禁用并重新启用已在原始群集上为 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 启用的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 功能。|  
|每个服务器实例都要求数据库镜像端点。 请注意，此端点由服务器实例上的所有可用性副本以及数据库镜像伙伴和见证服务器共享。<br /><br /> 如果你选择承载可用性副本的服务器实例正在某一域用户帐户下运行并且尚不具有数据库镜像端点，则 [新建可用性组向导](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md) （或者 [将副本添加到可用性组向导](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)）可以创建该端点并将 CONNECT 权限授予服务器实例的服务帐户。 但是，如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务正在以内置帐户（例如 Local System、Local Service 或 Network Service）或非域帐户运行，您必须使用证书来进行端点身份验证，并且该向导将无法在服务器实例上创建数据库镜像端点。 在此情况下，我们建议您首先手动创建数据库镜像端点，然后启动该向导。<br /><br /> <br /><br /> <b>\*\* 安全说明 \*\*</b>[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的传输安全性与数据库镜像的传输安全性相同。|[数据库镜像终结点 (SQL Server)](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)<br /><br /> [针对数据库镜像和 AlwaysOn 可用性组的传输安全性 (SQL Server)](../../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)|  
|如果使用 FILESTREAM 的任何数据库将添加到某一可用性组，请确保在将承载该可用性组的可用性副本的每个服务器实例上都启用 FILESTREAM。|[启用和配置 FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)|  
|如果任何包含的数据库将添加到某一可用性组，请确保在将承载该可用性组的可用性副本的每个服务器实例上将 **contained database authentication** 服务器选项都设置为 **1** 。|[contained database authentication 服务器配置选项](../../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md)<br /><br /> [服务器配置选项 (SQL Server)](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)|  
  
###  <a name="thread-usage-by-availability-groups"></a><a name="ThreadUsage"></a> 可用性组的线程使用情况  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的工作线程具有以下要求：  
  
-   在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的一个空闲实例上， [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 使用 0 线程。  
  
-   可用性组使用的最大线程数为配置的最大服务器线程数（“**最大工作线程**”）减去 40。  
  
-   给定服务器实例上承载的可用性副本共享一个线程池。  
  
     线程是按需共享的，如下所示：  
  
    -   一般情况下，存在 3–10 个共享线程，但此数目会根据主副本工作负荷增加。  
  
    -   如果给定线程空闲一段时间，则将释放回常规 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 线程池。 正常情况下，不活动线程在处于不活动状态 15 秒后释放。 但是，根据上一次活动，空闲线程可能保留更长时间。  

    -   对于次要副本，SQL Server 实例最多使用 100 个线程进行并行重做。 每个数据库最多使用 CPU 内核总数的一半，但是每个数据库不能超过 16 个线程。 如果单个实例的所需线程总数超过 100 个，则 SQL Server 会对每个其余数据库使用单个重做线程。 串行重做线程将在处于不活动状态约 15 秒后释放。 
     
-   此外，可用性组使用未共享的线程，如下所示：  
  
    -   每个主副本为每个主数据库使用 1 个日志捕获线程。 此外，它为每个辅助数据库使用 1 个日志发送线程。 日志发送线程将在处于不活动状态 15 秒后释放。    
  
    -   辅助副本上的备份将在备份操作持续时间内包含主副本上的一个线程。 

-  SQL Server 2019 为内存优化可用性组数据库引入了并行重做。 在 SQL Server 2016 和 2017 中，如果可用性组中的数据库也是内存优化的，则基于磁盘的表不会使用并行重做。 
  
 有关详细信息，请参阅 [Always On - HADRON 学习系列：启用了 HADRON 的数据库的工作线程池用法](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)（CSS [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 工程师博客）。  
  
###  <a name="permissions-server-instance"></a><a name="PermissionsSI"></a> 权限（服务器实例）  
  
|任务|所需的权限|  
|----------|--------------------------|  
|创建数据库镜像端点|要求具有 CREATE ENDPOINT 权限，或者具有 **sysadmin** 固定服务器角色的成员身份。  此外，还要求 CONTROL ON ENDPOINT 权限。 有关详细信息，请参阅 [GRANT 终结点权限 (Transact-SQL)](../../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)。|  
|启用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]|要求本地计算机上“管理员”组中的成员身份以及对 WSFC 的完全控制。|  
  
###  <a name="related-tasks-server-instance"></a><a name="RelatedTasksSI"></a> 相关任务（服务器实例）  
  
|任务|项目|  
|----------|-----------|  
|确定数据库镜像端点是否存在|[sys.database_mirroring_endpoints (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)|  
|创建数据库镜像端点（如果它尚不存在）|[为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)<br /><br /> [使用数据库镜像终结点证书 (Transact-SQL)](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)<br /><br /> [为 AlwaysOn 可用性组创建数据库镜像终结点 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)|  
|启用可用性组|[启用和禁用 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)|  
  
###  <a name="related-content-server-instance"></a><a name="RelatedContentSI"></a> 相关内容（服务器实例）  
  
-   [Always On - HADRON 学习系列：启用了 HADRON 的数据库的工作线程池用法](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
##  <a name="network-connectivity-recommendations"></a><a name="NetworkConnect"></a> 网络连接建议  
 强烈建议为 WSFC 节点之间的通信和可用性副本之间的通信使用相同的网络链接。  如果某些链接失败（甚至间歇性断开），使用单独的网络链接可能会导致意外行为。  
  
 例如，要使可用性组支持自动故障转移，作为自动故障转移伙伴的辅助副本必须处于 SYNCHRONIZED 状态。 如果到此辅助副本的网络链接失败（甚至间歇性断开），副本将进入 UNSYNCHRONIZED 状态，并且在该链接恢复之前无法重新同步。 如果该次要副本未同步并且 WSFC 请求自动故障转移，此时不会发生自动故障转移。  
  
##  <a name="client-connectivity-support"></a><a name="ClientConnSupport"></a> 客户端连接支持  
 有关 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 支持客户端连接的详细信息，请参阅 [AlwaysOn 客户端连接 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)。  
  
##  <a name="prerequisites-and-restrictions-for-using-a-sql-server-failover-cluster-instance-fci-to-host-an-availability-replica"></a><a name="FciArLimitations"></a> 使用 SQL Server 故障转移群集实例 (FCI) 承载可用性副本的先决条件和限制  
 **本节内容：**  
  
-   [限制](#RestrictionsFCI)  
  
-   [清单：先决条件](#PrerequisitesFCI)  
  
-   [相关任务](#RelatedTasksFCIs)  
  
-   [相关内容](#RelatedContentFCIs)  
  
###  <a name="restrictions-fcis"></a><a name="RestrictionsFCI"></a> 限制 (FCI)  
  
> [!NOTE]  
> 故障转移群集实例支持群集共享卷 (CSV)。 有关 CSV 的详细信息，请参阅 [了解故障转移群集中的群集共享卷](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759255(v=ws.11))。  
  
-   FCI 的群集节点仅可以托管给定可用性组的一个副本：如果在 FCI 上添加可用性副本，作为 FCI 的可能所有者的 WSFC 节点不能托管同一个可用性组的另一个副本。  若要避免可能出现的冲突，建议配置故障转移群集实例的可能所有者。 这将阻止可能会导致单个 WSFC 尝试在同一可用性组上同时承载两个可用性副本的情况的发生。
  
     此外，其他每个副本都必须由驻留在同一个 Windows Server 故障转移群集中其他群集节点上的 SQL Server 2016 实例托管。 唯一的例外是在迁移到另一个群集时，一个可用性组可能会暂时跨两个群集。 

  >[!WARNING]
  > 使用故障转移群集管理器将托管可用性组的故障转移群集实例移动到已在托管同一个可用性组副本的节点，可能会导致可用性组副本丢失，使其无法在目标节点上重新联机。 故障转移群集的单个节点不能托管同一个可用性组的多个副本。 有关如何发生这种情况以及如何恢复的详细信息，请参阅博客[在可用性组中意外删除副本](/archive/blogs/alwaysonpro/issue-replica-unexpectedly-dropped-in-availability-group)。 

  
-   FCI 不支持可用性组自动故障转移：FCI 不支持通过可用性组来自动进行故障转移，因此只能为手动故障转移配置任何由 FCI 承载的可用性副本。  
  
-   **更改 FCI 网络名称：** 如果你需要更改托管可用性副本的 FCI 的网络名称，则需要从副本的可用性组中删除它，然后将它添加回可用性组中。 您不能删除主副本，因此，如果您在重命名承载主副本的 FCI，则应故障转移到某一辅助副本，然后删除之前的主副本并将其添加回去。 请注意，重命名 FCI 可能会更改其数据库镜像端点的 URL。 当您添加副本时，请确保指定当前端点的 URL。  
  
###  <a name="checklist-prerequisites-fcis"></a><a name="PrerequisitesFCI"></a>清单：先决条件 (FCI)  
  
|先决条件|链接|  
|------------------|----------|  
|请确保每个 SQL Server 故障转移群集实例 (FCI) 都拥有标准 SQL Server 故障转移群集实例安装所要求的共享存储。||  
  
###  <a name="related-tasks-fcis"></a><a name="RelatedTasksFCIs"></a> 相关任务 (FCI)  
  
|任务|项目|  
|----------|-----------|  
|安装 SQL Server 故障转移群集|[创建新的 SQL Server 故障转移群集（安装程序）](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)|  
|您的现有 SQL Server 故障转移群集的就地升级|[升级 SQL Server 故障转移群集实例（安装程序）](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)|  
|维护您的现有 SQL Server 故障转移群集|[在 SQL Server 故障转移群集中添加或删除节点（安装程序）](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)|  
  
###  <a name="related-content-fcis"></a><a name="RelatedContentFCIs"></a> 相关内容 (FCI)  
  
-   [故障转移群集和可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)  
  
-   [Always On 体系结构指南：使用故障转移群集实例和可用性组生成高可用性和灾难恢复解决方案](/previous-versions/sql/sql-server-2012/jj215886(v=msdn.10))  
  
##  <a name="availability-group-prerequisites-and-restrictions"></a><a name="PrerequisitesForAGs"></a> 可用性组先决条件和限制  
 **本节内容：**  
  
-   [限制](#RestrictionsAG)  
  
-   [要求](#RequirementsAG)  
  
-   [安全性](#SecurityAG)  
  
-   [相关任务](#RelatedTasksAGs)  
  
###  <a name="restrictions-availability-groups"></a><a name="RestrictionsAG"></a> 限制（可用性组）  
  
-   可用性副本必须由一个 WSFC 的不同节点承载：对于某个给定可用性组，可用性副本必须由在同一 WSFC 的不同节点上运行的服务器实例承载。 唯一的例外是在迁移到另一个群集时，一个可用性组可能会暂时跨两个群集。  
  
    > [!NOTE]  
    >  同一物理计算机上的多个虚拟机可分别为同一可用性组承载可用性副本，因为每个虚拟机都充当一个单独的计算机。  
  
-   **唯一的可用性组名称：** 每个可用性组名称在 WSFC 上必须唯一。 可用性组名称的最大长度为 128 个字符。  
  
-   **可用性副本：** 每个可用性组都支持一个主副本和最多八个辅助副本。 所有副本都可在异步提交模式下运行，或最多三个副本可在同步提交模式下运行（具有两个同步辅助副本的一个主副本）。  
  
-   每台计算机的可用性组和可用性数据库的最大数目：可以在计算机（VM 或物理机）上放置的数据库和可用性组的实际数目取决于硬件和工作负荷，但是没有强制限制。 Microsoft 已测试每台物理计算机多达 10 个 AG 和 100 个数据库，但这不是绑定限制。 根据服务器上的硬件规范和工作负载，可以在 SQL Server 的实例上放置更多数据库和可用性组。 系统过载的信号可能包括但不限于工作线程用尽、可用性组系统视图和 DMV 响应时间很长和/或调度程序系统转储停滞。 请务必用接近生产的工作负荷彻底测试您的环境，确保它可以应对您的应用程序 SLA 内的工作负荷蜂值。 考虑 SLA 时，确保考虑故障条件下的负荷以及期望的响应时间。  
  
-   **不要使用故障转移群集管理器来操作可用性组**。 SQL Server 故障转移群集实例 (FCI) 的状态在 SQL Server 和 Windows故障转移群集 (WSFC) 之间共享，并且 SQL Server 保留的实例状态信息的详细程度超出群集所需的程度。 管理模型为 SQL Server 必须驱动事务，并负责使群集的状态视图与 SQL Server 的状态视图保持同步。 如果在 SQL Server 外部更改了群集的状态，则该状态有可能在 WSFC 和 SQL Server 之间不同步，这可能会导致意外行为。
  
     例如：  
  
    -   不要更改任何可用性组属性，例如可能的所有者。  
  
    -   不要使用故障转移群集管理器来故障转移可用性组。 必须使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]。  
  
###  <a name="prerequisites-availability-groups"></a><a name="RequirementsAG"></a> 先决条件（可用性组）  
 在创建或重新配置可用性组配置时，请确保您遵守以下要求。  
  
|先决条件|说明|  
|------------------|-----------------|  
|如果您计划使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例 (FCI) 承载可用性副本，则请确保您理解 FCI 限制并且满足 FCI 要求。|[有关使用 SQL Server 故障转移群集实例 (FCI) 承载可用性副本的先决条件和限制](#FciArLimitations)（本文前面已予以介绍）|  
  
###  <a name="security-availability-groups"></a><a name="SecurityAG"></a> 安全性（可用性组）  
  
-   从 WSFC 继承安全性。 Windows Server 故障转移群集为整个群集粒度提供两级用户安全性：  
  
    -   只读访问  
  
    -   完全控制  
  
         [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 需要完全控制，在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上启用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 将（通过服务 SID）向其提供对群集的完全控制。  
  
         不能在群集管理器中直接添加或删除服务器实例的安全性。 若要管理群集安全性会话，请使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中等效的 WMI。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的每个实例都必须具有访问注册表、群集等的权限。  
  
-   我们建议您为承载 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 可用性副本的服务器实例之间的连接使用加密。  
  
#### <a name="permissions-availability-groups"></a>权限（可用性组）  
  
|任务|所需的权限|  
|----------|--------------------------|  
|创建可用性组|需要 **sysadmin** 固定服务器角色的成员资格，以及 CREATE AVAILABILITY GROUP 服务器权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。|  
|更改可用性组|对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。<br /><br /> 此外，将数据库联接到可用性组要求具有 **db_owner** 固定服务器角色的成员身份。|  
|删除可用性组|对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。 若要删除并非在本地副本位置上承载的某一可用性组，您需要针对该可用性组的 CONTROL SERVER 权限或 CONTROL 权限。|  
  
###  <a name="related-tasks-availability-groups"></a><a name="RelatedTasksAGs"></a> 相关任务（可用性组）  
  
|任务|项目|  
|----------|-----------|  
|创建可用性组|[使用可用性组（“新建可用性组”向导）](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)<br /><br /> [创建可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)<br /><br /> [创建可用性组 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)<br /><br /> [在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)|  
|修改可用性副本的数目|[将辅助副本添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)<br /><br /> [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)<br /><br /> [将次要副本从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)|  
|创建可用性组侦听器|[创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)|  
|删除可用性组|[删除可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)|  
  
##  <a name="availability-database-prerequisites-and-restrictions"></a><a name="PrerequisitesForDbs"></a> 可用性数据库先决条件和限制  
 若要符合添加到可用性组的条件，数据库必须满足以下先决条件和限制。  
  
 **本节内容：**  
  
-   [要求](#RequirementsDb)  
  
-   [限制](#RestrictionsDb)  
  
-   [针对承载可用性副本的计算机的建议（Windows 系统）](#TDEdbs)  
  
-   [权限](#PermissionsDbs)  
  
-   [相关任务](#RelatedTasksADb)  
  
###  <a name="checklist-requirements-availability-databases"></a><a name="RequirementsDb"></a>清单：要求（可用性数据库）  
 为了符合添加到可用性组的条件，数据库必须：  
  
|要求|链接|  
|------------------|----------|  
|是用户数据库。 系统数据库无法属于可用性组。||  
|驻留在您在其中创建可用性组并且对于服务器实例可访问的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上。||  
|是读写数据库。 只读数据库不能添加到可用性组。|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**is_read_only** = 0)|  
|是多用户数据库。|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**user_access** = 0)|  
|未使用 AUTO_CLOSE。|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**is_auto_close_on** = 0)|  
|使用完整恢复模式。|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**recovery_model** = 1)|  
|拥有至少一个完整的数据库备份。<br /><br /> 注意：将数据库设置为完整恢复模式之后，将需要一个完整的备份来启动完整恢复日志链。|[创建完整数据库备份 (SQL Server)](../../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)|  
|不属于任何现有可用性组。|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**group_database_id** = NULL)|  
|不是为数据库镜像配置的。|[sys.database_mirroring](../../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md) （如果数据库未参与镜像，则所有带有“mirroring_”前缀的列将为 NULL。）|  
|在将使用 FILESTREAM 的数据库添加到某一可用性组之前，请确保在承载或将承载该可用性组的可用性副本的每个服务器实例上都启用 FILESTREAM。|[启用和配置 FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)|  
|在将包含的数据库添加到某一可用性组之前，请确保在将承载该可用性组的可用性副本的每个服务器实例上 **contained database authentication** 服务器选项都设置为 **1** 。|[contained database authentication 服务器配置选项](../../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md)<br /><br /> [服务器配置选项 (SQL Server)](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)|  
  
> [!NOTE]  
>  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]可使用任何受支持的数据库兼容性级别。  
  
###  <a name="restrictions-availability-databases"></a><a name="RestrictionsDb"></a> 限制（可用性数据库）  
  
-   如果辅助数据库的文件路径（包括驱动器号）不同于相应主数据库的路径，则以下限制适用：  
  
    -   [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]/[!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)]：不支持“完全”选项（在[选择初始数据同步页](../../../database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards.md)页面上），  
  
    -   **RESTORE WITH MOVE：** 若要创建辅助数据库，在承载辅助副本的每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上数据库文件必须是 RESTORED WITH MOVE。  
  
    -   对添加文件操作的影响：以后针对主要副本的添加文件操作在辅助数据库上可能会失败。 此失败可能导致辅助数据库暂停。 而这又会导致辅助副本进入“非同步”状态。  
  
        > [!NOTE]  
        >  有关如何处理失败的添加文件操作的详细信息，请参阅[添加文件操作失败的故障排除（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)。  
  
-   您不能删除当前属于可用性组的数据库。  
  
###  <a name="follow-up-for-tde-protected-databases"></a><a name="TDEdbs"></a> 关注受 TDE 保护的数据库  
 如果使用透明数据加密 (TDE)，则用于创建和解密其他密钥的证书或非对称密钥在承载可用性组的可用性副本的每个服务器实例上必须相同。 有关详细信息，请参阅 [将受 TDE 保护的数据库移到其他 SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)。  
  
###  <a name="permissions-availability-databases"></a><a name="PermissionsDbs"></a> 权限（可用性数据库）  
 需要对数据库拥有 ALTER 权限。  
  
###  <a name="related-tasks-availability-databases"></a><a name="RelatedTasksADb"></a> 相关任务（可用性数据库）  
  
|任务|项目|  
|----------|-----------|  
|准备辅助数据库（手动）|[为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)|  
|将辅助数据库联接到可用性组（手动）|[将辅助数据库联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)|  
|修改可用性数据库的数目|[将数据库添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)<br /><br /> [将辅助数据库从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)<br /><br /> [将主数据库从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)|  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [用于高可用性和灾难恢复的 Microsoft SQL Server AlwaysOn 解决方案指南](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [SQL Server Always On 团队博客：SQL Server Always On 团队官方博客](/archive/blogs/sqlalwayson/)  
  
-   [Always On - HADRON 学习系列：启用了 HADRON 的数据库的工作线程池用法](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [故障转移群集和 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)   
 [AlwaysOn 客户端连接 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)  
  
    
  
--------------------------------------------------