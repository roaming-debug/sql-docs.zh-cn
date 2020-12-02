---
description: STDEV (Transact-SQL)
title: STDEV (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STDEV_TSQL
- STDEV
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], statistical standard deviation
- STDEV function [Transact-SQL]
- statistical standard deviation
ms.assetid: ff41b4fc-4f71-4f18-bf78-96614ea908cc
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7e361eba9ad7256cb067d0c6726ca1467af26b48
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91379852"
---
# <a name="stdev-transact-sql"></a>STDEV (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回指定表达式中所有值的标准偏差。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql    
-- Aggregate Function Syntax   
STDEV ( [ ALL | DISTINCT ] expression )  
  
-- Analytic Function Syntax   
STDEV ([ ALL ] expression) OVER ( [ partition_by_clause ] order_by_clause)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **ALL**  
 对所有值应用该函数。 ALL 为默认值。  
  
 DISTINCT  
 指定考虑每一个唯一值。  
  
 *expression*  
 数值[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 不允许使用聚合函数和子查询。 expression 是精确数值或近似数值数据类型类别（bit 数据类型除外）的表达式。  
  
 OVER **(** [ *partition_by_clause* ] _order\_by\_clause_ **)**  
 _partition\_by\_clause_ 将 FROM 子句生成的结果集划分为要应用函数的分区。 如果未指定，则此函数将查询结果集的所有行视为单个组。 _order\_by\_clause_ 确定执行操作的逻辑顺序。 _order\_by\_clause_ 是必需的。 有关详细信息，请参阅 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)。  
  
## <a name="return-types"></a>返回类型  
 **float**  
  
## <a name="remarks"></a>注解  
 如果在 SELECT 语句中的所有项目上使用 STDEV，则计算中包括结果集内的每个值。 STDEV 只能用于数字列。 Null 值会被忽略。  
  
 STDEV 不与 OVER 和 ORDER BY 子句配合使用时为确定性函数。 与 OVER 和 ORDER BY 子句一同指定时，它具有不确定性。 有关详细信息，请参阅 [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-stdev"></a>A：使用 STDEV  
 以下示例返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `SalesPerson` 表中所有奖金值的标准偏差。  
  
```sql  
SELECT STDEV(Bonus)  
FROM Sales.SalesPerson;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-using-stdev"></a>B：使用 STDEV  
 以下示例返回 `dbo.FactSalesQuota` 表中的销售配额值的标准差。 第一列中包含所有非重复值的标准差，第二列中包含所有值（包括任何重复值）的标准差。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT STDEV(DISTINCT SalesAmountQuota)AS Distinct_Values, STDEV(SalesAmountQuota) AS All_Values  
FROM dbo.FactSalesQuota;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Distinct_Values   All_Values
----------------  ----------------
398974.27         398450.57
 ```  
  
### <a name="c-using-stdev-with-over"></a>C. 通过 OVER 使用 STDEV  
 下面的示例返回日历年中每季度的销售配额值的标准差。 请注意，OVER 子句中的 ORDER BY 对 STDEV 进行排序，SELECT 中的 ORDER BY 则对结果集进行排序。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CalendarYear AS Year, CalendarQuarter AS Quarter, SalesAmountQuota AS SalesQuota,  
       STDEV(SalesAmountQuota) OVER (ORDER BY CalendarYear, CalendarQuarter) AS StdDeviation  
FROM dbo.FactSalesQuota  
WHERE EmployeeKey = 272 AND CalendarYear = 2002  
ORDER BY CalendarQuarter;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Year  Quarter  SalesQuota              StdDeviation
----  -------  ----------------------  -------------------
2002  1         91000.0000             null
2002  2        140000.0000             34648.23
2002  3         70000.0000             35921.21
2002  4        154000.0000             39752.36
 ```  
  
## <a name="see-also"></a>另请参阅  
 [聚合函数 (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)  
  
  

