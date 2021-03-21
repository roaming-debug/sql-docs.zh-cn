---
description: sys.dm_tran_active_snapshot_database_transactions (Transact-SQL)
title: sys.dm_tran_active_snapshot_database_transactions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_active_snapshot_database_transactions_TSQL
- dm_tran_active_snapshot_database_transactions
- sys.dm_tran_active_snapshot_database_transactions
- dm_tran_active_snapshot_database_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_active_snapshot_database_transactions dynamic management view
ms.assetid: 55b83f9c-da10-4e65-9846-f4ef3c0c0f36
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f766ed71d42f761e03b1b63c74aad638f022c177
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756467"
---
# <a name="sysdm_tran_active_snapshot_database_transactions-transact-sql"></a>sys.dm_tran_active_snapshot_database_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中，此动态管理视图为所有生成行版本或可能访问行版本的活动事务返回虚拟表。 存在下列一个或多个条件时便包含事务：  
  
-   当 ALLOW_SNAPSHOT_ISOLATION 和 READ_COMMITTED_SNAPSHOT 数据库选项之一或全部设置为 ON 时：  
  
    -   在快照隔离级别或使用行版本控制的已提交读隔离级别下运行的每一个事务都占有一行。  
  
    -   导致在当前数据库中创建行版本的每一个事务都占有一行。 例如，事务通过在当前数据库中更新或删除行来生成行版本。  
  
-   触发器触发之后，在其中执行触发器的事务占有一行。  
  
-   运行联机索引过程时，创建索引的事务占有一行。  
  
-   多个活动的结果集 (MARS) 会话启用之后，访问行版本的每一个事务都占有一行。  
  
 此动态管理视图不包括系统事务。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_tran_active_snapshot_database_transactions**。  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.dm_tran_active_snapshot_database_transactions  
```  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**transaction_id**|**bigint**|分配给事务的唯一标识号。 事务 ID 主要用于在锁定操作中标识事务。|  
|**transaction_sequence_num**|**bigint**|事务序列号。 它是在事务启动时分配给事务的唯一序列号。 不生成版本记录且不使用快照扫描的事务不会接收到事务序列号。|  
|**commit_sequence_num**|**bigint**|指示事务何时完成（提交或停止）的序列号。 对于活动事务，该值为 NULL。|  
|**is_snapshot**|**int**|0 = 不是快照隔离事务。<br /><br /> 1 = 是快照隔离事务。|  
|**session_id**|**int**|启动事务的会话的 ID。|  
|**first_snapshot_sequence_num**|**bigint**|拍摄快照时处于活动状态的事务的最小事务序列号。 当执行快照事务时，它会拍摄当时所有活动事务的快照。 对于非快照事务，此列显示 0。|  
|**max_version_chain_traversed**|**int**|为查找在事务上一致的版本而遍历的版本链的最大长度。|  
|**average_version_chain_traversed**|**real**|被遍历的版本链中的行版本平均数。|  
|**elapsed_time_seconds**|**bigint**|自事务获取其事务序列号以来所经过的时间。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="remarks"></a>备注  
 **sys.dm_tran_active_snapshot_database_transactions** 报告分配有事务序列号 (.xsn) 的事务。 XSN 在事务首次访问版本存储区时分配。 在为快照隔离或使用行版本控制的已提交读隔离启用的数据库中，下面的示例说明何时将 XSN 分配给事务：  
  
-   如果事务在可序列化隔离级别下运行，则 XSN 在事务首次执行导致创建行版本的语句（例如，UPDATE 操作）时分配。  
  
-   如果事务在快照隔离下运行，则 XSN 在任何数据操作语言 (DML) 语句（包括 SELECT 操作）执行时分配。  
  
 对于[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例中启动的每个事务而言，事务序列号按序列递增。  
  
## <a name="examples"></a>示例  
 下面的示例使用具有四个并发事务的测试方案，每一个事务都由事务序列号 (XSN) 标识，并在 ALLOW_SNAPSHOT_ISOLATION 和 READ_COMMITTED_SNAPSHOT 选项设置为 ON 的数据库中运行。 下列事务正在运行：  
  
-   XSN-57 是序列化隔离下的更新操作。  
  
-   XSN-58 与 XSN-57 相同。  
  
-   XSN-59 是快照隔离下的选择操作  
  
-   XSN-60 与 XSN-59 相同。  
  
 执行以下查询。  
  
```  
SELECT   
    transaction_id,  
    transaction_sequence_num,  
    commit_sequence_num,  
    is_snapshot session_id,  
    first_snapshot_sequence_num,  
    max_version_chain_traversed,  
    average_version_chain_traversed,  
    elapsed_time_seconds  
  FROM sys.dm_tran_active_snapshot_database_transactions;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
transaction_id  transaction_sequence_num  commit_sequence_num  
--------------  ------------------------  -------------------  
9295            57                        NULL  
9324            58                        NULL  
9387            59                        NULL  
9400            60                        NULL  
  
is_snapshot  session_id   first_snapshot_sequence_num  
-----------  -----------  ---------------------------  
0            54           0  
0            53           0  
1            52           57  
1            51           57  
  
max_version_chain_traversed  average_version_chain_traversed  
---------------------------  -------------------------------  
0                            0  
0                            0  
1                            1  
1                            1  
  
elapsed_time_seconds  
--------------------  
419  
397  
359  
333  
```  
  
 以下信息将评估 **sys.dm_tran_active_snapshot_database_transactions** 的结果：  
  
-   XSN-57：由于此事务未在快照隔离下运行，因此， `is_snapshot` 值 `first_snapshot_sequence_num` 为 `0` 。 由于 ALLOW_SNAPSHOT_ISOLATION 或 READ_COMMITTED_SNAPSHOT 数据库选项中有一个为 ON 或两者均为 ON，因此 `transaction_sequence_num` 表明已为此事务分配事务序列号。  
  
-   XSN-58：此事务未在快照隔离下运行，因此适用与 XSN-57 相同的信息。  
  
-   XSN-59：这是在快照隔离下运行的第一个活动事务。 此事务读取在 XSN-57 之前提交的数据，如由 `first_snapshot_sequence_num` 指示的数据。 此事务的输出结果还说明，为一行遍历的最大版本链是 `1`，并且该事务为所访问的每一行遍历的版本链平均值为 `1`。 这表示事务 XSN-57、XSN-58 和 XSN-60 尚未对行进行修改并提交。  
  
-   XSN-60：这是在快照隔离下运行的第二个事务。 输出显示了与 XSN-59 相同的信息。  
  
## <a name="see-also"></a>另请参阅  
 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与事务相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
