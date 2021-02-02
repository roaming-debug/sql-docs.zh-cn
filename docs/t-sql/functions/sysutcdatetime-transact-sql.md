---
description: SYSUTCDATETIME (Transact-SQL)
title: SYSUTCDATETIME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SYSUTCDATETIME
- SYSUTCDATETIME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- system time [SQL Server]
- functions [SQL Server], date and time
- time [SQL Server], functions
- date and time [SQL Server], SYSUTCDATETIME
- SYSUTCDATETIME function [SQL Server]
- time [SQL Server], system
ms.assetid: f14fc2cd-9ea8-4daf-88f4-418cf523ab55
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fce96cdf1b28d72a7be47cbdb4dd8293431589a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99100842"
---
# <a name="sysutcdatetime-transact-sql"></a>SYSUTCDATETIME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回包含计算机的日期和时间的 datetime2 值，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例正在该计算机上运行。 日期和时间作为 UTC 时间（通用协调时间）返回。 秒部分精度规范的范围为 1 至 7 位。 默认精度为 7 位数。  
  
> [!NOTE]  
>  与 GETDATE 和 GETUTCDATE 比较而言，SYSDATETIME 和 SYSUTCDATETIME 的秒的小数部分精度更高。 SYSDATETIMEOFFSET 包含系统时区偏移量。 SYSDATETIME、SYSUTCDATETIME 和 SYSDATETIMEOFFSET 可以分配给采用任意日期和时间类型的变量。  
  
 有关所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间数据类型及函数的概述，请参阅[日期和时间数据类型及函数](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
SYSUTCDATETIME ( )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>返回类型  
 **datetime2**  
  
## <a name="remarks"></a>注解  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在其可以引用 datetime2 表达式的任何位置，均可引用 SYSUTCDATETIME。  
  
 SYSUTCDATETIME 是不确定性函数。 不能对在列中引用该函数的视图和表达式建立索引。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 GetSystemTimeAsFileTime() Windows API 来获取日期和时间值。 精确程度取决于运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机硬件和 Windows 版本。 此 API 的精度固定为 100 纳秒。 可通过使用 GetSystemTimeAdjustment() Windows API 来确定该精确度。  
  
## <a name="examples"></a>示例  
 以下示例使用六个返回当前日期和时间的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统函数来返回日期和/或时间。 这些值是连续返回的；因此，它们的秒小数部分可能有所不同。  
  
### <a name="a-showing-the-formats-that-are-returned-by-the-date-and-time-functions"></a>A. 显示由日期和时间函数返回的格式  
 下面的示例显示由日期和时间函数返回的其他格式。  
  
```sql
SELECT SYSDATETIME() AS [SYSDATETIME()]  
    ,SYSDATETIMEOFFSET() AS [SYSDATETIMEOFFSET()]  
    ,SYSUTCDATETIME() AS [SYSUTCDATETIME()]  
    ,CURRENT_TIMESTAMP AS [CURRENT_TIMESTAMP]  
    ,GETDATE() AS [GETDATE()]  
    ,GETUTCDATE() AS [GETUTCDATE()];  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
SYSDATETIME()      2007-04-30 13:10:02.0474381
SYSDATETIMEOFFSET()2007-04-30 13:10:02.0474381 -07:00
SYSUTCDATETIME()   2007-04-30 20:10:02.0474381
CURRENT_TIMESTAMP  2007-04-30 13:10:02.047
GETDATE()          2007-04-30 13:10:02.047
GETUTCDATE()       2007-04-30 20:10:02.047
```  
  
### <a name="b-converting-date-and-time-to-date"></a>B. 将日期和时间转换为日期  
 下面的示例说明如何将日期和时间值转换为 `date`。  
  
```sql
SELECT CONVERT (date, SYSDATETIME())  
    ,CONVERT (date, SYSDATETIMEOFFSET())  
    ,CONVERT (date, SYSUTCDATETIME())  
    ,CONVERT (date, CURRENT_TIMESTAMP)  
    ,CONVERT (date, GETDATE())  
    ,CONVERT (date, GETUTCDATE());  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
2007-04-30
2007-04-30
2007-04-30
2007-04-30
2007-04-30
2007-04-30
```  
  
### <a name="c-converting-date-and-time-values-to-time"></a>C. 将日期和时间值转换为时间  
 下面的示例说明如何将日期和时间值转换为 `time`。  
  
 ```sql
DECLARE @DATETIME DATETIME = GetDate();
DECLARE @TIME TIME
SELECT @TIME = CONVERT(time, @DATETIME)
SELECT @TIME AS 'Time', @DATETIME AS 'Date Time'
```
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Time             Date Time  
13:49:33.6330000 2009-04-22 13:49:33.633
```  
  
## <a name="see-also"></a>另请参阅  
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)   
 [AT TIME ZONE (Transact-SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)  
  
  


