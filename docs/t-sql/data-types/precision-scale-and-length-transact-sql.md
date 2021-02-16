---
description: 精度、小数位数和长度 (Transact-SQL)
title: 精度、小数位数和长度 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], length
- data types [SQL Server], scale
- precision [SQL Server], data types
- lengths [SQL Server], data types
- number of digits
- scale [SQL Server]
- scale [SQL Server], data types
- data types [SQL Server], precision
ms.assetid: fbc9ad2c-0d3b-4e98-8fdd-4d912328e40a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: da816b6e55bc1578a820a53d6e1036b685086713
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338045"
---
# <a name="precision-scale-and-length-transact-sql"></a>精度、小数位数和长度 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

精度指数字的位数。 小数位数指小数点后的数字位数。 例如，数 123.45 的精度是 5，小数位数是 2。
  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，numeric 和 decimal 数据类型的默认最大精度为 38。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本中，默认最大精度为 28。
  
数字数据类型的长度是存储此数所占用的字节数。 对于 varchar 和 char，字符串的长度是指字节数。 对于 nvarchar 和 nchar，字符串的长度是指字节对数。 binary、varbinary 和 image 数据类型的长度是字节数。 例如，int 数据类型可以有 10 位数，用 4 个字节存储，不接受小数点。 int 数据类型的精度是 10，长度是 4，小数位数是 0。
  
如果 char、varchar、binary 和 varbinary 表达式中的两种相串联，所生成表达式的长度是两个源表达式长度之和（不超过 8,000 个字节）。
  
如果两个 nchar 或 nvarchar 表达式相串联，所生成表达式的长度是两个源表达式长度之和（不超过 4,000 个字节对）。
  
使用 UNION、EXCEPT 或 INTERSECT 对数据类型相同但长度不同的两个表达式进行比较时，得到的长度为两个表达式中较大的长度。
  
除了 decimal 类型之外，数字数据类型的精度和小数位数都是固定的。 如果算术运算符有两个相同类型的表达式，结果就为该数据类型，并且具有对此类型定义的精度和小数位数。 如果运算符有两个不同数字数据类型的表达式，则由数据类型优先级决定结果的数据类型。 结果具有为该数据类型定义的精度和小数位数。
  
下表定义了当运算结果是 decimal 类型时，如何计算结果的精度和小数位数。 出现以下任一情况时，结果为 decimal 类型：
-   两个表达式都是 decimal 类型。  
-   一个表达式是 decimal 类型，而另一个是比 decimal 优先级低的数据类型。  
  
操作数表达式由表达式 e1（精度为 p1，小数位数为 s1）和表达式 e2（精度为 p2，小数位数为 s2）来表示。 非 decimal 类型的任何表达式的精度和小数位数，是对此表达式数据类型定义的精度和小数位数。 函数 max(a,b) 的涵义如下：取 "a" 和 "b" 中较大的值。 同样，min(a,b) 表示取 "a" 和 "b" 中较小的值。
  
|操作|结果精度|结果小数位数 *|  
|---|---|---|
|e1 + e2|max(s1, s2) + max(p1-s1, p2-s2) + 1|max(s1, s2)|  
|e1 - e2|max(s1, s2) + max(p1-s1, p2-s2) + 1|max(s1, s2)|  
|e1 * e2|p1 + p2 + 1|s1 + s2|  
|e1 / e2|p1 - s1 + s2 + max(6, s1 + p2 + 1)|max(6, s1 + p2 + 1)|  
|e1 { UNION &#124; EXCEPT &#124; INTERSECT } e2|max(s1, s2) + max(p1-s1, p2-s2)|max(s1, s2)|  
|e1 % e2|min(p1-s1, p2 -s2) + max( s1,s2 )|max(s1, s2)|  
  
\* 结果精度和小数位数的绝对最大值为 38。 当结果精度大于 38 时，它会减少到 38，并且相应的小数位数会减少，以尽量避免截断结果的整数部分。 在某些情况下（如乘法或除法），为了保持小数精度，比例因子将不会减少，虽然这可能引发溢出错误。

在加法和减法运算中，我们需要 `max(p1 - s1, p2 - s2)` 个位置来存储十进制数的整数部分。 如果空间不足，无法存储它们，即 `max(p1 - s1, p2 - s2) < min(38, precision) - scale`，则会减少小数位数以为整数部分提供足够空间。 生成的小数位数是 `MIN(precision, 38) - max(p1 - s1, p2 - s2)`，因此可能舍入小数部分，使其适合生成的小数位数。

在乘法和除法运算中，我们需要 `precision - scale` 个位置来存储结果的整数部分。 可能会使用以下规则减少小数位数：
1.  如果整数部分小于 32，则生成的小数位数减少到 `min(scale, 38 - (precision-scale))`，因为它不能大于 `38 - (precision-scale)`。 在这种情况下，结果可能会舍入。
1. 如果小数位数小于 6 且整数部分大于 32，则小数位数将保持不变。 在这种情况下，如果它不适合 decimal(38, scale)，则可能引发溢出错误 
1. 如果小数位数大于 6 且整数部分大于 32，则小数位数将设置为 6。 在这种情况下，整数部分和小数位数都会减少，生成的类型是 decimal(38,6)。 结果可能舍入为 6 位小数位数，否则如果整数部分不适合 32 位数字，将引发溢出错误。

## <a name="examples"></a>示例
下面的表达式返回结果 `0.00000090000000000`（未舍入），因为结果适合 `decimal(38,17)`：
```sql
select cast(0.0000009000 as decimal(30,20)) * cast(1.0000000000 as decimal(30,20)) [decimal 38,17]
```
在这种情况下，精度为 61，小数位数为 40。
整数部分 (precision-scale = 21) 小于 32，因此这是乘法规则中的情况 (1)，小数位数计算为 `min(scale, 38 - (precision-scale)) = min(40, 38 - (61-40)) = 17`。 结果类型是 `decimal(38,17)`。

下面的表达式返回结果 `0.000001` 以适合 `decimal(38,6)`：
```sql
SELECT CAST(0.0000009000 AS DECIMAL(30,10)) * CAST(1.0000000000 AS DECIMAL(30,10)) [decimal(38, 6)]
```
在这种情况下，精度为 61，小数位数为 20。
小数位数大于 6，整数部分 (`precision-scale = 41`) 大于 32。 这是乘法规则中的情况 (3)，结果类型是 `decimal(38,6)`。

## <a name="see-also"></a>另请参阅
[表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)  
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)
  
