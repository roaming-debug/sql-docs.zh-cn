---
description: 从 C 到 SQL：字符
title: C 到 SQL：字符 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- character data type [ODBC]
- data conversions from C to SQL types [ODBC], character
- converting data from c to SQL types [ODBC], character
ms.assetid: be66188a-ebdb-4c9e-af72-c379886766fa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c387c2063bea8f16c7ec5ec90aa0271830f3e5f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212353"
---
# <a name="c-to-sql-character"></a>从 C 到 SQL：字符
字符 ODBC C 数据类型的标识符为：  
  
 SQL_C_CHAR  
  
 SQL_C_WCHAR  
  
 下表显示了可转换 C 字符数据的 ODBC SQL 数据类型。 有关表中的列和字词的说明，请参阅 [将数据从 C 转换为 SQL 数据类型](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)。  
  
> [!NOTE]  
>  将字符 C 数据转换为 Unicode SQL 数据时，Unicode 数据的长度必须为偶数。  
  
|SQL 类型标识符|测试|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|数据 <的字节长度 = 列长度。<br /><br /> 数据 > 列长度的字节长度。|不适用<br /><br /> 22001|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|数据 <的字符长度 = 列长度。<br /><br /> 数据 > 列长度的字符长度。|不适用<br /><br /> 22001|  
|SQL_DECIMAL<br /><br /> SQL_NUMERIC<br /><br /> SQL_TINYINT<br /><br /> SQL_SMALLINT<br /><br /> SQL_INTEGER SQL_BIGINT|未截断转换的数据<br /><br /> 用小数位截断转换的数据 [e]<br /><br /> 数据的转换会导致整个 (丢失，而不是) 数字的小数部分 [e]<br /><br /> 数据值不是 *数字文本*|不适用<br /><br /> 22001<br /><br /> 22001<br /><br /> 22018|  
|SQL_REAL<br /><br /> SQL_FLOAT<br /><br /> SQL_DOUBLE|数据在要将数字转换到的数据类型的范围内<br /><br /> 数据超出了数字要转换到的数据类型的范围<br /><br /> 数据值不是 *数字文本*|不适用<br /><br /> 22003<br /><br /> 22018|  
|SQL_BIT|数据为0或1<br /><br /> 数据大于0，小于2，并且不等于1<br /><br /> 数据小于0或大于等于2<br /><br /> 数据不是 *数字文本*|不适用<br /><br /> 22001<br /><br /> 22003<br /><br /> 22018|  
|SQL_BINARY<br /><br /> SQL_VARBINARY<br /><br /> SQL_LONGVARBINARY|数据)  (字节长度 <= 列字节长度<br /><br /> 数据)  (字节长度 > 列字节长度<br /><br /> 数据值不是一个十六进制值|不适用<br /><br /> 22001<br /><br /> 22018|  
|SQL_TYPE_DATE|数据值是有效的 *ODBC 日期文本*<br /><br /> 数据值是有效的 *ODBC 时间戳文本*;时间部分为零<br /><br /> 数据值是有效的 *ODBC 时间戳文本*;时间部分为非零 [a]<br /><br /> 数据值不是有效的 *ODBC 日期文本* 或 *odbc 时间戳文本*|不适用<br /><br /> 不适用<br /><br /> 22008<br /><br /> 22018|  
|SQL_TYPE_TIME|数据值是有效的 *ODBC 时文本*<br /><br /> 数据值是有效的 *ODBC 时间戳文本*;秒的小数部分为零 [b]<br /><br /> 数据值是有效的 *ODBC 时间戳文本*;秒的小数部分为非零 [b]<br /><br /> 数据值不是有效的 *odbc 时文本* 或 *odbc 时间戳文本*|不适用<br /><br /> 不适用<br /><br /> 22008<br /><br /> 22018|  
|SQL_TYPE_TIMESTAMP|数据值是有效的 *ODBC 时间戳文本*;秒小数部分未截断<br /><br /> 数据值是有效的 *ODBC 时间戳文本*;秒的小数部分被截断<br /><br /> 数据值是有效的 *ODBC 日期文本*[c]<br /><br /> 数据值是有效的 *ODBC 时文本*[d]<br /><br /> 数据值不是有效的 *odbc 日期文本*、 *odbc 时* 文本或 *odbc 时间戳文本*|不适用<br /><br /> 22008<br /><br /> 不适用<br /><br /> 不适用<br /><br /> 22018|  
|所有 SQL 时间间隔类型|数据值是有效的 *间隔值*;不发生截断<br /><br /> 数据值是有效的 *间隔值*;其中一个字段的值被截断<br /><br /> 数据值不是有效的间隔文本|不适用<br /><br /> 22015<br /><br /> 22018|  
  
 [a] 时间戳的时间部分被截断。  
  
 [b] 忽略时间戳的日期部分。  
  
 [c] 时间戳的时间部分设置为零。  
  
 [d] 时间戳的日期部分设置为当前日期。  
  
 [e] 在尝试执行转换之前，驱动程序/数据源实际上会一直等待，直到收到整个字符串 (即使字符数据是通过调用 **SQLPutData**) 发送的。  
  
 将字符 C 数据转换为数字、日期、时间或时间戳 SQL 数据时，将忽略前导空格和尾随空格。  
  
 将字符 C 数据转换为二进制 SQL 数据时，字符数据的每个字节都转换为单个字节 (8 位) 二进制数据。 每两个字节的字符数据表示一个十六进制形式的数字。 例如，将 "01" 转换为二进制00000001，将 "FF" 转换为二进制11111111。  
  
 驱动程序始终将十六进制数字对转换为单个字节，并忽略 null 终止字节。 为此，如果字符串的长度为奇数，则字符串的最后一个字节 (不包括 null 终止字节，如果不转换任何) 。  
  
> [!NOTE]  
>  不建议应用程序开发人员将字符 C 数据绑定到二进制 SQL 数据类型。 这种转换通常效率低下且速度缓慢。
