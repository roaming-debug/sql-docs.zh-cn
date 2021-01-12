---
title: 日期和时间数据类型及函数
description: 提供指向日期和时间数据类型及函数文章的链接。
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 09/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- dates [SQL Server]
- date and time [SQL Server], all data types and functions
- date and time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: 83e378a2-6e89-4c80-bc4f-644958d9e0a9
author: cawrites
ms.author: chadam
monikerRange: = azure-sqldw-latest||= azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 461b4db55574fcad2fb5ff7250ce88f508999fa6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102604"
---
# <a name="date-and-time-data-types-and-functions-transact-sql"></a>日期和时间数据类型及函数 (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

本主题的各节涵盖了所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间数据类型及函数。

- [日期和时间数据类型](#DateandTimeDataTypes)
- [日期和时间函数](#DateandTimeFunctions)
  - [返回系统日期和时间值的函数](#GetSystemDateandTimeValues)
  - [返回日期和时间部分的函数](#GetDateandTimeParts)
  - [从相应部分返回日期和时间值的函数](#fromParts)
  - [返回日期和时间差异值的函数](#GetDateandTimeDifference)
  - [用于修改日期和时间值的函数](#ModifyDateandTimeValues)
  - [设置或返回会话格式函数的函数](#SetorGetSessionFormatFunctions)
  - [用于验证日期和时间值的函数](#ValidateDateandTimeValues)
- [日期和时间相关主题](#DateandTimeRelatedTopics)

## <a name="date-and-time-data-types"></a><a name="DateandTimeDataTypes"></a>日期和时间数据类型

下表列出了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的日期和时间数据类型：

|数据类型|格式|范围|精确度|存储大小（字节）|用户定义的秒的小数部分精度|时区偏移量|
|---|---|---|---|---|---|---|
|[time](../../t-sql/data-types/time-transact-sql.md)|hh:mm:ss[.nnnnnnn]|00:00:00.0000000 到 23:59:59.9999999|100 纳秒|3 到 5|是|否|
|[date](../../t-sql/data-types/date-transact-sql.md)|YYYY-MM-DD|0001-01-01 到 31.12.99|1 天|3|否|否|
|[smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md)|YYYY-MM-DD hh:mm:ss|1900-01-01 到 2079-06-06|1 分钟|4|否|否|
|[datetime](../../t-sql/data-types/datetime-transact-sql.md)|YYYY-MM-DD hh:mm:ss[.nnn]|1753-01-01 到 9999-12-31|0.00333 秒|8|否|否|
|[datetime2](../../t-sql/data-types/datetime2-transact-sql.md)|YYYY-MM-DD hh:mm:ss[.nnnnnnn]|0001-01-01 00:00:00.0000000 到 9999-12-31 23:59:59.9999999|100 纳秒|6 到 8|是|否|
|[datetimeoffset](../../t-sql/data-types/datetimeoffset-transact-sql.md)|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|0001-01-01 00:00:00.0000000 到 9999-12-31 23:59:59.9999999（以 UTC 时间表示）|100 纳秒|8 到 10|是|是|

> [!NOTE]
> [!INCLUDE[tsql](../../includes/tsql-md.md)] [rowversion](../../t-sql/data-types/rowversion-transact-sql.md) 数据类型不是日期或时间数据类型。 timestamp 是 rowversion 的已弃用同义词   。

## <a name="date-and-time-functions"></a><a name="DateandTimeFunctions"></a>日期和时间函数

以下各表列出了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间函数。 有关确定性的详细信息，请参阅[确定性函数和非确定性函数](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。

### <a name="function-that-return-system-date-and-time-values"></a><a name="GetSystemDateandTimeValues"></a>返回系统日期和时间值的函数

[!INCLUDE[tsql](../../includes/tsql-md.md)] 从运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机的操作系统派生所有系统日期和时间值。

#### <a name="higher-precision-system-date-and-time-functions"></a>精度较高的系统日期和时间函数

[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 使用 GetSystemTimeAsFileTime() Windows API 派生日期和时间值。 精确度取决于运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机硬件和 Windows 版本。 此 API 的精确度固定为 100 纳秒。 可使用 GetSystemTimeAdjustment() Windows API 确定该精确度。

|函数|语法|返回值|返回数据类型|确定性|
|---|---|---|---|---|
|[SYSDATETIME](../../t-sql/functions/sysdatetime-transact-sql.md)|SYSDATETIME ()|返回包含计算机的日期和时间的 datetime2(7) 值，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例在该计算机上运行  。 返回值不包括时区偏移量。|**datetime2(7)**|不具有确定性|
|[SYSDATETIMEOFFSET](../../t-sql/functions/sysdatetimeoffset-transact-sql.md)|SYSDATETIMEOFFSET ( )|返回包含计算机的日期和时间的 datetimeoffset(7) 值，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例在该计算机上运行  。 返回值包括时区偏移量。|**datetimeoffset(7)**|不具有确定性|
|[SYSUTCDATETIME](../../t-sql/functions/sysutcdatetime-transact-sql.md)|SYSUTCDATETIME ( )|返回包含计算机的日期和时间的 datetime2(7) 值，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例正在该计算机上运行  。 该函数返回日期和时间作为 UTC 时间（协调世界时）。|**datetime2(7)**|不具有确定性|

#### <a name="lower-precision-system-date-and-time-functions"></a>精度较低的系统日期和时间函数

|函数|语法|返回值|返回数据类型|确定性|
|---|---|---|---|---|
|[CURRENT_TIMESTAMP](../../t-sql/functions/current-timestamp-transact-sql.md)|CURRENT_TIMESTAMP|返回包含计算机的日期和时间的 datetime 值，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例在该计算机上运行  。 返回值不包括时区偏移量。|**datetime**|不具有确定性|
|[GETDATE](../../t-sql/functions/getdate-transact-sql.md)|GETDATE ( )|返回包含计算机的日期和时间的 datetime 值，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例在该计算机上运行  。 返回值不包括时区偏移量。|**datetime**|不具有确定性|
|[GETUTCDATE](../../t-sql/functions/getutcdate-transact-sql.md)|GETUTCDATE ( )|返回包含计算机的日期和时间的 datetime 值，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例在该计算机上运行  。 该函数返回日期和时间作为 UTC 时间（协调世界时）。|**datetime**|不具有确定性|

### <a name="functions-that-return-date-and-time-parts"></a><a name="GetDateandTimeParts"></a>返回日期和时间部分的函数

|函数|语法|返回值|返回数据类型|确定性|
|--------------|------------|------------------|----------------------|-----------------|
|[DATENAME](../../t-sql/functions/datename-transact-sql.md)|DATENAME ( datepart , date )  |返回表示指定 date 的指定 datepart 的字符串  。|**nvarchar**|不具有确定性|
|[DATEPART](../../t-sql/functions/datepart-transact-sql.md)|DATEPART ( datepart , date )  |返回表示指定 date 的指定 datepart 的整数   。|**int**|不具有确定性|
|[DAY](../../t-sql/functions/day-transact-sql.md)|DAY ( date ) |返回表示指定 date 的“日”部分的整数  。|**int**|具有确定性|
|[MONTH](../../t-sql/functions/month-transact-sql.md)|MONTH ( date ) |返回表示指定 date 的“月”部分的整数  。|**int**|具有确定性|
|[YEAR](../../t-sql/functions/year-transact-sql.md)|YEAR ( date ) |返回表示指定 date 的“年”部分的整数  。|**int**|具有确定性|

### <a name="functions-that-return-date-and-time-values-from-their-parts"></a><a name="fromParts"></a>从相应部分返回日期和时间值的函数

|函数|语法|返回值|返回数据类型|确定性|
|---|---|---|---|---|
|[DATEFROMPARTS](../../t-sql/functions/datefromparts-transact-sql.md)|DATEFROMPARTS ( year, month, day )   |返回表示指定年、月、日的 date 值  。|**date**|具有确定性|
|[DATETIME2FROMPARTS](../../t-sql/functions/datetime2fromparts-transact-sql.md)|DATETIME2FROMPARTS ( year, month, day, hour, minute, seconds, fractions, precision)        |对指定的日期和时间返回 datetime2 值（具有指定精度）  。|datetime2( precision )   |具有确定性|
|[DATETIMEFROMPARTS](../../t-sql/functions/datetimefromparts-transact-sql.md)|DATETIMEFROMPARTS ( year, month, day, hour, minute, seconds, milliseconds)       |为指定的日期和时间返回 datetime 值  。|**datetime**|具有确定性|
|[DATETIMEOFFSETFROMPARTS](../../t-sql/functions/datetimeoffsetfromparts-transact-sql.md)|DATETIMEOFFSETFROMPARTS ( year, month, day, hour, minute, seconds, fractions, hour_offset, minute_offset, precision)          |对指定的日期和时间返回 datetimeoffset 值（具有指定的偏移量和精度）  。|datetimeoffset( precision )   |具有确定性|
|[SMALLDATETIMEFROMPARTS](../../t-sql/functions/smalldatetimefromparts-transact-sql.md)|SMALLDATETIMEFROMPARTS ( year, month, day, hour, minute )     |为指定的日期和时间返回 smalldatetime 值  。|**smalldatetime**|具有确定性|
|[TIMEFROMPARTS](../../t-sql/functions/timefromparts-transact-sql.md)|TIMEFROMPARTS ( hour, minute, seconds, fractions, precision )     |对指定的时间返回 time 值（具有指定精度）  。|time( precision )   |具有确定性|

### <a name="functions-that-return-date-and-time-difference-values"></a><a name="GetDateandTimeDifference"></a>返回日期和时间差异值的函数

|函数|语法|返回值|返回数据类型|确定性|
|---|---|---|---|---|
|[DATEDIFF](../../t-sql/functions/datediff-transact-sql.md)|DATEDIFF ( datepart , startdate , enddate )   |返回两个指定日期之间所跨的日期或时间 datepart 边界数  。|**int**|具有确定性|
|[DATEDIFF_BIG](../../t-sql/functions/datediff-big-transact-sql.md)|DATEDIFF_BIG ( datepart , startdate , enddate )   |返回两个指定日期之间所跨的日期或时间 datepart 边界数  。|**bigint**|具有确定性|

### <a name="functions-that-modify-date-and-time-values"></a><a name="ModifyDateandTimeValues"></a>修改日期和时间值的函数

|函数|语法|返回值|返回数据类型|确定性|
|---|---|---|---|---|
|[DATEADD](../../t-sql/functions/dateadd-transact-sql.md)|DATEADD (datepart , number , date )   |通过将一个时间间隔与指定 date 的指定 datepart 相加，返回一个新的 datetime 值    。|date 参数的数据类型 |具有确定性|
|[EOMONTH](../../t-sql/functions/eomonth-transact-sql.md)|EOMONTH ( start_date [, month_to_add ] )  |返回包含指定日期的月份的最后一天（具有可选偏移量）。|返回类型为 start_date  参数类型或 date  数据类型。|具有确定性|
|[SWITCHOFFSET](../../t-sql/functions/switchoffset-transact-sql.md)|SWITCHOFFSET (DATETIMEOFFSET , time_zone)  |SWITCHOFFSET 更改 DATETIMEOFFSET 值的时区偏移量并保留 UTC 值。|具有 DATETIMEOFFSET 小数精度的 datetimeoffset  |具有确定性|
|[TODATETIMEOFFSET](../../t-sql/functions/todatetimeoffset-transact-sql.md)|TODATETIMEOFFSET (expression , time_zone)  |TODATETIMEOFFSET 将 datetime2 值转换为 datetimeoffset 值。 TODATETIMEOFFSET 针对指定 time_zone 解释本地时间的 datetime2 值  。|具有 datetime 参数小数精度的 datetimeoffset  |具有确定性|

### <a name="functions-that-set-or-return-session-format-functions"></a><a name="SetorGetSessionFormatFunctions"></a>设置或返回会话格式函数的函数

|函数|语法|返回值|返回数据类型|确定性|
|---|---|---|---|---|
|[@@DATEFIRST](../../t-sql/functions/datefirst-transact-sql.md)|@@DATEFIRST|返回对会话进行 SET DATEFIRST 操作所得结果的当前值。|**tinyint**|不具有确定性|
|[SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md)|SET DATEFIRST { number &#124; \@number_var }   |将一周的第一天设置为从 1 到 7 的一个数字。|不适用|不适用|
|[SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)|SET DATEFORMAT { format &#124; @format_var }   |设置用于输入 datetime 或 smalldatetime 数据的日期各部分（月/日/年）的顺序   。|不适用|不适用|
|[@@LANGUAGE](../../t-sql/functions/language-transact-sql.md)|@@LANGUAGE|返回当前所用语言的名称。 @@LANGUAGE 不是日期或时间函数。 但是，语言设置会影响日期函数的输出。|不适用|不适用|
|[SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md)|SET LANGUAGE { [ N ] 'language' &#124; \@language_var }     |设置会话和系统消息的语言环境。 SET LANGUAGE 不是日期或时间函数。 但是，语言设置会影响日期函数的输出。|不适用|不适用|
|[sp_helplanguage](../../relational-databases/system-stored-procedures/sp-helplanguage-transact-sql.md)|sp_helplanguage [ [ @language = ] 'language' ]     |返回有关所有支持语言的日期格式的信息。 sp_helplanguage 不是日期或时间存储过程  。 但是，语言设置会影响日期函数的输出。|不适用|不适用|

### <a name="functions-that-validate-date-and-time-values"></a><a name="ValidateDateandTimeValues"></a>验证日期和时间值的函数

|函数|语法|返回值|返回数据类型|确定性|
|---|---|---|---|---|
|[ISDATE](../../t-sql/functions/isdate-transact-sql.md)|ISDATE ( expression ) |确定 datetime 或 smalldatetime 输入表达式是否为有效的日期或时间值   。|**int**|在指定了 CONVERT 样式参数且样式不等于 0、100、9 或 109 时，ISDATE 只有在与 CONVERT 函数一起使用时才是确定的。|

## <a name="date-and-time-related-topics"></a><a name="DateandTimeRelatedTopics"></a>日期和时间相关主题

|主题|说明|
|-----------|-----------------|
|[FORMAT](../../t-sql/functions/format-transact-sql.md)|返回以指定的格式和可选的区域性格式化的值。 使用 FORMAT 函数将日期/时间和数字值格式化为识别区域设置的字符串。|
|[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)|提供有关在日期和时间值与字符串文字及其他日期和时间格式之间进行相互转换的信息。|
|[编写国际化 Transact-SQL 语句](../../relational-databases/collations/write-international-transact-sql-statements.md)|提供使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的数据库和数据库应用程序在不同语言之间的可移植性准则，或支持多种语言的数据库和数据库应用程序的可移植性准则。|
|[ODBC 标量函数 &#40;Transact-SQL&#41;](../../t-sql/functions/odbc-scalar-functions-transact-sql.md)|提供有关可在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中使用的 ODBC 标量函数的信息。 这包括 ODBC 日期和时间函数。|
|[AT TIME ZONE (Transact-SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)|提供时区转换。|

## <a name="see-also"></a>另请参阅

- [函数](../../t-sql/functions/functions.md)
- [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)
