---
title: 从 Microsoft Azure 中存储的备份还原 | Microsoft Docs
description: 了解在使用存储在 Azure Blob 存储中的备份还原 SQL Server 数据库时的注意事项。
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 6ae358b2-6f6f-46e0-a7c8-f9ac6ce79a0e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5329181957e09e169caae74f9611a2426f377f7b
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125506"
---
# <a name="restoring-from-backups-stored-in-microsoft-azure"></a>从 Microsoft Azure 中存储的备份还原
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题概述在使用存储在 Azure Blob 存储服务中的备份还原数据库时的注意事项。 这适用于使用“SQL Server 备份到 URL”备份或由 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]创建的备份。  
  
 如果在 Azure Blob 存储服务中存储了要还原的备份，则我们建议查看本主题，然后查看介绍关于如何还原数据库的步骤的主题，本地和 Azure 备份的还原步骤相同。  
  
## <a name="overview"></a>概述  
 用于从本地备份还原数据库的工具和方法也适用于从云备份还原数据库。  以下各节介绍了这些注意事项，以及在使用 Azure Blob 存储服务存储的备份时应当了解的所有差异。  
  
### <a name="using-transact-sql"></a>“使用 Transact-SQL”  
  
-   因为 SQL Server 必须连接外部源才能检索备份文件，所以会使用 SQL 凭据来对存储帐户进行身份验证。 因此，RESTORE 语句需要有 WITH CREDENTIAL 选项。 有关详细信息，请参阅[使用 Microsoft Azure Blob 存储服务执行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)。  
  
-   如果你正在使用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 管理备份到云中的备份，则可以通过使用 **smart_admin.fn_available_backups** 系统函数，查看存储中的所有可用备份。 此系统函数会在一个表中返回数据库的所有可用备份。 因为结果以表的形式返回，所以您可以对结果进行筛选或排序。 有关详细信息，请参阅 [managed_backup.fn_available_backups (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-available-backups-transact-sql.md)。  
  
### <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio  
  
-   还原任务用于使用 SQL Server Management Studio 还原数据库。 备份介质页现在包含“URL”选项，可以显示存储在 Azure Blob 存储服务中的备份文件。 您还必须提供用于对存储帐户进行身份验证的 SQL 凭据。 随后，使用 Azure Blob 存储中的可用备份填充“要还原的备份集”网格。 有关详细信息，请参阅[使用 SQL Server Management Studio 从 Azure 存储还原](../../relational-databases/backup-restore/sql-server-backup-to-url.md#RestoreSSMS)。  
  
### <a name="optimizing-restores"></a>优化还原  
 要减小还原写入时间，请将 **“执行卷维护任务”** 用户权限添加到 SQL Server 用户帐户。 有关详细信息，请参阅 [数据库文件初始化](/previous-versions/sql/sql-server-2008-r2/ms175935(v=sql.105))。 如果在开启即时文件初始化后还原操作仍很慢，请查看执行数据库备份的实例的日志文件的大小。 如果日志非常大（好几 GB），则还原操作慢是正常的。 在还原过程中，必须清空日志文件，这需要花费相当多的时间。  
  
 要减少还原次数，建议使用压缩的备份。  对于大小超过 25 GB 的备份，请使用 [AzCopy 实用工具](/archive/blogs/windowsazurestorage/azcopy-uploadingdownloading-files-for-windows-azure-blobs) 下载到本地驱动器，然后执行还原。 有关其他备份最佳实践和建议，请参阅 [SQL Server Backup to URL Best Practices and Troubleshooting](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)。  
  
 在执行还原操作时，也可开启跟踪标志 3051，以生成详细日志。 此日志文件放置在日志目录中，以下面的格式命名：BackupToUrl-\<instancename>-\<dbname>-action-\<PID>.log。 日志文件包含每次往返 Azure 存储的相关信息（包括计时），这在诊断问题时可能非常有用。  
  
### <a name="topics-on-performing-restore-operations"></a>关于执行还原操作的主题  
  
-   [完整数据库还原（简单恢复模式）](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [完整数据库还原（完整恢复模式）](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md)  
  
-   [文件还原（简单恢复模式）](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)  
  
-   [文件还原（完整恢复模式）](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)  
  
-   [段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
