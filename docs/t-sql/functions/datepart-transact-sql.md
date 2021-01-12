---
title: DATEPART (Transact-SQL) | Microsoft Docs
description: DATEPART 函数的 Transact-SQL 参考。 此函数返回与指定 date 的 datepart 相对应的整数。
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEPART_TSQL
- DATEPART
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], DATEPART
- dateparts [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dateparts [SQL Server], datepart
- comparing dates times [SQL Server]
- DATEPART function [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 15f1a5bc-4c0c-4c48-848d-8ec03473e6c1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: adae6be66b149f930bee51c636bf86d7614e3bf2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093654"
---
# <a name="datepart-transact-sql"></a>DATEPART (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


此函数返回表示指定 date 的指定 datepart 的整数   。
  
有关所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间数据类型及函数的概述，请参阅[日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DATEPART ( datepart , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*datepart*  
`DATEPART` 将返回表示 date 参数的特定部分的整数   。 此表列出了所有有效的 datepart 参数  。

> [!NOTE]
> 对于 datepart 参数，`DATEPART` 不接受用户定义的变量等效项  。
  
|*datepart*|缩写形式|  
|---|---|
|**year**|yy, yyyy  |  
|quarter |qq, q  |  
|month |mm, m  |  
|dayofyear |dy, y  |  
|day |dd, d  |  
|week |wk, ww  |  
|weekday |**dw**|  
|hour |**hh**|  
|minute |**mi, n**|  
|second |ss, s  |  
|millisecond |ms |  
|microsecond |mcs |  
|nanosecond |ns |  
|**tzoffset**|**tz**|  
|**iso_week**|**isowk**, **isoww**|  
  
*date*  
解析为下列某种数据类型的表达式： 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

对于 date，`DATEPART` 接受列表达式、表达式、字符串文本或用户定义的变量  。 使用四位数年份可避免含糊不清问题。 有关两位数年份的信息，请参阅[配置两位数年份截止服务器配置选项](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md)。
  
## <a name="return-type"></a>返回类型  
 **int**  
  
## <a name="return-value"></a>返回值  
每个 datepart 及其缩写都返回相同的值  。
  
返回值取决于 [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) 和登录时选择的[配置默认语言服务器配置选项](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md)设定的语言环境。 如果 date 是某些格式的字符串文本，则返回值取决于 [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)  。 当 date 为日期或时间数据类型的列表达式时，SET DATEFORMAT 不更改返回值。
  
此表列出了所有 datepart 参数以及 `SELECT DATEPART(datepart,'2007-10-30 12:15:32.1234567 +05:10')` 语句相应的返回值  。 date 参数具有 datetimeoffset(7) 数据类型   。 nanosecond datepart 返回值的最后两位始终为 `00`，并且此值的小数位数为 9 位   ：

.123456700 
  
|*datepart*|返回值|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|month, mm, m |10|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|3|  
|hour, hh |12|  
|minute, n |15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**tzoffset, tz**|310|  
|**iso_week, isowk, isoww**|44|  
  
## <a name="week-and-weekday-datepart-arguments"></a>周和工作日日期部分参数
对于 week (wk, ww) 或 weekday (dw) datepart，`DATEPART` 返回值取决于 [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md) 设置的值       。
  
任何年份的 1 月 1 日都用来定义 week datepart 的起始数字   。 例如：

DATEPART (wk, 'Jan 1, xxxx') = 1  

其中，xxxx 为任何年份  。
  
下表列出了每个 SET DATEFIRST 参数的“2007-04-21”的 week 和 weekday datepart 返回值    。 2007 年 1 月 1 日为星期一。 2007 年 4 月 21 日为星期六。 对于美式英语，

`SET DATEFIRST 7 -- ( Sunday )`

用作默认值。 设置 DATEFIRST 后，请将此建议的 SQL 语句用于 datepart 表值：

`SELECT DATEPART(week, '2007-04-21 '), DATEPART(weekday, '2007-04-21 ')`
  
|SET DATEFIRST<br /><br /> 参数|week<br /><br /> 返回|weekday<br /><br /> 返回|  
|---|---|---|
|1|16|6|  
|2|17|5|  
|3|17|4|  
|4|17|3|  
|5|17|2|  
|6|17|1|  
|7|16|7|  
  
## <a name="year-month-and-day-datepart-arguments"></a>年、月和日日期部分参数  
DATEPART (year, date)、DATEPART (month, date) 和 DATEPART (day, date) 的返回值分别与函数 [YEAR](../../t-sql/functions/year-transact-sql.md)、[MONTH](../../t-sql/functions/month-transact-sql.md) 和 [DAY](../../t-sql/functions/day-transact-sql.md) 的返回值相同       。
  
## <a name="iso_week-datepart"></a>iso_week datepart  
ISO 8601 包括 ISO 周-日期系统，即周的编号系统。 每周都与该周内星期四所在的年份关联。 例如，2004 年的第一周 (2004W01) 包括 2003 年 12 月 29 日（星期一）到 2004 年 1 月 4 日（星期日）。 欧洲国家/地区通常使用此编号样式。 非欧洲国家/地区则通常不使用。

注意：一年中的最多周数可能是 52 或 53。
  
不同的国家/地区的编号系统可能不符合 ISO 标准。 此表显示六种可能情况：
  
|每周的第一天|一年的第一周包含|分配两次的周|使用的国家/地区|  
|---|---|---|---|
|星期日|1 月 1 日，<br /><br /> 第一个星期六，<br /><br /> 其中有 1–7 天属于此年|是|United States|  
|星期一|1 月 1 日，<br /><br /> 第一个星期日，<br /><br /> 其中有 1–7 天属于此年|是|大多数欧洲国家地区和英国|  
|星期一|1 月 4 日<br /><br /> 第一个星期四，<br /><br /> 其中有 4–7 天属于此年|否|ISO 8601，挪威和瑞典|  
|星期一|1 月 7 日<br /><br /> 第一个星期一，<br /><br /> 7 天均属于此年|否||  
|星期三|1 月 1 日，<br /><br /> 第一个星期二，<br /><br /> 其中有 1–7 天属于此年|是||  
|星期六|1 月 1 日，<br /><br /> 第一个星期五，<br /><br /> 其中有 1–7 天属于此年|是||  
  
## <a name="tzoffset"></a>tzoffset  
`DATEPART` 返回以分钟数表示的 tzoffset (tz) 值（带有签名）   。 此语句返回了 310 分钟的时区偏移量：
  
```sql
SELECT DATEPART (tzoffset, '2007-05-10  00:00:01.1234567 +05:10');  
```  
`DATEPART` 呈现的 tzoffset 值如下所示：
- 对于 datetimeoffset 和 datetime2，tzoffset 返回的时间偏移量以分钟为单位，其中 datetime2 的偏移量始终为 0 分钟。
- 对于可隐式转换为 datetimeoffset 或 datetime2 的数据类型，`DATEPART` 返回以分钟数表示的时间偏移量   。 异常：其他 date/time 数据类型。
- 所有其他类型的参数都会导致错误。
  
  
## <a name="smalldatetime-date-argument"></a>smalldatetime 日期参数  
对于 [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) date 值，`DATEPART` 返回的秒数为 00  。
  
## <a name="default-returned-for-a-datepart-that-is-not-in-a-date-argument"></a>对不在 date 参数中的 datepart 返回默认值  
如果 date 参数数据类型不具有特定的 datepart，则仅当为 date 指定文本时，`DATEPART` 才返回该 datepart 的默认值     。
  
例如，任意 date 数据类型的默认年-月-日都为 1900-01-01  。 此语句具有 datepart 的日期部分参数、date 的时间参数，并且返回 `1900, 1, 1, 1, 2`  。
  
```sql
SELECT DATEPART(year, '12:10:30.123')  
    ,DATEPART(month, '12:10:30.123')  
    ,DATEPART(day, '12:10:30.123')  
    ,DATEPART(dayofyear, '12:10:30.123')  
    ,DATEPART(weekday, '12:10:30.123');  
```  
  
如果将 date 指定为变量或表列，并且该变量或列的数据类型没有指定的 datepart，`DATEPART` 将返回错误 9810   。 在此示例中，变量 \@t 具有 time 数据类型   。 示例失败，因为日期部分年份对于 time 数据类型无效  ：
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATEPART(year, @t);  
```  
  
## <a name="fractional-seconds"></a>秒的小数形式
这些语句显示 `DATEPART` 返回了秒的小数形式：
  
```sql
SELECT DATEPART(millisecond, '00:00:01.1234567'); -- Returns 123  
SELECT DATEPART(microsecond, '00:00:01.1234567'); -- Returns 123456  
SELECT DATEPART(nanosecond,  '00:00:01.1234567'); -- Returns 123456700  
```  
  
## <a name="remarks"></a>备注  
`DATEPART` 可用在 select list、WHERE、HAVING、GROUP BY 和 ORDER BY 子句中。
  
在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中，DATEPART 将字符串文字隐式强制转换为 datetime2 类型  。 这就意味着，日期在作为字符串传递时，DATENAME 不会支持 YDM 格式。 必须先将字符串显式转换为 datetime 或 smalldatetime 类型，然后才能使用 YDM 格式   。
  
## <a name="examples"></a>示例  
此示例返回基准年。 基准年有助于进行日期计算。 该示例用数字指定日期。 请注意，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将 0 解释为 1900 年 1 月 1 日。
  
```sql
SELECT DATEPART(year, 0), DATEPART(month, 0), DATEPART(day, 0);  

-- Returns: 1900    1    1 
```  
  
此示例返回日期 `12/20/1974` 的日期部分。
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (day,'12/20/1974') FROM dbo.DimCustomer;  

-- Returns: 20
```  
  
此示例返回日期 `12/20/1974` 的年份部分。
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (year,'12/20/1974') FROM dbo.DimCustomer;  

-- Returns: 1974
```  
  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
