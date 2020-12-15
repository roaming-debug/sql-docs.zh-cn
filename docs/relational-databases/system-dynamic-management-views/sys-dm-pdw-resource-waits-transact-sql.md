---
description: 'sys.dm_pdw_resource_waits (Transact-sql) '
title: sys.dm_pdw_resource_waits (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/26/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: a43ce9a2-5261-41e3-97f0-555ba05ebed9
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 95e974c7a62722ea63510504d057b2e67370925b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440722"
---
# <a name="sysdm_pdw_resource_waits-transact-sql"></a>sys.dm_pdw_resource_waits (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  显示中所有资源类型的等待信息 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|wait_id|**bigint**|请求在等待列表中的位置。|从0开始的序号。 所有等待条目都不是唯一的。|  
|session_id|**nvarchar(32)**|发生等待状态的会话的 ID。|请参阅 [sys.dm_pdw_exec_sessions &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md)中的 session_id。|  
|type|**nvarchar(255)**|此项表示的等待类型。|可能的值：<br /><br /> 连接<br /><br /> 并发查询并发<br /><br /> 分布式查询并发<br /><br /> DMS 并发<br /><br /> 备份并发|  
|object_type|**nvarchar(255)**|受等待影响的对象的类型。|可能的值：<br /><br /> **对象**<br /><br /> **DATABASE**<br /><br /> **主板**<br /><br /> **SCHEMA**<br /><br /> **程序**|  
|object_name|**nvarchar (386)**|受等待影响的指定对象的名称或 GUID。|表和视图显示有由三部分组成的名称。<br /><br /> 索引和统计信息显示为由四部分组成的名称。<br /><br /> "名称"、"主体" 和 "数据库" 是字符串名称。|  
|request_id|**nvarchar(32)**|发生等待状态的请求的 ID。|请求的 QID 标识符。<br /><br /> 加载请求的 GUID 标识符。|  
|request_time|**datetime**|请求锁或资源的时间。||  
|acquire_time|**datetime**|获取锁或资源的时间。||  
|state|**nvarchar(50)**|等待状态的状态。|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|priority|**int**|等待项的优先级。|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|concurrency_slots_used|**int**|内部|请参阅下面的[监视资源等待](#monitor-resource-waits)|  
|resource_class|**nvarchar (20)**|内部 |请参阅下面的[监视资源等待](#monitor-resource-waits)|  
  
## <a name="monitor-resource-waits"></a>监视资源等待 
引入 [工作负荷组](/azure/sql-data-warehouse/sql-data-warehouse-workload-isolation)后，并发槽将不再适用。  使用以下查询和 `resources_requested` 列了解执行请求所需的资源。

```sql
select rw.wait_id
      ,rw.session_id
      ,rw.type
      ,rw.object_type
      ,rw.object_name
      ,rw.request_id
      ,rw.request_time
      ,rw.acquire_time
      ,rw.state
      ,resources_requested = s.effective_request_min_resource_grant_percent
      ,r.group_name
  from sys.dm_workload_management_workload_groups_stats s
  join sys.dm_pdw_exec_requests r on r.group_name = s.name collate SQL_Latin1_General_CP1_CI_AS
  join sys.dm_pdw_resource_waits rw on rw.request_id = r.request_id
```

## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
