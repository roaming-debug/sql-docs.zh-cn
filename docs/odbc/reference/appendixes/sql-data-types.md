---
description: SQL 数据类型
title: SQL 数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL data types [ODBC]
- SQL data types [ODBC], about SQL data types
- data types [ODBC], SQL data types
ms.assetid: 1b22f985-f5e4-4779-87eb-e43329a442b1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e15771f52af0830f07f135453d08f4355a3fae8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199865"
---
# <a name="sql-data-types"></a>SQL 数据类型
每个 DBMS 定义自己的 SQL 类型。 每个 ODBC 驱动程序只公开关联 DBMS 定义的 SQL 数据类型。 有关驱动程序如何将 DBMS SQL 类型映射到 ODBC 定义的 SQL 类型标识符以及驱动程序如何将 DBMS SQL 类型映射到其自己的特定于驱动程序的 SQL 类型标识符的信息是通过调用 **SQLGetTypeInfo** 返回的。 当通过调用 **SQLColAttribute**、 **SQLColumns**、 **SQLDescribeCol**、 **SQLDescribeParam**、 **SQLProcedureColumns** 和 **SQLSpecialColumns** 描述列和参数的数据类型时，驱动程序还会返回 SQL 数据类型。  
  
> [!NOTE]  
>  SQL 数据类型包含在实现描述符的 SQL_DESC_ CONCISE_TYPE、SQL_DESC_TYPE 和 SQL_DESC_DATETIME_INTERVAL_CODE 字段中。 SQL 数据类型的特性包含在实现描述符的 SQL_DESC_PRECISION、SQL_DESC_SCALE、SQL_DESC_LENGTH 和 SQL_DESC_OCTET_LENGTH 字段中。 有关详细信息，请参阅本附录后面的 [数据类型标识符和描述符](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) 。  
  
 给定的驱动程序和数据源不一定支持此附录中定义的所有 SQL 数据类型。 驱动程序对 SQL 数据类型的支持取决于驱动程序符合的 SQL-92 级别。 若要确定驱动程序支持的 SQL-92 语法级别，应用程序需要使用 SQL_SQL_CONFORMANCE 信息类型调用 **SQLGetInfo** 。 此外，给定的驱动程序和数据源可能支持其他驱动程序特定的 SQL 数据类型。 若要确定驱动程序支持的数据类型，应用程序将调用 **SQLGetTypeInfo**。 有关特定于驱动程序的 SQL 数据类型的信息，请参阅驱动程序的文档。 有关特定数据源中的数据类型的信息，请参阅该数据源的文档。  
  
> [!IMPORTANT]  
>  整个附录中的表只是指导原则，并显示 SQL 数据类型的常用名称、范围和限制。 给定的数据源可能仅支持某些列出的数据类型，并且支持的数据类型的特征可能与列出的数据类型不同。  
  
 下表列出了所有 SQL 数据类型的有效 SQL 类型标识符。 该表还列出了 SQL-92 (中对应数据类型的名称和说明（如果) 存在该数据类型）。  
  
