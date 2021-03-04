---
description: sys.dm_exec_background_job_queue (Transact-SQL)
title: sys.dm_exec_background_job_queue (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_background_job_queue
- sys.dm_exec_background_job_queue_TSQL
- dm_exec_background_job_queue_TSQL
- sys.dm_exec_background_job_queue
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_background_job_queue dynamic management function
ms.assetid: 05d9884f-b74c-4e3c-a23b-c90c1ea5ef02
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6c7fbf4e7d6ed7b9f541d12215b10f311a14ee2c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839302"
---
# <a name="sysdm_exec_background_job_queue-transact-sql"></a>sys.dm_exec_background_job_queue (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  对计划异步（后台）执行的每个查询处理器作业返回一行。  
  
> **注意！！** 若要从或调用此 **[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]** **[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]** ，请使用名称 **sys.dm_pdw_nodes_exec_background_job_queue**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**time_queued**|**datetime**|作业添加到队列中的时间。|  
|**job_id**|**int**|作业标识符。|  
|database_id|**int**|执行作业所在的数据库。|  
|**object_id1**|**int**|值取决于作业类型。 有关详细信息，请参阅“备注”部分。|  
|**object_id2**|**int**|值取决于作业类型。 有关详细信息，请参阅“备注”部分。|  
|**object_id3**|**int**|值取决于作业类型。 有关详细信息，请参阅“备注”部分。|  
|**object_id4**|**int**|值取决于作业类型。 有关详细信息，请参阅“备注”部分。|  
|**error_code**|**int**|如果因失败而重新插入作业，则为错误代码。 如果作业已挂起、未被选取或已完成，则为 NULL。|  
|**request_type**|**smallint**|作业请求的类型。|  
|**retry_count**|**smallint**|从队列中选取作业又因缺少资源或其他原因而将其重新插入队列的次数。|  
|**in_progress**|**smallint**|指示作业是否已开始执行。<br /><br /> 1 = 已开始<br /><br /> 0 = 仍在等待|  
|**session_id**|**smallint**|会话标识符。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="remarks"></a>备注  
 该视图只返回有关异步更新统计作业的信息。 有关异步更新统计信息的详细信息，请参阅 [统计](../../relational-databases/statistics/statistics.md)信息。  
  
 通过 **object_id4** **object_id1** 的值取决于作业请求的类型。 下表总结了这些列对于不同作业类型的含义。  
  
|请求类型|object_id1|object_id2|object_id3|object_id4|  
|------------------|-----------------|-----------------|-----------------|-----------------|  
|异步更新统计信息|表或视图的 ID|统计信息 ID|未使用|未使用|  
  
## <a name="examples"></a>示例  
 以下示例为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的每个数据库返回后台队列中活动异步作业的数量。  
  
```  
SELECT DB_NAME(database_id) AS [Database], COUNT(*) AS [Active Async Jobs]  
FROM sys.dm_exec_background_job_queue  
WHERE in_progress = 1  
GROUP BY database_id;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与执行相关的动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [统计信息](../../relational-databases/statistics/statistics.md)   
 [KILL STATS JOB (Transact-SQL)](../../t-sql/language-elements/kill-stats-job-transact-sql.md)  
  
