---
description: DATEFROMPARTS (Transact-SQL)
title: DATEFROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEFROMPARTS_TSQL
- DATEFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATEFROMPARTS function
ms.assetid: 5b885376-87aa-41f1-9e18-04987aead250
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 345cdd42525fb4227989320f16d28a6ca79e1255
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735550"
---
# <a name="datefromparts-transact-sql"></a>DATEFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数返回映射到指定年、月、日值的 date 值。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DATEFROMPARTS ( year, month, day )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*year*  
指定年份的整数表达式。
  
*month*  
指定月份（从1 到12）的整数表达式。
  
*day*  
指定日期的整数表达式。
  
## <a name="return-types"></a>返回类型
**date**
  
## <a name="remarks"></a>备注  
`DATEFROMPARTS` 返回一个 date 值，其中日期部分设置为指定的年、月和日，时间部分设置为默认值。 对于无效参数，`DATEFROMPARTS` 将引发错误。 如果至少有一个必需参数具有 NULL 值，则 `DATEFROMPARTS` 返回 NULL。
  
此函数可在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本的服务器上执行远程处理。 它不能无法在版本低于 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 的服务器上执行远程处理。
  
## <a name="examples"></a>示例  
此示例演示了操作中的 `DATEFROMPARTS` 函数。
  
```sql
SELECT DATEFROMPARTS ( 2010, 12, 31 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
----------------------------------  
2010-12-31  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅
[date (Transact-SQL)](../../t-sql/data-types/date-transact-sql.md)
  
  

