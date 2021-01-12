---
description: DATETIMEOFFSETFROMPARTS (Transact-SQL)
title: DATETIMEOFFSETFROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATETIMEOFFSETFROMPARTS_TSQL
- DATETIMEOFFSETFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIMEOFFSETFROMPARTS function
ms.assetid: 463da1f4-b4b6-45a3-9a95-ea1f99575542
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ab37520d60c482afccb40bde7392de2515a79753
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101183"
---
# <a name="datetimeoffsetfromparts-transact-sql"></a>DATETIMEOFFSETFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

返回指定日期和时间参数的 datetimeoffset 值。 返回值包含精度参数指定的精度，以及偏移参数指定的偏移量。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DATETIMEOFFSETFROMPARTS ( year, month, day, hour, minute, seconds, fractions, hour_offset, minute_offset, precision )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

*year*  
指定年份的整数表达式。  
  
*month*  
指定月份的整数表达式。  
  
*day*  
指定日期的整数表达式。  
  
hour  
指定小时的整数表达式。  
  
minute  
指定分钟的整数表达式。  
  
*seconds*  
指定秒数的整数表达式。  
  
fractions  
指定秒的小数形式值的整数表达式。  
  
hour_offset  
整数表达式，用于指定时区偏移量的小时部分。  
  
minute_offset  
整数表达式，用于指定时区偏移量的分钟部分。  
  
*精度*  
整数文本值，用于指定 `DATETIMEOFFSETFROMPARTS` 要返回的 datetimeoffset 值的精度。  
  
## <a name="return-types"></a>返回类型
datetimeoffset( precision )    
  
## <a name="remarks"></a>注解  

`DATETIMEOFFSETFROMPARTS` 返回已完全初始化的 datetimeoffset 数据类型。 偏移参数表示时区偏移量。 对于省略的偏移参数，`DATETIMEOFFSETFROMPARTS` 假定时区偏移量为 `00:00`，换言之，即没有时区偏移量。 对于指定的偏移参数，`DATETIMEOFFSETFROMPARTS` 需要这两个参数的值，要么都是正值，要么都是负值。 如果 minute_offset 具有值而 hour_offset 没有值，`DATETIMEOFFSETFROMPARTS` 将引发错误。 如果其他参数具有无效值，`DATETIMEOFFSETFROMPARTS` 将引发错误。 如果至少一个必需参数具有 `NULL` 值，则 `DATETIMEOFFSETFROMPARTS` 将返回 `NULL`。 但是，如果 precision 参数具有 `NULL` 值，`DATETIMEOFFSETFROMPARTS` 将引发错误。  
  
fractions 参数取决于 precision 参数。 例如，如果 precision 值为 7，则每个分数表示 100 纳秒；如果 precision 为 3，则每个分数表示 1 毫秒。 如果 precision 的值为零，则 fractions 的值也必须为零；否则 `DATETIMEOFFSETFROMPARTS` 将引发错误。  
  
此函数支持在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 及更高版本的服务器上远程执行。 但不支持在版本低于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的服务器上远程执行。
  
## <a name="examples"></a>示例  
  
### <a name="a-an-example-without-fractions-of-a-second"></a>A. 不包含秒的小数部分的示例  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2010, 12, 31, 14, 23, 23, 0, 12, 0, 7 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
----------------------------------
2010-12-31 14:23:23.0000000 +12:00  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. 包含秒的小数部分的示例  

此示例介绍了 fractions 和 precision 参数的用法：  

1. 如果 fractions 的值为 5、precision 的值为 1，则 fractions 的值表示 5/10 秒。  

2. 如果 fractions 的值为 50、precision 的值为 2，则 fractions 的值表示 50/100 秒。  

3. 如果 fractions 的值为 500、precision 的值为 3，则 fractions 的值表示 500/1000 秒。  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 5, 12, 30, 1 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 50, 12, 30, 2 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 500, 12, 30, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------------------------------  
2011-08-15 14:30:00.5 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.50 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.500 +12:30  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅
[datetimeoffset (Transact-SQL)](../../t-sql/data-types/datetimeoffset-transact-sql.md)  
[AT TIME ZONE (Transact-SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)
  
  


