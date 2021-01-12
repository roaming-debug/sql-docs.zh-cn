---
description: sys.dm_repl_traninfo (Transact-SQL)
title: sys.dm_repl_traninfo (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_repl_traninfo
- dm_repl_traninfo
- sys.dm_repl_traninfo_TSQL
- dm_repl_traninfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_repl_traninfo dynamic management view
ms.assetid: 5abe2605-0506-46ec-82b5-6ec08428ba13
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f8a0613d83cebfed56ba5202a6c3535ef31c8a84
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098828"
---
# <a name="sysdm_repl_traninfo-transact-sql"></a>sys.dm_repl_traninfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关每个复制的事务或变更数据捕获事务的信息。  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**fp2p_pub_exists**|**tinyint**|事务是否位于使用对等事务复制发布的数据库中。 如果为 true，该值为 1；否则该值为 0。|  
|**db_ver**|**int**|数据库版本。|  
|**comp_range_address**|**varbinary(8)**|定义必须跳过的部分回滚范围。|  
|**textinfo_address**|**varbinary(8)**|缓存的文本信息结构的内存中的地址。|  
|**fsinfo_address**|**varbinary(8)**|缓存的文件流信息结构的内存中的地址。|  
|**begin_lsn**|**nvarchar (64)**|事务的开始日志记录的日志序列号 (LSN)。|  
|**commit_lsn**|**nvarchar (64)**|事务的提交日志记录的 LSN。|  
|**dbid**|**smallint**|数据库 ID。|  
|**rows**|**int**|事务中复制的命令的 ID。|  
|**xdesid**|**nvarchar (64)**|事务 ID。|  
|**artcache_table_address**|**varbinary(8)**|上次用于该事务的缓存的项目表结构的内存中的地址。|  
|服务器|**nvarchar (514)**|服务器名称。|  
|**server_len_in_bytes**|**smallint**|服务器名称的字符长度（字节）。|  
|**database**|**nvarchar (514)**|数据库名称。|  
|**db_len_in_bytes**|**smallint**|数据库名称的字符长度（字节）。|  
|**发出**|**nvarchar (514)**|发起事务的服务器的名称。|  
|**originator_len_in_bytes**|**smallint**|发起事务的服务器的字符长度（字节）。|  
|**orig_db**|**nvarchar (514)**|发起事务的数据库的名称。|  
|**orig_db_len_in_bytes**|**smallint**|发起事务的数据库的字符长度（字节）。|  
|**cmds_in_tran**|**int**|当前事务中复制的命令数，用于确定应何时提交逻辑事务。|  
|**is_boundedupdate_singleton**|**tinyint**|指定唯一列更新是否仅影响单行。|  
|**begin_update_lsn**|**nvarchar (64)**|唯一列更新中使用的 LSN。|  
|**delete_lsn**|**nvarchar (64)**|要作为更新的一部分删除的 LSN。|  
|**last_end_lsn**|**nvarchar (64)**|逻辑事务中的最后一个 LSN。|  
|**fcomplete**|**tinyint**|指定命令是否为部分更新。|  
|**fcompensated**|**tinyint**|指定事务是否包含在部分回滚中。|  
|**fprocessingtext**|**tinyint**|指定事务是否包含二进制大型数据类型列。|  
|**max_cmds_in_tran**|**int**|日志读取器代理指定的逻辑事务中的最大命令数。|  
|**begin_time**|**datetime**|事务的开始时间。|  
|**commit_time**|**datetime**|提交事务的时间。|  
|**session_id**|**int**|变更数据捕获日志扫描会话的 ID。 此列映射到 [sys.dm_cdc_logscan_sessions](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md)中的 **session_id** 列。|  
|**session_phase**|**int**|指示出错时会话所处阶段的数字。 此列映射到 [sys.dm_cdc_errors](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md)中的 **phase_number** 列。|  
|**is_known_cdc_tran**|**bit**|指示变更数据捕获所跟踪的事务。<br /><br /> 0 = 事务复制事务。<br /><br /> 1 = 变更数据捕获事务。|  
|**error_count**|**int**|遇到的错误数。|  
  
## <a name="permissions"></a>权限  
 要求对发布数据库或启用了变更数据捕获的数据库拥有 VIEW DATABASE STATE 权限。  
  
## <a name="remarks"></a>备注  
 仅为项目缓存中当前加载的复制数据库对象或启用了变更数据捕获的表返回信息。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与复制相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)   
 [与变更数据捕获相关的动态管理视图 (Transact-SQL)](./system-dynamic-management-views.md)  
  
