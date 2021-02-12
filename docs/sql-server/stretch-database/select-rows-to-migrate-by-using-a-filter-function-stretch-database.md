---
description: 使用筛选器函数选择要迁移的行 (Stretch Database)
title: 使用筛选器函数选择要迁移的行
ms.date: 06/27/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, predicates
- predicates for Stretch Database
- Stretch Database, inline table-valued functions
- inline table-valued functions for Stretch Database
ms.assetid: 090890ee-7620-4a08-8e15-d2fbc71dd12f
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 72162b4ed6339bcf7939e1903886170e3df36e63
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030890"
---
# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>使用筛选器函数选择要迁移的行 (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  如果在单独的某个表中存储了冷数据，则可以将 Stretch Database 配置为迁移整个表。 另一方面，如果表中包含热数据和冷数据，则可以指定筛选器谓词以选择要迁移的行。 筛选器谓词是一个内联表值函数。 本文介绍如何编写内联表值函数以选择要迁移的行。  
  
> [!IMPORTANT]
> 如果提供的筛选器函数性能不佳，则数据迁移性能也不佳。 Stretch Database 通过使用 CROSS APPLY 运算符将筛选器函数应用到表。  
  
 如果未指定筛选器函数，则将迁移整个表。  
  
 如果运行“启用数据库延伸向导”，则可以迁移整个表，或在向导中指定一个简单的筛选器函数。 如果想要使用不同类型的筛选器函数来选择要迁移的行，请执行以下操作之一。  
  
-   退出向导并运行 ALTER TABLE 语句以对表启用 Stretch 并指定筛选器函数。  
  
-   运行 ALTER TABLE 语句以在退出向导后指定筛选器函数。  
  
 本主题的后面部分将介绍用于添加函数的 ALTER TABLE 语法。  
  
## <a name="basic-requirements-for-the-filter-function"></a>筛选器函数的基本要求  
 Stretch Database 筛选器谓词所需的内联表值函数如下面的示例所示。  
  
```sql  
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE <predicate>  
```  
  
 该函数的参数必须是表中列的标识符。  
  
 需要进行架构绑定以防止筛选器函数使用的列被删除或被更改。  
  
### <a name="return-value"></a>返回值  
 如果该函数返回非空结果，则该行符合迁移条件。 否则（也就是说，如果该函数未返回结果），该行不符合迁移条件。  
  
### <a name="conditions"></a>Conditions  
 &lt;*谓词*&gt; 可以包含一个条件，或使用 AND 逻辑运算符联接的多个条件。  
  
```  
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]  
```  
  
 而每个条件又包含一个基元条件或者使用 OR 逻辑运算符联接的多个基元条件。  
  
```  
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]  
```  
  
### <a name="primitive-conditions"></a>基元条件  
 基元条件可以执行以下比较之一。  
  
```  
<primitive_condition> ::=   
{  
<function_parameter> <comparison_operator> constant  
| <function_parameter> { IS NULL | IS NOT NULL }  
| <function_parameter> IN ( constant [ ,...n ] )  
}  
  
```  
  
-   将函数参数与常量表达式进行比较。 例如，`@column1 < 1000`。  
  
     以下示例会检查 *date* 列的值是否 &lt; 1/1/2016。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
    GO  
  
    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),  
        MIGRATION_STATE = OUTBOUND  
    ) )  
  
    ```  
  
-   向函数参数应用 IS NULL 或 IS NOT NULL 运算符。  
  
-   使用 IN 运算符将函数参数与常量值列表进行比较。  
  
     下面是一个示例，用于检查 *shipment_status*  列的值是否为 `IN (N'Completed', N'Returned', N'Cancelled')`。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')  
    GO  
  
    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),  
        MIGRATION_STATE = OUTBOUND  
    ) )  
  
    ```  
  
### <a name="comparison-operators"></a>比较运算符  
 支持下列比较运算符。  
  
 `<, <=, >, >=, =, <>, !=, !<, !>`  
  
```  
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }  
```  
  
### <a name="constant-expressions"></a>常量表达式  
 在筛选器函数中使用的常量可以是定义函数时可以计算的任何确定性表达式。 常量表达式可以包含以下内容。  
  
-   文字。 例如，`N'abc', 123`。  
  
-   代数表达式。 例如，`123 + 456`。  
  
-   确定性函数。 例如，`SQRT(900)`。  
  
