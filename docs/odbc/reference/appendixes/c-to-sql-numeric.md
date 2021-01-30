---
description: 从 C 到 SQL：数字
title: 从 C 到 SQL：数值 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- numeric data type [ODBC], converting
- data conversions from C to SQL types [ODBC], numeric
- converting data from c to SQL types [ODBC], numeric
ms.assetid: af4095ff-06c3-4b04-83bf-19f9ee098dc2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b825725ada434a72e14f9631504ee343951d6a4a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207821"
---
# <a name="c-to-sql-numeric"></a>从 C 到 SQL：数字
数字 ODBC C 数据类型的标识符为：  
  
 SQL_C_STINYINT  
  
 SQL_C_SLONG  
  
 SQL_C_UTINYINT  
  
 SQL_C_ULONG  
  
 SQL_C_TINYINT  
  
 SQL_C_LONG  
  
 SQL_C_SSHORT  
  
 SQL_C_FLOAT  
  
 SQL_C_USHORT  
  
 SQL_C_DOUBLE  
  
 SQL_C_SHORT  
  
 SQL_C_NUMERIC  
  
 SQL_C_SBIGINT  
  
 SQL_C_UBIGINT  
  
 下表显示了可转换数值 C 数据的 ODBC SQL 数据类型。 有关表中的列和字词的说明，请参阅 [将数据从 C 转换为 SQL 数据类型](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)。  
  
|SQL 类型标识符|测试|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|位数 <= 列字节长度<br /><br /> 列字节长度 > 位数|不适用<br /><br /> 22001|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|<字符数 = 列字符长度<br /><br /> 字符数 > 列字符长度|不适用<br /><br /> 22001|  
|SQL_DECIMAL [b]<br /><br /> SQL_NUMERIC [b]<br /><br /> SQL_TINYINT [b]<br /><br /> SQL_SMALLINT [b]<br /><br /> SQL_INTEGER [b]<br /><br /> SQL_BIGINT [b]|转换的数据没有截断或小数位数被截断<br /><br /> 用整位数截断转换的数据|不适用<br /><br /> 22003|  
|SQL_REAL<br /><br /> SQL_FLOAT<br /><br /> SQL_DOUBLE|数据在要将数字转换到的数据类型的范围内<br /><br /> 数据超出了数字要转换到的数据类型的范围|不适用<br /><br /> 22003|  
|SQL_BIT|数据为0或1<br /><br /> 数据大于0，小于2，并且不等于1<br /><br /> 数据小于0或大于等于2|不适用<br /><br /> 22001<br /><br /> 22003|  
|SQL_INTERVAL_YEAR [a]<br /><br /> SQL_INTERVAL_MONTH [a]<br /><br /> SQL_INTERVAL_DAY [a]<br /><br /> SQL_INTERVAL_HOUR [a]<br /><br /> SQL_INTERVAL_MINUTE [a]<br /><br /> SQL_INTERVAL_SECOND [a]|数据不会被截断。<br /><br /> 数据被截断。|不适用<br /><br /> 22015|  
  
 [a] 仅支持 (SQL_C_STINYINT、SQL_C_UTINYINT、SQL_C_SSHORT、SQL_C_USHORT、SQL_C_SLONG、SQL_C_ULONG 或 SQL_C_NUMERIC) 的精确数值数据类型的转换。 它们不支持近似数值数据类型 (SQL_C_FLOAT 或 SQL_C_DOUBLE) 。 不能将确切的数字 C 数据类型转换为其间隔精度不是单个字段的间隔 SQL 类型。  
  
 [b] 为 "n/a" 的情况，驱动程序可以选择在存在小数部分截断时返回 SQL_SUCCESS_WITH_INFO 和01S07。  
  
 驱动程序在转换数字 C 数据类型的数据时忽略长度/指示器值，并假定数据缓冲区的大小为数值 C 数据类型的大小。 长度/指示器值传入 **SQLPutData** 中的 *StrLen_or_Ind* 参数和在 **SQLBindParameter** 中通过 *StrLen_or_IndPtr* 参数指定的缓冲区中。 数据缓冲区是通过 **SQLPutData** 中的 *DataPtr* 参数和 **SQLBindParameter** 中的 *ParameterValuePtr* 参数指定的。
