---
description: 'sys.dm_pdw_request_steps (Transact-sql) '
title: sys.dm_pdw_request_steps (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/28/2020
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: cc563e88-0d34-436e-b914-b60d6ee0d50b
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 839a1d906fcd7b6a4a980a7381b4f5fcdcf10d5d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644033"
---
# <a name="sysdm_pdw_request_steps-transact-sql"></a>sys.dm_pdw_request_steps (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  保存有关在中编写给定请求或查询的所有步骤的信息 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。 每个查询步骤在表中各占一行。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|request_id 和 step_index 构成此视图的键。<br /><br /> 与请求关联的唯一数字 ID。|请参阅 [sys.dm_pdw_exec_requests &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md)中的 request_id。|  
|step_index|**int**|request_id 和 step_index 构成此视图的键。<br /><br /> 此步骤在组成请求的步骤序列中的位置。|对于包含 n 个步骤的请求，为 0 (n-1) 。|  
|plan_node_id|**int**|对应于执行计划中该步骤的操作员 ID 的节点 ID。|无|  
|operation_type|**nvarchar(35)**|此步骤表示的操作类型。|**DMS 查询计划操作：** "ReturnOperation"、"PartitionMoveOperation"、"MoveOperation"、"BroadcastMoveOperation"、"ShuffleMoveOperation"、"TrimMoveOperation"、"CopyOperation"、"DistributeReplicatedTableMoveOperation"<br /><br /> **SQL 查询计划操作：** "OnOperation"、"RemoteOperation"<br /><br /> **其他查询计划操作：** 'MetaDataCreateOperation', 'RandomIDOperation'<br /><br /> **读取的外部操作：** 'HadoopShuffleOperation', 'HadoopRoundRobinOperation', 'HadoopBroadcastOperation'<br /><br /> **MapReduce 的外部操作：** 'HadoopJobOperation', 'HdfsDeleteOperation'<br /><br /> **写入的外部操作：** 'ExternalExportDistributedOperation', 'ExternalExportReplicatedOperation', 'ExternalExportControlOperation'<br /><br /> 有关详细信息，请参阅中的 "了解查询计划" [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] 。 <br /><br />  查询计划还可能会受数据库设置的影响。  有关详细信息，请参阅 [ALTER DATABASE SET 选项](../../t-sql/statements/alter-database-transact-sql-set-options.md?bc=%252fazure%252fsql-data-warehouse%252fbreadcrumb%252ftoc.json&toc=%252fazure%252fsql-data-warehouse%252ftoc.json&view=azure-sqldw-latest&preserve-view=true) 。|  
|distribution_type|**nvarchar(32)**|此步骤将经历的分发类型。|"AllNodes"、"AllDistributions"、"AllComputeNodes"、"ComputeNode"、"分发"、"SubsetNodes"、"SubsetDistributions"、"未指定"|  
|location_type|**nvarchar(32)**|步骤的运行位置。|"Compute"、"Control"、"DMS"|  
|status|**nvarchar(32)**|此步骤的状态。|挂起、正在运行、已完成、失败、UndoFailed、PendingCancel、已取消、已中止|  
|error_id|**nvarchar (36)**|与此步骤关联的错误的唯一 ID （如果有）。|请参阅 [&#40;transact-sql&#41;sys.dm_pdw_errors ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md)error_id。 如果未发生错误，则为 NULL。|  
|start_time|**datetime**|步骤开始执行的时间。|小于或等于当前时间，大于或等于此步骤所属的查询 end_compile_time。 有关查询的详细信息，请参阅 [&#40;transact-sql&#41;sys.dm_pdw_exec_requests ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md)。|  
|end_time|**datetime**|此步骤完成执行、已取消或失败的时间。|小于或等于当前时间，大于或等于 start_time。 对于当前正在执行或已排队的步骤，将设置为 NULL。|  
|total_elapsed_time|**int**|查询步骤已运行的总时间（以毫秒为单位）。|介于0与 end_time 与 start_time 之间的差异。 对于排队步骤，为0。<br /><br /> 如果 total_elapsed_time 超过整数的最大值，则 total_elapsed_time 将继续作为最大值。 此条件将生成警告 "已超过最大值。"<br /><br /> 最大值（以毫秒为单位）等效于24.8 天。|  
|row_count|**bigint**|此请求更改或返回的总行数。|受步骤影响的行数。  对于数据操作步骤，大于或等于零。  对于不处理数据的步骤，则为-1。|  
|estimated_rows|**bigint**|查询编译期间计算的工作总行数。|步骤估计的行数。  对于数据操作步骤，大于或等于零。  对于不处理数据的步骤，则为-1。|  
|command|**nvarchar(4000)**|保存此步骤的命令的完整文本。|步骤的任何有效请求字符串。 如果操作的类型为 MetaDataCreateOperation，则为 NULL。 如果长度超过4000个字符，则截断。|  
  
 有关此视图保留的最大行的信息，请参阅中的 "最小值和最大值" 部分中的 "系统查看值最大值" 部分 [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] 。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
