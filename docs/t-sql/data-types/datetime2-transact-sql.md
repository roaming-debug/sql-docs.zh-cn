---
description: datetime2 (Transact-SQL)
title: datetime2 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetime2
- datetime2_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- dates [SQL Server], data types
- date and time [SQL Server], datetime2
- data types [SQL Server], date and time
- datetime2 data type [SQL Server]
ms.assetid: 868017f3-214f-43ef-8536-cc1632a2288f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 837f8bdf2680d62c2b4d6992fc97c65ef59842bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161287"
---
# <a name="datetime2-transact-sql"></a>datetime2 (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

定义结合了 24 小时制时间的日期。 可将 datetime2 视作现有 datetime 类型的扩展，其数据范围更大，默认的小数精度更高，并具有可选的用户定义的精度。
  
## <a name="datetime2-description"></a>datetime2 说明
  
|属性|值|  
|--------------|-----------|  
|语法|datetime2 [ (fractional seconds precision) ]|  
|使用情况|DECLARE \@MyDatetime2 datetime2(7)<br /><br /> CREATE TABLE Table1 ( Column1 datetime2(7))|  
|默认的字符串文字格式<br /><br /> （用于下级客户端）|YYYY-MM-DD hh:mm:ss[.fractional seconds]<br /><br /> 有关详细信息，请参阅后面的“下级客户端的向后兼容性”部分。|  
|日期范围|0001-01-01 到 31.12.99<br /><br /> 公元 1 年 1 月 1 日到公元 9999 年 12 月 31 日|  
|时间范围|00:00:00 到 23:59:59.9999999|  
|时区偏移量范围|无|  
|各元素的范围|YYYY 是一个四位数，范围从 0001 到 9999，表示年份。<br /><br /> MM 是一个两位数，范围从 01 到 12，它表示指定年份中的月份。<br /><br /> DD 是一个两位数，范围为 01 到 31（具体取决于月份），它表示指定月份中的某一天。<br /><br /> hh 是一个两位数，范围从 00 到 23，它表示小时。<br /><br /> mm 是一个两位数，范围从 00 到 59，它表示分钟。<br /><br /> ss 是一个两位数，范围从 00 到 59，它表示秒钟。<br /><br /> n* 代表 0 到 7 位数字，范围从 0 到 9999999，它表示秒小数部分。 在 Informatica 中，当 n > 3 时，秒的小数部分会被截断。|  
|字符长度|最低 19 位 (YYYY-MM-DD hh:mm:ss )，最高 27 位 (YYYY-MM-DD hh:mm:ss.0000000)|  
|精度、小数位数|0 至 7 位，准确度为 100ns。 默认精度为 7 位数。|  
|存储大小 <sup>1</sup>|精度小于 3 的 6 个字节。<br/>精度为 3 和 4 的 6 个字节。<br/>所有其他精度需要 8 个字节。<sup>2</sup>|  
|精确度|100 纳秒|  
|默认值|1900-01-01 00:00:00|  
|日历|公历|  
|用户定义的秒的小数部分精度|是|  
|时区偏移量感知和保留|否|  
|夏时制感知|否|  

<sup>1</sup> 提供的值用于未压缩的行存储。 使用[数据压缩](../../relational-databases/data-compression/data-compression.md)或[列存储](../../relational-databases/indexes/columnstore-indexes-overview.md)可能会改变每个精度的存储大小。 此外，磁盘上和内存中的存储大小可能有所不同。 例如，采用批处理模式时，datetime2 值始终需要 8 个字节的内存。

<sup>2</sup> datetime2 值强制转换为 varbinary 值时，会向 varbinary 值添加一个额外的字节，用于存储精度  。

