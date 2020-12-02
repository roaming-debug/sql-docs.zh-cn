---
title: 备份设备 (SQL Server) | Microsoft Docs
description: 本文介绍 SQL Server 数据库的备份设备，包括术语和使用备份设备。
ms.custom: ''
ms.date: 08/12/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- tape backup devices, about tape backup devices
- backup devices [SQL Server]
- disk backup devices [SQL Server]
- database backups [SQL Server], backup devices
- logical devices [SQL Server]
- backup devices [SQL Server], about backup devices
- backing up [SQL Server], backup devices
- removable media [SQL Server]
- network shares [SQL Server]
- backups [SQL Server], backup devices
- tape backup devices
- physical devices [SQL Server]
- backing up databases [SQL Server], backup devices
- devices [SQL Server]
ms.assetid: 35a8e100-3ff2-4844-a5da-dd088c43cba4
author: cawrites
ms.author: chadam
ms.openlocfilehash: b0e264f1111ffc924466fdbb658b3f1e278d3f3a
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130511"
---
# <a name="backup-devices-sql-server"></a>备份设备 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库上执行备份操作期间，将备份的数据（“备份”  ）写入物理备份设备。 将介质集中的第一个备份写入物理备份设备时，便会初始化此备份设备。 包含一个或多个备份设备的集合的备份构成一个媒体集。  
   
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> 术语和定义  
 备份磁盘 (backup disk)  
 包含一个或多个备份文件的硬盘或其他磁盘存储介质。 备份文件是常规操作系统文件。  
  
 介质集 (media set)  
 备份介质（磁带或磁盘文件）的有序集合，它使用固定类型和数量的备份设备。 有关媒体集的信息，请参阅 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)。  
  
 物理备份设备 (physical backup device)  
 磁带机或操作系统提供的磁盘文件。 可以将备份数据写入 1 到 64 个备份设备。 如果备份数据需要多个备份设备，则所有设备必须对应于一种设备类型（磁盘或磁带）。  
  
 除了磁盘或磁带外，还可以将 SQL Server 备份写入 Azure Blob 存储服务。  
 
  
