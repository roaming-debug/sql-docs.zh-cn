---
description: sp_create_plan_guide (Transact-SQL)
title: sp_create_plan_guide (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_plan_guide
- sp_create_plan_guide_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_create_plan_guide
ms.assetid: 5a8c8040-4f96-4c74-93ab-15bdefd132f0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0203cbe0539d0733740cf495aa0c149409468fa8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205215"
---
# <a name="sp_create_plan_guide-transact-sql"></a>sp_create_plan_guide (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  创建用于将查询提示或实际查询计划与数据库中的查询关联的计划指南。 有关计划指南的详细信息，请参阅 [Plan Guides](../../relational-databases/performance/plan-guides.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_create_plan_guide [ @name = ] N'plan_guide_name'  
    , [ @stmt = ] N'statement_text'  
    , [ @type = ] N'{ OBJECT | SQL | TEMPLATE }'  
    , [ @module_or_batch = ]  
      {   
                    N'[ schema_name. ] object_name'  
        | N'batch_text'  
        | NULL  
      }  
    , [ @params = ] { N'@parameter_name data_type [ ,...n ]' | NULL }   
    , [ @hints = ] { N'OPTION ( query_hint [ ,...n ] )'   
                 | N'XML_showplan'  
                 | NULL }  
```  
  
## <a name="arguments"></a>参数  
 [ \@ name =] N '*plan_guide_name*'  
 计划指南的名称。 计划指南名称的作用域限于当前数据库。 *plan_guide_name* 必须符合 [标识符](../../relational-databases/databases/database-identifiers.md) 规则，且不能以数字符号 ( # ) 开头。 *Plan_guide_name* 的最大长度为124个字符。  
  
 [ \@ stmt =] N '*statement_text*'  
 根据其创建计划指南的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器识别与 *statement_text* 匹配的查询时， *plan_guide_name* 生效。 若要成功创建计划指南， *statement_text* 必须出现在由 \@ type、 \@ module_or_batch 和 params 参数指定的上下文中 \@ 。  
  
 必须以允许查询优化器将它与 module_or_batch 和 params 标识的批处理或模块中提供的对应语句匹配的方式提供 *statement_text* \@ \@ 。 有关更多信息，请参见“备注”部分。 *Statement_text* 的大小仅受服务器的可用内存限制。  
  
 [ \@ type =] N ' {OBJECT |SQL |TEMPLATE} "  
 *Statement_text* 出现在其中的实体的类型。 这会指定要 *plan_guide_name* *statement_text* 匹配的上下文。  
  
 OBJECT  
 指示 *statement_text* 出现在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 当前数据库中的存储过程、标量函数、多语句表值函数或 DML 触发器的上下文中 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。  
  
 SQL  
 指示 *statement_text* 出现在可以通过任何机制提交到的独立语句或批处理的上下文中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 由公共语言运行时 (CLR) 对象或扩展存储过程或通过使用 EXEC N '*sql_string*' 提交的语句在服务器上以批处理形式处理，因此应将其标识为 \@ **=** "sql" 类型。 如果指定了 SQL，查询提示参数化 {强制 |SIMPLE} 不能在 \@ 提示参数中指定。  
  
 TEMPLATE  
 指示计划指南适用于参数化 *statement_text* 中指定的窗体的任何查询。 如果指定了 TEMPLATE，则仅参数化 {强制 |SIMPLE} 查询提示可在 \@ 提示参数中指定。 有关模板计划指南的详细信息，请参阅 [使用计划指南指定查询参数化行为](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md)。  
  
 [ \@ module_or_batch =] {N ' [ *schema_name*。 ] *object_name*"|N "*batch_text*" |无效  
 指定在其中出现 *statement_text* 的对象的名称，或 *statement_text* 出现的批处理文本。 批处理文本不能包含 USE *database* 语句。  
  
 若要将计划指南与从应用程序提交的批处理相匹配，则必须以与提交到的格式相同的格式（字符对字符）提供 *batch_tex* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 不会执行内部转换来帮助完成该匹配。 有关详细信息，请参见“备注”部分。  
  
 [*schema_name*.]*object_name* 指定 [!INCLUDE[tsql](../../includes/tsql-md.md)] 包含 statement_text 的存储过程、标量函数、多语句表值函数或 [!INCLUDE[tsql](../../includes/tsql-md.md)] DML 触发器的名称。  如果未指定 *schema_name* ， *schema_name* 将使用当前用户的架构。 如果指定了 NULL 并且 \@ 类型 = ' SQL '，则 module_or_batch 的值将 \@ 设置为 stmt 的值 \@ 。如果 \@ type = ' TEMPLATE **\'** ， \@ MODULE_OR_BATCH 必须为 NULL。  
  
 [ \@ params =] {N '*\@ parameter_name data_type* [，*.。。n* ] ' |无效  
 指定 *statement_text* 中嵌入的所有参数的定义。 \@仅当满足以下任一条件时，params 才适用：  
  
-   \@键入 = ' SQL ' 或 ' TEMPLATE '。 如果为 "TEMPLATE"，则 \@ params 不能为 NULL。  
  
-   *statement_text* 使用 sp_executesql 进行提交，并 \@ 指定 params 参数的值，或者 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在参数化语句后在内部提交该语句。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]来说，从数据库 API（包括 ODBC、OLE DB 和 ADO.NET）提交参数化查询类似于调用 sp_executesql 或 API 服务器游标例程；因此，它们也可以通过 SQL 或 TEMPLATE 计划指南进行匹配。  
  
 必须以完全相同的格式提供 *\@ parameter_name data_type* ， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 方法是使用 sp_executesql 或在参数化后在内部提交。 有关详细信息，请参见“备注”部分。 如果批处理不包含参数，则必须指定 NULL。 Params 的大小 \@ 仅受可用服务器内存限制。  
  
 [ \@ 提示 =] {N'OPTION (*query_hint* [，*.。。n* ] ) "|N "*XML_showplan*" |无效  
 N'OPTION (*query_hint* [，*.。。n* ] )   
 指定一个 OPTION 子句以附加到与 stmt 匹配的查询 \@ 。 \@提示必须与 SELECT 语句中的 OPTION 子句采用相同的语法，并且可以包含任何有效的查询提示序列。  
  
 N '*XML_showplan*'  
 要作为提示应用的、采用 XML 格式的查询计划。  
  
 建议将 XML 显示计划分配给变量；否则，必须通过在单引号前面再加上一个单引号来对显示计划中的任何单引号进行转义。 请参见示例 E。  
  
 Null  
 指示查询的 OPTION 子句中指定的任何现有提示不应用于该查询。 有关详细信息，请参阅 [OPTION 子句 &#40;transact-sql&#41;](../../t-sql/queries/option-clause-transact-sql.md)。  
  
## <a name="remarks"></a>备注  
 sp_create_plan_guide 的参数必须以显示的顺序提供。 为 **sp_create_plan_guide** 的参数提供值时，必须显式指定所有的参数名称，或全部都不指定。 例如，如果指定了 **\@ name =** ，则也必须指定 **\@ stmt =** 、 **\@ type =** 等。 同样，如果省略 **\@ name =** 并仅提供参数值，则其余的参数名称也必须省略并仅提供其值。 参数名称仅用于说明，以帮助了解语法。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会验证指定的参数名称是否与使用此名称的位置中的参数名称相匹配。  
  
 您可以为相同的查询和批处理或模块创建多个 OBJECT 或 SQL 计划指南。 但是，在任何给定的时间只能启用一个计划指南。  
  
 无法为 \@ 引用存储过程、函数或 DML 触发器（指定了 WITH ENCRYPTION 子句或为临时子句）的 module_or_batch 值创建 OBJECT 类型的计划指南。  
  
 如果尝试删除或修改的函数、存储过程或 DML 触发器由某个计划指南引用，则不管该指南为启用状态还是禁用状态，都会导致错误。 尝试删除计划指南被引用并已为其定义触发器的表也将导致错误。  
  
> [!NOTE]
> 计划指南不适用于每个 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。 计划指南在任何版本中可见。 包含计划指南的数据库可以附加到任何版本。 将数据库还原或附加到升级版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]后，计划指南保持不变。 执行服务器升级后，应验证每个数据库中计划指南的性能。  
  
## <a name="plan-guide-matching-requirements"></a>计划指南匹配要求  
 对于指定 \@ type = ' SQL ' 或 \@ type = ' TEMPLATE ' 的计划指南与查询成功匹配， *batch_text* 和 parameter_name 的值 *\@ data_type* [，*.。。n* ] 的提供格式必须与应用程序提交的对应项的格式完全相同。 这表示必须完全按照 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 编译器接收批处理文本的方式来提供批处理文本。 若要捕获实际的批处理和参数文本，可以使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]。 有关详细信息，请参阅 [使用 SQL Server Profiler 创建和测试计划指南](../../relational-databases/performance/use-sql-server-profiler-to-create-and-test-plan-guides.md)。  
  
 当 \@ type = ' SQL ' 并且 \@ module_or_batch 设置为 NULL 时，module_or_batch 的值将 \@ 设置为 stmt 的值 \@ 。这意味着 *statement_text* 的值必须以与提交到的格式完全相同的格式提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 不会执行内部转换来帮助完成该匹配。  
  
 当将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *statement_text* 的值与 *batch_text* 并 *\@ parameter_name data_type* [，*.。。n* ]，或者，如果 \@ type = **\'** OBJECT ' *object_name*，则不考虑以下字符串元素：  
  
-   字符串内部的空白字符（制表符、空格、回车符或换行符）。  
  
-   注释 (**--** 或 **/\*   \*/**) 。  
  
-   尾随分号  
  
 例如， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以将 *statement_text* 字符串与 `N'SELECT * FROM T WHERE a = 10'` 以下 *batch_text* 匹配：  
  
 ```
 N'SELECT *
 FROM T
 WHERE a = 10' 
 ```
 
 但是，同一个字符串不会与此 *batch_text* 匹配：  
  
 `N'SELECT * FROM T WHERE b = 10'`  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将忽略第一个查询内部的回车符、换行符和空格字符。 在第二个查询中，序列 `WHERE b = 10` 的解释方式不同于 `WHERE a = 10`。 除了在关键字情况（不区分大小写）中以外，匹配是区分大小写和区分重音的（即使数据库的排序规则不区分大小写）。 对于缩短形式的关键字，匹配不进行区分。 例如，关键字 `EXECUTE`、`EXEC` 和 `execute` 被看作是等价的。  
  
## <a name="plan-guide-effect-on-the-plan-cache"></a>计划缓存上的计划指南效果  
 对模块创建计划指南将会从计划缓存中删除该模块的查询计划。 对批处理创建类型为 OBJECT 或 SQL 的计划指南会删除具有相同哈希值的批处理的查询计划。 创建类型为 TEMPLATE 的计划指南会从该数据库中的计划缓存中删除所有单语句批处理。  
  
## <a name="permissions"></a>权限  
 若要创建 OBJECT 类型的计划指南，需要 `ALTER` 拥有对被引用对象的权限。 若要创建类型为 SQL 或 TEMPLATE 的计划指南，需要 `ALTER` 对当前数据库拥有权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-plan-guide-of-type-object-for-a-query-in-a-stored-procedure"></a>A. 为存储过程中的查询创建类型为 OBJECT 的计划指南  
 以下示例创建一个计划指南，它与在基于应用程序的存储过程的上下文中所执行的查询匹配，并将 `OPTIMIZE FOR` 提示应用于该查询。  
  
 下面是此存储过程：  
  
```sql  
IF OBJECT_ID(N'Sales.GetSalesOrderByCountry', N'P') IS NOT NULL  
    DROP PROCEDURE Sales.GetSalesOrderByCountry;  
GO  
CREATE PROCEDURE Sales.GetSalesOrderByCountry   
    (@Country_region nvarchar(60))  
AS  
BEGIN  
    SELECT *  
    FROM Sales.SalesOrderHeader AS h   
    INNER JOIN Sales.Customer AS c ON h.CustomerID = c.CustomerID  
    INNER JOIN Sales.SalesTerritory AS t   
        ON c.TerritoryID = t.TerritoryID  
    WHERE t.CountryRegionCode = @Country_region;  
END  
GO  
```  
  
 下面是为此存储过程中的查询所创建的计划指南：  
  
```sql  
EXEC sp_create_plan_guide   
    @name =  N'Guide1',  
    @stmt = N'SELECT *  
              FROM Sales.SalesOrderHeader AS h   
              INNER JOIN Sales.Customer AS c   
                 ON h.CustomerID = c.CustomerID  
              INNER JOIN Sales.SalesTerritory AS t   
                 ON c.TerritoryID = t.TerritoryID  
              WHERE t.CountryRegionCode = @Country_region',  
    @type = N'OBJECT',  
    @module_or_batch = N'Sales.GetSalesOrderByCountry',  
    @params = NULL,  
    @hints = N'OPTION (OPTIMIZE FOR (@Country_region = N''US''))';  
```  
  
### <a name="b-creating-a-plan-guide-of-type-sql-for-a-stand-alone-query"></a>B. 为独立查询创建类型为 SQL 的计划指南  
 以下示例创建一个计划指南，它与使用 sp_executesql 系统存储过程的应用程序所提交的批处理中的查询匹配。  
  
 下面是这个批处理：  
  
```sql  
SELECT TOP 1 * FROM Sales.SalesOrderHeader ORDER BY OrderDate DESC;  
```  
  
 若要防止为该查询生成并行执行计划，请创建以下计划指南：  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide1',   
    @stmt = N'SELECT TOP 1 *   
              FROM Sales.SalesOrderHeader   
              ORDER BY OrderDate DESC',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (MAXDOP 1)';  
```  
  
### <a name="c-creating-a-plan-guide-of-type-template-for-the-parameterized-form-of-a-query"></a>C. 为参数化格式的查询创建类型为 TEMPLATE 的计划指南  
 以下示例创建一个计划指南，它与被参数化为指定格式的任何查询匹配，并使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 强制执行查询参数化。 下列两个查询在语法上是等价的，差别只是它们的常量文字值。  
  
```sql  
SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
    ON h.SalesOrderID = d.SalesOrderID  
WHERE h.SalesOrderID = 45639;  
  
SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
    ON h.SalesOrderID = d.SalesOrderID  
WHERE h.SalesOrderID = 45640;  
```  
  
 下面是参数化格式的查询的计划指南：  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'TemplateGuide1',  
    @stmt = N'SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
              INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
                  ON h.SalesOrderID = d.SalesOrderID  
              WHERE h.SalesOrderID = @0',  
    @type = N'TEMPLATE',  
    @module_or_batch = NULL,  
    @params = N'@0 int',  
    @hints = N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
 在上一个示例中， `@stmt` 参数的值是参数化格式的查询。 获取此值以在 sp_create_plan_guide 中使用的唯一可靠方法是使用 [sp_get_query_template](../../relational-databases/system-stored-procedures/sp-get-query-template-transact-sql.md) 系统存储过程。 以下脚本既可用来获得参数化查询，又可用来为它创建计划指南。  
  
```sql  
DECLARE @stmt nvarchar(max);  
DECLARE @params nvarchar(max);  
EXEC sp_get_query_template   
    N'SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
      INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
          ON h.SalesOrderID = d.SalesOrderID  
      WHERE h.SalesOrderID = 45639;',  
    @stmt OUTPUT,   
    @params OUTPUT  
EXEC sp_create_plan_guide N'TemplateGuide1',   
    @stmt,   
    N'TEMPLATE',   
    NULL,   
    @params,   
    N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
> [!IMPORTANT]  
>  传递到 sp_get_query_template 的 `@stmt`参数中的常量文字值可能会影响为替换该文字的参数选择的数据类型。 这将影响计划指南的匹配。 可能必须创建多个计划指南，以处理不同的参数值范围。  
  
### <a name="d-creating-a-plan-guide-on-a-query-submitted-by-using-an-api-cursor-request"></a>D. 为通过使用 API 游标请求所提交的查询创建计划指南  
 计划指南可以与通过 API 服务器游标例程所提交的查询匹配。 这些例程包括 sp_cursorprepare、sp_cursorprepexec 和 sp_cursoropen。 使用 ADO、OLE DB 和 ODBC API 的应用程序将使用 API 服务器游标与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 频繁交互。 通过查看 RPC:Starting 事件探查器跟踪事件，可以在 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 跟踪中看到对 API 服务器游标例程的调用。  
  
 假设以下数据出现在希望用计划指南进行优化的查询的 RPC:Starting 事件探查器跟踪事件中：  
  
```sql  
DECLARE @p1 int;  
SET @p1=-1;  
DECLARE @p2 int;  
SET @p2=0;  
DECLARE @p5 int;  
SET @p5=4104;  
DECLARE @p6 int;  
SET @p6=8193;  
DECLARE @p7 int;  
SET @p7=0;  
EXEC sp_cursorprepexec @p1 output,@p2 output,N'@P1 varchar(255),@P2 varchar(255)',N'SELECT * FROM Sales.SalesOrderHeader AS h INNER JOIN Sales.SalesOrderDetail AS d ON h.SalesOrderID = d.SalesOrderID WHERE h.OrderDate BETWEEN @P1 AND @P2',@p5 OUTPUT,@p6 OUTPUT,@p7 OUTPUT,'20040101','20050101'  
SELECT @p1, @p2, @p5, @p6, @p7;  
```  
  
 您会注意到，在 `SELECT` 调用中，`sp_cursorprepexec` 查询计划正在使用合并联接，但您希望使用哈希联接。 使用 `sp_cursorprepexec` 所提交的查询将被参数化，包括查询字符串和参数字符串。 您可以完全按照查询字符串和参数字符串在 `sp_cursorprepexec` 调用中的显示方式（字符对字符）来使用它们创建以下计划指南，从而更改对计划的选择。  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'APICursorGuide',  
    @stmt = N'SELECT * FROM Sales.SalesOrderHeader AS h   
              INNER JOIN Sales.SalesOrderDetail AS d   
                ON h.SalesOrderID = d.SalesOrderID   
              WHERE h.OrderDate BETWEEN @P1 AND @P2',  
    @type = N'SQL',  
    @module_or_batch = NULL,  
    @params = N'@P1 varchar(255),@P2 varchar(255)',  
    @hints = N'OPTION(HASH JOIN)';  
```  
  
 该计划指南将影响应用程序随后对该查询的执行，并且哈希联接将用来处理该查询。  
  
### <a name="e-creating-a-plan-guide-by-obtaining-the-xml-showplan-from-a-cached-plan"></a>E. 通过从缓存计划中获取 XML 显示计划来创建计划指南  
 下面的示例为简单的临时 SQL 语句创建一个计划指南。 在计划指南中，直接在 `@hints` 参数中为查询指定 XML 显示计划，从而为该语句提供了所需的查询计划。 该示例首先通过执行 SQL 语句在计划缓存中生成一个计划。 对于此示例，假定所生成的计划就是所需的计划，不需要做进一步的查询优化。 此查询的 XML 显示计划可通过查询 `sys.dm_exec_query_stats`、 `sys.dm_exec_sql_text`和 `sys.dm_exec_text_query_plan` 动态管理视图获得，并可以分配给 `@xml_showplan` 变量。 然后，将 `@xml_showplan` 变量传递给 `sp_create_plan_guide` 语句中的 `@hints` 参数。 也可以使用 [sp_create_plan_guide_from_handle](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md) 存储过程从计划缓存中的查询计划中创建计划指南。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;  
GO  
DECLARE @xml_showplan nvarchar(max);  
SET @xml_showplan = (SELECT query_plan  
    FROM sys.dm_exec_query_stats AS qs   
    CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
    CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, DEFAULT, DEFAULT) AS qp  
    WHERE st.text LIKE N'SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;%');  
  
EXEC sp_create_plan_guide   
    @name = N'Guide1_from_XML_showplan',   
    @stmt = N'SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints =@xml_showplan;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [计划指南](../../relational-databases/performance/plan-guides.md)   
 [sp_control_plan_guide (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)   
 [sys.plan_guides (Transact-SQL)](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)   
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_cached_plans (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sp_create_plan_guide_from_handle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)   
 [sys.fn_validate_plan_guide (Transact-SQL)](../../relational-databases/system-functions/sys-fn-validate-plan-guide-transact-sql.md)   
 [sp_get_query_template &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-get-query-template-transact-sql.md)  
  
  
