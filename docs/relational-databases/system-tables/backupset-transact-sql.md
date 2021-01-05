---
description: backupset (Transact-SQL)
title: backupset (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 12/24/2020
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- backupset
- backupset_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backupset system table
- backup media [SQL Server], backupset system table
- backup sets [SQL Server]
ms.assetid: 6ff79bbf-4acf-4f75-926f-38637ca8a943
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7653ad7ed1d69e7de97e66d1003d6f131be45142
ms.sourcegitcommit: d8a9ad86401bff422d506078c6200494c795e7c0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/24/2020
ms.locfileid: "97765204"
---
# <a name="backupset-transact-sql"></a>backupset (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  每个备份集在表中占一行。 备份集包含来自单个成功备份操作的备份。 RESTORE、RESTORE FILELISTONLY、RESTORE HEADERONLY 和 RESTORE VERIFYONLY 语句对指定的一个或多个备份设备上的介质集中的单个备份集进行操作。  
  
 该表存储在 **msdb** 数据库中。  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**backup_set_id**|**int**|标识备份集的唯一备份集标识号。 标识，主键。|  
|**backup_set_uuid**|**uniqueidentifier**|标识备份集的唯一备份集标识号。|  
|**media_set_id**|**int**|标识备份集所在介质集的唯一介质集标识号。 引用 **backupmediaset (media_set_id)**。|  
|**first_family_number**|**tinyint**|备份集中第一个介质簇的编号。 可以为 NULL。|  
|**first_media_number**|**smallint**|备份集从此处开始的介质的编号。 可以为 NULL。|  
|**last_family_number**|**tinyint**|备份从此处结束的介质的编号。 可以为 NULL。|  
|**last_media_number**|**smallint**|备份集从此处结束的介质的编号。 可以为 NULL。|  
|**catalog_family_number**|**tinyint**|包含备份集目录开始部分的介质簇的编号。 可以为 NULL。|  
|**catalog_media_number**|**smallint**|包含备份集目录开始部分介质的介质编号。 可以为 NULL。|  
|**position**|**int**|还原操作中用来定位相应的备份集和文件的备份集位置。 可以为 NULL。 有关详细信息，请参阅 [BACKUP &#40;transact-sql&#41;](../../t-sql/statements/backup-transact-sql.md)中的文件。|  
|**expiration_date**|**datetime**|备份集过期的日期和时间。 可以为 NULL。|  
|**software_vendor_id**|**int**|写入备份介质标头的软件供应商标识号。 可以为 NULL。|  
|name|**nvarchar(128)**|备份集的名称。 可以为 NULL。|  
|description|**nvarchar(255)**|备份集的说明。 可以为 NULL。|  
|user_name|**nvarchar(128)**|执行备份操作的用户的名称。 可以为 NULL。|  
|**software_major_version**|**tinyint**|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]主要版本号。 可以为 NULL。|  
|**software_minor_version**|**tinyint**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 次版本号。 可以为 NULL。|  
|**software_build_version**|**smallint**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部版本号。 可以为 NULL。|  
|time_zone|**smallint**|使用备份操作开始时的时区信息，本地时间 (备份操作的发生间隔时间) 和协调世界)  (时，以15分钟为间隔。 值可介于（含） -48 到 +48 之间。 值 127 表示未知。 例如，-20 为美国东部标准时间 (EST)，即比 UTC 晚 5 小时。 可以为 NULL。|  
|**mtf_minor_version**|**tinyint**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] 磁带格式的次版本号。 可以为 NULL。|  
|**first_lsn**|**numeric(25,0)**|备份集中第一条或最早的日志记录的日志序列号。 可以为 NULL。|  
|**last_lsn**|**numeric(25,0)**|备份集之后的下一条日志记录的日志序列号。 可以为 NULL。|  
|**checkpoint_lsn**|**numeric(25,0)**|日志记录中重做必须开始的日志序列号。 可以为 NULL。|  
|**database_backup_lsn**|**numeric(25,0)**|最近的数据库完整备份的日志序列号。 可以为 NULL。<br /><br /> **database_backup_lsn** 是在备份开始时触发的 "检查点的起点"。 如果数据库处于空闲状态且未配置复制，则此 LSN 会与 **first_lsn** 一致。|  
|**database_creation_date**|**datetime**|数据库最初创建的日期和时间。 可以为 NULL。|  
|**backup_start_date**|**datetime**|备份操作的开始日期和时间。 可以为 NULL。|  
|**backup_finish_date**|**datetime**|备份操作的结束日期和时间。 可以为 NULL。|  
|type|**char(1)**|备份类型。 可以是：<br /><br /> D = 数据库<br /><br /> I = 差异数据库<br /><br /> L = 日志<br /><br /> F = 文件或文件组<br /><br /> G = 差异文件<br /><br /> P = 部分<br /><br /> Q = 差异部分<br /><br /> 可以为 NULL。|  
|**sort_order**|**smallint**|执行备份操作的服务器的排序顺序。 可以为 NULL。 有关排序顺序和排序规则的详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。|  
|**code_page**|**smallint**|执行备份操作的服务器的代码页。 可以为 NULL。 有关代码页的详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。|  
|**compatibility_level**|**tinyint**|数据库的兼容级别设置。 可以是：<br /><br /> 90 = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> 100 = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]<br /><br /> 110 = [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]<br /><br /> 120 = [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]<br /><br /> 可以为 NULL。<br /><br /> 有关兼容性级别的详细信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。|  
|**database_version**|**int**|数据库的版本号。 可以为 NULL。|  
|**backup_size**|**numeric(20,0)**|备份集的大小（以字节为单位）。 可以为 NULL。 对于 VSS 备份，backup_size 是一个估计值。|  
|**database_name**|**nvarchar(128)**|备份操作中涉及的数据库的名称。 可以为 NULL。|  
|server_name|**nvarchar(128)**|运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份操作的服务器的名称。 可以为 NULL。|  
|**machine_name**|**nvarchar(128)**|运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的计算机的名称。 可以为 NULL。|  
|**flag**|**int**|在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ， **标志** 列已被弃用，并且将被替换为以下位列：<br /><br /> **has_bulk_logged_data** <br /> **is_snapshot** <br /> **is_readonly** <br /> **is_single_user** <br /> **has_backup_checksums** <br /> **is_damaged** <br /> **begins_log_chain** <br /> **has_incomplete_metadata** <br /> **is_force_offline** <br /> **is_copy_only**<br /><br /> 可以为 NULL。<br /><br /> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]　早期版本的备份集中，标志位如下：<br />1 = 备份包含最少的记录数据。 <br />2 = 使用了 WITH SNAPSHOT。 <br />4 = 备份时数据库为只读。<br />8 = 备份时数据库处于单用户模式。|  
|**unicode_locale**|**int**|Unicode 区域设置。 可以为 NULL。|  
|**unicode_compare_style**|**int**|Unicode 比较风格。 可以为 NULL。|  
|**collation_name**|**nvarchar(128)**|排序规则名。 可以为 NULL。|  
|**Is_password_protected**|**bit**|备份集是否<br /><br /> 受密码保护：<br /><br /> 0 = 未受到保护<br /><br /> 1 = 受到保护|  
|**recovery_model**|**nvarchar(60)**|数据库的恢复模式：<br /><br /> FULL<br /><br /> BULK-LOGGED<br /><br /> SIMPLE|  
|**has_bulk_logged_data**|**bit**|1 = 备份包含大容量日志数据。|  
|**is_snapshot**|**bit**|1 = 备份时使用了 SNAPSHOT 选项。|  
|**is_readonly**|**bit**|1 = 备份时数据库为只读。|  
|**is_single_user**|**bit**|1 = 备份时数据库处于单用户模式。|  
|**has_backup_checksums**|**bit**|1 = 备份包含备份校验和。|  
|**is_damaged**|**bit**|1 = 创建此备份时，检测到数据库损坏。 已要求备份操作忽略错误，继续执行备份。|  
|**begins_log_chain**|**bit**|1 = 这是一个连续的日志备份链中的第一个环节。 日志链的开始处是创建数据库后所做的第一个日志备份，或者是数据库从简单模式切换到完整模式或大容量日志恢复模式时所做的第一个日志备份。|  
|**has_incomplete_metadata**|**bit**|1 = 带不完整元数据的结尾日志备份。 有关详细信息，请参阅[结尾日志备份 (SQL Server)](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)。|  
|**is_force_offline**|**bit**|1 = 进行备份时，使用了 NORECOVERY 选项使数据库脱机。|  
|**is_copy_only**|**bit**|1 = 仅复制备份。 有关详细信息，请参阅[仅复制备份 (SQL Server)](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)。|  
|**first_recovery_fork_guid**|**uniqueidentifier**|起始恢复分叉的 ID。 这对应于 RESTORE HEADERONLY 的 **FirstRecoveryForkID** 。<br /><br /> 对于数据备份， **first_recovery_fork_guid** 等于 **last_recovery_fork_guid**。|  
|**last_recovery_fork_guid**|**uniqueidentifier**|结束恢复分叉的 ID。 这对应于 RESTORE HEADERONLY 的 **RecoveryForkID** 。<br /><br /> 对于数据备份， **first_recovery_fork_guid** 等于 **last_recovery_fork_guid**。|  
|**fork_point_lsn**|**numeric(25,0)**|如果 **first_recovery_fork_guid** 不等于 **last_recovery_fork_guid**，则这是分叉点的日志序列号。 否则，该值为 NULL。|  
|**database_guid**|**uniqueidentifier**|数据库的唯一 ID。 这对应于 RESTORE HEADERONLY 的 **BindingID** 。 还原数据库时，将分配一个新值。|  
|**family_guid**|**uniqueidentifier**|创建时原始数据库的唯一 ID。 还原数据库时，即使还原为其他名称，此值也保持不变。|  
|**differential_base_lsn**|**numeric(25,0)**|差异备份的基准 LSN。 对于基于单一的差异备份，为;在差异备份中包括 Lsn 大于或等于 **differential_base_lsn** 的更改。<br /><br /> 对于多基准差异，该值为 NULL，并且必须在文件级别确定基本 LSN (请参阅 [backupfile &#40;transact-sql&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)) 。<br /><br /> 对于非差异备份类型，该值始终为 NULL。|  
|**differential_base_guid**|**uniqueidentifier**|对于单基准的差异备份，该值为差异基准的唯一标识符。<br /><br /> 对于多基准的差异备份，该值为 NULL，并且必须在文件级别确定差异基准。<br /><br /> 对于非差异备份类型，该值为 NULL。|  
|**compressed_backup_size**|**数值 (20，0)**|磁盘上存储的备份的总字节数。<br /><br /> 若要计算压缩率，请使用 **compressed_backup_size** 和 **backup_size**。<br /><br /> 在 **msdb** 升级期间，此值设置为 NULL。 表示未压缩的备份。|  
|**key_algorithm**|**nvarchar(32)**|用于加密备份的加密算法。 NO_Encryption 值指示备份未加密。|  
|**encryptor_thumbprint**|**varbinary(20)**|可用于在数据库中查找证书或非对称密钥的加密程序的指纹。 在备份未加密的情况下，此值为 NULL。|  
|**encryptor_type**|**nvarchar(32)**|使用的加密程序的类型：证书或非对称密钥。 . 在备份未加密的情况下，此值为 NULL。|  
  
## <a name="remarks"></a>备注
- RESTORE VERIFYONLY FROM *backup_device* with LOADHISTORY，用介质集标头中的相应值填充 **backupmediaset** 表的列。  
- 若要减少此表以及其他备份和历史记录表中的行数，请执行 [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) 存储过程。  
- 对于 SQL 托管实例，backupset 表只显示用户启动的 [仅复制备份](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)的备份历史记录。 Backupset 表不显示服务执行的自动备份的备份历史记录。 
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;备份和还原表 ](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile (Transact-SQL)](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup (Transact-SQL)](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediafamily (Transact-SQL)](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupmediaset (Transact-SQL)](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [备份和还原期间可能出现的媒体错误 (SQL Server)](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [&#40;Transact-sql&#41;备份和还原表 ](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)  
  
  
