---
title: 在数据库损坏时备份事务日志 (SQL Server) | Microsoft Docs
description: 本主题演示如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中当数据库损坏时备份事务日志。
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- databases [SQL Server], damaged
- backing up [SQL Server]. damaged database
- transaction log backups [SQL Server], damaged databases
ms.assetid: 9b8873cc-df54-4336-ab9b-8f525132c2b0
author: cawrites
ms.author: chadam
ms.openlocfilehash: e03f51fa7a90e7a547e85f226148870043d89a0e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077085"
---
# <a name="back-up-the-transaction-log-when-the-database-is-damaged-sql-server"></a>在数据库损坏时备份事务日志 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明当数据库损坏时如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中备份事务日志。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要在数据库损坏时备份事务日志，可使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   不允许在显式或隐式事务中使用 BACKUP 语句。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   对于使用完整恢复模式或大容量日志恢复模式的数据库，通常需要在开始还原数据库前备份日志尾部。 在对日志传送配置进行故障转移之前，还应当备份主数据库的日志尾部。 将结尾日志备份作为恢复数据库之前的最后一个日志备份还原可以防止失败后丢失工作。 有关结尾日志备份的详细信息，请参阅[结尾日志备份 (SQL Server) ](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，为 **sysadmin** 固定服务器角色以及 **db_owner** 和 **db_backupoperator** 固定数据库角色的成员授予 BACKUP DATABASE 和 BACKUP LOG 权限。  
  
 备份设备的物理文件的所有权和权限问题可能会妨碍备份操作。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须能够读取和写入设备；运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的帐户必须具有写入权限。 但是，用于在系统表中为备份设备添加项目的 [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)不检查文件访问权限。 备份设备物理文件的这些问题可能直到为备份或还原而访问物理资源时才会出现。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-back-up-the-tail-of-the-transaction-log"></a>备份事务日志尾部  

1.  连接到相应的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例之后，在“对象资源管理器”中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** ，然后根据数据库的不同，选择用户数据库，或展开 **“系统数据库”** ，再选择系统数据库。  
  
3.  右键单击数据库，指向 **“任务”** ，再单击 **“备份”** 。 将出现 **“备份数据库”** 对话框。  
  
4.  在 **“数据库”** 列表框中，验证数据库名称。 您也可以从列表中选择其他数据库。  
  
5.  验证恢复模式是 **FULL** 还是 **BULK_LOGGED**。  
  
6.  在 **“备份类型”** 列表框中，选择 **“事务日志”** 。  
  
7.  使 **“仅复制备份”** 处于取消选中状态。  
  
8.  在 **“备份集”** 区域中，可以接受 **“名称”** 文本框中建议的默认备份集名称，也可以为备份集输入其他名称。  
  
9. 在 **“说明”** 文本框中，输入结尾日志备份的说明。  
  
10. 指定备份集的过期时间：  
  
    -   若要使备份集在特定天数后过期，请单击 **“之后”** （默认选项），并输入备份集从创建到过期所需的天数。 此值范围为 0 到 99999 天；0 天表示备份集将永不过期。  
  
         默认值在 **“服务器属性”** 对话框（位于 **“数据库设置”** 页上）的 **“默认备份媒体保持期(天)”** 选项中设置。 若要访问此对话框，请在对象资源管理器中右键单击服务器名称，选择“属性”，再选择 **“数据库设置”** 页。  
  
    -   若要使备份集在特定日期过期，请单击 **“在”** ，并输入备份集的过期日期。  
  
11. 通过单击 **“磁盘”** 或 **“磁带”** ，选择备份目标的类型。 若要选择包含单个介质集的多个磁盘或磁带机（最多为 64 个）的路径，请单击 **“添加”** 。 选择的路径将显示在 **“备份到”** 列表框中。  
  
     若要删除备份目标，请选择该备份目标并单击 **“删除”** 。 若要查看备份目标的内容，请选择该备份目标并单击 **“内容”** 。  
  
