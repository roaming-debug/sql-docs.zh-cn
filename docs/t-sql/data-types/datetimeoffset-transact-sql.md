---
description: datetimeoffset (Transact-SQL)
title: datetimeoffset (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetimeoffset_TSQL
- datetimeoffset
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- date and time [SQL Server], datetimeoffset
- datetimeoffset data type [SQL Server]
- dates [SQL Server], data types
- data types [SQL Server], date and time
- time zones [SQL Server]
ms.assetid: a0455b71-ca25-476e-a7a8-0770f1860bb7
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 76913dfeded63354a8597027380e7e966ff73f31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172577"
---
# <a name="datetimeoffset-transact-sql"></a>datetimeoffset (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

用于定义一个与采用 24 小时制并可识别时区的一日内时间相组合的日期。

## <a name="datetimeoffset-description"></a>datetimeoffset 说明
  
|属性|值|  
|---|---|
|语法|datetimeoffset [ (fractional seconds precision) ]|  
|使用情况|DECLARE \@MyDatetimeoffset datetimeoffset(7)<br /><br /> CREATE TABLE Table1 (Column1 datetimeoffset(7))|  
|默认字符串文字格式（用于下级客户端）|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [{+&#124;-}hh:mm]<br /><br /> 有关详细信息，请参阅后面的“下级客户端的向后兼容性”部分。|  
|日期范围|0001-01-01 到 31.12.99<br /><br /> 公元 1 年 1 月 1 日到公元 9999 年 12 月 31 日|  
|时间范围|00:00:00 到 23:59:59.9999999|  
|时区偏移量范围|-14:00 到 +14:00|  
|各元素的范围|YYYY 是表示年份的四位数字，范围为 0001 到 9999。<br /><br /> MM 是表示指定年份中的月份的两位数字，范围为 01 到 12。<br /><br /> DD 是表示指定月份中的某一天的两位数字，范围为 01 到 31（最高值取决于相应月份）。<br /><br /> hh 是表示小时的两位数字，范围为 00 到 23。<br /><br /> mm 是表示分钟的两位数字，范围为 00 到 59。<br /><br /> ss 是表示秒钟的两位数字，范围为 00 到 59。<br /><br /> n* 是 0 到 7 位数字，范围为 0 到 9999999，它表示秒的小数部分。<br /><br /> hh 是两位数，范围为 -14 到 +14。 <br /><br /> mm 是两位数，范围为 00 到 59。|  
|字符长度|最低 26 位 (YYYY-MM-DD hh:mm:ss {+&#124;-}hh:mm) 到最高 34 位 (YYYY-MM-DD hh:mm:ss.nnnnnnn {+&#124;-}hh:mm)|  
|精度、小数位数|请参阅下表。|  
|存储大小|默认值为 10 个字节的固定大小，默认的秒的小数部分精度为 100ns。|  
|精确度|100 纳秒|  
|默认值|1900-01-01 00:00:00 00:00|  
|日历|公历|  
|用户定义的秒的小数部分精度|是|  
|时区偏移量感知和保留|是|  
|夏时制感知|否|  
  
|指定的小数位数|结果 (精度, 小数位数)|列长度（以字节为单位）|秒的小数部分精度|  
|---|---|---|---|
|**datetimeoffset**|(34,7)|10|7|  
|**datetimeoffset(0)**|(26,0)|8|0-2|  
|**datetimeoffset(1)**|(28,1)|8|0-2|  
|**datetimeoffset(2)**|(29,2)|8|0-2|  
|**datetimeoffset(3)**|(30,3)|9|3-4|  
|**datetimeoffset(4)**|(31,4)|9|3-4|  
|**datetimeoffset(5)**|(32,5)|10|5-7|  
|**datetimeoffset(6)**|(33,6)|10|5-7|  
|**datetimeoffset(7)**|(34,7)|10|5-7|  
  
## <a name="supported-string-literal-formats-for-datetimeoffset"></a>datetimeoffset 支持的字符串文字格式
下表列出了 datetimeoffset 支持的 ISO 8601 字符串文字格式。 有关 datetimeoffset 日期和时间部分的字母、数值、未分隔的的字符串文字格式和时间格式的信息，请参阅 [date (Transact-SQL)](../../t-sql/data-types/date-transact-sql.md) 和 [time (Transact-SQL)](../../t-sql/data-types/time-transact-sql.md)。
  
|ISO 8601|说明|  
|---|---|
|YYYY-MM-DDThh:mm:ss[.nnnnnnn][{+&#124;-}hh:mm]|这两种格式不受 SET LANGUAGE 和 SET DATEFORMAT 会话的区域设置的影响。 datetimeoffset 和 datetime 部分之间不允许有空格。|  
|YYYY-MM-DDThh:mm:ss[.nnnnnnn]Z (UTC)|这种遵从 ISO 定义的格式表明 datetime 部分应采用协调世界时 (UTC) 表示。 例如，1999-12-12 12:30:30.12345 -07:00 应表示为 1999-12-12 19:30:30.12345Z。|  
  
## <a name="time-zone-offset"></a>时区偏移量
时区偏移量指定某个 time 或 datetime 值相对于 UTC 的时区偏移量。 时区偏移量可以表示为 [+|-] hh:mm：
-   hh 是两位数，范围为 00 到 14，表示时区偏移量中的小时数。  
-   mm 是两位数，范围为 00 到 59，表示时区偏移量中的额外分钟数。  
-   时区偏移量中必须包含 \+（加）或 -（减）号。 这两个符号表示是在 UTC 时间的基础上加上还是从中减去时区偏移量以得出本地时间。 时区偏移量的有效范围为 -14:00 到 +14:00。  
  
时区偏移量的范围遵循 XSD 架构定义的 W3C XML 标准，与 SQL 2003 标准定义（12:59 到 +14:00）略有不同。
  
可选的类型参数 fractional seconds precision 指定了秒小数部分的位数。 该值可以是一个 0 到 7（100 纳秒）的整数。 默认 fractional seconds precision 为 100ns（秒的小数部分有第 7 位）。
  
此数据存储在数据库中，并以与 UTC 相同的方式在服务器中进行处理、比较、排序和索引。 时区偏移量将保留在数据库中以供检索。
  
给定时区偏移量将假定为可以识别夏时制时间 (DST)，并会针对 DST 期间内的任何给定 datetime 进行调整。
  
对于 datetimeoffset 类型，在插入、更新、转换或赋值操作中将验证 UTC 和本地（相对于一致的或转换的时区偏移量）datetime 值。 如果检测到任何无效的 UTC 或本地（相对于一致的或转换的时区偏移量）datetime 值，将引发一个无效值错误。 例如，9999-12-31 10:10:00 在 UTC 中有效，但在本地时间中会溢出时区偏移量 +13:50。
  
若要将日期转换为目标时区中的相应 datetimeoffset 值，请参阅 [AT TIME ZONE (Transact SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)。
  
## <a name="ansi-and-iso-8601-compliance"></a>对 ANSI 和 ISO 8601 的遵从性  
[date](../../t-sql/data-types/date-transact-sql.md) 和 [time](../../t-sql/data-types/time-transact-sql.md) 主题的“对 ANSI 和 ISO 8601 的遵从性”部分也适用于 datetimeoffset。
  
## <a name="backward-compatibility-for-down-level-clients"></a>下级客户端的向后兼容性
某些下级客户端不支持 time、time、datetime2 和 datetimeoffset 数据类型。 下表显示了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上级实例与下级客户端之间的类型映射。
  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型|传递给下级客户端的默认字符串文字格式|下级 ODBC|下级 OLEDB|下级 JDBC|下级 SQLCLIENT|  
|---|---|---|---|---|---|
|**time**|hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
|**date**|YYYY-MM-DD|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
|**datetime2**|YYYY-MM-DD hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
|**datetimeoffset**|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
  
## <a name="converting-date-and-time-data"></a>转换日期和时间数据
当转换为日期和时间数据类型时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将会拒绝它无法识别为日期或时间的所有值。 有关日期和时间数据使用 CAST 和 CONVERT 函数的信息，请参阅 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
### <a name="converting-datetimeoffset-data-type-to-other-date-and-time-types"></a>将 datetimeoffset 数据类型转换为其他日期和时间类型
此部分介绍了在 datetimeoffset 数据类型转换为其他日期和时间数据类型时发生的具体情况。
  
转换成 date 时，会复制年、月和日。 下面的代码显示将 `datetimeoffset(4)` 值转换为 `date` 值的结果。  
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '12-10-25 12:32:10 +01:00';  
DECLARE @date date= @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset ', @date AS 'date';  
  
--Result  
--@datetimeoffset                date  
-------------------------------- ----------  
--2025-12-10 12:32:10.0000 +01:0 2025-12-10  
--  
--(1 row(s) affected)  
  
```  
  
如果转换成 time(n)，会复制小时、分钟、秒数和秒的小数部分。 时区值被截断。 当 datetimeoffset(n) 值的精度大于 time(n) 值的精度时，将对该值进行向上舍入。 下面的代码显示将 `datetimeoffset(4)` 值转换为 `time(3)` 值的结果。
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '12-10-25 12:32:10.1237 +01:0';  
DECLARE @time time(3) = @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset ', @time AS 'time';  
  
--Result  
--@datetimeoffset                time  
-------------------------------- ------------  
-- 2025-12-10 12:32:10.1237 +01:00    12:32:10.124  
  
--  
--(1 row(s) affected)  
  
```  
  
转换到 datetime 时，会复制日期和时间值，时区被截断。 当 datetimeoffset(n) 值的小数部分精度大于三位时，该值被截断。 下面的代码显示将 `datetimeoffset(4)` 值转换为 `datetime` 值的结果。
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '12-10-25 12:32:10.1237 +01:0';  
DECLARE @datetime datetime = @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset ', @datetime AS 'datetime';  
  
--Result  
--@datetimeoffset                datetime  
-------------------------------- -----------------------  
--2025-12-10 12:32:10.1237 +01:0 2025-12-10 12:32:10.123  
--  
--(1 row(s) affected)  
```  
  
转换成 smalldatetime 时，会复制日期和小时。 分钟数会根据秒值向上舍入，秒数设置为 0。 下面的代码显示将 `datetimeoffset(3)` 值转换为 `smalldatetime` 值的结果。  
  
```sql
DECLARE @datetimeoffset datetimeoffset(3) = '1912-10-25 12:24:32 +10:0';  
DECLARE @smalldatetime smalldatetime = @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset', @smalldatetime AS '@smalldatetime';  
  
--Result  
--@datetimeoffset                @smalldatetime  
-------------------------------- -----------------------  
--1912-10-25 12:24:32.000 +10:00 1912-10-25 12:25:00  
--  
--(1 row(s) affected)  
```  
  
如果转换成 datetime2(n)，日期和时间会复制到 datetime2 值，时区被截断。 当 datetime2(n) 值的精度大于 datetimeoffset(n) 值的精度时，秒的小数部分将被截断，以适合目标精度。 下面的代码显示将 `datetimeoffset(4)` 值转换为 `datetime2(3)` 值的结果。
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '1912-10-25 12:24:32.1277 +10:0';  
DECLARE @datetime2 datetime2(3)=@datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset', @datetime2 AS '@datetime2';  
  
--Result  
@datetimeoffset                    @datetime2  
---------------------------------- ----------------------  
1912-10-25 12:24:32.1277 +10:00    1912-10-25 12:24:32.12  
  
--(1 row(s) affected)  
```  
  
### <a name="converting-string-literals-to-datetimeoffset"></a>将字符串文字转换为 datetimeoffset
如果字符串所有部分的格式均有效，则允许从字符串文字转换为日期和时间类型。 否则，将引发运行时错误。 从日期和时间类型向字符串文字进行的未指定样式的隐式转换或显式转换将采用当前会话的默认格式。 下表显示用于将字符串文字转换为 datetimeoffset 数据类型的规则。
  
|输入字符串文字|**datetimeoffset(n)**|  
|---|---|
|ODBC DATE|ODBC 字符串文字映射到 datetime 数据类型。 从 ODBC DATETIME 文字到 datetimeoffset 类型的任何赋值操作都会导致在 datetime 与此类型之间按照转换规则的定义进行隐式转换。|  
|ODBC TIME|请参阅前面的 ODBC DATE 规则。|  
|ODBC DATETIME|请参阅前面的 ODBC DATE 规则。|  
|仅 DATE|TIME 部分默认为 00:00:00。 TIMEZONE 默认为 +00:00。|  
|仅 TIME|DATE 部分默认为 1900-1-1。 TIMEZONE 将默认为 +00:00。|  
|仅 TIMEZONE|提供默认值|  
|DATE + TIME|TIMEZONE 默认为 +00:00。|  
|DATE + TIMEZONE|不允许|  
|TIME + TIMEZONE|DATE 部分默认为 1900-1-1。|  
|DATE + TIME + TIMEZONE|无庸赘述|  
  
## <a name="examples"></a>示例  
下例比较了将一个字符串分别转换为各种 date 和 time 数据类型时所产生的结果 。
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29. 1234567 +12:15' AS time(7)) AS 'time'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS date) AS 'date'   
    ,CAST('2007-05-08 12:35:29.123' AS smalldatetime) AS   
        'smalldatetime'   
    ,CAST('2007-05-08 12:35:29.123' AS datetime) AS 'datetime'   
    ,CAST('2007-05-08 12:35:29.1234567+12:15' AS datetime2(7)) AS   
        'datetime2'  
    ,CAST('2007-05-08 12:35:29.1234567 +12:15' AS datetimeoffset(7)) AS   
        'datetimeoffset'  
    ,CAST('2007-05-08 12:35:29.1234567+12:15' AS datetimeoffset(7)) AS  
        'datetimeoffset IS08601';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|数据类型|输出|  
|---|---|
|**时间**|12:35:29. 1234567|  
|**日期**|2007-05-08|  
|**Smalldatetime**|2007-05-08 12:35:00|  
|**日期时间**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**Datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[AT TIME ZONE (Transact-SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)
  
  
