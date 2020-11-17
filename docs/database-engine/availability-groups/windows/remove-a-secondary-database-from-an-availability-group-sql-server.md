---
title: 从可用性组中删除辅助数据库
description: 使用 Transact-SQL (T-SQL)、PowerShell 或 SQL Server Management Studio 从 AlwaysOn 可用性组中删除辅助数据库的步骤。
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.unjoindb.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- Availability Groups [SQL Server], removing
- Availability Groups [SQL Server], databases
ms.assetid: 4e51a570-58d7-4f01-9390-4198f3602576
author: cawrites
ms.author: chadam
ms.openlocfilehash: 60ce75bd240c8658bbf3a8174064c017456f6225
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584004"
---
# <a name="remove-a-secondary-database-from-an-availability-group-sql-server"></a>从可用性组中删除辅助数据库 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何通过在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]中使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)]、 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]或 PowerShell 从 AlwaysOn 可用性组中删除辅助数据库。  
   
  
##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> 先决条件和限制  
  
-   只有辅助副本支持该任务。 您必须连接到承载要从中删除数据库的辅助副本的服务器实例。  
  
 
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **从可用性组中删除辅助数据库**  
  
1.  在对象资源管理器中，连接到承载您要从中删除一个或多个辅助数据库的辅助副本的服务器实例，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  选择可用性组，然后展开 **“可用性数据库”** 节点。  
  
4.  此步骤取决于您是要删除多个数据库组，还是只删除一个数据库，如下所示：  
  
    -   若要删除多个数据库，请使用 **“对象资源管理器详细信息”** 窗格查看并选择所有要删除的数据库。 有关详细信息，请参阅[使用对象资源管理器详细信息监视可用性组 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)。  
  
    -   若要删除单个数据库，请在 **“对象资源管理器”** 窗格或 **“对象资源管理器详细信息”** 窗格中选中该数据库。  
  
5.  右键单击选定的一个或多个数据库，然后在命令菜单中选择“删除辅助数据库”  。  
  
6.  在 **“从可用性组删除数据库”** 对话框中，要删除所有列出的数据库，则单击 **“确定”** 。 如果您不想删除所有列出的数据库，请单击 **“取消”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **从可用性组中删除辅助数据库**  
  
1.  连接到承载辅助副本的服务器实例。  
  
2.  使用 [ALTER DATABASE 语句的 SET HADR 子句](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) ，如下所述：  
  
     ALTER DATABASE *database_name* SET HADR OFF  
  
     其中， *database_name* 为要从其所属的可用性组中删除的辅助数据库的名称。  
  
     下面的示例将本地辅助数据库 *MyDb2* 从其可用性组中删除。  
  
    ```  
    ALTER DATABASE MyDb2 SET HADR OFF;  
    GO  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **从可用性组中删除辅助数据库**  
  
1.  将目录 (**cd**) 更改为托管辅助副本的服务器实例。  
  
2.  使用 **Remove-SqlAvailabilityDatabase** cmdlet，指定要从可用性组中删除的可用性数据库的名称。 当您连接到承载辅助副本的服务器实例时，只能从可用性组中删除本地辅助数据库。  
  
     例如，下面的命令从名为 `MyDb8` 的服务器实例承载的次要副本中删除辅助数据库 `SecondaryComputer\Instance`。 与已删除的辅助数据库的数据同步将停止。 此命令将不会影响主数据库或任何其他辅助数据库。  
  
    ```  
    Remove-SqlAvailabilityDatabase `  
    -Path SQLSERVER:\Sql\SecondaryComputer\InstanceName\AvailabilityGroups\MyAg\Databases\MyDb8  
    ```  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-removing-a-secondary-database-from-an-availability-group"></a><a name="FollowUp"></a> 跟进：从可用性组中删除辅助数据库之后  
 删除辅助数据库之后，它不再加入到可用性组中，有关删除的辅助数据库的所有信息都会被可用性组丢弃。 删除的辅助数据库处于 RESTORING 状态。  
  
> [!TIP]  
>  在删除辅助数据库后的较短时间中，你可能能够通过将其重新联接到可用性组，在数据库上重新启动 AlwaysOn 数据同步。 有关详细信息，请参阅 [将辅助数据库联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)。  
  
 此时，可以通过多种备选方法处理删除的辅助数据库：  
  
-   如果不再需要该辅助数据库，则可以将其删除。  
  
     有关详细信息，请参阅 [DROP DATABASE (Transact SQL)](../../../t-sql/statements/drop-database-transact-sql.md) 或[删除数据库](../../../relational-databases/databases/delete-a-database.md)。  
  
-   如果当辅助数据库已从可用性组中删除后要访问它，则可以恢复此数据库。 但是，如果恢复删除的辅助数据库，则会有两个同名的、独立但不同的数据库处于联机状态。 您必须确保客户端仅可访问当前主数据库。  
  
     有关详细信息，请参阅[恢复数据库而不还原数据 (Transact-SQL)](../../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [将主数据库从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
