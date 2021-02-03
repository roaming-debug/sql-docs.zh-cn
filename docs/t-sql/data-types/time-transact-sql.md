---
description: time (Transact-SQL)
title: time (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- time_TSQL
- time
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- time [SQL Server]
- date and time [SQL Server], time
- data types [SQL Server], date and time
- time data type [SQL Server]
ms.assetid: 30a6c681-8190-48e4-94d0-78182290a402
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7425622f808eccb983b73670b57ec3295de15fb3
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237344"
---
# <a name="time-transact-sql"></a>time (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  定义一天中的某个时间。 此时间不能感知时区且基于 24 小时制。  
  
  > [!NOTE]  
  > 使用 Informatica 连接器为 PDW 客户提供 Informatica 信息。 
  
## <a name="time-description"></a>time 说明  
  
|属性|值|  
|--------------|-----------|  
|语法|time [ (fractional second scale) ]|  
|使用情况|DECLARE \@MyTime time(7)<br /><br /> CREATE TABLE Table1 ( Column1 time(7))|  
|fractional seconds scale|为秒的小数部分指定数字的位数。<br /><br /> 这可以是从 0 到 7 的整数。 对于 Informatica，这可以是从 0 到 3 的整数。<br /><br /> 默认小数位数为 7 (100ns)。|  
|默认的字符串文字格式<br /><br /> （用于下级客户端）|对于 Informatica，为 hh:mm:ss[.nnnnnnn]）<br /><br /> 有关详细信息，请参阅[下级客户端的后向兼容性](#BackwardCompatibilityforDownlevelClients)部分。|  
|范围|00:00:00.0000000 到 23:59:59.9999999（对于 Informatica，为 00:00:00.000 到 23:59:59.999）|  
|各元素的范围|hh 是表示小时的两位数字，范围为 0 到 23。<br /><br /> mm 是表示分钟的两位数字，范围为 0 到 59。<br /><br /> ss 是表示秒的两位数字，范围为 0 到 59。<br /><br /> n\* 是 0 到 7 位数字，范围为 0 到 9999999，它表示秒的小数部分。 对于 Informatica，n\* 是零到三位数字，范围为 0 到 999。|  
|字符长度|最小 8 位 (hh:mm:ss)，最大 16 位 (hh:mm:ss.nnnnnnn)。 对于 Informatica，最大值为 12 位 (hh:mm:ss.nnn)。|  
|精度、小数位数<br /><br /> （用户只能指定小数位数）|请参阅下表。|  
|存储大小|固定 5 个字节，是使用默认的 100ns 秒的小数部分精度时的默认存储大小。 在 Informatica 中，默认为 4 个字节，固定不变，同时秒的小数部分精度默认为 1 毫秒。|  
|精确度|100 纳秒（Informatica 中为 1 毫秒）|  
|默认值|00:00:00<br /><br /> 此值用作从 date 隐式转换到datetime2 或 datetimeoffset 时追加的时间部分。|  
|用户定义的秒的小数部分精度|是|  
|时区偏移量感知和保留|否|  
|夏时制感知|否|  
  
|指定的小数位数|结果 (精度, 小数位数)|列长度（以字节为单位）|小数<br /><br /> seconds<br /><br /> 精准率|  
|---------------------|---------------------------------|-----------------------------|------------------------------------------|  
|**time**|(16,7)[Informatica 中为 (12,3)]|5（Informatica 中为 4）|7（Informatica 中为 3）|  
|**time(0)**|(8,0)|3|0-2|  
|**time(1)**|(10,1)|3|0-2|  
|**time(2)**|(11,2)|3|0-2|  
|**time(3)**|(12,3)|4|3-4|  
|**time(4)**<br /><br /> Informatica 中不支持。|(13,4)|4|3-4|  
|**time(5)**<br /><br /> Informatica 中不支持。|(14,5)|5|5-7|  
|**time(6)**<br /><br /> Informatica 中不支持。|(15,6)|5|5-7|  
|**time(7)**<br /><br /> Informatica 中不支持。|(16,7)|5|5-7|  
  
## <a name="supported-string-literal-formats-for-time"></a>time 支持的字符串文字格式  
 下表显示的是适用于 time 数据类型的有效字符串文字格式。  
  
|SQL Server|描述|  
|----------------|-----------------|  
|hh:mm[:ss][:fractional seconds][AM][PM]<br /><br /> hh:mm[:ss][.fractional seconds][AM][PM]<br /><br /> hhAM[PM]<br /><br /> hh AM[PM]|如果小时值为 0，则不论是否指定了 AM，都表示午夜 (AM) 后的小时。 当小时值等于 0 时，不能指定 PM。<br /><br /> 如果 AM 和 PM 均未指定，则小时值为 01 到 11 时，表示中午以前的小时。 如果指定了 AM，则这些值表示中午以前的小时。 如果指定了 PM，则这些值表示中午以后的小时。<br /><br /> 如果既未指定 AM，也未指定 PM，则小时值 12 表示始于中午的小时。 如果指定了 AM，则该值表示始于午夜的小时。 如果指定了 PM，则该值表示始于中午的小时。 例如：12:01 是指中午过后 1 分钟，与 12:01 PM 的含义相同，而 12:01 AM 则指午夜过后 1 分钟。 指定 12:01 AM 与指定 00:01 或 00:01 AM 等效。<br /><br /> 如果未指定 AM 或 PM，则小时值 13 到 23 表示中午以后的小时。 如果指定了 PM，这些值也表示中午以后的小时。 如果小时值为 13 到 23，不能指定 AM。<br /><br /> 如果小时值为 24，则该值无效。 若要表示午夜，请使用 12:00 AM 或 00:00。<br /><br /> 可以在毫秒之前加上冒号 (:) 或者句点 (.)。 如果使用冒号，这个数字表示千分之一秒。 如果使用句点，则单个数字表示十分之一秒，两个数字表示百分之一秒，三个数字表示千分之一秒。 例如，12:30:20:1 表示到了 12:30 后又过了二十又千分之一秒；12:30:20.1 表示到了 12:30 后又过了二十又十分之一秒。|  
  
|ISO 8601|备注|  
|--------------|-----------|  
|hh:mm:ss<br /><br /> hh:mm[:ss][.fractional seconds]|hh 是两位数，介于 0 和 23 范围之间，表示时区偏移小时数。<br /><br /> mm 是两位数，范围为 0 到 59，它表示时区偏移量中的额外分钟数。|  
  
|ODBC|备注|  
|----------|-----------|  
|{t 'hh:mm:ss[.fractional seconds]'}|特定于 ODBC API。|  
  
## <a name="compliance-with-ansi-and-iso-8601-standards"></a>对 ANSI 和 ISO 8601 标准的遵从性  
 为满足向后兼容的需要以及与现有日期和时间类型保持一致，不支持 ISO 8601（5.3.2 和 5.3）规定的如下用法：用 24 点表示午夜和允许大于 59 的闰秒。  
  
 默认字符串文字格式（用于下级客户端）将遵照 SQL 标准格式（定义为 hh:mm:ss[.nnnnnnn]）。 这种格式类似于 ISO 8601 对不包含秒小数部分的 TIME 的定义。  
  
##  <a name="backward-compatibility-for-down-level-clients"></a><a name="BackwardCompatibilityforDownlevelClients"></a> 下级客户端的向后兼容性  
 某些下级客户端不支持 time、time、datetime2 和 datetimeoffset 数据类型。 下表显示了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上级实例与下级客户端之间的类型映射。  
  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型|传递给下级客户端的默认字符串文字格式|下级 ODBC|下级 OLEDB|下级 JDBC|下级 SQLCLIENT|  
|-----------------------------------------|----------------------------------------------------------------|----------------------|-----------------------|----------------------|---------------------------|  
|**time**|hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
|**date**|YYYY-MM-DD|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
|**datetime2**|YYYY-MM-DD hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
|**datetimeoffset**|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
  
## <a name="converting-date-and-time-data"></a>转换日期和时间数据  
 当转换为日期和时间数据类型时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将会拒绝它无法识别为日期或时间的所有值。 有关日期和时间数据使用 CAST 和 CONVERT 函数的信息，请参阅 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
### <a name="converting-timen-data-type-to-other-date-and-time-types"></a>将 time(n) 数据类型转换为其他日期和时间类型  
 本部分介绍当 time 数据类型转换为其他日期和时间数据类型时发生的情况。  
  
 转换到 time(n) 时，会复制小时、分钟和秒数。 当目标精度小于源精度时，将对秒的小数部分进行向上舍入，以适合目标精度。 下面的示例显示了将 `time(4)` 值转换为 `time(3)` 值的结果。  
  
```sql
DECLARE @timeFrom time(4) = '12:34:54.1237';  
DECLARE @timeTo time(3) = @timeFrom;  
  
SELECT @timeTo AS 'time(3)', @timeFrom AS 'time(4)';  
  
--Results  
--time(3)      time(4)  
-------------- -------------  
--12:34:54.124 12:34:54.1237  
--  
--(1 row(s) affected)  
```  
  
 如果转换到“date”，转换失败，并引发错误消息 206：“操作数类型冲突: date 与 time 不兼容”。  
  
 转换到 datetime 时，会复制小时、分钟和秒数，且日期部分设为“1900-01-01”。 当 time(n) 值的秒的小数部分精度大于三位时，datetime 结果将被截断。 下面的代码显示将 `time(4)` 值转换为 `datetime` 值的结果。  
  
```sql
DECLARE @time time(4) = '12:15:04.1237';  
DECLARE @datetime datetime= @time;  
SELECT @time AS '@time', @datetime AS '@datetime';  
  
--Result  
--@time         @datetime  
--------------- -----------------------  
--12:15:04.1237 1900-01-01 12:15:04.123  
--  
--(1 row(s) affected)  
  
```  
  
 转换到 smalldatetime 时，日期设置为“1900-01-01”，小时和分钟值向上舍入。 秒和秒的小数部分设置为 0。 下面的代码显示将 `time(4)` 值转换为 `smalldatetime` 值的结果。  
  
```sql
-- Shows rounding up of the minute value.  
DECLARE @time time(4) = '12:15:59.9999';   
DECLARE @smalldatetime smalldatetime= @time;    
SELECT @time AS '@time', @smalldatetime AS '@smalldatetime';   
  
--Result  
@time            @smalldatetime  
---------------- -----------------------  
12:15:59.9999    1900-01-01 12:16:00--  
--(1 row(s) affected)  
  
-- Shows rounding up of the hour value.  
DECLARE @time time(4) = '12:59:59.9999';   
DECLARE @smalldatetime smalldatetime= @time;    
  
SELECT @time AS '@time', @smalldatetime AS '@smalldatetime';  
@time            @smalldatetime  
---------------- -----------------------  
12:59:59.9999    1900-01-01 13:00:00  
  
(1 row(s) affected)  
  
```  
  
 如果转换到 datetimeoffset(n)，则日期设置为“1900-01-01”，且复制时间。 时区偏移量设置为 +00:00。 当 time(n) 值秒的小数部分精度大于 datetimeoffset(n) 值的精度时，将对值进行向上舍入以适合精度。 下面的示例显示将 `time(4)` 值转换为 `datetimeoffset(3)` 类型的结果。  
  
```sql
DECLARE @time time(4) = '12:15:04.1237';  
DECLARE @datetimeoffset datetimeoffset(3) = @time;  
  
SELECT @time AS '@time', @datetimeoffset AS '@datetimeoffset';  
  
--Result  
--@time         @datetimeoffset  
--------------- ------------------------------  
--12:15:04.1237 1900-01-01 12:15:04.124 +00:00  
--  
--(1 row(s) affected)  
  
```  
  
 转换到 datetime2(n) 时，日期设置为“1900-01-01”，复制时间部分，时区偏移量设置为 00:00。 当 datetime2(n) 值秒的小数部分精度大于 time(n) 值时，将对值进行向上舍入以适合精度。  下面的示例显示了将 `time(4)` 值转换为 `datetime2(2)` 值的结果。  
  
```sql
DECLARE @time time(4) = '12:15:04.1237';  
DECLARE @datetime2 datetime2(3) = @time;  
  
SELECT @datetime2 AS '@datetime2', @time AS '@time';  
  
--Result  
--@datetime2              @time  
------------------------- -------------  
--1900-01-01 12:15:04.124 12:15:04.1237  
--  
--(1 row(s) affected)  
```  
  
### <a name="converting-string-literals-to-timen"></a>将字符串文字转换为 time(n)  
 如果字符串所有部分的格式均有效，则允许从字符串文字转换为日期和时间类型。 否则，将引发运行时错误。 从日期和时间类型向字符串文字进行的未指定样式的隐式转换或显式转换将采用当前会话的默认格式。 下表显示用于将字符串文字转换为 time 数据类型的规则。  
  
|输入字符串文字|转换规则|  
|--------------------------|---------------------|  
|ODBC DATE|ODBC 字符串文字映射到 datetime 数据类型。 从 ODBC DATETIME 文字到 time 类型的任何赋值操作都会导致在 datetime 与此类型之间按照转换规则的定义进行隐式转换。|  
|ODBC TIME|请参阅上面的 ODBC DATE 规则。|  
|ODBC DATETIME|请参阅上面的 ODBC DATE 规则。|  
|仅 DATE|提供默认值。|  
|仅 TIME|无庸赘述|  
|仅 TIMEZONE|提供默认值。|  
|DATE + TIME|使用输入字符串的 TIME 部分。|  
|DATE + TIMEZONE|不允许。|  
|TIME + TIMEZONE|使用输入字符串的 TIME 部分。|  
|DATE + TIME + TIMEZONE|将使用本地 DATETIME 的 TIME 部分。|  
  
## <a name="examples"></a>示例  
  
### <a name="a-comparing-date-and-time-data-types"></a>A. 比较日期和时间数据类型  
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
  
|数据类型|输出|  
|---------------|------------|  
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
###  <a name="b-inserting-valid-time-string-literals-into-a-time7-column"></a><a name="ExampleB"></a> B. 将有效的时间字符串文字插入 time(7) 列  
 下表列出了可插入到数据类型为 time(7) 的一个列中的不同字符串文字，以及在插入后存储到该列中的对应值。  
  
|字符串文字格式类型|插入的字符串文字|存储的 time(7) 值|描述|  
|--------------------------------|-----------------------------|------------------------------------|-----------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01:123AM'|01:01:01.1230000|如果在秒的小数部分精度之前使用冒号 (:)，则小数位数不能超过三位，否则将引发错误。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01.1234567 AM'|01:01:01.1234567|如果指定了 AM 或 PM，则时间以不带 AM 或 PM 文字的 24 小时格式存储|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01.1234567 PM'|13:01:01.1234567|如果指定了 AM 或 PM，则时间以不带 AM 或 PM 文字的 24 小时格式存储|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01.1234567PM'|13:01:01.1234567|AM 或 PM 之前的空格可有可无。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01AM'|01:00:00.0000000|仅指定小时时，所有其他值均为 0。|  
|SQL Server|'01 AM'|01:00:00.0000000|AM 或 PM 之前的空格可有可无。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01'|01:01:01.0000000|如果秒的小数部分精度未指定，则由数据类型定义的各数位均为 0。|  
|ISO 8601|'01:01:01.1234567'|01:01:01.1234567|为遵从 ISO 8601，使用 24 小时格式，而非 AM 或 PM 格式。|  
|ISO 8601|'01:01:01.1234567 +01:01'|01:01:01.1234567|允许输入内容中包含可选的时区差异 (TZD)，但不存储它。|  
  
### <a name="c-inserting-time-string-literal-into-columns-of-each-date-and-time-date-type"></a>C. 将时间字符串文字插入到各种日期和时间数据类型的列中  
 下表中的第一列显示的是时间字符串文字，第二列显示的是日期或时间数据类型，第一列中的时间字符串文字将插入到第二列中与之对应的数据类型的数据库表列中。 第三列显示的是将存储在对应数据库表列中的值。  
  
|插入的字符串文字|列数据类型|存储在列中的值|描述|  
|-----------------------------|----------------------|------------------------------------|-----------------|  
|'12:12:12.1234567'|**time(7)**|12:12:12.1234567|如果秒的小数部分精度超过为列指定的值，则字符串将被截断，且不会出错。|  
|'2007-05-07'|**date**|Null|任何时间值均将导致 INSERT 语句失败。|  
|“12:12:12”|**smalldatetime**|1900-01-01 12:12:00|任何秒的小数部分精度值都将导致 INSERT 语句失败。|  
|'12:12:12.123'|**datetime**|1900-01-01 12:12:12.123|任何长于三位的秒精度都将导致 INSERT 语句失败。|  
|'12:12:12.1234567'|**datetime2(7)**|1900-01-01 12:12:12.1234567|如果秒的小数部分精度超过为列指定的值，则字符串将被截断，且不会出错。|  
|'12:12:12.1234567'|**datetimeoffset(7)**|1900-01-01 12:12:12.1234567 +00:00|如果秒的小数部分精度超过为列指定的值，则字符串将被截断，且不会出错。|  
  
## <a name="see-also"></a>另请参阅  
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  
