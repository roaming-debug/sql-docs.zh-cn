---
description: ODBC 标量函数 (Transact-SQL)
title: ODBC 标量函数 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CURDATE ODBC function
- DAYOFMONTH ODBC function
- WEEK ODBC function
- functions, ODBC CONCAT
- SECOND ODBC function
- functions, ODBC CURRENT_TIME
- functions, ODBC HOUR
- functions, ODBC QUARTER
- TRUNCATE ODBC function
- functions, ODBC SECOND
- DAYOFWEEK ODBC function
- CURRENT_DATE ODBC function
- DAYNAME ODBC function
- CURTIME ODBC function
- functions, ODBC CURRENT_DATE
- MINUTE ODBC function
- functions, ODBC BIT_LENGTH
- functions, ODBC OCTET_LENGTH
- CONCAT ODBC function
- MONTHNAME ODBC function
- functions, ODBC DAYOFYEAR
- OCTET_LENGTH ODBC function
- functions [SQL Server], ODBC scalar functions
- BIT_LENGTH ODBC function
- functions, ODBC DAYOFMONTH
- CURRENT_TIME ODBC function
- functions, ODBC CURDATE
- functions, ODBC CURTIME
- functions, ODBC TRUNCATE
- functions, ODBC MONTHNAME
- functions, ODBC DAYNAME
- ODBC scalar functions
- functions, ODBC MINUTE
- functions, ODBC DAYOFWEEK
- QUARTER ODBC function
- DAYOFYEAR ODBC function
- functions, ODBC WEEK
- HOUR ODBC function
ms.assetid: a0df1ac2-6699-4ac0-8f79-f362f23496f1
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0f40b647d524aedb70c80bc54a22da0ffee97e21
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754377"
---
# <a name="odbc-scalar-functions-transact-sql"></a>ODBC 标量函数 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  可以在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中使用 [ODBC 标量函数](../../odbc/reference/appendixes/appendix-e-scalar-functions.md)。 这些语句由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 解释。 它们可以用在存储过程和用户定义函数中。 这些函数包括字符串函数、数值函数、时间函数、日期函数、时间间隔函数和系统函数。  
  
## <a name="usage"></a>使用情况  
 ```syntaxsql
 SELECT {fn <function_name> [ (<argument>,....n) ] }
 ```
  
## <a name="functions"></a>函数  
 以下各表列出了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中非重复的 ODBC 标量函数。  
  
### <a name="string-functions"></a>字符串函数  
  
|函数|说明|  
|--------------|-----------------|  
|BIT_LENGTH( string_exp ) (ODBC 3.0)|返回字符串表达式的长度（以位为单位）。<br /><br /> 返回给定数据类型的内部大小，而不将 string_exp 转换为字符串。|  
|CONCAT( string_exp1,string_exp2) (ODBC 1.0)|返回一个表示将 string_exp2 连接到 string_exp1 的结果的字符串。 生成的字符串依赖于 DBMS。 例如，如果 string_exp1 所表示的列包含一个 NULL 值，则 DB2 将返回 NULL，但 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回非 NULL 的字符串。|  
|OCTET_LENGTH( string_exp ) (ODBC 3.0)|返回字符串表达式的长度（以字节为单位）。 结果为不小于位数除以 8 所得数的最小整数。<br /><br /> 返回给定数据类型的内部大小，而不将 string_exp 转换为字符串。|  
  
### <a name="numeric-function"></a>数值函数  
  
|函数|说明|  
|--------------|-----------------|  
|TRUNCATE( numeric_exp, integer_exp) (ODBC 2.0)|返回截断到小数点右侧 integer_exp 位置的 numeric_exp。 如果 integer_exp 为负数，则 numeric_exp 会截断到小数点左侧 &#124;integer_exp&#124; 位置。|  
  
### <a name="time-date-and-interval-functions"></a>时间、日期和时间间隔函数  
  
