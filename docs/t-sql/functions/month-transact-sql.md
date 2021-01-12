---
description: MONTH (Transact-SQL)
title: MONTH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- MONTH_TSQL
- MONTH
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], date and time
- dates [SQL Server], functions
- month of year [SQL Server]
- date and time [SQL Server], MONTH
- dateparts [SQL Server], month
- functions [SQL Server], date and time
- dates [SQL Server], MONTH
- MONTH function [SQL Server]
ms.assetid: 9dd8aff7-b0fc-45df-b316-ead14ee9b8b7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9e743089dcaac2ad48f978b7e0808150f9d36ef0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101039"
---
# <a name="month-transact-sql"></a>MONTH (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回表示指定日期的月份的整数。  
  
 有关所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 日期和时间数据类型及函数的概述，请参阅[日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sqlsyntax  
MONTH ( date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *date*  
 一个表达式，它可以解析为 time、date、smalldatetime、datetime、datetime2 或 datetimeoffset 值。 date 参数可以是表达式、列表达式、用户定义变量或字符串文字。  
  
## <a name="return-type"></a>返回类型  
 **int**  
  
## <a name="return-value"></a>返回值  
 MONTH 返回的值与 [DATEPART](../../t-sql/functions/datepart-transact-sql.md) (month, date) 所返回的值相同。  
  
 如果 date 只包含时间部分，则返回值为 1，即基准月。  
  
## <a name="examples"></a>示例  
 下面的语句将返回 `4`。 这是月份的数字。  
  
```sql  
SELECT MONTH('2007-04-30T01:01:01.1234567 -07:00');  
```  
  
 下面的语句将返回 `1900, 1, 1`。 date 的参数为数字 `0`。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将 `0` 解释为 1900 年 1 月 1 日。  
  
```sql  
SELECT YEAR(0), MONTH(0), DAY(0);  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 下面的示例将返回 `4`。 这是月份的数字。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT TOP 1 MONTH('2007-04-30T01:01:01.1234')   
FROM dbo.DimCustomer;  
```  
  
 下面的示例将返回 `1900, 1, 1`。 date 的参数为数字 `0`。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将 `0` 解释为 1900 年 1 月 1 日。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT TOP 1 YEAR(0), MONTH(0), DAY(0) FROM dbo.DimCustomer;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  

