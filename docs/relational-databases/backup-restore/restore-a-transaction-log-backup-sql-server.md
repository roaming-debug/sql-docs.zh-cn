---
title: 还原事务日志备份 (SQL Server) | Microsoft Docs
description: 本文演示如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中还原事务日志备份。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restoretlog.general.f1
- sql13.swb.restoretlog.options.f1
helpviewer_keywords:
- restore log
- backing up transaction logs [SQL Server], restoring
- transaction log backups [SQL Server], restoring
- restoring transaction logs [SQL Server], restoring backups
- transaction log restores [SQL Server], SQL Server Management Studio
ms.assetid: 1de2b888-78a6-4fb2-a647-ba4bf097caf3
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7d18348dbfc80a6f3b5bec94eb47a8bf17baa76a
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075829"
---
# <a name="restore-a-transaction-log-backup-sql-server"></a>还原事务日志备份 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中还原事务日志备份。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [先决条件](#Prerequisites)  
  
     [安全性](#Security)  
  
-   **若要还原事务日志备份，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   备份必须按照其创建顺序进行还原。 在还原特定的事务日志备份之前，必须先还原下列以前备份，而不回滚未提交的事务，即 WITH NORECOVERY：  
  
    -   在特定事务日志备份之前执行的完整数据库备份和上次差异备份（如果有）。 创建最新的完整数据库备份或差异数据库备份之前，数据库必须使用完整恢复模式或大容量日志恢复模式。  
  
    -   在完整数据库备份之后执行的所有事务日志备份或在特定事务日志备份之前执行的差异备份（如果您还原了差异备份）。 必须按照创建日志备份的顺序应用它们，并且日志链没有间隔。  
  
         有关事务日志备份的详细信息，请参阅[事务日志备份 (SQL Server)](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md) 和[应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 RESTORE 权限被授予那些成员身份信息始终可由服务器使用的角色。 因为只有在固定数据库可以访问且没有损坏时（在执行 RESTORE 时并不会总是这样）才能检查固定数据库角色成员身份，所以 **db_owner** 固定数据库角色成员没有 RESTORE 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
> [!WARNING]  
>  一般的还原过程需要在“还原数据库”对话框中同时选择日志备份以及数据和差异备份。  
  
#### <a name="to-restore-a-transaction-log-backup"></a>还原事务日志备份  
  
1.  连接到相应的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例之后，在“对象资源管理器”中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** ，然后根据数据库的不同，选择用户数据库，或展开 **“系统数据库”** ，再选择系统数据库。  
  
3.  右键单击该数据库，指向“任务”，再指向“还原”，然后单击“事务日志”，这将打开“还原事务日志”对话框。  
  
    > [!NOTE]  
    >  如果 **“事务日志”** 灰显，您可能需要首先还原完整备份或差异备份。 使用 **“数据库”** 备份对话框。  
  
4.  在 **“常规”** 页上的 **“数据库”** 列表框中，选择数据库名称。 仅列出处于还原状态的数据库。  
  
5.  若要指定要还原的备份集的源和位置，请单击以下选项之一：  
  
    -   **从数据库以前的备份**  
  
         从下拉列表中选择要还原的数据库。 此列表仅包含已根据 **msdb** 备份历史记录进行备份的数据库。  
  
    -   **从文件或磁带**  
  
         单击“浏览”按钮 ( **...** ) 以打开“选择备份设备”对话框。 在 **“备份介质类型”** 框中，从列出的设备类型中选择一种。 若要为 **“备份介质”** 框选择一个或多个设备，请单击 **“添加”** 。  
  
         将所需设备添加到 **“备份介质”** 列表框后，单击 **“确定”** 返回到 **“常规”** 页。  
  
6.  在 **“选择要还原的事务日志备份”** 网格中，选择要还原的备份。 此网格列出了选定数据库可以使用的事务日志备份。 只有在日志备份的 **“第一个 LSN”** 大于数据库的 **“最后一个 LSN”** 时，此日志备份才可用。 日志备份按照它们所包含的日志序列号 (LSN) 的顺序排列，并且也必须按照这种顺序还原。  
  
     下表列出了网格的列标题并对列值进行了说明。  
  
    |标头|值|  
    |------------|-----------|  
    |**还原**|如果复选框处于选中状态，则指示要还原相应的备份集。|  
    |**名称**|备份集的名称。|  
    |组件|备份组件：数据库、文件或 \<blank>（对于事务日志） 。|  
    |**Database**|备份操作中涉及的数据库的名称。|  
    |**开始日期**|备份操作开始的日期和时间（按客户端的区域设置显示）。|  
    |**完成日期**|备份操作完成的日期和时间（按客户端的区域设置显示）。|  
    |**第一个 LSN**|备份集中第一个事务的日志序列号。 对于文件备份为空。|  
    |**最后一个 LSN**|备份集中最后一个事务的日志序列号。 对于文件备份为空。|  
    |**检查点 LSN**|创建备份时最后一个检查点的日志序号。|  
    |**完整 LSN**|最近的数据库完整备份的日志序列号。|  
    |**Server**|执行备份操作的数据库引擎实例的名称。|  
    |**用户名**|执行备份操作的用户的名称。|  
    |**大小**|备份集的大小（字节）。|  
    |**位置**|备份集在卷中的位置。|  
    |**过期日期**|备份集过期的日期和时间。|  
  
7.  选择以下方案之一：  
  
    -   **时间点**  
  
         保留默认值（“最近状态”）；或者通过单击“浏览”按钮，打开“时间点还原”对话框，从中选择特定的日期和时间。  
  
    -   **标记的事务**  
  
         将数据库还原为以前标记的事务。 选择此选项会启动 **“选择标记的事务”** 对话框，从而显示一个网格，列出选定事务日志备份中可以使用的标记的事务。  
  
         默认情况下，将一直还原到（但不包含）标记的事务为止。 若要同时还原标记的事务，请选择 **“包含标记的事务”** 。  
  
         下表列出了网格的列标题并对列值进行了说明。  
  
        |标头|值|  
        |------------|-----------|  
        |\<blank>|显示一个用于选择标记的复选框。|  
        |**事务标记**|提交事务时，用户为标记的事务指定的名称。|  
        |**Date**|事务的提交日期及时间。 事务日期和时间显示为 **msdbgmarkhistory** 表中所记录的日期和时间，而非客户端计算机的日期和时间。|  
        |**说明**|提交事务时，用户为标记的事务指定的说明（如果有的话）。|  
        |**LSN**|所标记事务的日志序列号。|  
        |**Database**|提交标记的事务时所在数据库的名称。|  
        |**用户名**|提交标记事务的数据库用户的名称。|  
  
8.  若要查看或选择高级选项，请在 **“选择页”** 窗格中单击 **“选项”** 。  
  
9. 在 **“还原选项”** 部分中，选项有：  
  
    -   **保留复制设置(WITH KEEP_REPLICATION)**  
  
         将已发布的数据库还原到创建该数据库的服务器之外的服务器时，保留复制设置。  
  
         此选项只能与“回退未提交的事务，使数据库处于可以使用的状态...”选项（等效于使用 **RECOVERY** 选项还原备份，将在后面予以介绍）一起使用。  
  
         选择此选项等效于在 **RESTORE** 语句中使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]**KEEP_REPLICATION** 选项。  
  
    -   **还原每个备份之前进行提示**  
  
         如果选中此选项，则在第一个备份集之后还原每个备份集之前，将显示“继续还原”对话框，询问你是否要继续此还原顺序。 此对话框显示下一个介质集（如果可用）的名称、备份集的名称以及备份集的说明。  
  
         如果对于不同介质集必须更换磁带，则此选项特别有用。 例如，如果服务器只有一个磁带设备，则可以使用此选项。 待您做好继续操作的准备后，再单击 **“确定”** 。  
  
         单击 **“否”** 将使数据库保持还原状态。 完成上次还原之后，您可以在方便时继续按顺序还原。 如果下一个备份是数据备份或差异备份，请再次使用 **“还原数据库”** 任务。 如果下一个备份是日志备份，请使用 **“还原事务日志”** 任务。  
  
    -   **限制对还原数据库的访问(WITH RESTRICTED_USER)**  
  
         使还原的数据库仅供 **db_owner**、 **dbcreator** 或 **sysadmin** 的成员使用。  
  
         选择此选项等效于在 **RESTORE** 语句中使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]**RESTRICTED_USER** 选项。  
  
