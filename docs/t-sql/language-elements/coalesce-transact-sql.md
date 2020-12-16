---
title: COALESCE (Transact-SQL) | Microsoft Docs
description: COALESCE 的 Transact-SQL 参考，返回值不是 NULL 的第一个表达式的值。
ms.date: 08/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COALESCE
- COALESCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], nonnull
- COALESCE function
- first nonnull expressions [SQL Server]
- nonnull expressions
ms.assetid: fafc0dba-f8a8-4aad-9b7f-908e34b74d88
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9af0697af3d751382d9beea576e97ec59a09fd18
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439174"
---
# <a name="coalesce-transact-sql"></a>COALESCE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

按顺序计算变量并返回最初不等于 `NULL` 的第一个表达式的当前值。 例如，`SELECT COALESCE(NULL, NULL, 'third_value', 'fourth_value');` 返回第三个值，因为第三个值是首个为非 Null 的值。 
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
COALESCE ( expression [ ,...n ] )   
```  
  
## <a name="arguments"></a>参数  
_expression_  
是任何类型的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
返回数据类型优先级最高的 expression 的数据类型  。 如果所有表达式都不可为 Null，则结果的类型也不可为 Null。  
  
## <a name="remarks"></a>备注  
如果所有参数都为 `NULL`，则 `COALESCE` 返回 `NULL`。 至少应有一个 Null 值为 `NULL` 类型。  
  
## <a name="comparing-coalesce-and-case"></a>比较 COALESCE 和 CASE  
`COALESCE` 表达式是 `CASE` 表达式的语法快捷方式。  即查询优化器将代码 `COALESCE`(expression1,...n) 重写为以下 `CASE` 表达式   ：  
  
```sql  
CASE  
WHEN (expression1 IS NOT NULL) THEN expression1  
WHEN (expression2 IS NOT NULL) THEN expression2  
...  
ELSE expressionN  
END  
```  
  
因此，输入值（_expression1_、_expression2_、_expressionN_ 等）会被计算多次。 包含子查询的值表达式被视为不确定的且子查询会被计算两次。 此结果符合 SQL 标准。 在每种情况中，第一次计算和后续计算可能返回不同的结果。  
  
例如，执行代码 `COALESCE((subquery), 1)` 时，计算子查询两次。 因此，您可能得到不同的结果，具体取决于查询的隔离级别。 例如，在多用户环境中，代码在 `READ COMMITTED` 隔离级别下可能返回 `NULL`。 要确保返回稳定的结果，请使用 `SNAPSHOT ISOLATION` 隔离级别，或使用 `ISNULL` 函数替换 `COALESCE`。 此外，可以重写查询以将子查询推送到嵌套 select，如以下示例中所示：  
  
```sql  
SELECT CASE WHEN x IS NOT NULL THEN x ELSE 1 END  
FROM  
(  
SELECT (SELECT Nullable FROM Demo WHERE SomeCol = 1) AS x  
) AS T;  
  
```  
  
## <a name="comparing-coalesce-and-isnull"></a>比较 COALESCE 和 ISNULL  
`ISNULL` 函数和 `COALESCE` 表达式具有相似的用途，但是行为可能不同。  
  
1.  因为 `ISNULL` 是函数，它只能被计算一次。  如上所述，可以多次计算 `COALESCE` 表达式的输入值。  
  
2.  确定结果表达式的数据类型方式不同。 `ISNULL` 使用第一个参数的数据类型，`COALESCE` 则遵循 `CASE` 表达式规则并返回具有最高优先级的值的数据类型。  
  
3.  结果表达式是否可为 NULL 对于 `ISNULL` 和 `COALESCE` 是不同的。 `ISNULL` 返回值始终被视为不可为 NULL（假定返回值不可为 null）。 相反，具有非 null 参数的 `COALESCE` 被视为可以为 `NULL`。 因此，尽管表达式 `ISNULL(NULL, 1)` 和 `COALESCE(NULL, 1)` 是等效的，但其在结果是否为 null 值方面是不同的。 如果正在计算列中使用这些表达式、创建键约束或生成标量 UDF 确定性的返回值以便可以编制索引，这些值可能会带来不同结果，如以下示例中所示：  
  
    ```sql  
    USE tempdb;  
    GO  
    -- This statement fails because the PRIMARY KEY cannot accept NULL values  
    -- and the nullability of the COALESCE expression for col2   
    -- evaluates to NULL.  
    CREATE TABLE #Demo   
    (   
      col1 INTEGER NULL,   
      col2 AS COALESCE(col1, 0) PRIMARY KEY,   
      col3 AS ISNULL(col1, 0)   
    );   
  
    -- This statement succeeds because the nullability of the   
    -- ISNULL function evaluates AS NOT NULL.  
  
    CREATE TABLE #Demo   
    (   
      col1 INTEGER NULL,   
      col2 AS COALESCE(col1, 0),   
      col3 AS ISNULL(col1, 0) PRIMARY KEY   
    );  
    ```  
  
4.  `ISNULL` 和 `COALESCE` 的验证也不同。 例如，可以将 `ISNULL` 的 `NULL` 值转换为 **int**；而对于 `COALESCE`，则必须提供数据类型。  
  
5.  `ISNULL` 仅采用两个参数。 与此相反，`COALESCE` 采用可变数量的参数。  
  
## <a name="examples"></a>示例  
  
### <a name="a-running-a-simple-example"></a>A. 运行简单示例  
下面的示例演示 `COALESCE` 如何从第一个具有非 Null 值的列中选择数据。 此示例使用 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库。  
  
```sql  
SELECT Name, Class, Color, ProductNumber,  
COALESCE(Class, Color, ProductNumber) AS FirstNotNull  
FROM Production.Product;  
```  
  
### <a name="b-running-a-complex-example"></a>B. 运行复杂示例  
在以下示例中，`wages` 表中包括以下三列，它们包含有关雇员的年薪的信息：hourly wage、salary 和 commission。 但是，每个雇员只能接受一种付款方式。 若要确定支付给所有雇员的金额总数，请使用 `COALESCE` 仅接受在 `hourly_wage`、`salary` 和 `commission` 中找到的非 Null 值。  
  
```sql  
SET NOCOUNT ON;  
GO  
USE tempdb;  
IF OBJECT_ID('dbo.wages') IS NOT NULL  
    DROP TABLE wages;  
