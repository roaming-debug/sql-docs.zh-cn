---
title: 从可用性组中删除数据库
description: 使用 Transact-SQL (T-SQL)、PowerShell 或 SQL Server Management Studio 从 Always On 可用性组中删除主数据库的步骤。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.removeprimarydb.f1
helpviewer_keywords:
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], removing
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: 6d4ca31e-ddf0-44bf-be5e-a5da060bf096
author: cawrites
ms.author: chadam
ms.openlocfilehash: ca813d490f97bf8e501ada801599cef74e2b9c05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351029"
---
# <a name="remove-a-primary-database-from-an-always-on-availability-group"></a>从 Always On 可用性组中删除主数据库
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何通过使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]、 [!INCLUDE[tsql](../../../includes/tsql-md.md)]或 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]中的 PowerShell 从 Always On 可用性组中删除主数据库和对应的辅助数据库。  
  
##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> 先决条件和限制  
  
-   只有主副本支持该任务。 您必须连接到承载主副本的服务器实例。  
  
 
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **删除可用性数据库**  
  
1.  在对象资源管理器中，连接到承载要删除的一个或多个数据库的主副本的服务器实例，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  选择可用性组，然后展开 **“可用性数据库”** 节点。  
  
4.  此步骤取决于您是要删除多个数据库组，还是只删除一个数据库，如下所示：  
  
    -   若要删除多个数据库，请使用 **“对象资源管理器详细信息”** 窗格查看并选择所有要删除的数据库。 有关详细信息，请参阅[使用对象资源管理器详细信息监视可用性组 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)。  
  
    -   若要删除单个数据库，请在 **“对象资源管理器”** 窗格或 **“对象资源管理器详细信息”** 窗格中选中该数据库。  
  
5.  右键单击选定的一个或多个数据库，然后在命令菜单中选择“从可用性组中删除数据库”  。  
  
6.  在 **“从可用性组中删除数据库”** 对话框中，删除所有列出的数据库，然后单击 **“确定”** 。 如果您不想全部删除这些数据库，请单击 **“取消”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **删除可用性数据库**  
  
1.  连接到承载主副本的服务器实例。  
  
2.  按如下所示使用 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) 语句：  
  
     ALTER AVAILABILITY GROUP *group_name* REMOVE DATABASE *availability_database_name*  
  
     其中， *group_name* 是可用性组的名称， *database_name* 是要删除的数据库的名称。  
  
     下面的示例将从 `Db6` 可用性组中删除名为 `MyAG` 的数据库。  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG REMOVE DATABASE Db6;  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **删除可用性数据库**  
  
1.  将目录 (**cd**) 更改为托管主副本的服务器实例。  
  
2.  使用 **Remove-SqlAvailabilityDatabase** cmdlet，指定要从可用性组中删除的可用性数据库的名称。 当您连接到承载主副本的服务器实例时，主数据库及其对应的辅助数据库将从可用性组中全部删除。  
  
     例如，下面的命令从名为 `MyDb9` 的可用性组中删除可用性数据库 `MyAg`。 因为此命令在承载主副本的服务器实例上执行，所以，主数据库及其对应的所有辅助数据库都将从可用性组中删除。 在任何辅助副本上都不会出现针对此数据库的数据同步。  
  
    ```  
    Remove-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\Sql\PrimaryComputer\InstanceName\AvailabilityGroups\MyAg\AvailabilityDatabases\MyDb9
    ```  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-removing-an-availability-database-from-an-availability-group"></a><a name="FollowUp"></a> 跟进：在从可用性组中删除可用性数据库之后  
 从其可用性组中删除可用性数据库后，将结束先前主数据库与对应的辅助数据库之间的数据同步。 以前的主数据库保持联机状态。 每个对应的辅助数据库都处于 RESTORING 状态。  
  
 此时，可以通过多种备选方法处理删除的辅助数据库：  
  
-   如果不再需要给定的辅助数据库，则可以将其删除。  
  
     有关详细信息，请参阅 [删除数据库](../../../relational-databases/databases/delete-a-database.md)。  
  
-   如果当辅助数据库已从可用性组中删除后要访问它，则可以恢复此数据库。 但是，如果恢复删除的辅助数据库，则会有两个同名的、独立但不同的数据库处于联机状态。 您必须确保客户端仅可访问其中一个数据库，通常为最新的主数据库。  
  
     有关详细信息，请参阅[恢复数据库而不还原数据 (Transact-SQL)](../../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [将辅助数据库从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
