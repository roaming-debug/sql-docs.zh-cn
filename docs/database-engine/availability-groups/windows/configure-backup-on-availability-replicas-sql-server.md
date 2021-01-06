---
title: 配置可用性组的次要副本备份
description: 介绍如何使用 Transact-SQL (T-SQL)、PowerShell 或 SQL Server Management Studio 配置 AlwaysOn 可用性组的次要副本备份。
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- backup priority
- backup on secondary replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], backup on secondary replicas
- active secondary replicas [SQL Server], backup on
- automated backup preference
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 74bc40bb-9f57-44e4-8988-1d69c0585eb6
author: cawrites
ms.author: chadam
ms.openlocfilehash: f274f22e2ba9e9bdffa2c2446b527250ea175807
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643046"
---
# <a name="configure-backups-on-secondary-replicas-of-an-always-on-availability-group"></a>配置 AlwaysOn 可用性组的次要副本备份
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何通过在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]中使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)]、 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]或 PowerShell 配置 AlwaysOn 可用性组的次要副本的备份。  
  
> [!NOTE]  
>  有关辅助副本备份的介绍，请参阅 [活动辅助副本：辅助副本备份（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)或 PowerShell 配置 AlwaysOn 可用性组的次要副本的备份。  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 您必须连接到承载主副本的服务器实例。  
 
   > [!NOTE]
   > 无需读取次要副本即可将备份卸载到该副本。 即使将 `Readable Secondary` 设置为 `no`，仍可在次要副本上成功进行备份。 
  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
  
|任务|权限|  
|----------|-----------------|  
|创建可用性组时配置辅助副本备份|需要 **sysadmin** 固定服务器角色的成员资格，以及 CREATE AVAILABILITY GROUP 服务器权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。|  
|修改可用性组或可用性副本|对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。|  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **配置辅助副本备份**  
  
1.  在对象资源管理器中，连接到承载主副本的服务器实例，然后单击服务器名称以便展开服务器树。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  单击要为其配置备份首选项的可用性组，然后选择 **“属性”** 命令。  
  
4.  在 **“可用性组属性”** 对话框中，选择 **“备份首选项”** 页。  
  