-   使用 CAST 或 CONVERT 的确定性转换。 例如，`CONVERT(datetime, '1/1/2016', 101)`。  
  
### <a name="other-expressions"></a>其他表达式  
 如果在将 BETWEEN 和 NOT BETWEEN 运算符替换为等效的 AND 和 OR 表达式后，生成的函数符合本文所述的规则，则可以使用这些 BETWEEN 和 NOT BETWEEN 运算符。  
  
 不能使用子查询或非确定性函数，如 RAND() 或 GETDATE()。  
  
## <a name="add-a-filter-function-to-a-table"></a>向表中添加筛选器函数  
 通过运行 **ALTER TABLE** 语句并将现有的内联表值函数指定为 **FILTER_PREDICATE** 参数的值，向表中添加筛选器函数。 例如：  
  
```sql  
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),  
    MIGRATION_STATE = <desired_migration_state>  
) )  
  
```  
  
 将函数作为谓词绑定到表后，将发生以下情况。  
  
-   下次进行数据迁移时，只有该函数返回非空值的行才会迁移。  
  
-   该函数使用的列将绑定到架构。 只要表使用该函数作为其筛选器谓词，你就不能更改这些列。  
  
 只要表使用该函数作为其筛选器谓词，你就不能删除内联表值函数。 

> [!TIP]
> 若要提高筛选器函数的性能，请在列上创建由该函数使用的索引。

 ### <a name="passing-column-names-to-the-filter-function"></a>将列名传递给筛选器函数
 
 当向表分配筛选器函数时，请将传递给筛选器函数的列名称指定为单个部分组成的名称。 如果在传递列名称时指定三个部分组成的名称，则对已启用 Stretch 的表的后续查询将失败。

例如，如果指定下面的示例中所示的三部分列名，该语句将成功运行，但对该表的后续查询会失败。

```sql
ALTER TABLE SensorTelemetry 
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

请改为将筛选器函数指定为单个部分组成的列名称，如下例所示。

```sql
ALTER TABLE SensorTelemetry 
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```
  
## <a name="add-a-filter-function-after-running-the-wizard"></a><a name="addafterwiz"></a>运行向导后添加筛选器函数  
  
如果想要使用无法在“启用数据库延伸”向导中创建的函数，请退出向导，然后运行 **ALTER TABLE** 语句以指定函数。 但是，在应用函数之前，必须停止已在进行的数据迁移并取回已迁移的数据。 （有关其必要性的原因的详细信息，请参阅 [替换现有的筛选器函数](#replacePredicate)。）
  
1. 反向迁移和取回已迁移的数据。 启动此操作后将无法取消此操作。 由于出站数据传输（流出），所以也会在 Azure 上产生成本。 有关详细信息，请参阅 [Azure 定价方式](https://azure.microsoft.com/pricing/details/data-transfers/)。  
  
    ```sql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  
  
2. 等待迁移完成。 你可以从 SQL Server Management Studio 检查 **Stretch Database 监视器** 中的状态，或者可以查询 **sys.dm_db_rda_migration_status** 视图。 有关详细信息，请参阅 [数据迁移的监视与故障排除](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) 或 [sys.dm_db_rda_migration_status](../../relational-databases/system-dynamic-management-views/stretch-database-sys-dm-db-rda-migration-status.md)。  
  
3. 创建要应用到表的筛选器函数。  
  
4. 将该函数添加到表，并重新启动到 Azure 的数据迁移。  
  
    ```sql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
            );   
    ```  
  
## <a name="filter-rows-by-date"></a>按日期筛选行  
 下面的示例将迁移 **日期** 列中包含值早于 2016 年 1 月 1 日的行。  
  
```sql  
-- Filter by date  
--  
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)  
GO  
  
```  
  
## <a name="filter-rows-by-the-value-in-a-status-column"></a>按状态列中的值筛选行  
 下面的示例将迁移 **状态** 列中包含指定值之一的行。  
  
```sql  
-- Filter by status column  
--  
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')  
GO  
  
```  
  
## <a name="filter-rows-by-using-a-sliding-window"></a>使用滑动窗口筛选行  
 若要使用滑动窗口筛选行，请记住筛选器函数的下列要求。  
  
-   该函数必须是确定性函数。 因此，不能创建随着时间推移自动重新计算滑动窗口的函数。  
  
