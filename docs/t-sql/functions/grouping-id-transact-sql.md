---
description: GROUPING_ID (Transact-SQL)
title: GROUPING_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GROUPING_ID_TSQL
- GROUPING_ID
dev_langs:
- TSQL
helpviewer_keywords:
- GROUP BY clause, GROUPING_ID
- GROUPING_ID function
ms.assetid: c1050658-b19f-42ee-9a05-ecd6a73b896c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 828a4c0e33722a22aae4779c7197a7630578a438
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338026"
---
# <a name="grouping_id-transact-sql"></a>GROUPING_ID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  这是计算分组级别的函数。 指定 GROUP BY 时，GROUPING_ID 只能在 SELECT \<select> 列表、HAVING 或 ORDER BY 子句中使用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
GROUPING_ID ( <column_expression>[ ,...n ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 \<column_expression>  
 是 [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md) 子句中的 column_expression。  
  
## <a name="return-type"></a>返回类型  
 **int**  
  
## <a name="remarks"></a>备注  
 GROUPING_ID \<column_expression> 必须与 GROUP BY 列表中的表达式完全匹配。 例如，如果按 DATEPART (yyyy, \<*column name*>) 分组，则使用 GROUPING_ID (DATEPART (yyyy, \<*column name*>))；或者，如果按 \<*column name*> 分组，则使用 GROUPING_ID (\<*column name*>)。  
  
## <a name="comparing-grouping_id--to-grouping-"></a>比较 GROUPING_ID () 与 GROUPING ()  
 GROUPING_ID (\<column_expression> [ **,** ...*n* ]) 将 GROUPING (\<column_expression>) 在每个输出行中为其列列表中的每个列返回的对应值作为 0 和 1 的字符串输入。 GROUPING_ID 将该字符串解释为二进制数并返回对应的整数。 例如，请考虑以下语句：`SELECT a, b, c, SUM(d),``GROUPING_ID(a,b,c)``FROM T GROUP BY <group by list>`。 下表显示了 GROUPING_ID () 的输入值和输出值。  
  
|聚合的列|GROUPING_ID (a, b, c) 输入 = GROUPING(a) + GROUPING(b) + GROUPING(c)|GROUPING_ID () 输出|  
|------------------------|---------------------------------------------------------------------------------------|------------------------------|  
|`a`|`100`|`4`|  
|`b`|`010`|`2`|  
|`c`|`001`|`1`|  
|`ab`|`110`|`6`|  
|`ac`|`101`|`5`|  
|`bc`|`011`|`3`|  
|`abc`|`111`|`7`|  
  
## <a name="technical-definition-of-grouping_id-"></a>GROUPING_ID () 的技术定义  
 每个 GROUPING_ID 参数都必须是 GROUP BY 列表的一个元素。 GROUPING_ID () 返回一个 integer 位图，其最低 N 位可能为文字。 文字 bit 表明对应参数不是给定输出行的分组列。 最低顺序 bit 对应于参数 N，第 N-1 个最低顺序 bit 对应于参数 1<sup></sup>。  
  
## <a name="grouping_id--equivalents"></a>GROUPING_ID () 等效项  
 对于单个分组查询，GROUPING (\<column_expression>) 与 GROUPING_ID (\<column_expression>) 等价，两者均返回 0。  
  
 例如，以下语句是等价的：  
  
 语句 A：  
  
```sql  
SELECT GROUPING_ID(A,B)  
FROM T   
GROUP BY CUBE(A,B)   
```  
  
 语句 B：  
  
```sql  
SELECT 3 FROM T GROUP BY ()  
UNION ALL  
SELECT 1 FROM T GROUP BY A  
UNION ALL  
SELECT 2 FROM T GROUP BY B  
UNION ALL  
SELECT 0 FROM T GROUP BY A,B  
```  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-grouping_id-to-identify-grouping-levels"></a>A. 使用 GROUPING_ID 标识分组级别  
 下面的示例返回按 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `Name` 和 `Title` 汇总的雇员计数以及 `Name,` 和公司总计。 `GROUPING_ID()` 用于为 `Title` 列中的每行创建一个值以标识聚合级别。  
  
```sql  
SELECT D.Name  
    ,CASE   
    WHEN GROUPING_ID(D.Name, E.JobTitle) = 0 THEN E.JobTitle  
    WHEN GROUPING_ID(D.Name, E.JobTitle) = 1 THEN N'Total: ' + D.Name   
    WHEN GROUPING_ID(D.Name, E.JobTitle) = 3 THEN N'Company Total:'  
        ELSE N'Unknown'  
    END AS N'Job Title'  
    ,COUNT(E.BusinessEntityID) AS N'Employee Count'  
FROM HumanResources.Employee E  
    INNER JOIN HumanResources.EmployeeDepartmentHistory DH  
        ON E.BusinessEntityID = DH.BusinessEntityID  
    INNER JOIN HumanResources.Department D  
        ON D.DepartmentID = DH.DepartmentID       
WHERE DH.EndDate IS NULL  
    AND D.DepartmentID IN (12,14)  
GROUP BY ROLLUP(D.Name, E.JobTitle);  
```  
  
### <a name="b-using-grouping_id-to-filter-a-result-set"></a>B. 使用 GROUPING_ID 筛选结果集  
  
#### <a name="simple-example"></a>简单示例  
 在以下代码中，若要只返回包含按职务汇总的雇员计数的行，请在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中删除 `HAVING GROUPING_ID(D.Name, E.JobTitle); = 0` 中的注释字符。 若要只返回包含按部门汇总的雇员计数的行，请删除 `HAVING GROUPING_ID(D.Name, E.JobTitle) = 1;` 中的注释字符。  
  
```sql  
SELECT D.Name  
    ,E.JobTitle  
    ,GROUPING_ID(D.Name, E.JobTitle) AS 'Grouping Level'  
    ,COUNT(E.BusinessEntityID) AS N'Employee Count'  
FROM HumanResources.Employee AS E  
    INNER JOIN HumanResources.EmployeeDepartmentHistory AS DH  
        ON E.BusinessEntityID = DH.BusinessEntityID  
    INNER JOIN HumanResources.Department AS D  
        ON D.DepartmentID = DH.DepartmentID       
WHERE DH.EndDate IS NULL  
    AND D.DepartmentID IN (12,14)  
GROUP BY ROLLUP(D.Name, E.JobTitle)  
--HAVING GROUPING_ID(D.Name, E.JobTitle) = 0; --All titles  
--HAVING GROUPING_ID(D.Name, E.JobTitle) = 1; --Group by Name;  
```  
  
 下面是未筛选的结果集。  
  
|名称|标题|Grouping Level|Employee Count|名称|  
|----------|-----------|--------------------|--------------------|----------|  
|Document Control|Control Specialist|0|2|Document Control|  
|Document Control|Document Control Assistant|0|2|Document Control|  
|Document Control|Document Control Manager|0|1|Document Control|  
|Document Control|Null|1|5|Document Control|  
|Facilities and Maintenance|Facilities Administrative Assistant|0|1|Facilities and Maintenance|  
|Facilities and Maintenance|Facilities Manager|0|1|Facilities and Maintenance|  
|Facilities and Maintenance|Janitor|0|4|Facilities and Maintenance|  
|Facilities and Maintenance|Maintenance Supervisor|0|1|Facilities and Maintenance|  
|Facilities and Maintenance|Null|1|7|Facilities and Maintenance|  
|Null|Null|3|12|Null|  
  
#### <a name="complex-example"></a>复杂示例  
 以下示例使用 `GROUPING_ID()` 按分组级别筛选包含多个分组级别的结果集。 可以使用类似的代码创建一个具有多个分组级别的视图和调用该视图（通过传递一个按分组级别筛选该视图的参数）的存储过程。 该示例使用 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库。  
  
```sql  
DECLARE @Grouping NVARCHAR(50);  
DECLARE @GroupingLevel SMALLINT;  
SET @Grouping = N'CountryRegionCode Total';  
  
SELECT @GroupingLevel = (  
    CASE @Grouping  
        WHEN N'Grand Total'             THEN 15  
        WHEN N'SalesPerson Total'       THEN 14  
        WHEN N'Store Total'             THEN 13  
        WHEN N'Store SalesPerson Total' THEN 12  
        WHEN N'CountryRegionCode Total' THEN 11  
        WHEN N'Group Total'             THEN 7  
        ELSE N'Unknown'  
    END);  
  
SELECT   
    T.[Group]  
    ,T.CountryRegionCode  
    ,S.Name AS N'Store'  
    ,(SELECT P.FirstName + ' ' + P.LastName   
        FROM Person.Person AS P   
        WHERE P.BusinessEntityID = H.SalesPersonID)  
        AS N'Sales Person'  
    ,SUM(TotalDue)AS N'TotalSold'  
    ,CAST(GROUPING(T.[Group])AS char(1)) +   
        CAST(GROUPING(T.CountryRegionCode)AS char(1)) +   
        CAST(GROUPING(S.Name)AS char(1)) +   
        CAST(GROUPING(H.SalesPersonID)AS char(1))   
        AS N'GROUPING base-2'  
    ,GROUPING_ID((T.[Group])  
        ,(T.CountryRegionCode),(S.Name),(H.SalesPersonID)  
        ) AS N'GROUPING_ID'  
    ,CASE   
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 15 THEN N'Grand Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 14 THEN N'SalesPerson Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 13 THEN N'Store Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 12 THEN N'Store SalesPerson Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 11 THEN N'CountryRegionCode Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) =  7 THEN N'Group Total'  
        ELSE N'Error'  
        END AS N'Level'  
FROM Sales.Customer AS C  
    INNER JOIN Sales.Store AS S  
        ON C.StoreID  = S.BusinessEntityID   
    INNER JOIN Sales.SalesTerritory AS T  
        ON C.TerritoryID  = T.TerritoryID   
    INNER JOIN Sales.SalesOrderHeader AS H  
        ON C.CustomerID = H.CustomerID  
GROUP BY GROUPING SETS ((S.Name,H.SalesPersonID)  
    ,(H.SalesPersonID),(S.Name)  
    ,(T.[Group]),(T.CountryRegionCode),()  
    )  
HAVING GROUPING_ID(  
    (T.[Group]),(T.CountryRegionCode),(S.Name),(H.SalesPersonID)  
    ) = @GroupingLevel  
ORDER BY   
    GROUPING_ID(S.Name,H.SalesPersonID),GROUPING_ID((T.[Group])  
    ,(T.CountryRegionCode)  
    ,(S.Name)  
    ,(H.SalesPersonID))ASC;  
```  
  
### <a name="c-using-grouping_id--with-rollup-and-cube-to-identify-grouping-levels"></a>C. 将 GROUPING_ID () 与 ROLLUP 和 CUBE 一起使用以标识分组级别  
 下面示例中的代码演示如何使用 `GROUPING()` 来计算 `Bit Vector(base-2)` 列。 `GROUPING_ID()` 用于计算对应的 `Integer Equivalent` 列。 `GROUPING_ID()` 函数中的列顺序与 `GROUPING()` 函数所连接的列的列顺序相反。  
  
 在这些示例中，`GROUPING_ID()` 用于为 `Grouping Level` 列中的每行创建一个值以标识分组级别。 分组级别并不总是从 1 开始的连续整数列表（0、1、2、...n）。  
  
> [!NOTE]  
>  可以在 HAVING 子句中使用 GROUPING 和 GROUPING_ID 来筛选结果集。  
  
#### <a name="rollup-example"></a>ROLLUP 示例  
 在此示例中，不会像在下面的 CUBE 示例中那样显示所有分组级别。 如果更改 `ROLLUP` 列表中列的顺序，则也必须更改 `Grouping Level` 列中的级别值。 该示例使用 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库。  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,DATEPART(mm,OrderDate) AS N'Month'  
    ,DATEPART(dd,OrderDate) AS N'Day'  
    ,SUM(TotalDue) AS N'Total Due'  
    ,CAST(GROUPING(DATEPART(dd,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(mm,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(yyyy,OrderDate)) AS CHAR(1))   
     AS N'Bit Vector(base-2)'  
    ,GROUPING_ID(DATEPART(yyyy,OrderDate)  
        ,DATEPART(mm,OrderDate)  
        ,DATEPART(dd,OrderDate))   
        AS N'Integer Equivalent'  
    ,CASE  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 0 THEN N'Year Month Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 1 THEN N'Year Month'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 2 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 3 THEN N'Year'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 4 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 5 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 6 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 7 THEN N'Grand Total'  
    ELSE N'Error'  
    END AS N'Grouping Level'  
FROM Sales.SalesOrderHeader  
WHERE DATEPART(yyyy,OrderDate) IN(N'2007',N'2008')  
    AND DATEPART(mm,OrderDate) IN(1,2)  
    AND DATEPART(dd,OrderDate) IN(1,2)  
GROUP BY ROLLUP(DATEPART(yyyy,OrderDate)  
        ,DATEPART(mm,OrderDate)  
        ,DATEPART(dd,OrderDate))  
ORDER BY GROUPING_ID(DATEPART(mm,OrderDate)  
    ,DATEPART(yyyy,OrderDate)  
    ,DATEPART(dd,OrderDate)  
    )  
    ,DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate);  
```  
  
 以下为部分结果集。  
  
|年龄|月份|日期|Total Due|Bit Vector (base-2)|Integer Equivalent|Grouping Level|  
|----------|-----------|---------|---------------|----------------------------|------------------------|--------------------|  
|2007|1|1|1497452.6066|000|0|Year Month Day|  
|2007|1|2|21772.3494|000|0|Year Month Day|  
|2007|2|1|2705653.5913|000|0|Year Month Day|  
|2007|2|2|21684.4068|000|0|Year Month Day|  
|2008|1|1|1908122.0967|000|0|Year Month Day|  
|2008|1|2|46458.0691|000|0|Year Month Day|  
|2008|2|1|3108771.9729|000|0|Year Month Day|  
|2008|2|2|54598.5488|000|0|Year Month Day|  
|2007|1|Null|1519224.956|100|1|Year Month|  
|2007|2|Null|2727337.9981|100|1|Year Month|  
|2008|1|Null|1954580.1658|100|1|Year Month|  
|2008|2|Null|3163370.5217|100|1|Year Month|  
|2007|Null|Null|4246562.9541|110|3|年龄|  
|2008|Null|Null|5117950.6875|110|3|年龄|  
|Null|Null|Null|9364513.6416|111|7|总计|  
  
#### <a name="cube-example"></a>CUBE 示例  
 在此示例中，`GROUPING_ID()` 函数用于为 `Grouping Level` 列中的每行创建一个值以标识分组级别。  
  
 与上例中的 `ROLLUP` 不同，`CUBE` 会输出所有分组级别。 如果更改 `CUBE` 列表中列的顺序，则也必须更改 `Grouping Level` 列中的级别值。 该示例使用 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,DATEPART(mm,OrderDate) AS N'Month'  
    ,DATEPART(dd,OrderDate) AS N'Day'  
    ,SUM(TotalDue) AS N'Total Due'  
    ,CAST(GROUPING(DATEPART(dd,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(mm,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(yyyy,OrderDate)) AS CHAR(1))   
        AS N'Bit Vector(base-2)'  
    ,GROUPING_ID(DATEPART(yyyy,OrderDate)  
        ,DATEPART(mm,OrderDate)  
        ,DATEPART(dd,OrderDate))   
        AS N'Integer Equivalent'  
    ,CASE  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 0 THEN N'Year Month Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 1 THEN N'Year Month'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 2 THEN N'Year Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 3 THEN N'Year'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 4 THEN N'Month Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 5 THEN N'Month'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 6 THEN N'Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 7 THEN N'Grand Total'  
    ELSE N'Error'  
    END AS N'Grouping Level'  
FROM Sales.SalesOrderHeader  
WHERE DATEPART(yyyy,OrderDate) IN(N'2007',N'2008')  
    AND DATEPART(mm,OrderDate) IN(1,2)  
    AND DATEPART(dd,OrderDate) IN(1,2)  
GROUP BY CUBE(DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate))  
ORDER BY GROUPING_ID(DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate)  
    )  
    ,DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate);  
```  
  
 以下为部分结果集。  
  
|年龄|月份|日期|Total Due|Bit Vector (base-2)|Integer Equivalent|Grouping Level|  
|----------|-----------|---------|---------------|----------------------------|------------------------|--------------------|  
|2007|1|1|1497452.6066|000|0|Year Month Day|  
|2007|1|2|21772.3494|000|0|Year Month Day|  
|2007|2|1|2705653.5913|000|0|Year Month Day|  
|2007|2|2|21684.4068|000|0|Year Month Day|  
|2008|1|1|1908122.0967|000|0|Year Month Day|  
|2008|1|2|46458.0691|000|0|Year Month Day|  
|2008|2|1|3108771.9729|000|0|Year Month Day|  
|2008|2|2|54598.5488|000|0|Year Month Day|  
|2007|1|Null|1519224.956|100|1|Year Month|  
|2007|2|Null|2727337.9981|100|1|Year Month|  
|2008|1|Null|1954580.1658|100|1|Year Month|  
|2008|2|Null|3163370.5217|100|1|Year Month|  
|2007|Null|1|4203106.1979|010|2|Year Day|  
|2007|Null|2|43456.7562|010|2|Year Day|  
|2008|Null|1|5016894.0696|010|2|Year Day|  
|2008|Null|2|101056.6179|010|2|Year Day|  
|2007|Null|Null|4246562.9541|110|3|年龄|  
|2008|Null|Null|5117950.6875|110|3|年龄|  
|Null|1|1|3405574.7033|001|4|Month Day|  
|Null|1|2|68230.4185|001|4|Month Day|  
|Null|2|1|5814425.5642|001|4|Month Day|  
|Null|2|2|76282.9556|001|4|Month Day|  
|Null|1|Null|3473805.1218|101|5|月份|  
|Null|2|Null|5890708.5198|101|5|月份|  
|Null|Null|1|9220000.2675|011|6|日期|  
|Null|Null|2|144513.3741|011|6|日期|  
|Null|Null|Null|9364513.6416|111|7|总计|  
  
## <a name="see-also"></a>另请参阅  
 [GROUPING (Transact-SQL)](../../t-sql/functions/grouping-transact-sql.md)   
 [GROUP BY (Transact-SQL)](../../t-sql/queries/select-group-by-transact-sql.md)  
  
  
