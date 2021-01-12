---
title: DATEADD (Transact-SQL) | Microsoft Docs
description: DATEADD 函数的 Transact-SQL 参考。 此函数返回已由指定日期部分修改的日期。
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEADD
- DATEADD_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- add interval to date or time [SQL Server]
- subtract interval from date or time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], intervals
- date and time [SQL Server], DATEADD
- DATEADD function [SQL Server]
ms.assetid: 89c5ae32-89c6-47e1-979e-15d97908b9f1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4edc9ff57e9f8d1bd875df6d62814b3317c7ac45
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093673"
---
# <a name="dateadd-transact-sql"></a>DATEADD (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数将指定的 number 值（作为带符号整数）与输入 date 值的指定 datepart 相加，然后返回该修改值    。
  
有关所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间数据类型及函数的概述，请参阅[日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```sql
DATEADD (datepart , number , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*datepart*  
`DATEADD` 要将其与整数值相加的 date 的一部分    。 此表列出了所有有效的 datepart 参数  。 

> [!NOTE]
> 对于 datepart 参数，`DATEADD` 不接受用户定义的变量等效项  。 
  
|*datepart*|缩写形式|  
|---|---|
|**year**|yy, yyyy  |  
|quarter |qq, q  |  
|month |mm, m  |  
|dayofyear |dy, y  |  
|day |dd, d  |  
|week |wk, ww  |  
|weekday |dw, w  |  
|hour |**hh**|  
|minute |mi, n  |  
|second |ss, s  |  
|millisecond |ms |  
|microsecond |mcs |  
|nanosecond |ns |  
  
*数字*  
一个表达式，可解析为 `DATEADD` 将其与 date 的 datepart 相加的 [int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)   。 对于 number，`DATEADD` 接受 用户定义的变量值  。 `DATEADD` 将截断带小数部分的指定 number 值  。 在这种情况下，它不对 number 值进行舍入  。
  
*date*  
可解析为下列值之一的表达式： 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

对于 date，`DATEADD` 接受列表达式、表达式、字符串文本或用户定义的变量  。 字符串文字值必须解析为 datetime  。 使用四位数年份可避免含糊不清问题。 有关两位数年份的信息，请参阅[配置两位数年份截止服务器配置选项](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md)。
  
## <a name="return-types"></a>返回类型

此方法的返回值数据类型是动态的。 返回类型取决于为 `date` 提供的参数。 如果 `date` 的值是字符串文本日期，则 `DATEADD` 返回日期/时间值  。 如果为 `date` 提供了其他的有效输入数据类型，则 `DATEADD` 返回相同的数据类型。 如果字符串文本秒数小数位超过三位 (.nnn) 或如果字符串文本包含时区偏移量部分，则 `DATEADD` 引发错误。
  
## <a name="return-value"></a>返回值  
  
## <a name="datepart-argument"></a>datepart 参数  
dayofyear、day 和 weekday 返回相同的值    。
  
每个 datepart 及其缩写都返回相同的值  。
  
如果以下各项为 true：

+ datepart 为 month  
+ date 月份比返回月份的天数多 
+ date 日在返回月份中不存在 

`DATEADD` 则返回返回月份的最后一天。 例如，9 月份有 30 天；因此，以下语句返回 2006-09-30 00:00:00.000：
  
```sql
SELECT DATEADD(month, 1, '20060830');
SELECT DATEADD(month, 1, '20060831');
```
  
## <a name="number-argument"></a>number 参数  
number 参数不能超出 int 的范围   。在以下语句中，number 的参数超出 int 的范围（超出 1）   。 以下语句均返回以下错误消息："`Msg 8115, Level 16, State 2, Line 1. Arithmetic overflow error converting expression to data type int."`
  
```sql
SELECT DATEADD(year,2147483648, '20060731');  
SELECT DATEADD(year,-2147483649, '20060731');  
```  
  
## <a name="date-argument"></a>date 参数  
`DATEADD` 不允许 date 参数增加至其数据范围之外的值  。 在以下语句中，与 date 值相加的 number 值超出了 date 数据类型的范围    。 `DATEADD` 返回以下错误消息：“`Msg 517, Level 16, State 1, Line 1 Adding a value to a 'datetime' column caused overflow`。”
  
```sql
SELECT DATEADD(year,2147483647, '20060731');  
SELECT DATEADD(year,-2147483647, '20060731');  
```  
  
## <a name="return-values-for-a-smalldatetime-date-and-a-second-or-fractional-seconds-datepart"></a>date 为 smalldatetime 型、datepart 为秒或秒小数部分时的返回值  
[smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) 值的秒数部分始终为 00。 以下原则适用于 smalldatetime date 值   ： 

-   若 datepart 为 second 且 number 值在 -30 和 +29 之间，`DATEADD` 不进行任何更改    。  
-   若 datepart 为 second 且 number 值小于 -30 或大于 +29，`DATEADD` 从一分钟开始执行相加操作    。  
-   若 datepart 为 millisecond 且 number 值在 -30001 和 +29998 之间，`DATEADD` 不进行任何更改    。  
-   若 datepart 为 millisecond 且 number 值小于 -30001 或大于 +29998，`DATEADD` 从一分钟开始执行相加操作    。  
  
## <a name="remarks"></a>备注  
在以下子句中使用 `DATEADD`：

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE
  
## <a name="fractional-seconds-precision"></a>秒的小数部分精度
对于 date 数据类型 smalldatetime、date 和 datetime，`DATEADD` 不允许执行 microsecond 或 nanosecond 的 datepart 添加操作        。
  
毫秒的小数位数为 3 (.123)，微秒的小数位数为 6 (.123456)，而纳秒的小数位数为 9 (.123456789)。 time、datetime2 和 datetimeoffset 数据类型的最大小数位数为 7 (.1234567)    。 若 datepart 为 nanosecond，则在 date 的秒数小数部分增加前 number 必须为 100     。 介于 1 和 49 之间的 number 向下舍入为 0，介于 50 和 99 之间的 number 向上舍入为 100  。
  
以下语句添加 millisecond、microsecond 或 nanosecond 的 datepart     。
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT '1 millisecond', DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT '2 milliseconds', DATEADD(millisecond,2,@datetime2)  
UNION ALL  
SELECT '1 microsecond', DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT '2 microseconds', DATEADD(microsecond,2,@datetime2)  
UNION ALL  
SELECT '49 nanoseconds', DATEADD(nanosecond,49,@datetime2)  
UNION ALL  
SELECT '50 nanoseconds', DATEADD(nanosecond,50,@datetime2)  
UNION ALL  
SELECT '150 nanoseconds', DATEADD(nanosecond,150,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
1 millisecond     2007-01-01 13:10:10.1121111  
2 milliseconds    2007-01-01 13:10:10.1131111  
1 microsecond     2007-01-01 13:10:10.1111121  
2 microseconds    2007-01-01 13:10:10.1111131  
49 nanoseconds    2007-01-01 13:10:10.1111111  
50 nanoseconds    2007-01-01 13:10:10.1111112  
150 nanoseconds   2007-01-01 13:10:10.1111113  
```  
  
## <a name="time-zone-offset"></a>时区偏移量
`DATEADD` 不允许对时区偏移量执行加法。
  
## <a name="examples"></a>示例  

### <a name="a-incrementing-datepart-by-an-interval-of-1"></a>A. 以 1 为增量递增 datepart  
以下每条语句以 1 为增量递增 datepart  ：
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT 'year', DATEADD(year,1,@datetime2)  
UNION ALL  
SELECT 'quarter',DATEADD(quarter,1,@datetime2)  
UNION ALL  
SELECT 'month',DATEADD(month,1,@datetime2)  
UNION ALL  
SELECT 'dayofyear',DATEADD(dayofyear,1,@datetime2)  
UNION ALL  
SELECT 'day',DATEADD(day,1,@datetime2)  
UNION ALL  
SELECT 'week',DATEADD(week,1,@datetime2)  
UNION ALL  
SELECT 'weekday',DATEADD(weekday,1,@datetime2)  
UNION ALL  
SELECT 'hour',DATEADD(hour,1,@datetime2)  
UNION ALL  
SELECT 'minute',DATEADD(minute,1,@datetime2)  
UNION ALL  
SELECT 'second',DATEADD(second,1,@datetime2)  
UNION ALL  
SELECT 'millisecond',DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT 'microsecond',DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT 'nanosecond',DATEADD(nanosecond,1,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Year         2008-01-01 13:10:10.1111111  
quarter      2007-04-01 13:10:10.1111111  
month        2007-02-01 13:10:10.1111111  
dayofyear    2007-01-02 13:10:10.1111111  
day          2007-01-02 13:10:10.1111111  
week         2007-01-08 13:10:10.1111111  
weekday      2007-01-02 13:10:10.1111111  
hour         2007-01-01 14:10:10.1111111  
minute       2007-01-01 13:11:10.1111111  
second       2007-01-01 13:10:11.1111111  
millisecond  2007-01-01 13:10:10.1121111  
microsecond  2007-01-01 13:10:10.1111121  
nanosecond   2007-01-01 13:10:10.1111111  
```  
  
### <a name="b-incrementing-more-than-one-level-of-datepart-in-one-statement"></a>B. 在一条语句中将 datepart 增加一级以上  
以下每个语句按足够大的 number 逐量增加 datepart，该 number 还应足以增加 date 的下一个更高的 datepart     ：
  
```sql
DECLARE @datetime2 datetime2;  
SET @datetime2 = '2007-01-01 01:01:01.1111111';  
--Statement                                 Result     
-------------------------------------------------------------------   
SELECT DATEADD(quarter,4,@datetime2);     --2008-01-01 01:01:01.1111111  
SELECT DATEADD(month,13,@datetime2);      --2008-02-01 01:01:01.1111111  
SELECT DATEADD(dayofyear,365,@datetime2); --2008-01-01 01:01:01.1111111  
SELECT DATEADD(day,365,@datetime2);       --2008-01-01 01:01:01.1111111  
SELECT DATEADD(week,5,@datetime2);        --2007-02-05 01:01:01.1111111  
SELECT DATEADD(weekday,31,@datetime2);    --2007-02-01 01:01:01.1111111  
SELECT DATEADD(hour,23,@datetime2);       --2007-01-02 00:01:01.1111111  
SELECT DATEADD(minute,59,@datetime2);     --2007-01-01 02:00:01.1111111  
SELECT DATEADD(second,59,@datetime2);     --2007-01-01 01:02:00.1111111  
SELECT DATEADD(millisecond,1,@datetime2); --2007-01-01 01:01:01.1121111  
```  
  
### <a name="c-using-expressions-as-arguments-for-the-number-and-date-parameters"></a>C. 使用表达式作为 number 和 date 形参的实参  
以下示例使用不同类型的表达式作为 number 和 date 形参的实参   。 示例使用 AdventureWorks 数据库。
  
#### <a name="specifying-a-column-as-date"></a>将一列指定为 date  
此示例将 `2` 天加到 `OrderDate` 列中的每个值，以便派生名为 `PromisedShipDate` 的新列：
  
```sql
SELECT SalesOrderID  
    ,OrderDate   
    ,DATEADD(day,2,OrderDate) AS PromisedShipDate  
FROM Sales.SalesOrderHeader;  
```  
  
部分结果集：
  
```
SalesOrderID OrderDate               PromisedShipDate  
------------ ----------------------- -----------------------  
43659        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43660        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43661        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
...  
43702        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43703        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43704        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43705        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43706        2005-07-03 00:00:00.000 2005-07-05 00:00:00.000  
...  
43711        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
43712        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
...  
43740        2005-07-11 00:00:00.000 2005-07-13 00:00:00.000  
43741        2005-07-12 00:00:00.000 2005-07-14 00:00:00.000  
  
```  
  
#### <a name="specifying-user-defined-variables-as-number-and-date"></a>将用户定义的变量指定为 number 和 date  
此示例将用户定义的变量指定为 number 和 date 的参数   ：
  
```sql
DECLARE @days INT = 365,   
        @datetime DATETIME = '2000-01-01 01:01:01.111'; /* 2000 was a leap year */;  
SELECT DATEADD(day, @days, @datetime);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------------------  
2000-12-31 01:01:01.110  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-system-function-as-date"></a>将标量系统函数指定为 date  
此示例指定 `SYSDATETIME` 为 date  。 返回的确切值取决于语句执行的日期和时间：
  
```sql
SELECT DATEADD(month, 1, SYSDATETIME());  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------------------------  
2013-02-06 14:29:59.6727944  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-subqueries-and-scalar-functions-as-number-and-date"></a>将标量子查询和标量函数指定为 number 和 date  
此示例使用标量子查询 `MAX(ModifiedDate)` 作为 number 和 date 的参数   。 `(SELECT TOP 1 BusinessEntityID FROM Person.Person)` 充当 number 形参的假实参，用来说明如何从值列表中选择 number 实参  。
  
```sql
SELECT DATEADD(month,(SELECT TOP 1 BusinessEntityID FROM Person.Person),  
    (SELECT MAX(ModifiedDate) FROM Person.Person));  
```  
  
#### <a name="specifying-numeric-expressions-and-scalar-system-functions-as-number-and-date"></a>将数值表达式和标量系统函数指定为 number 和 date  
此示例将数值表达式 (-`(10/2))`、[一元运算符](../../mdx/unary-operators.md) (`-`)、[算术运算符](../../mdx/arithmetic-operators.md) (`/`) 和标量系统函数 (`SYSDATETIME`) 作为 number 和 date 的参数   。
  
```sql
SELECT DATEADD(month,-(10/2), SYSDATETIME());  
```  
  
#### <a name="specifying-ranking-functions-as-number"></a>将排名函数指定为 number  
此示例使用排名函数作为 number 的参数  。
  
```sql
SELECT p.FirstName, p.LastName  
    ,DATEADD(day,ROW_NUMBER() OVER (ORDER BY  
        a.PostalCode),SYSDATETIME()) AS 'Row Number'  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
```  
  
#### <a name="specifying-an-aggregate-window-function-as-number"></a>将聚合开窗函数指定为 number  
此示例使用聚合开窗函数作为 number 的参数  。
  
```sql
SELECT SalesOrderID, ProductID, OrderQty  
    ,DATEADD(day,SUM(OrderQty)   
        OVER(PARTITION BY SalesOrderID),SYSDATETIME()) AS 'Total'  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

