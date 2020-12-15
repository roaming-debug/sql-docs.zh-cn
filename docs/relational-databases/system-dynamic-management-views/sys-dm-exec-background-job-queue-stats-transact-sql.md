---
description: sys.dm_exec_background_job_queue_stats (Transact-SQL)
title: sys.dm_exec_background_job_queue_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_background_job_queue_stats
- sys.dm_exec_background_job_queue_stats
- dm_exec_background_job_queue_stats_TSQL
- sys.dm_exec_background_job_queue_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_background_job_queue_stats dynamic management view
ms.assetid: 27f62ab5-46c4-417e-814d-8d6437034d1c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e07b59a7aa57897c882f58087dea4d7476810326
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474918"
---
# <a name="sysdm_exec_background_job_queue_stats-transact-sql"></a>sys.dm_exec_background_job_queue_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  对于每个为异步（后台）执行而提交的查询处理器作业，相应地返回一行，以提供聚合统计信息。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_exec_background_job_queue_stats**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**queue_max_len**|**int**|最大队列长度。|  
|**enqueued_count**|**int**|成功投递到队列的请求数。|  
|**started_count**|**int**|已经开始执行的请求数。|  
|**ended_count**|**int**|最终成功或失败的请求数。|  
|**failed_lock_count**|**int**|由于锁争用或死锁而失败的请求数。|  
|**failed_other_count**|**int**|由于其他原因而失败的请求数。|  
|**failed_giveup_count**|**int**|由于已达到重试限制而失败的请求数。|  
|**enqueue_failed_full_count**|**int**|由于队列已满而失败的排队尝试的数字。|  
|**enqueue_failed_duplicate_count**|**int**|复制排队尝试的数字。|  
|**elapsed_avg_ms**|**int**|请求的平均占用时间（毫秒）。|  
|**elapsed_max_ms**|**int**|最长请求的占用时间（毫秒）。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="remarks"></a>备注  
 该视图只返回有关异步更新统计作业的信息。 有关异步更新统计信息的详细信息，请参阅 [统计](../../relational-databases/statistics/statistics.md)信息。  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="examples"></a>示例  
  
### <a name="a-determining-the-percentage-of-failed-background-jobs"></a>A. 确定失败的后台作业的百分比  
 以下示例针对所有已执行查询返回失败的后台作业的百分比。  
  
```  
SELECT   
        CASE ended_count WHEN 0   
                THEN 'No jobs ended'   
                ELSE CAST((failed_lock_count + failed_giveup_count + failed_other_count) / CAST(ended_count AS float) * 100 AS varchar(20))   
        END AS [Percent Failed]  
FROM sys.dm_exec_background_job_queue_stats;  
GO  
```  
  
### <a name="b-determining-the-percentage-of-failed-enqueue-attempts"></a>B. 确定失败的排队尝试的百分比  
 以下示例针对所有已执行查询返回失败的排队尝试的百分比。  
  
```  
SELECT   
        CASE enqueued_count WHEN 0   
                THEN 'No jobs posted'   
                ELSE CAST((enqueue_failed_full_count + enqueue_failed_duplicate_count) / CAST(enqueued_count + enqueue_failed_full_count + enqueue_failed_duplicate_count AS float) * 100 AS varchar(20))   
        END AS [Percent Enqueue Failed]  
FROM sys.dm_exec_background_job_queue_stats;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与执行相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


