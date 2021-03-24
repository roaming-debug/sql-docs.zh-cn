---
title: 通过备份和还原复制数据库 | Microsoft Docs
description: 在 SQL Server 中，可以通过还原使用某些以前版本创建的用户数据库备份来创建新数据库。
ms.custom: ''
ms.date: 07/15/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], back up and restore
- restoring databases [SQL Server], previous SQL Server versions
- database restores [SQL Server], copying databases
- restoring databases [SQL Server], onto another server instance
- restoring [SQL Server], onto another server instance
- backing up databases [SQL Server], copying databases
- database backups [SQL Server], copying databases
ms.assetid: b93e9701-72a0-408e-958c-dc196872c040
author: stevestein
ms.author: sstein
ms.openlocfilehash: 6be7ab989d06f807f33342bbe7e9978a23f809ad
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890646"
---
# <a name="copy-databases-with-backup-and-restore"></a>通过备份和还原来复制数据库
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中，可以通过还原使用 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本创建的用户数据库备份来创建新数据库。 但是， **无法还原使用** 早期版本创建的 **master** 、 **model** 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]备份。 此外，任何早期版本的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 都无法还原 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]备份。  
  
> [!IMPORTANT]
> SQL Server 2016 使用与早期版本不同的默认路径。 因此，若要还原在早期版本的默认位置中创建的数据库备份，必须使用 MOVE 选项。 有关新的默认路径的信息，请参阅 [SQL Server 的默认实例和命名实例的文件位置](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md)。 有关移动数据库文件的详细信息，请参阅本主题中后面的“移动数据库文件”。  
  
## <a name="general-steps-for-using-backup-and-restore-to-copy-a-database"></a>使用备份和还原复制数据库的一般步骤  
 使用备份和还原将数据库复制到其他 SQL Server 实例时，源计算机和目标计算机可以是运行 SQL Server 的任何平台。  
  
 一般步骤如下：  
  
1.  备份可能位于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本的实例上的源数据库。 运行此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机为“源计算机”  。  
  
2.  在要存放复制的数据库的计算机（“目标计算机”  ）上，连接到计划为其还原数据库的 SQL Server 实例。 如果需要，在“目标”  服务器实例上创建与“源”  数据库备份设备相同的设备。  
  
3.  在“目标”  计算机上还原“源”  数据库的备份。 还原数据库操作将自动创建所有数据库文件。  
  
一些可能影响此过程的其他注意事项：
  
## <a name="before-you-restore-database-files"></a>还原数据库文件之前  
 还原数据库操作将自动创建还原数据库所需的数据库文件。 默认情况下，在还原过程中 SQL Server 创建的文件的名称和路径与源计算机上原始数据库中备份文件的名称和路径相同。  
  
 或者，在还原数据库时，可以指定用于还原数据库的设备映射、文件名或路径。 
 
 这在下列情况下可能很有必要：  
  
-   在其他计算机上不存在原始计算机上的数据库使用的目录结构或驱动器映射。 例如，备份中可能包含一个默认要还原到驱动器 E 的文件，而目标计算机上没有驱动器 E。  
  
-   目标位置可能空间不足。  
  
-   您在重复使用在还原目标上存在的数据库名称并且其任何文件的名称与备份集中的某个数据库文件同名，则会发生以下情况之一：  
  
    -   如果可以覆盖现有的数据库文件，将覆盖该文件（这不会影响属于不同数据库名称的文件）。  
  
    -   如果无法覆盖现有文件，则会出现还原错误。  
  
 若要避免错误和令人不愉快的结果，在还原操作前，你可以使用 [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md) 历史记录表确定备份中你计划还原的数据库文件和日志文件。  
  
## <a name="moving-the-database-files"></a>移动数据库文件  
 如果无法将数据库备份中的文件还原到目标计算机上，则必须在还原这些文件时将它们移到新的位置。 例如：  
  
