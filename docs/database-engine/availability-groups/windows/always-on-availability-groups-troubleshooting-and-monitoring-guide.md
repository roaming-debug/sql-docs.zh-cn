---
title: 可用性组的监视和故障排除指南
description: 一个内容索引，用于帮助开始监视 Always On 可用性组并排除在其中发现的一些常见问题。
ms.custom: seo-lt-2019
ms.date: 05/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
ms.assetid: 8d6d9954-ff6b-4e58-882e-eff0174f0d07
author: rothja
ms.author: jroth
ms.openlocfilehash: 5594a39b6ab4f8c9c6bd2edaadbe64e99c983229
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643690"
---
# <a name="monitor-and-troubleshoot-availability-groups"></a>监视可用性组并对其进行故障排除
 本指南可帮助你开始监视 Always On 可用性组并排查可用性组的一些常见问题。 它提供了原始内容，以及在其他位置发布的有用信息的登陆页。 尽管本指南不能全面探讨大范围可用性组中出现的所有问题，但它可为分析根本原因和解决问题指出正确的方向。 
 
 由于可用性组是集成技术，因此遇到的许多问题可能是数据库系统中其他问题的症状。 部分问题由可用性组内的设置造成，例如可用性数据库被挂起。 其他问题可能包括 SQL Server 其他方面的问题，例如 SQL Server 设置、数据库文件部署和与可用性无关的系统性能问题。 可能还有一些问题发生在 SQL Server 之外，例如网络 I/O、TCP/IP、Active Directory 和 Windows Server 故障转移群集 (WSFC) 问题。 通常情况下，出现在可用性组、副本或数据库中的问题需要对多个技术进行故障排除才能确定根本原因。  
  
  
##  <a name="troubleshooting-scenarios"></a><a name="BKMK_SCENARIOS"></a>故障排除方案  
 下表包含指向可用性组常见故障排除方案的链接。 它们按方案类型（如配置、客户端连接、故障转移和性能）分类。  
  
