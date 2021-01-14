---
description: 'sys.dm_exec_query_statistics_xml (Transact-sql) '
title: sys.dm_exec_query_statistics_xml (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_query_statistics_xml
- sys.dm_exec_query_statistics_xml_TSQL
- dm_exec_query_statistics_xml_TSQL
- dm_exec_query_statistics_xml
helpviewer_keywords:
- sys.dm_exec_query_statistics_xml management view
ms.assetid: fdc7659e-df41-488e-b2b5-0d79734dfecb
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 9bfce6f56bdf39597def73884f12ca06f6eb56b7
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170269"
---
# <a name="sysdm_exec_query_statistics_xml-transact-sql"></a>sys.dm_exec_query_statistics_xml (Transact-sql) 

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

返回正在进行的请求的查询执行计划。 使用此 DMV 检索带有暂时性统计信息的显示计划 XML。 

## <a name="syntax"></a>语法

```
sys.dm_exec_query_statistics_xml(session_id)  
``` 

## <a name="arguments"></a>参数 
*session_id*  
 要查找的批处理的会话 id。 *session_id* 为 **smallint**。 可以从以下动态管理对象中获取 *session_id* ：  
  
-   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
-   [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
  
-   [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)  

## <a name="table-returned"></a>返回的表

|列名|数据类型|说明|  
|-----------------|---------------|-----------------|
|session_id|**smallint**|会话的 ID。 不可为 Null。|
|request_id|**int**|请求的 ID。 不可为 Null。|
|sql_handle|**varbinary(64)**|是唯一标识查询所属的批处理或存储过程的标记。 可以为 NULL。|
|plan_handle|**varbinary(64)**|是一个标记，用于为当前正在执行的批处理唯一标识查询执行计划。 可以为 NULL。|
|query_plan|**xml**|包含与 *plan_handle* 包含部分统计信息的查询执行计划的运行时显示计划表示形式。 显示计划的格式为 XML。 为包含即席 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句、存储过程调用以及用户定义函数调用等内容的每个批查询生成一个计划。 可以为 NULL。|

## <a name="remarks"></a>备注
从 SP1 开始，此系统函数可用 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 。 请参阅 KB [3190871](https://support.microsoft.com/help/3190871)

此系统函数适用于 **标准** 和 **轻型** 查询执行统计分析基础结构。 有关详细信息，请参阅[查询分析基础结构](../../relational-databases/performance/query-profiling-infrastructure.md)。  

在以下条件下，在 **sys.dm_exec_query_statistics_xml** 的返回表的 **query_plan** 列中不返回显示计划输出：  
  
-   如果不再执行与指定 *session_id* 对应的查询计划，则返回的表的 **query_plan** 列将为 null。 例如，如果在计划句柄被捕获到与 **sys.dm_exec_query_statistics_xml** 一起使用时存在时间延迟，则可能会出现这种情况。  
    
由于 **xml** 数据类型允许的嵌套级别数有限制， **sys.dm_exec_query_statistics_xml** 无法返回满足或超过128嵌套元素级别的查询计划。 在早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，这种情况将导致无法返回查询计划，并生成错误 6335。 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 及更高版本中， **query_plan** 列返回 NULL。   

## <a name="permissions"></a>权限  
在上 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，需要对 `VIEW SERVER STATE` 服务器的权限。  
在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 高级层上，需要具有 `VIEW DATABASE STATE` 数据库中的权限。 在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 标准层和基本层上，需要 **服务器管理员** 或 **Azure Active Directory 管理员** 帐户。

## <a name="examples"></a>示例  
  
### <a name="a-looking-at-live-query-plan-and-execution-statistics-for-a-running-batch"></a>A. 查看正在运行的批处理的实时查询计划和执行统计信息  
 下面的示例查询 **sys.dm_exec_requests** 以查找有趣的查询并 `session_id` 从输出复制其。  
  
```sql  
SELECT * FROM sys.dm_exec_requests;  
GO  
```  
  
 然后，若要获取实时查询计划和执行统计信息，请使用 `session_id` 与系统函数一起复制 **sys.dm_exec_query_statistics_xml**。  
  
```sql  
--Run this in a different session than the session in which your query is running.
SELECT * FROM sys.dm_exec_query_statistics_xml(< copied session_id >);  
GO  
```   

 或结合所有正在运行的请求。  
  
```sql  
--Run this in a different session than the session in which your query is running.
SELECT * FROM sys.dm_exec_requests
CROSS APPLY sys.dm_exec_query_statistics_xml(session_id);  
GO  
```   
  
## <a name="see-also"></a>另请参阅
  [跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  

