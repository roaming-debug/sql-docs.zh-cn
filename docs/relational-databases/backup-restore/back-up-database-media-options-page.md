---
title: 备份数据库（“介质选项”页）| Microsoft Docs
description: 在 SQL Server 中，使用“备份数据库”对话框中的“媒体选项”可查看/修改媒体选项，包括覆盖媒体、可靠性和事务日志。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- swb.backupdatabase.mediaoptions.f1
- sql13.swb.backupdatabase.mediaoptions.f1
ms.assetid: eff36228-710c-4ed5-9af5-95859575dc0f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 384233f6fdc7379ad72b120972b16ca5aba736cb
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129384"
---
# <a name="back-up-database-media-options-page"></a>备份数据库（“介质选项”页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用  **“备份数据库”** 对话框的 **“介质选项”** 页可以查看或修改数据库介质选项。  
  
 **使用 SQL Server Management Studio 创建备份**  
  
-   [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [创建差异数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
> [!IMPORTANT]  
>  可以定义用于创建数据库备份的数据库维护计划。 有关详细信息，请参阅[维护计划](../../relational-databases/maintenance-plans/maintenance-plans.md)和[使用维护计划向导](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)。  
  
> [!NOTE]  
>  使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 指定备份任务时，可以通过单击“脚本”  按钮，再为脚本选择一个目标，生成对应的 [!INCLUDE[tsql](../../includes/tsql-md.md)][BACKUP](../../t-sql/statements/backup-transact-sql.md) 脚本。  
  
## <a name="options"></a>选项  
  
### <a name="overwrite-media"></a>覆盖介质  
 **“覆盖介质”** 面板中的选项可以控制如何将备份写入介质。 如果在“备份数据库”对话框的“常规”页上选择了 URL（Azure 存储）作为备份目标，则禁用“覆盖介质”部分下的选项。 可使用 **BACKUP TO URL.WITH FORMAT** Transact-SQL 语句覆盖备份。 有关详细信息，请参阅 [SQL Server Backup to URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md)。  

 如果在“常规”页中选择了“URL”作为备份目标，则禁用“覆盖介质”选项。   
  
 仅支持选项 **“备份到新介质集并清除所有现有备份集”** 与加密选项一起使用。 如果选择 **“备份到现有介质”** 部分下的选项，则将禁用 **“备份选项”** 页上的加密选项。  
  
> [!NOTE]  
>  有关媒体集的信息，请参阅 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)实例的计算机附连有磁带机时，此选项才可用。  
  
**备份到现有介质集**：将数据库备份到现有介质集。 选择此选项按钮将激活三个选项。  
  
 选择以下选项之一：  
  
 - **追加到现有备份集**：将备份集追加到现有介质集，并保留以前的所有备份。  
  
 - **覆盖所有现有备份集**：将现有介质集上以前的所有备份替换为当前备份。  
  
 - **检查介质集名称和备份集过期时间**：如果备份到现有介质集，还可以要求备份操作验证备份集的名称和过期时间。  
  
 - **介质集名称**：如果选中了“检查媒体集名称和备份集过期时间”  ，还可以指定用于此备份操作的媒体集的名称。  
  
 - **备份到新介质集并清除所有现有备份集**：使用新介质集，并清除以前的备份集。  
  
 单击此选项可以激活以下选项：  
  
 - **新建介质集名称**：根据需要，可以输入介质集的新名称。  
  
 - **新建介质集说明**：根据需要，可以输入新介质集的贴切说明。 此说明应该足够具体，可以准确地表述内容。  
  
### <a name="reliability"></a>可靠性  
 **“事务日志”** 面板中的选项可以控制备份操作的错误管理。  
  
 - **完成后验证备份**：验证备份集是否完整以及所有卷是否都可读。  
  
 - **写入介质前检查校验和**：在写入备份介质前验证校验和。 选择此选项等效于在 [!INCLUDE[tsql](../../includes/tsql-md.md)]的 BACKUP 语句中指定 CHECKSUM 选项。 选择此选项可能会增大工作负荷，并降低备份操作的备份吞吐量。 有关校验和的详细信息，请参阅[在备份和还原期间可能的媒体错误 (SQL Server)](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)。  
  
 - **出错时继续**：备份操作继续进行，即使在遇到一个或多个错误后。  
  
### <a name="transaction-log"></a>事务日志  
 **“事务日志”** 面板中的选项可以控制事务日志备份的行为。 这些选项只在完整恢复模式或大容量日志恢复模式下相关。 仅在 **“备份数据库”** 对话框的 **“常规”** 页上的 [“备份类型”](../../relational-databases/backup-restore/back-up-database-general-page.md) 字段中选中了 **“事务日志”** 时，才会激活这些选项。  
  
> [!NOTE]  
>  有关事务日志备份的信息，请参阅[事务日志备份 (SQL Server)](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)。  
  
 **截断事务日志**  
 备份事务日志并将其截断以释放日志空间。 数据库仍然处于联机状态。 这是默认选项。  
  
 **备份日志尾部，并使数据库处于还原状态**  
 备份日志末尾并将数据库保留在还原状态。 此选项创建 *结尾日志备份*，通常用于在准备还原数据库时备份尚未备份的日志（活动日志）。 在数据库完全还原之前，用户将无法使用。  
  
 选择此选项等效于在 [BACKUP](../../t-sql/statements/backup-transact-sql.md) 语句 ([!INCLUDE[tsql](../../includes/tsql-md.md)]) 中指定“WITH NO_TRUNCATE, NORECOVERY”。 有关详细信息，请参阅[结尾日志备份 (SQL Server)](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)。  
  
### <a name="tape-drive"></a>磁带机  
 **“磁带机”** 面板中的选项可以控制备份操作期间的磁带管理。 仅在 **“备份数据库”** 对话框的 **“常规”** 页上的 [“目标”](../../relational-databases/backup-restore/back-up-database-general-page.md) 面板中选中了 **“磁带”** ，才会激活这些选项。  
  
> [!NOTE]  
>  有关如何使用磁带设备的信息，请参阅[备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。  
  
 - **备份后取下磁带**：在备份完成后，卸载磁带。  
  
 - **取下前倒带**：取下磁带前，释放空间并进行倒带。 仅在选中了 **“备份后卸载磁带”** 的时候，才会启用该选项。  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)   
 [备份文件和文件组 (SQL Server)](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [在数据库损坏时备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md)  
  
  
