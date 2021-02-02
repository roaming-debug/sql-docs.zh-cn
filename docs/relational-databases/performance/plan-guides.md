---
title: 计划指南 | Microsoft Docs
description: 了解计划指南，它允许你优化查询性能，而无需在 SQL Server 中直接更改查询的文本。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- TEMPLATE plan guide
- SQL plan guides
- OPTIMIZE FOR query hint
- RECOMPILE query hint
- OBJECT plan guide
- plan guides [SQL Server], about plan guides
- OPTION clause
- plan guides [SQL Server]
- USE PLAN query hint
ms.assetid: bfc97632-c14c-4768-9dc5-a9c512f6b2bd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e230b163fdbe9f48ec4f436b4ef2453040b89b24
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048837"
---
# <a name="plan-guides"></a>计划指南
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  如果您无法或不希望直接在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中更改实际查询文本，则可以使用计划指南来优化查询性能。 计划指南通过将查询提示或固定的查询计划附加到查询来影响查询的优化。 当第三方供应商提供的数据库应用程序中的一个小的查询子集没有按预期执行时，计划指南将很有用。 在计划指南中，您需要指定要优化的 Transact-SQL 语句以及包含要使用的查询提示的 OPTION 子句或要用于优化查询的特定查询计划。 当执行查询时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将 Transact-SQL 语句与计划指南进行匹配，然后在运行时将此 OPTION 子句附加到查询，或使用指定的查询计划。  
  
 可创建的计划指南总数仅受可用系统资源的限制。 尽管如此，计划指南还是应当限于针对提高或稳定性能的关键查询。 计划指南不应用来影响已部署应用程序的大部分查询负荷。  
  
> [!NOTE]
>  计划指南不适用于每个 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。 计划指南在任何版本中可见。 包含计划指南的数据库可以附加到任何版本。 将数据库还原或附加到升级版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]后，计划指南保持不变。  
  
## <a name="types-of-plan-guides"></a>计划指南的类型  
 可以创建以下类型的计划指南。  
  
 ### <a name="object-plan-guide"></a>OBJECT 计划指南  
 OBJECT 计划指南与在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程、用户定义标量函数、多语句表值用户定义函数和 DML 触发器上下文中执行的查询匹配。  
  
 假设下面的存储过程采用 `@Country_region`参数，位于对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库部署的数据库应用程序中：  
  
```sql  
CREATE PROCEDURE Sales.GetSalesOrderByCountry (@Country_region nvarchar(60))  
AS  
BEGIN  
    SELECT *  
    FROM Sales.SalesOrderHeader AS h, Sales.Customer AS c,   
        Sales.SalesTerritory AS t  
    WHERE h.CustomerID = c.CustomerID  
        AND c.TerritoryID = t.TerritoryID  
        AND CountryRegionCode = @Country_region  
END;  
```  
  
 假设已为 `@Country_region = N'AU'`（澳大利亚）编译并优化了此存储过程。 但是，由于来自澳大利亚的销售订单相对较少，当使用具有较多销售订单的国家/地区的参数值执行查询时，性能会降低。 因为大多数销售订单来自美国，所以针对 `@Country_region = N'US'` 生成的查询计划的性能对于所有可能的 `@Country_region` 参数值而言都可能会更好一些。  
  
 您可以通过修改存储过程以将 `OPTIMIZE FOR` 查询提示添加到查询中来解决这一问题。 但是，因为存储过程在部署应用程序中，所以您不能直接修改应用程序代码。 不过，您可以在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中创建下面的计划指南。  
  
```sql  
sp_create_plan_guide   
@name = N'Guide1',  
@stmt = N'SELECT *FROM Sales.SalesOrderHeader AS h,  
        Sales.Customer AS c,  
        Sales.SalesTerritory AS t  
        WHERE h.CustomerID = c.CustomerID   
            AND c.TerritoryID = t.TerritoryID  
            AND CountryRegionCode = @Country_region',  
@type = N'OBJECT',  
@module_or_batch = N'Sales.GetSalesOrderByCountry',  
@params = NULL,  
@hints = N'OPTION (OPTIMIZE FOR (@Country_region = N''US''))';  
```  
  
 执行在 `sp_create_plan_guide` 语句中指定的查询时，将在优化之前修改该查询以使其包括 `OPTIMIZE FOR (@Country = N''US'')` 子句。  
  
 ### <a name="sql-plan-guide"></a>SQL 计划指南  
 SQL 计划指南与在独立 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句和不属于数据库对象的批处理上下文中执行的查询匹配。 基于 SQL 的计划指南还可用于与参数化为指定形式的查询匹配。 SQL 计划指南适用于独立 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句和批处理。 通常，这些语句由应用程序使用 [sp_executesql](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md) 系统存储过程来提交。 以下面的独立批处理为例：  
  
```sql  
SELECT TOP 1 * FROM Sales.SalesOrderHeader ORDER BY OrderDate DESC;  
```  
  
 为了防止对该查询生成并行执行计划，请创建以下计划指南并在 `MAXDOP` 参数中将 `1` 查询提示设置为 `@hints` 。  
  
```sql  
sp_create_plan_guide   
@name = N'Guide2',   
@stmt = N'SELECT TOP 1 * FROM Sales.SalesOrderHeader ORDER BY OrderDate DESC',  
@type = N'SQL',  
@module_or_batch = NULL,   
@params = NULL,   
@hints = N'OPTION (MAXDOP 1)';  
```  
作为另一个示例，请考虑使用 [sp_executesql](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md) 提交的以下 SQL 语句。

```sql  
exec sp_executesql N'SELECT * FROM Sales.SalesOrderHeader
where SalesOrderID =  @so_id', N'@so_id int', @so_id = 43662;  
```  
 若要为此查询的每次执行创建唯一计划，请创建以下计划指南并使用 `@hints` 参数中的 `OPTION (RECOMPILE)` 查询提示。 

