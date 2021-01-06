---
title: 删除数据库镜像 (SQL Server) | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中删除数据库的数据库镜像。
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], removing
- removing database mirroring [SQL Server]
ms.assetid: bbc4d7f7-3bc7-40d6-a822-af195fe7f8c0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 5a3cd9ef4faf8e11832288cb2cc6a0ec512f0ded
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641322"
---
# <a name="remove-database-mirroring-sql-server"></a>删除数据库镜像 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中从数据库删除数据库镜像。  数据库所有者可以随时通过从数据库中删除镜像来手动停止数据库镜像会话。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **删除数据库镜像，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [在删除数据库镜像之后](#FollowUp)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-remove-database-mirroring"></a>删除数据库镜像  
  
1.  在数据库镜像会话期间，连接到主体服务器实例，然后在对象资源管理器中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** 并选择数据库。  
  
3.  右键单击数据库，选择 **“任务”** ，再单击 **“镜像”** 。 这样便可打开 **“数据库属性”** 对话框的 **“镜像”** 页。  
  
4.  在 **“选择页”** 窗格中，单击 **“镜像”** 。  
  
5.  若要删除镜像，请单击 **“删除镜像”** 。 此时，将显示一个提示，要求您进行确认。 如果单击 **“是”** ，会话将停止，并从数据库中删除镜像。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 若要删除数据库镜像，请使用 **“数据库属性”** ， 即使用 **“数据库属性”** 对话框的 **“镜像”** 页。  
  
#### <a name="to-remove-database-mirroring"></a>删除数据库镜像  
  
1.  为任一镜像伙伴连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  发出以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```  
    ALTER DATABASE database_name SET PARTNER OFF  
    ```  
  
     其中 *database_name* 是要删除其会话的镜像数据库。  
  
     以下示例从 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库删除数据库镜像。  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER OFF;  
    ```  
  
##  <a name="follow-up-removing-database-mirroring"></a><a name="FollowUp"></a> 跟进：删除数据库镜像  
  
> [!NOTE]  
>  有关删除镜像的影响的信息，请参阅[删除数据库镜像 (SQL Server)](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md)。  
  
-   **如果您打算在数据库上重新启动镜像**  
  
     重新启动镜像之前，必须将在删除镜像后对主体数据库执行的日志备份全部应用到镜像数据库中。  
  
-   **如果不打算重启镜像**  
  
     或者，可以恢复以前的镜像数据库。 在作为镜像服务器的服务器实例上，可以使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```  
    RESTORE DATABASE database_name WITH RECOVERY;  
    ```  
  
    > [!IMPORTANT]  
    >  如果恢复该数据库，则两个同名的不同数据库处于联机状态。 因此，需要确保客户端仅可访问其中一个数据库，通常为最新的主体数据库。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [暂停或恢复数据库镜像会话 (SQL Server)](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)  
  
-   [从数据库镜像会话删除见证服务器 (SQL Server)](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)  
  
-   [使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [使用 Windows 身份验证建立数据库镜像会话 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [示例：使用证书设置数据库镜像 (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [设置数据库镜像 (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
