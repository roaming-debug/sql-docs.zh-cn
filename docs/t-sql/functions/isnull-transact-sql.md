---
description: ISNULL (Transact-SQL)
title: ISNULL (Transact-SQL)
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ISNULL
- ISNULL_TSQL
- IFNULL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- replacing null values
- null values [SQL Server], ISNULL
- null values [SQL Server], replacement values
- ISNULL function
ms.assetid: 6f3e5802-864b-4e77-9862-657bb5430b68
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6c7fc33998078c4fc853fbabcf26fb2c48226792
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746747"
---
# <a name="isnull-transact-sql"></a>ISNULL (Transact-SQL) 

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

使用指定的替换值替换 NULL。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ISNULL ( check_expression , replacement_value )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 check_expression  
 将被检查是否为 NULL 的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 check_expression 可以是任何类型  。  
  
 replacement_value  
 check_expression为 NULL 时要返回的表达式  。 replacement_value 必须是可隐式转换为 check_expression 类型的类型   。  
  
## <a name="return-types"></a>返回类型  
 返回与该 check_expression 相同的类型  。 如果提供了文本 NULL 作为 check_expression，则返回replacement_value 数据类型。 如果提供了文本 NULL 作为 check_expression 且未提供 replacement_value，则返回 int    。  
  
## <a name="remarks"></a>备注  
 如果 check_expression 不为 NULL，则将返回该表达式的值；否则，将返回 replacement_value。如果类型不同，则 replacement_value 会隐式转换为 check_expression 的类型    。 如果 replacement_value 长于 check_expression，则可能截断 replacement_value    。  
  
> [!NOTE]  
>  使用 [COALESCE (Transact-SQL)](../../t-sql/language-elements/coalesce-transact-sql.md) 返回第一个非 null 值。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-isnull-with-avg"></a>A. 将 ISNULL 与 AVG 一起使用  
 以下示例查找所有产品的重量平均值。 它用值 `50` 替换 `Weight` 表的 `Product` 列中的所有 NULL 项。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT AVG(ISNULL(Weight, 50))  
FROM Production.Product;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 -------------------------- 
59.79  
  
 (1 row(s) affected)
 ```  
  
### <a name="b-using-isnull"></a>B. 使用 ISNULL  
 以下示例选择 `AdventureWorks2012` 中所有特价产品的说明、折扣百分比、最小量和最大量。 如果某个特殊特价产品的最大量为 NULL，则结果集中显示的 `MaxQty` 为 `0.00`。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description, DiscountPct, MinQty, ISNULL(MaxQty, 0.00) AS 'Max Quantity'  
FROM Sales.SpecialOffer;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|  说明       |  DiscountPct    |   MinQty    |   最大数量       |
|  ---------------   |  -------------  |   --------  |   ---------------    |
|  No Discount       |  0.00           |   0         |   0                  |
|  Volume Discount   |  0.02           |   11        |   14                 |
|  Volume Discount   |  0.05           |   15        |   4                  |
|  Volume Discount   |  0.10           |   25        |   0                  |
|  Volume Discount   |  0.15           |   41        |   0                  |
|  Volume Discount   |  0.20           |   61        |   0                  |
|  Mountain-100 Cl   |  0.35           |   0         |   0                  |
|  Sport Helmet Di   |  0.10           |   0         |   0                  |
|  Road-650 Overst   |  0.30           |   0         |   0                  |
|  Mountain Tire S   |  0.50           |   0         |   0                  |
|  Sport Helmet Di   |  0.15           |   0         |   0                  |
|  LL Road Frame S   |  0.35           |   0         |   0                  |
|  Touring-3000 Pr   |  0.15           |   0         |   0                  |
|  Touring-1000 Pr   |  0.20           |   0         |   0                  |
|  Half-Price Peda   |  0.50           |   0         |   0                  |
|  Mountain-500 Si   |  0.40           |   0         |   0                  |

 `(16 row(s) affected)`  
  
### <a name="c-testing-for-null-in-a-where-clause"></a>C. 测试 WHERE 子句中的 NULL  
 请勿使用 ISNULL 查找 NULL 值。 而应使用 IS NULL。 下面的示例查找 weight 列中存在 `NULL` 的所有产品。 请注意 `IS` 和 `NULL` 之间的空格。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name, Weight  
FROM Production.Product  
WHERE Weight IS NULL;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-isnull-with-avg"></a>D. 将 ISNULL 与 AVG 一起使用  
 以下示例查找示例表中所有产品的重量平均值。 它用值 `50` 替换 `Weight` 表的 `Product` 列中的所有 NULL 项。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT AVG(ISNULL(Weight, 50))  
FROM dbo.DimProduct;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------------------------   
52.88   
```  
  
### <a name="e-using-isnull"></a>E. 使用 ISNULL  
 以下示例使用 ISNULL 测试列 `MinPaymentAmount` 中的 NULL 值，并显示这些行的值 `0.00`。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ResellerName,   
       ISNULL(MinPaymentAmount,0) AS MinimumPayment  
FROM dbo.DimReseller  
ORDER BY ResellerName;  
  
```  
  
 以下为部分结果集。  
  
|  ResellerName                |  MinimumPayment    |
|  -------------------------   |  --------------    |
|  自行车协会       |     0.0000         |
|  自行车商店                |     0.0000         |
|  自行车店                |     0.0000         |
|  杰出的自行车公司     |     0.0000         |
|  典型自行车店         |   200.0000         |
|  可接受的销售和服务  |     0.0000         |
  
### <a name="f-using-is-null-to-test-for-null-in-a-where-clause"></a>F. 使用 IS NULL 在 WHERE 子句中测试 NULL  
 下面的示例查找 `NULL` 列中存在 `Weight` 的所有产品。 请注意 `IS` 和 `NULL` 之间的空格。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EnglishProductName, Weight  
FROM dbo.DimProduct  
WHERE Weight IS NULL;  
```  
  
## <a name="see-also"></a>另请参阅  
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [IS NULL (Transact-SQL)](../../t-sql/queries/is-null-transact-sql.md)   
 [System Functions (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)   
 [COALESCE (Transact-SQL)](../../t-sql/language-elements/coalesce-transact-sql.md)  
  
  

