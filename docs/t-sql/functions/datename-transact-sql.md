---
description: DATENAME (Transact-SQL)
title: DATENAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATENAME_TSQL
- DATENAME
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- DATENAME function [SQL Server
- functions [SQL Server], time
- functions [SQL Server], date and time
- dateparts [SQL Server], datename
- date and time [SQL Server], DATENAME
- comparing dates times [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 11855b56-c554-495d-aad4-ba446990153b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b7b246557e68cedc9995aca8fb7a5b943b4195ce
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752117"
---
# <a name="datename-transact-sql"></a>DATENAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数返回表示指定 date 的指定 datepart 的字符串。

有关所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间数据类型及函数的概述，请参阅[日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DATENAME ( datepart , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*datepart*  
`DATENAME` 将返回的 date 参数的特定部分。 此表列出了所有有效的 datepart 参数  。

> [!NOTE]
> 对于 datepart 参数，`DATENAME` 不接受用户定义的变量等效项  。
  
|*datepart*|缩写形式|  
|---|---|
|**year**|**yy, yyyy**|  
|quarter |**qq, q**|  
|month |**mm, m**|  
|dayofyear |**dy, y**|  
|day |**dd, d**|  
|week |**wk, ww**|  
|weekday |**dw, w**|  
|hour|**hh**|  
|minute |**mi, n**|  
|second |**ss, s**|  
|millisecond |ms |  
|microsecond |mcs |  
|nanosecond |ns |  
|**TZoffset**|**tz**|  
|**ISO_WEEK**|**ISOWK, ISOWW**|  
  
*date*  

可解析为下列某种数据类型的表达式： 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

对于 date，`DATENAME` 接受列表达式、表达式、字符串文本或用户定义的变量  。 使用四位数年份可避免含糊不清问题。 有关两位数年份的信息，请参阅[配置两位数年份截止服务器配置选项](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md)。
  
## <a name="return-type"></a>返回类型  
**nvarchar**
  
## <a name="return-value"></a>返回值  
  
-   每个 datepart 及其缩写都返回相同的值  。  
  
返回值取决于 [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) 和登录时选择的[配置默认语言服务器配置选项](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md)设定的语言环境。 如果 date 是某些格式的字符串文本，则返回值取决于 [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)  。 当 date 为日期或时间数据类型的列表达式时，SET DATEFORMAT 不更改返回值。
  
当 date 参数具有 date 数据类型参数时，返回值取决于 [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md) 指定的设置。
  
## <a name="tzoffset-datepart-argument"></a>TZoffset 日期部分参数  
如果 datepart 参数为 TZoffset (tz)，并且 date 参数没有时区偏移量，则 `DATEADD` 返回 0。
  
## <a name="smalldatetime-date-argument"></a>smalldatetime 日期参数  
当 date 为 [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) 时，`DATENAME` 返回的秒显示为 00。
  
## <a name="default-returned-for-a-datepart-that-is-not-in-the-date-argument"></a>对不在日期参数中的日期部分返回默认值  
如果 date 参数的数据类型不具有特定的 datepart，则仅当 date 参数具有文本时，`DATENAME` 才返回该 datepart 的默认值。
  
例如，任意 date 数据类型的默认年-月-日都为 1900-01-01  。 此语句具有 datepart 的日期部分参数、date 的时间参数，并且 `DATENAME` 返回 `1900, January, 1, 1, Monday`。
  
```sql
SELECT DATENAME(year, '12:10:30.123')  
    ,DATENAME(month, '12:10:30.123')  
    ,DATENAME(day, '12:10:30.123')  
    ,DATENAME(dayofyear, '12:10:30.123')  
    ,DATENAME(weekday, '12:10:30.123');  
```  
  
如果将 date 指定为变量或表列，并且该变量或列的数据类型没有指定的 datepart，`DATENAME` 将返回错误 9810   。 在此示例中，变量 \@t 具有 time 数据类型   。 示例失败，因为日期部分年份对于 time 数据类型无效  ：
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATENAME(year, @t);  
```  
  
## <a name="remarks"></a>备注  

在以下子句中使用 `DATENAME`：

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE
  
在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中，DATENAME 将字符串文字作为 datetime2 类型隐式转换。 也就是说，日期在作为字符串传递时，`DATENAME` 不支持 YDM 格式。 必须先将字符串显式转换为 datetime 或 smalldatetime 类型，然后才能使用 YDM 格式   。
  
## <a name="examples"></a>示例  
此示例返回指定日期的日期部分。 用表中的 datepart 值替换 SELECT 语句中的 `datepart` 参数：
  
`SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');`
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|返回值|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|month, mm, m |10 月|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|星期二|  
|hour, hh |12|  
|minute, n |15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|+05:10|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

此示例返回指定日期的日期部分。 用表中的 datepart 值替换 SELECT 语句中的 `datepart` 参数：
  
```sql
SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|返回值|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|month, mm, m |10 月|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|星期二|  
|hour, hh |12|  
|minute, n |15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|+05:10|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

