---
description: CHECKSUM_AGG (Transact-SQL)
title: CHECKSUM_AGG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKSUM_AGG
- CHECKSUM_AGG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- checksum values
- CHECKSUM_AGG function
- groups [SQL Server], checksum values
ms.assetid: cdede70c-4eb5-4c92-98ab-b07787ab7222
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a423fefd8acfa2e917605955658f18c0a55c77d7
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96119044"
---
# <a name="checksum_agg-transact-sql"></a>CHECKSUM_AGG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函数返回组中各值的校验和。 `CHECKSUM_AGG` 将忽略 null 值。 [OVER 子句](../../t-sql/queries/select-over-clause-transact-sql.md)可以遵循 `CHECKSUM_AGG`。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CHECKSUM_AGG ( [ ALL | DISTINCT ] expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
**ALL**  
向所有值应用此聚合函数。 ALL 为默认参数。
  
DISTINCT  
指定 `CHECKSUM_AGG` 返回唯一值的校验和。
  
*expression*  
整数[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 `CHECKSUM_AGG` 不允许使用聚合函数或子查询。
  
## <a name="return-types"></a>返回类型
将所有表达式值的校验和作为 int 类型返回。
  
## <a name="remarks"></a>注解  
`CHECKSUM_AGG` 可以检测表中的更改。
  
`CHECKSUM_AGG` 结果不取决于表中行的顺序。 此外，`CHECKSUM_AGG` 函数允许使用 `DISTINCT` 关键字和 `GROUP BY` 子句。
  
如果表达式列表值更改，则列表校验和值列表很可能也会更改。 但是，也存在计算校验和不会更改这一很小的可能性。
  
`CHECKSUM_AGG` 具有类似于其他聚合函数的功能。 有关详细信息，请参阅[聚合函数 (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)。
  
## <a name="examples"></a>示例  
这些示例使用 `CHECKSUM_AGG` 检测 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中 `ProductInventory` 表的 `Quantity` 列中的更改。
  
```sql
--Get the checksum value before the column value is changed.  

SELECT CHECKSUM_AGG(CAST(Quantity AS INT))  
FROM Production.ProductInventory;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
------------------------  
262  
```  
  
```sql
UPDATE Production.ProductInventory   
SET Quantity=125  
WHERE Quantity=100;  
GO  

--Get the checksum of the modified column.  
SELECT CHECKSUM_AGG(CAST(Quantity AS INT))  
FROM Production.ProductInventory;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
------------------------  
287  
```  
  
## <a name="see-also"></a>另请参阅
[CHECKSUM (Transact-SQL)](../../t-sql/functions/checksum-transact-sql.md)  
[HASHBYTES (Transact-SQL)](../../t-sql/functions/hashbytes-transact-sql.md)  
[BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md)
[OVER Clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
