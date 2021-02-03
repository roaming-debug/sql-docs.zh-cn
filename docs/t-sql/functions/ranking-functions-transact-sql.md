---
description: 排名函数 (Transact-SQL)
title: 排名函数 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- ranking functions
- row ranking [SQL Server]
- functions [SQL Server], ranking
- ranking rows
ms.assetid: e7f917ba-bf4a-4fe0-b342-a91bcf88a71b
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0be4ad27b55f5edbf5615adbf8ebfb5890be56d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182321"
---
# <a name="ranking-functions-transact-sql"></a>排名函数 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  排名函数为分区中的每一行返回一个排名值。 根据所用函数的不同，某些行可能与其他行接收到相同的值。 排名函数具有不确定性。  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 提供下列排名函数：  

:::row:::
    :::column:::
        [RANK](../../t-sql/functions/rank-transact-sql.md)
    :::column-end:::
    :::column:::
        [NTILE](../../t-sql/functions/ntile-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [DENSE_RANK](../../t-sql/functions/dense-rank-transact-sql.md)
    :::column-end:::
    :::column:::
        [ROW_NUMBER](../../t-sql/functions/row-number-transact-sql.md)
    :::column-end:::
:::row-end:::
  
## <a name="examples"></a>示例  
 以下示例显示了用在同一查询中的四个排名函数。 有关函数特定的示例，请参阅每个排名函数。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,ROW_NUMBER() OVER (ORDER BY a.PostalCode) AS "Row Number"  
    ,RANK() OVER (ORDER BY a.PostalCode) AS Rank  
    ,DENSE_RANK() OVER (ORDER BY a.PostalCode) AS "Dense Rank"  
    ,NTILE(4) OVER (ORDER BY a.PostalCode) AS Quartile  
    ,s.SalesYTD  
    ,a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL AND SalesYTD <> 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|FirstName|LastName|Row Number|Rank|Dense Rank|Quartile|SalesYTD|PostalCode|  
|---------------|--------------|----------------|----------|----------------|--------------|--------------|----------------|  
|Michael|Blythe|1|1|1|1|4557045.0459|98027|  
|Linda|Mitchell|2|1|1|1|5200475.2313|98027|  
|Jillian|Carson|3|1|1|1|3857163.6332|98027|  
|Garrett|Vargas|4|1|1|1|1764938.9859|98027|  
|Tsvi|Reiter|5|1|1|2|2811012.7151|98027|  
|Shu|Ito|6|6|2|2|3018725.4858|98055|  
|José|Saraiva|7|6|2|2|3189356.2465|98055|  
|David|Campbell|8|6|2|3|3587378.4257|98055|  
|Tete|Mensa-Annan|9|6|2|3|1931620.1835|98055|  
|Lynn|Tsoflias|10|6|2|3|1758385.926|98055|  
|Rachel|Valdez|11|6|2|4|2241204.0424|98055|  
|Jae|Pak|12|6|2|4|5015682.3752|98055|  
|Ranjit|Varkey Chudukatil|13|6|2|4|3827950.238|98055|  
  
## <a name="see-also"></a>另请参阅  
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)  
  
