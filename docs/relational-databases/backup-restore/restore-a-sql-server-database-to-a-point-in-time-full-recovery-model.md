---
description: 将 SQL Server 数据库还原到某个时点（完整恢复模式）
title: 将 SQL Server 数据库还原到某个时点（完整恢复模式）| Microsoft Docs
decription: Learn how to restore a database to a point in time in SQL Server using SQL Server Management Studio or Transact-SQL in the full or bulk-logged recovery models.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- STOPAT clause [RESTORE LOG statement]
- point in time recovery [SQL Server]
- restoring databases [SQL Server], point in time
ms.assetid: 3a5daefd-08a8-4565-b54f-28ad01a47d32
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4038ba678af8635f3ac2bd12f68c818a61e2c87f
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075628"
---
# <a name="restore-a-sql-server-database-to-a-point-in-time-full-recovery-model"></a>将 SQL Server 数据库还原到某个时点（完整恢复模式）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 将数据库还原到 [!INCLUDE[tsql](../../includes/tsql-md.md)]中的某个时间点。 本主题仅与使用完整恢复模式或大容量日志恢复模式的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库有关。  
  
> [!IMPORTANT]  
>  在大容量日志恢复模式下，如果日志备份包含大容量更改，则不能使用时点恢复方式恢复到该备份内的某个点。 必须将数据库恢复到事务日志备份的结尾。  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **如何将 SQL Server 数据库还原到某个时间点，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   使用 STANDBY 查找未知的时间点。  
  
-   在还原顺序中尽早指定时间点  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 如果不存在要还原的数据库，则用户必须有 CREATE DATABASE 权限才能执行 RESTORE。 如果数据库存在，则 RESTORE 权限默认授予 **sysadmin** 和 **dbcreator** 固定服务器角色成员以及数据库的所有者 (**dbo**)（对于 FROM DATABASE_SNAPSHOT 选项，数据库始终存在）。  
  
 RESTORE 权限被授予那些成员身份信息始终可由服务器使用的角色。 因为只有在固定数据库可以访问且没有损坏时（在执行 RESTORE 时并不会总是这样）才能检查固定数据库角色成员身份，所以 **db_owner** 固定数据库角色成员没有 RESTORE 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **将数据库还原到时间点**  
  
1.  在“对象资源管理器”中，连接到相应的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]实例，然后展开服务器树。  
  
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
  
6.  单击 **“时间线”** 以访问 **“备份时间线”** 对话框。  
  
7.  在 **“还原到”** 部分中，单击 **“具体日期和时间”**。  
  
