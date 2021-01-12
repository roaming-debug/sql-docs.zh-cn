---
description: DAY (Transact-SQL)
title: DAY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DAY_TSQL
- DAY
dev_langs:
- TSQL
helpviewer_keywords:
- date and time [SQL Server], DAY
- dates [SQL Server], functions
- DAY function [SQL Server]
- dates [SQL Server], days
- functions [SQL Server], date and time
- dateparts [SQL Server], day
ms.assetid: 2f4410ea-fd3e-4d69-ac4b-3b0091a084bc
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 841c240ae454ce11a54b821871971d97504b2ba8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101185"
---
# <a name="day-transact-sql"></a>DAY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数返回表示指定 date 的日期（某月的一天）的整数。
  
有关所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间数据类型及函数的概述，请参阅[日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DAY ( date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*date*  
解析为下列某种数据类型的表达式：

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

对于 date，`DAY` 接受列表达式、表达式、字符串文本或用户定义的变量  。
  
## <a name="return-type"></a>返回类型  
**int**
  
## <a name="return-value"></a>返回值  
DAY 与 [DATEPART](../../t-sql/functions/datepart-transact-sql.md) (day, date) 返回相同的值。
  
如果 date 只包含时间部分，则 `DAY` 将返回 1，即基准日。
  
## <a name="examples"></a>示例  
此语句返回 `30`，即天数本身。
  
```sql
SELECT DAY('2015-04-30 01:01:01.1234567');  
```  
  
此语句返回 `1900, 1, 1`。 date 参数具有数值 `0`。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将 `0` 解释为 1900 年 1 月 1 日。
  
```sql
SELECT YEAR(0), MONTH(0), DAY(0);  
```  
  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  


