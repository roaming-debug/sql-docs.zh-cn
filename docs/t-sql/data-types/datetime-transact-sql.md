---
description: datetime (Transact-SQL)
title: datetime (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- datetime_TSQL
- datetime
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- date and time [SQL Server], datetime
- dates [SQL Server], data types
- datetime data type [SQL Server]
- data types [SQL Server], date and time
ms.assetid: 9bd1cc5b-227b-4032-95d6-7581ddcc9924
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 689a494e12f9b31f88d3ea060fe2c1b4a5fb2a3b
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126305"
---
# <a name="datetime-transact-sql"></a>datetime (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

用于定义一个与采用 24 小时制并带有秒小数部分的一日内时间相组合的日期。
  
> [!NOTE]  
>  为新的工作使用 time、date、datetime2 和 datetimeoffset 数据类型     。 这些类型符合 SQL 标准。 它们更易于移植。 time、datetime2 和 datetimeoffset 提供更高精度的秒数    。 datetimeoffset 为全局部署的应用程序提供时区支持  。  
  
## <a name="datetime-description"></a>datetime 说明  
  
|属性|值|  
|---|---|
|语法|**datetime**|  
|使用情况|DECLARE \@MyDatetime datetime<br /><br /> CREATE TABLE Table1 (Column1 datetime)|  
|默认的字符串文字格式<br /><br /> （用于下级客户端）|不适用|  
|日期范围|1753 年 1 月 1 日到 9999 年 12 月 31 日|  
|时间范围|00:00:00 到 23:59:59.997|  
|时区偏移量范围|无|  
|各元素的范围|YYYY 是表示年份的四位数字，范围为 1753 到 9999。<br /><br /> MM 是表示指定年份中的月份的两位数字，范围为 01 到 12。<br /><br /> DD 是表示指定月份中的某一天的两位数字，范围为 01 到 31（最高值取决于相应月份）。<br /><br /> hh 是表示小时的两位数字，范围为 00 到 23。<br /><br /> mm 是表示分钟的两位数字，范围为 00 到 59。<br /><br /> ss 是表示秒钟的两位数字，范围为 00 到 59。<br /><br /> n* 为一个 0 到 3 位的数字，范围为 0 到 999，表示秒的小数部分。|  
|字符长度|最低 19 位到最高 23 位|  
|存储大小|8 个字节|  
|精确度|舍入到 .000、.003 或 .007 秒三个增量。|  
|默认值|1900-01-01 00:00:00|  
|日历|公历（包括完整的年份范围。）|  
|用户定义的秒的小数部分精度|否|  
|时区偏移量感知和保留|否|  
|夏时制感知|否|  
  
## <a name="supported-string-literal-formats-for-datetime"></a>datetime 支持的字符串文字格式  
以下各表列出了 datetime 支持的字符串文字格式。 datetime 字符串文字位于单引号 (') 中，例如 'string_literaL'，但 ODBC 除外。 如果环境不是 us_english，则字符串文字应采用 N'string_literaL' 格式。
  
|Numeric|说明|  
|---|---|
|日期格式：<br /><br /> [0]4/15/[19]96 -- (mdy)<br /><br /> [0]4-15-[19]96 -- (mdy)<br /><br /> [0]4.15.[19]96 -- (mdy)<br /><br /> [0]4/[19]96/15 -- (myd)<br /><br /> 15/[0]4/[19]96 -- (dmy)<br /><br /> 15/[19]96/[0]4 -- (dym)<br /><br /> [19]96/15/[0]4 -- (ydm)<br /><br /> [19]96/[0]4/15 -- (ymd)<br /><br /> 时间格式：<br /><br /> 14:30<br /><br /> 14:30[:20:999]<br /><br /> 14:30[:20.9]<br /><br /> 4am<br /><br /> 4 PM|您可以指定日期数据，其中月份也通过数值指定。 例如 5/20/97 表示 1997 年 5 月 20 日。 当使用数值日期格式时，请在使用正斜杠符号 (/)、连字符 (-) 或句点 (.) 作为分隔符的字符串中指定年、月、日。 此字符串必须采用以下格式：<br /><br /> *数字 分隔符 数字 分隔符 数字 [时间] [时间]*<br /><br /> <br /><br /> 当语言设置为 us_english 时，默认的日期顺序是 mdy。 可以使用 [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) 语句更改日期顺序。<br /><br /> SET DATEFORMAT 的设置决定了如何解释日期值。 如果顺序不符合设置，这些值不会被解释为日期。 无序的日期可能会被错误解释成超出范围或使用错误的值。 例如，12/10/08 可以解释成六个不同的日期，具体解释为哪一日期取决于 DATEFORMAT 的设置。 四位数字的年份被解释为年。|  
  
|字母|说明|  
|---|---|
|Apr[il] [15][,] 1996<br /><br /> Apr[il] 15[,] [19]96<br /><br /> Apr[il] 1996 [15]<br /><br /> [15] Apr[il][,] 1996<br /><br /> 15 Apr[il][,][19]96<br /><br /> 15 [19]96 apr[il]<br /><br /> [15] 1996 apr[il]<br /><br /> 1996 APR[IL] [15]<br /><br /> 1996 [15] APR[IL]|您可以指定一个日期数据，其中使用完整的月份名称来指定月份。 例如，月份用英语 April 或使用其缩写 Apr 指定；逗号是可选的，且忽略大小写。<br /><br /> 下面是使用字母日期格式的一些准则：<br /><br /> 1) 日期和时间数据要放在单引号 (') 内。 对于英语以外的其他语言，使用 N'。<br /><br /> 2) 方括号中的字符是可选的。<br /><br /> 3) 如果只指定年份的最后两位数字，则小于[配置 two digit year cutoff（两位数年份截止）服务器配置选项](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md)配置选项值最后两位数字的值与截止年份处于同一个世纪。 大于或等于该选项值的值处于截止年份的上一个世纪。 例如，如果“两位数年份截止”为 2050（默认值），则两位数年份 25 被解释为 2025 年，而 50 被解释为 1950 年。 为避免不确定性，请使用四位数年份。<br /><br /> 4) 如果没有指定日，则默认为当月第一天。<br /><br /> <br /><br /> 当按字母形式指定月份时，SET DATEFORMAT 会话设置不起作用。|  
  
