---
description: NTILE (Transact-SQL)
title: NTILE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NTILE_TSQL
- NTILE
dev_langs:
- TSQL
helpviewer_keywords:
- distributing rows
- groups [SQL Server], row distribution
- row distribution [SQL Server]
- NTILE function
ms.assetid: 1c364511-d72a-4789-8efa-3cf2a1f6b791
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d0386795164484da0932ff7b6963c68c6eaf414a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189945"
---
# <a name="ntile-transact-sql"></a>NTILE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  将有序分区中的行分发到指定数目的组中。 各个组有编号，编号从一开始。 对于每一个行，NTILE 将返回此行所属的组的编号。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
NTILE (integer_expression) OVER ( [ <partition_by_clause> ] < order_by_clause > )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *integer_expression*  
 一个正整数表达式，用于指定每个分区必须被划分成的组数。 integer_expression 可以是 int 或 bigint 类型 。  
  
 \<partition_by_clause>  
 将由 [FROM](../../t-sql/queries/from-transact-sql.md) 子句生成的结果集划分为要应用函数的分区。 有关 PARTITION BY 语法的信息，请参阅 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)。  
  
 \<order_by_clause>  
 确定 NTILE 值分配到分区中各行的顺序。 在排名函数中使用 \<order_by_clause> 时，不能用整数表示列。  
  
## <a name="return-types"></a>返回类型  
 **bigint**  
  
