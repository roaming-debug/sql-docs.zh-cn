---
title: 还原页 (SQL Server) | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中还原页面。 还原损坏的页面，而不是还原整个数据库。
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restorepage.general.f1
helpviewer_keywords:
- restoring pages [SQL Server]
- pages [SQL Server], restoring
- databases [SQL Server], damaged
- page restores [SQL Server]
- pages [SQL Server], damaged
- restoring [SQL Server], pages
ms.assetid: 07e40950-384e-4d84-9ac5-84da6dd27a91
author: cawrites
ms.author: chadam
ms.openlocfilehash: 902cb988e52eecedaa8ce95aeb3d922b6daef42c
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125533"
---
# <a name="restore-pages-sql-server"></a>还原页 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本主题说明如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)]在  中还原页。 页面还原的目的是还原一个或多个损坏的页，而不还原整个数据库。 通常，要进行还原的页已经由于在访问该页时遇到错误而标记为“可疑”。 可疑页在 [msdb](../../relational-databases/system-tables/suspect-pages-transact-sql.md) 数据库的 **suspect_pages** 表中进行了标识。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [何时适合页面还原？](#WhenUseful)  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要还原页，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="when-is-a-page-restore-useful"></a><a name="WhenUseful"></a> 何时适合页面还原？  
 页面还原用于修复隔离出来的损坏页。 还原和恢复少量页面的速度可能比还原一个文件更快，因此减少了还原操作中处于脱机状态的数据量。 然而，如果文件中要还原的不只是少量页面，则通常还原整个文件更为有效。 例如，如果某个设备上的大量页都指出此设备有未解决的故障；不妨考虑还原该文件（可以还原到另一位置）并修复该设备。  
  
 此外，并非所有的页面错误都需要还原。 缓存数据（例如辅助索引）中可能出现的问题可以通过重新计算这些数据来解决。 例如，如果数据库管理员删除一个辅助索引，然后再重新生成一个辅助索引，则损坏的数据虽然已修复，但并没有在 [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) 表中反映出这一情况。  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   页面还原适用于使用完整或大容量日志恢复模式的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。 只有读/写文件组支持页面还原。  
  
-   仅可以还原数据库页。 页面还原不能用于还原下列内容：  
  
    -   事务日志  
  
    -   分配页：全局分配映射 (GAM) 页、共享全局分配映射 (SGAM) 页和页可用空间 (PFS) 页。  
  
    -   所有数据文件的页 0（文件引导页）  
  
    -   页 1:9（数据库引导页）  
  
    -   全文目录  
  