5.  在 **“应在何处进行备份?”** 面板中，选择可用性组的下列自动备份首选项之一：  
  
     **优先辅助**  
     指定备份应在辅助副本上发生，但在主副本是唯一联机的副本时除外。 在该情况下，备份应在主副本上发生。 这是默认选项。  
  
     **仅辅助**  
     指定备份应该永远不会在主副本上执行。 如果主副本是唯一的联机副本，则备份应不会发生。  
  
     **主要节点**  
     指定备份应该始终在主副本上发生。 如果您需要在对辅助副本运行备份时不支持的备份功能，例如创建差异备份，此选项将很有用。  
  
    > [!IMPORTANT]  
    >  如果你计划使用日志传送为可用性组准备任何辅助数据库，请将自动备份首选项设置为“主要”  ，直到准备好所有辅助数据库并将其联接到可用性组。  
  
     **任何副本**  
     指定您希望在选择要执行备份的副本时备份作业将忽略可用性副本的角色。 请注意，备份作业可能评估其他因素，例如每个可用性副本的备份优先级及其操作状态和已连接状态。  
  
    > [!IMPORTANT]  
    >  没有强制的自动备份首选项设置。 对此首选项的解释取决于您为给定可用性组中的数据库撰写备份作业脚本的逻辑（如果有）。 自动备份首选项设置对即席备份没有影响。 有关详细信息，请参阅本主题后面的 [跟进：配置辅助副本备份之后](#FollowUp) 。  
  
6.  使用 **“副本备份优先级”** 网格更改可用性副本的备份优先级。 此网格将显示每个承载可用性组的副本的服务器实例的当前备份优先级。 网格列如下所示：  
  
     **服务器实例**  
     承载可用性副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。  
  
     **备份优先级(最低 = 1，最高 = 100)**  
     指定相对于同一可用性组中的其他副本，在此副本上执行备份的优先级。 该值是范围 0..100 中的整数。 1 表示最低优先级，100 表示最高优先级。 如果“备份优先级”  = 1，则仅在当前没有更高优先级的可用性副本可用时，才选择此可用性副本来执行备份。  
  
     **排除副本**  
     如果从不希望选择此可用性副本来执行备份，请选择此选项。 例如，这对于您永远不希望备份故障转移到的远程可用性副本十分有用。  
  
7.  若要提交更改，请单击 **“确定”** 。  
  
 **用于访问“备份首选项”页的其他方法**  
  
-   [使用“新建可用性组”对话框 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [使用“将副本添加到可用性组向导”(SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [使用“新建可用性组”对话框 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **配置辅助副本备份**  
  
1.  连接到承载主副本的服务器实例。  
  
2.  对于新的可用性组，请使用 [CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md) 语句。 如果要修改现有可用性组，请使用 [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md) 语句。  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **配置辅助副本备份**  
  
1.  将默认值 (**cd**) 设置为托管主副本的服务器实例。  
  
2.  （可选）配置要添加或修改的每个可用性副本的备份优先级。 此优先级由承载主副本的服务器实例用来确定哪一副本应该用于针对可用性组中某一数据库的自动备份请求（选择具有最高优先级的副本）。 该优先级可以是 0 和 100 之间（含 0 和 100）的任何数字。 优先级 0 指示副本不应视作支持备份请求的候选。  默认设置为 50。  
  
     在将可用性副本添加到可用性组中时，使用 **New-SqlAvailabilityReplica** cmdlet。 在修改现有可用性副本时，使用 **Set-SqlAvailabilityReplica** cmdlet。 在任一情况下，指定 **BackupPriority**_n_ 参数，其中 *n* 是一个介于 0 和 100 之间的值。  
  
     例如，以下命令会将可用性副本 `MyReplica` 的备份优先级设置为 **60**。  
  
    ```  
    Set-SqlAvailabilityReplica -BackupPriority 60 `  
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
3.  （可选）配置要创建或修改的可用性组的自动备份首选项。 此首选项指示在选择执行备份的位置时备份作业应该如何评估主副本。 默认设置是首选辅助副本。  
  
     在创建可用性组时，使用 **New-SqlAvailabilityGroup** cmdlet。 在修改现有可用性组时，使用 **Set-SqlAvailabilityGroup** cmdlet。 在任一情况下，指定 **AutomatedBackupPreference** 参数。  
  
     其中：  
  
     **主要节点**  
     指定备份应该始终在主副本上发生。 如果您需要在对辅助副本运行备份时不支持的备份功能，例如创建差异备份，此选项将很有用。  
  
    > [!IMPORTANT]  
    >  如果你计划使用日志传送为可用性组准备任何辅助数据库，请将自动备份首选项设置为“主要”  ，直到准备好所有辅助数据库并将其联接到可用性组。  
  
     **SecondaryOnly**  
     指定备份应该永远不会在主副本上执行。 如果主副本是唯一的联机副本，则备份应不会发生。  
  
     **辅助节点**  
     指定备份应在辅助副本上发生，但在主副本是唯一联机的副本时除外。 在该情况下，备份应在主副本上发生。 此选项为默认行为。  
  
     无   
     指定您希望在选择要执行备份的副本时备份作业将忽略可用性副本的角色。 请注意，备份作业可能评估其他因素，例如每个可用性副本的备份优先级及其操作状态和已连接状态。  
  
    > [!IMPORTANT]  
    >  没有强制的 **AutomatedBackupPreference**。 对此首选项的解释取决于您为给定可用性组中的数据库撰写备份作业脚本的逻辑（如果有）。 自动备份首选项设置对即席备份没有影响。 有关详细信息，请参阅本主题后面的 [跟进：配置辅助副本备份之后](#FollowUp) 。  
  
     例如，以下命令将可用性组 **上的** AutomatedBackupPreference `MyAg` 属性设置为 **SecondaryOnly**。 此可用性组中的数据库自动备份将永远不会在主副本上发生，但将重定向到具有最高备份优先级设置的辅助副本。  
  
    ```  
    Set-SqlAvailabilityGroup `  
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg `  
    -AutomatedBackupPreference SecondaryOnly  
    ```  
  
> [!NOTE]  
>  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
##  <a name="follow-up-after-configuring-backup-on-secondary-replicas"></a><a name="FollowUp"></a> 跟进：配置辅助副本备份之后  
 若要为某一给定可用性组考虑使用自动备份首选项，则对于承载备份优先级大于零 (>0) 的可用性副本的每个服务器实例，您需要为该可用性组中的数据库的备份作业编写脚本。 若要确定当前副本是否为首选备份副本，请在备份脚本中使用 [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md) 函数。 如果当前服务器实例承载的可用性副本是用于备份的首选副本，则此函数将返回 1。 否则，该函数返回 0。 通过对查询此函数的每个可用性副本运行简单的脚本，可以确定哪个副本应运行给定的备份作业。 例如，备份作业脚本的典型代码段如下所示：  
  
```sql  
IF (NOT sys.fn_hadr_backup_is_preferred_replica(@DBNAME))  
BEGIN  
      Select 'This is not the preferred replica, exiting with success';  
      RETURN 0 -- This is a normal, expected condition, so the script returns success  
END  
BACKUP DATABASE @DBNAME TO DISK=<disk>  
   WITH COPY_ONLY;  
```  
  
 通过使用此逻辑编写备份作业脚本，可以在同一个计划中安排对每个可用性副本运行的作业。 上述每个作业都应该查看相同数据以便确定哪一作业应该运行，因此，实际上只有一个计划作业将前进到备份阶段。  在发生故障转移时，无需修改任何脚本或作业。 此外，如果重新配置可用性组以便添加可用性副本，则管理备份作业只需复制或计划备份作业即可。 如果您删除可用性副本，只需从承载该副本的服务器实例上删除备份作业即可。  
  
> [!TIP]  
>  如果使用 [维护计划向导](../../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)创建某个给定的备份作业，该作业将自动包括调用和校验 **sys.fn_hadr_backup_is_preferred_replica** 函数的脚本逻辑。 但是，备份作业不会返回“这不是首选副本…”消息。请确保在托管可用性组的可用性副本的每个服务器实例上为每个可用性数据库创建作业。  
  
##  <a name="to-obtain-information-about-backup-preference-settings"></a><a name="ForInfoAboutBuPref"></a> 获取有关备份首选项设置的信息  
 以下内容对于获取辅助副本备份的相关信息很有用。  
  
|查看|信息|相关列|  
|----------|-----------------|----------------------|  
|[sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)|当前副本是否为首选备份副本？|不适用。|  
|[sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)|自动备份首选项|**automated_backup_preference**<br /><br /> **automated_backup_preference_desc**|  
|[sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)|给定可用性副本的备份优先级|**backup_priority**|  
|[sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)|是否为服务器实例的本地副本？<br /><br /> 当前角色<br /><br /> 操作状态<br /><br /> 连接状态<br /><br /> 可用性副本的同步运行状况|**is_local**<br /><br /> **role**、 **role_desc**<br /><br /> **operational_state**、 **operational_state_desc**<br /><br /> **connected_state**、 **connected_state_desc**<br /><br /> **synchronization_health**、 **synchronization_health_desc**|  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [用于高可用性和灾难恢复的 Microsoft SQL Server AlwaysOn 解决方案指南](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [SQL Server AlwaysOn 团队博客：SQL Server AlwaysOn 团队官方博客](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [活动次要副本：次要副本备份（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)  
  