10. 对于 **“恢复状态”** 选项，请指定还原操作之后的数据库状态。  
  
    -   **回退未提交的事务，使数据库处于可以使用的状态。无法还原其他事务日志。(RESTORE WITH RECOVERY)**  
  
         恢复数据库。 此选项等效于 **RESTORE** 语句中的 [!INCLUDE[tsql](../../includes/tsql-md.md)]**RECOVERY** 选项。  
  
         请仅在没有要还原的日志文件时选择此选项。  
  
    -   **不对数据库执行任何操作，不回退未提交的事务。可以还原其他事务日志。(RESTORE WITH NORECOVERY)**  
  
         使数据库处于未恢复的 **RESTORING** 状态。 此选项等效于在 **RESTORE** 语句中使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]**NORECOVERY** 选项。  
  
         如果选择此选项， **“保留复制设置”** 选项将不可用。  
  
        > [!IMPORTANT]  
        >  对于镜像或辅助数据库，应始终选择此选项。  
  
    -   **使数据库处于只读模式。撤消未提交的事务，但将撤消操作保存在文件中，以便可使恢复效果逆转。(RESTORE WITH STANDBY)**  
  
         使数据库处于备用状态。 此选项等效于在 **RESTORE** 语句中使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]**STANDBY** 选项。  
  
         选择此选项需要您指定一个备用文件。  
  
