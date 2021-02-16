---
title: DATEDIFF (Transact-SQL) | Microsoft Docs
description: DATEDIFF 函数的 Transact-SQL 参考。 返回基于 datepart 的开始和结束日期之间的数值差异。
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEDIFF_TSQL
- DATEDIFF
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- DATEDIFF function [SQL Server]
- time [SQL Server], crossed boundaries
- differences in date and time [SQL Server]
- counting crossed date time boundaries [SQL Server]
- date and time [SQL Server], DATEDIFF
- dates [SQL Server], crossed boundaries
- boundary differences date and time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- interval dates [SQL Server]
- time [SQL Server], functions
- crossing date time boundaries [SQL Server]
- calculating dates times [SQL Server]
ms.assetid: eba979f2-1a8d-4cce-9d75-b74f9b519b37
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 202134aeb21010a666eeb3b33fa54ee0143e3b63
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341664"
---
# <a name="datediff-transact-sql"></a>DATEDIFF (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数返回指定的 startdate 和 enddate 之间所跨的指定 datepart 边界的计数（作为带符号整数值）   。
  
有关处理 startdate 和 enddate 值之间较大差异的函数，请参阅 [DATEDIFF_BIG (TRANSACT-SQ)](../../t-sql/functions/datediff-big-transact-sql.md)   。 有关所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间数据类型及函数的概述，请参阅[日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DATEDIFF ( datepart , startdate , enddate )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

*datepart*  
DATEDIFF  用于报告 startdate  与 enddate  之间差异的单位。 常用 datepart  单位包括 `month` 或 `second`。

datepart  值不能在变量中指定，也不能指定为带引号的字符串，如 `'month'`。

下表列出了所有有效的 datepart 值  。 DATEDIFF  接受 datepart  的全名或任何列出的全名缩写形式。

|datepart 名称 |datepart 缩写 |  
|-----------|------------|
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
| &nbsp; | &nbsp; |

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
  
使用四位数年份可避免含糊不清。 有关两位数年份值的信息，请参阅[配置两位数年份截止服务器配置选项](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md)。
  
enddate   
请参阅 startdate  。
  
## <a name="return-type"></a>返回类型  
 **int**  
  
## <a name="return-value"></a>返回值  

startdate 与 enddate 之间的 int 差异，以 datepart 设置的边界表示     。
  
例如，`SELECT DATEDIFF(day, '2036-03-01', '2036-02-28');` 返回 -2，提示 2036 必须为闰年。 这种情况意味着如果从 _startdate_ '2036-03-01' 开始，然后计数 -2 天，则会得到 _enddate_ '2036-02-28'。
  
若 bigint 的返回值超出范围（-2,147,483,648 到 +2,147,483,647），`DATEDIFF` 返回错误  。  对于 millisecond，startdate 和 enddate 之间的最大差值为 24 天 20 小时 31 分钟 23.647 秒    。 对于 second，最大差值为 68 年 19 天 3 小时 14 分 7 秒  。
  
如果为 startdate 和 enddate 都只指定了时间值，并且 datepart 不是时间 datepart，则 `DATEDIFF` 返回 0     。
  
`DATEDIFF` 使用 startdate  或 enddate  的时区偏移部分来计算返回值。
  
由于 [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) 仅精确到分钟，因此在 startdate 或 enddate 具有 smalldatetime 值时，返回值中的秒和毫秒将始终设置为 0    。
  
如果只为某个日期数据类型变量指定时间值，`DATEDIFF` 会将所缺日期部分的值设置为默认值：`1900-01-01`。 如果只为某个时间或日期数据类型的变量指定日期值，`DATEDIFF` 会将所缺时间部分的值设置为默认值：`00:00:00`。 如果 startdate 和 enddate 中有一个只含时间部分，另一个只含日期部分，`DATEDIFF` 会将所缺时间和日期部分设置为各自的默认值   。
  
如果 startdate 和 enddate 具有不同的日期数据类型，并且其中一个的时间部分或秒小数部分精度比另一个高，`DATEDIFF` 会将另一个的所缺部分设置为 0   。
  
## <a name="_datepart_-boundaries"></a>datepart  边界

以下语句具有相同的 startdate 和 enddate 值   。 这些日期是相邻的，它们在时间上相差一百纳秒（0.0000001 秒）。 每个语句中 startdate 与 enddate 之间的差跨其 datepart 的一个日历或时间边界    。 每个语句都返回 1。
  
```sql
SELECT DATEDIFF(year,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(quarter,     '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(month,       '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(dayofyear,   '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(day,         '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(week,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(hour,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(minute,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(second,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(millisecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(microsecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
```

如果 startdate 和 enddate 的年份值不同，但它们的日历周值相同，`DATEDIFF` 将对 datepart week 返回 0     。

## <a name="remarks"></a>备注  
在 `SELECT <list>`、`WHERE`、`HAVING`、`GROUP BY` 和 `ORDER BY` 子句中使用 `DATEDIFF`。
  
`DATEDIFF` 将字符串文字隐式转换为 datetime2 类型  。 这就意味着，日期在作为字符串传递时，`DATEDIFF` 不会支持 YDM 格式。 必须先将字符串显式转换为 datetime 或 smalldatetime 类型，然后才能使用 YDM 格式   。
  
指定 `SET DATEFIRST` 对 `DATEDIFF` 没有影响。 `DATEDIFF` 始终使用星期日作为每周的第一天，确保函数以确定性方式运行。

如果 enddate 和 startdate 之间的差值返回一个超出 int 范围的值，`DATEDIFF` 可能会溢出，且其精度为分钟或更高     。
  
## <a name="examples"></a>示例  
以下示例使用不同类型的表达式作为 startdate 和 enddate 形参的实参   。
  
### <a name="a-specifying-columns-for-startdate-and-enddate"></a>A. 为 startdate 和 enddate 指定列  
此示例计算一个表的两列中的日期之间所跨越的日边界数。
  
```sql
CREATE TABLE dbo.Duration  
    (startDate datetime2, endDate datetime2);  
    
INSERT INTO dbo.Duration(startDate, endDate)  
    VALUES ('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT DATEDIFF(day, startDate, endDate) AS 'Duration'  
    FROM dbo.Duration;  
-- Returns: 1  
```  
  
### <a name="b-specifying-user-defined-variables-for-startdate-and-enddate"></a>B. 为 startdate 和 enddate 指定用户定义的变量  
在此示例中，用户定义的变量充当 startdate 和 enddate 的参数   。
  
```sql
DECLARE @startdate DATETIME2 = '2007-05-05 12:10:09.3312722';  
DECLARE @enddate   DATETIME2 = '2007-05-04 12:10:09.3312722';   
SELECT DATEDIFF(day, @startdate, @enddate);  
```  
  
### <a name="c-specifying-scalar-system-functions-for-startdate-and-enddate"></a>C. 为 startdate 和 enddate 指定标量系统函数  
此示例使用标量系统函数作为 startdate 和 enddate 的参数   。
  
```sql
SELECT DATEDIFF(millisecond, GETDATE(), SYSDATETIME());  
```  
  
### <a name="d-specifying-scalar-subqueries-and-scalar-functions-for-startdate-and-enddate"></a>D. 为 startdate 和 enddate 指定标量子查询和标量函数  
此示例使用标量子查询和标量函数作为 startdate 和 enddate 的参数   。
  
```sql
USE AdventureWorks2012;  
GO  
SELECT DATEDIFF(day,
    (SELECT MIN(OrderDate) FROM Sales.SalesOrderHeader),  
    (SELECT MAX(OrderDate) FROM Sales.SalesOrderHeader));  
```  
  
### <a name="e-specifying-constants-for-startdate-and-enddate"></a>E. 为 startdate 和 enddate 指定常量  
此示例使用字符常量作为 startdate 和 enddate 的参数   。
  
```sql
SELECT DATEDIFF(day,
   '2007-05-07 09:53:01.0376635',
   '2007-05-08 09:53:01.0376635');  
```  
  
### <a name="f-specifying-numeric-expressions-and-scalar-system-functions-for-enddate"></a>F. 为 enddate 指定数值表达式和标量系统函数  
此示例使用数值表达式（`(GETDATE() + 1)`）和标量系统函数（`GETDATE` 和 `SYSDATETIME`）作为 enddate 的参数  。
  
```sql
USE AdventureWorks2012;  
GO  
SELECT DATEDIFF(day, '2007-05-07 09:53:01.0376635', GETDATE() + 1)
    AS NumberOfDays  
    FROM Sales.SalesOrderHeader;  
GO  
USE AdventureWorks2012;  
GO  
SELECT
    DATEDIFF(
            day,
            '2007-05-07 09:53:01.0376635',
            DATEADD(day, 1, SYSDATETIME())
        ) AS NumberOfDays  
    FROM Sales.SalesOrderHeader;  
GO  
```  
  
### <a name="g-specifying-ranking-functions-for-startdate"></a>G. 为 startdate 指定排名函数  
此示例使用排名函数作为 startdate 的参数  。
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,DATEDIFF(day, ROW_NUMBER() OVER (ORDER BY   
        a.PostalCode), SYSDATETIME()) AS 'Row Number'  
FROM Sales.SalesPerson s   
    INNER JOIN Person.Person p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
```  
  
### <a name="h-specifying-an-aggregate-window-function-for-startdate"></a>H. 为 startdate 指定聚合开窗函数  
此示例使用聚合开窗函数作为 startdate 的参数  。
  
```sql
USE AdventureWorks2012;  
GO  
SELECT soh.SalesOrderID, sod.ProductID, sod.OrderQty, soh.OrderDate,
    DATEDIFF(day, MIN(soh.OrderDate)   
        OVER(PARTITION BY soh.SalesOrderID), SYSDATETIME()) AS 'Total'  
FROM Sales.SalesOrderDetail sod  
    INNER JOIN Sales.SalesOrderHeader soh  
        ON sod.SalesOrderID = soh.SalesOrderID  
WHERE soh.SalesOrderID IN(43659, 58918);  
GO  
```  

### <a name="i-finding-difference-between-startdate-and-enddate-as-date-parts-strings"></a>I. 查找作为日期部分字符串的 startdate 和 enddate 之间的差异

```sql
-- DOES NOT ACCOUNT FOR LEAP YEARS
DECLARE @date1 DATETIME, @date2 DATETIME, @result VARCHAR(100);
DECLARE @years INT, @months INT, @days INT,
    @hours INT, @minutes INT, @seconds INT, @milliseconds INT;

SET @date1 = '1900-01-01 00:00:00.000'
SET @date2 = '2018-12-12 07:08:01.123'

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
     + CASE
            WHEN @milliseconds > 0
                THEN '.' + CAST(@milliseconds AS VARCHAR(10)) 
            ELSE ''
       END 
     + ' seconds','')

