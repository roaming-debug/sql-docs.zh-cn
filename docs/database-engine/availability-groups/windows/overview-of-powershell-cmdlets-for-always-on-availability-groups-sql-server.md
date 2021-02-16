---
title: 可用性组的 PowerShell Cmdlet
description: '关于可用于管理 AlwaysOn 可用性组的不同 PowerShell cmdlet 的参考。 '
ms.custom: seo-lt-2019
ms.date: 08/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], PowerShell cmdlets
- Availability Groups [SQL Server], about
- PowerShell [SQL Server], cmdlets
ms.assetid: b3fef0d5-b6d7-4386-a0f0-d06c165ad4de
author: cawrites
ms.author: chadam
ms.openlocfilehash: 122a4133de7965b193ee2ca2708da9b48a5c1239
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337905"
---
# <a name="overview-of-powershell-cmdlets-for-always-on-availability-groups"></a>Always On 可用性组的 PowerShell Cmdlet 概述
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] PowerShell 是专门设计用于系统管理的基于任务的命令行 Shell 和脚本语言。 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 在 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 中提供一组 PowerShell cmdlet，支持您部署、管理和监视可用性组、可用性副本和可用性数据库。  
  
> [!NOTE]  
>  一旦成功启动某操作，PowerShell cmdlet 即告完成。 这并不表示要执行的工作（例如可用性组的故障转移）已经完成。 对一系列操作编写脚本时，您可能必须检查这些操作的状态，并等待这些操作完成。  
  
> [!NOTE]  
>  有关描述如何使用 cmdlet 来执行 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 任务的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 联机丛书中的主题列表，请参阅 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)的“相关任务”一节。  
  
##  <a name="configuring-a-server-instance-for-always-on-availability-groups"></a><a name="ConfiguringServerInstance"></a> 服务器实例配置 Alwayson 可用性组  
  
|Cmdlet|说明|支持平台|  
|-------------|-----------------|------------------|
|[**Disable-SqlAlwaysOn**](/powershell/module/sqlserver/disable-sqlalwayson)|禁用服务器实例上的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 功能。|由 **Path**、 **InputObject** 或 **Name** 参数指定的服务器实例。 （必须为支持 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]版本。)|  
|[**Enable-SqlAlwaysOn**](/powershell/module/sqlserver/enable-sqlalwayson)|在支持 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 功能的 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 实例上启用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 。 有关针对 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的支持的信息，请参阅[针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。|任何支持 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]版本。|  
|[**New-SqlHadrEndPoint**](/powershell/module/sqlserver/new-sqlhadrendpoint)|在服务器实例上创建新的数据库镜像端点。 在主数据库和辅数据库之间移动数据时需要此端点。|任何 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|  
|[**Set-SqlHadrEndpoint**](/powershell/module/sqlserver/set-sqlhadrendpoint)|更改现有数据库镜像端点的属性，如名称、状态或身份验证属性。|支持 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 并缺少数据库镜像端点的服务器实例|  

  
##  <a name="backing-up-and-restoring-databases-and-transaction-logs"></a><a name="BnRcmdlets"></a> 备份和还原数据库和事务日志  
  
|Cmdlet|说明|支持平台|  
|-------------|-----------------|------------------|  
|[**Backup-SqlDatabase**](/powershell/module/sqlserver/backup-sqldatabase)|创建数据或日志备份。|任何联机数据库（对于 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]，则为承载主副本的服务器实例上的数据库）|  
|[**Restore-SqlDatabase**](/powershell/module/sqlserver/restore-sqldatabase)|还原备份。|任何 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例（对于 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]，则为承载辅助副本的服务器实例）<br /><br />

  >[!Important]
  >准备辅助数据库时，必须在每个 Restore-SqlDatabase 命令中使用 -NoRecovery 参数   。 
  
 有关使用这些 cmdlet 来准备辅助数据库的信息，请参阅[为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)。  
  
##  <a name="creating-and-managing-an-availability-group"></a><a name="DeployManageAGs"></a> 创建和管理可用性组  
  
|Cmdlet|说明|支持平台|  
|-------------|-----------------|------------------|  
|[**New-SqlAvailabilityGroup**](/powershell/module/sqlserver/new-sqlavailabilitygroup)|创建新的可用性组。|承载主副本的服务器实例|  
|[**Remove-SqlAvailabilityGroup**](/powershell/module/sqlserver/remove-sqlavailabilitygroup)|删除可用性组。|启用 HADR 的服务器实例|  
|[**Set-SqlAvailabilityGroup**](/powershell/module/sqlserver/set-sqlavailabilitygroup)|设置可用性组的属性；使可用性组联机/脱机|承载主副本的服务器实例|  
|[**Switch-SqlAvailabilityGroup**](/powershell/module/sqlserver/switch-sqlavailabilitygroup)|启动下列形式之一的故障转移：<br /><br /> 可用性组的强制故障转移（可能会丢失数据）。<br /><br /> 可用性组的手动故障转移。|承载目标辅助副本的服务器实例|  
  
