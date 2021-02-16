---
title: 暂停和恢复数据库镜像会话
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL (T-SQL) 暂停和恢复 SQL Server 数据库镜像会话。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- resuming database mirroring
- database mirroring [SQL Server], sessions
- database mirroring [SQL Server], pausing
- database mirroring [SQL Server], resuming
- pausing database mirroring
ms.assetid: 05ede3b4-6abe-4442-abb7-9f5aee1d6bc0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 371331c20297f19f46384ac3f742b28448cb7459
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343726"
---
# <a name="pause-or-resume-a-database-mirroring-session-sql-server"></a>暂停或恢复数据库镜像会话 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中暂停或恢复数据库镜像。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要替换此文本，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [暂停或恢复数据库镜像之后](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
 您可以随时挂起数据库镜像会话，这可能提高瓶颈期间的性能，之后您可以随时恢复挂起的会话。  
  
> [!CAUTION]  
>  在强制服务后，当原始的主体服务器重新连接时，镜像将挂起。 在这种情况下，恢复镜像可能会导致原始主体服务器上的数据丢失。 有关管理潜在的数据丢失的信息，请参阅[数据库镜像会话期间的角色切换 (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 若要暂停或恢复数据库镜像会话，请使用 **“数据库属性镜像”** 页。  
  
#### <a name="to-pause-or-resume-database-mirroring"></a>暂停或恢复数据库镜像  
  
1.  在数据库镜像会话期间，连接到主体服务器实例，然后在对象资源管理器中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** 并选择数据库。  
  
3.  右键单击数据库，选择 **“任务”** ，再单击 **“镜像”** 。 这样便可打开 **“数据库属性”** 对话框的 **“镜像”** 页。  
  
4.  若要暂停会话，请单击 **“暂停”** 。  
  
     此时，将显示一个提示，要求您确认；如果单击 **“是”** ，则会话将暂停，并且该按钮改为 **“恢复”** 。  
  
     有关暂停会话的影响的详细信息，请参阅[暂停和恢复数据库镜像 (SQL Server)](../../database-engine/database-mirroring/pausing-and-resuming-database-mirroring-sql-server.md)。  
  
5.  若要恢复会话，请单击 **“恢复”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-pause-database-mirroring"></a>暂停数据库镜像  
  
1.  为任一伙伴连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  发出以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
     ALTER DATABASE *database_name* SET PARTNER SUSPEND  
  
     其中 *database_name* 是要挂起其会话的镜像数据库。  
  
     下面的示例暂停 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库。  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER SUSPEND;  
    ```  
  
##### <a name="to-resume-database-mirroring"></a>恢复数据库镜像  
  
1.  为任一伙伴连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  发出以下 Transact-SQL 语句：  
  
     ALTER DATABASE *database_name* SET PARTNER RESUME  
  
     其中 *database_name* 是要恢复其会话的镜像数据库。  
  
     下面的示例暂停 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库。  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER RESUME;  
    ```  
  
##  <a name="follow-up-after-pausing-or-resuming-database-mirroring"></a><a name="FollowUp"></a> 跟进：暂停或恢复数据库镜像之后  
  
-   **暂停数据库镜像之后**  
  
     在主数据库上采取预防措施以避免填满事务日志。 有关详细信息，请参阅 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)。  
  
-   **恢复数据库镜像之后**  
  
     恢复数据库镜像会将镜像数据库置于 SYNCHRONIZING 状态。 如果安全级别为 FULL，镜像将达到与主体相同的状态，镜像数据库将进入 SYNCHRONIZED 状态。 此时，可以进行故障转移。 如果见证服务器存在并且设置为 ON，则可以进行自动故障转移。 在缺少见证服务器的情况下，可以进行手动故障转移。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [删除数据库镜像 (SQL Server)](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
