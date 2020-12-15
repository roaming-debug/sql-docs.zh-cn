---
description: sys.dm_exec_external_operations (Transact-SQL)
title: sys.dm_exec_external_operations (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- DM_EXEC_EXTERNAL_OPERATIONS_TSQL
- DM_EXEC_EXTERNAL_OPERATIONS
- SYS.DM_EXEC_EXTERNAL_OPERATIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- sys.dm_exec_external_operations management view
- dm_exec_external_operations management view
ms.assetid: d268217a-85b8-4b7f-9cd1-87865eba2be1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: abf383a1dddac7ac6cf3b347b19edd5023b21154
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464518"
---
# <a name="sysdm_exec_external_operations-transact-sql"></a>sys.dm_exec_external_operations (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  捕获有关外部 PolyBase 操作的信息。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**nvarchar(32)**|与 PolyBase 查询关联的唯一查询标识符|请参阅[sys.dm_exec_requests &#40;transact-sql](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)中的 ID&#41;|  
|step_index|**int**|查询步骤的索引|请参阅[sys.dm_exec_distributed_request_steps &#40;transact-sql](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-request-steps-transact-sql.md)中的 step_index&#41;|  
|operation_ 类型|**nvarchar(128)**|描述 Hadoop 操作或其他外部操作|"外部 Hadoop 操作"|  
|operation_ 名称|**nvarchar(4000)**|指示作业状态的百分比 (所用输入量的百分比) |0-1-乘以系数 100 (完成) |  
|map_ 进度|**float**|指示缩减作业状态的百分比（如果有）|0-1-乘以系数 100 (完成) |  
  
## <a name="see-also"></a>另请参阅  
 [通过动态管理视图进行 PolyBase 故障排除](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