8.  使用 **“日期”** 和 **“时间”** 框或滑动条来指定应停止还原的具体日期和时间。 [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >   使用 **“时间线间隔”** 框更改时间线上显示的时间量。  
  
9. 指定具体时点后，数据库恢复顾问确保只有需要还原到该时点的那些备份在 **“要还原的备份集”** 网格的 **“还原”** 列中处于选中状态。 这些选定的备份构成了为您的时点还原建议的还原计划。 应当仅使用选定的备份进行时点还原操作。  
  
     有关“用于还原的备份集”  网格中的列的信息，请参阅[还原数据库（“常规”页）](../../relational-databases/backup-restore/restore-database-general-page.md)。 有关数据库恢复顾问的信息，请参阅[还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)。  
  
10. 在 **“选项”** 页的 **“还原选项”** 面板中，可以根据您的实际情况选择下列任意选项：  
  
    -   **覆盖现有数据库(WITH REPLACE)**  
  
    -   **保留复制设置(WITH KEEP_REPLICATION)**  
  
    -   **限制对还原数据库的访问(WITH RESTRICTED_USER)**  
  
     有关这些选项的详细信息，请参阅[还原数据库（“选项”页）](../../relational-databases/backup-restore/restore-database-options-page.md)。  
  
11. 为 **“恢复状态”** 框选择一个选项。 此框确定还原操作之后的数据库状态。  
  
    -   **RESTORE WITH RECOVERY** 是默认行为，它通过回滚未提交的事务，使数据库处于可以使用的状态。 无法还原其他事务日志。 如果您要立即还原所有必要的备份，则选择此选项。  
  
    -   **RESTORE WITH NORECOVERY** 不对数据库执行任何操作，不回滚未提交的事务。 可以还原其他事务日志。 除非恢复数据库，否则无法使用数据库。  
  
    -   **RESTORE WITH STANDBY** 使数据库处于只读模式。 它撤消未提交的事务，但将撤消操作保存在备用文件中，以便能够还原恢复结果。  
  
     有关这些选项的说明，请参阅[还原数据库（“选项”页）](../../relational-databases/backup-restore/restore-database-options-page.md)。  
  
12. 如果对于选择的时间点是必需的，则选择“还原前进行结尾日志备份”。 无需修改此设置，但可以选择备份日志尾部（即使不需要）。  
  
13. 如果存在与数据库的活动连接，则还原操作可能会失败。 选中 **“关闭现有连接”** 以确保关闭 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 和数据库之间的所有活动连接。 此复选框可在执行还原操作之前将数据库设置为单用户模式，并在该操作完成后将数据库设置为多用户模式。  
  
14. 如果要在每个还原操作之间进行提示，请选择 **“还原每个备份之前进行提示”** 。 除非数据库过大并且您要监视还原操作的状态，否则通常没有必要选中该选项。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **开始之前**  
  
 始终从日志备份还原到指定时间。 在还原序列的每个 RESTORE LOG 语句中，必须在相同的 STOPAT 子句中指定目标时间或事务。 作为时点还原的先决条件，必须首先还原其端点早于目标还原时间的完整数据库备份。 只要您之后还原每个随后日志备份（到达和包括包含目标时间点的日志备份），该完整数据库备份就可以早于最近的完整数据库备份。  
  
 如果数据备份太临近指定的目标时间，而需帮助识别要还原哪个数据库备份，则可以在 RESTORE DATABASE 语句中可选地指定 WITH STOPAT 子句以引发错误。 始终会还原完整数据备份，即使该数据备份包含目标时间也同样如此。  
  
 **基本 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法**  
  
 RESTORE LOG database_name FROM <backup_device> WITH STOPAT =time, RECOVERY…  
  
 恢复点是在 **time** 指定的 *datetime* 值或之前发生的最新的事务提交。  
  
 要只还原在特定时间点之前所做的修改，请为还原的每个备份指定 WITH STOPAT = time。 这样确保了不会超出目标时间。  
  
 **将数据库还原到时间点**  
  
> [!NOTE]  
>  有关此过程的示例，请参阅本节后面的 [示例 (Transact-SQL)](#TsqlExample)。  
  
1.  连接到您要还原数据库的服务器实例。  
  
2.  执行使用 NORECOVERY 选项的 RESTORE DATABASE 语句。  
  
    > [!NOTE]  
    >  如果部分还原顺序不包括任何 [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md) 文件组，则不支持时间点还原。 可以强制该还原顺序以继续执行操作。 但在 RESTORE 语句中省略的 FILESTREAM 文件组将永远无法还原。 若要强制执行时点还原，请指定 CONTINUE_AFTER_ERROR 选项以及 STOPAT、STOPATMARK 或 STOPBEFOREMARK 选项，还必须在随后的 RESTORE LOG 语句中指定后面的三个选项。 如果指定 CONTINUE_AFTER_ERROR，则部分还原顺序将成功，但 FILESTREAM 文件组将不可恢复。  
  
3.  还原上次差异数据库备份（如果有），而不恢复数据库 (RESTORE DATABASE *database_name* FROM *backup_device* WITH NORECOVERY)。  
  
4.  以创建事务日志备份的相同顺序应用每个事务日志备份，同时指定要停止还原日志的时间 (RESTORE DATABASE *database_name* FROM <backup_device> WITH STOPAT **=**_time_**,** RECOVERY)。  
  
    > [!NOTE]  
    >  RECOVERY 和 STOPAT 选项。 如果事务日志备份不包含要求的时间（例如，如果指定的时间超出了事务日志所包含的时间范围），则会生成警告，并且不会恢复数据库。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 下面的示例将数据库还原到它在 `12:00 AM` 的 `April 15, 2020` 的状态，并显示涉及多个日志备份的还原操作。 在备份设备上，要还原的完整数据库备份 `AdventureWorksBackups`是设备上的第三个备份集 (`FILE = 3`)，第一个日志备份是第四个备份集 (`FILE = 4`)，第二个日志备份是第五个备份集 (`FILE = 5`)。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库使用简单恢复模式。 若要允许日志备份，请在完整备份数据库之前，使用 `ALTER DATABASE AdventureWorks SET RECOVERY FULL`将数据库设置为使用完整恢复模式。  
  
```  
RESTORE DATABASE AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=3, NORECOVERY;  
  
RESTORE LOG AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=4, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';  
  
RESTORE LOG AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=5, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';  
RESTORE DATABASE AdventureWorks WITH RECOVERY;   
GO  
  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [在完整恢复模式下将数据库还原到故障点 (Transact-SQL)](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [将数据库还原到标记的事务 (SQL Server Management Studio)](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
-   [恢复到日志序列号 (SQL Server)](../../relational-databases/backup-restore/recover-to-a-log-sequence-number-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ToPointInTime%2A> (SMO)  
  
## <a name="see-also"></a>另请参阅  
 [backupset (Transact-SQL)](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
  
