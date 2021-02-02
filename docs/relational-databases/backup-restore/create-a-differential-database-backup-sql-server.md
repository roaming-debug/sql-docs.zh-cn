---
title: 差异备份
description: 本文演示如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中创建差异数据库备份。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full differential backups [SQL Server]
- database backups [SQL Server], full differential backups
- backing up databases [SQL Server], full differential backups
- backups [SQL Server], creating
ms.assetid: 70f49794-b217-4519-9f2a-76ed61fa9f99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5a59b05df4805b580aade70a9d2bf11cc3399f04
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076119"
---
# <a name="create-a-differential-database-backup-sql-server"></a>创建差异数据库备份 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建差异数据库备份。  
  
 **本主题的内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [先决条件](#Prerequisites)  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要创建差异数据库备份，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 准备工作  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   不允许在显式或隐式事务中使用 BACKUP 语句。  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   创建差异数据库备份需要有以前的完整数据库备份。 如果你的数据库从未进行过备份，则请在创建任何差异备份之前，先执行完整数据库备份。 有关详细信息，请参阅 [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)中创建差异数据库备份。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   当差异备份的大小增大时，还原差异备份会显著延长还原数据库所需的时间。 建议按设定的间隔执行新的完整备份，以便为数据建立新的差异基准。 例如，您可以每周执行一次整个数据库的完整备份（即完整数据库备份），然后在该周内执行一系列常规的差异数据库备份。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="check-your-permissions-first"></a><a name="Permissions"></a> 首先检查你的权限！  
 BACKUP DATABASE 和 BACKUP LOG 权限默认为 **sysadmin** 固定服务器角色以及 **db_owner** 和 **db_backupoperator** 固定数据库角色。  
  
 备份设备的物理文件的所有权和权限问题将会妨碍备份操作。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 需能够读取和写入设备；运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的帐户必须具有写入权限。 但是，用于在系统表中为备份设备添加项目的 [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)**不** 检查文件访问权限。 在你因尝试备份或还原而访问物理资源之前，备份设备物理文件中的权限问题并不明显。  
  
##  <a name="sql-server-management-studio"></a><a name="SSMSProcedure"></a> SQL Server Management Studio  
  
#### <a name="create-a-differential-database-backup"></a>创建差异数据库备份  

1.  连接到相应的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例之后，在“对象资源管理器”中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** ，然后根据数据库的不同，选择用户数据库，或展开 **“系统数据库”** ，再选择系统数据库。  
  
3.  右键单击数据库，指向 **“任务”** ，再单击 **“备份”** 。 将出现 **“备份数据库”** 对话框。  
  
4.  在 **“数据库”** 列表框中，验证数据库名称。 您也可以从列表中选择其他数据库。  
  
     可以执行任意恢复模式（完整、大容量日志或简单）的差异备份。  
  
5.  在 **“备份类型”** 列表框中，选择 **“差异”** 。  
  
    > [!IMPORTANT]  
    >  选择了“差异”  后，请验证是否清除了“仅复制备份”  复选框。  
  
6.  对于 **“备份组件”** ，请单击 **“数据库”** 。  
  
7.  可以接受 **“名称”** 文本框中建议的默认备份集名称，也可以为备份集输入其他名称。  
  
8.  或者，在 **“说明”** 文本框中，输入备份集的说明。  
  
9. 指定备份集的过期时间：  
  
    -   若要使备份集在特定天数后过期，请单击 **“之后”** （默认选项），并输入备份集从创建到过期所需的天数。 此值范围为 0 到 99999 天；0 天表示备份集将永不过期。  
  
         默认值在 **“服务器属性”** 对话框（位于 **“数据库设置”** 页上）的 **“默认备份媒体保持期(天)”** 选项中设置。 若要访问它，请在对象资源管理器中右键单击服务器名称，选择属性，再选择“数据库设置”  页。  
  
    -   若要使备份集在特定日期过期，请单击 **“在”** ，并输入备份集的过期日期。  
  
10. 通过单击 **“磁盘”** 或 **“磁带”** ，选择备份目标的类型。 若要选择包含单个介质集的多个磁盘或磁带机（最多为 64 个）的路径，请单击 **“添加”** 。 选择的路径将显示在 **“备份到”** 列表框中。  
  
     若要删除备份目标，请选择该备份目标并单击 **“删除”** 。 若要查看备份目标的内容，请选择该备份目标并单击 **“内容”** 。  
  
11. 若要查看或选择高级选项，请在 **“选择页”** 窗格中单击 **“选项”** 。  
  
12. 可以通过单击以下选项之一来选择 **“覆盖介质”** 选项：  
  
    -   **备份到现有介质集**  
  
         对于此选项，请单击 **“追加到现有备份集”** 或 **“覆盖所有现有备份集”** 。 或者，选中 **“检查介质集名称和备份集过期时间”** 复选框，并在 **“介质集名称”** 文本框中输入名称（可选）。 如果没有指定名称，将使用空白名称创建介质集。 如果指定了某个介质集名称，将检查该介质（磁带或磁盘）的实际名称是否与在此输入的名称相符。  
  
         如果将介质名称保留空白，并选中该框以便与介质进行核对，则只有当介质上的介质名称也是空白时才能成功。  
  
    -   **备份到新介质集并清除所有现有备份集**  
  
         对于该选项，请在 **“新建介质集名称”** 文本框中输入名称，并在 **“新建介质集说明”** 文本框中描述介质集（可选）。  
  
13. 或者，在 **“可靠性”** 部分中，选中：  
  
    -   **完成后验证备份**。  
  
    -   **“写入介质前检查校验和”** 和 **“出现校验和错误时继续”** （可选）。 有关校验和的信息，请参阅[在备份和还原期间可能的媒体错误 (SQL Server)](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)。  
  
14. 如果备份到磁带驱动器（如同 **“常规”** 页的 **“目标”** 部分指定的一样），则 **“备份后卸载磁带”** 选项处于活动状态。 单击此选项可以激活 **“卸载前倒带”** 选项。  
  
    > [!NOTE]  
    >  除非备份的是事务日志（如同“常规”  页的“备份类型”  部分中指定的一样），否则“事务日志”  部分中的选项处于不活动状态。  
  
15. [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] 及更高版本支持 [备份压缩](../../relational-databases/backup-restore/backup-compression-sql-server.md)。 默认情况下，是否压缩备份取决于 **backup-compression default** 服务器配置选项的值。 但是，不管当前服务器级默认设置如何，都可以通过选中 **“压缩备份”** 来压缩备份，并且可以通过选中 **“不压缩备份”** 来防止压缩备份。  
  
     **查看当前备份压缩默认值**  
  
    -   [查看或配置 backup compression default 服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  
  
    > [!NOTE]  
    >  另外，可以使用维护计划向导创建差异数据库备份。  
  
##  <a name="transact-sql"></a><a name="TsqlProcedure"></a> Transact-SQL  
  
#### <a name="create-a-differential-database-backup"></a>创建差异数据库备份  
  
1.  执行 BACKUP DATABASE 语句可以创建差异数据库备份，同时指定：  
  
    -   要备份的数据库的名称。  
  
    -   写入完整数据库备份的备份设备。  
  
    -   DIFFERENTIAL 子句，用于指定仅备份自上次创建完整数据库备份之后已更改的数据库部分。  
  
     要求语法为：  
  
     BACKUP DATABASE *database_name* TO <backup_device> WITH DIFFERENTIAL  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 以下示例为 `MyAdvWorks` 数据库创建完整数据库备份和差异数据库备份。  
  
```sql  
-- Create a full database backup first.  
BACKUP DATABASE MyAdvWorks   
   TO MyAdvWorks_1   
   WITH INIT;  
GO  
-- Time elapses.  
-- Create a differential database backup, appending the backup  
-- to the backup device containing the full database backup.  
BACKUP DATABASE MyAdvWorks  
   TO MyAdvWorks_1  
   WITH DIFFERENTIAL;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [差异备份 (SQL Server)](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)   
 [备份文件和文件组 (SQL Server)](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [还原差异数据库备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-differential-database-backup-sql-server.md)   
 [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)   
 [维护计划](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [完整文件备份 (SQL Server)](../../relational-databases/backup-restore/full-file-backups-sql-server.md)  
  
  
