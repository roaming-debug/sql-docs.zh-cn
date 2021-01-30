---
description: 从 SQL 到 C：日期时间间隔
title: SQL 到 C： Day-Time 间隔 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], day-time intervals
- day-time intervals [ODBC]
- data conversions from SQL to C types [ODBC], day-time intervals
- intervals [ODBC], converting
ms.assetid: 8ea84d69-2292-4128-89a0-f184f68abb98
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ae81fdf91c59457a6453958cb1c906abdd9d53fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203034"
---
# <a name="sql-to-c-day-time-intervals"></a>从 SQL 到 C：日期时间间隔

日期时间间隔 ODBC SQL 数据类型的标识符如下：

- SQL_INTERVAL_DAY
- SQL_INTERVAL_DAY_TO_MINUTE
- SQL_INTERVAL_HOUR
- SQL_INTERVAL_DAY_TO_SECOND
- SQL_INTERVAL_MINUTE
- SQL_INTERVAL_HOUR_TO_MINUTE
- SQL_INTERVAL_SECOND
- SQL_INTERVAL_HOUR_TO_SECOND
- SQL_INTERVAL_DAY_TO_HOUR
- SQL_INTERVAL_MINUTE_TO_SECOND

下表显示了可以将日期时间间隔转换到的 ODBC C 数据类型。 有关表中的列和字词的说明，请参阅将 [数据从 SQL 转换为 C 数据类型](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)。

|C 类型标识符|测试|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|所有日期时间 C 间隔类型|不截断尾部字段部分<br /><br /> 尾随字段部分已截断<br /><br /> 目标的前导精度不够大，无法保存源中的数据|数据<br /><br /> 截断的数据<br /><br /> Undefined|数据的长度<br /><br /> 数据的长度<br /><br /> Undefined|不适用<br /><br /> 01S07<br /><br /> 22015|  
|SQL_C_STINYINT [b] SQL_C_UTINYINT [b] SQL_C_USHORT [b] SQL_C_SHORT [b] SQL_C_SLONG [b] SQL_C_ULONG [b] SQL_C_NUMERIC [b] SQL_C_BIGINT [b]|间隔精度是单个字段，数据已转换但未截断<br /><br /> 间隔精度是单个字段，截断了小数部分<br /><br /> 间隔精度是单个字段并且截断了整个<br /><br /> 间隔精度不是单个字段|数据<br /><br /> 截断的数据<br /><br /> 截断的数据<br /><br /> Undefined|C 数据类型的大小<br /><br /> 数据的长度<br /><br /> 数据的长度<br /><br /> C 数据类型的大小|不适用<br /><br /> 01S07<br /><br /> 22003<br /><br /> 07006|  
|SQL_C_BINARY|Data <的字节长度 = *BufferLength*<br /><br /> 数据 > 的字节长度 *BufferLength*|数据<br /><br /> Undefined|数据的长度<br /><br /> Undefined|不适用<br /><br /> 22003|  
|SQL_C_CHAR|字符字节长度 < *BufferLength*<br /><br /> 整个 (的数目，相对于) 数字的小数位数 < *BufferLength*<br /><br /> 整个 (的数目，相对于小数) 位数 >= *BufferLength*|数据<br /><br /> 截断的数据<br /><br /> Undefined|C 数据类型的大小<br /><br /> C 数据类型的大小<br /><br /> Undefined|不适用<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|字符长度 < *BufferLength*<br /><br /> 整个 (的数目，相对于) 数字的小数位数 < *BufferLength*<br /><br /> 整个 (的数目，相对于小数) 位数 >= *BufferLength*|数据<br /><br /> 截断的数据<br /><br /> Undefined|C 数据类型的大小<br /><br /> C 数据类型的大小<br /><br /> Undefined|不适用<br /><br /> 01004<br /><br /> 22003|  
  
 [a] 日期-时间间隔 SQL 类型可以转换为任何日期时间间隔 C 类型。  
  
 [b] 如果间隔精度是 (DAY、HOUR、MINUTE 或 SECOND) 中的一个字段，则间隔 SQL 类型可以转换为任何精确数值 (SQL_C_STINYINT、SQL_C_UTINYINT、SQL_C_USHORT、SQL_C_SHORT、SQL_C_SLONG、SQL_C_ULONG 或 SQL_C_NUMERIC) 。  
  
间隔 SQL 类型的默认转换为相应的 C 间隔数据类型。 然后，应用程序绑定列或参数 (或将 ARD) 相应记录中的 SQL_DESC_DATA_PTR 字段设置为指向已初始化的 SQL_INTERVAL_STRUCT 结构 (或将指向 SQL_ INTERVAL_STRUCT 结构的指针作为 *TargetValuePtr* 参数传递到对 **SQLGetData**) 的调用中。  
  
下面的示例演示如何将 SQL_INTERVAL_DAY_TO_MINUTE 类型的列中的数据传输到 SQL_INTERVAL_STRUCT 结构，以使其返回 DAY_TO_HOUR 时间间隔。  

```cpp
SQL_INTERVAL_STRUCT is;  
SQLINTEGER    cbValue;  
SQLUINTEGER   days, hours;  
  
// Execute a select statement; "interval_column" is a column  
// whose data type is SQL_INTERVAL_DAY_TO_MINUTE.  
SQLExecDirect(hstmt, "SELECT interval_column FROM table", SQL_NTS);  
  
// Bind  
SQLBindCol(hstmt, 1, SQL_C_INTERVAL_DAY_TO_MINUTE, &is, sizeof(SQL_INTERVAL_STRUCT), &cbValue);  
  
// Fetch  
SQLFetch(hstmt);  
  
// Process data  
days = is.intval.day_second.day;  
hours = is.intval.day_second.hour;  
```
