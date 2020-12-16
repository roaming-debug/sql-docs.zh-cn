---
title: 使用 PIVOT 和 UNPIVOT | Microsoft Docs
description: PIVOT 和 UNPIVOT 关系运算符的 Transact-SQL 参考。 使用 SELECT 语句上的这些运算符可将表值表达式更改为另一个表。
ms.date: 10/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- PIVOT_TSQL
helpviewer_keywords:
- FROM clause, UNPIVOT operator
- unpivoting tables
- table pivoting [SQL Server]
- UNPIVOT operator
- crosstab query
- PIVOT operator
- rotating table-valued expressions
- pivoting tables
- FROM clause, PIVOT operator
- rotating columns
ms.assetid: 24ba54fc-98f7-4d35-8881-b5158aac1d66
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8495d1e9a4f5294b63e307569bb904be5c777e28
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464228"
---
# <a name="from---using-pivot-and-unpivot"></a>FROM - 使用 PIVOT 和 UNPIVOT

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

可以使用 `PIVOT` 和 `UNPIVOT` 关系运算符将表值表达式更改为另一个表。 `PIVOT` 通过将表达式中的一个列的唯一值转换为输出中的多列，来轮替表值表达式。 `PIVOT` 在需要对最终输出所需的所有剩余列值执行聚合时运行聚合。 与 PIVOT 执行的操作相反，`UNPIVOT` 将表值表达式的列轮换为列值。  
  
`PIVOT` 提供的语法比一系列复杂的 `SELECT...CASE` 语句中所指定的语法更简单和更具可读性。 有关 `PIVOT` 语法的完整说明，请参阅 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)。  
  
## <a name="syntax"></a>语法  
以下语法总结了如何使用 `PIVOT` 运算符。  
  
```syntaxsql  
SELECT <non-pivoted column>,  
    [first pivoted column] AS <column name>,  
    [second pivoted column] AS <column name>,  
    ...  
    [last pivoted column] AS <column name>  
FROM  
    (<SELECT query that produces the data>)   
    AS <alias for the source query>  
PIVOT  
(  
    <aggregation function>(<column being aggregated>)  
FOR   
[<column that contains the values that will become column headers>]   
    IN ( [first pivoted column], [second pivoted column],  
    ... [last pivoted column])  
) AS <alias for the pivot table>  
<optional ORDER BY clause>;  
```  

## <a name="remarks"></a>备注  
`UNPIVOT` 子句中的列标识符需遵循目录排序规则。 对于 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]，排序规则始终是 `SQL_Latin1_General_CP1_CI_AS`。 对于 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 部分包含的数据库，排序规则始终是 `Latin1_General_100_CI_AS_KS_WS_SC`。 如果将该列与与其他列合并，则需要 collate 子句 (`COLLATE DATABASE_DEFAULT`) 以避免冲突。  

  
## <a name="basic-pivot-example"></a>简单 PIVOT 示例  
下面的代码示例生成一个两列四行的表。  
  
```sql
USE AdventureWorks2014 ;  
GO  
SELECT DaysToManufacture, AVG(StandardCost) AS AverageCost   
FROM Production.Product  
GROUP BY DaysToManufacture;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
DaysToManufacture AverageCost
----------------- -----------
0                 5.0885
1                 223.88
2                 359.1082
4                 949.4105
```
  
没有定义 `DaysToManufacture` 为 3 的产品。  
  
以下代码显示相同的结果，该结果经过透视以使 `DaysToManufacture` 值成为列标题。 提供一个列表示三 `[3]` 天，即使结果为 `NULL`。  
  
```sql
-- Pivot table with one row and five columns  
SELECT 'AverageCost' AS Cost_Sorted_By_Production_Days,   
[0], [1], [2], [3], [4]  
FROM  
(SELECT DaysToManufacture, StandardCost   
    FROM Production.Product) AS SourceTable  
PIVOT  
(  
AVG(StandardCost)  
FOR DaysToManufacture IN ([0], [1], [2], [3], [4])  
) AS PivotTable;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Cost_Sorted_By_Production_Days 0           1           2           3           4         
------------------------------ ----------- ----------- ----------- ----------- -----------
AverageCost                    5.0885      223.88      359.1082    NULL        949.4105
```
  
## <a name="complex-pivot-example"></a>复杂 PIVOT 示例  
若要生成交叉表报表来汇总数据，通常可能会发现 `PIVOT` 很有用。 例如，假设需要在 `PurchaseOrderHeader` 示例数据库中查询 `AdventureWorks2014` 表以确定由某些特定雇员所下的采购订单数。 以下查询提供了此报表（按供应商排序）。  
  
