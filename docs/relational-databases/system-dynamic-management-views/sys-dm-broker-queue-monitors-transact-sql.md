---
description: sys.dm_broker_queue_monitors (Transact-SQL)
title: sys.dm_broker_queue_monitors (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_broker_queue_monitors
- sys.dm_broker_queue_monitors_TSQL
- dm_broker_queue_monitors_TSQL
- sys.dm_broker_queue_monitors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_queue_monitors dynamic management view
ms.assetid: 401207dc-ef4a-4a3f-879c-76dcbb52d6bc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e587a9f1507e2640ca91e72d134fadb8030bf58b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174543"
---
# <a name="sysdm_broker_queue_monitors-transact-sql"></a>sys.dm_broker_queue_monitors (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对于实例中的每个队列监视器，相应地返回一行。 队列监视器负责管理队列的激活。  
  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|数据库的对象标识符，该数据库包含了监视器所观察的队列。 可以为 null.|  
|**queue_id**|**int**|监视器所观察的队列的对象标识符。 可以为 null.|  
|**state**|**nvarchar(32)**|监视器的状态。 可以为 null. 这是以下各项之一：<br /><br /> **不用**<br /><br /> **NOTIFIED**<br /><br /> **RECEIVES_OCCURRING**|  
|**last_empty_rowset_time**|**datetime**|来自队列的 RECEIVE 上次返回空结果时的时间。 可以为 null.|  
|**last_activated_time**|**datetime**|此队列监视器上次激活存储过程时的时间。 可以为 null.|  
|**tasks_waiting**|**int**|当前正在 RECEIVE 语句中等待此队列的会话数。 可以为 null.<br /><br /> 注意：此数字包括执行 receive 语句的任何会话，不管队列监视器是否启动了该会话。 它表示您是否与 RECEIVE 一起使用 WAITFOR。 基本上，这些任务都在等待到达队列的消息。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-current-status-queue-monitor"></a>A. 当前状态队列监视器  
 此方案提供了所有消息队列的当前状态。  
  
```  
SELECT t1.name AS [Service_Name],  t3.name AS [Schema_Name],  t2.name AS [Queue_Name],    
CASE WHEN t4.state IS NULL THEN 'Not available'   
ELSE t4.state   
END AS [Queue_State],    
CASE WHEN t4.tasks_waiting IS NULL THEN '--'   
ELSE CONVERT(VARCHAR, t4.tasks_waiting)   
END AS tasks_waiting,   
CASE WHEN t4.last_activated_time IS NULL THEN '--'   
ELSE CONVERT(varchar, t4.last_activated_time)   
END AS last_activated_time ,    
CASE WHEN t4.last_empty_rowset_time IS NULL THEN '--'   
ELSE CONVERT(varchar,t4.last_empty_rowset_time)   
END AS last_empty_rowset_time,   
(   
SELECT COUNT(*)   
FROM sys.transmission_queue t6   
WHERE (t6.from_service_name = t1.name) ) AS [Tran_Message_Count]   
FROM sys.services t1    INNER JOIN sys.service_queues t2   
ON ( t1.service_queue_id = t2.object_id )     
INNER JOIN sys.schemas t3 ON ( t2.schema_id = t3.schema_id )    
LEFT OUTER JOIN sys.dm_broker_queue_monitors t4   
ON ( t2.object_id = t4.queue_id  AND t4.database_id = DB_ID() )    
INNER JOIN sys.databases t5 ON ( t5.database_id = DB_ID() );  
```  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与 Service Broker 有关的动态管理视图 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  

