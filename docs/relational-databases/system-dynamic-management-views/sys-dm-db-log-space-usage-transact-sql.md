---
description: 'sys.dm_db_log_space_usage (Transact-sql) '
title: sys.dm_db_log_space_usage (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_log_space_usage
- sys.dm_db_log_space_usage_TSQL
- dm_db_log_space_usage
- dm_db_log_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_space_usage dynamic management view
ms.assetid: f6b40060-c17d-472f-b0a3-3b350275d487
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0f6c2f10d5865305896a2e10405e2ca6f05399f8
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251119"
---
# <a name="sysdm_db_log_space_usage-transact-sql"></a>sys.dm_db_log_space_usage (Transact-sql) 
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

返回事务日志的空间使用情况信息。 
  
> [!NOTE]
> 所有事务日志文件都是组合的。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**smallint**|数据库 ID。|  
|total_log_size_in_bytes |**bigint** |日志的大小  |
|used_log_space_in_bytes |**bigint** |日志的占用大小  |     
|used_log_space_in_percent |**real** |日志大小占总日志大小的百分比 |
|log_space_in_bytes_since_last_backup |**bigint** |自上次日志备份后使用的空间量 <br />**适用于：** [!INCLUDE[sssql14-md](../../includes/sssql14-md.md)] 到 [!INCLUDE[sscurrent-md](../../includes/ssnoversion-md.md)] ，  [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 。|
    
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="examples"></a>示例  
  
### <a name="a-determine-the-amount-of-free-log-space-in-tempdb"></a>A. 确定 tempdb 中的可用日志空间量   
下面的查询返回 tempdb 中可用的总可用日志空间（以 mb 为单位） (MB) 。

```sql
USE tempdb;  
GO  

SELECT (total_log_size_in_bytes - used_log_space_in_bytes)*1.0/1024/1024 AS [free log space in MB]  
FROM sys.dm_db_log_space_usage;  
```
  
## <a name="see-also"></a>另请参阅  
[动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_file_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md)    
[sys.dm_db_task_space_usage &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md)   
[sys.dm_db_session_space_usage (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)  
[sys.dm_db_log_info (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)    
[sys.dm_db_log_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-stats-transact-sql.md) 



