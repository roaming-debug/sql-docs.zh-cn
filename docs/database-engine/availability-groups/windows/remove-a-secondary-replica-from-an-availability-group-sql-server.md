---
title: 从可用性组中删除次要副本
description: '使用 Transact-SQL (T-SQL)、PowerShell 或 SQL Server Management Studio 从 AlwaysOn 可用性组中删除次要副本的步骤。 '
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.removesecondaryar.f1
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], configuring
ms.assetid: 35ddc8b6-3e7c-4417-9a0a-d4987a09ddf7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 014636ac35e9f9bf79a22e2603355dd7d904c4bb
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783433"
---
# <a name="remove-a-secondary-replica-from-an-availability-group-sql-server"></a>从可用性组中删除辅助副本 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明了如何通过在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]中使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)]、 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]或 PowerShell 从 AlwaysOn 可用性组中删除次要副本。  
 
   
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   只有主副本支持该任务。    
-   从可用性组中仅可删除辅助副本。  
  
## <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   您必须连接到承载可用性组的主副本的服务器实例。  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **删除辅助副本**  
  
1.  在对象资源管理器中，连接到承载主副本的服务器实例，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  选择可用性组，然后展开 **“可用性副本”** 节点。  
  
4.  此步骤取决于您是要删除多个副本，还是只删除一个副本，如下所示：  
  
    -   若要删除多个副本，请使用 **“对象资源管理器详细信息”** 窗格查看并选择所有要删除的副本。 有关详细信息，请参阅[使用对象资源管理器详细信息监视可用性组 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)。  
  
    -   若要删除单个副本，请在 **“对象资源管理器”** 窗格或 **“对象资源管理器详细信息”** 窗格中选中此副本。  
  
5.  右键单击选定的一个或多个次要副本，然后在命令菜单中选择“从可用性组中删除”  。  
  
6.  在 **“从可用性组删除辅助副本”** 对话框中，要删除所有列出的辅助副本，请单击 **“确定”** 。 如果您不想删除所有列出的副本，请单击 **“取消”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **删除辅助副本**  
  
1.  连接到承载主副本的服务器实例。  
  
2.  按如下所示使用 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) 语句：  
  
     ALTER AVAILABILITY GROUP group_name  REMOVE REPLICA ON 'instance_name  ' [,...n  ]  
  
     其中，group_name  为可用性组的名称，instance_name  为该次要副本所在的服务器实例。  
  
     下面的示例将次要副本从 *MyAG* 可用性组中删除。 目标次要副本位于名为 COMPUTER02 的计算机上的服务器实例（名为 HADR_INSTANCE）上。  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG REMOVE REPLICA ON 'COMPUTER02\HADR_INSTANCE';  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **删除辅助副本**  
  
1.  将目录 (**cd**) 更改为托管主副本的服务器实例。  
  
2.  使用 **Remove-SqlAvailabilityReplica** cmdlet。  
  
     例如，下面的命令从名为 `MyReplica` 的可用性组中删除服务器 `MyAg`上的可用性副本。  此命令必须在承载可用性组的主副本的服务器实例上运行。  
  
    ```  
    Remove-SqlAvailabilityReplica `   
    -Path SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-removing-a-secondary-replica"></a><a name="PostBestPractices"></a> 跟进：在删除辅助副本之后  
 如果您指定一个当前不可用的副本，则在该副本联机时，将发现该副本已被删除。  
  
 删除副本会导致它停止接收数据。 在某个辅助副本确认其已从全局存储中删除之后，该副本将从其数据库（在本地服务器实例上保留为 RECOVERING 状态）中删除可用性组设置。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [将次要副本添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)   
 [删除可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  
