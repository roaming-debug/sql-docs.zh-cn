---
description: 从 SQL 到 C：字符
title: SQL 到 C：字符 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], character
- character data type [ODBC]
- data conversions from SQL to C types [ODBC], character
ms.assetid: 7fdb7f38-b64d-48f2-bcb4-1ca96b2bbdb6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 78bbf45362216f8fb2a0706977347219ac6ac28b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203064"
---
# <a name="sql-to-c-character"></a>从 SQL 到 C：字符

字符 ODBC SQL 数据类型的标识符如下：

- SQL_CHAR
- SQL_VARCHAR
- SQL_LONGVARCHAR
- SQL_WCHAR
- SQL_WVARCHAR
- SQL_WLONGVARCHAR

下表显示了可将字符 SQL 数据转换为的 ODBC C 数据类型。 有关表中的列和字词的说明，请参阅将 [数据从 SQL 转换为 C 数据类型](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)。  

|C 类型标识符|测试|TargetValuePtr|StrLen_or_IndPtr|SQLSTATE|
|:----------------|:---|:-------------|:---------------|:-------|
|SQL_C_CHAR|数据 < 的字节长度 *BufferLength*<br /><br /> Data >的字节长度 = *BufferLength*|数据<br /><br /> 截断的数据|数据的长度（以字节为单位）<br /><br /> 数据的长度（以字节为单位）|不适用<br /><br /> 01004|  
|SQL_C_WCHAR|数据 < *BufferLength* 的字符长度<br /><br /> 数据长度 >= *BufferLength*|数据<br /><br /> 截断的数据|数据的长度（以字符为长度）<br /><br /> 数据的长度（以字符为长度）|不适用<br /><br /> 01004|  
|SQL_C_STINYINT SQL_C_UTINYINT SQL_C_TINYINT SQL_C_SBIGINT SQL_C_UBIGINT SQL_C_SSHORT SQL_C_USHORT SQL_C_SHORT SQL_C_SLONG SQL_C_ULONG SQL_C_LONG SQL_C_NUMERIC|转换数据时不截断 [b]<br /><br /> 用小数位截断转换的数据 [a]<br /><br /> 数据的转换会导致整个 (丢失，而不是) 数字的小数部分 [a]<br /><br /> 数据不是 *数字文本*[b]|数据<br /><br /> 截断的数据<br /><br /> Undefined<br /><br /> Undefined|C 数据类型的字节数<br /><br /> C 数据类型的字节数<br /><br /> Undefined<br /><br /> Undefined|不适用<br /><br /> 01S07<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_FLOAT SQL_C_DOUBLE|数据在要将数字转换到的数据类型的范围内 [a]<br /><br /> 数据在要将数字转换到的数据类型的范围之外 [a]<br /><br /> 数据不是 *数字文本*[b]|数据<br /><br /> Undefined<br /><br /> Undefined|C 数据类型的大小<br /><br /> Undefined<br /><br /> Undefined|不适用<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_BIT|数据为0或1<br /><br /> 数据大于0，小于2，并且不等于1<br /><br /> 数据小于0或大于等于2<br /><br /> 数据不是 *数字文本*|数据<br /><br /> 截断的数据<br /><br /> Undefined<br /><br /> Undefined|1 [b]<br /><br /> 1 [b]<br /><br /> Undefined<br /><br /> Undefined|不适用<br /><br /> 01S07<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_BINARY|Data <的字节长度 = *BufferLength*<br /><br /> 数据 > 的字节长度 *BufferLength*|数据<br /><br /> 截断的数据|数据的长度（以字节为单位）<br /><br /> 数据的长度|不适用<br /><br /> 01004|  
|SQL_C_TYPE_DATE|数据值为有效的 *日期-值*[a]<br /><br /> 数据值是有效的 *时间戳值*;时间部分为零 [a]<br /><br /> 数据值是有效的 *时间戳值*;时间部分为非零 [a]，[c]<br /><br /> 数据值不是有效的 *日期值* 或 *时间戳值*[a]|数据<br /><br /> 数据<br /><br /> 截断的数据<br /><br /> Undefined|6 [b]<br /><br /> 6 [b]<br /><br /> 6 [b]<br /><br /> Undefined|不适用<br /><br /> 不适用<br /><br /> 01S07<br /><br /> 22018|  
|SQL_C_TYPE_TIME|数据值是有效的 *时间值，秒的小数部分的值为 0*[a]<br /><br /> 数据值为有效的 *时间戳值或有效的时间值*;秒的小数部分为零 [a]，[d]<br /><br /> 数据值是有效的 *时间戳值*;秒的小数部分为非零 [a]，[d]，[e]<br /><br /> 数据值不是有效的 *时间值* 或 *时间戳值*[a]|数据<br /><br /> 数据<br /><br /> 截断的数据<br /><br /> Undefined|6 [b]<br /><br /> 6 [b]<br /><br /> 6 [b]<br /><br /> Undefined|不适用<br /><br /> 不适用<br /><br /> 01S07<br /><br /> 22018|  
|SQL_C_TYPE_TIMESTAMP|数据值为有效的 *时间戳值或有效的时间值*;不截断的秒小数部分 [a]<br /><br /> 数据值为有效的 *时间戳值或有效的时间值*;秒的小数部分截断 [a]<br /><br /> 数据值为有效的 *日期-值*[a]<br /><br /> 数据值是有效的 *时间值*[a]<br /><br /> 数据值不是有效的 *日期值*、 *时间值* 或 *时间戳值*[a]|数据<br /><br /> 截断的数据<br /><br /> 数据 [f]<br /><br /> 数据 [g]<br /><br /> Undefined|16 [b]<br /><br /> 16 [b]<br /><br /> 16 [b]<br /><br /> 16 [b]<br /><br /> Undefined|不适用<br /><br /> 01S07<br /><br /> 不适用<br /><br /> 不适用<br /><br /> 22018|  
|所有 C 间隔类型|数据值是有效的 *间隔值*;不截断<br /><br /> 数据值是有效的 *间隔值*;截断一个或多个尾随字段<br /><br /> 数据是有效的间隔;前导字段显著精度丢失<br /><br /> 数据值不是有效的间隔值|数据<br /><br /> 截断的数据<br /><br /> Undefined<br /><br /> Undefined|数据的长度（以字节为单位）<br /><br /> 数据的长度（以字节为单位）<br /><br /> Undefined<br /><br /> Undefined|不适用<br /><br /> 01S07<br /><br /> 22015<br /><br /> 22018|  
|&nbsp;|&nbsp;|&nbsp;|&nbsp;|&nbsp;|

 [a] 此转换将忽略 *BufferLength* 的值。 驱动程序假设大小 **TargetValuePtr* 是 C 数据类型的大小。  
  
 [b] 这是对应的 C 数据类型的大小。  
  
 [c] 时间 *戳值* 的时间部分被截断。  
  
 [d] 将忽略 *时间戳值* 的日期部分。  
  
 [e] 时间戳的秒的小数部分被截断。  
  
 [f] timestamp 结构的时间字段设置为零。  
  
 [g] 时间戳结构的日期字段设置为当前日期。  

**额外的空格**

当 SQL 字符数据转换为以下类型时，将忽略前导空格和尾随空格：

- date
- numeric
- time
- timestamp
- 间隔 C 数据