##  <a name="using-disk-backup-devices"></a><a name="DiskBackups"></a> 使用磁盘备份设备  
  
 如果在备份操作将备份数据追加到介质集时磁盘文件已满，则备份操作会失败。 备份文件的最大大小由磁盘设备上的可用磁盘空间决定，因此，备份磁盘设备的适当大小取决于备份数据的大小。  
  
 磁盘备份设备可以是简单的磁盘设备，如 ATA 驱动器。 或者，您可以使用热交换磁盘驱动器，它允许您将驱动器上的已满磁盘透明地替换为空磁盘。 备份磁盘可以是服务器上的本地磁盘，也可以是作为共享网络资源的远程磁盘。 有关如何使用远程磁盘的信息，请参阅本主题后面的 [备份到网络共享文件](#NetworkShare)。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理工具在处理磁盘备份设备时非常灵活，因为它们会自动生成标有时间戳的磁盘文件名称。  
  
> **重要说明！** 我们建议备份磁盘应不同于数据库数据和日志的磁盘。 这是数据或日志磁盘出现故障时访问备份数据必不可少的。 
>
>如果数据库文件和备份文件位于同一台设备上并且该设备出现故障，数据库和备份都将不可用。 此外，将数据库和备份文件放到不同的设备上还可以优化使用数据库和写入备份时的 I/O 性能。
  
##  <a name="specify-a-backup-file-using-its-physical-name-transact-sql"></a><a name="BackupFileUsingPhysicalName"></a> 使用物理名称指定备份文件 (Transact-SQL)  
 使用物理设备名称指定备份文件的基本 [BACKUP](../../t-sql/statements/backup-transact-sql.md) 语法为：  
  
 BACKUP DATABASE *database_name*  
  
 TO DISK **=** { **'** _physical_backup_device_name_ **'**  |  **@** _physical_backup_device_name_var_ }  
  
 例如：  
  
```sql  
BACKUP DATABASE AdventureWorks2012   
   TO DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak';  
GO  
```  
  
 若要在 [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) 语句中指定物理磁盘设备，基本语法为：  
  
 RESTORE { DATABASE | LOG } *database_name*  
  
 FROM DISK **=** { **'** _physical_backup_device_name_ **'**  |  **@** _physical_backup_device_name_var_ }  
  
 例如，  
  
```sql  
RESTORE DATABASE AdventureWorks2012   
   FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak';   
```  
  
  
##  <a name="specify-the-disk-backup-file-path"></a><a name="BackupFileDiskPath"></a> 指定磁盘备份文件路径 
 指定备份文件时，应输入其完整路径和文件名。 如果您在备份到文件时仅指定文件名或相对路径，则备份文件将存储到默认备份目录中。 默认备份目录为 C:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\Backup，其中 *n* 是服务器实例号。 因此，对于默认服务器实例，默认备份目录为：C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup。  
  
 为防止产生歧义，尤其是在脚本中，我们建议您在每个 DISK 子句中显式指定备份目录的路径。 但是，当您使用查询编辑器时这一点不再那么重要。 此时，如果您确定备份文件位于默认备份目录中，则可以省略 DISK 子句中的路径。 例如，下面的 `BACKUP` 语句将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库备份到默认的备份目录中。  
  
```sql  
BACKUP DATABASE AdventureWorks2012   
   TO DISK = 'AdventureWorks2012.bak';  
GO  
```  
  
> **注意：** 默认位置存储在 **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL.n\MSSQLServer** 下的 **BackupDirectory** 注册表项中。  
  
   
###  <a name="back-up-to-a-network-share-file"></a><a name="NetworkShare"></a> 备份到网络共享文件  
 要让 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 访问远程磁盘文件， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户必须有权访问网络共享。 这包括备份操作向网络共享中写入所需的权限以及还原操作从网络共享中读取所需的权限。 网络驱动器和权限的可用性取决于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务运行的环境：  
  
-   若要在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用域用户帐户运行时备份到网络驱动器，共享驱动器必须在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 运行的会话中作为网络驱动器进行映射。 如果是通过命令行启动 Sqlservr.exe 的，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以看到在登录会话中映射的所有网络驱动器。  
  
-   作为服务运行 Sqlservr.exe 时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在单独的会话中运行，该会话与登录会话无关。 运行服务的会话可以具有自己的映射驱动器（虽然它一般没有映射驱动器）。  
  
-   可以使用计算机帐户（而不是域用户）连接网络服务帐户。 若要允许从特定计算机备份到共享驱动器，请向计算机帐户授予访问权限。 只要写入备份的 Sqlservr.exe 进程具有访问权限，那么发送 BACKUP 命令的用户是否具有访问权限便会无关紧要。  
  
    > **重要说明！** 在网络上备份数据可能受网络错误的影响；因此，建议在使用远程磁盘时，完成备份后验证备份操作。 有关详细信息，请参阅 [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)。  
  
## <a name="specify-a-universal-naming-convention-unc-name"></a>指定通用命名约定 (UNC) 名称  
 若要在备份或还原命令中指定网络共享，应针对备份设备使用文件的完全限定通用命名约定 (UNC) 名称。 UNC 名称采用以下格式： **\\\\** _系统名称_ **\\** _共享名称_ **\\** _路径_ **\\** _文件名_。  
  
 例如：  
  
```sql  
BACKUP DATABASE AdventureWorks2012   
   TO DISK = '\\BackupSystem\BackupDisk1\AW_backups\AdventureWorksData.Bak';  
GO  
```  
  
 
##  <a name="using-tape-devices"></a><a name="TapeDevices"></a> 使用磁带设备  
  
> **注意：** 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的未来版本中将不再支持磁带备份设备。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。  
   
 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据备份到磁带时要求 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 操作系统支持一个或多个磁带机。 另外，对于给定的磁带机，我们建议您仅使用磁带机制造商推荐的磁带。 有关如何安装磁带机的详细信息，请参阅 Windows 操作系统的文档。  
  
 在使用磁带机时，备份操作可能会写满一个磁带，并继续在另一个磁带上进行。 每个磁带包含一个介质标头。 使用的第一个介质称为“起始磁带  ”。 每个后续磁带称为“延续磁带”  ，其介质序列号比前一磁带的介质序列号大一。 例如，与四个磁带设备相关联的介质集至少含有四个起始磁带（如果该数据库过大，还会有四个系列的延续磁带）。 在追加备份集时，必须在序列中装入最后一个磁带。 如果没有装入最后一个磁带， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将向前扫描到已装入磁带的末尾，然后要求更换磁带。 此时，请装入最后一个磁带。  
  
 磁带备份设备的用法类似于磁盘设备，但下述情况例外：  
  
-   磁带设备必须物理连接到运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的计算机上。 不支持备份到远程磁带设备上。  
  
-   如果磁带备份设备在备份操作过程中已满，但还必须写入一些数据，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将提示更换新磁带并在加载新磁带后继续备份操作。  
  
##  <a name="specify-a-backup-tape-using-its-physical-name-transact-sql"></a><a name="BackupTapeUsingPhysicalName"></a> 使用物理名称指定备份磁带 (Transact-SQL)  
 使用磁带机物理设备名称指定备份磁带的基本 [BACKUP](../../t-sql/statements/backup-transact-sql.md) 语法为：  
  
 BACKUP { DATABASE | LOG } *database_name*  
  
 TO TAPE **=** { **'** _physical_backup_device_name_ **'**  |  **@** _physical_backup_device_name_var_ }  
  
 例如：  
  
```sql  
BACKUP LOG AdventureWorks2012   
   TO TAPE = '\\.\tape0';  
GO  
```  
  
 若要在 [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) 语句中指定物理磁带设备，基本语法为：  
  
 RESTORE { DATABASE | LOG } *database_name*  
  
 FROM TAPE **=** { **'** _physical_backup_device_name_ **'**  |  **@** _physical_backup_device_name_var_ }  
  
###  <a name="tape-specific-backup-and-restore-options-transact-sql"></a><a name="TapeOptions"></a> 磁带专用的 BACKUP 选项和 RESTORE 选项 (Transact-SQL)  
 为了便于磁带管理，BACKUP 语句提供了下列磁带专用的选项：  
  
-   { NOUNLOAD | **UNLOAD** }  
  
     您可以控制在备份或还原操作后备份磁带是否自动从磁带机卸载。 UNLOAD/NOUNLOAD 这一会话设置可在整个会话期间存在，或者在通过指定其他设置而进行重置之前一直存在。  
  
-   { **REWIND** | NOREWIND }  
  
     您可以控制在备份或还原操作后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 是保持磁带处于打开状态，还是在磁带满后释放磁带并倒带。 默认行为是倒带 (REWIND)。  
  
> **注意：** 有关 BACKUP 语法和参数的详细信息，请参阅 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)。 有关 RESTORE 语法和参数的详细信息，请分别参阅 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md) 和 [RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)。  
  
