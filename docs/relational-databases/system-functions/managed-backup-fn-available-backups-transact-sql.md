---
description: " (transact-sql 的 managed_backup.fn_available_backups managed_backup) "
title: managed_backup managed_backup.fn_available_backups (transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- smart_admin.fn_available_backups
- smart_admin.fn_available_backups_TSQL
- fn_available_backups_TSQL
- fn_available_backups
dev_langs:
- TSQL
helpviewer_keywords:
- fn_available_backups
- smart_admin.fn_available_backups
ms.assetid: 7aa84474-16e5-49bd-a703-c8d1408ef107
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e64cba484413a0d5ebc1a8365d0e843c49ba7692
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207442"
---
# <a name="managed_backupfn_available_backups-transact-sql"></a> (transact-sql 的 managed_backup.fn_available_backups managed_backup) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  返回一个表格，其中包含 0 行、一行或多行的指定数据库可用备份文件。 返回的备份文件是由 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 创建的备份。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sql  
managed_backup.fn_available_backups ([@database_name = ] 'database name')  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> 参数  
 @database_name  
 数据库的名称。 @database_name为 NVARCHAR (512) 。  
  
## <a name="table-returned"></a>返回的表  
 此表已启用唯一的聚集约束（database_guid、backup_start_date、first_lsn 和 backup_type）。   
如果删除某个数据库然后重新创建它，则将返回所有数据库的备份集。 输出按 database_guid（唯一标识每个数据库）排列。   
如果 LSN 中存在间距，则表示日志链中存在中断，则表将为每个缺少的 LSN 段包含一个特殊行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|Backup_path|NVARCHAR(260) COLLATE Latin1_General_CI_AS_KS_WS|备份文件的 URL。|  
|backup_type|NVARCHAR (6) |用于数据库备份的 "数据库" 日志备份|  
|expiration_date|DATETIME|预期删除此文件的日期。 这是基于将数据库恢复到指定保持期内某一时间点的能力设置的。|  
|database_guid|UNIQUEIDENTIFIER|指定数据库的 GUID 值。  GUID 唯一地标识一个数据库。|  
|first_lsn|NUMERIC(25, 0)|备份集中第一条或最早的日志记录的日志序列号。 可以为 NULL。|  
|last_lsn|NUMERIC(25, 0)|备份集之后的下一条日志记录的日志序列号。 可以为 NULL。|  
|backup_start_date|DATETIME|备份操作的开始日期和时间。|  
|backup_finish_date|NVARCHAR(128)|备份操作的结束日期和时间。|  
|machine_name|NVARCHAR(128)|安装有 SQL Server 实例并运行 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 的计算机的名称。|  
|last_recovery_fork_id|UNIQUEIDENTIFIER|结束恢复分叉的标识号。|  
|first_recovery_fork_id|UNIQUEIDENTIFIER|起始恢复分叉的 ID。 对于数据备份，first_recovery_fork_guid 等于 last_recovery_fork_guid。|  
|fork_point_lsn|NUMERIC(25, 0)|如果 first_recovery_fork_id 不等于 last_recovery_fork_id，这将是分叉点的日志序列号。 否则，此值为 NULL。|  
|availability_group_guid|UNIQUEIDENTIFIER|如果数据库是 Always On 数据库，则这是可用性组的 GUID。 否则，此值为 NULL。|  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）或 1（失败）。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 需要对此函数的 **SELECT** 权限。  
  
## <a name="examples"></a>示例  
 以下示例列出了通过 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 数据库 "MyDB" 备份的所有可用备份  
  
```  
SELECT *   
FROM msdb.managed_backup.fn_available_backups ('MyDB')  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 托管备份到 Microsoft Azure](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)   
 [从 Microsoft Azure 中存储的备份还原](../../relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure.md)  
  
  
