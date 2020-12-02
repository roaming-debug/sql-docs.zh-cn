---
title: 备份事务日志 | Microsoft Docs
description: 本文介绍如何使用 SQL Server Management Studio、Transact-SQL 或 PowerShell 在 SQL Server 中备份事务日志。
ms.custom: ''
ms.date: 02/02/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- transaction log backups [SQL Server], SQL Server Management Studio
- backups [SQL Server], creating
- backing up transaction logs [SQL Server], SQL Server Management Studio
ms.assetid: 3426b5eb-6327-4c7f-88aa-37030be69fbf
author: cawrites
ms.author: chadam
ms.openlocfilehash: 57c864996d15886eb69ef7caf0f000349b238b9c
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130579"
---
# <a name="back-up-a-transaction-log"></a>备份事务日志
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或 PowerShell 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中备份事务日志。  

## <a name="before-you-begin"></a>开始之前
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
不允许在显式或[隐式](../../t-sql/statements/set-implicit-transactions-transact-sql.md)事务中使用 `BACKUP` 语句。 显式事务就是可以显式地在其中定义事务的开始和结束的事务。

### <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
- 如果数据库使用完整[恢复模式](recovery-models-sql-server.md)或大容量日志恢复模式，则必须足够频繁地备份事务日志，以保护数据和避免[事务日志变满](../logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)。 这将截断日志，并且支持将数据库还原到特定时间点。 
  
- 默认情况下，每个成功的备份操作都会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志和系统事件日志中添加一个条目。 如果频繁地备份日志，这些成功消息会迅速累积，从而产生巨大的错误日志，这样会使查找其他消息变得非常困难。 在这些情况下，如果任何脚本均不依赖于这些日志条目，则可以使用跟踪标志 3226 取消这些条目，请参阅 [跟踪标志 &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。  
  
### <a name="permissions"></a><a name="Permissions"></a> 权限

默认情况下，为 sysadmin 固定服务器角色以及 db_owner 和 db_backupoperator 固定数据库角色的成员授予所需的 `BACKUP DATABASE` 和 `BACKUP LOG` 权限    。 开始操作前，先检查是否有正确的权限。
  
 备份设备的物理文件的所有权和权限问题可能会妨碍备份操作。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须能够读取和写入设备；运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的帐户必须具有写入权限。 但是，用于在系统表中为备份设备添加项目的 [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)不检查文件访问权限。 在因尝试备份或还原而访问[物理资源](backup-devices-sql-server.md) 之前，备份设备物理文件中的权限问题可能并不明显。 因此再次强调，开始操作前，先检查权限。

## <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio

1. 连接到相应的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]实例之后，在对象资源管理器中，单击服务器名称以展开服务器树。  
  
1. 展开 **“数据库”** ，然后根据数据库的不同，选择用户数据库，或展开 **“系统数据库”** ，再选择系统数据库。  
  
1. 右键单击数据库，指向 **“任务”** ，再单击 **“备份”** 。 将出现 **“备份数据库”** 对话框。  
 
1. 在 **“数据库”** 列表框中，验证数据库名称。 您也可以从列表中选择其他数据库。  
  
1. 验证恢复模式是 **FULL** 还是 **BULK_LOGGED**。  
  
1. 在 **“备份类型”** 列表框中，选择 **“事务日志”** 。  
  