## <a name="remarks"></a>备注  
 如果分区的行数不能被 integer_expression 整除，则将导致一个成员有两种大小不同的组。 按照 OVER 子句指定的顺序，较大的组排在较小的组前面。 例如，如果总行数是 53，组数是 5，则前三个组每组包含 11 行，其余两个组每组包含 10 行。 另一方面，如果总行数可被组数整除，则行数将在组之间平均分布。 例如，如果总行数为 50，有五个组，则每组将包含 10 行。  
  
 NTILE 具有不确定性。 有关详细信息，请参阅 [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-dividing-rows-into-groups"></a>A. 将行分为组  
 下面的示例根据员工的年初至今销售额将行分到四个员工组中。 由于总行数不能被组数整除，因此前两个组将包含四行，而其余各组包含三行。  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT p.FirstName, p.LastName  
    ,NTILE(4) OVER(ORDER BY SalesYTD DESC) AS Quartile  
    ,CONVERT(NVARCHAR(20),s.SalesYTD,1) AS SalesYTD  
    , a.PostalCode  
FROM Sales.SalesPerson AS s   
INNER JOIN Person.Person AS p   
    ON s.BusinessEntityID = p.BusinessEntityID  
INNER JOIN Person.Address AS a   
    ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
FirstName      LastName              Quartile  SalesYTD       PostalCode  
-------------  --------------------- --------- -------------- ----------  
Linda          Mitchell              1         4,251,368.55   98027  
Jae            Pak                   1         4,116,871.23   98055  
Michael        Blythe                1         3,763,178.18   98027  
Jillian        Carson                1         3,189,418.37   98027  
Ranjit         Varkey Chudukatil     2         3,121,616.32   98055  
José           Saraiva               2         2,604,540.72   98055  
Shu            Ito                   2         2,458,535.62   98055  
Tsvi           Reiter                2         2,315,185.61   98027  
Rachel         Valdez                3         1,827,066.71   98055  
Tete           Mensa-Annan           3         1,576,562.20   98055  
David          Campbell              3         1,573,012.94   98055  
Garrett        Vargas                4         1,453,719.47   98027  
Lynn           Tsoflias              4         1,421,810.92   98055  
Pamela         Ansman-Wolfe          4         1,352,577.13   98027  

(14 row(s) affected)  
```  
  
### <a name="b-dividing-the-result-set-by-using-partition-by"></a>B. 使用 PARTITION BY 划分结果集  
 以下示例将 `PARTITION BY` 参数添加到示例 A 中的代码。首先按 `PostalCode` 将行分区，然后在每个 `PostalCode` 内将行分成四个组。 该示例还声明一个变量 `@NTILE_Var` 并使用该变量指定 integer_expression 参数的值。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @NTILE_Var INT = 4;  
  
SELECT p.FirstName, p.LastName  
    ,NTILE(@NTILE_Var) OVER(PARTITION BY PostalCode ORDER BY SalesYTD DESC) AS Quartile  
    ,CONVERT(NVARCHAR(20),s.SalesYTD,1) AS SalesYTD  
    ,a.PostalCode  
FROM Sales.SalesPerson AS s   
INNER JOIN Person.Person AS p   
    ON s.BusinessEntityID = p.BusinessEntityID  
INNER JOIN Person.Address AS a   
    ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
FirstName    LastName             Quartile SalesYTD      PostalCode  
------------ -------------------- -------- ------------  ----------  
Linda        Mitchell             1        4,251,368.55  98027  
Michael      Blythe               1        3,763,178.18  98027  
Jillian      Carson               2        3,189,418.37  98027  
Tsvi         Reiter               2        2,315,185.61  98027  
Garrett      Vargas               3        1,453,719.47  98027  
Pamela       Ansman-Wolfe         4        1,352,577.13  98027  
Jae          Pak                  1        4,116,871.23  98055  
Ranjit       Varkey Chudukatil    1        3,121,616.32  98055  
José         Saraiva              2        2,604,540.72  98055  
Shu          Ito                  2        2,458,535.62  98055  
Rachel       Valdez               3        1,827,066.71  98055  
Tete         Mensa-Annan          3        1,576,562.20  98055  
David        Campbell             4        1,573,012.94  98055  
Lynn         Tsoflias             4        1,421,810.92  98055  
  
(14 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-dividing-rows-into-groups"></a>C. 将行分为组  
 以下示例使用 NTILE 函数根据其 2003 年的分配销售配额将一组销售人员划分为四个组。 由于总行数不能被组数整除，因此第一个组将包含五行，其余每组包含四行。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.LastName, NTILE(4) OVER(ORDER BY SUM(SalesAmountQuota) DESC) AS Quartile,  
       CONVERT (VARCHAR(13), SUM(SalesAmountQuota), 1) AS SalesQuota  
FROM dbo.DimEmployee AS e   
INNER JOIN dbo.FactSalesQuota AS sq   
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE sq.CalendarYear = 2003  
    AND SalesTerritoryKey IS NOT NULL AND SalesAmountQuota <> 0  
GROUP BY e.LastName  
ORDER BY Quartile, e.LastName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName          Quartile SalesYTD  
----------------- -------- ------------`  
Blythe            1        4,716,000.00  
Carson            1        4,350,000.00  
Mitchell          1        4,682,000.00  
Pak               1        5,142,000.00  
Varkey Chudukatil 1        2,940,000.00  
Ito               2        2,644,000.00  
Saraiva           2        2,293,000.00  
Vargas            2        1,617,000.00  
Ansman-Wolfe      3        1,183,000.00  
Campbell          3        1,438,000.00  
Mensa-Annan       3        1,481,000.00  
Valdez            3        1,294,000.00  
Abbas             4          172,000.00  
Albert            4          651,000.00  
Jiang             4          544,000.00  
Tsoflias          4          867,000.00
```  
  
### <a name="d-dividing-the-result-set-by-using-partition-by"></a>D. 使用 PARTITION BY 划分结果集  
 以下示例将 PARTITION BY 参数添加到示例 A 中的代码。首先按 `SalesTerritoryCountry` 将行分区，然后在每个 `SalesTerritoryCountry` 内将行分成两个组。 请注意，OVER 子句中的 ORDER BY 对 NTILE 进行排序，SELECT 中的 ORDER BY 则对结果集进行排序。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.LastName, NTILE(2) OVER(PARTITION BY e.SalesTerritoryKey ORDER BY SUM(SalesAmountQuota) DESC) AS Quartile,  
       CONVERT (VARCHAR(13), SUM(SalesAmountQuota), 1) AS SalesQuota  
   ,st.SalesTerritoryCountry  
FROM dbo.DimEmployee AS e   
INNER JOIN dbo.FactSalesQuota AS sq   
    ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st  
    ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE sq.CalendarYear = 2003  
GROUP BY e.LastName,e.SalesTerritoryKey,st.SalesTerritoryCountry  
ORDER BY st.SalesTerritoryCountry, Quartile;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName          Quartile SalesYTD       SalesTerritoryCountry  
----------------- -------- -------------- ------------------  
Tsoflias          1         867,000.00     Australia  
Saraiva           1        2,293,000.00    Canada  
Varkey Chudukatil 1        2,940,000.00    France  
Valdez            1        1,294,000.00    Germany  
Alberts           1          651,000.00    NA  
Jiang             1          544,000.00    NA  
Pak               1        5,142,000.00    United Kingdom  
Mensa-Annan       1        1,481,000.00    United States  
Campbell          1        1,438,000.00    United States  
Reiter            1        2,768,000.00    United States  
Blythe            1        4,716,000.00    United States  
Carson            1        4,350,000.00     United States  
Mitchell          1        4,682,000.00     United States  
Vargas            2        1,617,000.00     Canada  
Abbas             2          172,000.00     NA  
Ito               2        2,644,000.00     United States  
Ansman-Wolfe      2        1,183,000.00     United States
```  
  
## <a name="see-also"></a>另请参阅  
 [RANK (Transact-SQL)](../../t-sql/functions/rank-transact-sql.md)   
 [DENSE_RANK (Transact-SQL)](../../t-sql/functions/dense-rank-transact-sql.md)   
 [ROW_NUMBER (Transact-SQL)](../../t-sql/functions/row-number-transact-sql.md)   
 [排名函数 (Transact-SQL)](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)  
  
  


