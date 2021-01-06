---
title: 将数据库添加到可用性组
description: '使用 Transact-SQL (T-SQL)、PowerShell 或 SQL Server Management Studio，将数据库添加到 AlwaysOn 可用性组。 '
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: 2a54eef8-9e8e-4e04-909c-6970112d55cc
author: cawrites
ms.author: chadam
ms.openlocfilehash: f6af03aebbc04ce45f2dad51025bdce32a1443b5
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641207"
---
# <a name="add-a-database-to-an-always-on-availability-group"></a>将数据库添加到 AlwaysOn 可用性组
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何通过在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]中使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)]、 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]或 PowerShell 将数据库添加到 AlwaysOn 可用性组中。  
  

  
## <a name="prerequisites-and-restrictions"></a>先决条件和限制  
  
-   您必须连接到承载主副本的服务器实例。  
  
-   数据库必须位于承载主副本的服务器实例上并符合可用性数据库的先决条件和限制。 有关详细信息，请参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)配置服务器实例时遇到的典型问题。  
  
 
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio  

  
1.  在对象资源管理器中，连接到承载主副本的服务器实例，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  右键单击可用性组，然后选择下列命令之一：  
  
    -   若要启动“将数据库添加到可用性组向导”，请选择 **“添加数据库”** 命令。 有关详细信息，请参阅[使用“将数据库添加到可用性组向导”(SQL Server Management Studio)](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)。  
  
    -   若要通过在 **“可用性组属性”** 对话框中指定一个或多个数据库来进行添加，则选择 **“属性”** 命令。 添加数据库的步骤如下所示：  
  
        1.  在 **“可用性数据库”** 窗格中，单击 **“添加”** 按钮。 这将创建并选择一个空数据库字段。  
  
        2.  输入符合可用性数据库先决条件的数据库的名称。  
  
         若要添加其他数据库，请重复前面的步骤。 当您完成指定数据库后，请单击 **“确定”** 以完成此操作。  
  
         在您使用 **“可用性组属性”** 对话框将数据库添加到可用性组后，需要在承载辅助副本的每个服务器实例上配置相应的辅助数据库。 有关详细信息，请参阅本主题后面的 [启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)。  
  
##  <a name="use-transact-sql"></a><a name="TsqlProcedure"></a>使用 Transact-SQL  

  
1.  连接到承载主副本的服务器实例。    
2.  按如下所示使用 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) 语句：  
  
     ALTER AVAILABILITY GROUP *group_name* ADD DATABASE *database_name* [,...*n*]  
  
     其中 *group_name* 是可用性组的名称， *database_name* 是要添加到该组的数据库的名称。  
  
     以下示例添加 *MyDb3* 数据库到 *MyAG* 可用性组。  
  
    ```  
    -- Connect to the server instance that hosts the primary replica.  
    -- Add an existing database to the availability group.  
    ALTER AVAILABILITY GROUP MyAG ADD DATABASE MyDb3;  
    GO  
    ```  
  
3.  在您将数据库添加到可用性组后，需要在承载辅助副本的每个服务器实例上配置相应的辅助数据库。 有关详细信息，请参阅本主题后面的 [启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)。  
  
##  <a name="use-powershell"></a><a name="PowerShellProcedure"></a>使用 PowerShell  

  
1.  将目录 (**cd**) 更改为托管主副本的服务器实例。  
  
2.  使用 **Add-SqlAvailabilityDatabase** cmdlet。  
  
     例如，以下命令将添加辅助数据库 `MyDd` 到 `MyAG` 可用性组中，其主副本由 `PrimaryServer\InstanceName`承载。  
  
    ```  
  
    Add-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAG `   
    -Database "MyDb"  
    ```  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
3.  在您将数据库添加到可用性组后，需要在承载辅助副本的每个服务器实例上配置相应的辅助数据库。 有关详细信息，请参阅本主题后面的 [启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
 有关完整示例，请参阅下面的 [示例 (PowerShell)](#PSExample)。  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> 示例 (PowerShell)  
 下面的示例说明了一个完整过程：从承载可用性组主副本的服务器实例上的一个数据库中准备一个辅助数据库，将此数据库添加到可用性组（作为主数据库），然后将此辅助数据库加入可用性组。 首先，该示例备份数据库及其事务日志。 然后，此示例将数据库和日志备份还原到承载辅助副本的服务器实例。  
  
 此示例调用两次 **Add-SqlAvailabilityDatabase** ：第一次是针对主要副本调用，以便将数据库添加到可用性组；第二次是针对次要副本调用，以便将该副本上的辅助数据库加入到可用性组。 如果您有多个辅助副本，则对其中每个副本还原和加入辅助数据库。  
  
```  
$DatabaseBackupFile = "\\share\backups\MyDatabase.bak"  
$LogBackupFile = "\\share\backups\MyDatabase.trn"  
$MyAgPrimaryPath = "SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg"  
$MyAgSecondaryPath = "SQLSERVER:\SQL\SecondaryServer\InstanceName\AvailabilityGroups\MyAg"  
  
Backup-SqlDatabase -Database "MyDatabase" -BackupFile $DatabaseBackupFile -ServerInstance "PrimaryServer\InstanceName"  
Backup-SqlDatabase -Database "MyDatabase" -BackupFile $LogBackupFile -ServerInstance "PrimaryServer\InstanceName" -BackupAction 'Log'  
  
Restore-SqlDatabase -Database "MyDatabase" -BackupFile $DatabaseBackupFile -ServerInstance "SecondaryServer\InstanceName" -NoRecovery  
Restore-SqlDatabase -Database "MyDatabase" -BackupFile $LogBackupFile -ServerInstance "SecondaryServer\InstanceName" -RestoreAction 'Log' -NoRecovery  
  
Add-SqlAvailabilityDatabase -Path $MyAgPrimaryPath -Database "MyDatabase"  
Add-SqlAvailabilityDatabase -Path $MyAgSecondaryPath -Database "MyDatabase"  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [创建和配置可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [使用 AlwaysOn 仪表板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)   
 [监视可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)