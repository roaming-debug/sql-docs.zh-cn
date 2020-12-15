---
description: 'sys.dm_pdw_lock_waits (Transact-sql) '
title: sys.dm_pdw_lock_waits (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 8ef966f8-d14e-40d3-9626-3508ada9b8fb
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 1b68d0e88b3ce1cfa46a6efcdac98f04555dd2ef
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472758"
---
# <a name="sysdm_pdw_lock_waits-transact-sql"></a>sys.dm_pdw_lock_waits (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  保存有关正在等待锁的请求的信息。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|wait_id|**bigint**|请求在等待列表中的位置。|从0开始的序号。 所有等待条目都不是唯一的。|  
|session_id|**nvarchar(32)**|发生等待状态的会话的 ID。|请参阅 [sys.dm_pdw_exec_sessions &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md)中的 session_id。|  
|type|**nvarchar(255)**|此项表示的等待类型。|可能的值：<br /><br /> 共享<br /><br /> SharedUpdate<br /><br /> ExclusiveUpdate<br /><br /> 排他|  
|object_type|**nvarchar(255)**|受等待影响的对象的类型。|可能的值：<br /><br /> OBJECT<br /><br /> DATABASE<br /><br /> SYSTEM<br /><br /> SCHEMA<br /><br /> APPLICATION|  
|object_name|**nvarchar (386)**|受等待影响的指定对象的名称或 GUID。|表和视图显示有由三部分组成的名称。<br /><br /> 索引和统计信息显示为由四部分组成的名称。<br /><br /> "名称"、"主体" 和 "数据库" 是字符串名称。|  
|request_id|**nvarchar(32)**|发生等待状态的请求的 ID。|请求的 ID。<br /><br /> 这是负载请求的 GUID。|  
|request_time|**datetime**|请求锁或资源的时间。||  
|acquire_time|**datetime**|获取锁或资源的时间。||  
|state|**nvarchar(50)**|等待状态的状态。|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|priority|**int**|等待项的优先级。|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
