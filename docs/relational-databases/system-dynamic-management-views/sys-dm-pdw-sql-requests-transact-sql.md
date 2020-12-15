---
description: 'sys.dm_pdw_sql_requests (Transact-sql) '
title: sys.dm_pdw_sql_requests (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 44e19609-902c-46cf-acdf-19ea75011365
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 360d6c5f5535b0e0702d1c7aede8721f27f88daf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482477"
---
# <a name="sysdm_pdw_sql_requests-transact-sql"></a>sys.dm_pdw_sql_requests (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  将有关所有 SQL Server 查询分发的信息保存为查询中的 SQL 步骤的一部分。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|此 SQL 查询分发所属的查询的唯一标识符。<br /><br /> request_id、step_index 和 distribution_id 构成此视图的键。|请参阅 [sys.dm_pdw_exec_requests &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md)中的 request_id。|  
|step_index|**int**|此分发所属的查询步骤的索引。<br /><br /> request_id、step_index 和 distribution_id 构成此视图的键。|请参阅 [sys.dm_pdw_request_steps &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md)中的 step_index。|  
|pdw_node_id|**int**|运行此查询分发的节点的唯一标识符。|请参阅 [sys.dm_pdw_nodes &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md)中的 node_id。|  
|distribution_id|**int**|运行此查询分发的分发的唯一标识符。<br /><br /> request_id、step_index 和 distribution_id 构成此视图的键。|请参阅 [sys.pdw_distributions &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-pdw-distributions-transact-sql.md)中的 distribution_id。 对于在节点范围内运行的请求，将设置为-1，而不是分布作用域。|  
|status|**nvarchar(32)**|查询分发的当前状态。|挂起、正在运行、已失败、已取消、完成、已中止、CancelSubmitted|  
|error_id|**nvarchar (36)**|与此查询分布关联的错误的唯一标识符（如果有）。|请参阅 [sys.dm_pdw_errors &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md)中的 error_id。 如果未发生错误，则设置为 NULL。|  
|start_time|**datetime**|开始执行查询分发的时间。|小于或等于当前时间，大于或等于此查询分布所属的查询步骤 start_time|  
|end_time|**datetime**|此查询分发完成执行、已取消或失败的时间。|大于或等于开始时间; 如果查询分发正在进行或排队，则设置为 NULL。|  
|total_elapsed_time|**int**|表示查询分发已运行的时间（以毫秒为单位）。|大于或等于0。 与已完成、失败或已取消的查询分发的 start_time 和 end_time 的增量相等。<br /><br /> 如果 total_elapsed_time 超过整数的最大值，则 total_elapsed_time 将继续作为最大值。 此条件将生成警告 "已超过最大值。"<br /><br /> 最大值（以毫秒为单位）等效于24.8 天。|  
|row_count|**bigint**|此查询分布更改或读取的行数。|对于不更改或返回数据的操作（例如 CREATE TABLE 和删除表），为-1。|  
|spid|**int**|运行查询分发的 SQL Server 实例上的会话 id。||  
|command|**nvarchar(4000)**|此查询分发的命令的全文。|任何有效的查询或请求字符串。|  
  
 有关此视图保留的最大行的信息，请参阅 [容量限制](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) 主题中的元数据部分。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
