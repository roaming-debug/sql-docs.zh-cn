---
description: 'sys.dm_pdw_sys_info (Transact-sql) '
title: sys.dm_pdw_sys_info (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 686976b4-2d5d-4d64-bf12-56eba1dc59b1
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 837debe38f44a960b2fe7efec40379e822c5fe57
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440712"
---
# <a name="sysdm_pdw_sys_info-transact-sql"></a>sys.dm_pdw_sys_info (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  提供了一组在设备上反映总体活动的设备级计数器。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|total_sessions|**int**|系统中当前的会话数。|0到 max_active_sessions (请参阅下面) 。|  
|idle_sessions|**int**|当前空闲的会话数。||  
|active_requests|**int**|当前正在运行的活动请求数。||  
|queued_requests|**int**|当前排队的请求数。||  
|active_loads|**int**|系统中当前正在运行的加载的数目。||  
|queued_loads|**int**|等待执行的排队加载数。||  
|active_backups|**int**|当前正在运行的备份数。||  
|active_restores|**int**|当前正在运行的备份恢复次数。||  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
