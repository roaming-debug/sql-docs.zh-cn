---
description: SWITCHOFFSET (Transact-SQL)
title: SWITCHOFFSET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/02/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SWITCHTZ
- SWITCHTZ_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- functions [SQL Server], time
- functions [SQL Server], date and time
- SWITCHOFFSET function [SQL Server]
- time [SQL Server], functions
- date and time [SQL Server], SWITCHOFFSET
- time zones [SQL Server]
ms.assetid: 32a48e36-0aa4-4260-9fe9-cae9197d16c5
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dad555b6cd2e740d6d44f0dd4a3966f0e56e6b1b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461158"
---
# <a name="switchoffset-transact-sql"></a>SWITCHOFFSET (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回从存储的时区偏移量变为指定的新时区偏移量时得到的 datetimeoffset 值。  
  
 有关所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间数据类型及函数的概述，请参阅[日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
SWITCHOFFSET ( DATETIMEOFFSET, time_zone )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 DATETIMEOFFSET  
 是一个可以解析为 datetimeoffset(n) 值的表达式。  
  
 time_zone  
 是一个格式为 [+|-]TZH:TZM 的字符串，或是一个表示时区偏移量的带符号的整数（分钟数），假定它能够感知夏时制并作出相应的调整。  
  
## <a name="return-type"></a>返回类型  
 具有 DATETIMEOFFSET 参数小数精度的 datetimeoffset。  
  
## <a name="remarks"></a>注解  
 使用 SWITCHOFFSET 可选择与最初存储的时区偏移量不同的时区偏移量的 datetimeoffset 值。 SWITCHOFFSET 不会更新存储的 time_zone 值。  
  
 SWITCHOFFSET 可用于更新 datetimeoffset 列。  
  
 将 SWITCHOFFSET 用于函数 GETDATE() 可能导致查询运行缓慢。 这是因为查询优化器无法获取 datetime 值的准确基数估计值。 要解决此问题，请使用 OPTION (RECOMPILE) 查询提示以强制查询优化器在下次执行同一查询时重新编译查询计划。 优化器将得到准确的基数估计值并生成更高效的查询计划。 有关 RECOMPILE 查询提示的详细信息，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。  
  
```sql
DECLARE @dt datetimeoffset = switchoffset (CONVERT(datetimeoffset, GETDATE()), '-04:00');   
SELECT * FROM t    
WHERE c1 > @dt OPTION (RECOMPILE);  
```  
  
## <a name="examples"></a>示例  
 下例使用 `SWITCHOFFSET` 显示与数据库中所存储的值不同的时区偏移量。  
  
```sql  
CREATE TABLE dbo.test   
    (  
    ColDatetimeoffset datetimeoffset  
    );  
GO  
INSERT INTO dbo.test   
VALUES ('1998-09-20 7:45:50.71345 -5:00');  
GO  
SELECT SWITCHOFFSET (ColDatetimeoffset, '-08:00')   
FROM dbo.test;  
GO  
--Returns: 1998-09-20 04:45:50.7134500 -08:00  
SELECT ColDatetimeoffset  
FROM dbo.test;  
--Returns: 1998-09-20 07:45:50.7134500 -05:00  
```  
  
## <a name="see-also"></a>另请参阅  
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [AT TIME ZONE (Transact-SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)  
  
  