```sql
USE AdventureWorks2014;  
GO  
SELECT VendorID, [250] AS Emp1, [251] AS Emp2, [256] AS Emp3, [257] AS Emp4, [260] AS Emp5  
FROM   
(SELECT PurchaseOrderID, EmployeeID, VendorID  
FROM Purchasing.PurchaseOrderHeader) p  
PIVOT  
(  
COUNT (PurchaseOrderID)  
FOR EmployeeID IN  
( [250], [251], [256], [257], [260] )  
) AS pvt  
ORDER BY pvt.VendorID;  
```  
  
以下为部分结果集。  
  
```
VendorID    Emp1        Emp2        Emp3        Emp4        Emp5  
----------- ----------- ----------- ----------- ----------- -----------
1492        2           5           4           4           4
1494        2           5           4           5           4
1496        2           4           4           5           5
1498        2           5           4           4           4
1500        3           4           4           5           4
```
  
将在 `EmployeeID` 列上透视此嵌套 select 语句返回的结果。  
  
```sql
SELECT PurchaseOrderID, EmployeeID, VendorID  
FROM PurchaseOrderHeader;  
```  
  
`EmployeeID` 列返回的唯一值变成了最终结果集中的字段。 因此，在 pivot 子句中指定的每个 `EmployeeID` 号都有对应的列：在此示例中，为员工 `250`、`251`、`256`、`257` 和 `260`。 `PurchaseOrderID` 列作为值列，将根据此列对最终输出中返回的列（称为分组列）进行分组。 在本例中，通过 `COUNT` 函数聚合分组列。 请注意，系统会显示警告消息，以指明在为每个员工计算 `COUNT` 时，未考虑 `PurchaseOrderID` 列中的任何 NULL 值。  
  
> [!IMPORTANT]  
>  如果聚合函数与 `PIVOT` 一起使用，则计算聚合时将不考虑出现在值列中的任何空值。  

## <a name="unpivot-example"></a>逆透视示例
  
与 `PIVOT` 执行的操作几乎相反，`UNPIVOT` 将列轮换为行。 假设以上示例中生成的表在数据库中存储为 `pvt`，并且您需要将列标识符 `Emp1`、`Emp2`、`Emp3`、`Emp4` 和 `Emp5` 旋转为对应于特定供应商的行值。 因此，必须标识另外两个列。 包含要轮换的列值（`Emp1`、`Emp2`...）的列称为 `Employee`，保留要轮换列下的现有值的列称为 `Orders`。 这些列分别对应于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 定义中的 pivot_column 和 value_column   。 以下为该查询。  
  
```sql
-- Create the table and insert values as portrayed in the previous example.  
CREATE TABLE pvt (VendorID INT, Emp1 INT, Emp2 INT,  
    Emp3 INT, Emp4 INT, Emp5 INT);  
GO  
INSERT INTO pvt VALUES (1,4,3,5,4,4);  
INSERT INTO pvt VALUES (2,4,1,5,5,5);  
INSERT INTO pvt VALUES (3,4,3,5,4,4);  
INSERT INTO pvt VALUES (4,4,2,5,5,4);  
INSERT INTO pvt VALUES (5,5,1,5,5,5);  
GO  
-- Unpivot the table.  
SELECT VendorID, Employee, Orders  
FROM   
   (SELECT VendorID, Emp1, Emp2, Emp3, Emp4, Emp5  
   FROM pvt) p  
UNPIVOT  
   (Orders FOR Employee IN   
      (Emp1, Emp2, Emp3, Emp4, Emp5)  
)AS unpvt;  
GO  
```  
  
以下为部分结果集。  
  
```
VendorID    Employee    Orders
----------- ----------- ------
1            Emp1       4
1            Emp2       3 
1            Emp3       5
1            Emp4       4
1            Emp5       4
2            Emp1       4
2            Emp2       1
2            Emp3       5
2            Emp4       5
2            Emp5       5
...
```
  
请注意，`UNPIVOT` 并不完全是 `PIVOT` 的逆操作。 `PIVOT` 执行聚合，并将多个可能的行合并为输出中的一行。 `UNPIVOT` 不重现原始表值表达式的结果，因为行已被合并。 另外，`UNPIVOT` 输入中的 NULL 值也在输出中消失了。 如果值消失，表明在执行 `PIVOT` 操作前，输入中可能就已存在原始 NULL 值。  
  
[!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库中的 `Sales.vSalesPersonSalesByFiscalYears` 视图将使用 `PIVOT` 返回每个销售人员在每个会计年度的总销售额。 若要在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中编写视图脚本，请在“对象资源管理器”中的“视图”文件夹下找到 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库对应的视图   。 右键单击该视图名称，再选择“编写视图脚本为”  。  
  
## <a name="see-also"></a>另请参阅  
[FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)   
[CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)  
  
