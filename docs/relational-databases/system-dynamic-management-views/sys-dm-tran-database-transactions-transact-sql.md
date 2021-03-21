---
description: sys.dm_tran_database_transactions (Transact-SQL)
title: sys.dm_tran_database_transactions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_tran_database_transactions
- sys.dm_tran_database_transactions_TSQL
- dm_tran_database_transactions_TSQL
- sys.dm_tran_database_transactions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_database_transactions dynamic management view
ms.assetid: 82a44295-4cbc-4a5b-891a-8ebaf307b8f5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d6e7eeaff64056c0091b170ae18ab35fa7be89f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753777"
---
# <a name="sysdm_tran_database_transactions-transact-sql"></a>sys.dm_tran_database_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回有关数据库级事务的信息。  
  
> [!NOTE]  
>  若要从或调用此 DMV [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_tran_database_transactions**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|transaction_id|**bigint**|实例级而非数据库级的事务 ID。 仅在一个实例内的所有数据库中唯一，在所有服务器实例中则不唯一。|  
|database_id|**int**|与事务关联的数据库的 ID。|  
|database_transaction_begin_time|**datetime**|数据库参与事务的时间。 具体而言，它是事务的数据库中第一个日志记录的时间。|  
|database_transaction_type|**int**|1 = 读/写事务<br /><br /> 2 = 只读事务<br /><br /> 3 = 系统事务|  
|database_transaction_state|**int**|1 = 未初始化事务。<br /><br /> 3 = 已初始化事务，但未生成任何日志记录。<br /><br /> 4 = 事务已生成日志记录。<br /><br /> 5 = 事务已准备就绪。<br /><br /> 10 = 事务已提交。<br /><br /> 11 = 已回滚事务。<br /><br /> 12 = 正在提交事务。  (正在生成日志记录，但尚未具体化或持久化。 ) |  
|database_transaction_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|database_transaction_status2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|database_transaction_log_record_count|**bigint**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 在事务的数据库中生成的日志记录数。|  
|database_transaction_replicate_record_count|**int**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 已复制的事务在数据库中生成的日志记录数。|  
|database_transaction_log_bytes_used|**bigint**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 到目前为止，在事务的数据库日志中使用的字节数。|  
|database_transaction_log_bytes_reserved|**bigint**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 为事务的数据库日志保留的字节数。|  
|database_transaction_log_bytes_used_system|**int**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 到目前为止，在代表事务的系统事务的数据库日志中使用的字节数。|  
|database_transaction_log_bytes_reserved_system|**int**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 为代表事务的系统事务的数据库日志保留的字节数。|  
|database_transaction_begin_lsn|**numeric(25,0)**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 数据库日志中事务的起始记录的日志序列号 (LSN)。|  
|database_transaction_last_lsn|**numeric(25,0)**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 数据库日志中最近记录的事务记录的 LSN。|  
|database_transaction_most_recent_savepoint_lsn|**numeric(25,0)**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 数据库日志中事务的最近保存点的 LSN。|  
|database_transaction_commit_lsn|**numeric(25,0)**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 数据库日志中事务的提交日志记录的 LSN。|  
|database_transaction_last_rollback_lsn|**numeric(25,0)**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 最近回滚到的 LSN。 如果未发生回滚，则值为 MaxLSN。|  
|database_transaction_next_undo_lsn|**numeric(25,0)**|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 要撤消的下一个记录的 LSN。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="see-also"></a>另请参阅  
 [sys.dm_tran_active_transactions &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-transactions-transact-sql.md)   
 [sys.dm_tran_session_transactions &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与事务相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
