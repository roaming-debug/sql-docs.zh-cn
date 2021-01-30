---
description: 十进制数字
title: 十进制数字 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- size of data types [ODBC]
- decimal digits of data types [ODBC]
- data types [ODBC], decimal digits
- SQL data types [ODBC], column characteristics
ms.assetid: 07f3d1fc-b4ee-4693-b342-330b2231b6d0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f7a4d37601891c26ce4d59a9bfd788ce1c702ce5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194911"
---
# <a name="decimal-digits"></a>十进制数字
Decimal 和 numeric 数据类型的 *小数位数* 定义为小数点右边的最大位数或数据的小数位数。 对于近似浮点数列或参数，由于小数点右侧的位数不固定，所以未定义小数位数。 对于包含秒部分的日期时间或时间间隔数据，小数位数定义为数据的秒数部分中小数点右边的位数。  
  
 对于 SQL_DECIMAL 和 SQL_NUMERIC 数据类型，最大刻度通常与最大精度相同。 但是，某些数据源对最大刻度施加了单独的限制。 若要确定数据类型允许的最小和最大刻度，应用程序将调用 **SQLGetTypeInfo**。  
  
 下表显示了为每种简洁的 SQL 数据类型定义的十进制数字。  
  
|SQL 类型|十进制数字|  
|--------------|--------------------|  
|所有字符和二进制类型 [a]|不适用|  
|SQL_DECIMAL<br />SQL_NUMERIC|小数点右边定义的位数。 例如，定义为数值 (10，3) 的列的小数位数为3。 此值可以为负数以支持存储非常大的数字，而无需使用指数表示法;例如，"12000" 可存储为 "12"，小数位数为-3。|  
|除 SQL_DECIMAL 和 SQL_NUMERIC 之外的所有精确数值类型 [a]|0|  
|所有近似数据类型 [a]|不适用|  
|SQL_TYPE_DATE，以及不带秒的所有间隔类型组件 [a]|不适用|  
|除 SQL_TYPE_DATE 之外的所有日期时间类型以及具有秒部分的所有间隔类型|值的秒部分中小数点右边的位数 (秒) 的小数部分。 此数值不能为负数。|  
|SQL_GUID|不适用|  
  
 [a] 对于此数据类型，将忽略 **SQLBindParameter** 的 *DecimalDigits* 参数。  
  
 为十进制数字返回的值与任何一个描述符字段中的值都不对应。 值可以来自 SQL_DESC_SCALE 或 SQL_DESC_PRECISION 字段，具体取决于数据类型，如下表所示。  
  
|SQL 类型|对应的描述符字段<br /><br /> 十进制数字|  
|--------------|----------------------------------------------------------|  
|所有字符和二进制类型|不适用|  
|所有精确数值类型|SCALE|  
|SQL_BIT|不适用|  
|所有近似数值类型|不适用|  
|所有日期时间类型|PRECISION|  
|具有秒部分的所有间隔类型|PRECISION|  
|所有不包含秒的间隔类型组件|n/a|
