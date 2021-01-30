---
description: sys.dm_exec_distributed_request_steps (Transact-SQL)
title: sys.dm_exec_distributed_request_steps (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.DM_EXEC_DISTRIBUTED_REQUEST_STEPS_TSQL
- DM_EXEC_DISTRIBUTED_REQUEST_STEPS_TSQL
- DM_EXEC_DISTRIBUTED_REQUEST_STEPS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_distributed_request_steps
- sys.dm_exec_distributed_request_steps management view
ms.assetid: 1954541d-b716-4e03-8fcc-7022f428e01d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 23992d8ed0c2974c78f3fcd5db3686683b9d6ee3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160055"
---
# <a name="sysdm_exec_distributed_request_steps-transact-sql"></a>sys.dm_exec_distributed_request_steps (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  保存有关组成给定 PolyBase 请求或查询的所有步骤的信息。 每个查询步骤在表中各占一行。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**int**|execution_id 和 step_index 构成此视图的键。 与请求关联的唯一数字 id。|请参阅 [sys.dm_exec_requests &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)中的 ID。|  
|step_index|**int**|此步骤在组成请求的步骤序列中的位置。|对于包含 n 个步骤的请求，为 0 (n-1) 。|  
|operation_type|**nvarchar(128)**|此步骤表示的操作的类型。|"MoveOperation"、"OnOperation"、"RandomIDOperation"、"RemoteOperation"、"ReturnOperation"、"ShuffleMoveOperation"、"TempTablePropertiesOperation"、"DropDiagnosticsNotifyOperation"、"HadoopShuffleOperation"、"HadoopBroadCastOperation"、"HadoopRoundRobinOperation"|  
|distribution_type|**nvarchar(32)**|步骤的执行位置。|"AllComputeNodes"、"AllDistributions"、"ComputeNode"、"分发"、"AllNodes"、"SubsetNodes"、"SubsetDistributions"、"未指定"。|  
|location_type|**nvarchar(32)**|步骤的执行位置。|"Compute"、"Head" 或 "DMS"。 所有数据移动步骤显示 "DMS"。|  
|status|**nvarchar(32)**|此步骤的状态|"挂起"、"正在运行"、"完成"、"失败"、"UndoFailed"、"PendingCancel"、"已取消"、"撤消"、"已中止"|  
|error_id|**nvarchar (36)**|与此步骤关联的错误的唯一 id （如果有）|请参阅 [sys.dm_exec_compute_node_errors &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md)的 id，如果未发生错误，则为 NULL。|  
|start_time|**datetime**|步骤开始执行的时间|小于或等于当前时间，大于或等于此步骤所属的查询 end_compile_time。|  
|end_time|**datetime**|此步骤完成执行、已取消或失败的时间。|小于或等于当前时间，大于或等于 start_time，则将设置为 NULL 以查看当前正在执行或已排队的步骤。|  
|total_elapsed_time|**int**|执行查询步骤的总时间（毫秒）|介于0与 end_time 与 start_time 之间的差异。 对于排队步骤，为0。|  
|row_count|**bigint**|此请求更改或返回的总行数|0表示未更改或返回数据的步骤，否则为受影响的行数。 对于 DMS 步骤，将设置为-1。|  
|命令|nvarchar(4000)|保存此步骤的命令的完整文本。|步骤的任何有效请求字符串。 如果长度超过4000个字符，则截断。|  
  
## <a name="see-also"></a>另请参阅  
 [通过动态管理视图进行 PolyBase 故障排除](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
