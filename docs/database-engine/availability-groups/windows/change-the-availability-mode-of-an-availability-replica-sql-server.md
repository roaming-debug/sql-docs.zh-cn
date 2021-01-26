---
title: 更改可用性组副本的可用性模式
description: 有关如何使用 Transact-SQL (T-SQL)、PowerShell 或 SQL Server Management Studio 更改 Always On 可用性组中可用性副本的可用性模式的说明。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], availability modes
ms.assetid: c4da8f25-fb1b-45a4-8bf2-195df6df634c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 72ee8486291574631a8683334664e97a927b93e2
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766010"
---
# <a name="change-availability-mode-of-a-replica-within-an-always-on-availability-group"></a>更改 Always On 可用性组中副本的可用性模式
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何通过使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]或 PowerShell 来更改 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中 AlwaysOn 可用性组的可用性副本的可用性模式。 可用性模式是一个副本属性，用于控制以异步还是同步方式提交副本。 异步提交模式可使性能最大化，但要牺牲高可用性；它仅支持强制手动故障转移（有可能丢失数据），该故障转移一般称为 *强制故障转移*。 *同步提交模式* 更多地强调高可用性而不是性能，并且一旦同步次要副本，即支持手动故障转移（也可以支持自动故障转移）。  
    
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
您必须连接到承载主副本的服务器实例。  
  

##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **更改可用性组的可用性模式**  
  
1.  在对象资源管理器中，连接到承载主副本的服务器实例，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”节点和“可用性组”节点。  
  
3.  单击要更改其副本的可用性组。  
  
4.  右键单击该副本，然后单击“属性”。  
  
5.  在 **“可用性副本属性”** 对话框中，使用 **“可用性模式”** 下拉列表更改此副本的可用性模式。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **更改可用性组的可用性模式**  
  
1.  连接到承载主副本的服务器实例。  
  
2.  按如下所示使用 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) 语句：  
  
     ```sql
     ALTER AVAILABILITY GROUP *group_name* MODIFY REPLICA ON '*server_name*'  
     WITH ( AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT , FAILOVER_MODE = MANUAL );  
     ```
     
     其中，“group_name”为可用性组的名称，“server_name”为承载要修改的副本的服务器实例的名称。  
  
    > [!NOTE]  
    > 指定 `AVAILABILITY_MODE = SYNCHRONOUS_COMMIT` 时 `FAILOVER_MODE = AUTOMATIC` 才受支持。  
  
     在 `AccountsAG` 可用性组的主副本上输入的以下示例，针对 `INSTANCE09` 服务器实例承载的副本，将可用性模式和故障转移模式分别更改为同步提交和自动故障转移。  
  
    ```sql
    ALTER AVAILABILITY GROUP AccountsAG MODIFY REPLICA ON 'INSTANCE09'  
       WITH (AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);  
    ALTER AVAILABILITY GROUP AccountsAG MODIFY REPLICA ON 'INSTANCE09'  
       WITH (FAILOVER_MODE = AUTOMATIC);  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **更改可用性组的可用性模式**  
  
1.  将目录 (**cd**) 更改为托管主副本的服务器实例。  
  
2.  结合使用 **Set-SqlAvailabilityReplica** cmdlet 与 **AvailabilityMode** 参数和 **FailoverMode** 参数（可选）。  
  
     例如，以下命令将修改可用性组 `MyReplica` 中的副本 `MyAg` 以使用同步提交可用性模式和支持自动故障转移。  
  
    ```powershell  
    Set-SqlAvailabilityReplica -AvailabilityMode "SynchronousCommit" -FailoverMode "Automatic" `   
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
    > [!NOTE]  
    > 若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)  
  
