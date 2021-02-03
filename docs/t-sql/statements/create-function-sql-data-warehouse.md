---
description: CREATE FUNCTION (Azure Synapse Analytics)
title: CREATE FUNCTION (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 09/17/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 8cad1b2c-5ea0-4001-9060-2f6832ccd057
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: ffa098db4ca61746a3301702cd526498f0cd2f85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191016"
---
# <a name="create-function-azure-synapse-analytics"></a>CREATE FUNCTION (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  在 [!INCLUDE[ssSDW](../../includes/ssazuresynapse_md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中创建用户定义函数。 用户定义函数是接受参数、执行操作（例如复杂计算）并将操作结果以值的形式返回的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 例程。 在 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中，返回值必须是标量（单个）值。 在 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 中，CREATE FUNCTION 可以通过使用内联表值函数（预览）的语法来返回表，也可以通过使用标量函数的语法来返回单个值。 使用此语句可以创建可通过以下方式使用的重复使用的例程：  
  
-   在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句（如 SELECT）中  
  
-   在调用该函数的应用程序中  
  
-   在另一个用户定义函数的定义中  
  
-   用于为列定义 CHECK 约束  
  
-   用于替换存储过程  
  
-   使用内联函数作为安全策略的筛选器谓词  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Transact-SQL Scalar Function Syntax  (in Azure Synapse Analytics and Parallel Data Warehouse)
CREATE FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS return_data_type  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    BEGIN   
        function_body   
        RETURN scalar_expression  
    END  
[ ; ]  
  
<function_option>::=   
{  
    [ SCHEMABINDING ]  
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]  
}  
```

```syntaxsql
-- Transact-SQL Inline Table-Valued Function Syntax (Preview in Azure Synapse Analytics only)
CREATE FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] parameter_data_type
    [ = default ] }
    [ ,...n ]
  ]
)
RETURNS TABLE
    [ WITH SCHEMABINDING ]
    [ AS ]
    RETURN [ ( ] select_stmt [ ) ]
[ ; ]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>参数  
 *schema_name*  
 用户定义函数所属的架构的名称。  
  
 function_name  
 用户定义函数的名称。 函数名称必须符合标识符规则，并且在数据库中以及对其架构来说是唯一的。  
  
> [!NOTE]  
>  即使未指定参数，函数名称后也需要加上括号。  
  
 @parameter_name  
 用户定义函数中的参数。 可声明一个或多个参数。  
  
 一个函数最多可以有 2,100 个参数。 执行函数时，如果未定义参数的默认值，则用户必须提供每个已声明参数的值。  
  
 通过将 at 符号 (@) 用作第一个字符来指定参数名称。 参数名称必须符合标识符规则。 参数是对应于函数的局部参数；其他函数中可使用相同的参数名称。 参数只能代替常量，而不能用于代替表名、列名或其他数据库对象的名称。  
  
> [!NOTE]  
>  在传递存储过程或用户定义函数中的参数时，或在声明和设置批语句中的变量时，不会遵守 ANSI_WARNINGS。 例如，如果将一个变量定义为 char(3)，然后将其值设置为大于三个字符，则数据会被截断为定义的大小，并且 INSERT 或 UPDATE 语句可以成功执行。  
  
 parameter_data_type  
 参数数据类型。 对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数，允许在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 中支持所有标量数据类型。 不支持 timestamp (rowversion) 数据类型。  
  
 [ =default ]  
 参数的默认值。 如果定义了 default 值，则无需指定此参数的值即可执行函数。  
  
 如果函数的参数有默认值，则调用该函数以检索默认值时，必须指定关键字 DEFAULT。 此行为与在存储过程中使用具有默认值的参数不同，在后一种情况下，不提供参数同样意味着使用默认值。  
  
 return_data_type  
 标量用户定义函数的返回值。 对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数，允许在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 中支持所有标量数据类型。 不支持 timestamp (rowversion) 数据类型。 不允许光标和表的非标量类型。  
  
 function_body  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句系列。  function_body 不能包含 SELECT 语句且不能引用数据库数据。  function_body 不能引用表或视图。 function body 可以调用其他确定性的函数但不能调用不确定性函数。 
  
 在标量函数中，function_body 是一系列 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，这些语句一起使用可计算出标量值。  
  
 *scalar_expression*  
 指定标量函数返回的标量值。  

 select_stmt 适用范围：Azure Synapse Analytics  
 定义内联表值函数（预览）返回值的单个 SELECT 语句。

 TABLE 适用范围：Azure Synapse Analytics  
 指定表值函数 (TVF) 的返回值为表。 只有常量和 @local_variables 可以传递到 TVF。

 在内联 TVF（预览）中，TABLE 返回值通过单个 SELECT 语句定义。 内联函数没有关联的返回变量。
  
 **\<function_option>::=** 
  
 指定函数将具有以下一个或多个选项：  
  
 SCHEMABINDING  
 指定将函数绑定到其引用的数据库对象。 如果指定了 SCHEMABINDING，则不能按照将影响函数定义的方式修改基对象。 必须首先修改或删除函数定义本身，才能删除将要修改的对象的依赖关系。  
  
 只有发生下列操作之一时，才会删除函数与其引用对象的绑定：  
  
-   删除函数。  
  
-   在未指定 SCHEMABINDING 选项的情况下，使用 ALTER 语句修改函数。  
  
 只有满足以下条件时，函数才能绑定到架构：  
  
