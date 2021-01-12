---
description: sys.dm_exec_query_plan (Transact-SQL)
title: sys.dm_exec_query_plan (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_plan_TSQL
- sys.dm_exec_query_plan
- dm_exec_query_plan
- sys.dm_exec_query_plan_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_plan dynamic management function
ms.assetid: e26f0867-9be3-4b2e-969e-7f2840230770
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 18d6e4e8e3c6fe097676d1bf65f337610127566a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094104"
---
# <a name="sysdm_exec_query_plan-transact-sql"></a>sys.dm_exec_query_plan (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

以 XML 格式返回计划句柄指定的批查询的显示计划。 计划句柄指定的计划可以处于缓存或正在执行状态。  
  
显示计划的 XML 架构已发布，并在 [此 Microsoft 网站](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409)上提供。 也可在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装目录中找到该架构。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sys.dm_exec_query_plan(plan_handle)  
```  
  
## <a name="arguments"></a>参数  
*plan_handle*  
是一个标记，用于唯一标识已执行并且其计划驻留在计划缓存中或当前正在执行的批处理的查询执行计划。 *plan_handle* 为 **varbinary (64)**。   

可以从以下动态管理对象中获取 *plan_handle* ：
  
-   [sys.dm_exec_cached_plans (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**dbid**|**smallint**|在编译对应于此计划的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句时有效的上下文数据库的 ID。 对于临时和预定义 SQL 语句，指编译这些语句时所在的数据库的 ID。<br /><br /> 此列可为空值。|  
|**objectid**|**int**|此查询计划的对象（如存储过程或用户定义函数）的 ID。 对于即席批处理和已准备好的批处理，此列为 **null**。<br /><br /> 此列可为空值。|  
|**数字**|**smallint**|为存储过程编号的整数。 例如，用于 **orders** 应用程序的一组过程可命名为 **orderproc;1**、**orderproc;2** 等等。 对于即席批处理和已准备好的批处理，此列为 **null**。<br /><br /> 此列可为空值。|  
|**过**|**bit**|指示对应的存储过程是否已加密。<br /><br /> 0 = 未加密<br /><br /> 1 = 已加密<br /><br /> 此列不可为空值。|  
|**query_plan**|**xml**|包含与 *plan_handle* 一起指定的查询执行计划的编译时显示计划表示形式。 显示计划的格式为 XML。 为包含即席 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句、存储过程调用以及用户定义函数调用等内容的每个批查询生成一个计划。<br /><br /> 此列可为空值。|  
  
## <a name="remarks"></a>备注  
 在以下条件下，不会在为 **sys.dm_exec_query_plan** 返回的表的 **query_plan** 列中返回显示计划输出：  
  
-   如果使用 *plan_handle* 指定的查询计划已从计划缓存中逐出，则返回的表的 **query_plan** 列将为 null。 例如，如果在捕获计划句柄的时间与将其用于 **sys.dm_exec_query_plan** 的时间之间存在时间延迟，则可能会出现该情况。  
  
-   有些 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句未放入缓存，如大容量操作语句或包含大于 8 KB 的字符串文字的语句。 除非当前正在执行此批处理，否则由于这些语句不在缓存中，而无法使用 **sys.dm_exec_query_plan** 来检索这些语句的 XML 显示计划。  
  
-   如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理或存储过程包含对用户定义函数或动态 SQL 调用的调用（例如，使用 EXEC (*string*) ），则用户定义函数的编译的 XML 显示计划不包含在批处理或存储过程的 **sys.dm_exec_query_plan** 返回的表中。 相反，必须为与用户定义函数相对应的计划句柄单独调用 **sys.dm_exec_query_plan** 。  
  
 当即席查询使用简单参数化或强制参数化时，**query_plan** 列将仅包含语句文本，而非实际查询计划。 若要返回查询计划，则应为已准备好的参数化查询的计划句柄调用 **sys.dm_exec_query_plan**。 可以通过引用 [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) 视图的 **sql** 列或 [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) 动态管理视图的文本列来确定查询是否已参数化。  
  
> [!NOTE] 
> 由于 **xml** 数据类型允许的嵌套级别数有限制， **sys.dm_exec_query_plan** 无法返回满足或超过128嵌套元素级别的查询计划。 在早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，这种情况将导致无法返回查询计划，并生成错误 6335。 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 及更高版本中， **query_plan** 列返回 NULL。   
> 您可以使用 [sys.dm_exec_text_query_plan &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md) 动态管理函数以文本格式返回查询计划的输出。  
  
## <a name="permissions"></a>权限  
 若要执行 **sys.dm_exec_query_plan**，用户必须是 **sysadmin** 固定服务器角色的成员或具有 `VIEW SERVER STATE` 对服务器的权限。  
  
## <a name="examples"></a>示例  
 以下示例显示如何使用 **sys.dm_exec_query_plan** 动态管理视图。  
  
 若要查看 XML 显示计划，请在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 的查询编辑器中执行以下查询，然后在 **sys.dm_exec_query_plan** 返回的表的 **query_plan** 列中单击 **ShowPlanXML**。 XML 显示计划显示在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 摘要窗格中。 若要将 XML 显示计划保存到文件中，请在 **query_plan** 列中右键单击 " **ShowPlanXML** "，单击 "**将结果另存为**"，将文件命名为 \<*file_name*> .sqlplan; 例如，myxmlshowplan.sqlplan. .sqlplan。  
  
### <a name="a-retrieve-the-cached-query-plan-for-a-slow-running-transact-sql-query-or-batch"></a>A. 检索运行速度缓慢的 Transact-SQL 查询或批查询的缓存查询计划  
 不同类型的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批查询（如即席批查询、存储过程和用户定义函数）的查询计划缓存在称作计划缓存的内存区域中。 每个缓存查询计划均由称作计划句柄的唯一标识符进行标识。 您可以通过 **sys.dm_exec_query_plan** 动态管理视图指定此计划句柄，以便检索特定 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询或批处理的执行计划。  
  
 如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询或批查询在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的特定连接上运行的时间很长，请检索该查询或批查询的执行计划，以查找导致延迟的原因。 以下示例显示如何检索运行速度缓慢的查询或批查询的 XML 显示计划。  
  
> [!NOTE]  
>  若要运行此示例，请将 " *session_id* " 和 " *plan_handle* " 的值替换为特定于服务器的值。  
  
 首先，使用 `sp_who` 存储过程检索正在执行查询或批查询的进程的服务器进程 ID (SPID)。  
  
```sql  
USE master;  
GO  
exec sp_who;  
GO  
```  
  
 由 `sp_who` 返回的结果集指示 SPID 为 `54`。 可以在 `sys.dm_exec_requests` 动态管理视图中使用该 SPID，以便使用以下查询来检索计划句柄：  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_requests  
WHERE session_id = 54;  
GO  
```  
  
 **Sys.dm_exec_requests** 返回的表指示运行速度缓慢的查询或批处理的计划句柄为 `0x06000100A27E7C1FA821B10600` ，您可以将指定为的 *plan_handle* 参数作为参数，如下所示 `sys.dm_exec_query_plan` 。 运行速度缓慢的查询或批处理的 XML 格式的执行计划包含在 `sys.dm_exec_query_plan` 返回的表的 **query_plan** 列中。  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_query_plan (0x06000100A27E7C1FA821B10600);  
GO  
```  
  
### <a name="b-retrieve-every-query-plan-from-the-plan-cache"></a>B. 从计划缓存中检索每个查询计划  
 若要检索驻留在计划缓存中的所有查询计划的快照，请通过查询 `sys.dm_exec_cached_plans` 动态管理视图来检索缓存中所有查询计划的计划句柄。 计划句柄存储在 `plan_handle` 的 `sys.dm_exec_cached_plans` 列中。 然后，使用 CROSS APPLY 运算符将计划句柄传递给 `sys.dm_exec_query_plan`，如下所示。 当前在计划缓存中的每个计划的 XML 显示计划输出位于返回的表的 `query_plan` 列中。  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_cached_plans AS cp 
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
GO  
```  
  
### <a name="c-retrieve-every-query-plan-for-which-the-server-has-gathered-query-statistics-from-the-plan-cache"></a>C. 检索服务器已从计划缓存中收集了其查询统计信息的每个查询计划  
 若要检索服务器已收集了其当前驻留在计划缓存中的统计信息的所有查询计划的快照，请通过查询 `sys.dm_exec_query_stats` 动态管理视图来检索缓存中这些计划的计划句柄。 计划句柄存储在 `plan_handle` 的 `sys.dm_exec_query_stats` 列中。 然后，使用 CROSS APPLY 运算符将计划句柄传递给 `sys.dm_exec_query_plan`，如下所示。 服务器已收集了其驻留在计划缓存中的统计信息的每个计划的 XML 显示计划输出在返回的表的 `query_plan` 列中。  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_query_stats AS qs 
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle);  
GO  
```  
  
### <a name="d-retrieve-information-about-the-top-five-queries-by-average-cpu-time"></a>D. 按平均 CPU 时间检索有关前五个查询的信息  
 以下示例为前五个查询返回查询计划和平均 CPU 时间。  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
   plan_handle, query_plan   
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
ORDER BY total_worker_time/execution_count DESC;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_exec_cached_plans (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sp_who (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Showplan 逻辑运算符和物理运算符参考](../../relational-databases/showplan-logical-and-physical-operators-reference.md)   
 [sys.dm_exec_text_query_plan &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)  
  
  