|ISO 8601|说明|  
|---|---|
|YYYY-MM-DDThh:mm:ss[.mmm]<br /><br /> YYYYMMDD[ hh:mm:ss[.mmm]]|示例：<br /><br /> 1) 2004-05-23T14:25:10<br /><br /> 2) 2004-05-23T14:25:10.487<br /><br /> <br /><br /> 若要使用 ISO 8601 格式，必须指定每个元素的格式，包括格式中显示的 T、冒号 (:)、句点 (.)。<br /><br /> 方括号表示秒小数部分是可选的。 时间部分按 24 小时制指定。<br /><br /> T 表示后面是 datetime 值的时间部分。<br /><br /> 使用 ISO 8601 格式的优点是它是一种国际标准，不会产生模糊的指定。 同时，此格式不受 SET DATEFORMAT 或 [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) 设置的影响。|  
  
|未分隔的|说明|  
|---|---|
|YYYYMMDD hh:mm:ss[.mmm]||  
  
|ODBC|说明|  
|---|---|
|{ ts '1998-05-02 01:23:56.123' }<br /><br /> { d '1990-10-02' }<br /><br /> { t '13:33:41' }|ODBC API 用于定义转义序列以表示日期和时间值，ODBC 称之为时间戳数据。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB provider for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所支持的 OLE DB 语言定义 (DBGUID-SQL) 也支持这种 ODBC 时间戳格式。 使用 ADO、OLE DB 和基于 ODBC 的 API 的应用程序可以使用这种 ODBC 时间戳格式来表示日期和时间。<br /><br /> ODBC 时间戳转义序列的格式为：{ *literal_type* '*constant_value*' }:<br /><br /> <br /><br /> - literal_type 指定转义序列的类型。 时间戳具有三个 literal_type 说明符：<br />1) d = 仅日期<br />2) t = 仅时间<br />3) ts = 时间戳（时间 + 日期）<br /><br /> <br /><br /> - 'constant_value' 是转义序列的值。 constant_value 必须遵循以下每种 literal_type 的格式。<br />d : yyyy-mm-dd<br />t : hh:mm:ss[.fff]<br />ts : yyyy-mm-dd hh:mm:ss[.fff]|  
  
## <a name="rounding-of-datetime-fractional-second-precision"></a>datetime 秒的小数部分精度的舍入  
如下表所示，将 datetime 值舍入到 .000、.003、或 .007 秒的增量。
  
|用户指定的值|系统存储的值|  
|---|---|
|01/01/98 23:59:59.999|1998-01-02 00:00:00.000|  
|01/01/98 23:59:59.995<br /><br /> 01/01/98 23:59:59.996<br /><br /> 01/01/98 23:59:59.997<br /><br /> 01/01/98 23:59:59.998|1998-01-01 23:59:59.997|  
|01/01/98 23:59:59.992<br /><br /> 01/01/98 23:59:59.993<br /><br /> 01/01/98 23:59:59.994|1998-01-01 23:59:59.993|  
|01/01/98 23:59:59.990<br /><br /> 01/01/98 23:59:59.991|1998-01-01 23:59:59.990|  
  
