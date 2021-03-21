---
title: ROW_NUMBER (Transact-SQL) | Microsoft Docs
description: ROW_NUMBER 函数的 Transact-SQL 参考。 此函数对结果集的输出进行编号。
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ROW_NUMBER
- ROW_NUMBER_TSQL
- ROW_NUMBER()_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ROW_NUMBER function
- row numbers [SQL Server]
- sequential row numbers [SQL Server]
ms.assetid: 82fa9016-77db-4b42-b4c8-df6095b81906
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 29cd02108b56799e0e23ebf0da26f33df93e28d7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735673"
---
# <a name="row_number-transact-sql"></a>ROW_NUMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

对结果集的输出进行编号。 具体来说，返回结果集分区内行的序列号，每个分区的第一行从 1 开始。 
  
`ROW_NUMBER` 和 `RANK` 类似。 `ROW_NUMBER` 按顺序对所有行进行编号（例如 1、2、3、4、5）。 `RANK` 为关系提供相同的数值（例如 1、2、2、4、5）。   
  
> [!NOTE]
> `ROW_NUMBER` 是运行查询时计算出的临时值。 若要将数值保存在表中，请参阅 [IDENTITY 属性](../../t-sql/statements/create-table-transact-sql-identity-property.md)和 [SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md)。 
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ROW_NUMBER ( )   
    OVER ( [ PARTITION BY value_expression , ... [ n ] ] order_by_clause )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 PARTITION BY value_expression   
 将 [FROM](../../t-sql/queries/from-transact-sql.md) 子句生成的结果集划分为应用 ROW_NUMBER 函数的分区。 value_expression 指定对结果集进行分区所依据的列  。 如果未指定 `PARTITION BY`，则此函数将查询结果集的所有行视为单个组。 有关详细信息，请参阅 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)。  
  
 order_by_clause   
 `ORDER BY` 子句可确定在特定分区中为行分配唯一 `ROW_NUMBER` 的顺序。 它是必需的。 有关详细信息，请参阅 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)。  
  
## <a name="return-types"></a>返回类型  
 **bigint**  
  
## <a name="general-remarks"></a>一般备注  
 除非以下条件成立，否则不保证在每次执行时，使用 `ROW_NUMBER()` 的查询所返回行的顺序都完全相同。  
  
1.  分区列的值是唯一的。  
  
2.  `ORDER BY` 列的值是唯一的。  
  