有关数据类型元数据，请参阅 [sys.systypes (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md) 或 [TYPEPROPERTY (Transact-SQL)](../../t-sql/functions/typeproperty-transact-sql.md)。 某些日期和时间数据类型的精度和小数位数是可变的。 若要获取列的精度和小数位数，请参阅 [COLUMNPROPERTY (Transact-SQL)](../../t-sql/functions/columnproperty-transact-sql.md)、[COL_LENGTH (Transact-SQL)](../../t-sql/functions/col-length-transact-sql.md) 或 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)。
  
## <a name="supported-string-literal-formats-for-datetime2"></a>datetime2 支持的字符串文字格式
以下各表列出了适用于 datetime2 的支持的 ISO 8601 和 ODBC 字符串文字格式。 有关 datetime2 日期和时间部分的字母、数值、未分隔和时间格式的信息，请参阅 [date (Transact-SQL)](../../t-sql/data-types/date-transact-sql.md) 和 [time (Transact-SQL)](../../t-sql/data-types/time-transact-sql.md)。
  
|ISO 8601|说明|  
|---|---|
|YYYY-MM-DDThh:mm:ss[.nnnnnnn]<br /><br /> YYYY-MM-DDThh:mm:ss[.nnnnnnn]|此格式不受 SET LANGUAGE 和 SET DATEFORMAT 会话区域设置的影响。 包括在字符串内的 T、冒号 (:) 和句点 (.)，例如“2007-05-02T19:58:47.1234567”。|  
  
|ODBC|说明|  
|---|---|
|{ ts 'yyyy-mm-dd hh:mm:ss[.fractional seconds]' }|特定于 ODBC API：<br /><br /> 小数点右侧的数字表示秒小数部分，可指定 0 到 7 位（100 纳秒）。|  
  
## <a name="ansi-and-iso-8601-compliance"></a>对 ANSI 和 ISO 8601 的遵从性  
datetime2 符合 [date](../../t-sql/data-types/date-transact-sql.md) 和 [time](../../t-sql/data-types/time-transact-sql.md) 的 ANSI 和 ISO 8601 标准。
  
##  <a name="backward-compatibility-for-down-level-clients"></a>下级客户端的向后兼容性  
某些下级客户端不支持 time、time、datetime2 和 datetimeoffset 数据类型。 下表显示了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上级实例与下级客户端之间的类型映射。
  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型|传递给下级客户端的默认字符串文字格式|下级 ODBC|下级 OLEDB|下级 JDBC|下级 SQLCLIENT|  
| --- | --- | --- | --- | --- | --- |
|**time**|hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
|**date**|YYYY-MM-DD|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
|**datetime2**|YYYY-MM-DD hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
|**datetimeoffset**|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
  
## <a name="converting-date-and-time-data"></a>转换日期和时间数据
当转换为日期和时间数据类型时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将会拒绝它无法识别为日期或时间的所有值。 有关日期和时间数据使用 CAST 和 CONVERT 函数的信息，请参阅 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
### <a name="converting-other-date-and-time-types-to-the-datetime2-data-type"></a>将其他日期和时间类型转换为 datetime2 数据类型
本部分介绍其他日期和时间数据类型转换为 datetime2 数据类型时会发生什么。  
  
从 date 转换时，会复制年、月和日。  时间部分设置为 00:00:00.0000000。  下面的代码显示将 `date` 值转换为 `datetime2` 值的结果。  
  
```sql
DECLARE @date date = '12-21-16';
DECLARE @datetime2 datetime2 = @date;

SELECT @datetime2 AS '@datetime2', @date AS '@date';
  
--Result  
--@datetime2                  @date
----------------------------- ----------
--2016-12-21 00:00:00.0000000 2016-12-21
```  
  
从 time(n) 转换时，会复制时间部分，日期部分设置为“1900-01-01”。 下面的示例显示了将 `time(7)` 值转换为 `datetime2` 值的结果。  
  
```sql
DECLARE @time time(7) = '12:10:16.1234567';
DECLARE @datetime2 datetime2 = @time;

SELECT @datetime2 AS '@datetime2', @time AS '@time';
  
--Result  
--@datetime2                  @time
----------------------------- ----------------
--1900-01-01 12:10:16.1234567 12:10:16.1234567
```  
  
