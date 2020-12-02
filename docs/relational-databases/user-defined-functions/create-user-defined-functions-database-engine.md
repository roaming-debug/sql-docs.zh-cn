---
description: 创建用户定义函数（数据库引擎）
title: 创建用户定义函数（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 11/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- SCHEMABINDING clause
- schema-bound functions [SQL Server]
- user-defined functions [SQL Server], creating
- CREATE FUNCTION statement
- valid statements [SQL Server]
- UDF
- TVF
ms.assetid: f0d5dd10-73fd-4e05-9177-07f56552bdf7
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9739c1c5476b189cce41d14190165c9b82587f93
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130834"
---
# <a name="create-user-defined-functions-database-engine"></a>创建用户定义函数（数据库引擎）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题介绍了如何通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建用户定义函数 (UDF)。  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   用户定义函数不能用于执行修改数据库状态的操作。  
  
-   用户定义函数不能包含将表作为其目标的 `OUTPUT INTO` 子句。  
  
-   用户定义函数不能返回多个结果集。 如果您需要返回多个结果集，请使用存储过程。  
  
-   在用户定义函数中，错误处理受限制。 UDF 不支持 `TRY...CATCH`、`@ERROR` 或 `RAISERROR`。  
  
-   用户定义函数不能调用存储过程，但是可调用扩展存储过程。  
  
-   用户定义函数不能使用动态 SQL 或临时表。 允许表变量。  
  
-   在用户定义函数中不允许 `SET` 语句。  
  
-   不允许使用 `FOR XML` 子句。  
  
-   用户定义函数可以嵌套；也就是说，用户定义函数可相互调用。 被调用函数开始执行时，嵌套级别将增加；被调用函数执行结束后，嵌套级别将减少。 用户定义函数的嵌套级别最多可达 32 级。 如果超出最大嵌套级别数，整个调用函数链将失败。 从 Transact-SQL 用户定义函数对托管代码的任何引用都将根据 32 级嵌套限制计入一个级别。 从托管代码内部调用的方法不根据此限制进行计数。  
  
-   下列 Service Broker 语句不能包含在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 用户定义函数的定义中：  
  
    -   `BEGIN DIALOG CONVERSATION`  
  
    -   `END CONVERSATION`  
  
    -   `GET CONVERSATION GROUP`  
  
    -   `MOVE CONVERSATION`  
  
    -   `RECEIVE`  
  
    -   `SEND`  
  
###  <a name="permissions"></a><a name="Security"></a> 权限 

需要在数据库中具有 `CREATE FUNCTION` 权限，并对创建函数时所在的架构具有 `ALTER` 权限。 如果函数指定用户定义类型，则需要对该类型具有 `EXECUTE` 权限。  
  
##  <a name="scalar-functions"></a><a name="Scalar"></a> 标量函数  
 下面的示例在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中创建一个多语句标量函数（ 标量 UDF）。 此函数输入一个值 `ProductID`，而返回一个单个数据值（指定库存产品的聚合量）。  
  
```sql  
IF OBJECT_ID (N'dbo.ufnGetInventoryStock', N'FN') IS NOT NULL  
    DROP FUNCTION ufnGetInventoryStock;  
GO  
CREATE FUNCTION dbo.ufnGetInventoryStock(@ProductID int)  
RETURNS int   
AS   
-- Returns the stock level for the product.  
BEGIN  
    DECLARE @ret int;  
    SELECT @ret = SUM(p.Quantity)   
    FROM Production.ProductInventory p   
    WHERE p.ProductID = @ProductID   
        AND p.LocationID = '6';  
     IF (@ret IS NULL)   
        SET @ret = 0;  
    RETURN @ret;  
END; 
```  
  
 下例使用 `ufnGetInventoryStock` 函数返回 `ProductModelID` 介于 75 和 80 之间的产品的当前库存量。  
  
```sql  
SELECT ProductModelID, Name, dbo.ufnGetInventoryStock(ProductID)AS CurrentSupply  
FROM Production.Product  
WHERE ProductModelID BETWEEN 75 and 80;  
```  

> [!NOTE]  
> 有关详细信息，请参阅 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)。 

##  <a name="table-valued-functions"></a><a name="TVF"></a> 表值函数  
下面的示例在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中创建内联表值函数 (TVF)。 此函数的输入参数为客户（商店）ID，而返回 `ProductID`、 `Name`以及 `YTD Total` （销售到商店的每种产品的本年度节截止到现在的销售总额）列。  
  
```sql  
IF OBJECT_ID (N'Sales.ufn_SalesByStore', N'IF') IS NOT NULL  
    DROP FUNCTION Sales.ufn_SalesByStore;  
GO  
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'  
    FROM Production.Product AS P   
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID  
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID  
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID  
    WHERE C.StoreID = @storeid  
    GROUP BY P.ProductID, P.Name  
);  
```  
  
下面的示例调用此函数并指定客户 ID 为 602。  
  
```sql  
SELECT * FROM Sales.ufn_SalesByStore (602);  
```  
  
下面的示例在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中创建多语句表值函数 (MSTVF)。 此函数具有一个输入参数 `EmployeeID` ，它返回直接或间接向指定员工报告的所有员工的列表。 然后在指定雇员 ID 109 的情况下调用此函数。  
  
