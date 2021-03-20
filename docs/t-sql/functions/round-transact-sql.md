---
description: ROUND (Transact-SQL)
title: ROUND (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ROUND_TSQL
- ROUND
dev_langs:
- TSQL
helpviewer_keywords:
- rounding expressions
- ROUND function [Transact-SQL]
ms.assetid: 23921ed6-dd6a-4c9e-8c32-91c0d44fe4b7
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: efcf7dbbfebaf00bcbba94b0b28cae0cdfcc700a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104734564"
---
# <a name="round-transact-sql"></a>ROUND (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

返回一个数值，舍入到指定的长度或精度。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ROUND ( numeric_expression , length [ ,function ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *numeric_expression*  
 是精确或近似数值数据类型类别（bit 数据类型除外）的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
 *length*  
 它是 numeric_expression 的舍入精度。 length 必须是 tinyint、smallint 或 int 类型的表达式  。如果 length 为正数，则将 numeric_expression 舍入到 length 指定的小数位数  。 如果 length 为负数，则将 numeric_expression 小数点左边部分舍入到 length 指定的长度。  
  
 *函数*  
 要执行的操作的类型。 function 的数据类型必须是 tinyint、smallint 或 int  。如果 function 省略或其值为 0（默认值），则对 numeric_expression  进行舍入。 如果指定了 0 以外的值，则将截断 numeric_expression。  
  
## <a name="return-types"></a>返回类型  
 返回以下数据类型。  
  
|表达式结果|返回类型|  
|-----------------------|-----------------|  
|**tinyint**|**int**|  
|**smallint**|**int**|  
|**int**|**int**|  
|**bigint**|**bigint**|  
|**decimal** 和 **numeric** 类别 (p, s)|**decimal(p, s)**|  
|money 和 smallmoney 类别|**money**|  
|float 和 real 类别|**float**|  
  
## <a name="remarks"></a>注解  
 ROUND 始终返回一个值。 如果 length 为负数，并且大于小数点前的数字个数，则 ROUND 将返回 0。  
  
|示例|结果|  
|-------------|------------|  
|ROUND(748.58, -4)|0|  
  
 如果 length 为负数，则无论什么数据类型，ROUND 都将返回一个舍入的 numeric_expression。  
  
|示例|结果|  
|--------------|------------|  
|ROUND(748.58, -1)|750.00|  
|ROUND(748.58, -2)|700.00|  
|ROUND(748.58, -3)|导致算术溢出，因为 748.58 默认为 decimal(5,2)，它无法返回 1000.00。|  
|若要向上舍入到 4 位，请更改输入的数据类型。 例如：<br /><br /> `SELECT ROUND(CAST (748.58 AS decimal (6,2)),-3);`|1000.00|  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-round-and-estimates"></a>A. 使用 ROUND 和估计值  
 以下示例显示了两个表达式，阐释使用了 `ROUND` 后，最后一位数将始终为估计值。  
  
```sql  
SELECT ROUND(123.9994, 3), ROUND(123.9995, 3);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------- -----------  
123.9990    124.0000      
```  
  
### <a name="b-using-round-and-rounding-approximations"></a>B. 使用 ROUND 和舍入近似值  
 以下示例显示舍入和近似值。  
  
```  
SELECT ROUND(123.4545, 2), ROUND(123.45, -2);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

 ```
----------  ----------
123.4500    100.00
```
  
### <a name="c-using-round-to-truncate"></a>C. 使用 ROUND 截断  
 以下示例使用了两个 `SELECT` 语句，用于阐释舍入和截断之间的区别。 第一个语句舍入结果。 第二个语句截断结果。  
  
```sql  
SELECT ROUND(150.75, 0);  
GO  
SELECT ROUND(150.75, 0, 1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------  
151.00  
  
(1 row(s) affected)  
  
--------  
150.00  
  
(1 row(s) affected)  
```
  
## <a name="see-also"></a>另请参阅  
 [CEILING (Transact-SQL)](../../t-sql/functions/ceiling-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [FLOOR (Transact-SQL)](../../t-sql/functions/floor-transact-sql.md)   
 [数学函数 (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)
