---
title: 为可用性组配置灵活的自动故障转移策略
description: 介绍如何使用 Transact-SQL (T-SQL)、PowerShell 或 SQL Server Management Studio 为 Always On 可用性组配置灵活的故障转移策略。
ms.date: 11/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], flexible failover policy
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 1ed564b4-9835-4245-ae35-9ba67419a4ce
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.custom: seo-lt-2019
ms.openlocfilehash: da02e9aeed2640b892bb2a24fca1aa450257bea1
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584491"
---
# <a name="configure-a-flexible-automatic-failover-policy-for-an-always-on-availability-group"></a>为 Always On 可用性组配置灵活的自动故障转移策略

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  本主题介绍如何在 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 中使用 SQL Server 或 PowerShell 为 Always On 可用性组配置灵活故障转移策略。 灵活的故障转移策略提供了对导致可用性组 [自动执行故障转移](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) 的条件的精确控制。 通过更改触发自动故障转移的失败条件和运行状况检查的频率，可增大或减小自动进行故障转移来支持高可用性 SLA 的可能性。  

   可用性组的灵活的故障转移策略是由其失败条件级别和运行状况检查超时阈值定义的。 在检测到某个可用性组已超出其失败条件级别或其运行状况检查超时阈值时，该可用性组的资源 DLL 会响应 Windows Server 故障转移群集 (WSFC) 群集。 之后，WSFC 群集会启动到辅助副本的自动故障转移。  
 
  > [!NOTE]  
  > 不能使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]配置可用性组的灵活故障转移策略。  
  
 
## <a name="limitations-on-automatic-failovers"></a><a name="Limitations"></a> 自动故障转移的限制  
  
-   要执行自动故障转移，当前主副本和一个辅助副本必须配置为使用自动故障转移的同步提交可用性模式，而且辅助副本必须与主副本同步。  
  
-   只有三个副本支持自动故障转移。  
  
-   如果某个可用性组超过其 WSFC 故障阈值，则该 WSFC 群集不会尝试为该可用性组执行自动故障转移。 此外，该可用性组的 WSFC 资源组始终保持失败状态，直到群集管理员手动将该失败的资源组联机，或是数据库管理员对该可用性组执行手动故障转移。 WSFC 故障阈值  定义为给定时间段中可用性组所支持的最大故障数。 默认时间段为六个小时，此时间段中最大故障数的默认值为 *n*-1，其中 *n* 是 WSFC 节点的数目。 若要更改给定的可用性组的故障阈值，请使用 WSFC 故障转移管理器控制台。  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   您必须连接到承载主副本的服务器实例。  
   
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
  
|任务|权限|  
|----------|-----------------|  
|为新的可用性组配置灵活故障转移策略|需要 **sysadmin** 固定服务器角色的成员资格，以及 CREATE AVAILABILITY GROUP 服务器权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。|  
|修改现有可用性组的策略|对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。|  

##  <a name="health-check-timeout-threshold"></a><a name="HCtimeout"></a> 运行状况检查超时阈值  
 可用性组的 WSFC 资源 DLL 将通过在承载主副本的 SQL Server 实例上调用 *sp_server_diagnostics* 存储过程来对主副本执行 [运行状况检查](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) 。 **sp_server_diagnostics** 按等于可用性组的运行状况检查超时阈值的 1/3 的时间间隔返回结果。 默认的运行状况检查超时阈值为 30 秒，这将导致 **sp_server_diagnostics** 按 10 秒的时间间隔返回结果。 如果 **sp_server_diagnostics** 的运行速度较慢且未返回信息，则资源 DLL 将在等待运行状况检查超时阈值的完全时间间隔后确定主副本已停止响应。 如果主副本停止响应，则启动自动故障转移（如果当前受支持）。  
  
> [!IMPORTANT]  
>  **sp_server_diagnostics** 在数据库级别不执行运行状况检查。  
  