从 smalldatetime 转换时，会复制小时和分钟。 秒和秒的小数部分设置为 0。 下面的代码显示将 `smalldatetime` 值转换为 `datetime2` 值的结果。  
  
```sql
DECLARE @smalldatetime smalldatetime = '12-01-16 12:32';
DECLARE @datetime2 datetime2 = @smalldatetime;

SELECT @datetime2 AS '@datetime2', @smalldatetime AS '@smalldatetime'; 
  
--Result  
--@datetime2                  @smalldatetime
----------------------------- -----------------------
--2016-12-01 12:32:00.0000000 2016-12-01 12:32:00 
```  
  
从 datetimeoffset(n) 转换时，会复制日期和时间部分。 时区被截断。 下面的示例显示了将 `datetimeoffset(7)` 值转换为 `datetime2` 值的结果。  
  
```sql
DECLARE @datetimeoffset datetimeoffset(7) = '2016-10-23 12:45:37.1234567 +10:0';
DECLARE @datetime2 datetime2 = @datetimeoffset;

SELECT @datetime2 AS '@datetime2', @datetimeoffset AS '@datetimeoffset'; 
  
--Result  
--@datetime2                  @datetimeoffset
----------------------------- ----------------------------------
--2016-10-23 12:45:37.1234567 2016-10-23 12:45:37.1234567 +10:00
```  

从 datetime 转换时，会复制日期和时间。 小数精度扩展到 7 位。 下面的示例显示了将 `datetime` 值转换为 `datetime2` 值的结果。

```sql
DECLARE @datetime datetime = '2016-10-23 12:45:37.333';
DECLARE @datetime2 datetime2 = @datetime;

SELECT @datetime2 AS '@datetime2', @datetime AS '@datetime';
   
--Result  
--@datetime2                  @datetime
------------------------- ---------------------------
--2016-10-23 12:45:37.3333333 2016-10-23 12:45:37.333
```  

> [!NOTE]
> 在数据库兼容性级别 130 下，通过考虑导致不同转换值的毫秒小数部分，从 datetime 到 datetime2 数据类型的隐式转换更加准确，如上例中所示。 只要 datetime 和 datetime2 数据类型之间存在混合比较情况，就需要使用 datetime2 数据类型的隐式转换。 有关详细信息，请参阅此 [Microsoft 支持文章](https://support.microsoft.com/help/4010261)。

### <a name="converting-string-literals-to-datetime2"></a>将字符串文字转换为 datetime2  
如果字符串所有部分的格式均有效，则允许从字符串文字转换为日期和时间类型。 否则，将引发运行时错误。 从日期和时间类型向字符串文字进行的未指定样式的隐式转换或显式转换将采用当前会话的默认格式。 下表显示用于将字符串文字转换为 datetime2 数据类型的规则。
  
|输入字符串文字|**datetime2(n)**|  
|---|---|
|ODBC DATE|ODBC 字符串文字映射到 datetime 数据类型。 从 ODBC DATETIME 文字到 datetime2 类型的任何赋值操作都会导致在 datetime 与此类型之间按照转换规则的定义进行隐式转换。|  
|ODBC TIME|请参阅前面的 ODBC DATE 规则。|  
|ODBC DATETIME|请参阅前面的 ODBC DATE 规则。|  
|仅 DATE|TIME 部分默认为 00:00:00。|  
|仅 TIME|DATE 部分默认为 1900-1-1。|  
|仅 TIMEZONE|提供默认值。|  
|DATE + TIME|无庸赘述|  
|DATE + TIMEZONE|不允许。|  
|TIME + TIMEZONE|DATE 部分默认为 1900-1-1。 忽略 TIMEZONE 输入。|  
|DATE + TIME + TIMEZONE|将使用本地 DATETIME。|  
  
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
  
  