GO  
CREATE TABLE dbo.wages  
(  
    emp_id        TINYINT   IDENTITY,  
    hourly_wage   DECIMAL   NULL,  
    salary        DECIMAL   NULL,  
    commission    DECIMAL   NULL,  
    num_sales     TINYINT   NULL  
);  
GO  
INSERT dbo.wages (hourly_wage, salary, commission, num_sales)  
VALUES  
    (10.00, NULL, NULL, NULL),  
    (20.00, NULL, NULL, NULL),  
    (30.00, NULL, NULL, NULL),  
    (40.00, NULL, NULL, NULL),  
    (NULL, 10000.00, NULL, NULL),  
    (NULL, 20000.00, NULL, NULL),  
    (NULL, 30000.00, NULL, NULL),  
    (NULL, 40000.00, NULL, NULL),  
    (NULL, NULL, 15000, 3),  
    (NULL, NULL, 25000, 2),  
    (NULL, NULL, 20000, 6),  
    (NULL, NULL, 14000, 4);  
GO  
SET NOCOUNT OFF;  
GO  
SELECT CAST(COALESCE(hourly_wage * 40 * 52,   
   salary,   
   commission * num_sales) AS money) AS 'Total Salary'   
FROM dbo.wages  
ORDER BY 'Total Salary';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```   
Total Salary  
------------  
10000.00  
20000.00  
20800.00  
30000.00  
40000.00  
41600.00  
45000.00  
50000.00  
56000.00  
62400.00  
83200.00  
120000.00  
  
(12 row(s) affected)
```  
  
### <a name="c-simple-example"></a>C:简单示例  
下面的示例演示 `COALESCE` 如何从第一个具有非 Null 值的列中选择数据。 对于此示例，假定 `Products` 表包含此数据：  
  
```  
Name         Color      ProductNumber  
------------ ---------- -------------  
Socks, Mens  NULL       PN1278  
Socks, Mens  Blue       PN1965  
NULL         White      PN9876
```  
 
然后，我们运行以下 COALESCE 查询：  
  
```sql  
SELECT Name, Color, ProductNumber, COALESCE(Color, ProductNumber) AS FirstNotNull   
FROM Products ;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Name         Color      ProductNumber  FirstNotNull  
------------ ---------- -------------  ------------  
Socks, Mens  NULL       PN1278         PN1278  
Socks, Mens  Blue       PN1965         Blue  
NULL         White      PN9876         White
```  
  
请注意，在第一行中，`FirstNotNull` 值是 `PN1278`，而不是 `Socks, Mens`。 此值之所以这样是因为示例中未将 `Name` 列指定为 `COALESCE` 的参数。  
  
### <a name="d-complex-example"></a>D:复杂示例  
以下示例使用 `COALESCE` 来比较三个列中的值，并仅返回列中找到的非 null 值。  
  
```sql  
CREATE TABLE dbo.wages  
(  
    emp_id        TINYINT   NULL,  
    hourly_wage   DECIMAL   NULL,  
    salary        DECIMAL   NULL,  
    commission    DECIMAL   NULL,  
    num_sales     TINYINT   NULL  
);  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (1, 10.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (2, 20.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (3, 30.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (4, 40.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (5, NULL, 10000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (6, NULL, 20000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (7, NULL, 30000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (8, NULL, 40000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (9, NULL, NULL, 15000, 3);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (10,NULL, NULL, 25000, 2);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (11, NULL, NULL, 20000, 6);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (12, NULL, NULL, 14000, 4);  
  
SELECT CAST(COALESCE(hourly_wage * 40 * 52,   
   salary,   
   commission * num_sales) AS DECIMAL(10,2)) AS TotalSalary   
FROM dbo.wages  
ORDER BY TotalSalary;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Total Salary  
------------  
10000.00  
20000.00  
20800.00  
30000.00  
40000.00  
41600.00  
45000.00  
50000.00  
56000.00  
62400.00  
83200.00  
120000.00
```  
  
## <a name="see-also"></a>另请参阅  
[ISNULL (Transact-SQL)](../../t-sql/functions/isnull-transact-sql.md)   
[CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)  
  
  