1. （可选）选择“仅复制备份”创建仅复制备份  。 “仅复制备份”是[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 独立于常规备份序列的[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]备份，请参阅[仅复制备份 &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)  。  
  
    > [!NOTE]
    > 选择“差异”  选项时，无法创建仅复制备份。  
  
1. 可以接受 **“名称”** 文本框中建议的默认备份集名称，也可以为备份集输入其他名称。  
  
1. （可选）在“说明”文本框中，输入备份集的说明  。  
  
1. 指定备份集的过期时间：  
  
    - 若要使备份集在特定天数后过期，请单击 **“之后”** （默认选项），并输入备份集从创建到过期所需的天数。 此值范围为 0 到 99999 天；0 天表示备份集将永不过期。  
  
         默认值在 **“服务器属性”** 对话框（位于 **“数据库设置”** 页上）的 **“默认备份媒体保持期(天)”** 选项中设置。 若要访问此对话框，请在对象资源管理器中右键单击服务器名称，选择“属性”，再选择 **“数据库设置”** 页。  
  
    - 若要使备份集在特定日期过期，请单击 **“在”** ，并输入备份集的过期日期。  
  
1. 通过单击 **“磁盘”** 、 **“URL”** 或 **“磁带”** ，选择备份目标的类型。 若要选择包含单个介质集的多个磁盘或磁带机（最多为 64 个）的路径，请单击 **“添加”** 。 选择的路径将显示在 **“备份到”** 列表框中。  
  
     若要删除备份目标，请选择该备份目标并单击 **“删除”** 。 若要查看备份目标的内容，请选择该备份目标并单击 **“内容”** 。  
  
1. 若要查看或选择高级选项，请在 **“选择页”** 窗格中单击 **“选项”** 。  
  
1. 可以通过单击以下选项之一来选择 **“覆盖介质”** 选项：  
  
    - **备份到现有介质集**  
  
         对于此选项，请单击“追加到现有备份集”或“覆盖所有现有备份集”，请参阅[媒体集、媒体系列和备份集 &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   。  
  
         - （可选）选择“检查介质集名称和备份集的过期日期”  ，以使备份操作对媒体集和备份集的过期日期和时间进行验证。  
  
         - （可选）在“介质集名称”文本框中输入名称  。 如果没有指定名称，将使用空白名称创建介质集。 如果指定了介质集名称，将检查介质（磁带或磁盘），以确定实际名称是否与此处输入的名称匹配。  
  
         如果将介质名称保留空白，并选中该框以便与介质进行核对，则只有当介质上的介质名称也是空白时才能成功。  
  
    - **备份到新介质集并清除所有现有备份集**  
  
         对于该选项，请在“新建介质集名称”文本框中输入名称，并在“新建介质集说明”文本框中描述介质集（可选），请参阅[媒体集、媒体系列和备份集 &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   。  
  
1. 或者，在 **“可靠性”** 部分中，选中：  
  
    - **完成后验证备份**。  
  
    - “写入介质前检查校验”和“出现校验和错误时继续”（可选）   。
    
       有关校验和的信息，请参阅[在备份和还原期间可能的媒体错误 (SQL Server)](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)。  
  
1. 在 **“事务日志”** 区域中：  
  
    - 对于例行的日志备份，请保留默认选项 **“通过删除不活动的条目截断事务日志”** 。  
  
    - 若要备份日志尾部（即活动的日志），请选中“备份日志尾部，并使数据库处于还原状态”  。  
  
         备份日志尾部失败后执行结尾日志备份，以防丢失所做的工作。 在失败之后且在开始还原数据库之前，或者在故障转移到辅助数据库时，备份活动日志（结尾日志备份）。 选择此选项等效于在 Transact-SQL BACKUP LOG 语句中指定 NORECOVERY 选项。
         
         有关结尾日志备份的详细信息，请参阅[结尾日志备份 (SQL Server) ](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)。  
  
1. 如果备份到磁带驱动器（如同 **“常规”** 页的 **“目标”** 部分指定的一样），则 **“备份后卸载磁带”** 选项处于活动状态。 单击此选项可以激活 **“卸载前倒带”** 选项。  
  
1. [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] 及更高版本支持 [备份压缩](../../relational-databases/backup-restore/backup-compression-sql-server.md)。 默认情况下，是否压缩备份取决于 **backup-compression default** 服务器配置选项的值。 但是，不管当前服务器级默认设置如何，都可以通过选中 **“压缩备份”** 来压缩备份，并且可以通过选中 **“不压缩备份”** 来防止压缩备份。  
  
     若要查看当前备份压缩默认值，请参阅[查看或配置备份压缩默认值服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)。
  
     若要加密备份文件，请选中 **“加密备份”** 复选框。 选择要用于加密备份文件的加密算法，并提供一个证书或非对称密钥。 可用于加密的算法是：  
  
   - AES 128  
  
   - AES 192  
  
   - AES 256  
  
   - 三重 DES  

## <a name="using-transact-sql"></a>“使用 Transact-SQL”  
  
执行 BACKUP LOG 语句以备份事务日志，同时指定下列对象：  
  
- 要备份的事务日志所属的数据库的名称。  
  
- 写入事务日志备份的备份设备。  
  
> [!IMPORTANT]
> 此示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库，该数据库使用简单恢复模式。 若要允许日志备份，请在完整备份数据库之前，将数据库设置为使用完整恢复模式。
>
> 有关详细信息，请参阅[查看或更改数据库的恢复模式 (SQL Server)](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md)。  
  
 以下示例将在以前创建的已命名备份设备 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 上创建 `MyAdvWorks_FullRM_log1`数据库的事务日志备份。  
  
```sql  
BACKUP LOG AdventureWorks2012  
   TO MyAdvWorks_FullRM_log1;  
GO  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell

设置和使用 [SQL Server PowerShell 提供程序](../../powershell/sql-server-powershell-provider.md)。 使用 **Backup-SqlDatabase** cmdlet 并为 **-BackupAction** 参数的值指定 **Log** 。  
  
下面的示例在服务器实例 `<myDatabase>` 的默认备份位置创建数据库 `Computer\Instance`的日志备份。  
  
```powershell
Backup-SqlDatabase -ServerInstance Computer\Instance -Database <myDatabase> -BackupAction Log  
```
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
  
- [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
- [将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
  
- [解决事务日志已满的问题（SQL Server 错误 9002）](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)  
  
## <a name="see-also"></a>另请参阅

 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [维护计划](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [完整文件备份 (SQL Server)](../../relational-databases/backup-restore/full-file-backups-sql-server.md)