## <a name="ansi-and-iso-8601-compliance"></a>对 ANSI 和 ISO 8601 的遵从性  
datetime 不遵从 ANSI 或 ISO 8601。
  
##  <a name="converting-date-and-time-data"></a><a name="_datetime"></a> 转换日期和时间数据  
当转换为日期和时间数据类型时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将会拒绝它无法识别为日期或时间的所有值。 有关对日期和时间数据使用 CAST 和 CONVERT 函数的信息，请参阅 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)。
  
### <a name="converting-other-date-and-time-types-to-the-datetime-data-type"></a>将其他日期和时间类型转换为 datetime 数据类型 
本部分介绍其他日期和时间数据类型转换为 datetime 数据类型时会发生什么。  
  
从 date 转换时，会复制年、月和日。 时间部分设置为 00:00:00.000。 下面的代码显示将 `date` 值转换为 `datetime` 值的结果。  
  
```sql
DECLARE @date date = '12-21-16';  
DECLARE @datetime datetime = @date;  
  
SELECT @datetime AS '@datetime', @date AS '@date';  
  
--Result  
--@datetime               @date  
------------------------- ----------  
--2016-12-21 00:00:00.000 2016-12-21  
```  
  
从 time(n) 转换时，会复制时间部分，日期部分设置为“1900-01-01”。 当 time(n) 值的小数部分精度大于三位时，该值将被截断，以适合目标精度。 下面的示例显示了将 `time(4)` 值转换为 `datetime` 值的结果。  
  
```sql
DECLARE @time time(4) = '12:10:05.1237';  
DECLARE @datetime datetime = @time;  
  
SELECT @datetime AS '@datetime', @time AS '@time';  
  
--Result  
--@datetime               @time  
------------------------- -------------  
--1900-01-01 12:10:05.123 12:10:05.1237  
```  
  
从 smalldatetime 转换时，会复制小时和分钟。 秒和秒的小数部分设置为 0。 下面的代码显示将 `smalldatetime` 值转换为 `datetime` 值的结果。  
  
```sql
DECLARE @smalldatetime smalldatetime = '12-01-16 12:32';  
DECLARE @datetime datetime = @smalldatetime;  
  
SELECT @datetime AS '@datetime', @smalldatetime AS '@smalldatetime';  
  
--Result  
--@datetime               @smalldatetime  
------------------------- -----------------------  
--2016-12-01 12:32:00.000 2016-12-01 12:32:00  
```  
  
从 datetimeoffset(n) 转换时，会复制日期和时间部分。 时区被截断。 当 datetimeoffset(n) 值的小数部分精度大于三位时，该值将被截断。 下面的示例显示了将 `datetimeoffset(4)` 值转换为 `datetime` 值的结果。  
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '1968-10-23 12:45:37.1234 +10:0';  
DECLARE @datetime datetime = @datetimeoffset;  
  
SELECT @datetime AS '@datetime', @datetimeoffset AS '@datetimeoffset';  
  
--Result  
--@datetime               @datetimeoffset  
------------------------- ------------------------------  
--1968-10-23 12:45:37.123 1968-10-23 12:45:37.1237 +01:0   
```  
  
从 datetime2(n) 转换时，会复制日期和时间。 当 datetime2(n) 值的小数部分精度大于三位时，该值将被截断。 下面的示例显示了将 `datetime2(4)` 值转换为 `datetime` 值的结果。  
  
```sql
DECLARE @datetime2 datetime2(4) = '1968-10-23 12:45:37.1237';  
DECLARE @datetime datetime = @datetime2;  
  
SELECT @datetime AS '@datetime', @datetime2 AS '@datetime2';  
  
--Result  
--@datetime               @datetime2  
------------------------- ------------------------  
--1968-10-23 12:45:37.123 1968-10-23 12:45:37.1237  
```  
  
## <a name="examples"></a>示例  
下例比较了将一个字符串分别转换为各种 date 和 time 数据类型时所产生的结果 。
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29. 1234567 +12:15' AS time(7)) AS 'time'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS date) AS 'date'   
    ,CAST('2007-05-08 12:35:29.123' AS smalldatetime) AS   
        'smalldatetime'   
    ,CAST('2007-05-08 12:35:29.123' AS datetime) AS 'datetime'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS datetime2(7)) AS   
        'datetime2'  
    ,CAST('2007-05-08 12:35:29.1234567 +12:15' AS datetimeoffset(7)) AS   
        'datetimeoffset';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|数据类型|输出|  
|---|---|
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