-   您想要通过在早期版本的默认位置创建的备份还原数据库。  
  
-   由于容量方面的原因，可能必须将备份中的一些数据库文件还原到其他驱动器上。 这种情况经常发生，因为组织中的大多数计算机的磁盘驱动器的数目和大小不同或软件配置不同。  
  
-   为了便于进行测试，可能必须在同一台计算机上创建现有数据库的副本。 在这种情况下，因为原始数据库的数据库文件已经存在，所以在还原操作过程中创建数据库副本时，必须指定其他文件名。  
  
 有关详细信息，请参阅本主题中后面的“将文件和文件组还原到新位置”。  
  
## <a name="changing-the-database-name"></a>更改数据库名称  
 在将数据库还原到目标计算机时可以更改其名称，而不必先还原数据库再手动更改其名称。 例如，可能有必要将数据库名称由 **Sales** 改为 **SalesCopy** 来表示这是数据库的一个副本。  
  
 还原数据库时，显式提供的数据库名称将自动用作新的数据库名称。 由于数据库名称尚不存在，因此需使用备份中的文件创建新的数据库名称。  
  
## <a name="when-upgrading-a-database-by-using-restore"></a>使用还原升级数据库时  
 从早期版本中还原备份时，提前了解目标计算机上是否存在备份中每个全文目录的路径（驱动器和目录）将很有帮助。 若要列出备份中每个文件（包括目录文件）的逻辑名称和物理名称、路径名和文件名，请使用 RESTORE FILELISTONLY FROM *<backup_device>* 语句。 有关详细信息，请参阅 [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)。  
  
 如果目标计算机上不存在相同的路径，则您可以执行下列两种操作：  
  
-   在目标计算机上创建相同的驱动器/目录映射。  
  
-   通过在 RESTORE DATABASE 语句中使用 WITH MOVE 子句，在还原操作过程中将目录文件移动到新位置。 有关详细信息，请参阅 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)备份。  
  
 有关升级全文检索的替代选项的信息，请参阅 [升级全文搜索](../../relational-databases/search/upgrade-full-text-search.md)。  
  
## <a name="database-ownership"></a>数据库所有权  
 在其他计算机上还原数据库时，启动还原操作的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录用户或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 用户将自动成为新数据库的所有者。 还原数据库时，系统管理员或新数据库所有者可以更改数据库所有权。 若要防止未经授权的数据库还原操作，请使用介质集或备份集密码。  
  
## <a name="managing-metadata-when-restoring-to-another-server-instance"></a>还原到另一个服务器实例时管理元数据  
 将数据库还原到另一个服务器实例上时，为了给用户和应用程序提供一致的体验，您可能需要在另一个服务器实例上为数据库重新创建部分或全部元数据（例如登录和作业）。 有关详细信息，请参阅 [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)。  
  
 **查看备份集中的数据文件和日志文件**  
  
-   [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
 **将文件和文件组还原到新位置**  
  
-   [将文件还原到新位置 (SQL Server)](../../relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
  
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
 **在现有文件上还原文件和文件组**  
  
-   [在现有文件上还原文件和文件组 (SQL Server)](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
 **用新名称还原数据库**  
  
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
 **重启中断的还原操作**  
  
-   [重新启动中断的还原操作 (Transact-SQL)](../../relational-databases/backup-restore/restart-an-interrupted-restore-operation-transact-sql.md)  
  
 **更改数据库所有者**  
  
-   [sp_changedbowner (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)  
  
 **使用 SQL Server 管理对象 (SMO) 复制数据库**  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReadFileList%2A>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.RelocateFiles%2A>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReplaceDatabase%2A>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore>  
  
## <a name="see-also"></a>另请参阅  
 [将数据库复制到其他服务器](../../relational-databases/databases/copy-databases-to-other-servers.md)   
 [SQL Server 的默认实例和命名实例的文件位置](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md)   
 [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