-   对于使用大容量日志恢复模式的数据库，页面还原还有下列附加条件：  
  
    -   对大容量日志数据而言，在文件组或页数据处于脱机状态时进行备份是有问题的，因为日志中不记录脱机数据。 任何脱机页都可能导致无法备份日志。 在这种情况下，则应考虑使用 DBCC REPAIR，因为此方式导致的数据丢失少于还原到最近备份引起的数据丢失。  
  
    -   如果大容量日志数据库的日志备份遇到错误页，除非指定了 WITH CONTINUE_AFTER_ERROR，否则将失败。  
  
    -   通常，页面还原不能与大容量日志恢复模式配合使用。  
  
         执行页面还原的最佳做法是将数据库设置为完整恢复模式，并尝试进行一次日志备份。 如果可以进行日志备份，则可以继续进行页面还原。 如果日志备份失败，则您将不得不丢失上一个日志备份之后的工作，或必须尝试运行 DBCC（必须使用 REPAIR_ALLOW_DATA_LOSS 选项）。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   页面还原方案：  
  
     脱机页面还原  
     [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的所有版本都支持在数据库脱机时还原页面。 在脱机还原页过程中，还原损坏的页时数据库处于脱机状态。 还原顺序结束时，数据库将联机。  
  
     联机页面还原  
     [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition 支持联机页面还原，但它们在数据库当前处于脱机状态时将使用脱机还原。 在大多数情况下，可以在数据库（包括页面要还原到的文件组）保持联机状态时还原损坏的页。 在主文件组处于联机状态时，即使有一个或多个辅助文件组处于脱机状态，页面还原也通常联机执行。 但有时候，损坏的页可能需要脱机还原。 例如，某些重要的页发生损坏可能会使数据库无法启动。  
  
    > [!WARNING]  
    >  如果损坏的页存储了重要的数据库元数据，则在联机页面还原尝试过程中对元数据的必需的更新可能失败。 在此情况下，你可以执行脱机页面还原，但首先，你必须创建一个 [结尾日志备份](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) （通过使用 RESTORE WITH NORECOVERY 备份事务日志）。  
  
-   页面还原利用了改进的页级错误报告（包含页校验和）和跟踪。 通过校验和或残缺写操作检测为已损坏的页（“损坏页”） 可以通过页还原操作进行还原。 仅还原显式指定的页。 每个指定页都被来自指定数据备份的页的副本替换。  
  
     在您还原随后的日志备份时，它们将仅适用于包含要还原的至少一页的数据库文件。 必须将不中断的日志备份链应用于最后一次完整或差异还原，以便让包含该页的文件组前进到当前的日志文件。 与文件还原中一样，每次传递日志重做，前滚集都会前进一步。 为了成功还原页，已还原的页必须恢复到与数据库一致的状态。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 如果不存在要还原的数据库，则用户必须有 CREATE DATABASE 权限才能执行 RESTORE。 如果数据库存在，则 RESTORE 权限默认授予 **sysadmin** 和 **dbcreator** 固定服务器角色成员以及数据库的所有者 (**dbo**)（对于 FROM DATABASE_SNAPSHOT 选项，数据库始终存在）。  
  
 RESTORE 权限被授予那些成员身份信息始终可由服务器使用的角色。 因为只有在固定数据库可以访问且没有损坏时（在执行 RESTORE 时并不会总是这样）才能检查固定数据库角色成员身份，所以 **db_owner** 固定数据库角色成员没有 RESTORE 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]从 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 开始， 支持页面还原。  
  
#### <a name="to-restore-pages"></a>还原页  
  
1.  [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]连接到相应的 实例，在对象资源管理器中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** 。 根据具体的数据库，选择一个用户数据库，或展开“系统数据库”并选择一个系统数据库。  
  
3.  右键单击该数据库，指向 **“任务”** ，再指向 **“还原”** ，然后单击 **“页”** ，这将打开“还原页”对话框。  
  
     **还原**  
     此部分执行的功能与 [还原数据库（常规页）](../../relational-databases/backup-restore/restore-database-general-page.md) 上的 **“还原到”** 的功能相同。  
  
     **Database**  
     指定要还原的数据库。 您可以输入新的数据库，也可以从下拉列表中选择现有的数据库。  该列表包含了服务器上除系统数据库 **master** 和 tempdb 之外的所有数据库。  
  
    > [!WARNING]  
    >  若要还原带有密码保护的备份，必须使用 [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) 语句。  
  
     **结尾日志备份**  
     在“备份设备”  中输入或选择一个文件名，将在其中存储数据库的结尾日志备份。  
  
     **备份集**  
     此部分显示参与还原的备份集。  
  
    |标头|值|  
    |------------|------------|  
    |**名称**|备份集的名称。|  
    |组件|备份组件：  数据库、文件或 \<blank>（对于事务日志）。|  
    |类型|执行的备份类型：“完整”、“差异”或“事务日志”  。|  
    |**Server**|[!INCLUDE[ssDE](../../includes/ssde-md.md)] 执行备份操作的实例的名称。|  
    |**Database**|备份操作中涉及的数据库的名称。|  
    |**位置**|备份集在卷中的位置。|  
    |**第一个 LSN**|备份集中第一个事务的日志序列号 (LSN)。 对于文件备份为空。|  
    |**最后一个 LSN**|备份集中最后一个事务的日志序列号 (LSN)。 对于文件备份为空。|  
    |**检查点 LSN**|创建备份时最新检查点的日志序列号 (LSN)。|  
    |**完整 LSN**|最近的完整数据库备份的日志序列号 (LSN)。|  
    |**开始日期**|备份操作开始的日期和时间，按客户端的区域设置显示。|  
    |**完成日期**|备份操作完成的日期和时间，按客户端的区域设置显示。|  
    |**大小**|备份集的大小（字节）。|  
    |**用户名**|执行备份操作的用户的名称。|  
    |**过期日期**|备份集的过期日期和时间。|  
  
      单击“验证”以检查执行页还原操作所需的备份文件的完整性。  
  
4.   若要标识已损坏的页，则在 **“数据库”** 框中选择了正确的数据库的情况下，单击“检查数据库页”。 此操作将运行较长时间。  
  
    > [!WARNING]  
    >  若要还原未损坏的特定页，请单击“添加”  ，然后输入要还原的页的“文件 ID”  和“页 ID”  。  
  
5.  页网格用于标识要还原的页。 最初，此网格将从 [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) 系统表填充。  若要从该网格添加或删除页，请单击 **“添加”** 或“删除”。 有关详细信息，请参阅 [管理 suspect_pages 表 (SQL Server)](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)在  中还原页。  
  
6.  **“备份集”** 网格将列出默认还原计划中的备份集。  或者，单击“验证”可验证备份是否可读取以及备份集是否完整而无需还原。 有关详细信息，请参阅 [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)。  
  
     **页**  
  
7.  若要还原在页网格中列出的页，请单击“确定”。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 若要在 RESTORE DATABASE 语句中指定一页，需要知道该页所在文件的文件 ID 和该页的页 ID。 所需语法如下：  
  
 `RESTORE DATABASE <database_name>`  
  
 `PAGE = '<file: page> [ ,... n ] ' [ ,... n ]`  
  
 `FROM <backup_device> [ ,... n ]`  
  
 `WITH NORECOVERY`  
  
 有关 PAGE 选项的参数的详细信息，请参阅 [RESTORE Arguments (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)。 有关 RESTORE DATABASE 语法的详细信息，请参阅 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)。  
  
#### <a name="to-restore-pages"></a>还原页  
  
1.  获取要还原的损坏页的页 ID。 校验和或残缺写错误将返回页 ID，并提供指定页所需的信息。 若要查找损坏页的页 ID，请使用下列任一来源。  
  
    |页 ID 源|主题|  
    |-----------------------|-----------|  
    |**msdb..suspect_pages**|[管理 suspect_pages 表 (SQL Server)](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)|  
    |错误日志|[查看 SQL Server 错误日志 (SQL Server Management Studio)](../../relational-databases/performance/view-the-sql-server-error-log-sql-server-management-studio.md)|  
    |事件跟踪|[监视事件和响应事件](../../ssms/agent/monitor-and-respond-to-events.md)|  
    |DBCC|[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)|  
    |WMI 提供程序|[WMI Provider for Server Events 的概念](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md)|  
  
2.  从包含页的完整数据库备份、文件备份或文件组备份开始进行页面还原。 在 RESTORE DATABASE 语句中，使用 PAGE 子句列出所有要还原的页的页 ID。  
  
3.  应用最近的差异。  
  
4.  应用后续日志备份。  
  
5.  创建新的数据库日志备份，使其包含已还原页的最终 LSN，即最后还原的页脱机的时间点。 设置为顺序中首先还原的最终 LSN 是重做目标 LSN。 包含该页的文件的联机前滚可以在重做目标 LSN 处停止。 若要了解文件的当前重做目标 LSN，请查看 **sys.master_files** 的 **redo_target_lsn** 列。 有关详细信息，请参阅 [sys.master_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)。  
  
6.  还原新的日志备份。 应用这个新的日志备份后，就完成了页面还原，可以开始使用页了。  
  
    > [!NOTE]  
    >  此顺序与文件还原顺序类似。 事实上，页面还原和文件还原都可以在相同的顺序中执行。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 以下示例使用 `B` 还原文件 `NORECOVERY`的四个损坏页。 随后，将使用 `NORECOVERY`应用两个日志备份，然后是结尾日志备份（使用 `RECOVERY`还原）。 此示例执行联机还原。 此示例中，文件 `B` 的文件 ID 为 `1`，损坏的页的页 ID 分别为 `57`、 `202`、 `916`和 `1016`。  
  
```sql  
RESTORE DATABASE <database> PAGE='1:57, 1:202, 1:916, 1:1016'  
   FROM <file_backup_of_file_B>   
   WITH NORECOVERY;  
RESTORE LOG <database> FROM <log_backup>   
   WITH NORECOVERY;  
RESTORE LOG <database> FROM <log_backup>   
   WITH NORECOVERY;   
BACKUP LOG <database> TO <new_log_backup>;   
RESTORE LOG <database> FROM <new_log_backup> WITH RECOVERY;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [管理 suspect_pages 表 (SQL Server)](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)   
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)  
  
  
