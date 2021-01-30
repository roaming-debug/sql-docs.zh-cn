---
description: sys.dm_broker_activated_tasks (Transact-SQL)
title: sys.dm_broker_activated_tasks (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_broker_activated_tasks
- sys.dm_broker_activated_tasks_TSQL
- dm_broker_activated_tasks
- dm_broker_activated_tasks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_activated_tasks dynamic management view
ms.assetid: 17e6f87f-8f56-489d-9aed-216afc8ef310
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 22c0c0678546899131154b31ec5a04b11c62a107
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202403"
---
# <a name="sysdm_broker_activated_tasks-transact-sql"></a>sys.dm_broker_activated_tasks (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为 Service Broker 激活的每个存储过程返回一行。  
 

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|spid|**int**|已激活存储过程的会话的 ID。 可以为 null.|  
|database_id|**smallint**|定义队列所用数据库的 ID。 可以为 null.|  
|**queue_id**|**int**|为其激活存储过程的队列的对象 ID。 可以为 null.|  
|procedure_name|**nvarchar (650)**|已激活的存储过程的名称。 可以为 null.|  
|**execute_as**|**int**|运行存储过程的用户的 ID。 可以为 null.|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="physical-joins"></a>物理联接  
 ![sys.dm_broker_activated_tasks 的联接](../../relational-databases/system-dynamic-management-views/media/join-dm-broker-activated-tasks-1.gif "sys.dm_broker_activated_tasks 的联接")  
  
## <a name="relationship-cardinalities"></a>关系基数  
  
|From|功能|关系|  
|----------|--------|------------------|  
|dm_broker_activated_tasks.spid|dm_exec_sessions.session_id|一对一|  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与 Service Broker 有关的动态管理视图 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  

