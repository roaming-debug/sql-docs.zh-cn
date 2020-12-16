---
description: smalldatetime (Transact-SQL)
title: smalldatetime (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- smalldatetime_TSQL
- smalldatetime
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- smalldatetime data type [SQL Server]
- dates [SQL Server], data types
- date and time [SQL Server], smalldatetime
- data types [SQL Server], date and time
ms.assetid: 68b74610-d54c-4c8e-b4b2-7e3747546ee0
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4f96bb94cc58a7b36a294c493f7c0f6e6d8a06b5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472198"
---
# <a name="smalldatetime-transact-sql"></a>smalldatetime (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

定义结合了一天中的时间的日期。 此时间为 24 小时制，秒始终为零 (:00)，并且不带秒小数部分。
  
> [!NOTE]  
>  为新的工作使用 time、date、datetime2 和 datetimeoffset 数据类型     。 这些类型符合 SQL 标准。 它们更易于移植。 time、datetime2 和 datetimeoffset 提供更高精度的秒数    。 datetimeoffset 为全局部署的应用程序提供时区支持  。  
  
## <a name="smalldatetime-description"></a>smalldatetime 说明
  
|属性|值|
|--------|-----|
|语法|**smalldatetime**|  
|使用情况|DECLARE \@MySmalldatetime smalldatetime<br /><br /> CREATE TABLE Table1 ( Column1 smalldatetime )|  
|默认的字符串文字格式<br /><br /> （用于下级客户端）|不适用|  
|日期范围|1900-01-01 到 2079-06-06<br /><br /> 1900 年 1 月 1 日到 2079 年 6 月 6 日|  
|时间范围|00:00:00 到 23:59:59<br /><br /> 2007-05-09 23:59:59 将舍入为<br /><br /> 2007-05-10 00:00:00|  
|各元素的范围|YYYY 是表示年份的四位数字，范围为 1900 到 2079。<br /><br /> MM 是表示指定年份中的月份的两位数字，范围为 01 到 12。<br /><br /> DD 是表示指定月份中的某一天的两位数字，范围为 01 到 31（最高值取决于相应月份）。<br /><br /> hh 是表示小时的两位数字，范围为 00 到 23。<br /><br /> mm 是表示分钟的两位数字，范围为 00 到 59。<br /><br /> ss 是表示秒钟的两位数字，范围为 00 到 59。 值为 29.998 秒或更小值时会向下舍入到最接近的分钟数。 值为 29.999 秒或更大值时会向上舍入到最接近的分钟数。|  
|字符长度|最高 19 位|  
|存储大小|固定 4 个字节。|  
|精确度|一分钟|  
|默认值|1900-01-01 00:00:00|  
|日历|公历<br /><br /> （不包括完整的年份范围。）|  
|用户定义的秒的小数部分精度|否|  
|时区偏移量感知和保留|否|  
|夏时制感知|否|  
  
## <a name="ansi-and-iso-8601-compliance"></a>对 ANSI 和 ISO 8601 的遵从性  
**smalldatetime** 不遵从 ANSI 或 ISO 8601。
  
## <a name="converting-date-and-time-data"></a>转换日期和时间数据
当转换为日期和时间数据类型时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将会拒绝它无法识别为日期或时间的所有值。 有关对日期和时间数据使用 CAST 和 CONVERT 函数的信息，请参阅 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)。
  
### <a name="converting-smalldatetime-to-other-date-and-time-types"></a>将 smalldatetime 转换为其他日期和时间类型
本部分介绍当 smalldatetime 数据类型转换为其他日期和时间数据类型时发生的情况。
  
转换成 **date** 时，会复制年、月和日。 下面的代码显示将 `smalldatetime` 值转换为 `date` 值的结果。
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @date date = @smalldatetime  
  
SELECT @smalldatetime AS '@smalldatetime', @date AS 'date';  
  
--Result  
--@smalldatetime          date  
------------------------- ----------  
--1955-12-13 12:43:00     1955-12-13  
--  
--(1 row(s) affected)  
```  
  
转换到 time(n) 时，会复制小时、分钟和秒钟。 秒的小数部分设置为 0。 下面的代码显示将 `smalldatetime` 值转换为 `time(4)` 值的结果。
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @time time(4) = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @time AS 'time';  
  
--Result  
--@smalldatetime          time  
------------------------- -------------  
--1955-12-13 12:43:00     12:43:00.0000  
--  
--(1 row(s) affected)  
```  
  
转换成 datetime 时，会将 smalldatetime 值复制到 datetime 值  。 秒的小数部分设置为 0。 下面的代码显示将 `smalldatetime` 值转换为 `datetime` 值的结果。
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @datetime datetime = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @datetime AS 'datetime';  
  
--Result  
--@smalldatetime          datetime  
------------------------- -----------------------  
--1955-12-13 12:43:00     1955-12-13 12:43:00.000  
--  
--(1 row(s) affected)  
```  
  
转换成 **datetimeoffset(n)** 时，会将 **smalldatetime** 值复制到 **datetimeoffset(n)** 值。 秒的小数部分设置为 0，时区偏移量设置为 +00:0。 下面的代码显示将 `smalldatetime` 值转换为 `datetimeoffset(4)` 值的结果。
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @datetimeoffset datetimeoffset(4) = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @datetimeoffset AS 'datetimeoffset(4)';  
  
--Result  
--@smalldatetime          datetimeoffset(4)  
------------------------- ------------------------------  
--1955-12-13 12:43:00     1955-12-13 12:43:00.0000 +00:0  
--  
--(1 row(s) affected)  
```  
  
转换成 datetime2(n) 时，会将 smalldatetime 值复制到 datetime2(n) 值  。 秒的小数部分设置为 0。 下面的代码显示将 `smalldatetime` 值转换为 `datetime2(4)` 值的结果。
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @datetime2 datetime2(4) = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @datetime2 AS ' datetime2(4)';  
  
--Result  
--@smalldatetime           datetime2(4)  
------------------------- ------------------------  
--1955-12-13 12:43:00     1955-12-13 12:43:00.0000  
--  
--(1 row(s) affected)  
```  
  
## <a name="examples"></a>示例  
  
### <a name="a-casting-string-literals-with-seconds-to-smalldatetime"></a>A. 将带秒数的字符串文字转换为 smalldatetime  
下例比较了将字符串文字中的秒数转换成 `smalldatetime` 时产生的结果。
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29'     AS smalldatetime)  
    ,CAST('2007-05-08 12:35:30'     AS smalldatetime)  
    ,CAST('2007-05-08 12:59:59.998' AS smalldatetime);  
```  
  
|输入|Output|  
|---|---|
|2007-05-08 12:35:29|2007-05-08 12:35:00|  
|2007-05-08 12:35:30|2007-05-08 12:36:00|  
|2007-05-08 12:59:59.998|2007-05-08 13:00:00|  
  
### <a name="b-comparing-date-and-time-data-types"></a>B. 比较日期和时间数据类型  
下例比较了将一个字符串分别转换为各种 date 和 time 数据类型时所产生的结果。
  
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
|---|---|
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