SELECT @result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 

```
118 years, 11 months, 11 days, 7 hours, 8 minutes and 1.123 seconds
```
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
以下示例使用不同类型的表达式作为 startdate 和 enddate 形参的实参   。
  
### <a name="j-specifying-columns-for-startdate-and-enddate"></a>J. 为 startdate 和 enddate 指定列  
此示例计算一个表的两列中的日期之间所跨越的日边界数。
  
```sql
CREATE TABLE dbo.Duration 
    (startDate datetime2, endDate datetime2);
    
INSERT INTO dbo.Duration (startDate, endDate)  
    VALUES ('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT TOP(1) DATEDIFF(day, startDate, endDate) AS Duration  
    FROM dbo.Duration;  
-- Returns: 1  
```  
  
### <a name="k-specifying-scalar-subqueries-and-scalar-functions-for-startdate-and-enddate"></a>K. 为 startdate 和 enddate 指定标量子查询和标量函数  
此示例使用标量子查询和标量函数作为 startdate 和 enddate 的参数   。
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEDIFF(day, (SELECT MIN(HireDate) FROM dbo.DimEmployee),  
    (SELECT MAX(HireDate) FROM dbo.DimEmployee))   
FROM dbo.DimEmployee;  
  
```  
  
### <a name="l-specifying-constants-for-startdate-and-enddate"></a>L. 为 startdate 和 enddate 指定常量  
此示例使用字符常量作为 startdate 和 enddate 的参数   。
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEDIFF(day,
    '2007-05-07 09:53:01.0376635',
    '2007-05-08 09:53:01.0376635') FROM DimCustomer;  
```  
  
### <a name="m-specifying-ranking-functions-for-startdate"></a>M. 为 startdate 指定排名函数  
此示例使用排名函数作为 startdate 的参数  。
  
```sql
-- Uses AdventureWorks  
  
SELECT FirstName, LastName,
    DATEDIFF(day, ROW_NUMBER() OVER (ORDER BY   
        DepartmentName), SYSDATETIME()) AS RowNumber  
FROM dbo.DimEmployee;  
```  
  
### <a name="n-specifying-an-aggregate-window-function-for-startdate"></a>N. 为 startdate 指定聚合开窗函数  
此示例使用聚合开窗函数作为 startdate 的参数  。
  
```sql
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, DepartmentName,
    DATEDIFF(year, MAX(HireDate)  
        OVER (PARTITION BY DepartmentName), SYSDATETIME()) AS SomeValue  
FROM dbo.DimEmployee  
```  
  
## <a name="see-also"></a>另请参阅
[DATEDIFF_BIG (Transact-SQL)](../../t-sql/functions/datediff-big-transact-sql.md)  
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
