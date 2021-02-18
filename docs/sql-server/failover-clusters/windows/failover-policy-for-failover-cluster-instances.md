---
title: 故障转移群集实例的故障转移策略
description: 介绍了适用于 SQL Server 故障转移群集实例的不同故障转移策略。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- flexible failover policy
ms.assetid: 39ceaac5-42fa-4b5d-bfb6-54403d7f0dc9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4816bae2ba4317970dd801b07499c3653e774d4a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340025"
---
# <a name="failover-policy-for-failover-cluster-instances"></a>Failover Policy for Failover Cluster Instances
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例 (FCI) 中，在给定的时间只有一个节点可以拥有 Windows Server 故障转移群集 (WSFC) 群集资源组。 客户端请求通过 FCI 中的此节点进行处理。 在发生故障和重新启动失败时，组的所有权将转移给 FCI 中的另一个 WSFC 节点。 此过程称为故障转移。 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 提高了故障检测的可靠性，并提供灵活的故障转移策略。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 依赖于基础 WSFC 服务进行故障转移检测。 因此，两个机制决定了 FCI 的故障转移行为：一个是本机 WSFC 功能，一个是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序带来的功能。  
  
-   WSFC 群集维护仲裁配置，这确保自动故障转移中的故障转移目标唯一。 WSFC 服务决定群集在任何时间是否处于最佳仲裁状态并相应使资源组联机或脱机。  
  
-   活动的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例通过专用连接定期向 WSFC 资源组报告一组组件诊断信息。 WSFC 资源组维护故障转移策略，该策略定义触发重新启动和故障转移的故障条件。  
  
 本主题讨论上面的第二个机制。 有关仲裁配置和运行状况检测的 WSFC 行为的详细信息，请参阅 [WSFC 仲裁模式和投票配置 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md)。  
  
> [!IMPORTANT]  
>  AlwaysOn 可用性组中不允许自动故障转移到 FCI 或从 FCI 自动故障转移。 但是，可以在 AlwaysOn 可用性组中手动故障转移到 FCI 或从 FCI 手动故障转移。  
  
##  <a name="failover-policy-overview"></a><a name="Concepts"></a> 故障转移策略概述  
 故障转移过程可以分为以下步骤：  
  