##  <a name="creating-and-managing-an-availability-group-listener"></a><a name="AGlisteners"></a> 创建和管理可用性组侦听器  
  
|Cmdlet|说明|支持平台|  
|------------|-----------------|------------------|  
|[**New-SqlAvailabilityGroupListener**](/powershell/module/sqlserver/new-sqlavailabilitygrouplistener)|创建一个新的可用性组侦听器，并将其附加到一个现有可用性组。|承载主副本的服务器实例|  
|[**Set-SqlAvailabilityGroupListener**](/powershell/module/sqlserver/set-sqlavailabilitygrouplistener)|修改现有可用性组侦听器的端口设置。|承载主副本的服务器实例|  
|[**Add-SqlAvailabilityGroupListenerStaticIp**](/powershell/module/sqlserver/add-sqlavailabilitygrouplistenerstaticip)|将一个静态 IP 地址添加到现有的可用性组侦听器配置。 此 IP 地址可以是带子网的 IPv4 地址或 IPv6 地址。|承载主副本的服务器实例|  
  
##  <a name="creating-and-managing-an-availability-replica"></a><a name="DeployManageARs"></a> 创建和管理可用性副本  
  
|Cmdlet|说明|支持平台|  
|-------------|-----------------|------------------|  
|[**New-SqlAvailabilityReplica**](/powershell/module/sqlserver/new-sqlavailabilityreplica)|创建新的可用性副本。 你可以使用 **-AsTemplate** 参数为每个新的可用性副本创建内存中的可用性副本对象。|承载主副本的服务器实例|  
|[**Join-SqlAvailabilityGroup**](/powershell/module/sqlserver/join-sqlavailabilitygroup)|将辅助副本联接到该可用性组。|承载辅助副本的服务器实例|  
|[**Remove-SqlAvailabilityReplica**](/powershell/module/sqlserver/remove-sqlavailabilityreplica)|删除可用性副本。|承载主副本的服务器实例|  
|[**Set-SqlAvailabilityReplica**](/powershell/module/sqlserver/set-sqlavailabilityreplica)|设置可用性副本的属性。|承载主副本的服务器实例|  
  
##  <a name="adding-and-managing-an-availability-database"></a><a name="DeployManageDbs"></a> 添加和管理可用性数据库  
  
|Cmdlet|说明|支持平台|  
|-------------|-----------------|------------------|  
|[**Add-SqlAvailabilityDatabase**](/powershell/module/sqlserver/add-sqlavailabilitydatabase)|在主副本上，将数据库添加到可用性组。<br /><br /> 在辅助副本上，将辅助数据库联接到可用性组。|承载可用性副本的任何服务器实例（主副本与辅助副本的行为不同）|  
|[**Remove-SqlAvailabilityDatabase**](/powershell/module/sqlserver/remove-sqlavailabilitydatabase)|在主副本上，从可用性组中删除数据库。<br /><br /> 在辅助副本上，从本地辅助副本中删除本地辅助数据库。|承载可用性副本的任何服务器实例（主副本与辅助副本的行为不同）|  
|[**Resume-SqlAvailabilityDatabase**](/powershell/module/sqlserver/resume-sqlavailabilitydatabase)|恢复已挂起的可用性数据库的数据移动。|已挂起数据库所在的服务器实例。|  
|[**Suspend-SqlAvailabilityDatabase**](/powershell/module/sqlserver/suspend-sqlavailabilitydatabase)|挂起可用性数据库的数据移动。|承载可用性副本的任何服务器实例。|  
  
##  <a name="monitoring-availability-group-health"></a><a name="MonitorTblshtAGs"></a> 监视可用性组运行状况  
 以下 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet 支持您监视可用性组及其副本和数据库的运行状况。  
  
> [!IMPORTANT]  
>  您必须具有 CONNECT、VIEW SERVER STATE 和 VIEW ANY DEFINITION 权限才能执行这些 cmdlet。  
  
|Cmdlet|说明|支持平台|  
|------------|-----------------|------------------|  
|[**Test-SqlAvailabilityGroup**](/powershell/module/sqlserver/test-sqlavailabilitygroup)|通过评估 SQL Server 基于策略的管理 (PBM) 策略来评估可用性组的运行状况。|承载可用性副本的任何服务器实例。*|  
|[**Test-SqlAvailabilityReplica**](/powershell/module/sqlserver/test-sqlavailabilityreplica)|通过评估 SQL Server 基于策略的管理 (PBM) 策略来评估可用性副本的运行状况。|承载可用性副本的任何服务器实例。*|  
|[**Test-SqlDatabaseReplicaState**](/powershell/module/sqlserver/test-sqldatabasereplicastate)|通过评估 SQL Server 基于策略的管理 (PBM) 策略来评估所有联接的可用性副本的可用性数据库的运行状况。|承载可用性副本的任何服务器实例。*|  
  
 *若要查看有关可用性组中所有可用性副本的信息，请使用承载主副本的服务器实例。  
  
 有关详细信息，请参阅[使用 AlwaysOn 策略查看可用性组的运行状况 (SQL Server)](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
