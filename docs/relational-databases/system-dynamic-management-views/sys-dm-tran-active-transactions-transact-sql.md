---
description: sys.dm_tran_active_transactions (Transact-SQL)
title: sys.dm_tran_active_transactions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_tran_active_transactions
- sys.dm_tran_active_transactions_TSQL
- dm_tran_active_transactions_TSQL
- dm_tran_active_transactions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_active_transactions dynamic management view
ms.assetid: 154ad6ae-5455-4ed2-b014-e443abe2c6ee
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 967ee5953c146c8b732dddbf493931de2b355a18
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474868"
---
# <a name="sysdm_tran_active_transactions-transact-sql"></a>sys.dm_tran_active_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的事务的信息。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_tran_active_transactions**。  
  
|列名|数据类型|说明|  
|-----------------|---------------|-----------------|  
|transaction_id|**bigint**|实例级而非数据库级的事务 ID。 仅在一个实例的所有数据库中唯一，在所有服务器实例中则不唯一。|  
|name|**nvarchar(32)**|事务名称。 如果事务已被标记且标记的名称替换事务名称，则此名称被覆盖。|  
|transaction_begin_time|**datetime**|事务的开始时间。|  
|transaction_type|**int**|事务的类型。<br /><br /> 1 = 读/写事务<br /><br /> 2 = 只读事务<br /><br /> 3 = 系统事务<br /><br /> 4 = 分布式事务|  
|transaction_uow|**uniqueidentifier**|分布式事务的事务工作单元 (UOW) 标识符。 MS DTC 使用 UOW 标识符来处理分布式事务。|  
|transaction_state|**int**|0 = 事务尚未完全初始化。<br /><br /> 1 = 事务已初始化但尚未启动。<br /><br /> 2 = 事务处于活动状态。<br /><br /> 3 = 事务已结束。 该状态用于只读事务。<br /><br /> 4 = 已对分布式事务启动提交进程。 仅用于分布式事务。 分布式事务仍然处于活动状态，但不会进行进一步处理。<br /><br /> 5 = 事务处于准备就绪状态且等待解析。<br /><br /> 6 = 事务已提交。<br /><br /> 7 = 事务正在被回滚。<br /><br /> 8 = 事务已回滚。|  
|transaction_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|transaction_status2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|dtc_state|**int**|**适用** 于： [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 通过 [当前版本](/previous-versions/azure/ee336279(v=azure.100)))  (初始版本。<br /><br /> 1 = 活动<br /><br /> 2 = 准备就绪<br /><br /> 3 = 已提交<br /><br /> 4 = 中止<br /><br /> 5 = 已恢复|  
|dtc_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|dtc_isolation_level|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|filestream_transaction_id|**varbinary(128)**|**适用** 于： [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 通过 [当前版本](/previous-versions/azure/ee336279(v=azure.100)))  (初始版本。<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_tran_session_transactions &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)   
 [sys.dm_tran_database_transactions &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md)   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与事务相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
