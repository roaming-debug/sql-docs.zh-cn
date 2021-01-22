---
title: DATEDIFF_BIG (Transact-SQL)
description: DATEDIFF_BIG (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEDIFF_BIG
- DATEDIFF_BIG_TSQL
helpviewer_keywords:
- DATEDIFF_BIG function [SQL Server]
- dates [SQL Server]. functions
- date and time [SQL Server], DATEDIFF_BIG
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: 19ac1693-3cfa-400d-bf83-20a9cb46599a
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 01/12/2021
ms.openlocfilehash: 8f6078aebab5456b8867aca1b7f6987d8a1eb6fd
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98168070"
---
# <a name="datediff_big-transact-sql"></a>DATEDIFF_BIG (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

此函数返回指定的 startdate 和 enddate 之间所跨的指定 datepart 边界的计数（作为带符号大整数值）。
  
有关所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间数据类型及函数的概述，请参阅[日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DATEDIFF_BIG ( datepart , startdate , enddate )  
```  

## <a name="arguments"></a>参数
*datepart*  
指定所跨边界类型的 startdate 和 enddate 的一部分。

> [!NOTE]
> `DATEDIFF_BIG` 不会接受来自用户定义的变量或作为带引号的字符串的 datepart 值。

此表列出了所有有效的 datepart 参数名称和缩写。
  
|datepart 名称 | datepart 缩写 |  
|---|---|
|**year**|**yy, yyyy**|  
|quarter |**qq, q**|  
|month |**mm, m**|  
|dayofyear |**dy, y**|  
|day |**dd, d**|  
|week |**wk, ww**|  
|hour |**hh**|  
|minute |**mi, n**|  
|second |**ss, s**|  
|millisecond |ms |  
|microsecond |mcs |  
|nanosecond |ns |  

> [!NOTE]
> 每个特定的 datepart 名称及其相应名称的缩写将返回相同的值   。

*startdate*  
可解析为下列值之一的表达式：

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

对于 date，`DATEDIFF_BIG` 接受列表达式、表达式、字符串文本或用户定义的变量  。 字符串文字值必须解析为 datetime  。 使用四位数年份可避免含糊不清问题。 `DATEDIFF_BIG` 从 enddate 中减去 startdate。 为避免不确定性，请使用四位数年份。 有关两位数年份的信息，请参阅[配置两位数年份截止服务器配置选项](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md)。
  
enddate   
请参阅 startdate  。
  
## <a name="return-type"></a>返回类型  
签名的 bigint  
  
## <a name="return-value"></a>返回值  
返回 startdate 与 enddate 之间的 bigint 差异，以 datepart 设置的边界表示  。
  
若 bigint 的返回值超出范围（-9223372036854775808 到 9223372036854775807），`DATEDIFF_BIG` 返回错误。 与返回 int 且因此可能以分钟或更高精度溢出的 DATEDIFF 不同，`DATEDIFF_BIG` 仅在使用纳秒精度（其中 enddate 和 startdate 之间的差异超过 292 年 3 个月 10 天 23 小时 47 分钟 16.8547758 秒）时才会溢出   。
  
如果为 startdate 和 enddate 都只指定了时间值，并且 datepart 不是时间 datepart，则 `DATEDIFF_BIG` 返回 0   。
  
`DATEDIFF_BIG` 使用 startdate 或 enddate 的时区偏移量部分来计算返回值 。
  
对于用于 startdate 或 enddates 的 malldatetime 值，由于 [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) 仅精确到分钟，因此 `DATEDIFF_BIG` 始终将返回值中的秒和毫秒设置为 0。
  
如果只为某个日期数据类型变量指定时间值，`DATEDIFF_BIG` 会将所缺日期部分的值设置为默认值：`1900-01-01`。 如果只为某个时间或日期数据类型的变量指定日期值，`DATEDIFF_BIG` 会将所缺时间部分的值设置为默认值：`00:00:00`。 如果 startdate 和 enddate 中有一个只含时间部分，另一个只含日期部分，`DATEDIFF_BIG` 会将所缺时间和日期部分设置为各自的默认值   。
  
如果 startdate 和 enddate 具有不同的日期数据类型，并且其中一个的时间部分或秒小数部分精度比另一个高，`DATEDIFF_BIG` 会将另一个的所缺部分设置为 0   。
  
## <a name="datepart-boundaries"></a>datepart 边界
以下语句具有相同的 startdate 和 enddate 值   。 这些日期是相邻的，它们在时间上相差一微秒（0.0000001 秒）。 每个语句中 startdate 与 enddate 之间的差跨其 datepart 的一个日历或时间边界    。 每个语句都返回 1。 如果 startdate 和 enddate 的年份值不同，但它们的日历周值相同，`DATEDIFF_BIG` 将对 datepart week 返回 0  。

```sql
SELECT DATEDIFF_BIG(year,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(quarter,     '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(month,       '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(dayofyear,   '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(day,         '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(week,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(hour,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(minute,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(second,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(millisecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
```
  
## <a name="remarks"></a>备注  
在 `SELECT <list>`、`WHERE`、`HAVING`、`GROUP BY` 和 `ORDER BY` 子句中使用 `DATEDIFF_BIG`。
  
`DATEDIFF_BIG` 将字符串文字隐式转换为 datetime2 类型  。 这就意味着，日期在作为字符串传递时，`DATEDIFF_BIG` 不会支持 YDM 格式。 必须先将字符串显式转换为 datetime 或 smalldatetime 类型，然后才能使用 YDM 格式   。
  
指定 `SET DATEFIRST` 对 `DATEDIFF_BIG` 没有影响。 `DATEDIFF_BIG` 始终使用星期日作为每周的第一天，确保函数以确定性方式运行。

如果 enddate 和 startdate 之间的差值返回一个超出 bigint 范围的值，`DATEDIFF_BIG` 可能会溢出，且其精度为纳秒 。
  
## <a name="examples"></a>示例 
  
### <a name="specifying-columns-for-startdate-and-enddate"></a>为 startdate 和 enddate 指定列  
此示例使用不同类型的表达式作为 startdate 和 enddate 形参的实参。 它计算一个表的两列中的日期之间所跨越的日边界数。
  
```sql
CREATE TABLE dbo.Duration  
    (startDate datetime2, endDate datetime2);  
    
INSERT INTO dbo.Duration(startDate,endDate)  
    VALUES('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT DATEDIFF_BIG(day, startDate, endDate) AS 'Duration'  
    FROM dbo.Duration;  
-- Returns: 1  
```  

### <a name="finding-difference-between-startdate-and-enddate-as-date-parts-strings"></a>查找作为日期部分字符串的 startdate 和 enddate 之间的差异

```sql
DECLARE @date1 DATETIME2, @date2 DATETIME2, @result VARCHAR(100)
DECLARE @years BIGINT, @months BIGINT, @days BIGINT, @hours BIGINT, @minutes BIGINT, @seconds BIGINT, @milliseconds BIGINT

SET @date1 = '0001-01-01 00:00:00.00000000'
SET @date2 = '2018-12-12 07:08:01.12345678'

SELECT @years = DATEDIFF(yy, @date1, @date2)
IF DATEADD(yy, -@years, @date2) < @date1 
SELECT @years = @years-1
SET @date2 = DATEADD(yy, -@years, @date2)

SELECT @months = DATEDIFF(mm, @date1, @date2)
IF DATEADD(mm, -@months, @date2) < @date1 
SELECT @months=@months-1
SET @date2= DATEADD(mm, -@months, @date2)

SELECT @days=DATEDIFF(dd, @date1, @date2)
IF DATEADD(dd, -@days, @date2) < @date1 
SELECT @days=@days-1
SET @date2= DATEADD(dd, -@days, @date2)

SELECT @hours=DATEDIFF(hh, @date1, @date2)
IF DATEADD(hh, -@hours, @date2) < @date1 
SELECT @hours=@hours-1
SET @date2= DATEADD(hh, -@hours, @date2)

SELECT @minutes=DATEDIFF(mi, @date1, @date2)
IF DATEADD(mi, -@minutes, @date2) < @date1 
SELECT @minutes=@minutes-1
SET @date2= DATEADD(mi, -@minutes, @date2)

SELECT @seconds=DATEDIFF(s, @date1, @date2)
IF DATEADD(s, -@seconds, @date2) < @date1 
SELECT @seconds=@seconds-1
SET @date2= DATEADD(s, -@seconds, @date2)

SELECT @milliseconds=DATEDIFF(ms, @date1, @date2)

SELECT @result= ISNULL(CAST(NULLIF(@years,0) AS VARCHAR(10)) + ' years,','')
     + ISNULL(' ' + CAST(NULLIF(@months,0) AS VARCHAR(10)) + ' months,','')    
     + ISNULL(' ' + CAST(NULLIF(@days,0) AS VARCHAR(10)) + ' days,','')
     + ISNULL(' ' + CAST(NULLIF(@hours,0) AS VARCHAR(10)) + ' hours,','')
     + ISNULL(' ' + CAST(@minutes AS VARCHAR(10)) + ' minutes and','')
     + ISNULL(' ' + CAST(@seconds AS VARCHAR(10)) 
          + CASE WHEN @milliseconds > 0 THEN '.' + CAST(@milliseconds AS VARCHAR(10)) 
               ELSE '' END 
          + ' seconds','')

SELECT @result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 

```
2017 years, 11 months, 11 days, 7 hours, 8 minutes and 1.123 seconds
```   

请参阅 [DATEDIFF (Transact-SQL)](../../t-sql/functions/datediff-transact-sql.md) 中更紧密地相关的示例。
  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[DATEDIFF (Transact-SQL)](../../t-sql/functions/datediff-transact-sql.md)
  
  
