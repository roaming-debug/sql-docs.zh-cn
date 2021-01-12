---
description: CURRENT_TIMESTAMP (Transact-SQL)
title: CURRENT_TIMESTAMP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CURRENT_TIMESTAMP
- CURRENT_TIMESTAMP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- niladic functions
- current date and time [SQL Server]
- time [SQL Server], current
- date and time [SQL Server], CURRENT_TIMESTAMP
- functions [SQL Server], time
- system date and time [SQL Server]
- system time [SQL Server]
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], current date and time
- dates [SQL Server], system date and time
- CURRENT_TIMESTAMP function [SQL Server]
- time [SQL Server], system
ms.assetid: c724d9cc-7b1f-4c71-bdf5-08bc52b33afc
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8725fa782ef5918ac952e927097721d2cbf14afa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093708"
---
# <a name="current_timestamp-transact-sql"></a>CURRENT_TIMESTAMP (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数返回当前数据库系统时间戳，返回值的类型为 datetime，并且不含数据库时区偏移量。 `CURRENT_TIMESTAMP` 从运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机的操作系统中获得此值。
  
> [!NOTE]  
>  `SYSDATETIME` 和 `SYSUTCDATE` 采用秒的小数部分精度度量，因此其精度比 `GETDATE` 和 `GETUTCDATE` 的精度更高。 `SYSDATETIMEOFFSET` 函数包含系统时区偏移量。 可向任意日期和时间类型的变量分配 `SYSDATETIME`、`SYSUTCDATE` 和 `SYSDATETIMEOFFSET`。  
  
此函数是 ANSI SQL，等价于 [GETDATE](../../t-sql/functions/getdate-transact-sql.md)。
  
有关所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间数据类型及函数的概述，请参阅[日期和时间数据类型及函数](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CURRENT_TIMESTAMP  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
此函数没有参数。
  
## <a name="return-type"></a>返回类型  
**datetime**
  
## <a name="remarks"></a>备注  
[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句可以在能引用 datetime 表达式的所有位置引用 `CURRENT_TIMESTAMP`。
  
`CURRENT_TIMESTAMP` 是非确定性的函数。 引用该列的视图和表达式无法进行索引。
  
## <a name="examples"></a>示例  
以下示例使用六个返回当前日期和时间值的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统函数来返回日期和/或时间。 这些示例连续返回值，因此，它们的秒小数部分可能有所不同。 注意：返回的实际值可反映出执行的时间/时间。
  
### <a name="a-get-the-current-system-date-and-time"></a>A. 获取当前系统日期和时间  
  
```sql
SELECT SYSDATETIME()  
    ,SYSDATETIMEOFFSET()  
    ,SYSUTCDATETIME()  
    ,CURRENT_TIMESTAMP  
    ,GETDATE()  
    ,GETUTCDATE();  
/* Returned:  
SYSDATETIME()      2007-04-30 13:10:02.0474381  
SYSDATETIMEOFFSET()2007-04-30 13:10:02.0474381 -07:00  
SYSUTCDATETIME()   2007-04-30 20:10:02.0474381  
CURRENT_TIMESTAMP  2007-04-30 13:10:02.047  
GETDATE()          2007-04-30 13:10:02.047  
GETUTCDATE()       2007-04-30 20:10:02.047  
*/
```  
  
### <a name="b-get-the-current-system-date"></a>B. 获取当前系统日期  
  
```sql
SELECT CONVERT (DATE, SYSDATETIME())  
    ,CONVERT (DATE, SYSDATETIMEOFFSET())  
    ,CONVERT (DATE, SYSUTCDATETIME())  
    ,CONVERT (DATE, CURRENT_TIMESTAMP)  
    ,CONVERT (DATE, GETDATE())  
    ,CONVERT (DATE, GETUTCDATE());  
  
/* Returned   
SYSDATETIME()      2007-05-03  
SYSDATETIMEOFFSET()2007-05-03  
SYSUTCDATETIME()   2007-05-04  
CURRENT_TIMESTAMP  2007-05-03  
GETDATE()          2007-05-03  
GETUTCDATE()       2007-05-04  
*/  
```  
  
### <a name="c-get-the-current-system-time"></a>C. 获取当前系统时间  
  
```sql
SELECT CONVERT (TIME, SYSDATETIME())  
    ,CONVERT (TIME, SYSDATETIMEOFFSET())  
    ,CONVERT (TIME, SYSUTCDATETIME())  
    ,CONVERT (TIME, CURRENT_TIMESTAMP)  
    ,CONVERT (TIME, GETDATE())  
    ,CONVERT (TIME, GETUTCDATE());  
  
/* Returned  
SYSDATETIME()      13:18:45.3490361  
SYSDATETIMEOFFSET()13:18:45.3490361  
SYSUTCDATETIME()   20:18:45.3490361  
CURRENT_TIMESTAMP  13:18:45.3470000  
GETDATE()          13:18:45.3470000  
GETUTCDATE()       20:18:45.3470000  
*/  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
```sql
SELECT CURRENT_TIMESTAMP;  
```  
  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

