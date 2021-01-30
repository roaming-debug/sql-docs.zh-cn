---
description: 'sys.dm_pdw_os_threads (Transact-sql) '
title: sys.dm_pdw_os_threads (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: ddc12f05-edeb-4848-b6d7-e851684cf044
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 20bde7fc1c72918d0d89e407fd4a175acf3802b2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99139611"
---
# <a name="sysdm_pdw_os_threads-transact-sql"></a>sys.dm_pdw_os_threads (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|受影响的节点的 ID。<br /><br /> pdw_node_id 和 thread_id 构成此视图的键。|请参阅 [sys.dm_pdw_nodes &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md)中的 node_id。|  
|thread_id|**int**|pdw_node_id 和 thread_id 构成此视图的键。||  
|process_id|**int**|||  
|name|**nvarchar(255)**|||  
|priority|**int**|||  
|start_time|**datetime**|||  
|state|**nvarchar(32)**|||  
|wait_reason|**nvarchar(32)**|||  
|total_processor_elapsed_time|**bigint**|线程使用的内核时间总量。||  
|total_user_elapsed_time|**bigint**|线程使用的总用户时间||  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
