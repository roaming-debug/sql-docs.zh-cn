---
title: 还原数据库：差异
description: 本文演示如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中还原差异数据库备份。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full differential backups [SQL Server]
- restoring databases [SQL Server], full differential backups
- database backups [SQL Server], full differential backups
- database restores [SQL Server], full differential backups
- backing up databases [SQL Server], full differential backups
ms.assetid: 0dd971a4-ee38-4dd3-9f30-ef77fc58dd11
author: cawrites
ms.author: chadam
ms.openlocfilehash: 028e5b2d1f26da698edae325b003fb402f721ee1
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075839"
---
# <a name="restore-a-differential-database-backup-sql-server"></a>还原差异数据库备份 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中还原差异数据库备份。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [先决条件](#Prerequisites)  
  
     [安全性](#Security)  
  
-   **若要还原差异数据库备份，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   不允许在显式或隐式事务中使用 RESTORE。  
  
-   无法在早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中还原较新版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]创建的备份。  
  
-   在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中，可以从使用 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本创建的数据库备份来还原用户数据库。  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   在完整恢复模式或大容量日志恢复模式下，必须先备份活动事务日志（称为日志尾部），然后才能还原数据库。 有关详细信息，请参阅 [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)数据库还原到一个新位置并且可以选择重命名该数据库。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 如果不存在要还原的数据库，则用户必须有 CREATE DATABASE 权限才能执行 RESTORE。 如果数据库存在，则 RESTORE 权限默认授予 **sysadmin** 和 **dbcreator** 固定服务器角色成员以及数据库的所有者 (**dbo**)（对于 FROM DATABASE_SNAPSHOT 选项，数据库始终存在）。  
  
 RESTORE 权限被授予那些成员身份信息始终可由服务器使用的角色。 因为只有在固定数据库可以访问且没有损坏时（在执行 RESTORE 时并不会总是这样）才能检查固定数据库角色成员身份，所以 **db_owner** 固定数据库角色成员没有 RESTORE 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-restore-a-differential-database-backup"></a>还原差异数据库备份  
  
1.  连接到相应的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例之后，在“对象资源管理器”中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** 。  根据具体的数据库，选择一个用户数据库，或展开“系统数据库”并选择一个系统数据库。  
  
3.  右键单击数据库，指向“任务”  ，再指向“还原”  ，然后单击“数据库”  。  
  
4.  在 **“常规”** 页上，使用 **“源”** 部分指定要还原的备份集的源和位置。 选择以下选项之一：  
  
    -   **Database**  
  
         从下拉列表中选择要还原的数据库。 此列表仅包含已根据 **msdb** 备份历史记录进行备份的数据库。  
  
    > [!NOTE]  
    >  如果备份是从另一台服务器执行的，则目标服务器不具有指定数据库的备份历史记录信息。 这种情况下，请选择 **“设备”** 以手动指定要还原的文件或设备。  
  
    -   **设备**  
  
         单击“浏览”按钮 ( **...** ) 以打开“选择备份设备”  对话框。 在 **“备份介质类型”** 框中，从列出的设备类型中选择一种。 若要为 **“备份介质”** 框选择一个或多个设备，请单击 **“添加”** 。  
  
         将所需设备添加到 **“备份介质”** 列表框后，单击 **“确定”** 返回到 **“常规”** 页。  
  
         在“源:设备:数据库”列表框中，选择应还原的数据库名称 **。**  
  
         **注意** ：此列表仅在选择了 **“设备”** 时才可用。 只有在所选设备上具有备份的数据库才可用。  
  
5.  在 **“目标”** 部分中， **“数据库”** 框自动填充要还原的数据库的名称。 若要更改数据库名称，请在 **“数据库”** 框中输入新名称。  
  
    > [!NOTE]  
    >  若要在特定的时间点停止还原，请单击 **“时间线”** 以访问 **“备份时间线”** 对话框。 有关在特定时间点停止数据库还原的帮助，请参阅[将 SQL Server 数据库还原到某个时点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)。  
  
6.  在 **“要还原的备份集”** 网格中，选择要通过差异备份还原的备份。  
  
     有关“用于还原的备份集”  网格中的列的信息，请参阅[还原数据库（“常规”页）](../../relational-databases/backup-restore/restore-database-general-page.md)。  
  
