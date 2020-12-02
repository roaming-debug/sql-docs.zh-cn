---
title: 设置数据库恢复模式
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中将 SQL Server 数据库从一种恢复模式切换为另一种。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- database backups [SQL Server], recovery models
- recovery [SQL Server], recovery model
- backing up databases [SQL Server], recovery models
- recovery models [SQL Server], switching
- recovery models [SQL Server], viewing
- database restores [SQL Server], recovery models
- modifying database recovery models
ms.assetid: 94918d1d-7c10-4be7-bf9f-27e00b003a0f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6f4b1c8c2bcdc3a755a0311d83a08f7b083d4676
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125314"
---
# <a name="view-or-change-the-recovery-model-of-a-database-sql-server"></a>查看或更改数据库的恢复模式 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)]查看或更改数据库。 
  
  “恢复模式”  是一种数据库属性，它控制如何记录事务，事务日志是否需要（以及允许）进行备份，以及可以使用哪些类型的还原操作。 有三种恢复模式：简单恢复模式、完整恢复模式和大容量日志恢复模式。 通常，数据库使用完整恢复模式或简单恢复模式。 数据库可以随时切换为其他恢复模式。 **model** 数据库将设置新数据库的默认恢复模式。  
  
  有关更深入的说明，请参阅[恢复模式](recovery-models-sql-server.md)。
  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 准备工作  
  

-   在从[完整恢复模式或大容量日志恢复模式](recovery-models-sql-server.md)切换前，先[备份事务日志](back-up-a-transaction-log-sql-server.md)  。  
  
-   时点恢复在大容量日志模式下不可能进行。 在需要事务日志还原的大容量日志恢复模式下运行事务可能会导致事务丢失数据。 若要在灾难恢复方案中最大程度地恢复数据，则仅在以下条件下切换到大容量日志恢复模式：  
  
    -   数据库中当前不允许存在用户。  
  
    -   在大容量处理过程中进行的所有修改均不依靠日志备份就可恢复；例如，通过重新运行大容量处理。  
  
     如果满足这两个条件，在大容量日志恢复模式下还原备份的事务日志时将不会丢失任何数据。  
  
**注意！** 如果在大容量操作过程中切换到完整恢复模式，则大容量操作日志记录将从最小日志记录更改为最大日志记录，反之亦然。  
  
###  <a name="required-permissions"></a><a name="Security"></a> 所需的权限  
   需要对数据库拥有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-view-or-change-the-recovery-model"></a>查看或更改恢复模式  
  
1.  连接到相应的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]实例之后，在对象资源管理器中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** ，然后根据数据库的不同，选择用户数据库，或展开 **“系统数据库”** ，再选择系统数据库。  
  
3.  右键单击该数据库，再单击“属性”  ，这将打开“数据库属性”  对话框。  
  
4.  在 **“选择页”** 窗格中，单击 **“选项”** 。  
  
5.  当前恢复模式显示在 **“恢复模式”** 列表框中。  
  
6.  也可以从列表中选择不同的模式来更改恢复模式。 可以选择“完整”  、“大容量日志”  或“简单”  。  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-view-the-recovery-model"></a>查看恢复模式  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。 此示例说明如何对 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图执行查询以了解 **模型** 数据库的恢复模式。  
  
```sql  
SELECT name, recovery_model_desc  
   FROM sys.databases  
      WHERE name = 'model' ;  
GO  
  
```  
  
#### <a name="to-change-the-recovery-model"></a>更改恢复模式  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。 此示例说明如何使用 `model` ALTER DATABASE `FULL` 语句的 `SET RECOVERY` 选项将 [数据库中的恢复模式更改为](../../t-sql/statements/alter-database-transact-sql-set-options.md) 。  
  
```sql  
USE [master] ;  
ALTER DATABASE [model] SET RECOVERY FULL ;  
```  
  
##  <a name="recommendations-after-you-change-the-recovery-model"></a><a name="FollowUp"></a> 建议：在更改恢复模式后  
  
-   **在完整恢复模式和大容量日志恢复模式之间切换后**  
  
    -   完成大容量操作之后，立即切换回完整恢复模式。  
  
    -   在从大容量日志恢复模式切换回完整恢复模式后，备份日志。  
  
        >**注意：** 您的备份策略保持不变：继续执行定期数据库备份、日志备份和差异备份。  
  
-   **从简单恢复模式切换之后**  
  
    -   切换到完整恢复模式或大容量日志恢复模式之后，立即进行完整数据库备份或差异数据库备份以启动日志链。  
  
        >**注意：** 到完整恢复模式或大容量日志恢复模式的切换仅在第一个数据备份之后才生效。  
  
    -   计划安排定期日志备份并相应地更新还原计划。  
  
        > **重要说明！！！！** 备份日志！！ 如果不经常备份日志，事务日志可能会扩展直到占满磁盘空间！  
  
-   **切换到简单恢复模式之后**  
  
    -   中断用于备份事务日志的所有计划作业。  
  
    -   确保定期执行数据库备份。 备份数据库对于保护数据和截断事务日志的不活动部分是基本操作。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
  
-   [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [创建作业](../../ssms/agent/create-a-job.md)  
  
-   [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [数据库维护计划](../maintenance-plans/maintenance-plans.md) （ [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 联机丛书中）  
  
## <a name="see-also"></a>另请参阅  
 [恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)  
  
  
