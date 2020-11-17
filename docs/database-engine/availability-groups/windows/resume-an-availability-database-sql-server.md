---
title: 恢复可用性组数据库
description: 在 SQL Server 中使用 SQL Server Management Studio、Transact-SQL 或 PowerShell，恢复 Always On 可用性组中已挂起的可用性数据库。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.resumedatamove.f1
helpviewer_keywords:
- Availability Groups [SQL Server], resuming a database
- secondary databases [SQL Server], in availability group
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], databases
ms.assetid: 20e9147b-e985-4caa-910e-fc4b38dbf9a1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7e2ea65c58dbc3c22dcda8a5766913079f0baaf1
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583946"
---
# <a name="resume-an-availability-database-sql-server"></a>恢复可用性数据库 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  您可以通过使用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 、 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]或 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中的 PowerShell，在 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]中恢复已挂起的可用性数据库。 恢复挂起的数据库会将数据库置于 SYNCHRONIZING 状态。 恢复主数据库还将恢复挂起主数据库时导致挂起的任何辅助数据库。 如果任何辅助数据库是从承载辅助副本的服务器实例中本地挂起的，则该辅助数据库必须进行本地恢复。 一旦给定的辅助数据库和相应的主数据库处于 SYNCHRONIZING 状态，则在辅助数据库上将恢复数据同步。  
  
> [!NOTE]  
>  挂起和恢复 AlwaysOn 辅助数据库并不直接影响主数据库的可用性。 但是，暂停某一辅助数据库可能会在恢复这个暂停的辅助数据库之前影响主数据库的冗余和故障转移功能。 这与数据库镜像相反，在数据库镜像中，在恢复镜像前镜像状态在镜像数据库和主数据库上都挂起。 挂起 AlwaysOn 主数据库将挂起所有相应辅助数据库上的数据移动，并且在恢复主数据库前针对该数据库的冗余和故障转移功能将停止。  
  
  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 RESUME 命令只要被承载目标数据库的副本接受后就返回，但是实际上继续数据库以异步方式发生。  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   您必须连接到承载要恢复的数据库的服务器实例。    
-   可用性组必须联机。    
-   主数据库必须处于联机状态且可用。  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 ALTER 权限。  
  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **恢复辅助数据库**  
  
1.  在对象资源管理器中，连接到承载要恢复的数据库所在的可用性副本的服务器实例，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”节点和“可用性组”节点。  
  
3.  展开该可用性组。  
  
4.  展开“可用性数据库”节点，右键单击该数据库，然后单击“恢复数据移动”。  
  
5.  在 **“恢复数据移动”** 对话框中，单击 **“确定”** 。  
  
> [!NOTE]  
>  若要恢复此副本位置上的其他数据库，请对每个数据库重复执行步骤 4 和 5。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **恢复本地挂起的辅助数据库**  
  
1.  连接到承载想要恢复其数据库的辅助副本的服务器实例。  
  
2.  通过使用下面的 [ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) 语句恢复辅助数据库：  

     ALTER DATABASE *database_name* SET HADR RESUME;
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **恢复辅助数据库**  
  
1.  将目录 (**cd**) 更改为托管要恢复其数据库的副本的服务器实例。 有关详细信息，请参阅本主题前面的 [先决条件](#Prerequisites)。  
  
2.  使用 **Resume-SqlAvailabilityDatabase** cmdlet 恢复可用性组。  
  
     例如，下面的命令针对可用性组 `MyDb3` 中的可用性数据库 `MyAg`恢复数据同步。  
  
    ```  
    Resume-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\Databases\MyDb3  
    ```  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [挂起可用性数据库 (SQL Server)](../../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
