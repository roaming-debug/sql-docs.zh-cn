---
title: 'sys.dm_exec_query_optimizer_memory_gateways (Transact-sql) '
description: 返回用于限制并发查询优化的资源信号量的当前状态
ms.custom: seo-dt-2019
ms.date: 04/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: reference
f1_keywords:
- dm_exec_query_optimizer_memory_gateways_TSQL
- dm_exec_query_optimizer_memory_gateways
- sys.dm_exec_query_optimizer_memory_gateways_TSQL
- sys.dm_exec_query_optimizer_memory_gateways
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_optimizer_memory_gateways dynamic management view
author: josack
ms.author: josack
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 66e5d3e4509b8593a41a53348e466054ee01b4f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342908"
---
# <a name="sysdm_exec_query_optimizer_memory_gateways-transact-sql"></a>sys.dm_exec_query_optimizer_memory_gateways (Transact-sql) 

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

返回用于限制并发查询优化的资源信号量的当前状态。

|列|类型|描述|  
|----------|---------------|-----------------|  
|**pool_id**|**int**|Resource Governor 下的资源池 ID|  
|name|**sysname**| (小型网关、中型网关、大网关) 编译入口名称|
|**max_count**|**int**|已配置的并发编译的最大计数|
|**active_count**|**int**|当前在此入口中编译的活动计数|
|**waiter_count**|**int**|此入口中的等待进程数|
|**threshold_factor**|**bigint**|阈值因子，定义查询优化使用的最大内存部分。  对于小型网关，threshold_factor 指示一个查询的最大优化器内存使用情况（以字节为单位），以便在小型网关中获取访问权限。  对于中型和大型网关，threshold_factor 显示此入口的可用服务器内存的部分。 它在计算入口的内存使用量阈值时用作除数。|
|**threshold**|**bigint**|下一个阈值内存（字节）。  如果其内存消耗达到此阈值，则需要查询才能获得对此网关的访问权限。  如果不需要查询即可获取此网关的访问权限，则为 "-1"。|
|**is_active**|**bit**|是否需要查询来传递当前入口。|


## <a name="permissions"></a>权限  
SQL Server 要求对服务器具有 VIEW SERVER STATE 权限。

Azure SQL 数据库需要数据库中的 VIEW DATABASE STATE 权限。


## <a name="remarks"></a>备注  
SQL Server 使用分层网关方法来限制允许的并发编译数。  使用三个网关，包括小型、中型和大型。 通过更大的编译内存（需要使用者），网关可帮助防止总体内存资源耗尽。

等待网关导致延迟编译。 除了编译延迟外，限制请求还会将关联的 RESOURCE_SEMAPHORE_QUERY_COMPILE 等待类型累积。 RESOURCE_SEMAPHORE_QUERY_COMPILE 等待类型可能表示查询正在使用大量内存进行编译，并且该内存已用尽，或者有足够的内存可用，但是特定网关上的可用单元已经耗尽。 **Sys.dm_exec_query_optimizer_memory_gateways** 的输出可用于排查没有足够的内存来编译查询执行计划的情况。  

## <a name="examples"></a>示例  

### <a name="a-viewing-statistics-on-resource-semaphores"></a>A. 查看资源信号量的统计信息  
SQL Server 的此实例的当前优化器内存网关统计信息是什么？

```  
SELECT [pool_id], [name], [max_count], [active_count],
       [waiter_count], [threshold_factor], [threshold],
       [is_active]
FROM sys.dm_exec_query_optimizer_memory_gateways;   

```  

## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](./system-dynamic-management-views.md)   
 [与执行相关的动态管理视图和函数 (Transact-SQL)](./execution-related-dynamic-management-views-and-functions-transact-sql.md)  
[如何使用 DBCC MEMORYSTATUS 命令监视 SQL Server 2005](https://support.microsoft.com/help/907877/how-to-use-the-dbcc-memorystatus-command-to-monitor-memory-usage-on-sql-server-2005) 
 上的内存使用量[大型查询编译在 SQL Server 2014 中等待 RESOURCE_SEMAPHORE_QUERY_COMPILE](https://support.microsoft.com/help/3024815/large-query-compilation-waits-on-resource-semaphore-query-compile-in-sql-server-2014)
