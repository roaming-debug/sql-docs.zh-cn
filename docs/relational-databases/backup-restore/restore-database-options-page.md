---
title: 还原数据库（“选项”页）| Microsoft Docs
description: 在 SQL Server 中还原数据库时，使用“还原数据库”对话框的“选项”页可修改还原操作的行为和结果。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restoredb.options.f1
ms.assetid: 9a75d48b-c25f-40f3-8ea1-32cfa8211754
author: cawrites
ms.author: chadam
ms.openlocfilehash: 244b815a0e670cd0e697c037c62d66cc78159d81
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129102"
---
# <a name="restore-database-options-page"></a>还原数据库（“选项”页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用 **“还原数据库”** 对话框的 **“选项”** 页可修改还原操作的行为和结果。  
  
 **使用 SQL Server Management Studio 还原数据库备份**  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [为磁带驱动器定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
> [!NOTE]  
>  使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]指定还原任务时，您可以为此还原操作生成一个包含 RESTORE 语句的对应的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。 若要生成该脚本，请单击 **“脚本”** ，然后为脚本选择一个目标。 有关 RESTORE 语法的信息，请参阅 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)。  
  
## <a name="options"></a>选项  
  
### <a name="restore-options"></a>还原选项  
 若要修改还原操作行为的各个方面，请使用使用 **“还原选项”** 面板中的选项。  
  
 **覆盖现有数据库 [WITH REPLACE]**  
 还原操作将覆盖当前使用你指定的数据库名称（在“还原数据库”对话框中“[常规](../../relational-databases/backup-restore/restore-database-general-page.md)”页上“还原到”字段中指定）的任何数据库文件。   即使将备份从其他数据库还原到现有的数据库名称，现有数据库的文件也将被覆盖。 选择此选项等效于在 [RESTORE](../../t-sql/statements/restore-statements-arguments-transact-sql.md) 语句 ([!INCLUDE[tsql](../../includes/tsql-md.md)]) 中使用 REPLACE 选项。  
  
> [!CAUTION]  
>  只有在仔细考虑后，才能使用此选项。 有关详细信息，请参阅 [RESTORE Arguments (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)。  
  
 **保留复制设置 [WITH KEEP_REPLICATION]**  
 将已发布的数据库还原到创建该数据库的服务器之外的服务器时，保留复制设置。 此选项只适用于在创建备份时对数据库进行了复制的情况。  
  
 仅在选择“回滚未提交的事务，使数据库处于可以使用的状态”  选项（在本表的后面部分中说明）时，此选项才可用，其功能等效于使用 RECOVERY 选项还原备份。  
  
 选择此选项等效于在 [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) 语句中使用 KEEP_REPLICATION 选项。  
  
 有关详细信息，请参阅 [备份和还原复制的数据库](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)。  
  
 **限制还原数据库的访问 [WITH RESTRICTED_USER]**  
 使还原的数据库仅供 **db_owner**、 **dbcreator** 或 **sysadmin** 的成员使用。  
  
 选择此选项等效于在 RESTORE 语句中使用 RESTRICTED_USER 选项。  
  
### <a name="recovery-state"></a>恢复状态  
 若要在完成存储操作后确定数据库的状态，则必须选择 **“恢复状态”** 面板中的选项之一。  
  
 **RESTORE WITH RECOVERY**  
 在还原了在[“常规”](../../relational-databases/backup-restore/restore-database-general-page.md)页的“用于还原的备份集”  网格中选中的最后一个备份之后，恢复数据库。 这是默认选项，等效于在 [RESTORE](../../t-sql/statements/restore-statements-arguments-transact-sql.md) 语句 ([!INCLUDE[tsql](../../includes/tsql-md.md)]) 中指定 WITH RECOVERY。  
  
> [!NOTE]  
>  在完整恢复模式或大容量日志恢复模式下，只有在需要还原所有日志文件时才选择此选项。  
  
 **RESTORE WITH NORECOVERY**  
 使数据库处于还原状态。 这允许您还原当前恢复路径中的其他备份。 若要恢复数据库，则必须使用 RESTORE WITH RECOVERY 选项（请参阅前面的选项）来执行还原操作。  
  
 此选项等效于在 RESTORE 语句中指定 WITH NORECOVERY。  
  
 如果选择此选项， **“保留复制设置”** 选项将不可用。  
  
 **RESTORE WITH STANDBY**  
 使数据库处于备用状态，在该状态下只能对数据库进行有限的只读访问。 此选项等效于在 RESTORE 语句中指定 WITH STANDBY。  
  
 选择该选项要求您在 **“备用文件”** 文本框中指定一个备用文件。 备用文件允许撤消恢复效果。  
  
 **“备用文件”**  
 指定备用文件。 您可以浏览到该备用文件，也可以在文本框中直接输入其路径名。  
  
### <a name="tail-log-backup"></a>结尾日志备份  
 允许您指定结尾日志备份与数据库还原一起执行。  
  
 **在还原前执行结尾日志备份**  
 选中此复选框可以指定应执行结尾日志备份。  
  
> [!NOTE]  
>  如果你在[“备份时间线”](../../relational-databases/backup-restore/backup-timeline.md) 对话框中选择的时间点要求结尾日志备份，则将选择此框并且你将不能对其进行编辑。  
  
 **“备份文件”**  
 为日志的结尾指定备份文件。 您可以浏览备份文件，也可以在文本框中直接输入其名称。  
  
### <a name="server-connections"></a>服务器连接  
 可用于关闭现有的数据库连接。  
  
 **关闭现有连接**  
 如果存在与数据库的活动连接，则还原操作可能会失败。 选中 **“关闭现有连接”** 以确保关闭 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 和数据库之间的所有活动连接。 此复选框可在执行还原操作之前将数据库设置为单用户模式，并在该操作完成后将数据库设置为多用户模式。  
  
### <a name="prompt"></a>Prompt  
 **还原每个备份之前进行提示**  
 指定在还原了每个备份之后，将显示“继续还原”  对话框，询问你是否要继续还原顺序。 该对话框将显示下一个介质集（如果已知）的名称以及下一个备份集的名称和说明。  
  
 此选项允许您在还原了任何备份后暂停还原顺序。 如果必须为不同介质集更换磁带，例如在服务器仅具有一个磁带设备时，此选项非常有用。 准备就绪后，请单击 **“确定”** 以继续。  
  
 可以通过单击 **“否”** 中断还原顺序。 这样可以使数据库保持还原状态。 在日后方便的时候，可以通过恢复执行 **“继续还原”** 对话框中所列出的下一个备份，继续该还原顺序。 还原下一个备份的过程取决于其是否包含数据或事务日志，如下所示：  
  
-   如果下一个备份是完整备份或差异备份，请再次使用 **“还原数据库”** 任务。  
  
-   如果下一个备份是文件备份，请使用 **“还原文件和文件组”** 任务。 有关详细信息，请参阅[还原文件和文件组 (SQL Server)](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)。  
  
-   如果下一个备份是日志备份，请使用 **“还原事务日志”** 任务。 有关通过还原事务日志来继续还原顺序的信息，请参阅 [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)。  
  
## <a name="see-also"></a>另请参阅  
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [从设备还原备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-backup-from-a-device-sql-server.md)   
 [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [还原数据库（“常规”页）](../../relational-databases/backup-restore/restore-database-general-page.md)  
  
  
