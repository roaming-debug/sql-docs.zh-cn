---
description: sys.dm_exec_external_work (Transact-SQL)
title: sys.dm_exec_external_work (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DM_EXEC_EXTERNAL_WORK
- DM_EXEC_EXTERNAL_WORK_TSQL
- SYS.DM_EXEC_EXTERNAL_WORK_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_external_work management view
- dm_exec_external_work management view
- PolyBase,views
- PolyBase
ms.assetid: 7597d97b-1fde-4135-ac35-4af12968f300
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 10f7f8ae36a35a39a303d9548b828768282df3f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159983"
---
# <a name="sysdm_exec_external_work-transact-sql"></a>sys.dm_exec_external_work (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  返回有关每个计算节点上每个工作负荷的工作负荷的信息。  
  
 查询 sys.dm_exec_external_work 标识与外部数据 (源（如 Hadoop 或外部 SQL Server) ）进行通信的工作。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|`nvarchar(32)`|关联的 PolyBase 查询的唯一标识符。|请参阅 [sys.dm_exec_requests &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)中的 *request_ID* 。|  
|step_index|`int`|此工作线程正在执行的请求。|请参阅 [sys.dm_exec_requests &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)中的 *step_index* 。|  
|dms_step_index|`int`|此工作线程正在执行的 DMS 计划的步骤。|请参阅 [&#40;transact-sql&#41;sys.dm_exec_dms_workers ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md)。|  
|compute_node_id|`int`|正在运行辅助角色的节点。|请参阅 [&#40;transact-sql&#41;sys.dm_exec_compute_nodes ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md)。|  
|type|`nvarchar(60)`|外部工作的类型。|"文件拆分"|  
|work_id|`int`|实际拆分的 ID。|大于或等于0。|  
|input_name|`nvarchar(4000)`|要读取的输入的名称|使用 Hadoop 时的文件名。|  
|read_location|`bigint`|偏移或读取位置。|要读取的文件的偏移量。|  
|bytes_processed|`bigint`|此辅助进程分配用于处理数据的总字节数。 这不一定表示查询返回的总数据 |大于或等于0。|  
|length|`bigint`|Hadoop 或 HDFS 块的长度（如果为 Hadoop）|用户可定义的。 默认值为 Ed-64m|  
|status|`nvarchar(32)`|辅助进程的状态|挂起，处理，已完成，失败，已中止|  
|start_time|`datetime`|工作开始||  
|end_time|`datetime`|工作结束||  
|total_elapsed_time|`int`|总时间（毫秒）||
|compute_pool_id|`int`|池的唯一标识符。|

## <a name="see-also"></a>另请参阅  
 [通过动态管理视图进行 PolyBase 故障排除](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
