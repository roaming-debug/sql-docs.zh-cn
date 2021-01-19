---
title: 目标为 Microsoft Azure 的 SQL Server 托管备份 | Microsoft Docs
description: 目标为 Microsoft Azure 的 SQL Server 托管备份管理并自动执行将 SQL Server 备份到 Microsoft Azure Blob 存储。
ms.custom: ''
ms.date: 10/18/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: afa01165-39e0-4efe-ac0e-664edb8599fd
author: cawrites
ms.author: chadam
ms.openlocfilehash: ce723dfbf0171d3c1885c1574bf681fbb2c8e714
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172559"
---
# <a name="sql-server-managed-backup-to-microsoft-azure"></a>目标为 Microsoft Azure 的 SQL Server 托管备份
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 管理并自动执行将 SQL Server 备份到 Microsoft Azure Blob 存储。 你可以选择允许 SQL Server 基于你的数据库的事务工作负荷确定备份计划。 或者可以使用高级选项来定义计划。 保留期设置可确定备份存储在 Azure Blob 存储上的时间。 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 支持指定保持期的时间点还原。  
  
 从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]开始， [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 的过程和基础行为已更改。 有关详细信息，请参阅[将 SQL Server 2014 托管备份设置迁移到 SQL Server 2016](../../relational-databases/backup-restore/migrate-sql-server-2014-managed-backup-settings-to-sql-server-2016.md)。  
  
> [!TIP]  
>  [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 建议用于在 Microsoft Azure 虚拟机上运行的 SQL Server 实例。  
  
## <a name="benefits"></a>优点  
 目前，自动备份多个数据库需要制定备份策略、编写自定义代码并安排备份。 使用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]，你可以通过仅指定保持期和存储位置来创建备份计划。 虽然可以使用高级设置，但并非必需。 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 可安排、执行并维护备份。  
  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 可以在数据库级别或 SQL Server 实例级别进行配置。 在实例级别进行配置时，任何新数据库也将自动备份。 在数据库级别的设置可用于在个别情况下重写实例级别默认设置。  
  
 你还可以对备份进行加密以提高安全性，并且可以设置自定义计划以控制执行备份的时间。 有关使用用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份的 Microsoft Azure Blob 存储的优势详细信息，请参阅 [Microsoft Azure Blob 存储服务的 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)  
  
##  <a name="prerequisites"></a><a name="Prereqs"></a>先决条件  
 Microsoft Azure 存储空间是 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 用于存储备份文件的存储区。 需要以下先决条件：  
  
|先决条件|说明|  
|------------------|-----------------|  
|**Microsoft Azure 帐户**|可以在浏览 [购买选项](https://azure.microsoft.com/pricing/free-trial/) 之前，使用 [免费试用版](https://azure.microsoft.com/pricing/purchase-options/)开始使用 Azure。|  
|**Azure 存储帐户**|备份存储在与 Azure 存储帐户相关的 Azure Blob 存储中。 有关创建存储帐户的分步说明，请参阅 [About Azure Storage Accounts](/azure/storage/common/storage-account-create)（关于 Azure 存储帐户）。|  
|**Blob 容器**|BLOB 组织在容器中。 你可以为备份文件指定目标容器。 可以在 [Azure 管理门户](https://manage.windowsazure.com/)中创建容器，或者可以使用 **New-AzureStorageContainer**[Azure PowerShell](/powershell/azure/) 命令。|  
|**共享访问签名 (SAS)**|对目标容器的访问由共享访问签名 (SAS) 控制。 有关 SAS 的概述，请参阅 [共享访问签名（第 1 部分）：了解 SAS 模型](/azure/storage/common/storage-sas-overview)。 可以使用 **New-AzureStorageContainerSASToken** PowerShell 命令在代码中创建一个 SAS 令牌。 有关简化此过程的 PowerShell 脚本，请参阅 [Simplifying creation of SQL Credentials with Shared Access Signature ( SAS ) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell)（使用 Powershell 简化在 Azure 存储空间中使用共享访问签名 (SAS) 令牌创建 SQL 凭据的过程）。 SAS 令牌可以存储在 **SQL 凭据** 中，供 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]使用。|  
|**SQL Server 代理**|[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 必须运行 SQL Server 代理以进行工作。 考虑将启动选项设置为自动。|  
  
## <a name="components"></a>组件  
 Transact-SQL 是与 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]交互的主要界面。 系统存储过程用于启用、配置和监视 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]。 系统函数用于检索现有配置设置、参数值和备份文件信息。 扩展事件用于呈现错误和警告。 警报机制通过 SQL 代理作业和 SQL Server 基于策略的管理启用。 以下列表列出了与 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]相关的对象及其功能的说明。  
  
 也有 PowerShell cmdlet 可用于配置 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]。 SQL Server Management Studio 支持还原由 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 使用 **“还原数据库”** 任务创建的备份  
  