11. 可选选项。如果选中此选项，请在 **“备用文件”** 文本框中指定备用文件的名称。 如果您使数据库处于只读模式，则必须选中此选项。 您可以浏览到该备用文件，也可以在文本框中键入其路径名。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
> [!IMPORTANT]  
>  我们建议您在每个 RESTORE 语句中显式指定 WITH NORECOVERY 或 WITH RECOVERY 以消除混淆。 在编写脚本时，这样做尤其重要。  
  
#### <a name="to-restore-a-transaction-log-backup"></a>还原事务日志备份  
  
1.  执行 RESTORE LOG 语句应用事务日志备份，同时指定：  
  
    -   事务日志将应用到的数据库的名称。  
  
    -   将从其中还原事务日志备份的备份设备。  
  
    -   NORECOVERY 子句。  
  
     此语句的基本语法如下：  
  
     RESTORE LOG *database_name* FROM <backup_device> WITH NORECOVERY。  
  
     其中，*database_name* 是数据库的名称，<backup_device> 是包含正在还原的日志备份的设备的名称。  
  
2.  对必须应用的每个事务日志备份重复步骤 1。  
  
3.  按照还原顺序还原了最后一个备份之后，可使用以下语句之一恢复数据库：  
  
    -   作为上一个 RESTORE LOG 语句的一部分恢复数据库：  
  
        ```  
        RESTORE LOG <database_name> FROM <backup_device> WITH RECOVERY;  
        GO  
        ```  
  
    -   等待使用单独的 RESTORE DATABASE 语句恢复数据库：  
  
        ```  
        RESTORE LOG <database_name> FROM <backup_device> WITH NORECOVERY;   
        RESTORE DATABASE <database_name> WITH RECOVERY;  
        GO  
        ```  
  
         通过等待恢复数据库，可以确认已还原所有必需的日志备份。 执行时点还原时最好使用该方法。  
  
    > [!IMPORTANT]  
    >  如果要创建镜像数据库，则省略恢复步骤。 镜像数据库必须仍处于 RESTORING 状态。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 默认情况下， [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库使用简单恢复模式。 以下示例要求修改数据库以使用完整恢复模式，如下所示：  
  
```sql  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
```  
  
#### <a name="a-applying-a-single-transaction-log-backup"></a>A. 应用单个事务日志备份  
 以下示例开始时使用名为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 备份设备上的完整数据库备份来还原 `AdventureWorks2012_1`数据库。 然后该示例应用名为 `AdventureWorks2012_log`备份设备上的第一个事务日志备份。 最后，该示例恢复数据库。  
  
```sql  
RESTORE DATABASE AdventureWorks2012  
   FROM AdventureWorks2012_1  
   WITH NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 1,  
   WITH NORECOVERY;  
GO  
RESTORE DATABASE AdventureWorks2012  
   WITH RECOVERY;  
GO  
```  
  
#### <a name="b-applying-multiple-transaction-log-backups"></a>B. 应用多个事务日志备份  
 以下示例开始时使用名为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 备份设备上的完整数据库备份来还原 `AdventureWorks2012_1`数据库。 然后该示例逐一使用名为 `AdventureWorks2012_log`备份设备上的前三个事务日志备份。 最后，该示例恢复数据库。  
  
```sql  
RESTORE DATABASE AdventureWorks2012  
   FROM AdventureWorks2012_1  
   WITH NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 1,  
   NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 2,  
   WITH NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 3,  
   WITH NORECOVERY;  
GO  
RESTORE DATABASE AdventureWorks2012  
   WITH RECOVERY;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [在完整恢复模式下将数据库还原到故障点 (Transact-SQL)](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
  
-   [将数据库还原到标记的事务 (SQL Server Management Studio)](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
## <a name="see-also"></a>另请参阅  
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)  
  
  
