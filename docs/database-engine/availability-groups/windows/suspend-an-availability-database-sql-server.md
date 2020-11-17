---
title: 挂起可用性数据库
description: 了解如何使用 SQL Server Management Studio、Transact-SQL 或 PowerShell 挂起 Always On 可用性组中数据库的数据移动。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.suspenddatamove.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], suspending a database
- Availability Groups [SQL Server], databases
ms.assetid: 86858982-6af1-4e80-9a93-87451f0d7ee9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6550c409ee45e3287c6edf691fe9d5993e531a5a
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583782"
---
# <a name="suspend-an-availability-database-sql-server"></a>挂起可用性数据库 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  您可以通过使用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 、 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]或 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中的 PowerShell，在 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]中挂起可用性数据库。 请注意，挂起命令需要对承载要挂起或恢复的数据库的服务器实例发出。  
  
 挂起命令的效果取决于您挂起的是辅助数据库还是主数据库，如下所示：  
  
|挂起的数据库|挂起命令的效果|  
|------------------------|-------------------------------|  
|辅助数据库|仅挂起本地辅助数据库，并且其同步状态变为 NOT SYNCHRONIZING。 其他辅助数据库不受影响。 挂起的数据库将停止接收和应用数据（日志记录），并且开始落在主数据库的后面。 可读辅助副本上的现有连接会保持可用状态。 可读辅助副本上挂起数据库的新连接在数据移动恢复之前一直处于禁用状态。 仅当使用侦听器和只读路由打开连接时，此行为才适用。<br /><br /> 主数据库仍然可用。 如果您挂起相应的每个辅助数据库，则主数据库将公开运行。<br /><br /> **\*\* 重要提示 \*\*** 当挂起辅助数据库时，对应主数据库的发送队列将累积未发送的事务日志记录。 与辅助副本的连接将返回在数据移动挂起时可用的数据。|  
|主数据库|主数据库将停止数据移动到每个连接的辅助数据库。 主数据库将继续在公开模式下运行。 主数据库仍然保持对客户端可用，可读取辅助数据库上的现有连接保持可用并且可以建立新连接。|  
  
> [!NOTE]  
>  挂起 AlwaysOn 辅助数据库并不直接影响主数据库的可用性。 但是，挂起辅助数据库可能会影响主数据库的冗余和故障转移功能。 这与数据库镜像相反，在数据库镜像中，镜像状态在镜像数据库和主数据库上都挂起。 挂起 AlwaysOn 主数据库将挂起所有相应辅助数据库上的数据移动，并且在恢复主数据库前针对该数据库的冗余和故障转移功能将停止。  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [先决条件](#Prerequisites)  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **挂起数据库，使用：**  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [PowerShell](#PowerShellProcedure)  
  
-   **跟进：** [避免出现已满事务日志](#FollowUp)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 SUSPEND 命令只要被承载目标数据库的副本接受后就返回，但是实际上挂起数据库以异步方式发生。  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 您必须连接到承载要挂起的数据库的服务器实例。 若要挂起主数据库和相应的辅助数据库，请连接到承载主副本的服务器实例。 若要挂起辅助数据库但保持主数据库可用，请连接到辅助副本。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
 出现瓶颈时，短暂挂起一个或多个辅助数据库可能有助于暂时提高主副本的性能。 只要有一个辅助数据库仍挂起，就无法截断相应的主数据库的事务日志。 这将导致日志记录在主数据库上累积。 因此，我们建议您快速恢复或删除挂起的辅助数据库。 有关详细信息，请参阅本主题后面部分的[跟进：避免出现已满事务日志](#FollowUp)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 ALTER 权限。  
  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **挂起数据库**  
  
1.  在对象资源管理器中，连接到承载要挂起的数据库所在的可用性副本的服务器实例，然后展开服务器树。 有关详细信息，请参阅本主题前面的 [先决条件](#Prerequisites)。  
  
2.  依次展开“Always On 高可用性”节点和“可用性组”节点。  
  
3.  展开该可用性组。  
  
4.  展开“可用性数据库”节点，右键单击该数据库，然后单击“挂起数据移动”。  
  
5.  在 **“挂起数据移动”** 对话框中，单击 **“确定”** 。  
  
     对象资源管理器通过更改数据库图标以显示一个暂停指示符，来指示已挂起该数据库。  
  
> [!NOTE]  
>  若要挂起此副本位置上的其他数据库，请对每个数据库重复执行步骤 4 和 5。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **挂起数据库**  
  
1.  连接到承载要挂起数据库的副本的服务器实例。 有关详细信息，请参阅本主题前面的 [先决条件](#Prerequisites)。  
  
2.  通过使用以下 [ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) 语句，挂起数据库：  
  
     ALTER DATABASE *database_name* SET HADR SUSPEND;
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **挂起数据库**  
  
1.  将目录 (**cd**) 更改为承载要挂起其数据库的副本的服务器实例。 有关详细信息，请参阅本主题前面的 [先决条件](#Prerequisites)。  
  
2.  使用 **Suspend-SqlAvailabilityDatabase** cmdlet 挂起可用性组。  
  
     例如，以下命令为可用性数据库 `MyDb3` （位于 `MyAg` 服务器实例上的可用性组 `Computer\Instance`中）暂停数据同步。  
  
    ```  
    Suspend-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\Databases\MyDb3  
    ```  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-avoiding-a-full-transaction-log"></a><a name="FollowUp"></a> 跟进：避免出现已满事务日志  
 通常，在数据库上执行自动检查点操作时，事务日志将在下一个日志备份后截断到该检查点。 但是，当挂起辅助数据库时，当前的所有日志记录在主数据库上都保持活动状态。 如果填满该事务日志（因为它达到其最大大小或服务器实例耗尽空间），则数据库将无法再执行任何更新。  
  
 若要避免此问题，应执行以下操作之一：  
  
-   为主数据库添加更多日志空间。  
  
-   在日志填满之前恢复辅助数据库。 有关详细信息，请参阅本主题后面的 [恢复可用性数据库 (SQL Server)](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)中挂起可用性数据库。  
  
-   删除辅助数据库。 有关详细信息，请参阅[从可用性组中删除辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)。  
  
 **解决事务日志已满的问题**  
  
-   [解决事务日志已满的问题（SQL Server 错误 9002）](../../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [恢复可用性数据库 (SQL Server)](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [恢复可用性数据库 (SQL Server)](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