-   该函数使用架构绑定。 因此，不能只是通过调用 **ALTER FUNCTION** 移动滑动窗口来每日“就地”更新函数。  
  
 以筛选器函数开头，如下例所示，该示例迁移 **systemEndTime** 列在其中包含早于 2016 年 1 月 1 日的值的行。  
  
```sql  
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)   
RETURNS TABLE   
WITH SCHEMABINDING    
AS    
RETURN SELECT 1 AS is_eligible   
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;  
  
```  
  
 将筛选器函数应用于表。  
  
```sql  
ALTER TABLE <table name>   
SET (   
        REMOTE_DATA_ARCHIVE = ON   
                (   
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101(SysEndTime)  
                                , MIGRATION_STATE = OUTBOUND   
                )  
        )   
;  
  
```  
  
 如果要更新滑动窗口，请执行以下操作。  
  
1.  创建一个新函数，以指定新的滑动窗口。 下面的示例选择日期早于 2016 年 1 月 2 日，而不是 2016 年 1 月 1 日。  
  
2.  通过调用 **ALTER TABLE** 以前的筛选器函数替换为新的筛选器函数，如下面的示例中所示。  
  
3.  （可选）通过调用 **DROP FUNCTION** 删除你不再使用的前一个筛选器函数。 （示例中未显示此步骤。）  
  
```sql  
BEGIN TRAN  
GO  
        /*(1) Create new predicate function definition */  
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)  
        RETURNS TABLE  
        WITH SCHEMABINDING   
        AS   
        RETURN SELECT 1 AS is_eligible  
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)  
        GO  
  
        /*(2) Set the new function as the filter predicate */  
        ALTER TABLE <table name>  
        SET   
        (  
               REMOTE_DATA_ARCHIVE = ON  
               (  
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),  
                       MIGRATION_STATE = OUTBOUND  
               )  
        )   
COMMIT ;  
  
```  
  
## <a name="more-examples-of-valid-filter-functions"></a>有效筛选器函数的更多示例  
  
-   下面的示例通过使用 AND 逻辑运算符组合两个基元条件。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')  
    GO  
  
    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),  
        MIGRATION_STATE = OUTBOUND  
    ) )  
  
    ```  
  
-   下面的示例使用多个条件和通过 CONVERT 进行的确定性转换。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101) AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL) AND @column3 IN (N'Completed', N'Returned', N'Cancelled')  
    GO  
  
    ```  
  
-   下面的示例使用数学运算符和函数。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < SQRT(400) + 10  
    GO  
  
    ```  
  
-   以下示例使用 BETWEEN 和 NOT BETWEEN 运算符。 这种用法是有效的，因为在将 BETWEEN 和 NOT BETWEEN 运算符替换为等效的 AND 和 OR 表达式之后，生成的函数符合此处所述的规则。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 BETWEEN 0 AND 100  
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)  
    GO  
  
    ```  
  
     在将 BETWEEN 和 NOT BETWEEN 运算符替换为等效的 AND 和 OR 表达式后，上面的函数等效于下面的函数。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 >= 0 AND @column1 <= 100 AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)  
    GO  
  
    ```  
  
## <a name="examples-of-filter-functions-that-arent-valid"></a>无效的筛选器函数的示例  
  
-   下面的函数无效，因为它包含非确定性转换。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < CONVERT(datetime, '1/1/2016')  
    GO  
  
    ```  
  
-   下面的函数无效，因为它包含非确定性函数调用。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < DATEADD(day, -60, GETDATE())  
    GO  
  
    ```  
  
-   下面的函数无效，因为它包含子查询。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example7(@column1 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct')  
    GO  
  
    ```  
  
