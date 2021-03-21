---
description: ISDATE (Transact-SQL)
title: ISDATE (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ISDATETIME
- ISDATE_TSQL
- ISDATE
- ISDATETIME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], ISDATE
- validate dates times [SQL Server]
- formats [SQL Server], time
- dates [SQL Server], validate
- verify dates times [SQL Server]
- functions [SQL Server], time
- formats [SQL Server], dates
- functions [SQL Server], date and time
- time [SQL Server], functions
- time [SQL Server], validate
- ISDATE function [SQL Server]
ms.assetid: 8e2c9ee7-388a-432f-b2c9-7b398f26bf85
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b4e5166cba81e1fd1b7d08c9de742e5c7088a7c3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746807"
---
# <a name="isdate-transact-sql"></a>ISDATE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  如果表达式是有效的 date、time、或 datetime 值，则返回 1；否则返回 0     。  
  
 如果表达式为 datetime2 值，则 ISDATE 返回 0   。  
  
 有关所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间数据类型及函数的概述，请参阅[日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。 请注意，datetime 数据的范围为 1753-01-01 至 9999-12-31，而日期数据的范围为 0001-01-01 至 9999-12-31。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ISDATE ( expression )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *expression*  
 字符串或者可以转换为字符串的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 表达式的长度不得超过 4,000 个字符。 不允许将日期和时间数据类型（datetime 和 smalldatetime 除外）作为 ISDATE 的参数。  
  
## <a name="return-type"></a>返回类型  
 **int**  
  
## <a name="remarks"></a>备注  
 只有与 [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) 函数一起使用，同时指定了 CONVERT 样式参数且样式不等于 0、100、9 或 109 时，ISDATE 才是确定性的。  
  
 ISDATE 的返回值取决于 [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)、[SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) 和[配置默认语言服务器配置选项](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md)设定的设置。  
  
## <a name="isdate-expression-formats"></a>ISDATE 表达式格式  
 有关 ISDATE 将返回 1 的有效格式的示例，请参阅 [datetime](../../t-sql/data-types/datetime-transact-sql.md) 和 [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) 主题中的“datetime 支持的字符串文字格式”部分。 有关其他示例，请参阅 [CAST 和 CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) 的“参数”部分中的“输入/输出”列。  
  
 下表概述了无效的输入表达式格式（返回 0 或错误）。  
  
|ISDATE 表达式|ISDATE 返回值|  
|-----------------------|-------------------------|  
|Null|0|  
|[数据类型](../../t-sql/data-types/data-types-transact-sql.md)中除字符串、Unicode 字符串或日期和时间以外的任何数据类型类别中列出的数据类型值。|0|  
|text、ntext 或 image 数据类型的值  。|0|  
|秒精度小数位数超过 3 的任何值（.0000 到 .0000000...n）。 如果表达式为 datetime2 值，ISDATE 将返回 0，但如果表达式是有效的 datetime 值，则将返回 1。|0|  
|有效日期和无效值混在一起的任何值，例如 1995-10-1a。|0|  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-isdate-to-test-for-a-valid-datetime-expression"></a>A. 使用 ISDATE 测试是否为有效的 datetime 表达式  
 下面的示例说明如何使用 `ISDATE` 测试某一字符串是否是有效的 datetime。  
  
```sql  
IF ISDATE('2009-05-12 10:19:41.177') = 1  
    PRINT 'VALID'  
ELSE  
    PRINT 'INVALID';  
```  
  
### <a name="b-showing-the-effects-of-the-set-dateformat-and-set-language-settings-on-return-values"></a>B. 显示 SET DATEFORMAT 和 SET LANGUAGE 设置对返回值的影响  
 下面的语句显示了作为 `SET DATEFORMAT` 和 `SET LANGUAGE` 设置的结果返回的值。  
  
```sql  
/* Use these sessions settings. */  
SET LANGUAGE us_english;  
SET DATEFORMAT mdy;  
/* Expression in mdy dateformat */  
SELECT ISDATE('04/15/2008'); --Returns 1.  
/* Expression in mdy dateformat */  
SELECT ISDATE('04-15-2008'); --Returns 1.   
/* Expression in mdy dateformat */  
SELECT ISDATE('04.15.2008'); --Returns 1.   
/* Expression in myd  dateformat */  
SELECT ISDATE('04/2008/15'); --Returns 1.  
  
SET DATEFORMAT mdy;  
SELECT ISDATE('15/04/2008'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('15/2008/04'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('2008/15/04'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
SET DATEFORMAT dmy;  
SELECT ISDATE('15/04/2008'); --Returns 1.  
SET DATEFORMAT dym;  
SELECT ISDATE('15/2008/04'); --Returns 1.  
SET DATEFORMAT ydm;  
SELECT ISDATE('2008/15/04'); --Returns 1.  
SET DATEFORMAT ymd;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
SET LANGUAGE English;  
SELECT ISDATE('15/04/2008'); --Returns 0.  
SET LANGUAGE Hungarian;  
SELECT ISDATE('15/2008/04'); --Returns 0.  
SET LANGUAGE Swedish;  
SELECT ISDATE('2008/15/04'); --Returns 0.  
SET LANGUAGE Italian;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
/* Return to these sessions settings. */  
SET LANGUAGE us_english;  
SET DATEFORMAT mdy;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-isdate-to-test-for-a-valid-datetime-expression"></a>C. 使用 ISDATE 测试是否为有效的 datetime 表达式  
 下面的示例说明如何使用 `ISDATE` 测试某一字符串是否是有效的 datetime。  
  
```sql  
IF ISDATE('2009-05-12 10:19:41.177') = 1  
    SELECT 'VALID';  
ELSE  
    SELECT 'INVALID';  
```  
  
## <a name="see-also"></a>另请参阅  
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
