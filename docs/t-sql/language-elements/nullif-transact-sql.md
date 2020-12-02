---
description: NULLIF (Transact-SQL)
title: NULLIF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- NULLIF
- NULLIF_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- null values [SQL Server], equivalent expressions
- expressions [SQL Server], null values
- NULLIF function
- equivalent expressions [SQL Server]
ms.assetid: 44c7b67e-74c7-4bb9-93a4-7a3016bd2feb
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8bf1b3c571806aaaca17894c52c4673709074261
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92187847"
---
# <a name="nullif-transact-sql"></a>NULLIF (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  如果两个指定的表达式相等，则返回空值。 例如，`SELECT NULLIF(4,4) AS Same, NULLIF(5,7) AS Different;` 为第一列（4 和 4）返回 NULL，因为两个输入值相同。 第二列返回第一个值 (5)，因为两个输入值不同。 
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
NULLIF ( expression , expression )  
```  
  
## <a name="arguments"></a>参数  
 *expression*  
 为任意有效的标量[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 返回类型与第一个 expression 相同。  
  
 如果两个表达式不相等，则 NULLIF 返回第一个 expression 的值。 如果表达式相等，则 NULLIF 返回第一个 expression 类型的空值。  
  
## <a name="remarks"></a>注解  
 如果两个表达式相等且结果表达式为 NULL，则 NULLIF 等价于 CASE 搜索表达式。  
  
 我们建议在 NULLIF 函数内不要使用依赖于时间的函数，如 RAND()。 这会使函数计算两次并从两次调用中返回不同的结果。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-budget-amounts-that-have-not-changed"></a>A. 返回尚未更改的预算数量  
 以下示例创建 `budgets` 表以显示部门 (`dept`) 的当年预算 (`current_year`) 以及上一年预算 (`previous_year`)。 对于当年预算，那些同上一年相比预算没有改变的部门使用 `NULL`，那些预算还没有确定的部门使用 `0`。 若要只计算那些接收预算的部门的预算平均值，并包含上一年的预算值（`previous_year` 为 `current_year` 时，使用 `NULL` 的值），请组合使用 `NULLIF` 和 `COALESCE` 函数。  
  
```sql  
CREATE TABLE dbo.budgets  
(  
   dept            TINYINT   IDENTITY,  
   current_year    DECIMAL   NULL,  
   previous_year   DECIMAL   NULL  
);  
INSERT budgets VALUES(100000, 150000);  
INSERT budgets VALUES(NULL, 300000);  
INSERT budgets VALUES(0, 100000);  
INSERT budgets VALUES(NULL, 150000);  
INSERT budgets VALUES(300000, 250000);  
GO    
SET NOCOUNT OFF;  
SELECT AVG(NULLIF(COALESCE(current_year,  
   previous_year), 0.00)) AS 'Average Budget'  
FROM budgets;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Average Budget  
 --------------  
 212500.000000  
 (1 row(s) affected)
 ```  
  
### <a name="b-comparing-nullif-and-case"></a>B. 比较 NULLIF 和 CASE  
 若要显示 `NULLIF` 和 `CASE` 之间的相似性，则以下查询将计算 `MakeFlag` 和 `FinishedGoodsFlag` 列中的值是否相同。 第一个查询使用 `NULLIF`。 第二个查询使用 `CASE` 表达式。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ProductID, MakeFlag, FinishedGoodsFlag,   
   NULLIF(MakeFlag,FinishedGoodsFlag) AS 'Null if Equal'  
FROM Production.Product  
WHERE ProductID < 10;  
GO  
  
SELECT ProductID, MakeFlag, FinishedGoodsFlag,'Null if Equal' =  
   CASE  
       WHEN MakeFlag = FinishedGoodsFlag THEN NULL  
       ELSE MakeFlag  
   END  
FROM Production.Product  
WHERE ProductID < 10;  
GO  
```  

### <a name="c-returning-budget-amounts-that-contain-no-data"></a>C. 返回不包含数据的预算数量  
 如果 `current_year` 和 `previous_year` 都不包含数据，则以下示例创建 `budgets` 表、加载数据，并使用 `NULLIF` 返回 null。  
  
```sql  
CREATE TABLE budgets (  
   dept           TINYINT,  
   current_year   DECIMAL(10,2),  
   previous_year  DECIMAL(10,2)  
);  
  
INSERT INTO budgets VALUES(1, 100000, 150000);  
INSERT INTO budgets VALUES(2, NULL, 300000);  
INSERT INTO budgets VALUES(3, 0, 100000);  
INSERT INTO budgets VALUES(4, NULL, 150000);  
INSERT INTO budgets VALUES(5, 300000, 300000);  
  
SELECT dept, NULLIF(current_year,  
   previous_year) AS LastBudget  
FROM budgets;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 dept   LastBudget  
 ----   -----------  
 1      100000.00  
 2      null 
 3      0.00  
 4      null  
 5      null
 ```  
  
## <a name="see-also"></a>另请参阅  
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [decimal 和 numeric (Transact-SQL)](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)   
 [系统函数 (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  

