---
title: 标识与可用性组关联的等待
description: 使用 Transact-SQL (T-SQL) 和扩展事件标识与 Always On 可用性组关联的等待。
ms.custom: ag-guide, seodec18
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: afa8caff-f325-48d9-a8ef-a30beab60389
author: rothja
ms.author: jroth
ms.openlocfilehash: 4be170149181263e7d904f5f820a9dd50f6d3801
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347496"
---
# <a name="identify-waits-associated-with-availability-groups"></a>标识与可用性组关联的等待
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  排除 Always On 可用性组延迟问题时，可在动态管理视图 (DMV) 中使用特定于可用性组的等待类型 [sys.dm_os_wait_stats (Transact-SQL)](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) 监视累积统计信息。  
  
 有关使用等待统计信息的常规信息，请参阅 [SQL Server 2005 等待和队列](/previous-versions/sql/sql-server-2005/administrator/cc966413(v=technet.10))。 该文档针对 SQL Server 2005 编写，但其信息适用于更高版本的 SQL Server。  
  
## <a name="query-for-availability-groups-wait-types"></a>针对可用性组等待类型的查询  
 使用下面的 T-SQL 查询检索可用性组等待类型的所有等待统计信息：  
  
```sql  
SELECT * FROM sys.dm_os_wait_stats   
WHERE wait_type LIKE '%hadr%'  
ORDER BY wait_time_ms DESC  
```  
  
 要通过捕获扩展事件监视等待统计信息，请使用下面的 T-SQL 命令。  
  
```sql
CREATE EVENT SESSION [alwayson] ON SERVER   
ADD EVENT sqlos.wait_info(  
    WHERE ([wait_type]=(758) OR [wait_type]=(776) OR [wait_type]=(853) OR [wait_type]=(833)))  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,  
MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)  
GO  
```  
  
 可通过运行下面的查询，查看等待类型的键值映射：  
  
```sql
SELECT * FROM sys.dm_xe_map_values   
WHERE name='wait_types' AND map_value LIKE '%hadr%'   
ORDER BY map_key ASC  
```  
  
## <a name="next-steps"></a>后续步骤  
 [等待的类型](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md#WaitTypes)  
  