##  <a name="failure-condition-level"></a><a name="FClevel"></a> 故障条件级别  
 由 **sp_server_diagnostics** 返回的诊断数据和运行状况信息是否保证触发自动故障转移取决于可用性组的失败条件级别。 *失败条件级别* 指定触发自动故障转移的失败条件。 有 5 个失败条件级别，其范围从最少限制（级别 1）到最多限制（级别 5）。 给定级别包含限制较少的级别。 因此，最严格的级别 5 包含四个限制更少的条件，依此类推。  
  
> [!IMPORTANT]  
>  任何级别的故障条件均不检测已损坏的数据库和可疑数据库。 因此，已损坏的或可疑的数据库（无论是由于硬件故障、数据损坏还是其他问题导致）从不触发自动故障转移。  
  
 下表介绍了与各级别相对应的失败条件。  
  
|级别|失败条件|[!INCLUDE[tsql](../../../includes/tsql-md.md)] 值|PowerShell 值|  
|-----------|-----------------------|------------------------------|----------------------|  
|一种|当服务器关闭时。 指定在发生以下某种情况时启动自动故障转移：<br /><br /> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务停止。<br /><br /> 因为没有从服务器实例接收到 ACK，连接到 WSFC 群集的可用性组的租期到期。 有关详细信息，请参阅 [工作原理：SQL Server AlwaysOn 租约超时](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268)。<br /><br /> <br /><br /> 这是限制最少的级别。|1|**OnServerDown**|  
|两种|当服务器无响应时。 指定在发生以下某种情况时启动自动故障转移：<br /><br /> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例未连接到群集，并且超出了可用性组的用户指定的运行状况检查超时阈值。<br /><br /> 可用性副本处于失败状态。|2|**OnServerUnresponsive**|  
|三级|出现严重服务器错误时。 指定在发生了严重的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 内部错误（例如孤立的自旋锁、严重的写访问冲突或过多的转储）时启动自动故障转移。<br /><br /> 这是默认级别。|3|**OnCriticalServerError**|  
|四级|出现严重服务器错误时。 指定在发生了中等 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 内部错误（在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 内部资源池中出现持久的内存不足情况）时启动自动故障转移。|4|**OnModerateServerError**|  
|五级|在出现任何限定的失败条件时。 指定在出现任何符合的失败条件时启动自动故障转移，这些失败条件包括：<br /><br /> 检测计划程序死锁。<br /><br /> 检测到无法解决的死锁。<br /><br /> <br /><br /> 这是限制最多的级别。|5|**OnAnyQualifiedFailureConditions**|  
  
> [!NOTE]  
>  缺少 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例对客户端请求的响应与可用性组无关。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **配置灵活故障转移策略**  
  
1.  连接到承载主副本的服务器实例。  
  
