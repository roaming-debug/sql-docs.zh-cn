---
title: 监视可用性组的工具
description: '可用于监视 Always On 可用性组的性能和运行状态的各种工具的参考。 '
ms.custom: seodec18
ms.date: 06/08/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], troubleshooting
ms.assetid: 1d5e3291-0d0a-45a1-88e5-1fc242d17210
author: cawrites
ms.author: chadam
ms.openlocfilehash: 734c89393b77fa74db57ce2896af64bbac7c1a63
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643717"
---
# <a name="tools-to-monitor-always-on-availability-groups"></a>“监视 Always On 可用性组的工具”
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  若要监视 AlwaysOn 可用性组的属性和状态，可以使用以下工具。  
  
|工具|简要说明|链接|  
|----------|-----------------------|-----------|  
|SQL Server 的系统中心监视包|对于 IT 管理员，建议使用 SQL Server (SQLMP) 的监视包这一解决方案来监视可用性组、可用性副本和可用性数据库。 与 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 尤为相关的监视功能包括：<br /><br /> 可自动发现数百台计算机中的可用性组、可用性副本和可用性数据库。 这使您能够轻松地跟踪 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 清单。<br /><br /> 功能完善的 System Center Operations Manager (SCOM) 报警和票证。 这些功能提供了详细的知识，使您可以更快地解决问题。<br /><br /> 使用基于策略的管理 (PBM) 对 AlwaysOn 运行状况监视进行自定义扩展。<br /><br /> 从可用性数据库到可用性副本累积运行状况信息。<br /><br /> 用于从 System Center Operations Manager 控制台中管理 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的自定义任务。|若要下载监视包 (SQLServerMP.msi) 和 *用于 System Center Operations Manager 的 SQL Server 管理包指南* (SQLServerMPGuide.doc)，请参阅：<br /><br /> [SQL Server 的系统中心监视包](https://www.microsoft.com/download/details.aspx?displaylang=en&id=10631)|  
|[!INCLUDE[tsql](../../../includes/tsql-md.md)]|[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 目录和动态管理视图提供了有关可用性组及其副本、数据库、侦听器和 WSFC 群集环境的大量信息。|[监视可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)|  
|[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]|**“对象资源管理器详细信息”** 窗格显示有关您连接到的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例所承载的可用性组的基本信息。<br /><br /> **\*\* 提示 \*\*** 使用此窗格可以选择多个可用性组、副本或数据库，并能对选定对象执行常规管理任务；例如，从可用性组中删除多个可用性副本或数据库。|[使用对象资源管理器详细信息监视可用性组 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)|  
|[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]|**“属性”** 对话框使您能够查看可用性组、副本或侦听器的属性，并在某些情况下可更改这些属性的值。|-   [查看可用性组属性 (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)<br />-   [查看可用性副本属性 (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)<br />-   [查看可用性组侦听程序属性 (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)|  
|系统监视器|**SQLServer:Availability Replica** 性能对象包含性能计数器，可报告可用性副本的相关信息。|[SQL Server，可用性副本](../../../relational-databases/performance-monitor/sql-server-availability-replica.md)|  
|系统监视器|**SQLServer:Database Replica** 性能对象包含性能计数器，可报告给定次要副本上的辅助数据库的相关信息。<br /><br /> SQL Server 中的 **SQLServer:Databases** 对象包含用于监视事务日志活动（但不仅限于此）的性能计数器。 下列计数器特别适用于监视可用性数据库上的事务日志活动： **日志刷新写入时间(秒)** 、 **日志刷新次数/秒**、 **日志池缓存未命中数/秒**、 **日志池磁盘读取次数/秒** 和 **日志池请求数/秒**。|[SQL Server，数据库副本](../../../relational-databases/performance-monitor/sql-server-database-replica.md) ；和 [SQL Server，数据库对象](../../../relational-databases/performance-monitor/sql-server-databases-object.md)|  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   **博客：**  
  
     [AlwaysOn 运行状况模型第 1 部分 - 运行状况模型体系结构](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-1-health-model-architecture)  
  
     [AlwaysOn 运行状况模型第 2 部分 - 扩展运行状况模型](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model)  
  
     [使用 PowerShell 监视 AlwaysOn 运行状况 - 第 1 部分：基本 Cmdlet 概述](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-1-basic-cmdlet-overview)  
  
     [使用 PowerShell 监视 AlwaysOn 运行状况 - 第 2 部分：高级 Cmdlet 用法](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-2-advanced-cmdlet-usage)  
  
     [使用 PowerShell 监视 AlwaysOn 运行状况 - 第 3 部分：简单的监视应用程序](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-3-a-simple-monitoring-application)  
  
     [使用 PowerShell 监视 AlwaysOn 运行状况 - 第 4 部分：与 SQL Server 代理集成](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-4-integration-with-sql-server-agent)  
  
     [SQL Server AlwaysOn 团队博客：SQL Server AlwayOn 团队官方博客](/archive/blogs/sqlalwayson/)  
  
     [CSS SQL Server 工程师博客](/archive/blogs/psssql/)  
  
-   **白皮书：**  
  
     [针对 SQL Server 2012 的 Microsoft 白皮书](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [SQL Server 客户咨询团队白皮书](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组目录视图 (Transact-SQL)](../../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [AlwaysOn 可用性组动态管理视图和函数 (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [针对可用性组的自动故障转移的灵活的故障转移策略 (SQL Server)](./configure-flexible-automatic-failover-policy.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [自动页修复（可用性组：数据库镜像）](../../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [使用 AlwaysOn 面板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
