---
title: 更改可用性组的副本故障转移模式
description: 介绍如何使用 Transact-SQL (T-SQL)、PowerShell 或 SQL Server Management Studio 更改 Always On 可用性组中副本的故障转移模式。
ms.custom: seo-lt-2019
ms.date: 06/30/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- failover modes [SQL Server]
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], failover modes
- Availability Groups [SQL Server], configuring
ms.assetid: 619a826f-8e65-48eb-8c34-39497d238279
author: cawrites
ms.author: chadam
ms.openlocfilehash: ffd564df011932a0b40c0eff2426049fc3f6c315
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584549"
---
# <a name="change-the-failover-mode-for-a-replica-within-an-always-on-availability-group"></a>更改 Always On 可用性组中副本的故障转移模式
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]或 PowerShell 更改 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中 Always On 可用性组的可用性副本的故障转移模式。 故障转移模式是一个副本属性，用于确定在同步提交可用性模式下运行的副本的故障转移模式。 有关详细信息，请参阅 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) 和 [可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)。  
  
## <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> 先决条件和限制  
  
-   只有主副本支持该任务。 您必须连接到承载主副本的服务器实例。  
  
-   SQL Server 故障转移群集实例 (FCI) 不支持通过可用性组来自动进行故障转移，因此，只能为手动故障转移配置任何由 FCI 承载的可用性副本。  
  

##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **更改可用性副本的故障转移模式**  
  
1.  在对象资源管理器中，连接到承载主副本的服务器实例，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  单击要更改其副本的可用性组。  
  
4.  右键单击该副本，然后单击“属性”  。  
  
5.  在 **“可用性副本属性”** 对话框中，使用 **“故障转移模式”** 下拉列表更改此副本的故障转移模式。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **更改可用性副本的故障转移模式**  
  
1.  连接到承载主副本的服务器实例。  
  
2.  按如下所示使用 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) 语句：

    ```syntaxsql
    ALTER AVAILABILITY GROUP *group_name* MODIFY REPLICA ON '*server_name*'  
       WITH ( {  
             AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }
                | FAILOVER_MODE = { AUTOMATIC | MANUAL }
             }  )
    ```
    
    在以上脚本中：

    - 其中，*group_name* 是可用性组的名称。  
  
    - server_name  是计算机名或故障转移群集网络名称。 对于命名实例，添加“\instance_name”。 使用托管想要修改的副本的名称。
  
有关这些参数的详细信息，请参阅 [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)。  
  
以下示例（在 *MyAG* 可用性组的主要副本上输入）在可用性副本上（位于名为 *COMPUTER01* 的计算机的默认服务器实例上）将故障转移模式更改为自动故障转移。  
  
```sql
ALTER AVAILABILITY GROUP MyAG MODIFY REPLICA ON 'COMPUTER01' WITH  
    (FAILOVER_MODE = AUTOMATIC);  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **更改可用性副本的故障转移模式**  
  
1.  将目录 (**cd**) 更改为托管主副本的服务器实例。  
  
2.  使用 **Set-SqlAvailabilityReplica** cmdlet 与 **FailoverMode** 参数。 在将某一副本设置为自动故障转移时，你可能需要使用 **AvailabilityMode** 参数将该副本更改为同步提交可用性模式。  
  
    例如，以下命令将修改可用性组 `MyReplica` 中的副本 `MyAg` 以使用同步提交可用性模式和支持自动故障转移。  
  
    ```powershell
    Set-SqlAvailabilityReplica -AvailabilityMode "SynchronousCommit" -FailoverMode "Automatic" `   
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\Replicas\MyReplica  
    ```  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)  
  