2.  对于新的可用性组，请使用 [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句。 如果要修改现有可用性组，请使用 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句。  
  
    -   若要设置故障转移条件级别，请使用 FAILURE_CONDITION_LEVEL = *n* 选项，其中， *n* 是 1 到 5 的整数。  
  
         例如，以下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句将现有可用性组 `AG1`的故障条件级别更改为一级：  
  
        ```  
  
        ALTER AVAILABILITY GROUP AG1 SET (FAILURE_CONDITION_LEVEL = 1);  
        ```  
  
         这些整数值与故障条件级别的关系如下：  
  
        |[!INCLUDE[tsql](../../../includes/tsql-md.md)] 值|级别|当出现以下情况时，自动启动故障转移…|  
        |------------------------------|-----------|-------------------------------------------|  
        |1|一种|当服务器关闭时。 SQL Server 服务因故障转移或重新启动而停止。|  
        |2|两种|当服务器无响应时。 满足任何下限值条件，SQL Server 服务连接到群集，超过运行状况检查超时阈值，或当前主副本处于失败状态。|  
        |3|三级|出现严重服务器错误时。 满足任何下限值条件或发生严重的内部服务器错误。<br /><br /> 这是默认级别。|  
        |4|四级|出现严重服务器错误时。 满足任何下限值条件或发生中度的服务器错误。|  
        |5|五级|在出现任何限定的失败条件时。 满足任何下限值条件或出现限定的失败条件。|  
  
         有关故障转移条件级别的更多信息，请参阅[针对可用性组的自动故障转移的灵活的故障转移策略 (SQL Server)]()。  
  
    -   若要配置运行状况检查超时阈值，请使用 HEALTH_CHECK_TIMEOUT = *n* 选项，其中， *n* 是一个从 15000 毫秒（15 秒）到 4294967295 毫秒的整数。 默认值为 30000 毫秒（30 秒）  
  
         例如，以下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句会将现有可用性组 `AG1`的运行状况检查超时阈值更改为 60,000 毫秒（1 分钟）。  
  
        ```  
  
        ALTER AVAILABILITY GROUP AG1 SET (HEALTH_CHECK_TIMEOUT = 60000);  
        ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **配置灵活故障转移策略**  
  
1.  将默认值 (**cd**) 设置为托管主副本的服务器实例。  
  
2.  在将可用性副本添加到可用性组中时，请使用 **New-SqlAvailabilityGroup** cmdlet。 在修改现有可用性副本时，请使用 **Set-SqlAvailabilityGroup** cmdlet。  
  
    -   若要设置故障转移条件级别，请使用 **FailureConditionLevel**_level_ 参数，其中 *level* 为以下值之一：  
  
        |值|级别|当出现以下情况时，自动启动故障转移…|  
        |-----------|-----------|-------------------------------------------|  
        |**OnServerDown**|一种|当服务器关闭时。 SQL Server 服务因故障转移或重新启动而停止。|  
        |**OnServerUnresponsive**|两种|当服务器无响应时。 满足任何下限值条件，SQL Server 服务连接到群集，超过运行状况检查超时阈值，或当前主副本处于失败状态。|  
        |**OnCriticalServerError**|三级|出现严重服务器错误时。 满足任何下限值条件或发生严重的内部服务器错误。<br /><br /> 这是默认级别。|  
        |**OnModerateServerError**|四级|出现严重服务器错误时。 满足任何下限值条件或发生中度的服务器错误。|  
        |**OnAnyQualifiedFailureConditions**|五级|在出现任何限定的失败条件时。 满足任何下限值条件或出现限定的失败条件。|  
  
         有关故障转移条件级别的更多信息，请参阅[针对可用性组的自动故障转移的灵活的故障转移策略 (SQL Server)]()。  
  
         例如，以下命令会将现有可用性组 `AG1`的故障条件级别更改为一级。  
  
        ```  
        Set-SqlAvailabilityGroup `   
        -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg `   
        -FailureConditionLevel OnServerDown  
        ```  
  
    -   若要设置运行状况检查超时阈值，则使用 **HealthCheckTimeout**_n_ 参数，其中 *n* 是介于 15000 毫秒（15 秒）和 4294967295 毫秒之间的一个整数。 默认值为 30000 毫秒（30 秒）。  
  
         例如，以下命令会将现有可用性组 `AG1`的运行状况检查超时阈值更改为 120,000 毫秒（2 分钟）。  
  
        ```  
        Set-SqlAvailabilityGroup `   
        -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAG `   
        -HealthCheckTimeout 120000  
        ```  
  
> [!NOTE]  
>  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  

##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **配置自动故障转移**  
  
-   [更改可用性副本的可用性模式 (SQL Server) ](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)（自动故障转移要求同步提交可用性模式）  
  
-   [更改可用性副本的故障转移模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [配置灵活的故障转移策略以控制自动故障转移的条件（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [工作原理：SQL Server AlwaysOn 租约超时](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [Windows Server 故障转移群集 (WSFC) 与 SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [故障转移群集实例的故障转移策略](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)   
 [sp_server_diagnostics (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)  
  