|SQL 类型标识符 [1]|典型的 SQL 数据<br /><br /> 类型 [2]|典型类型说明|  
|------------------------------|------------------------------------|------------------------------|  
|SQL_CHAR|CHAR (*n*) |固定字符串长度为 *n* 的字符串。|  
|SQL_VARCHAR|VARCHAR (*n*) |长度可变的字符串，最大字符串长度为 *n*。|  
|SQL_LONGVARCHAR|LONG VARCHAR|变长字符数据。 最大长度取决于数据源。900|  
|SQL_WCHAR|WCHAR (*n*) |固定字符串长度为 *n* 的 Unicode 字符串|  
|SQL_WVARCHAR|OLEDBTYPE.VARWCHAR (*n*) |Unicode 可变长度字符串，最大字符串长度为 *n*|  
|SQL_WLONGVARCHAR|LONGWVARCHAR|Unicode 变长字符数据。 最大长度取决于数据源|  
|SQL_DECIMAL|DECIMAL (*p*，*s*) |带符号的精确数字值，精确到 *p* 和小数位数 *。*  (最大精度为驱动程序定义的。 )  (1 <= *p* <= 15;*s*  <= *p*) 。4|  
|SQL_NUMERIC|数值 (*p*，*s*) |具有精度 *p* 且小数位数为 *s* 的有符号的精确数值 (1 <= *p* <= 15;*s*  <= *p*) 。4|  
|SQL_SMALLINT|SMALLINT|精度为5且小数位数为0的精确数值 (带符号：-32768 <= *n* <= 32767，无符号： 0 <= *n* <= 65535) [3]。|  
|SQL_INTEGER|INTEGER|精度为10、小数位数为0的精确数值 (已签名：-2 [31] <= *n* <= 2 [31]-1，无符号： 0 <= *n* <= 2 [32]-1) [3]。|  
|SQL_REAL|real|带二进制精度 24 (零或绝对值为 10 [-38] 到 10 [38] ) 的已签名的近似值。|  
|SQL_FLOAT|FLOAT (*p*) |带符号的近似数值，其二进制精度至少为 *p*。  (最大精度为驱动程序定义的值。 ) [5]|  
|SQL_DOUBLE|DOUBLE PRECISION|带有二进制精度 53 (零或绝对值为 10 [-308] 到 10 [308] ) 的已签名的近似值。|  
|SQL_BIT|BIT|单比特二进制数据。8|  
|SQL_TINYINT|TINYINT|精度为3且小数位数为0的精确数值 (带符号：-128 <= *n* <= 127，无符号： 0 <= *n* <= 255) [3]。|  
|SQL_BIGINT|BIGINT|精确数值，精度为 19 (如果有符号的) 或 20 (，如果签名的) 和小数位数为 0 (签名：-2 [63] <= *n* <= 2 [63]-1，无符号： 0 <= *n* <= 2 [64]-1) [3]，[9]。|  
|SQL_BINARY|二进制 (*n*) |固定长度为 *n* 的二进制数据。900|  
|SQL_VARBINARY|VARBINARY (*n*) |长度可变的二进制数据，最大长度为 *n*。 最大值由用户设置。900|  
|SQL_LONGVARBINARY|长 VARBINARY|变长二进制数据。 最大长度取决于数据源。900|  
|SQL_TYPE_DATE [6]|DATE|Year、month 和 day 字段，符合公历规则。  (参阅本附录后面的 [公历的约束](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md)。 ) |  
|SQL_TYPE_TIME [6]| (*p*) 时间|小时、分钟和秒字段，值为00到23，分钟的有效值为00到59，值为00到61的有效值。 Precision *p* 指示秒精度。|  
|SQL_TYPE_TIMESTAMP [6]|TIMESTAMP (*p*) |Year、month、day、hour、minute 和 second 字段，其有效值为日期和时间数据类型的定义。|  
|SQL_TYPE_UTCDATETIME|DATETIMEOFFSET.UTCDATETIME|Year、month、day、hour、minute、second、utchour 和 utcminute 字段。 Utchour 和 utcminute 字段的精度为1/10 微秒。|  
|SQL_TYPE_UTCTIME|UTCTIME|Hour、minute、second、utchour 和 utcminute 字段。 Utchour 和 utcminute 字段的精度为1/10 微秒。|  
|SQL_INTERVAL_MONTH [7]|间隔月份 (*p*) |两个日期之间的月数; *p* 为间隔的起始精度。|  
|SQL_INTERVAL_YEAR [7]|间隔年份 (*p*) |两个日期之间的年数; *p* 为间隔的起始精度。|  
|SQL_INTERVAL_YEAR_TO_MONTH [7]|每月 (*p*) 的间隔年份|两个日期之间的年和月数; *p* 为间隔的起始精度。|  
|SQL_INTERVAL_DAY [7]|间隔日期 (*p*) |两个日期之间的天数; *p* 为间隔的起始精度。|  
|SQL_INTERVAL_HOUR [7]|间隔小时 (*p*) |两个日期/时间间隔的小时数; *p* 为间隔的起始精度。|  
|SQL_INTERVAL_MINUTE [7]|间隔分钟 (*p*) |两个日期/时间间隔的分钟数; *p* 为间隔的起始精度。|  
|SQL_INTERVAL_SECOND [7]|间隔秒 (*p*，*q*) |两个日期/时间之间的秒数; *p* 是时间间隔的精度，而 *q* 为间隔秒精度。|  
|SQL_INTERVAL_DAY_TO_HOUR [7]|间隔日期 (*p*) 到小时|两个日期/时间之间的天数/小时数; *p* 为间隔的起始精度。|  
|SQL_INTERVAL_DAY_TO_MINUTE [7]|间隔日期 (*p*) 到分钟|两个日期/时间之间的天数/小时/分钟数; *p* 为间隔的起始精度。|  
|SQL_INTERVAL_DAY_TO_SECOND [7]|间隔日期 (*p*) 第二 (*q*) |两个日期/时间之间的天数/小时/分钟/秒 *p* 是时间间隔的精度，而 *q* 为间隔秒精度。|  
|SQL_INTERVAL_HOUR_TO_MINUTE [7]|间隔小时 (*p*) 到分钟|两个日期/时间之间的小时/分钟数; *p* 为间隔的起始精度。|  
|SQL_INTERVAL_HOUR_TO_SECOND [7]|间隔小时 (*p*) 到第二 (*q*) |两个日期/时间之间的小时/分钟数/秒 *p* 是时间间隔的精度，而 *q* 为间隔秒精度。|  
|SQL_INTERVAL_MINUTE_TO_SECOND [7]|间隔分钟 (*p*) 第二 (*q*) |两个日期/时间间隔的分钟数/秒; *p* 是时间间隔的精度，而 *q* 为间隔秒精度。|  
|SQL_GUID|GUID|固定长度 GUID。|  
  
 [1] 这是通过调用 **SQLGetTypeInfo** 在 DATA_TYPE 列中返回的值。  
  
 [2] 这是在 "名称" 和 "参数" 列中通过调用 **SQLGetTypeInfo** 而返回的值。 NAME 列返回指定的名称（例如 CHAR），而 CREATE PARAMS 列返回以逗号分隔的创建参数列表，如精度、小数位数和长度。  
  
 [3] 应用程序使用 **SQLGetTypeInfo** 或 **SQLColAttribute** 来确定某一特定数据类型或结果集中的特定列是否无符号。  
  
 [4] SQL_DECIMAL 和 SQL_NUMERIC 数据类型的差异仅在于它们的精度。 DECIMAL (*p*，*s*) 的精度是实现定义的小数精度，它不小于 *p*，而数值 (*p*，*s*) 的精度完全等于 *p*。  
  
 [5] 根据实现，SQL_FLOAT 的精度可以是24或53：如果为24，则 SQL_FLOAT 数据类型与 SQL_REAL 相同;如果为53，则 SQL_FLOAT 的数据类型与 SQL_DOUBLE 相同。  
  
 [6] *在 ODBC 3.x 中，* SQL date、time 和 timestamp 数据类型分别分别为 SQL_TYPE_DATE、SQL_TYPE_TIME 和 SQL_TYPE_TIMESTAMP。在 ODBC *2.x 中，* 数据类型为 SQL_DATE、SQL_TIME 和 SQL_TIMESTAMP。  
  
 [7] 有关间隔 SQL 数据类型的详细信息，请参阅本附录后面的 " [间隔数据类型](../../../odbc/reference/appendixes/interval-data-types.md) " 部分。  
  
 [8] SQL_BIT 的数据类型与 SQL-92 中的位类型具有不同的特征。  
  
 [9] 此数据类型在 SQL-92 中没有相应的数据类型。  
  
 本部分提供以下示例。  
  
-   [示例 SQLGetTypeInfo 结果集](../../../odbc/reference/appendixes/example-sqlgettypeinfo-result-set.md)
