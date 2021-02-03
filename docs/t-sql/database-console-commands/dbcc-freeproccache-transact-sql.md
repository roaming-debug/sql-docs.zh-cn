---
description: DBCC FREEPROCCACHE (Transact-SQL)
title: DBCC FREEPROCCACHE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/13/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FREEPROCCACHE_TSQL
- FREEPROCCACHE
- DBCC_FREEPROCCACHE_TSQL
- DBCC FREEPROCCACHE
dev_langs:
- TSQL
helpviewer_keywords:
- freeing procedure cache
- removing procedure cache elements
- deleting procedure cache elements
- DBCC FREEPROCCACHE statement
- procedure cache [SQL Server]
- clearing procedure cache
ms.assetid: 0e09d210-6f23-4129-aedb-3d56b2980683
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1efc7b38c4f68a37c9c43b7dd00cfbf09c6dc277
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233244"
---
# <a name="dbcc-freeproccache-transact-sql"></a>DBCC FREEPROCCACHE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

删除计划高速缓存中的所有元素，通过指定计划句柄或 SQL 句柄从计划高速缓存中删除特定计划，或者删除与指定资源池相关联的所有高速缓存条目。

> [!NOTE]
> DBCC FREEPROCCACHE 不清除本机编译的存储过程的执行统计信息。 过程高速缓存不包含有关本机编译的存储过程的信息。 从过程执行中收集的任何执行统计信息都将显示在执行统计信息 DMV 中：[sys.dm_exec_procedure_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) 和 [sys.dm_exec_query_plan (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的语法：

```sql
DBCC FREEPROCCACHE [ ( { plan_handle | sql_handle | pool_name } ) ] [ WITH NO_INFOMSGS ]  
```  

[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 的语法：
  
```sql
DBCC FREEPROCCACHE [ ( COMPUTE | ALL ) ] 
     [ WITH NO_INFOMSGS ]   
[;]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 ( { plan_handle | sql_handle | pool_name } )    
plan_handle 用于唯一标识已执行并且其计划驻留在计划缓存中的批处理的查询计划。 plan_handle 为 varbinary(64)，可以从下列动态管理对象中获得计划句柄：  
 -   [sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
 -   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
 -   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
 -   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  

sql_handle 是要清除的批处理的 SQL 句柄。 sql_handle 为 varbinary(64)，可以从下列动态管理对象中获得计划句柄：  
 -   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
 -   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
 -   [sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)  
 -   [sys.dm_exec_xml_handles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)  
 -   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  

pool_name 是资源调控器资源池的名称。 pool_name 的数据类型为 sysname，可通过查询 [sys.dm_resource_governor_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md) 动态管理视图获得此参数。  
 若要将资源调控器工作负荷组与资源池相关联，请查询 [sys.dm_resource_governor_workload_groups](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md) 动态管理视图。 有关会话的工作负荷组的信息，请查询 [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md) 动态管理视图。  

  
 WITH NO_INFOMSGS  
 取消显示所有信息性消息。  
  
 COMPUTE  
 从每个“计算”节点，清除查询计划缓存。 这是默认值。  
  
 ALL  
 从每个“计算”节点和“管理”节点，清除查询计划缓存。  

> [!NOTE]
> 从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始，使用 `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE` 清除范围内数据库的过程（计划）缓存。

## <a name="remarks"></a>注解  
小心使用 DBCC FREEPROCCACHE 清除计划高速缓存。 清除过程（计划）缓存会逐出所有计划，并且传入查询执行将编译新计划，而不是重复使用任何以前缓存的计划。 

由于新编译数量增加，这可能导致查询性能骤降。 对于计划缓存中每个已清除的缓存，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志将包含以下信息性消息：

> `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to 'DBCC FREEPROCCACHE' or 'DBCC FREESYSTEMCACHE' operations.` 

每隔五分钟，只要缓存在这段时间间隔内得到刷新，此消息就记录一次。

以下重新配置操作也将清除过程缓存：
-   访问检查缓存存储桶计数  
-   访问检查缓存配额  
-   CLR 已启用  
-   并行的开销阈值  
-   跨数据库所有权链接  
-   索引创建内存  
-   最大并行度  
-   max server memory  
-   max text repl size  
-   最大工作线程数  
-   min memory per query  
-   min server memory  
-   查询调控器开销限制  
-   查询等待  
-   remote query timeout  
-   user options  
  
## <a name="result-sets"></a>结果集  
如果未指定 WITH NO_INFOMSGS 子句，DBCC FREEPROCCACHE 将返回：“DBCC 执行完毕。 如果 DBCC 输出了错误消息，请与系统管理员联系。”
  
## <a name="permissions"></a>权限  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 
- 需要对服务器的 ALTER SERVER STATE 权限。  

适用范围：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)]
- 要求具有 DB_OWNER 固定服务器角色中的成员资格。  

## <a name="general-remarks-for-sssdw-and-sspdw"></a>[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 的一般备注  
可以同时运行多个 DBCC FREEPROCCACHE 命令。
在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 或 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中，清除计划缓存可能导致查询性能暂时性降低，因为传入查询编译新计划，而不是重复使用任何以前缓存的计划。 

在计算节点上运行时，DBCC FREEPROCCACHE (COMPUTE) 仅会导致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 重新编译查询。 它不会导致 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 或 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 重新编译在控制节点上生成的并行查询计划。
可在执行期间取消 DBCC FREEPROCCACHE。
  
## <a name="limitations-and-restrictions-for-sssdw-and-sspdw"></a>[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 的限制与局限  
无法在事务中运行 DBCC FREEPROCCACHE。
EXPLAIN 语句中不支持使用 DBCC FREEPROCCACHE。
  
## <a name="metadata-for-sssdw-and-sspdw"></a>[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 的元数据  
运行 DBCC FREEPROCCACHE 时，会向 sys.pdw_exec_requests 系统视图中添加新行。

## <a name="examples-ssnoversion"></a>示例：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
### <a name="a-clearing-a-query-plan-from-the-plan-cache"></a>A. 从计划高速缓存中清除查询计划  
以下示例通过指定查询计划句柄从计划高速缓存中清除查询计划。 为了确保示例查询在计划高速缓存中，首先执行该查询。 将查询 `sys.dm_exec_cached_plans` 和 `sys.dm_exec_sql_text` 动态管理视图以返回查询的计划句柄。 

然后，将结果集中的计划句柄值插入 `DBCC FREEPROCACHE` 语句，以从计划高速缓存中仅删除该计划。
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT * FROM Person.Address;  
GO  
SELECT plan_handle, st.text  
FROM sys.dm_exec_cached_plans   
CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS st  
WHERE text LIKE N'SELECT * FROM Person.Address%';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
plan_handle                                         text  
--------------------------------------------------  -----------------------------  
0x060006001ECA270EC0215D05000000000000000000000000  SELECT * FROM Person.Address;  
  
(1 row(s) affected)
 ```
 
```sql  
-- Remove the specific plan from the cache.  
DBCC FREEPROCCACHE (0x060006001ECA270EC0215D05000000000000000000000000);  
GO  
```  
  
### <a name="b-clearing-all-plans-from-the-plan-cache"></a>B. 清除计划高速缓存中的所有计划  
以下示例清除计划高速缓存中的所有元素。 指定了 WITH `NO_INFOMSGS` 子句来阻止显示信息消息。
  
```sql  
DBCC FREEPROCCACHE WITH NO_INFOMSGS;  
```  
  
### <a name="c-clearing-all-cache-entries-associated-with-a-resource-pool"></a>C. 清除与资源池相关联的所有高速缓存条目  
以下示例清除与指定资源池相关联的所有高速缓存条目。 `sys.dm_resource_governor_resource_pools` 视图首先被查询，以便获取 pool_name 的值。
  
```sql  
SELECT * FROM sys.dm_resource_governor_resource_pools;  
GO  
DBCC FREEPROCCACHE ('default');  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>示例：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-dbcc-freeproccache-basic-syntax-examples"></a>D. DBCC FREEPROCCACHE 基本语法示例  
以下示例从计算节点中删除所有现有的查询计划缓存。 虽然上下文设置为 UserDbSales，但所有数据库的计算节点查询计划缓存都将删除。 WITH NO_INFOMSGS 子句可防止结果中显示信息性消息。  
  
```sql
USE UserDbSales;  
DBCC FREEPROCCACHE (COMPUTE) WITH NO_INFOMSGS;
```  
  
 以下示例的结果与上一个示例的结果相同，只是将在结果中显示信息性消息。  
  
```sql
USE UserDbSales;  
DBCC FREEPROCCACHE (COMPUTE);  
```  
  
如果请求了信息性消息且执行成功，则查询结果中每个计算节点各具一行。
  
### <a name="e-granting-permission-to-run-dbcc-freeproccache"></a>E. 授予权限运行 DBCC FREEPROCCACHE  
以下示例为登录名 David 提供权限，允许其运行 DBCC FREEPROCCACHE。  
  
```sql
GRANT ALTER SERVER STATE TO David; 
GO
```  
  
## <a name="see-also"></a>另请参阅  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[资源调控器](../../relational-databases/resource-governor/resource-governor.md)  
[ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)
  
  