###  <a name="managing-open-tapes"></a><a name="OpenTapes"></a> 管理打开的磁带  
 若要查看打开的磁带设备列表以及装入请求状态，请查询 [sys.dm_io_backup_tapes](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md) 动态管理视图。 此视图显示了所有打开的磁带。 其中包括正在使用的磁带，它们等待下一个 BACKUP 或 RESTORE 操作时暂时处于空闲状态。  
  
 如果意外使磁带处于打开状态，则释放磁带的最快方式是使用以下命令：RESTORE REWINDONLY FROM TAPE **=** _backup_device_name_。 有关详细信息，请参阅 [RESTORE REWINDONLY (Transact-SQL)](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)。  
  
  
## <a name="using-the-azure-blob-storage-service"></a>使用 Azure Blob 存储服务  
 可以将 SQL Server 备份写入 Azure Blob 存储服务。  有关如何将 Azure Blob 存储服务用于你的备份的详细信息，请参阅[使用 Microsoft Azure Blob 存储服务进行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)。  
  
##  <a name="use-a-logical-backup-device"></a><a name="LogicalBackupDevice"></a> 使用逻辑备份设备  
 “逻辑备份设备  ”是指向特定物理备份设备（磁盘文件或磁带机）的可选用户定义名称。 通过逻辑备份设备，您可以在引用相应的物理备份设备时使用间接寻址。  
  
 定义逻辑备份设备涉及为物理设备分配逻辑名称。 例如，逻辑设备 AdventureWorksBackups 可能被定义为指向 Z:\SQLServerBackups\AdventureWorks2012.bak 文件或 \\\\.\tape0 磁带驱动器。 备份和还原命令随后可以将 AdventureWorksBackups 指定为备份设备，而不是指定 DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak' 或 TAPE = '\\\\.\tape0'。  
  
 逻辑设备名称在服务器实例上的所有逻辑备份设备中必须是唯一的。 若要查看现有逻辑设备名称，请查询 [sys.backup_devices](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md) 目录视图。 此视图显示每个逻辑备份设备的名称，并说明了相应物理备份设备的类型、物理文件名或路径。  
  
 定义逻辑备份设备后，您可以在 BACKUP 或 RESTORE 命令中指定此逻辑备份设备而不是设备的物理名称。 例如，下面的语句将 `AdventureWorks2012` 数据库备份到 `AdventureWorksBackups` 逻辑备份设备。  
  
