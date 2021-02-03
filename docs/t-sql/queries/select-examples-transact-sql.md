---
description: SELECT 示例 (Transact-SQL)
title: SELECT 示例 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- parentheses [SQL Server]
- GROUP BY clause, SELECT statement
- query hints [SQL Server]
- ALL keyword
- ROLLUP operator
- SELECT statement [SQL Server], examples
- correlated subqueries, SELECT statement
- SELECT INTO statement
- ORDER BY clause [Transact-SQL]
- GROUPING function
- index hints [SQL Server]
- HAVING clause, SELECT statement
- DISTINCT keyword
- CUBE operator
- UNION operator [SQL Server]
- computed sums
- WHERE clause, SELECT statement
ms.assetid: 9b9caa3d-e7d0-42e1-b60b-a5572142186c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e2e12a155eb1a3c710b3289912f9e9e12f3642ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207590"
---
# <a name="select-examples-transact-sql"></a>SELECT 示例 (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本主题提供了使用 [SELECT](../../t-sql/queries/select-transact-sql.md) 语句的示例。  
  
## <a name="a-using-select-to-retrieve-rows-and-columns"></a>A. 使用 SELECT 检索行和列  
 以下示例显示三个代码示例。 第一个代码示例返回 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的 `*` 表中的所有行（未指定 WHERE 子句）和所有列（使用了 `Product`）。  
  
 [!code-sql[Select#SelectExamples1](../../t-sql/queries/codesnippet/tsql/select-examples-transact_1.sql)]  
  
 该示例返回 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的 `Name` 表的所有行（未指定 WHERE 子句）和列子集（`ProductNumber`、`ListPrice`、`Product`）。 此外，还添加了一个列标题。  
  
 [!code-sql[Select#SelectExamples2](../../t-sql/queries/codesnippet/tsql/select-examples-transact_2.sql)]  
  
 该示例仅返回 `Product` 表中产品系列为 `R` 且生产天数少于 `4` 的那些行。  
  
 [!code-sql[Select#SelectExamples3](../../t-sql/queries/codesnippet/tsql/select-examples-transact_3.sql)]  
  
## <a name="b-using-select-with-column-headings-and-calculations"></a>B. 将 SELECT 与列标题和列计算一起使用  
 下面的示例返回 `Product` 表中的所有行。 第一个示例返回每种产品的总销售额与总折扣。 在第二个示例中，计算每种产品的总收入。  
  
 [!code-sql[Select#SelectExamples4](../../t-sql/queries/codesnippet/tsql/select-examples-transact_4.sql)]  
  
 该查询将计算每个销售订单中每种产品的收入。  
  
 [!code-sql[Select#SelectExamples5](../../t-sql/queries/codesnippet/tsql/select-examples-transact_5.sql)]  
  
## <a name="c-using-distinct-with-select"></a>C. 将 DISTINCT 与 SELECT 一起使用  
 以下示例使用 `DISTINCT` 以避免检索重复标题。  
  
 [!code-sql[Select#SelectExamples6](../../t-sql/queries/codesnippet/tsql/select-examples-transact_6.sql)]  
  
## <a name="d-creating-tables-with-select-into"></a>D. 使用 SELECT INTO 创建表  
 以下第一个示例将在 `#Bicycles` 中创建一个名为 `tempdb` 的临时表。  
  
 [!code-sql[Select#SelectExamples7](../../t-sql/queries/codesnippet/tsql/select-examples-transact_7.sql)]  
  
 第二个示例创建永久表 `NewProducts`。  
  
 [!code-sql[Select#SelectExamples8](../../t-sql/queries/codesnippet/tsql/select-examples-transact_8.sql)]  
  
## <a name="e-using-correlated-subqueries"></a>E. 使用相关子查询
相关子查询依赖于外部查询来查询其值。 该查询能为外部查询可能选择的每一行各重复执行一次。

第一个示例展示了语义等效的查询，以说明使用 `EXISTS` 关键字和 `IN` 关键字的区别。 两个都是有效子查询示例，用于检索产品型号为长袖标志运动衫且 `ProductModelID` 编号在 `Product` 和 `ProductModel` 两个表中相匹配的每种产品名称的实例。   
  
 [!code-sql[Select#SelectExamples9](../../t-sql/queries/codesnippet/tsql/select-examples-transact_9.sql)]  
  
 下个示例使用 `IN` 并检索 `SalesPerson` 表中奖金为 `5000.00` 且雇员标识号在 `Employee` 和 `SalesPerson` 表中相匹配的每个雇员姓名的实例。  
  
 [!code-sql[Select#SelectExamples10](../../t-sql/queries/codesnippet/tsql/select-examples-transact_10.sql)]  
  
 该语句中前面的子查询无法独立于外部查询进行计算。 它需要使用 `Employee.EmployeeID` 值，但是该值随着 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]检查的 `Employee` 中的不同行而发生改变。  
  
 相关子查询还可以用于外部查询的 `HAVING` 子句。 以下示例查找其最高标价高于其平均标价两倍以上的产品型号。  
  
 [!code-sql[Select#SelectExamples11](../../t-sql/queries/codesnippet/tsql/select-examples-transact_11.sql)]  
  
 此示例使用两个相关子查询查找售出过某种特定产品的雇员的姓名。  
  
 [!code-sql[Select#SelectExamples12](../../t-sql/queries/codesnippet/tsql/select-examples-transact_12.sql)]  
  
## <a name="f-using-group-by"></a>F. 使用 GROUP BY  
 以下示例查找数据库中各销售订单的总额。  
  
 [!code-sql[Select#SelectExamples13](../../t-sql/queries/codesnippet/tsql/select-examples-transact_13.sql)]  
  
 由于使用了 `GROUP BY` 子句，因此针对每个销售订单只返回一行销售总额。  
  
## <a name="g-using-group-by-with-multiple-groups"></a>G. 对多个组使用 GROUP BY  
 以下示例查找按产品 ID 和特价产品 ID 分组的平均价格和迄今为止的年销售总额。  
  
 [!code-sql[Select#SelectExamples14](../../t-sql/queries/codesnippet/tsql/select-examples-transact_14.sql)]  
  
## <a name="h-using-group-by-and-where"></a>H. 使用 GROUP BY 和 WHERE  
 以下示例在只检索标价大于 `$1000` 的行后，将结果进行分组。  
  
 [!code-sql[Select#SelectExamples15](../../t-sql/queries/codesnippet/tsql/select-examples-transact_15.sql)]  
  
## <a name="i-using-group-by-with-an-expression"></a>I. 将 GROUP BY 与表达式一起使用  
 以下示例按表达式进行分组。 如果表达式不包含聚合函数，则可以按表达式进行分组。  
  
 [!code-sql[Select#SelectExamples16](../../t-sql/queries/codesnippet/tsql/select-examples-transact_16.sql)]  
  
## <a name="j-using-group-by-with-order-by"></a>J. 将 GROUP BY 与 ORDER BY 一起使用  
 以下示例查找每种产品的平均价格并按平均价格将结果排序。  
  
 [!code-sql[Select#SelectExamples18](../../t-sql/queries/codesnippet/tsql/select-examples-transact_17.sql)]  
  
## <a name="k-using-the-having-clause"></a>K. 使用 HAVING 子句  
 下面的第一个示例显示带聚合函数的 `HAVING` 子句。 该子句按产品 ID 将 `SalesOrderDetail` 表中的行进行分组并消除那些平均订单数量等于或小于五的产品。 第二个示例显示不带聚合函数的 `HAVING` 子句。  
  
 [!code-sql[Select#SelectExamples19](../../t-sql/queries/codesnippet/tsql/select-examples-transact_18.sql)]  
  
 该查询在 `LIKE` 子句中使用 `HAVING` 子句。  
  
```sql
USE AdventureWorks2012 ;  
GO  
SELECT SalesOrderID, CarrierTrackingNumber   
FROM Sales.SalesOrderDetail  
GROUP BY SalesOrderID, CarrierTrackingNumber  
HAVING CarrierTrackingNumber LIKE '4BD%'  
ORDER BY SalesOrderID ;  
GO  
```  
  
## <a name="l-using-having-and-group-by"></a>L. 使用 HAVING 和 GROUP BY  
 以下示例显示在一个 `GROUP BY` 语句中使用 `HAVING`、`WHERE`、`ORDER BY` 和 `SELECT` 子句。 该语句生成组和汇总值（但是组和汇总值是在消除价格超过 $25 且平均订单数量低于 5 的产品之后得出的）。 它还按 `ProductID` 组织其结果。  
  
 [!code-sql[Select#SelectExamples21](../../t-sql/queries/codesnippet/tsql/select-examples-transact_19.sql)]  
  
## <a name="m-using-having-with-sum-and-avg"></a>M. 将 HAVING 与 SUM 和 AVG 一起使用  
 以下示例按产品 ID 将 `SalesOrderDetail` 表进行分组，结果中仅包含订单总金额超过 `$1000000.00` 且其平均订单数量少于 `3` 的产品的组。  
  
 [!code-sql[Select#SelectExamples22](../../t-sql/queries/codesnippet/tsql/select-examples-transact_20.sql)]  
  
 若要查看总销售额超过 `$2000000.00` 的产品，请使用以下查询：  
  
 [!code-sql[Select#SelectExamples23](../../t-sql/queries/codesnippet/tsql/select-examples-transact_21.sql)]  
  
 若要确保在计算中至少为每种产品包含 1500 项，请使用 `HAVING COUNT(*) > 1500` 消除返回总数少于 `1500` 售出项的产品。 该查询如下所示：  
  
 [!code-sql[Select#SelectExamples24](../../t-sql/queries/codesnippet/tsql/select-examples-transact_22.sql)]  
  
## <a name="n-using-the-index-optimizer-hint"></a>N. 使用 INDEX 优化器提示  
 以下示例说明了使用 `INDEX` 优化器提示的两种方式。 第一个示例说明如何强制优化器使用非聚集索引检索表中的行，第二个示例使用索引 0 强制执行表扫描。  
  
 [!code-sql[Select#SelectExamples45](../../t-sql/queries/codesnippet/tsql/select-examples-transact_23.sql)]  
  
## <a name="m-using-option-and-the-group-hints"></a>M. 使用 OPTION 和 GROUP 提示  
 以下示例说明了如何将 `OPTION (GROUP)` 子句与 `GROUP BY` 子句一起使用。  
  
 [!code-sql[Select#SelectExamples46](../../t-sql/queries/codesnippet/tsql/select-examples-transact_24.sql)]  
  
## <a name="o-using-the-union-query-hint"></a>O. 使用 UNION 查询提示  
 以下示例使用 `MERGE UNION` 查询提示。  
  
 [!code-sql[Select#SelectExamples47](../../t-sql/queries/codesnippet/tsql/select-examples-transact_25.sql)]  
  
## <a name="p-using-a-simple-union"></a>P. 使用简单 UNION  
 在下面的示例中，结果集同时包含 `ProductModelID` 和 `Name` 表中的 `ProductModel` 与 `Gloves` 列中的内容。  
  
 [!code-sql[Select#SelectExamples48](../../t-sql/queries/codesnippet/tsql/select-examples-transact_26.sql)]  
  
## <a name="q-using-select-into-with-union"></a>问： 将 SELECT INTO 与 UNION 一起使用  
 在下面的示例中，第二个 `INTO` 语句中的 `SELECT` 子句指定名为 `ProductResults` 的表保存 `ProductModel` 和 `Gloves` 表中的指定列的并集（最终结果集）。 注意，`Gloves` 表是由第一个 `SELECT` 语句创建的。  
  
 [!code-sql[Select#SelectExamples49](../../t-sql/queries/codesnippet/tsql/select-examples-transact_27.sql)]  
  
## <a name="r-using-union-of-two-select-statements-with-order-by"></a>R. 将 ORDER BY 与两个 SELECT 语句的 UNION 一起使用  
 在 UNION 子句中使用的某些参数的顺序非常重要。 下面的示例通过两个 `UNION` 语句说明 `SELECT` 的错误用法和正确用法（在这两个语句的输出中将重命名一个列）。  
  
 [!code-sql[Select#SelectExamples50](../../t-sql/queries/codesnippet/tsql/select-examples-transact_28.sql)]  
  
## <a name="s-using-union-of-three-select-statements-to-show-the-effects-of-all-and-parentheses"></a>S. 使用三个 SELECT 语句的 UNION 来说明 ALL 和括号的作用  
 下列示例使用 `UNION` 来组合具有相同 5 行数据的三个表的结果。 第一个示例使用 `UNION ALL` 显示重复记录，返回所有的 15 行。 第二个示例使用不带 `UNION` 的 `ALL`，删除三个 `SELECT` 语句的组合结果中的重复行，返回 5 行。  
  
 第三个示例将 `ALL` 用于第一个 `UNION`，并用括号将第二个没有使用 `UNION` 的 `ALL` 括起来。 第二个 `UNION` 因位于括号内而首先得到处理，并且因为没有使用 `ALL` 选项，所以重复行被删除而返回 5 行。 通过使用 `SELECT` 关键字将这 5 行与第一个 `UNION ALL` 的结果组合在一起。 这不会删除两个 5 行结果集之间的重复行。 最终结果有 10 行。  
  
 [!code-sql[Select#SelectExamples51](../../t-sql/queries/codesnippet/tsql/select-examples-transact_29.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)   
 [UNION (Transact-SQL)](../../t-sql/language-elements/set-operators-union-transact-sql.md)   
 [EXCEPT 和 INTERSECT (Transact-SQL)](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)   
 [PathName (Transact-SQL)](../../relational-databases/system-functions/pathname-transact-sql.md)   
 [INTO 子句 (Transact-SQL)](../../t-sql/queries/select-into-clause-transact-sql.md)  
  
  
