---
description: sys.dm_tran_session_transactions (Transact-SQL)
title: sys.dm_tran_session_transactions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_tran_session_transactions
- sys.dm_tran_session_transactions
- sys.dm_tran_session_transactions_TSQL
- dm_tran_session_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_session_transactions dynamic management view
ms.assetid: c7157491-58c2-49fe-87d7-0c9723113adf
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5dcb5a90c960d616ced6852e3b42bc29f5e7e92d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101441"
---
# <a name="sysdm_tran_session_transactions-transact-sql"></a>sys.dm_tran_session_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回关联事务和会话的相关信息。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_tran_session_transactions**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|session_id|**int**|正在运行事务的会话的 ID。|  
|transaction_id|**bigint**|事务的 ID。|  
|transaction_descriptor|**二进制 (8)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在与客户端驱动程序通信时使用的事务标识符。|  
|enlist_count|**int**|正在处理事务的会话中的活动请求数。|  
|is_user_transaction|**bit**|1 = 事务由用户请求启动。<br /><br /> 0 = 系统事务。|  
|is_local|**bit**|1 = 本地事务。<br /><br /> 0 = 分布式事务或登记的绑定会话事务。|  
|is_enlisted|**bit**|1 = 登记的分布式事务。<br /><br /> 0 = 非登记的分布式事务。|  
|is_bound|**bit**|1 = 事务在通过绑定会话的会话中处于活动状态。<br /><br /> 0 = 事务在通过绑定会话的会话中处于非活动状态。|  
|open_transaction_count||每个会话的打开的事务数。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="remarks"></a>备注  
 通过绑定会话和分布式事务，一个事务可以运行于多个会话中。 在这种情况下，sys.dm_tran_session_transactions 将对同一 transaction_id 显示多个行，当前运行事务的每个会话对应一行。  
  
 通过使用多个活动的结果集 (MARS) 以自动提交模式执行多个请求，一个会话中可以有多个活动事务。 在这种情况下，sys.dm_tran_session_transactions 将对同一 session_id 显示多个行，在该会话中运行的每个事务对应一行。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与事务相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