|系统对象|说明|  
|-|-|  
|**MSDB**|存储由 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]创建的所有备份的元数据和备份历史记录。|  
|[managed_backup.sp_backup_config_basic (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md)|启用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]。|  
|[managed_backup.sp_backup_config_advanced (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-advanced-transact-sql.md)|为 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]配置高级设置，如加密。|  
|[managed_backup.sp_backup_config_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md)|为 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]创建自定义计划。|  
|[managed_backup.sp_ backup_master_switch (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-master-switch-transact-sql.md)|暂停和恢复 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]。|  
|[managed_backup.sp_set_parameter (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql.md)|启用和配置对 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]的监视。 示例：启用扩展事件、通知的邮件设置。|  
|[managed_backup.sp_backup_on_demand (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-on-demand-transact-sql.md)|对为使用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 而启用的数据库执行即席备份而不中断日志链。|  
|[managed_backup.fn_backup_db_config (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-backup-db-config-transact-sql.md)|返回数据库或实例的所有数据库的当前 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 状态和配置值。|  
|[managed_backup.fn_is_master_switch_on (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-is-master-switch-on-transact-sql.md)|返回主开关状态。|  
|[managed_backup.sp_get_backup_diagnostics (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql.md)|返回扩展事件所记录的事件。|  
|[managed_backup.fn_get_parameter (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-get-parameter-transact-sql.md)|返回备份系统设置当前值，如监视和警报的电子邮件设置。|  
|[managed_backup.fn_available_backups (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-available-backups-transact-sql.md)|用于检查指定数据库或实例所有数据库的可用备份。|  
|[managed_backup.fn_get_current_xevent_settings (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-get-current-xevent-settings-transact-sql.md)|返回当前扩展事件设置。|  
|[managed_backup.fn_get_health_status (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql.md)|返回由指定期间扩展事件记录的合计错误数。|  
  
## <a name="backup-strategy"></a>备份策略  
  
### <a name="backup-scheduling"></a>备份计划  
 你可以使用系统存储过程 [managed_backup.sp_backup_config_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md)。 如果没有指定自定义计划，则安排的备份类型和备份频率将根据数据库的工作负载确定。 保持期设置用于决定应在存储中保留备份文件的时间长度以及能否将数据库恢复到保留期内的某个时间点。  
  
### <a name="backup-file-naming-conventions"></a>备份文件命名约定  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 使用你指定的容器，因此你可以控制容器的名称。 对于备份文件，按照以下约定命名非可用性数据库：使用数据库名称的前 40 个字符、不含“-”的数据库 GUID 以及时间戳创建名称。 在各段之间插入下划线字符作为分隔符。 完整备份使用 **.bak** 文件扩展名，日志备份使用 **.log** 。 对于可用性组数据库，除了上方所述的文件命名约定之外，还在 40 个字符的数据库名称后添加可用性组数据库 GUID。 可用性组数据库 GUID 值为 sys.databases 中 group_database_id 的值。  
  
### <a name="full-database-backup"></a>完整数据库备份  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 代理便会安排一次完整数据库备份。  
  
-   初次为数据库启动 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] ，或在实例级使用默认设置启用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 时。  
  
-   自上次完备数据库备份以来的日志增长等于或超过 1 GB。  
  
-   距上次完整数据库备份以来已超过一周的最大时间间隔。  
  
-   日志链中断。 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 会定期进行检查，通过比较备份文件的第一个和最后一个 LSN 确定日志链是否完整。 如果日志链因为任何原因而存在中断，则 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 会安排一次完整数据库备份。 日志链断裂的最常见原因可能是使用 Transact-SQL 或通过 SQL Server Management Studio 中的备份任务发出了备份命令。  其他常见的情况包括意外删除了备份文件或意外的备份覆盖。  
  
### <a name="transaction-log-backup"></a>事务日志备份  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 安排一次日志备份：  
  
-   找不到日志备份历史记录。 在首次启动 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 时通常存在这种情况。  
  
-   使用的事务日志空间为 5 MB 或更大。  
  
-   距上次日志备份达到 2 小时的最大时间间隔。  
  
-   事务日志备份滞后于完整备份数据库时。 目标是使日志链保持在完整备份之前。  
  
## <a name="retention-period-settings"></a>保持期设置  
 在启用备份时，必须以天为单位设置保持期：最短为 1 天，最长为 30 天。  
  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 根据保持期设置评估能否恢复到指定的时间点，以决定要保留什么备份文件并找出要删除的备份文件。 备份的 backup_finish_date 用于确定和匹配由保持期设置指定的时间。  
  
## <a name="important-considerations"></a>重要注意事项  
 对于数据库，如果现有完整数据库备份作业正在运行，则 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 将等待当前作业完毕，然后对该数据库进行另一次完整数据库备份。 同样，给定时间只能运行一个事务日志备份。 但是，完整数据库备份和事务日志备份可以同时运行。 失败会记录为扩展事件。  
  
 如果安排了超过 10 个并发的完整数据库备份，则会通过扩展事件的调试渠道发出一个警告。 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 随后针对其余需要备份的数据库保留一个优先级队列，直到安排并完成所有备份。  

> [!NOTE]
> 代理服务器不支持 SQL Server 托管备份。
>
  
##  <a name="supportability"></a><a name="support_limits"></a> 可支持性  
 以下内容支持特定于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]的限制和注意事项：  
  
-   支持备份“主”  、“模型”  和“msdb”  系统数据库。 不支持备份“tempdb”  。 
  
-   对于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]，所有恢复模式（完整、大容量日志和简单）均受支持。  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 代理只支持数据库完整备份和日志备份。 不支持文件备份自动化。  
  
-   Microsoft Azure Blob 存储服务是唯一受支持的备份存储选项。 不支持备份到磁盘或磁带。  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 使用备份到块 Blob 的功能。 块 BLOB 的最大大小为 200 GB。 但通过使用带区，单个备份的最大大小可达 12 TB。 如果你的备份要求超出此值，请考虑使用压缩，并在设置 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]前测试备份文件大小。 可通过备份到本地磁盘或使用 **BACKUP TO URL** Transact-SQL 语句手动备份到 Microsoft Azure 进行测试。 有关详细信息，请参阅 [SQL Server Backup to URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md)。  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 可能有一些限制。  
  
## <a name="see-also"></a>另请参阅  
- [启用目标为 Microsoft Azure 的 SQL Server 托管备份](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md)   
- [为目标为 Microsoft Azure 的 SQL Server 托管备份配置高级选项](../../relational-databases/backup-restore/configure-advanced-options-for-sql-server-managed-backup-to-microsoft-azure.md)   
- [禁用目标为 Microsoft Azure 的 SQL Server 托管备份](../../relational-databases/backup-restore/disable-sql-server-managed-backup-to-microsoft-azure.md)
- [系统数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)
- [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
  
