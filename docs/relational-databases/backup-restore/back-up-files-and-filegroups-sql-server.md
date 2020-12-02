---
title: 备份文件和文件组 | Microsoft Docs
description: 本文介绍如何使用 SQL Server Management Studio、Transact-SQL 或 PowerShell 在 SQL Server 中备份文件和文件组。
ms.custom: ''
ms.date: 08/03/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backing up filegroups [SQL Server]
- file backups [SQL Server], how-to topics
- backing up files [SQL Server]
- backups [SQL Server], creating
- filegroups [SQL Server], backing up
ms.assetid: a0d3a567-7d8b-4cfe-a505-d197b9a51f70
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6b400efdcf437fd97952ae215f5279d770c0598a
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130560"
---
# <a name="back-up-files-and-filegroups"></a>备份文件和文件组
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或 PowerShell 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中备份文件和文件组。 当数据库大小和性能要求使完整数据库备份显得不切实际，则可以创建文件备份。 文件备份  包含一个或多个文件（或文件组）中的所有数据。
  
有关文件备份的详细信息，请参阅 [完整文件备份 (SQL Server)](../../relational-databases/backup-restore/full-file-backups-sql-server.md) 和 [差异备份 (SQL Server)](../../relational-databases/backup-restore/differential-backups-sql-server.md)。  

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
- 不允许在显式或隐式事务中使用 BACKUP 语句。  
  
