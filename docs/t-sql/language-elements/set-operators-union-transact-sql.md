---
description: 集运算符 - UNION (Transact-SQL)
title: UNION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- UNION
- UNION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- UNION queries
- combining query results
- UNION operator [SQL Server]
ms.assetid: 607c296f-8a6a-49bc-975a-b8d0c0914df7
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eee4538e96bdc4452091daf1a78302d56aedc09d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466068"
---
# <a name="set-operators---union-transact-sql"></a>集运算符 - UNION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

将两个查询的结果连接到一个结果集中。 可控制结果集是否包含重复行：

- **UNION ALL** - 包括重复行。
- **UNION** - 排除重复行。

UNION 操作不同于 [JOIN](../queries/from-transact-sql.md) 操作 ：

- UNION 连接两个查询中的结果集。 但 UNION 不会从两个表收集的列中创建单独的行。
- JOIN 比较两个表中的列，以创建由两个表中的列组成的结果行。
  
下面列出了使用 UNION 合并两个查询结果集的基本规则：  
  
-   所有查询中的列数和列的顺序必须相同。  
  
-   数据类型必须兼容。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
{ <query_specification> | ( <query_expression> ) }   
{ UNION [ ALL ]   
  { <query_specification> | ( <query_expression> ) } 
  [ ...n ] }
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
\<query_specification> | ( \<query_expression> )：查询规范或查询表达式，用于返回要与另一个查询规范或查询表达式所返回的数据合并的数据。 属于 UNION 运算的列定义不一定要相同，但必须可通过隐式转换实现兼容。 如果数据类型不同，则根据[数据类型优先级](../../t-sql/data-types/data-type-precedence-transact-sql.md)规则确定所产生的数据类型。 如果类型相同，但精度、确定位数或长度不同，那么结果以相同的表达式合并规则为依据。 有关详细信息，请参阅[精度、小数位数和长度 (Transact-SQL)](../../t-sql/data-types/precision-scale-and-length-transact-sql.md)。  
  
xml 数据类型的列必须相等。 所有的列必须类型化为 XML 架构或是非类型化的。 如果要类型化，这些列必须类型化为相同的 XML 架构集合。  
  
UNION  
指定合并多个结果集并将其作为单个结果集返回。  
  
ALL  
将全部行都纳入结果，包括重复行。 如果未指定该参数，则删除重复行。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-a-simple-union"></a>A. 使用简单 UNION  
在下面的示例中，结果集同时包含 `ProductModelID` 和 `Name` 表中的 `ProductModel` 与 `Gloves` 列中的内容。  
 
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
-- Here is the simple union.  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves  
ORDER BY Name;  
GO  
```  
  
### <a name="b-using-select-into-with-union"></a>B. 将 SELECT INTO 与 UNION 一起使用  
在下面的示例中，第二个 `SELECT` 语句中的 `INTO` 子句指定，`ProductResults` 表保留 `ProductModel` 和 `Gloves` 表中选定列的并集的最终结果集。 `Gloves` 表是在第一个 `SELECT` 语句中创建。  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.ProductResults', 'U') IS NOT NULL  
DROP TABLE dbo.ProductResults;  
GO  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
INTO dbo.ProductResults  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves;  
GO  
  
SELECT ProductModelID, Name   
FROM dbo.ProductResults;  
```  
  
### <a name="c-using-union-of-two-select-statements-with-order-by"></a>C. 将 ORDER BY 与两个 SELECT 语句的 UNION 一起使用  
在 UNION 子句中使用的某些参数的顺序非常重要。 下面的示例通过两个 `UNION` 语句说明 `SELECT` 的错误用法和正确用法（在这两个语句的输出中将重命名一个列）。  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
/* INCORRECT */  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
ORDER BY Name  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves;  
GO  
  
