---
description: sp_create_plan_guide_from_handle (Transact-SQL)
title: sp_create_plan_guide_from_handle (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_plan_guide_from_handle_TSQL
- sp_create_plan_guide_from_handle
dev_langs:
- TSQL
helpviewer_keywords:
- sp_create_plan_guide_from_handle
ms.assetid: 02cfb76f-a0f9-4b42-a880-1c3e7d64fe41
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2020abc49ccd29491f99adc77910868071e16e00
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186227"
---
# <a name="sp_create_plan_guide_from_handle-transact-sql"></a>sp_create_plan_guide_from_handle (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从计划缓存中的查询计划创建一个或多个计划指南。 可以使用此存储过程来确保查询优化器始终为指定查询使用特定的查询计划。 有关计划指南的详细信息，请参阅 [Plan Guides](../../relational-databases/performance/plan-guides.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_create_plan_guide_from_handle [ @name = ] N'plan_guide_name'  
    , [ @plan_handle = ] plan_handle  
    , [ [ @statement_start_offset = ] { statement_start_offset | NULL } ]  
```  
  
## <a name="arguments"></a>参数  
 [ @name =] N '*plan_guide_name*'  
 计划指南的名称。 计划指南名称的作用域限于当前数据库。 *plan_guide_name* 必须符合 [标识符](../../relational-databases/databases/database-identifiers.md) 规则，且不能以数字符号 ( # ) 开头。 *Plan_guide_name* 的最大长度为124个字符。  
  
 [ @plan_handle =] *plan_handle*  
 标识计划缓存中的批处理。 *plan_handle* 为 **varbinary (64)**。 *plan_handle* 可以从 [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) 动态管理视图中获取。  
  
 [ @statement_start_offset =] { *statement_start_offset* |NULL}]  
 标识指定 *plan_handle* 的批处理中的语句的起始位置。 *statement_start_offset* 的值为 **int**，默认值为 NULL。  
  
 语句偏移量对应于 [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) 动态管理视图中的 statement_start_offset 列。  
  
 指定 NULL 或未指定语句偏移量时，将使用指定计划句柄的查询计划为批处理中的每个语句创建一个计划指南。 生成的计划指南等同于使用 USE PLAN 查询提示来强制使用特定计划的计划指南。  
  
## <a name="remarks"></a>备注  
 无法创建适用于所有语句类型的计划指南。 如果不能为批处理中的某个语句创建计划指南，则存储过程将忽略此语句并继续执行批处理中的下一个语句。 如果在同一批处理中多次出现同一语句，将启用最后一处语句的计划并禁用此语句以前的计划。 如果批处理中没有语句可用于计划指南，将产生错误 10532 并且语句将失败。 建议您始终从 sys.dm_exec_query_stats 动态管理视图获得计划句柄，以帮助避免出现此错误。  
  
> [!IMPORTANT]  
>  sp_create_plan_guide_from_handle 根据计划在计划缓存中的显示方式创建计划指南。 这表示将从计划缓存中将批处理文本、[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句和 XML 显示计划（包括传递给查询的任何文字值）逐个字符地导入生成的计划指南中。 这些文本字符串可能包含敏感信息，这些信息将以数据库的元数据形式存储。 具有适当权限的用户可以通过使用中的 "sys.plan_guides 目录视图和" **计划指南属性** "对话框来查看此信息 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。 为了确保不会通过计划指南泄露敏感信息，建议查看从计划缓存创建的计划指南。  
  
## <a name="creating-plan-guides-for-multiple-statements-within-a-query-plan"></a>为查询计划中的多个语句创建计划指南  
 与 sp_create_plan_guide 一样，sp_create_plan_guide_from_handle 也会从计划缓存中删除目标批处理或模块的查询计划。 这样做是为了确保所有用户都开始使用新的计划指南。 当为单个查询计划中的多个语句创建计划指南时，可以通过在显式事务中创建所有计划指南来推迟从缓存中删除该计划。 使用此方法可在完成事务以及为每个指定语句创建计划指南之前将计划保留在缓存中。 请参阅示例 B。  
  
## <a name="permissions"></a>权限  
 需要 `VIEW SERVER STATE` 权限。 此外，对于使用 sp_create_plan_guide_from_handle 创建的每个计划指南，需要拥有单个权限。 若要创建 OBJECT 类型的计划指南，需要 `ALTER` 拥有对被引用对象的权限。 若要创建类型为 SQL 或 TEMPLATE 的计划指南，需要 `ALTER` 对当前数据库拥有权限。 若要确定将要创建的计划指南的类型，请运行以下查询：  
  
```sql  
SELECT cp.plan_handle, sql_handle, st.text, objtype   
FROM sys.dm_exec_cached_plans AS cp  
JOIN sys.dm_exec_query_stats AS qs ON cp.plan_handle = qs.plan_handle  
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS st;  
```  
  
 在结果集中包含为其创建计划指南的语句的行中，检查 `objtype` 列。 值 `Proc` 表示计划指南的类型为 OBJECT。 其他诸如 `AdHoc` 或 `Prepared` 之类的值表示计划指南的类型为 SQL。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-plan-guide-from-a-query-plan-in-the-plan-cache"></a>A. 从计划缓存中的查询计划创建计划指南  
 下面的示例通过指定计划缓存中的查询计划为单个 SELECT 语句创建计划指南。 该示例开始先执行将为其创建计划指南的 `SELECT` 语句。 此查询的计划通过使用 `sys.dm_exec_sql_text` 和 `sys.dm_exec_text_query_plan` 动态管理视图检查。 然后，通过指定计划缓存中与此查询相关的查询计划为此查询创建计划指南。 该示例中的最后一个语句用于验证计划指南是否已存在。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT WorkOrderID, p.Name, OrderQty, DueDate  
FROM Production.WorkOrder AS w   
JOIN Production.Product AS p ON w.ProductID = p.ProductID  
WHERE p.ProductSubcategoryID > 4  
ORDER BY p.Name, DueDate;  
GO  
-- Inspect the query plan by using dynamic management views.  
SELECT * FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(sql_handle)  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset) AS qp  
WHERE text LIKE N'SELECT WorkOrderID, p.Name, OrderQty, DueDate%';  
GO  
-- Create a plan guide for the query by specifying the query plan in the plan cache.  
DECLARE @plan_handle varbinary(64);  
DECLARE @offset int;  
SELECT @plan_handle = plan_handle, @offset = qs.statement_start_offset  
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS st  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset) AS qp  
WHERE text LIKE N'SELECT WorkOrderID, p.Name, OrderQty, DueDate%';  
  
EXECUTE sp_create_plan_guide_from_handle   
    @name =  N'Guide1',  
    @plan_handle = @plan_handle,  
    @statement_start_offset = @offset;  
GO  
-- Verify that the plan guide is created.  
SELECT * FROM sys.plan_guides  
WHERE scope_batch LIKE N'SELECT WorkOrderID, p.Name, OrderQty, DueDate%';  
GO  
```  
  
### <a name="b-creating-multiple-plan-guides-for-a-multistatement-batch"></a>B. 为多语句批处理创建多个计划指南  
 下面的示例为多语句批处理中的两个语句分别创建一个计划指南。 这些计划指南是在显式事务中创建的，这样做是为了在创建第一个计划指南之后不会从计划缓存中删除批处理的查询计划。 该示例开始先执行多语句批处理。 批处理的计划通过使用动态管理视图检查。 请注意，为批处理中的每个语句都返回了一行。 然后，通过指定 `@statement_start_offset` 参数为批处理中的第一和第三个语句创建计划指南。 该示例中的最后一个语句用于验证这些计划指南是否已存在。  
  
 [!code-sql[PlanGuides#Create_From_Handle2](../../relational-databases/system-stored-procedures/codesnippet/tsql/sp-create-plan-guide-fro_1.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [计划指南](../../relational-databases/performance/plan-guides.md)   
 [sp_create_plan_guide (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_text_query_plan &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)   
 [sp_control_plan_guide (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)  
  
  
