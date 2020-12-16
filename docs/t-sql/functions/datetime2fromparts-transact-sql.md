---
description: DATETIME2FROMPARTS (Transact-SQL)
title: DATETIME2FROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATETIME2FROMPARTS_TSQL
- DATETIME2FROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIME2FROMPARTS function
ms.assetid: 632b757d-d2d1-43a5-b870-792a779ae204
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 46dde800f82285639093b015f621902a727d33b1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478678"
---
# <a name="datetime2fromparts-transact-sql"></a>DATETIME2FROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数对指定日期和时间参数返回 datetime2 值。 返回值具有由 precision 参数指定的精度。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DATETIME2FROMPARTS ( year, month, day, hour, minute, seconds, fractions, precision )  
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
  
*精度*  
整数表达式，用于指定 `DATETIME2FROMPARTS` 将返回的 datetime2 值的精度。
  
## <a name="return-types"></a>返回类型
datetime2( precision )  
  
## <a name="remarks"></a>注解  
`DATETIME2FROMPARTS` 返回完全初始化的 datetime2 值。 如果至少有一个必需参数具有无效值，`DATETIME2FROMPARTS` 将引发错误。 如果至少有一个必需参数具有 NULL 值，则 `DATETIME2FROMPARTS` 返回 NULL。 但是，如果 precision 参数具有 NULL 值，`DATETIME2FROMPARTS` 将引发错误。

fractions 参数取决于 precision 参数。 例如，如果 precision 值为 7，则每个分数表示 100 纳秒；如果 precision 为 3，则每个分数表示 1 毫秒。 如果 precision 的值为零，则 fractions 的值也必须为零；否则 `DATETIME2FROMPARTS` 将引发错误。
  
此函数支持在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 及更高版本的服务器上远程执行。 但不支持在版本低于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的服务器上远程执行。
  
## <a name="examples"></a>示例  
  
### <a name="a-an-example-without-fractions-of-a-second"></a>A. 不包含秒的小数部分的示例  
  
```sql
SELECT DATETIME2FROMPARTS ( 2010, 12, 31, 23, 59, 59, 0, 0 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
---------------------------  
2010-12-31 23:59:59.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. 包含秒的小数部分的示例  
此示例介绍了 fractions 和 precision 参数的用法：
  
1.  如果 fractions 的值为 5、precision 的值为 1，则 fractions 的值表示 5/10 秒。  
  
2.  如果 fractions 的值为 50、precision 的值为 2，则 fractions 的值表示 50/100 秒。  
  
3.  如果 fractions 的值为 500、precision 的值为 3，则 fractions 的值表示 500/1000 秒。  
  
```sql
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 5, 1 );  
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 50, 2 );  
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 500, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------------------  
2011-08-15 14:23:44.5  
  
(1 row(s) affected)  
  
----------------------  
2011-08-15 14:23:44.50  
  
(1 row(s) affected)  
  
----------------------  
2011-08-15 14:23:44.500  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅
[datetime2 (Transact-SQL)](../../t-sql/data-types/datetime2-transact-sql.md)
  
  