1.  [监视运行状态](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#monitor)  
  
2.  [确定故障](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#determine)  
  
3.  [响应故障](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#respond)  
  
###  <a name="monitor-the-health-status"></a><a name="monitor"></a> 监视运行状态  
 为 FCI 监视三种运行状态：  
  
-   [SQL Server 服务的状态](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#service)  
  
-   [SQL Server 实例的响应情况](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#instance)  
  
-   [SQL Server 组件诊断](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#component)  
  
####  <a name="state-of-the-sql-server-service"></a><a name="service"></a> SQL Server 服务的状态  
 WSFC 服务监视活动 FCI 节点上的 SQL Server 服务的启动状态，以检测 SQL Server 服务何时停止。  
  
####  <a name="responsiveness-of-the-sql-server-instance"></a><a name="instance"></a> SQL Server 实例的响应情况  
 在 SQL Server 启动期间，WSFC 服务使用 SQL Server 数据库引擎资源 DLL 在它专用的单独线程上创建一个专门用于监视运行状态的新连接。 这确保 SQL 实例在负载下有所需的资源来报告其运行状态。 使用此专用连接，SQL Server 反复运行 [sp_server_diagnostics (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) 系统存储过程以定期将 SQL Server 组件的运行状态报告给资源 DLL。  
  
 资源 DLL 使用运行状况检查超时值确定 SQL 实例的响应情况。 HealthCheckTimeout 属性定义资源 DLL 在向 WSFC 服务报告 SQL 实例不响应前应等待 sp_server_diagnostics 存储过程多长时间。 此属性可使用 T-SQL 和故障转移群集管理器管理单元进行配置。 有关详细信息，请参阅 [Configure HealthCheckTimeout Property Settings](../../../sql-server/failover-clusters/windows/configure-healthchecktimeout-property-settings.md)。 以下项描述了此属性如何影响超时和重复间隔设置：  
  
-   资源 DLL 调用 sp_server_diagnostics 存储过程，并将重复间隔设置为 HealthCheckTimeout 值的三分之一。  
  
-   如果 sp_server_diagnostics 存储过程运行缓慢或没有返回信息，资源 DLL 将等待 HealthCheckTimeout 所指定的间隔后向 WSFC 服务报告 SQL 实例不响应。  
  
-   如果专用连接丢失，资源 DLL 将在 HealthCheckTimeout 所指定的间隔内重试连接到 SQL 实例，然后向 WSFC 服务报告 SQL 实例不响应。  
  
####  <a name="sql-server-component-diagnostics"></a><a name="component"></a> SQL Server 组件诊断  
 系统存储过程 sp_server_diagnostics 定期收集 SQL 实例的组件诊断信息。 收集的诊断信息将针对以下每个组件显示一行，并且这些信息将传递给调用线程：  
  
1.  system  
  
2.  resource  
  
3.  查询进程  
  
4.  io_subsystem  
  
5.  活动  
  
 系统、资源和查询进程组件用于故障检测。 io_subsytem 和事件组件仅用于诊断目的。  
  
 还将信息的每个行集写入 SQL Server 群集诊断日志。 有关详细信息，请参阅 [查看和读取故障转移群集实例诊断日志](../../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md)。  
  
> [!TIP]  
>  SQL Server AlwaysOn 技术使用 sp_server_diagnostic 存储过程时，该存储过程可用于任何 SQL Server 实例以帮助检测和解决问题。  
  
####  <a name="determining-failures"></a><a name="determine"></a> 确定故障  
 SQL Server 数据库引擎资源 DLL 使用 FailureConditionLevel 属性确定检测到的运行状态是否符合故障条件。 FailureConditionLevel 属性定义哪些检测到的运行状态导致重新启动或故障转移。 提供了多个选项级别，涵盖不自动重新启动或故障转移到导致自动重新启动或故障转移的所有可能故障条件。 有关如何配置此属性的详细信息，请参阅 [Configure FailureConditionLevel Property Settings](../../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md)。  
  
 针对递增的级别设置故障条件。 对于级别 1-5，每个级别除了自己的条件外，还包括之前级别的所有条件。 这意味着，每个级别越大，故障转移或重新启动的几率不断加大。 下表介绍了这些故障条件级别：  
  
 查看 [sp_server_diagnostics (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)，因为此系统存储过程在故障条件级别中起重要作用。  
  
|级别|条件|描述|  
|-----------|---------------|-----------------|  
|0|无自动故障转移或重新启动|表示对于任何故障条件将不自动触发故障转移或重新启动。 此级别仅适用于系统维护目的。|  
|1|服务器关闭时进行故障转移或重新启动|指示当满足以下条件时将触发服务器重新启动或故障转移：<br /><br /> SQL Server 服务停止。|  
|2|服务器不响应时进行故障转移或重新启动|指示当满足以下任意条件时将触发服务器重新启动或故障转移：<br /><br /> SQL Server 服务停止。<br /><br /> SQL Server 实例不响应（资源 DLL 在 HealthCheckTimeout 设置时间内未收到来自 sp_server_diagnostics 的数据）。|  
|3*|出现严重服务器错误时进行故障转移或重新启动|指示当满足以下任意条件时将触发服务器重新启动或故障转移：<br /><br /> SQL Server 服务停止。<br /><br /> SQL Server 实例不响应（资源 DLL 在 HealthCheckTimeout 设置时间内未收到来自 sp_server_diagnostics 的数据）。<br /><br /> 系统存储过程 sp_server_diagnostics 返回“系统错误”。|  
|4|出现中等服务器错误时进行故障转移或重新启动|指示当满足以下任意条件时将触发服务器重新启动或故障转移：<br /><br /> SQL Server 服务停止。<br /><br /> SQL Server 实例不响应（资源 DLL 在 HealthCheckTimeout 设置时间内未收到来自 sp_server_diagnostics 的数据）。<br /><br /> 系统存储过程 sp_server_diagnostics 返回“系统错误”。<br /><br /> 系统存储过程 sp_server_diagnostics 返回“资源错误”。|  
|5|对于任何合格的故障条件进行故障转移或重新启动|指示当满足以下任意条件时将触发服务器重新启动或故障转移：<br /><br /> SQL Server 服务停止。<br /><br /> SQL Server 实例不响应（资源 DLL 在 HealthCheckTimeout 设置时间内未收到来自 sp_server_diagnostics 的数据）。<br /><br /> 系统存储过程 sp_server_diagnostics 返回“系统错误”。<br /><br /> 系统存储过程 sp_server_diagnostics 返回“资源错误”。<br /><br /> 系统存储过程 sp_server_diagnostics 返回“query_processing 错误”。|  
  
 *默认值  
  
####  <a name="responding-to-failures"></a><a name="respond"></a> 响应故障  
 在检测到一个或多个故障转移条件后，WSFC 服务如何响应故障依赖于 WSFC 仲裁状态以及 FCI 资源组的重新启动和故障转移设置。 如果 FCI 失去 WSFC 仲裁，则整个 FCI 将脱机并且 FCI 失去高可用性。 如果 FCI 仍保留 WSFC 仲裁，则 WSFC 服务可能首次尝试重新启动失败的节点，如果重新启动尝试失败则进行故障转移。 在故障转移群集管理器管理单元中配置重新启动和故障转移设置。 有关这些设置的详细信息，请参阅 [\<Resource> 属性：“策略”选项卡](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc725685(v=ws.11))。  
  
 有关维护仲裁运行状况的详细信息，请参阅：[WSFC 仲裁模式和投票配置 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER SERVER CONFIGURATION (Transact-SQL)](../../../t-sql/statements/alter-server-configuration-transact-sql.md)  
  