```sql  
BACKUP DATABASE AdventureWorks2012   
   TO AdventureWorksBackups;  
GO  
```  
  
> **注意：** 在给定的 BACKUP 或 RESTORE 语句中，逻辑备份设备名称和相应的物理备份设备名称可以互换。  
  
 使用逻辑备份设备的一个优点是比使用长路径简单。 如果准备将一系列备份数据写入相同的路径或磁带设备，则使用逻辑备份设备非常有用。 逻辑备份设备对于标识磁带备份设备尤为有用。  
  
 可以编写一个备份脚本以使用特定逻辑备份设备。 这样，您无需更新脚本即可切换到新的物理备份设备。 切换涉及以下过程：  
  
1.  删除原来的逻辑备份设备。  
  
2.  定义新的逻辑备份设备，新设备使用原来的逻辑设备名称，但映射到不同的物理备份设备。 逻辑备份设备对于标识磁带备份设备尤为有用。  

##  <a name="mirrored-backup-media-sets"></a><a name="MirroredMediaSets"></a> 镜像备份媒体集  
 镜像备份介质集可减小备份设备故障的影响。 由于备份是防止数据丢失的最后防线，因此备份设备出现故障的后果是非常严重的。 随着数据库不断增大，备份设备或介质发生故障致使备份不可还原的可能性也相应增加。 镜像备份介质通过提供物理备份设备冗余来提高备份的可靠性。 有关详细信息，请参阅本主题后面的 [镜像备份媒体集 (SQL Server)](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md)不熟悉的读者。  
  
> **注意：** 只有 [!INCLUDE[ssEnterpriseEd2005](../../includes/ssenterpriseed2005-md.md)] 和更高版本支持镜像备份介质集。  
  
  
##  <a name="archive-sql-server-backups"></a><a name="Archiving"></a> 存档 SQL Server 备份  
 建议您使用文件系统备份实用工具对磁盘备份数据进行存档，并将存档存储在另一个位置。 使用磁盘的优点是您可以使用网络将已存档的备份数据写入另一个位置的磁盘。 Azure Blob 存储服务可作为站外存档选项。  可以上载磁盘备份，或直接将备份写入 Azure Blob 存储服务。  
  
 另一个常用的存档方法是将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份写入本地备份磁盘，将备份存档到磁带，然后在将磁带存储在站外位置。  

  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
 **指定磁盘设备 (SQL Server Management Studio)**  
  
-   [将磁盘或磁带指定为备份目标 (SQL Server)](../../relational-databases/backup-restore/specify-a-disk-or-tape-as-a-backup-destination-sql-server.md)  
  
 **指定磁带设备 (SQL Server Management Studio)**  
  
-   [将磁盘或磁带指定为备份目标 (SQL Server)](../../relational-databases/backup-restore/specify-a-disk-or-tape-as-a-backup-destination-sql-server.md)  
  
 **定义逻辑备份设备**  
  
-   [sp_addumpdevice (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)  
  
-   [为磁盘文件定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)  
  
-   [为磁带驱动器定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.BackupDevice> (SMO)  
  
 **使用逻辑备份设备**  
  
-   [将磁盘或磁带指定为备份目标 (SQL Server)](../../relational-databases/backup-restore/specify-a-disk-or-tape-as-a-backup-destination-sql-server.md)  
  
-   [从设备还原备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-backup-from-a-device-sql-server.md)  
  
-   [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 **查看有关备份设备的信息**  
  
-   [备份历史记录和标头信息 (SQL Server)](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
-   [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   [查看备份磁带或文件的内容 (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
 **删除逻辑备份设备**  
  
-   [sp_dropdevice (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md)  
  
-   [删除备份设备 (SQL Server)](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Backup Device 对象](../../relational-databases/performance-monitor/sql-server-backup-device-object.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [维护计划](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE LABELONLY (Transact-SQL)](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)   
 [sys.backup_devices (Transact-SQL)](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)   
 [sys.dm_io_backup_tapes (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md)   
 [镜像备份媒体集 (SQL Server)](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md)  
  
  
