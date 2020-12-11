---
description: sys.dm_tran_current_snapshot (Transact-SQL)
title: sys.dm_tran_current_snapshot (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_tran_current_snapshot_TSQL
- dm_tran_current_snapshot
- dm_tran_current_snapshot_TSQL
- sys.dm_tran_current_snapshot
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_current_snapshot dynamic management view
ms.assetid: 7509d595-c0e1-4237-a5ac-b41ad934544c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c1d6681ec21993481e7ff1c395b0ed2c61eb5190
ms.sourcegitcommit: 2991ad5324601c8618739915aec9b184a8a49c74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97334411"
---
# <a name="sysdm_tran_current_snapshot-transact-sql"></a>sys.dm_tran_current_snapshot (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回一个虚拟表，该表显示当前快照事务启动时所有处于活动状态的事务。 如果当前事务不是快照事务，则该函数不返回行。 **sys.dm_tran_current_snapshot** 类似于 **sys.dm_tran_transactions_snapshot**，但 **sys.dm_tran_current_snapshot** 只返回当前快照事务的活动事务。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_tran_current_snapshot**。  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.dm_tran_current_snapshot  
```  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**transaction_sequence_num**|**bigint**|活动事务的事务序列号。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="examples"></a>示例  
 下面的示例使用具有四个并发事务的测试方案，每一个事务都由事务序列号 (XSN) 标识，并在 ALLOW_SNAPSHOT_ISOLATION 和 READ_COMMITTED_SNAPSHOT 选项设置为 ON 的数据库中运行。 下列事务正在运行：  
  
-   XSN-57 是序列化隔离下的更新操作。  
  
-   XSN-58 与 XSN-57 相同。  
  
-   XSN-59 是快照隔离下的选择操作。  
  
-   XSN-60 与 XSN-59 相同。  
  
 下面的查询在 XSN-59 的作用域中执行。  
  
```  
SELECT   
    transaction_sequence_num  
  FROM sys.dm_tran_current_snapshot;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
transaction_sequence_num  
------------------------  
57  
58  
```  
  
 结果显示 XSN-57 和 XSN-58 在快照事务 XSN-59 启动时已处于活动状态。 即使 XSN-57 和 XSN-58 提交或回滚之后，相同的结果也将一直持续到快照事务完成。  
  
 在 XSN-60 的作用域中执行相同的查询。  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
transaction_sequence_num  
------------------------  
57  
58  
59  
```  
  
 XSN-60 的输出不仅包括为 XSN-59 显示的相同事务，而且还包括在 XSN-60 启动时处于活动状态的 XSN-59。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与事务相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