- 在简单恢复模式下，必须一起备份所有读/写文件。 这有助于确保将数据库还原到一致的时点。 请使用 READ_WRITE_FILEGROUPS 选项，而不是逐个指定每个读/写文件或文件组。 此选项用于备份数据库中的所有读/写文件组。 通过指定 READ_WRITE_FILEGROUPS 创建的备份称为部分备份，请参阅[部分备份 &#40;SQL Server&#41;](../../relational-databases/backup-restore/partial-backups-sql-server.md)  。  
  
有关限制的详细信息，请参阅 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议
  
默认情况下，每个成功的备份操作都会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志和系统事件日志中添加一个条目。 如果非常频繁地备份日志，这些成功消息会迅速累积，从而产生一个巨大的错误日志，这样会使查找其他消息变得非常困难。 在这些情况下，如果任何脚本均不依赖于这些日志条目，则可以使用跟踪标志 3226 取消这些条目，请参阅 [跟踪标志 &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。  

###  <a name="permissions"></a><a name="Permissions"></a> 权限

默认情况下，为 sysadmin 固定服务器角色以及 db_owner 和 db_backupoperator 固定数据库角色的成员授予 `BACKUP DATABASE` 和 `BACKUP LOG` 权限    。  
  
 备份设备的物理文件的所有权和权限问题可能会妨碍备份操作。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须能够读取和写入设备；运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的帐户必须具有写入权限。 但是，用于在系统表中为备份设备添加项目的 [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)不检查文件访问权限。 备份设备物理文件的这些问题可能直到为备份或还原而访问物理资源时才会出现。

## <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio   
  
1. 连接到相应的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]实例之后，在对象资源管理器中，单击服务器名称以展开服务器树。  
  
1. 展开 **“数据库”** ，然后根据数据库的不同，选择用户数据库，或展开 **“系统数据库”** ，再选择系统数据库。  
  
1. 右键单击数据库，指向 **“任务”** ，再单击 **“备份”** 。 将出现 **“备份数据库”** 对话框。  
  
1. 在 **“数据库”** 列表中，验证数据库名称。 您也可以从列表中选择其他数据库。  
  
1. 在 **“备份类型”** 列表中，选择 **“完整”** 或 **“差异”** 。  
  
1. 对于 **“备份组件”** 选项，单击 **“文件和文件组”** 。  
  
1. 在 **“选择文件和文件组”** 对话框中，选择要备份的文件和文件组。 可以选择一个或多个单个文件，也可以复选文件组的框，从而自动选择该文件组中的所有文件。  
  
1. 可以接受 **“名称”** 文本框中建议的默认备份集名称，也可以为备份集输入其他名称。  
  
1. （可选）在“说明”文本框中，输入备份集的说明  。  
  
1. 指定备份集的过期时间：  
  
    - 若要使备份集在特定天数后过期，请单击“之后”  （默认选项），并输入备份集从创建到过期所需的天数。 此值范围为 0 到 99999 天；0 天表示备份集将永不过期。  
  
         默认值在 **“服务器属性”** 对话框（位于 **“数据库设置”** 页上）的 **“默认备份媒体保持期(天)”** 选项中设置。 若要访问此选项，请在对象资源管理器中右键单击服务器名称，并选择“属性”，然后选择“数据库设置”  页。  
  
    - 若要使备份集在特定日期过期，请单击 **“在”** ，并输入备份集的过期日期。  
  
1. 通过单击 **“磁盘”** 或 **“磁带”** ，选择备份目标的类型。 若要选择包含单个介质集的多个磁盘驱动器或磁带机（最多为 64 个）的路径，请单击 **“添加”** 。 所选路径将显示在 **“备份到”** 列表中。  
  
    > [!NOTE]
    > 若要删除备份目标，请选择该备份目标并单击 **“删除”** 。 若要查看备份目标的内容，请选择该备份目标并单击 **“内容”** 。  
  
1. 若要查看或选择高级选项，请在 **“选择页”** 窗格中单击 **“选项”** 。  
  
1. 可以通过单击以下选项之一来选择 **“覆盖介质”** 选项：  
  
    - **备份到现有介质集**  
  
         对于此选项，请单击 **“追加到现有备份集”** 或 **“覆盖所有现有备份集”** 。 
         
         有关备份到现有媒体集的信息，请参阅[媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)。  
  
         - （可选）选择“检查介质集名称和备份集的过期日期”  ，以使备份操作对媒体集和备份集的过期日期和时间进行验证。  
  
         - （可选）在“介质集名称”文本框中输入名称  。 如果没有指定名称，将使用空白名称创建介质集。 如果指定了介质集名称，将检查介质（磁带或磁盘），以确定实际名称是否与此处输入的名称匹配。  
  
         如果将介质名称保留空白，并选中该框以便与介质进行核对，则只有当介质上的介质名称也是空白时才能成功。  
  
    - **备份到新介质集并清除所有现有备份集**  
  
         对于该选项，请在 **“新建介质集名称”** 文本框中输入名称，并在 **“新建介质集说明”** 文本框中描述介质集（可选）。
         
         有关创建新媒体集的详细信息，请参阅[媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)。  
  
1. （可选）在“可靠性”部分中，选中  ：  
  
    - **完成后验证备份**。  
  
    - “写入媒体前检查校验和”和“出现校验和错误时继续”（可选）   。
    
         有关校验和的详细信息，请参阅[在备份和还原期间可能的媒体错误 (SQL Server)](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)。  
  
1. 如果备份到磁带驱动器（如同 **“常规”** 页的 **“目标”** 部分指定的一样），则 **“备份后卸载磁带”** 选项处于活动状态。 单击此选项可以启用 **“卸载前倒带”** 选项。  
  
    > [!NOTE]
    > 除非备份的是事务日志（如同“常规”  页的“备份类型”  部分中指定的一样），否则“事务日志”  部分中的选项处于不活动状态。  
  
1. [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] 及更高版本支持 [备份压缩](../../relational-databases/backup-restore/backup-compression-sql-server.md)。 默认情况下，是否压缩备份取决于 **backup-compression default** 服务器配置选项的值。 但是，不管当前服务器级默认设置如何，都可以通过选中 **“压缩备份”** 来压缩备份，并且可以通过选中 **“不压缩备份”** 来防止压缩备份。  
  
     如需查看当前备份压缩默认值，请参阅[查看或配置备份压缩默认值服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  

## <a name="using-transact-sql"></a>“使用 Transact-SQL”
  
若要创建文件或文件组备份，请使用 [BACKUP DATABASE <file_or_filegroup>](../../t-sql/statements/backup-transact-sql.md) 语句。 此语句至少必须指定以下各项：  
  
  - 数据库名称。  
  
  - FILE 或 FILEGROUP 子句（为每个文件或文件组分别指定）。  
  
  - 将写入完整备份的备份设备。  
  
用于文件备份的基本 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法如下：  
  
  BACKUP DATABASE *database*  
  
  { FILE _=_ logical_file_name  | FILEGROUP _=_ logical_filegroup_name  } [ **,** ...f  ]  
  
  TO backup_device  [ **,** ...*n* ]  
  
  [ WITH with_options  [ **,** ...*o* ] ] ;  
  
|选项|说明|  
|------------|-----------------|  
|*database*|备份事务日志、部分数据库或完整的数据库时所用的源数据库。|  
|FILE _=_ logical_file_name |指定要包含在文件备份中的文件的逻辑名称。|  
|FILEGROUP _=_ logical_filegroup_name |指定要包含在文件备份中的文件组的逻辑名称。 在简单恢复模式下，只允许对只读文件组执行文件组备份。|  
|[ **,** ...*f* ]|表示可以指定多个文件和文件组的占位符。 不限制文件或文件组的数量。|  
|*backup_device* [ **,** ...*n* ]|指定一个列表，它包含 1 至 64 个用于备份操作的备份设备。 您可以指定物理备份设备，也可以指定对应的逻辑备份设备（如果已定义）。 若要指定物理备份设备，请使用 DISK 或 TAPE 选项：<br /><br /> { DISK &#124; TAPE } _=_ *physical_backup_device_name*<br /><br /> 有关详细信息，请参阅 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。|  
|WITH with_options  [ **,** ...*o* ]|您也可以指定一个或多个附加选项，如 DIFFERENTIAL。 差异文件备份需要以完整文件备份为基础。 <br /><br />有关详细信息，请参阅[创建差异数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)。|  
  
在完整恢复模式下，还必须备份事务日志。 若要使用一整套文件的完整备份来还原数据库，您还必须拥有足够的日志备份，以便涵盖从第一个文件备份开始的所有文件备份。

有关详细信息，请参阅 [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)数据库还原到一个新位置并且可以选择重命名该数据库。  
  
###  <a name="examples"></a><a name="TsqlExample"></a> 示例
下面的示例备份了 `Sales` 数据库的辅助文件组的一个或多个文件。 此数据库使用完整恢复模式并且包含以下辅助文件组：  
  
- 名为 `SalesGroup1` 的文件组，它包含文件 `SGrp1Fi1` 和 `SGrp1Fi2`。  
  
- 名为 `SalesGroup2` 的文件组，它包含文件 `SGrp2Fi1` 和 `SGrp2Fi2`。  
  
#### <a name="a-create-a-file-backup-of-two-files"></a>A. 为两个文件创建文件备份  
下面的示例仅为 `SGrp1Fi2` 文件组的 `SalesGroup1` 文件和 `SGrp2Fi2` 文件组的 `SalesGroup2` 文件创建差异文件备份。  
  
```sql  
--Backup the files in the SalesGroup1 secondary filegroup.  
BACKUP DATABASE Sales  
   FILE = 'SGrp1Fi2',   
   FILE = 'SGrp2Fi2'   
   TO DISK = 'G:\SQL Server Backups\Sales\SalesGroup1.bck';  
GO  
```  
  
#### <a name="b-create-a-full-file-backup-of-the-secondary-filegroups"></a>B. 创建辅助文件组的完整文件备份  
下面的示例将对两个辅助文件组中的各个文件创建完整文件备份。  
  
```sql  
--Back up the files in SalesGroup1.  
BACKUP DATABASE Sales  
   FILEGROUP = 'SalesGroup1',  
   FILEGROUP = 'SalesGroup2'  
   TO DISK = 'C:\MySQLServer\Backups\Sales\SalesFiles.bck';  
GO  
```  
  
#### <a name="c-create-a-differential-file-backup-of-the-secondary-filegroups"></a>C. 创建辅助文件组的差异文件备份  
下面的示例将对两个辅助文件组中的各个文件创建差异文件备份。  
  
```sql  
--Back up the files in SalesGroup1.  
BACKUP DATABASE Sales  
   FILEGROUP = 'SalesGroup1',  
   FILEGROUP = 'SalesGroup2'  
   TO DISK = 'C:\MySQLServer\Backups\Sales\SalesFiles.bck'  
   WITH   
      DIFFERENTIAL;  
GO  
```  
  
## <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell

设置和使用 [SQL Server PowerShell 提供程序](../../powershell/sql-server-powershell-provider.md)。
  
使用 **Backup-SqlDatabase** cmdlet 并为 **-BackupAction** 参数的值指定 **Files** 。 此外，还指定下列参数之一：  
  
- 若要备份某个特定文件，请指定 _-DatabaseFile_ String  参数，其中 String  是要备份的一个或多个数据库文件。  
  
- 若要备份某个给定文件组中的所有文件，请指定 _-DatabaseFileGroup_ String  参数，其中 String  是要备份的一个或多个数据库文件组。  
  
下面的示例在 `<myDatabase>` 数据库中创建辅助文件组“FileGroup1”和“FileGroup2”中的每个文件的完整文件备份。 将在服务器实例 `Computer\Instance`的默认备份位置上创建备份。  

```powershell
Backup-SqlDatabase -ServerInstance Computer\Instance -Database <myDatabase> -BackupAction Files -DatabaseFileGroup "FileGroup1","FileGroup2" 
```
  
## <a name="see-also"></a>另请参阅  
 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [备份历史记录和标头信息 (SQL Server)](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)   
 [备份数据库（“常规”页）](../../relational-databases/backup-restore/back-up-database-general-page.md)   
 [备份数据库（“备份选项”页）](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)   
 [完整文件备份 (SQL Server)](../../relational-databases/backup-restore/full-file-backups-sql-server.md)   
 [差异备份 (SQL Server)](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [文件还原（完整恢复模式）](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [文件还原（简单恢复模式）](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)