|函数|说明|  
|--------------|-----------------|  
|CURRENT_DATE( ) (ODBC 3.0)|返回当前日期。|  
|CURDATE( ) (ODBC 3.0)|返回当前日期。|  
|CURRENT_TIME`[( time-precision )]` (ODBC 3.0)|返回当前本地时间。 time-precision 参数确定返回值的秒精度。|  
|CURTIME() (ODBC 3.0)|返回当前本地时间。|  
|DAYNAME( date_exp ) (ODBC 2.0)|返回一个字符串，其中包含 date_exp 的“星期几”部分的数据源特定的星期名称。 例如，对于使用英语的数据源， 返回 Sunday 到 Saturday 或 Sun. 到 Sat.； 或 Sun. 至 Sat.。 对于使用德语的数据源，返回 Sonntag 至 Samstag。|
|DAYOFMONTH( date_exp ) (ODBC 1.0)|根据 date_exp 中的月份字段以整数形式返回当月的具体日期。 返回 1 至 31 范围内的值。|  
|DAYOFWEEK( date_exp ) (ODBC 1.0)|根据 date_exp 中的“周”字段以整数形式返回在该周的天数。 返回 1 至 7 范围内的值，其中 1 表示星期天。|  
|HOUR( time_exp ) (ODBC 1.0)|根据 time_exp 中的“小时”字段返回具体小时，返回值为 0 到 23 范围内的整数值。|  
|MINUTE( time_exp ) (ODBC 1.0)|根据 time_exp 中的分钟字段返回具体分钟，返回值为 0 到 59 范围内的整数值。|  
|SECOND( time_exp ) (ODBC 1.0)|根据 time_exp 中的秒数字段返回具体秒数，返回值为 0 到 59 范围内的整数值。|  
|MONTHNAME( date_exp ) (ODBC 2.0)|返回一个字符串，其中包含 date_exp 的“月份”部分的数据源特定的月份名称。 例如，对于使用英语的数据源，返回 January 至 December 或 Jan. 至 Dec.。 对于使用德语的数据源，返回 Januar 至 Dezember。|  
|QUARTER( date_exp ) (ODBC 1.0)|返回 date_exp 中的该季度，返回值为 1 到 4 范围内的整数值，其中 1 表示 1 月 1 日到 3 月 31 日。|  
|WEEK( date_exp ) (ODBC 1.0)|根据 date_exp 中的“星期”字段返回当年的具体星期数，返回值为 1 到 53 范围内的整数值。|  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-an-odbc-function-in-a-stored-procedure"></a>A. 在存储过程中使用 ODBC 函数  
 下例在存储过程中使用了 ODBC 函数：  
  

```sql 
CREATE PROCEDURE dbo.ODBCprocedure  
(  
    @string_exp NVARCHAR(4000)  
)  
AS  
SELECT {fn OCTET_LENGTH( @string_exp )};  
```  
  
### <a name="b-using-an-odbc-function-in-a-user-defined-function"></a>B. 在用户定义函数中使用 ODBC 函数  
 下例在用户定义函数中使用了 ODBC 函数：  
  
```sql  
CREATE FUNCTION dbo.ODBCudf  
(  
    @string_exp NVARCHAR(4000)  
)  
RETURNS INT  
AS  
BEGIN  
DECLARE @len INT  
SET @len = (SELECT {fn OCTET_LENGTH( @string_exp )})  
RETURN(@len)  
END ;  
  
SELECT dbo.ODBCudf('Returns the length.');  
--Returns 38  
```  
  
### <a name="c-using-an-odbc-functions-in-select-statements"></a>C. 在 SELECT 语句中使用 ODBC 函数  
 下面的 SELECT 语句使用了 ODBC 函数：  
  
