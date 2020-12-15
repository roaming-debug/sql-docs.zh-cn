---
description: 'sys.dm_pdw_os_performance_counters (Transact-sql) '
title: sys.dm_pdw_os_performance_counters (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 0673a8f8-8bed-41eb-8959-a9e3e9e03a65
author: markingmyname
ms.author: maghan
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 67a0bf0b52b0fc4d31400bf009476d76fdc1b1f5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440757"
---
# <a name="sysdm_pdw_os_performance_counters-transact-sql"></a>sys.dm_pdw_os_performance_counters (Transact-sql) 
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  包含有关中节点的 Windows 性能计数器的信息 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|包含计数器的节点的 ID。<br /><br /> pdw_node_id 和 counter_name 构成此视图的键。|请参阅 [sys.dm_pdw_nodes &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md)中的 node_id。|  
|counter_name|**nvarchar(255)**|Windows 性能计数器的名称。||  
|counter_category|**nvarchar(255)**|Windows 性能计数器类别的名称。||  
|instance_name|**nvarchar(255)**|计数器特定实例的名称。||  
|counter_value|**十进制 (38，10)**|计数器的当前值。||  
|last_update_time|**Datetime2 (3)**|上次更新值的时间戳。||  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
