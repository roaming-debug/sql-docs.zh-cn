---
description: sys.dm_db_log_stats (Transact-SQL)
title: sys.dm_db_log_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_log_stats_TSQL
- sys.dm_db_log_stats
- sys.dm_db_log_stats_TSQL
- dm_db_log_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_stats dynamic management function
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d9220bbd52a63f571287a76ccbfc7c88a064abd8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071651"
---
# <a name="sysdm_db_log_stats-transact-sql"></a>sys.dm_db_log_stats (Transact-SQL)   
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

返回数据库的事务日志文件的摘要级别属性和信息。 使用此信息监视和诊断事务日志的运行状况。   
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
 sys.dm_db_log_stats ( database_id )
```  
  
## <a name="arguments"></a>参数  

*database_id* |NULL | **默认值**

数据库的 ID。 `database_id` 上声明的默认值为 `int`。 有效的输入包括数据库的 ID 号、 `NULL` 或 `DEFAULT` 。 默认为 `NULL`。 `NULL` 和在 `DEFAULT` 当前数据库的上下文中是等效值。  
可以指定内置函数 [DB_ID](../../t-sql/functions/db-id-transact-sql.md)。 如果在 `DB_ID` 不指定数据库名称的情况下使用，则当前数据库的兼容级别必须为90或更高。

  
## <a name="tables-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id    |**int**    |数据库 ID |  
|recovery_model |**nvarchar(60)**   |   数据库的恢复模式。 可能的值包括： <br /> SIMPLE<br /> BULK_LOGGED <br /> FULL |  
|log_min_lsn    |**nvarchar(24)**   |   事务日志中 [ (LSN) 的当前开始日志序列号 ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) 。|  
|log_end_lsn    |**nvarchar(24)**   |   事务日志中最后一个日志记录[ (LSN) 的日志序列号](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)。|  
|current_vlf_sequence_number    |**bigint** |   当前 [虚拟日志文件 (](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 在执行时 VLF) 序列号。|  
|current_vlf_size_mb    |**float**  |   当前 [虚拟日志文件 (VLF) ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 大小（以 MB 为单位）。|   
|total_vlf_count    |**bigint** |   事务日志中 [ (vlf) 的虚拟日志文件 ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 总数。 |  
|total_log_size_mb  |**float**  |   总事务日志大小（MB）。 |  
|active_vlf_count   |**bigint** |   事务日志中 [ (vlf) 的活动虚拟日志文件 ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 总数。|  
|active_log_size_mb |**float**  |   活动事务日志总大小（MB）。|  
|log_truncation_holdup_reason   |**nvarchar(60)**   |   日志截断暂留原因。 该值与  `log_reuse_wait_desc` 的列相同 `sys.databases` 。  有关这些值的更详细说明，请参阅 [事务日志](../../relational-databases/logs/the-transaction-log-sql-server.md)) 的 (。 <br />可能的值包括： <br />NOTHING<br />CHECKPOINT<br />LOG_BACKUP<br />ACTIVE_BACKUP_OR_RESTORE<br />ACTIVE_TRANSACTION<br />DATABASE_MIRRORING<br />复制<br />DATABASE_SNAPSHOT_CREATION<br />LOG_SCAN<br />AVAILABILITY_REPLICA<br />OLDEST_PAGE<br />XTP_CHECKPOINT<br />其他暂时性 |  
|log_backup_time    |**datetime**   |   上次事务日志备份时间。|   
|log_backup_lsn |**nvarchar(24)**   |   上次事务日志备份 [日志序列号 (LSN) ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)。|   
|log_since_last_log_backup_mb   |**float**  |   自上一次事务日志备份日志序列号后，日志大小（MB） [)  (LSN ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)。|  
|log_checkpoint_lsn |**nvarchar(24)**   |   LSN)  (的最后一个检查点 [日志序列号 ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)。|  
|log_since_last_checkpoint_mb   |**float**  |   自上次检查点 [日志序列号 (LSN) ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)后的日志大小（MB）。|  
|log_recovery_lsn   |**nvarchar(24)**   |   数据库 [ (LSN) 的恢复日志序列号 ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) 。 如果在 `log_recovery_lsn` 检查点 lsn 之前出现， `log_recovery_lsn` 则是最早的活动事务 LSN，否则 `log_recovery_lsn` 是检查点 lsn。|  
|log_recovery_size_mb   |**float**  |   日志大小（MB），因为日志恢复 [日志序列号 (LSN) ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)。|  
|recovery_vlf_count |**bigint** |   在故障转移或服务器重新启动时， (要恢复 [) 的虚拟日志文件 ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 的总数。 |  


## <a name="remarks"></a>备注
`sys.dm_db_log_stats`针对作为辅助副本参与可用性组的数据库运行时，将只返回上述字段的子集。  目前， `database_id` `recovery_model` `log_backup_time` 对辅助数据库运行时，将仅返回、和。   

## <a name="permissions"></a>权限  
需要 `VIEW DATABASE STATE` 数据库中的权限。   
  
## <a name="examples"></a>示例  

### <a name="a-determining-databases-in-a-ssnoversion-instance-with-high-number-of-vlfs"></a>A. 确定包含大量 Vlf 的实例中的数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]   
下面的查询在日志文件中返回 Vlf 超过100的数据库。 大量 Vlf 可能会影响数据库的启动、还原和恢复时间。

```sql  
SELECT name AS 'Database Name', total_vlf_count AS 'VLF count' 
FROM sys.databases AS s
CROSS APPLY sys.dm_db_log_stats(s.database_id) 
WHERE total_vlf_count  > 100;
```   

### <a name="b-determining-databases-in-a-ssnoversion-instance-with-transaction-log-backups-older-than-4-hours"></a>B. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用超过4小时的事务日志备份确定实例中的数据库   
下面的查询确定实例中数据库的最后一次日志备份时间。

```sql  
SELECT name AS 'Database Name', log_backup_time AS 'last log backup time' 
FROM sys.databases AS s
CROSS APPLY sys.dm_db_log_stats(s.database_id); 
```

## <a name="see-also"></a>另请参阅  
[动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)   
[sys.dm_db_log_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)    
  