-   下面的函数无效，因为在定义函数时，使用代数运算符或内置函数的表达式的计算结果必须为常量。 不能在代数表达式或函数调用中包含列引用。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example8(@column1 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 % 2 =  0  
    GO  
  
    CREATE FUNCTION dbo.fn_example9(@column1 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE SQRT(@column1) = 30  
    GO  
  
    ```  
  
-   下面的函数无效，因为在将 BETWEEN 运算符替换为等效的 AND 表达式之后，它违反了此处所述的规则。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING  
    AS  
    RETURN  SELECT 1 AS is_eligible  
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000  
    GO  
  
    ```  
  
     在将 BETWEEN 运算符替换为等效的 AND 表达式后，上面的函数等效于下面的函数。 此函数无效，因为基元条件只能使用 OR 逻辑运算符。  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000  
    GO  
  
    ```  
  
## <a name="how-stretch-database-applies-the-filter-function"></a>Stretch Database 应用筛选器函数的方式  
 Stretch Database 通过使用 CROSS APPLY 运算符将筛选器函数应用到表并确定符合条件的行。 例如：  
  
```sql  
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)  
```  
  
 如果此函数针对行返回非空结果，则该行符合迁移条件。  
  
## <a name="replace-an-existing-filter-function"></a><a name="replacePredicate"></a>替换现有的筛选器函数  
 可以通过再次运行 **ALTER TABLE** 语句并为 **FILTER_PREDICATE** 参数指定新值来替换以前指定的筛选器函数。 例如：  
  
```sql  
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),  
    MIGRATION_STATE = <desired_migration_state>  
    ) ) 
  
```  
  
 新的内联表值函数具有以下要求。  
  
-   新函数必须比以前的函数限制少。  
  
-   旧函数中存在的所有运算符必须都存在于新函数中。  
  
-   新函数不能包含旧函数中不存在的运算符。  
  
-   运算符参数的顺序不能更改。  
  
-   只能在某种程度上更改是 `<, <=, >, >=`  比较的一部分的常量值，从而使函数限制更少。  
  
### <a name="example-of-a-valid-replacement"></a>有效替换的示例  
 假定下面的函数是当前的筛选器函数。  
  
```sql  
CREATE FUNCTION dbo.fn_stretchpredicate_old(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
            AND (@column2 < -100 OR @column2 > 100)  
GO  
  
```  
  
 下面的函数是有效替换，因为新的日期常量（它指定更后的截止日期）使函数的限制更少。  
  
```sql  
CREATE FUNCTION dbo.fn_stretchpredicate_new(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)  
            AND (@column2 < -50 OR @column2 > 50)  
GO  
  
```  
  
### <a name="examples-of-replacements-that-arent-valid"></a>无效替换的示例  
 下面的函数是无效替换，因为新的日期常量（它指定更早的截止日期）未使函数的限制更少。  
  
```sql  
CREATE FUNCTION dbo.fn_notvalidreplacement_1(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)  
            AND (@column2 < -100 OR @column2 > 100)  
GO  
  
```  
  
 下面的函数不是有效替换，因为删除了一个比较运算符。  
  
```sql  
CREATE FUNCTION dbo.fn_notvalidreplacement_2(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
            AND (@column2 < -50)  
GO  
  
```  
  
 下面的函数不是有效替换，因为使用 AND 逻辑运算符添加了一个新条件。  
  
```sql  
CREATE FUNCTION dbo.fn_notvalidreplacement_3(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
            AND (@column2 < -100 OR @column2 > 100)  
            AND (@column2 <> 0)  
GO  
  
```  
  
## <a name="remove-a-filter-function-from-a-table"></a>从表中删除筛选器函数  
 若要迁移整个表而不是所选的行，请通过将 **FILTER_PREDICATE**  设置为 null 来删除现有函数。 例如：  
  
```sql  
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
    FILTER_PREDICATE = NULL,  
    MIGRATION_STATE = <desired_migration_state>  
) )  
  
```  
  
 删除筛选器函数后，表中的所有行都将符合迁移条件。 因此，以后将不能为同一个表指定筛选器函数，除非你先从 Azure 取回该表的所有远程数据。 之所以存在此限制是为了避免出现这样的情况：即提供新的筛选器函数时不符合迁移条件的行已迁移到 Azure。  
  
## <a name="check-the-filter-function-applied-to-a-table"></a>检查应用到表的筛选器函数  
 若要检查应用到表的筛选器函数，请打开目录视图 **sys.remote_data_archive_tables** ，并检查 **filter_predicate** 列的值。 如果该值为 null，则整个表都符合存档条件。 有关详细信息，请参阅 [sys.remote_data_archive_tables (Transact-SQL)](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md)。  
  
## <a name="security-notes-for-filter-functions"></a>筛选器函数的安全说明  
被泄露的具有 db_owner 权限的帐户可以执行以下操作。  
  
-   创建并应用消耗大量服务器资源或长时间等待从而导致拒绝服务的表值函数。  
  
-   创建并应用表值函数，该函数可能推断出用户被显式拒绝读取访问权限的表的内容。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
  
  