3.  分区列和 `ORDER BY` 列的值的组合是唯一的。  
  
 `ROW_NUMBER()` 具有不确定性。 有关详细信息，请参阅 [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-simple-examples"></a>A. 简单示例 

以下查询按字母顺序返回四个系统表。

```sql
SELECT 
  name, recovery_model_desc
FROM sys.databases 
WHERE database_id < 5
ORDER BY name ASC;
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
   
|name    |recovery_model_desc |  
|-----------  |------------ |  
|主 |SIMPLE |
|模型 |FULL |
|msdb |SIMPLE |
|tempdb |SIMPLE |

要在每行的前面添加一个行编号列，请使用 `ROW_NUMBER` 函数添加一个列（此示例中名为 `Row#`）。 必须将 `ORDER BY` 子句向前移动到 `OVER` 子句处。

```sql
SELECT 
  ROW_NUMBER() OVER(ORDER BY name ASC) AS Row#,
  name, recovery_model_desc
FROM sys.databases 
WHERE database_id < 5;
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
   
|Row# |name    |recovery_model_desc |  
|------- |-----------  |------------ |  
|1 |主 |SIMPLE |
|2 |模型 |FULL |
|3 |msdb |SIMPLE |
|4 |tempdb |SIMPLE |

若是在 `recovery_model_desc` 列上添加 `PARTITION BY` 子句，当 `recovery_model_desc` 值发生更改时将重新开始编号。 
 
```sql
SELECT 
  ROW_NUMBER() OVER(PARTITION BY recovery_model_desc ORDER BY name ASC) 
    AS Row#,
  name, recovery_model_desc
FROM sys.databases WHERE database_id < 5;
```  

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
   
|Row# |name    |recovery_model_desc |  
|------- |-----------  |------------ |  
|1 |模型 |FULL |
|1 |主 |SIMPLE |
|2 |msdb |SIMPLE |
|3 |tempdb |SIMPLE |


### <a name="b-returning-the-row-number-for-salespeople"></a>B. 返回销售人员的行号  
 以下示例根据销售人员年初至今的销售额，计算 [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] 中销售人员的行号。  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT ROW_NUMBER() OVER(ORDER BY SalesYTD DESC) AS Row,   
    FirstName, LastName, ROUND(SalesYTD,2,1) AS "Sales YTD"   
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL AND SalesYTD <> 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
Row FirstName    LastName               SalesYTD  
--- -----------  ---------------------- -----------------  
1   Linda        Mitchell               4251368.54  
2   Jae          Pak                    4116871.22  
3   Michael      Blythe                 3763178.17  
4   Jillian      Carson                 3189418.36  
5   Ranjit       Varkey Chudukatil      3121616.32  
6   José         Saraiva                2604540.71  
7   Shu          Ito                    2458535.61  
8   Tsvi         Reiter                 2315185.61  
9   Rachel       Valdez                 1827066.71  
10  Tete         Mensa-Annan            1576562.19  
11  David        Campbell               1573012.93  
12  Garrett      Vargas                 1453719.46  
13  Lynn         Tsoflias               1421810.92  
14  Pamela       Ansman-Wolfe           1352577.13  
```  
  
### <a name="c-returning-a-subset-of-rows"></a>C. 返回行的子集  
 下面的示例按 `SalesOrderHeader` 的顺序计算 `OrderDate` 表中所有行的行号，并只返回行 `50` 到 `60`（含）。  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH OrderedOrders AS  
(  
    SELECT SalesOrderID, OrderDate,  
    ROW_NUMBER() OVER (ORDER BY OrderDate) AS RowNumber  
    FROM Sales.SalesOrderHeader   
)   
SELECT SalesOrderID, OrderDate, RowNumber    
FROM OrderedOrders   
WHERE RowNumber BETWEEN 50 AND 60;  
```  
  
### <a name="d-using-row_number-with-partition"></a>D. 将 ROW_NUMBER () 与 PARTITION 一起使用  
 以下示例使用 `PARTITION BY` 参数按列 `TerritoryName` 对结果集进行分区。 在 `ORDER BY` 子句中指定的 `OVER` 子句按列 `SalesYTD` 对每个分区中的行进行排序。 `ORDER BY` 语句中的 `SELECT` 按 `TerritoryName` 子句对整个查询结果集进行排序。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT FirstName, LastName, TerritoryName, ROUND(SalesYTD,2,1) AS SalesYTD,  
ROW_NUMBER() OVER(PARTITION BY TerritoryName ORDER BY SalesYTD DESC) 
  AS Row  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL AND SalesYTD <> 0  
ORDER BY TerritoryName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
FirstName  LastName             TerritoryName        SalesYTD      Row  
---------  -------------------- ------------------   ------------  ---  
Lynn       Tsoflias             Australia            1421810.92    1  
José       Saraiva              Canada               2604540.71    1  
Garrett    Vargas               Canada               1453719.46    2  
Jillian    Carson               Central              3189418.36    1  
Ranjit     Varkey Chudukatil    France               3121616.32    1  
Rachel     Valdez               Germany              1827066.71    1  
Michael    Blythe               Northeast            3763178.17    1  
Tete       Mensa-Annan          Northwest            1576562.19    1  
David      Campbell             Northwest            1573012.93    2  
Pamela     Ansman-Wolfe         Northwest            1352577.13    3  
Tsvi       Reiter               Southeast            2315185.61    1  
Linda      Mitchell             Southwest            4251368.54    1  
Shu        Ito                  Southwest            2458535.61    2  
Jae        Pak                  United Kingdom       4116871.22    1  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-returning-the-row-number-for-salespeople"></a>E. 返回销售人员的行号  
 以下示例根据销售代表所分配的销售配额返回各自的 `ROW_NUMBER`。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ROW_NUMBER() OVER(ORDER BY SUM(SalesAmountQuota) DESC) 
    AS RowNumber,  
    FirstName, LastName,   
    CONVERT(varchar(13), SUM(SalesAmountQuota),1) AS SalesQuota   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq  
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE e.SalesPersonFlag = 1  
GROUP BY LastName, FirstName;  
```  
  
 以下为部分结果集。  

```  

RowNumber  FirstName  LastName            SalesQuota  
---------  ---------  ------------------  -------------  
1          Jillian    Carson              12,198,000.00  
2          Linda      Mitchell            11,786,000.00  
3          Michael    Blythe              11,162,000.00  
4          Jae        Pak                 10,514,000.00  
```

### <a name="f-using-row_number-with-partition"></a>F. 将 ROW_NUMBER () 与 PARTITION 一起使用  
 以下示例显示了将 `ROW_NUMBER` 函数与 `PARTITION BY` 参数结合使用的情况。 这样会让 `ROW_NUMBER` 函数对每个分区中的行进行编号。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ROW_NUMBER() OVER(PARTITION BY SalesTerritoryKey 
        ORDER BY SUM(SalesAmountQuota) DESC) AS RowNumber,  
    LastName, SalesTerritoryKey AS Territory,  
    CONVERT(varchar(13), SUM(SalesAmountQuota),1) AS SalesQuota   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq  
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE e.SalesPersonFlag = 1  
GROUP BY LastName, FirstName, SalesTerritoryKey;  
```  
  
 以下为部分结果集。  
 
```  
 
RowNumber  LastName            Territory  SalesQuota  
---------  ------------------  ---------  -------------  
1          Campbell            1           4,025,000.00  
2          Ansman-Wolfe        1           3,551,000.00  
3          Mensa-Annan         1           2,275,000.00  
1          Blythe              2          11,162,000.00  
1          Carson              3          12,198,000.00  
1          Mitchell            4          11,786,000.00  
2          Ito                 4           7,804,000.00  
```
  
## <a name="see-also"></a>另请参阅  
 [RANK (Transact-SQL)](../../t-sql/functions/rank-transact-sql.md)   
 [DENSE_RANK (Transact-SQL)](../../t-sql/functions/dense-rank-transact-sql.md)   
 [NTILE (Transact-SQL)](../../t-sql/functions/ntile-transact-sql.md)  
  
  