```sql  
IF OBJECT_ID (N'dbo.ufn_FindReports', N'TF') IS NOT NULL  
    DROP FUNCTION dbo.ufn_FindReports;  
GO  
CREATE FUNCTION dbo.ufn_FindReports (@InEmpID INTEGER)  
RETURNS @retFindReports TABLE   
(  
    EmployeeID int primary key NOT NULL,  
    FirstName nvarchar(255) NOT NULL,  
    LastName nvarchar(255) NOT NULL,  
    JobTitle nvarchar(50) NOT NULL,  
    RecursionLevel int NOT NULL  
)  
--Returns a result set that lists all the employees who report to the   
--specific employee directly or indirectly.*/  
AS  
BEGIN  
WITH EMP_cte(EmployeeID, OrganizationNode, FirstName, LastName, JobTitle, RecursionLevel) -- CTE name and columns  
    AS (  
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, 0 -- Get the initial list of Employees for Manager n  
        FROM HumanResources.Employee e   
INNER JOIN Person.Person p   
ON p.BusinessEntityID = e.BusinessEntityID  
        WHERE e.BusinessEntityID = @InEmpID  
        UNION ALL  
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, RecursionLevel + 1 -- Join recursive member to anchor  
        FROM HumanResources.Employee e   
            INNER JOIN EMP_cte  
            ON e.OrganizationNode.GetAncestor(1) = EMP_cte.OrganizationNode  
INNER JOIN Person.Person p   
ON p.BusinessEntityID = e.BusinessEntityID  
        )  
-- copy the required columns to the result of the function   
   INSERT @retFindReports  
   SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel  
   FROM EMP_cte   
   RETURN  
END;  
GO  
```  
  
下面的示例调用此函数并指定员工 ID 为 1。  
  
```sql  
SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel  
FROM dbo.ufn_FindReports(1);  
```  

> [!NOTE]  
> 有关内联表值函数（内联 TVF）和多语句表值函数 (MSTVF) 的详细信息和示例，请参阅 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)。 

## <a name="best-practices"></a>最佳实践  
如果用户定义函数 (UDF) 不是使用 `SCHEMABINDING` 子句创建的，则对基础对象进行的任何更改可能会影响函数定义并在调用函数时可能导致意外结果。 我们建议您实现以下方法之一，以便确保函数不会由于对于其基础对象的更改而过期：  
  
-   创建 UDF 时指定 `WITH SCHEMABINDING` 子句。 这确保除非也修改了函数，否则无法修改在函数定义中引用的对象。  
  
-   在修改在 UDF 定义中指定的任何对象后执行 [sp_refreshsqlmodule](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md) 存储过程。  

如果要创建不访问数据的 UDF，请指定 `SCHEMABINDING` 选项。 这将阻止查询优化器为涉及这些 UDF 的查询计划生成不必要的 spool 运算符。 有关 spool 的详细信息，请参阅 [Showplan 逻辑运算符和物理运算符参考](../../relational-databases/showplan-logical-and-physical-operators-reference.md)。 有关创建架构绑定函数的详细信息，请参阅[架构绑定函数](../../relational-databases/user-defined-functions/user-defined-functions.md#SchemaBound)。

可以在 `FROM` 子句中加入 MSTVF，但是会降低性能。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法对可以加入 MSTVF 的某些语句使用所有优化技术，导致生成的查询计划不佳。 若要获得最佳的性能，尽可能在基表之间使用联接而不是函数。  

> [!IMPORTANT]
> 自 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 起，MSTVF 的固定基数猜测为“100”，而早期 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本为“1”。    
> 从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始，如果要优化使用 MSTVF 的执行计划，可以利用交错执行，这样会使用实际基数而不是上述启发。     
> 有关详细信息，请参阅[多语句表值函数的交错执行](../../relational-databases/performance/intelligent-query-processing.md#interleaved-execution-for-mstvfs)。

> [!NOTE]  
> 在传递存储过程或用户定义函数中的参数时，或在声明和设置批语句中的变量时，不会遵守 ANSI_WARNINGS。 例如，如果将一个变量定义为 char(3)，然后将其值设置为大于三个字符，则数据会被截断为定义的大小，并且 `INSERT` 或 `UPDATE` 语句可以成功执行。

## <a name="see-also"></a>另请参阅  
 [用户定义的函数](../../relational-databases/user-defined-functions/user-defined-functions.md)     
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)    
 [ALTER FUNCTION &#40;Transact-SQL&#41;](../../tools/sql-server-profiler/start-sql-server-profiler.md)    
 [DROP FUNCTION (Transact-SQL)](../../tools/sql-server-profiler/start-sql-server-profiler.md)     
 [DROP PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/drop-partition-function-transact-sql.md)    
 [社区](https://www.bing.com/search?q=user%20defined%20function%20%22sql%20server%202016%22%20examples&qs=n&form=QBRE&pq=user%20defined%20function%20%22sql%20server%202016%22%20examples&sc=0-48&sp=-1&sk=&cvid=C3AD337125A840AD9EEFA3AAC36A3712)中提供了更多示例   
  