```sql 
DECLARE @string_exp NVARCHAR(4000) = 'Returns the length.';  
SELECT {fn BIT_LENGTH( @string_exp )};  
-- Returns 304  
SELECT {fn OCTET_LENGTH( @string_exp )};  
-- Returns 38  
  
SELECT {fn CONCAT( 'CONCAT ','returns a character string')};  
-- Returns CONCAT returns a character string  
SELECT {fn TRUNCATE( 100.123456, 4)};  
-- Returns 100.123400  
SELECT {fn CURRENT_DATE( )};  
-- Returns 2007-04-20  
SELECT {fn CURRENT_TIME(6)};  
-- Returns 10:27:11.973000  
  
DECLARE @date_exp NVARCHAR(30) = '2007-04-21 01:01:01.1234567';  
SELECT {fn DAYNAME( @date_exp )};  
-- Returns Saturday  
SELECT {fn DAYOFMONTH( @date_exp )};  
-- Returns 21  
SELECT {fn DAYOFWEEK( @date_exp )};  
-- Returns 7  
SELECT {fn HOUR( @date_exp)};  
-- Returns 1   
SELECT {fn MINUTE( @date_exp )};  
-- Returns 1  
SELECT {fn SECOND( @date_exp )};  
-- Returns 1  
SELECT {fn MONTHNAME( @date_exp )};  
-- Returns April  
SELECT {fn QUARTER( @date_exp )};  
-- Returns 2  
SELECT {fn WEEK( @date_exp )};  
-- Returns 16  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-an-odbc-function-in-a-stored-procedure"></a>D. 在存储过程中使用 ODBC 函数  
 下例在存储过程中使用了 ODBC 函数：  
  
```sql  
CREATE PROCEDURE dbo.ODBCprocedure  
(  
    @string_exp NVARCHAR(4000)  
)  
AS  
SELECT {fn BIT_LENGTH( @string_exp )};  
```  
  
### <a name="e-using-an-odbc-function-in-a-user-defined-function"></a>E. 在用户定义函数中使用 ODBC 函数  
 下例在用户定义函数中使用了 ODBC 函数：  
  
```sql  
CREATE FUNCTION dbo.ODBCudf  
(  
    @string_exp NVARCHAR(4000)  
)  
RETURNS INT  
AS  
BEGIN  
DECLARE @len INT  
SET @len = (SELECT {fn BIT_LENGTH( @string_exp )})  
RETURN(@len)  
END ;  
  
SELECT dbo.ODBCudf('Returns the length in bits.');  
--Returns 432  
```  
  
### <a name="f-using-an-odbc-functions-in-select-statements"></a>F. 在 SELECT 语句中使用 ODBC 函数  
 下面的 SELECT 语句使用了 ODBC 函数：  
  
```sql  
DECLARE @string_exp NVARCHAR(4000) = 'Returns the length.';  
SELECT {fn BIT_LENGTH( @string_exp )};  
-- Returns 304  
  
SELECT {fn CONCAT( 'CONCAT ','returns a character string')};  
-- Returns CONCAT returns a character string  
SELECT {fn CURRENT_DATE( )};  
-- Returns today's date  
SELECT {fn CURRENT_TIME(6)};  
-- Returns the time  
  
DECLARE @date_exp NVARCHAR(30) = '2007-04-21 01:01:01.1234567';  
SELECT {fn DAYNAME( @date_exp )};  
-- Returns Saturday  
SELECT {fn DAYOFMONTH( @date_exp )};  
-- Returns 21  
SELECT {fn DAYOFWEEK( @date_exp )};  
-- Returns 7  
SELECT {fn HOUR( @date_exp)};  
-- Returns 1   
SELECT {fn MINUTE( @date_exp )};  
-- Returns 1  
SELECT {fn SECOND( @date_exp )};  
-- Returns 1  
SELECT {fn MONTHNAME( @date_exp )};  
-- Returns April  
SELECT {fn QUARTER( @date_exp )};  
-- Returns 2  
SELECT {fn WEEK( @date_exp )};  
-- Returns 16  
```  
  
## <a name="see-also"></a>另请参阅  
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)