12. 在 **“选项”** 页上，可以通过单击下列选项之一来选择 **“覆盖介质”** 选项：  
  
    -   **备份到现有介质集**  
  
         对于此选项，请单击 **“追加到现有备份集”** 或 **“覆盖所有现有备份集”** 。  
  
         或者选择 **“检查介质集名称和备份集过期时间”** ，以使备份操作对介质集和备份集的过期日期和时间进行验证。  
  
         或者在 **“介质集名称”** 文本框中输入名称。 如果没有指定名称，将使用空白名称创建介质集。 如果指定了介质集名称，将检查介质（磁带或磁盘），以确定实际名称是否与此处输入的名称匹配。  
  
         如果将介质名称保留空白，并选中该框以便与介质进行核对，则只有当介质上的介质名称也是空白时才能成功。  
  
    -   **备份到新介质集并清除所有现有备份集**  
  
         对于该选项，请在 **“新建介质集名称”** 文本框中输入名称，并在 **“新建介质集说明”** 文本框中描述介质集（可选）。  
  
     有关媒体集选项的详细信息，请参阅[集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)。  
  
13. 或者，在 **“可靠性”** 部分中，选中：  
  
    -   **完成后验证备份**。  
  
    -   **写入介质前检查校验和**。  
  
    -   **出现校验和错误时继续**  
  
     有关校验和的信息，请参阅[在备份和还原期间可能的媒体错误 (SQL Server)](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)。  
  
14. 在 **“事务日志”** 部分，选中 **“备份日志尾部，并使数据库处于还原状态”** 。  
  
     这相当于指定以下 [BACKUP](../../t-sql/statements/backup-transact-sql.md) 语句：  
  
     `BACKUP LOG <database_name> TO <backup_device> WITH NORECOVERY`  
  
    > [!IMPORTANT]  
    >  在还原时，“还原数据库”对话框将结尾日志备份的类型显示为“事务日志(仅备份)”  。  
  
15. 如果备份到磁带驱动器（如同 **“常规”** 页的 **“目标”** 部分指定的一样），则 **“备份后卸载磁带”** 选项处于活动状态。 单击此选项可以激活 **“卸载前倒带”** 选项。  
  
16. [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] 及更高版本支持 [备份压缩](../../relational-databases/backup-restore/backup-compression-sql-server.md)。 默认情况下，是否压缩备份取决于 **backup-compression default** 服务器配置选项的值。 但是，不管当前服务器级默认设置如何，都可以通过选中 **“压缩备份”** 来压缩备份，并且可以通过选中 **“不压缩备份”** 来防止压缩备份。  
  
     **查看当前备份压缩默认值**  
  
    -   [查看或配置 backup compression default 服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-backup-of-the-currently-active-transaction-log"></a>创建当前活动的事务日志的备份  
  
1.  执行 BACKUP LOG 语句以备份当前活动的事务日志，同时指定：  
  
    -   要备份的事务日志所属的数据库的名称。  
  
    -   事务日志备份将写入的备份设备。  
  
    -   NO_TRUNCATE 子句。  
  
         只要事务日志文件是可访问的并且没有损坏，那么即使数据库不可访问，该子句也允许备份事务日志的活动部分。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
  
> [!NOTE]  
>  此示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]，该对象使用简单恢复模式。 若要允许日志备份，请在完整备份数据库之前，将数据库设置为使用完整恢复模式。 有关详细信息，请参阅[查看或更改数据库的恢复模式 (SQL Server)](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md)。  
  
 此示例在数据库损坏且不可访问时备份当前活动事务日志（如果事务日志已损坏且可访问）。  
  
```sql  
BACKUP LOG AdventureWorks2012  
   TO MyAdvWorks_FullRM_log1  
   WITH NO_TRUNCATE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)   
 [将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)   
 [备份数据库（“备份选项”页）](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)   
 [备份数据库（“常规”页）](../../relational-databases/backup-restore/back-up-database-general-page.md)   
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [文件还原（简单恢复模式）](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [文件还原（完整恢复模式）](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)  
  
  