/* CORRECT */  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves  
ORDER BY Name;  
GO  
```  
  
### <a name="d-using-union-of-three-select-statements-to-show-the-effects-of-all-and-parentheses"></a>D. 使用三个 SELECT 语句的 UNION 来说明 ALL 和括号的作用  
下列示例使用 `UNION` 来组合具有相同 5 行数据的三个表的结果。 第一个示例使用 `UNION ALL` 显示重复记录，返回所有的 15 行。 第二个示例使用不带 `UNION` 的 `ALL`，删除三个 `SELECT` 语句的组合结果中的重复行，返回 5 行。  
  
第三个示例将 `ALL` 用于第一个 `UNION`，并用括号将没有使用 `ALL` 的第二个 `UNION` 括起来。 第二个 `UNION` 因位于括号内而首先得到处理，并返回 5 行，这是因为未使用 `ALL` 选项，重复行遭删除。 通过使用 `SELECT` 关键字将这 5 行与第一个 `UNION ALL` 的结果组合在一起。 下面的示例不删除两个 5 行结果集之间的重复行。 最终结果有 10 行。  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.EmployeeOne', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeOne;  
GO  
IF OBJECT_ID ('dbo.EmployeeTwo', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeTwo;  
GO  
IF OBJECT_ID ('dbo.EmployeeThree', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeThree;  
GO  
  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeOne  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeTwo  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeThree  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
-- Union ALL  
SELECT LastName, FirstName, JobTitle  
FROM dbo.EmployeeOne  
UNION ALL  
SELECT LastName, FirstName ,JobTitle  
FROM dbo.EmployeeTwo  
UNION ALL  
SELECT LastName, FirstName,JobTitle   
FROM dbo.EmployeeThree;  
GO  
  
SELECT LastName, FirstName,JobTitle  
FROM dbo.EmployeeOne  
UNION   
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeTwo  
UNION   
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeThree;  
GO  
  
SELECT LastName, FirstName,JobTitle   
FROM dbo.EmployeeOne  
UNION ALL  
(  
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeTwo  
UNION  
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeThree  
);  
GO  
  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-a-simple-union"></a>E. 使用简单 UNION  
在下面的示例中，结果集同时包含 `FactInternetSales` 和 `DimCustomer` 表中的 `CustomerKey` 列中的内容。 由于未使用 ALL 关键字，因此重复行会从结果中排除。  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="f-using-union-of-two-select-statements-with-order-by"></a>F. 将 ORDER BY 与两个 SELECT 语句的 UNION 一起使用  
 当 UNION 语句中的任何 SELECT 语句包含 ORDER BY 子句时，该子句应置于所有 SELECT 语句之后。 下面的示例通过两个 `SELECT` 语句说明 `UNION` 的错误用法和正确用法（在这两个语句中使用 ORDER BY 对一个列排序）。  
  
```sql
-- Uses AdventureWorks  
  
-- INCORRECT  
SELECT CustomerKey   
FROM FactInternetSales    
ORDER BY CustomerKey  
UNION   
SELECT CustomerKey   
FROM DimCustomer  
ORDER BY CustomerKey;  
  
-- CORRECT   
USE AdventureWorksPDW2012;  
  
SELECT CustomerKey   
FROM FactInternetSales    
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="g-using-union-of-two-select-statements-with-where-and-order-by"></a>G. 将 WHERE 和 ORDER BY 与两个 SELECT 语句的 UNION 一起使用  
下面的示例通过两个 `SELECT` 语句说明 `UNION` 的错误用法和正确用法（在这两个语句中需要 WHERE 和 ORDER BY）。  
  
```sql
-- Uses AdventureWorks  
  
-- INCORRECT   
SELECT CustomerKey   
FROM FactInternetSales   
WHERE CustomerKey >= 11000  
ORDER BY CustomerKey   
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
  
-- CORRECT  
USE AdventureWorksPDW2012;  
  
SELECT CustomerKey   
FROM FactInternetSales   
WHERE CustomerKey >= 11000  
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="h-using-union-of-three-select-statements-to-show-effects-of-all-and-parentheses"></a>H. 使用三个 SELECT 语句的 UNION 来说明 ALL 和括号的作用  
下面的示例使用 `UNION` 合并同一个表的结果，以展示使用 `UNION` 时 ALL 和括号的效果。  
  
第一个示例使用 `UNION ALL` 显示重复记录，并三次返回源表中的每一行。 第二个示例使用不带 `ALL` 的 `UNION`，删除三个 `SELECT` 语句的组合结果中的重复行，仅返回源表中的非重复行。  
  
第三个示例将 `ALL` 用于第一个 `UNION`，并用括号将没有使用 `ALL` 的第二个 `UNION` 括起来。 首先处理第二个 `UNION`，因为它位于括号中。 仅从表返回非重复行，因为未使用 `ALL` 选项，重复行遭删除。 通过使用 `UNION ALL` 关键字将这些行与第一个 `SELECT` 的结果合并在一起。 下面的示例不删除两个结果集之间的重复行。  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer;  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer;  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL  
(  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
);  
```  
  
## <a name="see-also"></a>另请参阅  
[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
[SELECT 示例 (Transact-SQL)](../../t-sql/queries/select-examples-transact-sql.md)  
