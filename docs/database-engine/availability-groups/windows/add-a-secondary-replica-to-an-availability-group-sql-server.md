---
title: 将次要副本添加到可用性组
description: 了解如何使用 Transact-SQL (T-SQL)、PowerShell 或 SQL Server Management Studio (SSMS) 中的“可用性组向导”将次要副本添加到 AlwaysOn 可用性组。
ms.custom: seodec18
ms.date: 05/18/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], configuring
ms.assetid: 6669dcce-85f9-495f-aadf-7f62cff4a9da
author: cawrites
ms.author: chadam
ms.openlocfilehash: 53d138f5273ef07174d1e926fe628f9e73146297
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584869"
---
# <a name="add-a-secondary-replica-to-an-always-on-availability-group"></a>将次要副本添加到 AlwaysOn 可用性组
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题描述了如何使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]、 [!INCLUDE[tsql](../../../includes/tsql-md.md)]或 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]中的 PowerShell 将次要副本添加到现有的 AlwaysOn 可用性组。  

  
##  <a name="prerequisites-and-restrictions"></a><a name="PrerequisitesRestrictions"></a> 先决条件和限制  
  
-   您必须连接到承载主副本的服务器实例。  
  
 有关详细信息，请参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)配置服务器实例时遇到的典型问题。  

##  <a name="security"></a><a name="Security"></a> Security  
  
###  <a name="permissions"></a><a name="Permissions"></a> 权限  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  

[!INCLUDE[Freshness](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **添加副本**  
  
1.  在对象资源管理器中，连接到承载主副本的服务器实例，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  右键单击可用性组，然后选择下列命令之一：  
  
    -   选择 **“添加副本”** 命令可启动“将副本添加到可用性组向导”。 有关详细信息，请参阅[使用“将副本添加到可用性组向导”(SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)。  
  
    -   或者，选择 **“属性”** 命令以便打开 **“可用性组属性”** 对话框。 用于在此对话框中添加副本的步骤如下所示：  
  
        1.  在对话框的 **“可用性副本”** 窗格中，单击 **“添加”** 按钮。 这将创建并选择已选中了空白“服务器实例”字段的副本项。  
  
        2.  输入符合用于承载可用性副本的先决条件的服务器实例的名称。  
  
         若要添加其他副本，请重复前面的步骤。 当您指定完副本后，单击 **“确定”** 以完成此操作。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **添加副本**  
  
1.  连接到承载主副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。  
  
2.  通过使用 ALTER AVAILABILITY GROUP 语句的 ADD REPLICA ON 子句将新辅助副本添加到可用性组。 在 ADD REPLICA ON 子句中，ENDPOINT_URL、AVAILABILITY_MODE 和 FAILOVER_MODE 选项是必需的。 其他副本选项（BACKUP_PRIORITY、SECONDARY_ROLE、PRIMARY_ROLE 和 SESSION_TIMEOUT）是可选的。 有关详细信息，请参阅 [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)中的 PowerShell 将次要副本添加到现有的 AlwaysOn 可用性组。  
  
     例如，下面的 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句将在 `MyAG` 承载的默认服务器实例（其端点 URL 为 `COMPUTER04`）上创建名为 `TCP://COMPUTER04.Adventure-Works.com:5022'`的可用性组的新副本。 此副本支持手动故障转移和异步提交可用性模式。  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG ADD REPLICA ON 'COMPUTER04'   
       WITH (  
             ENDPOINT_URL = 'TCP://COMPUTER04.Adventure-Works.com:5022',  
             AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
             FAILOVER_MODE = MANUAL  
             );  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **添加副本**  
  
1.  将目录 (**cd**) 更改为托管主副本的服务器实例。  
  
2.  使用 **New-SqlAvailabilityReplica** cmdlet。  
  
     例如，下面的命令将可用性副本添加到名为 `MyAg`的现有可用性组中。 此副本支持手动故障转移和异步提交可用性模式。 在辅助角色中，此副本将支持读访问连接，使您可以将只读处理转移到此副本。  
  
    ```  
    $agPath = "SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg"  
    $endpointURL = "TCP://PrimaryServerName.domain.com:5022"  
    $failoverMode = "Manual"  
    $availabilityMode = "AsynchronousCommit"  
    $secondaryReadMode = "AllowAllConnections"  
  
    New-SqlAvailabilityReplica -Name SecondaryServer\Instance `   
    -EndpointUrl $endpointURL `   
    -FailoverMode $failoverMode `   
    -AvailabilityMode $availabilityMode `   
    -ConnectionModeInSecondaryRole $secondaryReadMode `   
    -Path $agPath  
    ```  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-adding-a-secondary-replica"></a><a name="FollowUp"></a> 跟进：在添加辅助副本之后  
 若要为现有可用性组添加副本，您必须执行以下步骤：  
  
1.  连接到将要承载新辅助副本的服务器实例。  
  
2.  将新的辅助副本联接到可用性组。 有关详细信息，请参阅 [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
  
3.  对于可用性组中的每个数据库，在承载辅助副本的服务器实例上创建辅助数据库。 有关详细信息，请参阅 [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
  
4.  将每个新的辅助数据库联接到可用性组。 有关详细信息，请参阅 [将辅助数据库联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **管理可用性副本**  
  
-   [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [将次要副本从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [配置对可用性副本的只读访问 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [更改可用性副本的可用性模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [更改可用性副本的故障转移模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [更改可用性副本的会话超时期限 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
-   [更改可用性副本的会话超时期限 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [创建和配置可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [使用 AlwaysOn 仪表板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)   
 [监视可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
