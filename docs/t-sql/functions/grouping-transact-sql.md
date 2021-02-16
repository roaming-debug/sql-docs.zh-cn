---
description: GROUPING (Transact-SQL)
title: GROUPING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GROUPING
- GROUPING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- null values [SQL Server], GROUPING function
- grouping columns
- ROLLUP operator
- GROUP BY clause, GROUPING function
- GROUPING function
- CUBE operator
ms.assetid: 4efa3868-1fc4-4626-8fb1-e863cc03e422
author: cawrites
ms.author: chadam
ms.openlocfilehash: 684e626cb1dac79ef29c838396bb4e855e3d0e23
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338033"
---
# <a name="grouping-transact-sql"></a>GROUPING (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  指示是否聚合 GROUP BY 列表中的指定列表达式。 在结果集中，如果 GROUPING 返回 1 则指示聚合；返回 0 则指示不聚合。 如果指定了 GROUP BY，则 GROUPING 只能用在 SELECT \<select> 列表、HAVING 和 ORDER BY 子句中。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
GROUPING ( <column_expression> )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 \<column_expression>  
 列或者 [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md) 子句中包含列的表达式。  
  
## <a name="return-types"></a>返回类型  
 **tinyint**  
  
## <a name="remarks"></a>备注  
 GROUPING 用于区分标准空值和由 ROLLUP、CUBE 或 GROUPING SETS 返回的空值。 作为 ROLLUP、CUBE 或 GROUPING SETS 操作结果返回的 NULL 是 NULL 的特殊应用。 它在结果集内作为列的占位符，表示全体。  
  
## <a name="examples"></a>示例  
 以下示例将分组 `SalesQuota` 并聚合 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 `SaleYTD` 金额。 `GROUPING` 函数应用于 `SalesQuota` 列。  
  
```sql 
SELECT SalesQuota, SUM(SalesYTD) 'TotalSalesYTD', GROUPING(SalesQuota) AS 'Grouping'  
FROM Sales.SalesPerson  
GROUP BY SalesQuota WITH ROLLUP;  
GO  
```  
  
 结果集在 `SalesQuota` 下面显示两个 null 值。 第一个 `NULL` 代表从表中的这一列得到的 null 值组。 第二个 `NULL` 位于 ROLLUP 操作所添加的汇总行之中。 汇总行显示所有 `SalesQuota` 组的 `TotalSalesYTD` 金额，并以 `Grouping` 列中的 `1` 进行指示。  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 SalesQuota     TotalSalesYTD       Grouping  
------------   -----------------   --------  
NULL           1533087.5999          0  
250000.00      33461260.59           0  
300000.00      9299677.9445          0  
NULL           44294026.1344         1  

(4 row(s) affected)
```  
  
## <a name="see-also"></a>另请参阅  
 [GROUPING_ID (Transact-SQL)](../../t-sql/functions/grouping-id-transact-sql.md)   
 [GROUP BY (Transact-SQL)](../../t-sql/queries/select-group-by-transact-sql.md)  
  
  