-   该函数引用的任何用户定义函数也绑定到架构。  
  
-   使用由一个部分或两个部分组成的名称引用该函数和由该函数引用的其他 UDF。  
  
-   UDF 的正文中只能引用同一数据库中的内置函数和其他 UDF。  
  
-   执行 CREATE FUNCTION 语句的用户对该函数引用的数据库对象具有 REFERENCES 权限。  
  
 使用 ALTER 删除 SCHEMABINDING  
  
 RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT  
 指定标量值函数的 OnNULLCall 属性。 如果未指定，则默认为 CALLED ON NULL INPUT。 这意味着即使传递的参数为 NULL，也将执行函数体。  
  
## <a name="best-practices"></a>最佳实践  
 如果用户定义函数不是使用 SCHEMABINDING 子句创建的，则对基础对象进行的任何更改可能会影响函数定义并在调用函数时可能导致意外结果。 我们建议您实现以下方法之一，以便确保函数不会由于对于其基础对象的更改而过期：  
  
-   在您创建函数时指定 WITH SCHEMABINDING 子句。 这确保除非也修改了函数，否则无法修改在函数定义中引用的对象。  
  
## <a name="interoperability"></a>互操作性  
 下列语句在标量值函数中有效：  
  
-   赋值语句。  
  
-   TRY...CATCH 语句以外的流控制语句。  
  
-   定义本地数据变量的 DECLARE 语句。  

在内联表值函数（预览）中，只允许使用单个 SELECT 语句。
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 用户定义函数不能用于执行修改数据库状态的操作。  
  
 用户定义函数可以嵌套；也就是说，用户定义函数可相互调用。 被调用函数开始执行时，嵌套级别将增加；被调用函数执行结束后，嵌套级别将减少。 用户定义函数的嵌套级别最多可达 32 级。 如果超出最大嵌套级别数，整个调用函数链将失败。   
  
## <a name="metadata"></a>元数据  
 本部分列出可用于返回与用户定义函数有关的元数据的系统目录视图。  
  
 [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)：显示 [!INCLUDE[tsql](../../includes/tsql-md.md)] 用户定义函数的定义。 例如：  
  
```sql  
SELECT definition, type   
FROM sys.sql_modules AS m  
JOIN sys.objects AS o   
    ON m.object_id = o.object_id   
    AND type = ('FN');  
GO  
  
```  
  
 [sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)：显示用户定义函数中定义的参数的有关信息。  
  
 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)：显示函数所引用的基础对象。  
  
## <a name="permissions"></a>权限  
 需要在数据库中具有 CREATE FUNCTION 权限，并对创建函数时所在的架构具有 ALTER 权限。  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-using-a-scalar-valued-user-defined-function-to-change-a-data-type"></a>A. 使用标量值用户定义函数更改数据类型。  
 此简单函数将 int 数据类型作为输入，并返回 decimal(10,2) 数据类型作为输入 。  
  
```sql  
CREATE FUNCTION dbo.ConvertInput (@MyValueIn int)  
RETURNS decimal(10,2)  
AS  
BEGIN  
    DECLARE @MyValueOut int;  
    SET @MyValueOut= CAST( @MyValueIn AS decimal(10,2));  
    RETURN(@MyValueOut);  
END;  
GO  
  
SELECT dbo.ConvertInput(15) AS 'ConvertedValue';  
```  

## <a name="examples-sssdwfull"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]  

### <a name="a-creating-an-inline-table-valued-function-preview"></a>A. 创建内联表值函数（预览）
 下面的示例创建一个内联表值函数，以返回按 `objectType` 参数筛选的模块的某些关键信息。 它包含一个默认值，以在使用 DEFAULT 参数调用函数时返回所有模块。 此示例使用[元数据](#metadata)中提到的一些系统目录视图。

```sql
CREATE FUNCTION dbo.ModulesByType(@objectType CHAR(2) = '%%')
RETURNS TABLE
AS
RETURN
(
    SELECT 
        sm.object_id AS 'Object Id',
        o.create_date AS 'Date Created',
        OBJECT_NAME(sm.object_id) AS 'Name',
        o.type AS 'Type',
        o.type_desc AS 'Type Description', 
        sm.definition AS 'Module Description'
    FROM sys.sql_modules AS sm  
    JOIN sys.objects AS o ON sm.object_id = o.object_id
    WHERE o.type like '%' + @objectType + '%'
);
GO
```
然后，可以调用该函数以返回所有视图 (V) 对象：
```sql
select * from dbo.ModulesByType('V');
```

### <a name="b-combining-results-of-an-inline-table-valued-function-preview"></a>B. 合并内联表值函数（预览）的结果
 这个简单的示例使用以前创建的内联 TVF 来演示如何使用 CROSS APPLY 将其结果与其他表合并。 在这里，我们选择了 sys.objects 中的所有列，以及“类型”列上匹配的所有行的 `ModulesByType` 结果。 有关使用 APPLY 的详细信息，请参阅 [FROM 子句以及 JOIN、APPLY、PIVOT](../../t-sql/queries/from-transact-sql.md)。

```sql
SELECT * 
FROM sys.objects o
CROSS APPLY dbo.ModulesByType(o.type);
GO
```
  
## <a name="see-also"></a>另请参阅  
 [ALTER FUNCTION (SQL Server PDW)](/previous-versions/sql/)   
 [DROP FUNCTION (SQL Server PDW)](/previous-versions/sql/)  
  
  