|场景|方案类型|说明|  
|--------------|-------------------|-----------------|  
|[Always On 可用性组配置故障排除 (SQL Server)](troubleshoot-always-on-availability-groups-configuration-sql-server.md)|配置|提供相关信息，帮助解决在为可用性组配置服务器实例时遇到的典型问题。 典型配置问题包括可用性组被禁用、帐户配置不当、数据库镜像终结点不存在、终结点无法访问（SQL Server 错误 1418）、网络访问不存在，以及联接数据库命令失败（SQL Server 错误 35250）。|  
|[添加文件操作失败的故障排除（Always On 可用性组）](troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)|配置|添加文件操作导致辅助数据库挂起并处于“未同步”状态。|  
|[无法在多子网环境中连接到可用性组侦听程序](https://support.microsoft.com/kb/2792139/en-us)|客户端连接性|配置可用性组侦听程序后，便无法 ping 侦听程序或从应用程序中连接它。|  
|[自动故障转移失败的故障排除](https://support.microsoft.com/kb/2833707)|故障转移|自动故障转移未成功完成。|  
|[疑难解答：可用性组超过了 RTO](troubleshoot-availability-group-exceeded-rto.md)|性能|进行自动故障转移或计划的手动故障转移（无数据丢失）后，故障转移时间超过 RTO。 或者，在估计同步提交次要副本（如自动故障转移伙伴）的故障转移时间时，发现该时间超过 RTO。|  
|[疑难解答：可用性组超过了 RPO](troubleshoot-availability-group-exceeded-rpo.md)|性能|执行强制手动故障转移后，数据丢失超过 RPO。 或者，在计算异步提交次要副本可能丢失的数据时，发现它超过了 RPO。|  
|[故障排除：主要副本的更改未反映在次要副本上](troubleshoot-primary-changes-not-reflected-on-secondary.md)|性能|客户端应用程序在主要副本上成功完成更新，但查询次要副本显示更改未得到反映。|  
|[故障排除：Always On 可用性组的高 HADR_SYNC_COMMIT 等待类型](/archive/blogs/sql_server_team/troubleshooting-high-hadr_sync_commit-wait-type-with-always-on-availability-groups)|性能|如果 HADR_SYNC_COMMIT 特别长，则数据移动流或次要副本日志强化存在性能问题。|  

##  <a name="useful-tools-for-troubleshooting"></a><a name="BKMK_TOOLS"></a>有用的故障排除工具  
 当配置或运行可用性组时，不同的工具可帮助诊断不同类型的问题。 下表提供的链接指向有关工具的有用信息。  
  
|工具|说明|  
|----------|-----------------|  
|[使用 AlwaysOn 面板 (SQL Server Management Studio)](use-the-always-on-dashboard-sql-server-management-studio.md)|在用户友好界面中针对可用性组运行状况报告一目了然的视图。|  
|[Always On 策略](always-on-policies.md)|由 Always On 仪表板使用。|  
|[SQL Server 错误日志（Always On 可用性组）](sql-server-error-log-always-on-availability-groups.md)|记录可用性组、副本、数据库、其他 Always On 组件状态和 Always On 错误的状态转换事件。|  
|[CLUSTER.LOG（Always On 可用性组）](cluster-log-always-on-availability-groups.md)|记录群集事件，包括可用性组资源的状态转换以及 SQL Server 资源 DLL 中的事件和错误。|  
|[Always On 运行状况诊断日志](always-on-health-diagnostics-log.md)|记录由 [sp_server_diagnostics (Transact-SQL)](~/relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) 向 WSFC 群集（SQL Server 资源 DLL）报告的 SQL Server 运行状况诊断。|  
|[动态管理视图和系统目录视图（Always On 可用性组）](dynamic-management-views-and-system-catalog-views-always-on-availability-groups.md)|报告有关可用性组的信息，如配置、运行状况和性能指标。|  
|[Always On 扩展事件](always-on-extended-events.md)|提供有助于分析根本原因的详细可用性组诊断。|  
|[Always On 等待类型](always-on-wait-types.md)|提供特定于可用性组且可用于性能调整的等待统计信息。|  
|Always On 性能计数器|监视可用性组活动，反映在系统监视器中，并且可用于性能调整。 有关详细信息，请参阅 [SQL Server，可用性副本](~/relational-databases/performance-monitor/sql-server-availability-replica.md)和 [SQL Server，数据库副本](~/relational-databases/performance-monitor/sql-server-database-replica.md)。|  
|[Always On 环形缓冲区](always-on-ring-buffers.md)|记录 SQL Server 系统中的内部诊断警报，可用于调试可用性组相关问题。|  
  
##  <a name="monitoring-availability-groups"></a><a name="BKMK_MONITOR"></a>监视可用性组  
 对可用性组进行故障排除的最佳时机是在出现问题，导致需要故障转移（无论是自动还是手动）之前。 通过监视可用性组的性能指标，并在可用性组副本的执行超出服务级别协议 (SLA) 范围时发送警告，可实现此操作。 例如，同步次要副本出现性能问题，导致估计的故障转移时间增加，且你不想等到自动故障转移发生并发现故障转移时间超过恢复时间目标时。  
  
 可用性组是高可用性和灾难恢复解决方案，因此要监视的最重要的性能指标是估计的故障转移时间以及灾难中可能的数据丢失，前者会影响恢复时间目标 (RTO)，而后者会影响恢复点目标 (RPO)。 可在任何给定时间从 SQL Sever 公开的数据中收集这些指标，以便在发生实际故障事件前，通过系统的高可用性灾难恢复 (HADR) 功能收到有关问题的警报。 因此，请务必熟悉可用性组的数据同步流程并相应地收集有关指标。  
  
 下表指出了可帮助监视可用性组解决方案运行状况的主题。  
  
|主题|说明|  
|-----------|-----------------|  
|[监视 Always On 可用性组的性能](monitor-performance-for-always-on-availability-groups.md)|介绍可用性组的数据同步流程，流控制门以及监视可用性组时的有用指标，并演示如何收集 RTO 和 RPO 指标。|  
|[监视可用性组 (SQL Server)](monitoring-of-availability-groups-sql-server.md)|提供用于监视可用性组的工具的相关信息。|  
|[Always On 运行状况模型，第 1 部分：运行状况模型体系结构](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-1-health-model-architecture)|提供有关 Always On 运行状况模型的概述。|  
|[Always On 运行状况模型，第 2 部分：扩展运行状况模型](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model)|介绍如何自定义 Always On 运行状况模型和如何自定义 Always On 仪表板，以显示额外信息。|  
|[使用 PowerShell 监视 Always On 运行状况，第 1 部分：基本 cmdlet 概述](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-1-basic-cmdlet-overview)|提供有关可用于监视可用性组运行状况的 Always On PowerShell cmdlet 的基本概述。|  
|[使用 PowerShell 监视 Always On 运行状况，第 2 部分：高级 cmdlet 用法](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-2-advanced-cmdlet-usage)|提供有关用于监视可用性组运行状况的 Always On PowerShell cmdlet 的高级用法的信息。|  
|[使用 PowerShell 监视 Always On 运行状况，第 3 部分：简单的监视应用程序](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-3-a-simple-monitoring-application)|介绍如何使用应用程序自动监视可用性组。|  
|[使用 PowerShell 监视 Always On 运行状况，第 4 部分：与 SQL Server 代理集成](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-4-integration-with-sql-server-agent)|提供相关信息，介绍如何将可用性组监视与 SQL Server 代理集成，以及如何配置出现问题时向相应方发送的通知。|  

## <a name="next-steps"></a>后续步骤  
 [SQL Server Always On 团队博客](/archive/blogs/sqlalwayson/)   
 [CSS SQL Server 工程师博客](/archive/blogs/psssql/)  
  
