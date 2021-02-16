---
title: 将辅助数据库联接到可用性组
description: 使用 Transact-SQL (T-SQL)、PowerShell 或 SQL Server Management Studio 将辅助数据库加入 AlwaysOn 可用性组的步骤。
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.availabilitygroup.joindbs.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- secondary databases [SQL Server]
- Availability Groups [SQL Server], joining
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: fd7efe79-c1f9-497d-bfe7-b2a2b2321cf5
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9e1d98fc820215de35e11cb06843884968dd5b3a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348452"
---
# <a name="join-a-secondary-database-to-an-always-on-availability-group"></a>将辅助数据库联接到 AlwaysOn 可用性组
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何通过在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]中使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)]、 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]或 PowerShell 将辅助数据库联接到 Always On 可用性组。 在您为辅助副本准备了辅助数据库后，需要尽快将该数据库联接到可用性组。 这将启动从相应的主数据库到辅助数据库的数据移动。  
   
> [!NOTE]  
>  有关在辅助数据库联接到该组后所发生的情况的信息，请参阅 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
   
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   您必须连接到承载辅助副本的服务器实例。  
  
-   该辅助副本必须已联接到可用性组。 有关详细信息，请参阅 [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
  
-   辅助数据库必须已在最近进行了准备。 有关详细信息，请参阅 [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
  
###  <a name="permissions"></a><a name="Permissions"></a> 权限  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **将辅助数据库联接到可用性组**  
  
1.  在对象资源管理器中，连接到承载辅助副本的服务器实例，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  展开您要更改的可用性组，然后展开 **“可用性数据库”** 节点。  
  
4.  右键单击数据库，然后单击  “联接到可用性组”。  
  
5.  这将打开 **“将数据库联接到可用性组”** 对话框。 验证在标题栏上显示的可用性组名称以及在网格中显示的数据库名称，然后单击 **“确定”** 或单击 **“取消”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **将辅助数据库联接到可用性组**  
  
1.  连接到承载辅助副本的服务器实例。  
  
2.  使用 [ALTER DATABASE 语句的 SET HADR 子句](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) ，如下所述：  
  
     ALTER DATABASE *database_name* SET HADR AVAILABILITY GROUP = *group_name*  
  
     其中 *database_name* 是要联接的数据库名， *group_name* 是可用性组名。  
  
     下面的示例将辅助数据库 `Db1` 联接到 `MyAG` 可用性组的本地辅助副本。  
  
    ```  
    ALTER DATABASE Db1 SET HADR AVAILABILITY GROUP = MyAG;  
    ```  
  
    > [!NOTE]  
    >  若要查看此用于上下文的 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句，请参阅[创建可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)。  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **将辅助数据库联接到可用性组**  
  
1.  将目录 (**cd**) 更改为托管辅助副本的服务器实例。  
  
2.  使用 **Add-SqlAvailabilityDatabase** cmdlet 将一个或多个辅助数据库联接到可用性组。  
  
     例如，以下命令将辅助数据库 `Db1`联接到一个承载辅助副本的服务器实例上的可用性组 `MyAG` 。  
  
    ```  
    Add-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\SQL\SecondaryServer\InstanceName\AvailabilityGroups\MyAG `   
    -Database "Db1"  
    ```  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [AlwaysOn 可用性组配置故障排除 (SQL Server)](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