```sql  
exec sp_create_plan_guide   
@name = N'PlanGuide1_SalesOrders',   
@stmt = N'SELECT * FROM Sales.SalesOrderHeader
where SalesOrderID =  @so_id',
@type = N'SQL',  
@module_or_batch = NULL,   
@params = N'@so_id int',   
@hints = N'OPTION (recompile)';
```

> [!IMPORTANT]  
>  为 `@module_or_batch` 语句的 `@params` 和 `sp_create_plan guide` 参数提供的值必须与在实际查询中提交的相应文本匹配。 有关详细信息，请参阅 [sp_create_plan_guide (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md) 语句的 [使用 SQL Server Profiler 创建和测试计划指南](../../relational-databases/performance/use-sql-server-profiler-to-create-and-test-plan-guides.md)中更改实际查询文本，则可以使用计划指南来优化查询性能。  
  
 PARAMETERIZATION 数据库选项设置为 FORCED 后，或者创建了指定参数化查询类的 TEMPLATE 计划指南后，还可以对参数化为相同形式的查询创建 SQL 计划指南。  
  
 ### <a name="template-plan-guide"></a>TEMPLATE 计划指南  
 TEMPLATE 计划指南与参数化为指定形式的独立查询匹配。 这些计划指南用于覆盖查询类的数据库的当前 PARAMETERIZATION 数据库 SET 选项。  
  
 您可以在以下任一情况下创建 TEMPLATE 计划指南：  
  
-   PARAMETERIZATION 数据库选项设置为 FORCED，但是你要按照[简单参数化](../../relational-databases/query-processing-architecture-guide.md#SimpleParam)规则编译某些查询。  
  
-   PARAMETERIZATION 数据库选项设置为 SIMPLE（默认设置），但是您要尝试对某一类查询执行[强制参数化](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)。  
  
## <a name="plan-guide-matching-requirements"></a>符合要求的计划指南  
 计划指南的作用域是在其中创建这些计划指南的数据库。 因此，执行查询时，只有处于当前状态的数据库中的计划指南可以与该查询匹配。 例如，如果 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 是当前数据库并执行下面的查询：  
  
 ```sql
 SELECT FirstName, LastName FROM Person.Person;
 ```  
  
 则只有 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的计划指南可以与此查询匹配。 但是，如果 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 是当前数据库并运行下列语句：  
  
 ```sql
 USE DB1; 
 SELECT FirstName, LastName FROM Person.Person;
 ```  
  
 则只有 `DB1` 中的计划指南可以与该查询匹配，这是因为该查询是在 `DB1`的上下文中执行的。  
  
 对于基于 SQL 或 TEMPLATE 的计划指南， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过对 @module_or_batch 参数和 @params 参数的值逐个字符地进行比较来将这两个值与查询匹配。 这意味着必须提供与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在实际批处理中接收的文本完全相同的文本。  
  
 当 @type = 'SQL' 且 @module_or_batch 设置为 NULL 时，则将 @module_or_batch 的值设置为 @stmt 的值。这意味着 *statement_text* 值的提供格式必须与其提交到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]时所采用的格式完全相同（字符对字符）。 不会执行内部转换来帮助完成该匹配。  
  
 当常规（SQL 或 OBJECT）计划指南和 TEMPLATE 计划指南都适用于语句时，将只使用常规计划指南。  
  
> [!NOTE]  
>  包含要对其创建计划指南的语句的批处理不能包含 USE *database* 语句。  
  
## <a name="plan-guide-effect-on-the-plan-cache"></a>计划指南对计划缓存的影响  
 对模块创建计划指南将会从计划缓存中删除该模块的查询计划。 对批处理创建类型为 OBJECT 或 SQL 的计划指南会删除具有相同哈希值的批处理的查询计划。 创建类型为 TEMPLATE 的计划指南会从该数据库中的计划缓存中删除所有单语句批处理。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务|主题|  
|----------|-----------|  
|说明如何创建计划指南。|[创建新的计划指南](../../relational-databases/performance/create-a-new-plan-guide.md)|  
|说明如何为参数化查询创建计划指南。|[为参数化查询创建计划指南](../../relational-databases/performance/create-a-plan-guide-for-parameterized-queries.md)|  
|说明如何通过使用计划指南控制查询参数化行为。|[使用计划指南指定查询参数化行为](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md)|  
|说明如何在计划指南中包括固定查询计划。|[将现有查询计划应用到计划指南](../../relational-databases/performance/apply-a-fixed-query-plan-to-a-plan-guide.md)|  
|说明如何在计划指南中指定查询提示。|[将查询提示附加到计划指南](../../relational-databases/performance/attach-query-hints-to-a-plan-guide.md)|  
|说明如何查看计划指南属性。|[查看计划指南属性](../../relational-databases/performance/view-plan-guide-properties.md)|  
|说明如何使用 SQL Server 事件探查器创建和测试计划指南。|[使用 SQL Server Profiler 创建和测试计划指南](../../relational-databases/performance/use-sql-server-profiler-to-create-and-test-plan-guides.md)|  
|说明如何验证计划指南。|[升级后验证计划指南](../../relational-databases/performance/validate-plan-guides-after-upgrade.md)|  
  
## <a name="see-also"></a>另请参阅  
 [sp_create_plan_guide (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sp_create_plan_guide_from_handle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)   
 [sp_control_plan_guide (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)   
 [sys.plan_guides (Transact-SQL)](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)   
 [sys.fn_validate_plan_guide (Transact-SQL)](../../relational-databases/system-functions/sys-fn-validate-plan-guide-transact-sql.md)  
  
  
