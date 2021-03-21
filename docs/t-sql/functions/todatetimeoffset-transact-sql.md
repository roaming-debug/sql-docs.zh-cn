---
description: TODATETIMEOFFSET (Transact-SQL)
title: TODATETIMEOFFSET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TO_DATETIMEOFFSET_TSQL
- SWITCH_TZ_TSQL
- SWITCH_TZ
- TO_DATETIMEOFFSET
dev_langs:
- TSQL
helpviewer_keywords:
- date and time [SQL Server], TODATETIMEOFFSET
- TODATETIMEOFFSET function
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: b5fafc08-efd4-4a3b-a0b3-068981a0a685
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: edd9789d9aecc31b8f8e2dcf5682b08612df124b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104739007"
---
# <a name="todatetimeoffset-transact-sql"></a>TODATETIMEOFFSET (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回从 datetime2 表达式转换的 datetimeoffset 值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
TODATETIMEOFFSET ( datetime_expression , timezoneoffset_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 datetime_expression  
 一个解析为 [datetime2](../../t-sql/data-types/datetime2-transact-sql.md) 值的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
> [!NOTE]  
>  该表达式的类型不能为 text、ntext 或 image，因为这些类型无法隐式转换为 varchar 或 nvarchar。  
  
 timezoneoffset_expression  
 是表示时区偏移量（分钟）（如果为整数）的表达式，例如 -120；或表示小时和分钟数的表达式（如果为字符串），例如“+13.00”。 范围为 +14 到 -14（小时）。 此表达式被解释为指定的 timezoneoffset_expression 的本地时间。  

  
> [!NOTE]  
>  如果表达式是字符串，其格式必须为 {+|-}TZH:THM。  
  
## <a name="return-type"></a>返回类型  
 datetimeoffset。 小数精度与 datetime_expression 参数相同。  
  
## <a name="examples"></a>示例  
  
### <a name="a-changing-the-time-zone-offset-of-the-current-date-and-time"></a>A. 更改当前日期和时间的时区偏移量  
 下面的示例将当前日期和时间的时区偏移量更改为时区 `-07:00`。  
  
```sql  
DECLARE @todaysDateTime DATETIME2;  
SET @todaysDateTime = GETDATE();  
SELECT TODATETIMEOFFSET (@todaysDateTime, '-07:00');  
-- RETURNS 2019-04-22 16:23:51.7666667 -07:00  
```  
  
### <a name="b-changing-the-time-zone-offset-in-minutes"></a>B. 更改时区偏移量（以分钟为单位）  
 下面的示例将当前时区更改为 `-120` 分钟。  
  
```sql  
SELECT TODATETIMEOFFSET(SYSDATETIME(), -120)
-- RETURNS: 2019-04-22 11:39:21.6986813 -02:00  
```  
  
### <a name="c-adding-a-13-hour-time-zone-offset"></a>C. 添加 13 小时时区偏移量  
 下面的示例将 13 小时时区偏移量添加到日期和时间。  
  
```sql  
SELECT TODATETIMEOFFSET(SYSDATETIME(), '+13:00')
-- RETURNS: 2019-04-22 11:39:29.0339301 +13:00
```  
  
## <a name="see-also"></a>另请参阅  
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)   
 [AT TIME ZONE (Transact-SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)  
  
  

