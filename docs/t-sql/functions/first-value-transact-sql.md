---
description: FIRST_VALUE (Transact-SQL)
title: FIRST_VALUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/22/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FIRST_VALUE_TSQL
- FIRST_VALUE
dev_langs:
- TSQL
helpviewer_keywords:
- FIRST_VALUE function
- analytic functions, FIRST_VALUE
ms.assetid: 1990c3c7-dad2-48db-b2cd-3e8bd2c49d17
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cba74e40373a4b00a93d41ebc49e2a849df98572
ms.sourcegitcommit: 442fbe1655d629ecef273b02fae1beb2455a762e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235499"
---
# <a name="first_value-transact-sql"></a>FIRST_VALUE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回有序值集中的第一个值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
FIRST_VALUE ( [scalar_expression ] )  [ IGNORE NULLS | RESPECT NULLS ]
    OVER ( [ partition_by_clause ] order_by_clause [ rows_range_clause ] )
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *scalar_expression*  
 是要返回的值。 scalar_expression 可以是产生单个值的列、子查询或其他任意表达式。 不允许使用其他分析函数。  

 [ IGNORE NULLS | RESPECT NULLS ]     
 **适用对象** ：Azure SQL Edge

 IGNORE NULLS - 计算分区上的第一个值时，忽略数据集中的 null 值。     
 RESPECT NULLS - 计算分区上的第一个值时，考虑数据集中的 null 值。     
 
  有关详细信息，请参阅[输入缺失值](/azure/azure-sql-edge/imputing-missing-values/)。
  
 OVER **(** [ *partition_by_clause* ] *order_by_clause* [ *rows_range_clause* ] **)**  
 partition_by_clause 将 FROM 子句生成的结果集划分为要应用函数的分区  。 如果未指定，则此函数将查询结果集的所有行视为单个组。 order_by_clause 确定执行操作的逻辑顺序。 需要 order_by_clause。 rows_range_clause 通过指定起点和终点，限制分区中的行数  。 有关详细信息，请参阅 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)。  
  
## <a name="return-types"></a>返回类型  
 是与 scalar_expression 相同的类型  。  
  
## <a name="general-remarks"></a>一般备注  
 FIRST_VALUE 具有不确定性。 有关详细信息，请参阅 [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-first_value-over-a-query-result-set"></a>A. 对查询结果集使用 FIRST_VALUE  
 以下示例使用 FIRST_VALUE 返回给定产品类别中价格最低廉的产品的名称。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name, ListPrice,   
       FIRST_VALUE(Name) OVER (ORDER BY ListPrice ASC) AS LeastExpensive   
FROM Production.Product  
WHERE ProductSubcategoryID = 37;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
Name                    ListPrice             LeastExpensive  
----------------------- --------------------- --------------------  
Patch Kit/8 Patches     2.29                  Patch Kit/8 Patches  
Road Tire Tube          3.99                  Patch Kit/8 Patches  
Touring Tire Tube       4.99                  Patch Kit/8 Patches  
Mountain Tire Tube      4.99                  Patch Kit/8 Patches  
LL Road Tire            21.49                 Patch Kit/8 Patches  
ML Road Tire            24.99                 Patch Kit/8 Patches  
LL Mountain Tire        24.99                 Patch Kit/8 Patches  
Touring Tire            28.99                 Patch Kit/8 Patches  
ML Mountain Tire        29.99                 Patch Kit/8 Patches  
HL Road Tire            32.60                 Patch Kit/8 Patches  
HL Mountain Tire        35.00                 Patch Kit/8 Patches  
  
```  
  
### <a name="b-using-first_value-over-partitions"></a>B. 对分区使用 FIRST_VALUE  
 以下示例使用 FIRST_VALUE 返回与同职务的其他员工相比，其休假小时数最少的员工。 PARTITION BY 子句按职务对员工分区，而 FIRST_VALUE 函数独立应用于每个分区。 在 OVER 子句中指定的 ORDER BY 子句确定对每个分区中的行应用 FIRST_VALUE 函数的逻辑顺序。 ROWS UNBOUNDED PRECEDING 子句将窗口的起点指定为每个分区的第一行。  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT JobTitle, LastName, VacationHours,   
       FIRST_VALUE(LastName) OVER (PARTITION BY JobTitle   
                                   ORDER BY VacationHours ASC  
                                   ROWS UNBOUNDED PRECEDING  
                                  ) AS FewestVacationHours  
FROM HumanResources.Employee AS e  
INNER JOIN Person.Person AS p   
    ON e.BusinessEntityID = p.BusinessEntityID  
ORDER BY JobTitle;  
```  
  
 以下为部分结果集。  
  
```  
  
JobTitle                            LastName                  VacationHours FewestVacationHours  
----------------------------------- ------------------------- ------------- -------------------  
Accountant                          Moreland                  58            Moreland  
Accountant                          Seamans                   59            Moreland  
Accounts Manager                    Liu                       57            Liu  
Accounts Payable Specialist         Tomic                     63            Tomic  
Accounts Payable Specialist         Sheperdigian              64            Tomic  
Accounts Receivable Specialist      Poe                       60            Poe  
Accounts Receivable Specialist      Spoon                     61            Poe  
Accounts Receivable Specialist      Walton                    62            Poe  
```  
  
## <a name="see-also"></a>另请参阅  
 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)  
 [Last_Value &#40;Transact-SQL&#41;](last-value-transact-sql.md)  

