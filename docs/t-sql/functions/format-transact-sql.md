---
title: FORMAT (Transact-SQL) | Microsoft Docs
description: FORMAT 函数的 Transact-SQL 参考。
ms.date: 08/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FORMAT_TSQL
- FORMAT
dev_langs:
- TSQL
helpviewer_keywords:
- FORMAT function
ms.assetid: dad6f24c-b8d9-4dbe-a561-9b167b8f20c8
author: cawrites
ms.author: chadam
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: bfd9a1079dded6f6ffa466e8161aa98eb20c0106
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093566"
---
# <a name="format-transact-sql"></a>FORMAT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

返回以指定的格式和可选的区域性格式化的值。 使用 FORMAT 函数将日期/时间和数字值格式化为识别区域设置的字符串。 对于一般的数据类型转换，请使用 CAST 或 CONVERT。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
FORMAT( value, format [, culture ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

 *value*  
 支持格式化的数据类型的表达式。 有关有效类型的列表，请参阅下面“备注”部分中的表。  
  
 format   
 nvarchar 格式模式  。  
  
 format 参数必须包含一个有效的 .NET Framework 格式字符串，要么作为标准格式字符串（例如，“C”或“D”），要么作为日期值和数值的自定义字符模式（例如，“MMMM DD, yyyy (dddd)”）  。 不支持组合格式。 有关这些格式模式的完整解释，请查阅有关常规字符串格式、自定义日期和时间格式以及自定义数字格式的 .NET Framework 文档。 一个好的起点是主题“[格式类型](https://go.microsoft.com/fwlink/?LinkId=211776)”。  
  
 *区域性*  
 指定区域性的可选 nvarchar 参数  。  
  
 如果未提供 culture 参数，则使用当前会话的语言  。 可以使用 SET LANGUAGE 语句隐式或显式设置此语言。 culture 接受 .NET Framework 支持的任何区域性作为参数；它不局限于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 显式支持的语言  。 如果 culture 参数无效，FORMAT 将引发错误  。  
  
## <a name="return-types"></a>返回类型

 nvarchar 或 Null   
  
 返回值的长度由 format 确定  。  
  
## <a name="remarks"></a>备注

 FORMAT 将返回 NULL 错误，而不是非 valid 的 culture   。 例如，如果 format 中指定的值无效，则返回 NULL  。  

 FORMAT 函数具有不确定性。
  
 FORMAT 依赖于 .NET Framework 公共语言运行时 (CLR) 的存在。  
  
 此函数无法进行远程处理，因为它依赖于 CLR 的存在。 远程处理需要 CLR 的函数可能导致在远程服务器上出现错误。  
  
 FORMAT 依赖于 CLR 格式设置规则，规则规定冒号和句点必须进行转义。 因此，当格式字符串（第二个参数）包含冒号或句点时，如果输入值（第一个参数）属于 time 数据类型，则冒号或句点必须使用反斜杠转义  。 请参阅[时间数据类型的 D. FORMAT](#ExampleD)。  
  
 下表列出了 value 参数可接受的数据类型，其中还有相关的 .NET Framework 映射等效类型  。  
  
|类别|类型|.NET 类型|  
|--------------|----------|---------------|  
|Numeric|bigint|Int64|  
|Numeric|int|Int32|  
|Numeric|smallint|Int16|  
|Numeric|tinyint|Byte|  
|Numeric|Decimal|SqlDecimal|  
|Numeric|numeric|SqlDecimal|  
|Numeric|FLOAT|Double|  
|Numeric|real|Single|  
|Numeric|smallmoney|Decimal|  
|Numeric|money|Decimal|  
|日期和时间|date|DateTime|  
|日期和时间|time|TimeSpan|  
|日期和时间|datetime|DateTime|  
|日期和时间|smalldatetime|DateTime|  
|日期和时间|datetime2|DateTime|  
|日期和时间|datetimeoffset|DateTimeOffset|  
  
## <a name="examples"></a>示例  
  
### <a name="a-simple-format-example"></a>A. 简单 FORMAT 示例

 下面的示例返回针对不同区域性格式化的简单日期。  
  
```sql  
DECLARE @d DATE = '11/22/2020';
SELECT FORMAT( @d, 'd', 'en-US' ) 'US English'  
      ,FORMAT( @d, 'd', 'en-gb' ) 'Great Britain English'  
      ,FORMAT( @d, 'd', 'de-de' ) 'German'  
      ,FORMAT( @d, 'd', 'zh-cn' ) 'Simplified Chinese (PRC)';  
  
SELECT FORMAT( @d, 'D', 'en-US' ) 'US English'  
      ,FORMAT( @d, 'D', 'en-gb' ) 'Great Britain English'  
      ,FORMAT( @d, 'D', 'de-de' ) 'German'  
      ,FORMAT( @d, 'D', 'zh-cn' ) 'Chinese (Simplified PRC)';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
US English  Great Britain English German     Simplified Chinese (PRC)  
----------  --------------------- ---------- ------------------------  
11/22/2020  22/11/2020            22.11.2020 2020/11/22 
  
US English                  Great Britain English  German                      Chinese (Simplified PRC)  
--------------------------- ---------------------- --------------------------  ---------------------------------------  
Sunday, November 22, 2020   22 November 2020       Sonntag, 22. November 2020  2020年11月22日  
  
```  
  
### <a name="b-format-with-custom-formatting-strings"></a>B. 使用自定义格式字符串执行 FORMAT

 以下示例通过指定自定义格式显示格式数值。 该示例假定当前日期为 2012 年 9 月 27 日。 有关这些格式和其他自定义格式的详细信息，请参阅[自定义数字格式字符串](https://msdn.microsoft.com/library/0c899ak8.aspx)。  
  
```sql  
DECLARE @d DATE = GETDATE();  
SELECT FORMAT( @d, 'dd/MM/yyyy', 'en-US' ) AS 'Date'  
       ,FORMAT(123456789,'###-##-####') AS 'Custom Number';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Date        Custom Number  
----------  -------------  
22/11/2020  123-45-6789  
  
```  
  
### <a name="c-format-with-numeric-types"></a>C. 用于数值类型的 FORMAT

 下面的示例返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 Sales.CurrencyRate 表中的 5 个行  。 EndOfDateRate 列在该表中作为 money 类型存储   。 在本示例中，该列以非格式化形式返回，然后通过指定 .NET 数字格式、常规格式和货币格式类型进行格式化。 有关这些格式和其他数字格式的详细信息，请参阅[标准数字格式字符串](https://msdn.microsoft.com/library/dwhawy9k.aspx)。  
  
```sql  
SELECT TOP(5) CurrencyRateID, EndOfDayRate  
            ,FORMAT(EndOfDayRate, 'N', 'en-us') AS 'Number Format'  
            ,FORMAT(EndOfDayRate, 'G', 'en-us') AS 'General Format'  
            ,FORMAT(EndOfDayRate, 'C', 'en-us') AS 'Currency Format'  
FROM Sales.CurrencyRate  
ORDER BY CurrencyRateID;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
CurrencyRateID EndOfDayRate  Numeric Format  General Format  Currency Format  
-------------- ------------  --------------  --------------  ---------------  
1              1.0002        1.00            1.0002          $1.00  
2              1.55          1.55            1.5500          $1.55  
3              1.9419        1.94            1.9419          $1.94  
4              1.4683        1.47            1.4683          $1.47  
5              8.2784        8.28            8.2784          $8.28  
  
```  
  
 此示例指定了德语区域性 (de-de)。  
  
```sql  
SELECT TOP(5) CurrencyRateID, EndOfDayRate  
      ,FORMAT(EndOfDayRate, 'N', 'de-de') AS 'Numeric Format'  
      ,FORMAT(EndOfDayRate, 'G', 'de-de') AS 'General Format'  
      ,FORMAT(EndOfDayRate, 'C', 'de-de') AS 'Currency Format'  
FROM Sales.CurrencyRate  
ORDER BY CurrencyRateID;  
```  
  
```
CurrencyRateID EndOfDayRate  Numeric Format  General Format  Currency Format  
-------------- ------------  --------------  --------------  ---------------  
1              1.0002        1,00            1,0002          1,00 &euro;  
2              1.55          1,55            1,5500          1,55 &euro;  
3              1.9419        1,94            1,9419          1,94 &euro;  
4              1.4683        1,47            1,4683          1,47 &euro;  
5              8.2784        8,28            8,2784          8,28 &euro;  
  
```  
  
### <a name="d-format-with-time-data-types"></a><a name="ExampleD"></a> D. 时间数据类型的 D. FORMAT

 FORMAT 在这些情况下返回 NULL，因为 `.` 和 `:` 未进行转义。  
  
```sql  
SELECT FORMAT(cast('07:35' as time), N'hh.mm');   --> returns NULL  
SELECT FORMAT(cast('07:35' as time), N'hh:mm');   --> returns NULL  
```  
  
 Format 返回格式化的字符串，因为 `.` 和 `:` 已进行转义。  
  
```sql  
SELECT FORMAT(cast('07:35' as time), N'hh\.mm');  --> returns 07.35  
SELECT FORMAT(cast('07:35' as time), N'hh\:mm');  --> returns 07:35  
```  

Format 返回指定了 AM 或 PM 的格式化当前时间

```sql
SELECT FORMAT(SYSDATETIME(), N'hh:mm tt'); -- returns 03:46 PM
SELECT FORMAT(SYSDATETIME(), N'hh:mm t'); -- returns 03:46 P
```

Format 返回显示 AM 的指定时间

```sql
select FORMAT(CAST('2018-01-01 01:00' AS datetime2), N'hh:mm tt') -- returns 01:00 AM
select FORMAT(CAST('2018-01-01 01:00' AS datetime2), N'hh:mm t')  -- returns 01:00 A
```

Format 返回显示 PM 的指定时间

```sql
select FORMAT(CAST('2018-01-01 14:00' AS datetime2), N'hh:mm tt') -- returns 02:00 PM
select FORMAT(CAST('2018-01-01 14:00' AS datetime2), N'hh:mm t') -- returns 02:00 P
```
  
Format 返回采用 24 小时格式的指定时间

```sql
select FORMAT(CAST('2018-01-01 14:00' AS datetime2), N'HH:mm') -- returns 14:00
```
  
## <a name="see-also"></a>另请参阅

- [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
- [STR (Transact-SQL)](../../t-sql/functions/str-transact-sql.md)  
- [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)