7.  在 **“选项”** 页的 **“还原选项”** 面板中，可以根据您的实际情况选择下列任意选项：  
  
    -   **覆盖现有数据库(WITH REPLACE)**  
  
    -   **保留复制设置(WITH KEEP_REPLICATION)**  
  
    -   **还原每个备份之前进行提示**  
  
    -   **限制对还原数据库的访问(WITH RESTRICTED_USER)**  
  
     有关这些选项的详细信息，请参阅[还原数据库（“选项”页）](../../relational-databases/backup-restore/restore-database-options-page.md)。  
  
8.  为 **“恢复状态”** 框选择一个选项。 此框确定还原操作之后的数据库状态。  
  
    -   **RESTORE WITH RECOVERY** 是默认行为，它通过回滚未提交的事务，使数据库处于可以使用的状态。 无法还原其他事务日志。 如果您要立即还原所有必要的备份，则选择此选项。  
  
    -   **RESTORE WITH NORECOVERY** 不对数据库执行任何操作，不回滚未提交的事务。 可以还原其他事务日志。 除非恢复数据库，否则无法使用数据库。  
  
    -   **RESTORE WITH STANDBY** 使数据库处于只读模式。 它撤消未提交的事务，但将撤消操作保存在备用文件中，以便能够还原恢复结果。  
  
     有关这些选项的说明，请参阅[还原数据库（“选项”页）](../../relational-databases/backup-restore/restore-database-options-page.md)。  
  
9. 如果存在与数据库的活动连接，则还原操作将失败。 选中 **“关闭现有连接”** 以确保关闭 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 和数据库之间的所有活动连接。  
  
10. 如果要在每个还原操作之间进行提示，请选择 **“还原每个备份之前进行提示”** 。 除非数据库过大并且您要监视还原操作的状态，否则通常没有必要选中该选项。  
  
11. 可以使用 **“文件”** 页将数据库还原到一个新位置。 有关移动数据库的帮助，请参阅[将数据库还原到新位置 (SQL Server)](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)。  
  
12. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-restore-a-differential-database-backup"></a>还原差异数据库备份  
  
1.  执行 RESTORE DATABASE 语句并指定 NORECOVERY 子句，以还原在差异数据库备份之前执行的完整数据库备份。 有关详细信息，请参阅[操作说明：还原完整备份](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md)。  
  
2.  执行 RESTORE DATABASE 语句以还原差异数据库备份，同时指定：  
  
    -   要应用差异数据库备份的数据库的名称。  
  
    -   从其中还原差异数据库备份的备份设备。  
  
    -   NORECOVERY 子句，前提是在还原差异数据库备份之后，还要应用事务日志备份。 否则应指定 RECOVERY 子句。  
  
3.  通过完整恢复模式或大容量日志恢复模式，还原差异数据库备份可将数据库还原到差异数据库备份完成的点。 若要恢复到故障点，在创建完最后一个差异数据库备份之后，必须应用所有已创建的事务日志备份。 有关详细信息，请参阅[应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
  
#### <a name="a-restoring-a-differential-database-backup"></a>A. 还原差异数据库备份  
 以下示例将还原 `MyAdvWorks` 数据库及其差异数据库备份。  
  
```sql  
-- Assume the database is lost, and restore full database,   
-- specifying the original full database backup and NORECOVERY,   
-- which allows subsequent restore operations to proceed.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH NORECOVERY;  
GO  
-- Now restore the differential database backup, the second backup on   
-- the MyAdvWorks_1 backup device.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH FILE = 2,  
   RECOVERY;  
GO  
```  
  
#### <a name="b-restoring-a-database-differential-database-and-transaction-log-backup"></a>B. 还原数据库、差异数据库以及事务日志备份  
 以下示例将还原 `MyAdvWorks` 数据库及其差异数据库和事务日志备份。  
  
```sql  
-- Assume the database is lost at this point. Now restore the full   
-- database. Specify the original full database backup and NORECOVERY.  
-- NORECOVERY allows subsequent restore operations to proceed.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH NORECOVERY;  
GO  
-- Now restore the differential database backup, the second backup on   
-- the MyAdvWorks_1 backup device.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH FILE = 2,  
   NORECOVERY;  
GO  
-- Now restore each transaction log backup created after  
-- the differential database backup.  
RESTORE LOG MyAdvWorks  
   FROM MyAdvWorks_log1  
   WITH NORECOVERY;  
GO  
RESTORE LOG MyAdvWorks  
   FROM MyAdvWorks_log2  
   WITH RECOVERY;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [创建差异数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
-   [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [差异备份 (SQL Server)](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
