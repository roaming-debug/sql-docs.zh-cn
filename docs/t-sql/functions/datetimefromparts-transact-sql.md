---
description: DATETIMEFROMPARTS (Transact-SQL)
title: DATETIMEFROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATETIMEFROMPARTS_TSQL
- DATETIMEFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIMEFROMPARTS function
ms.assetid: 6008148b-bf75-4c98-9392-68a89fa0711c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 836a63302f21ae1fa1214cc6611c9cded2e1ecb4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478688"
---
# <a name="datetimefromparts-transact-sql"></a>DATETIMEFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数对指定日期和时间参数返回 datetime 值。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DATETIMEFROMPARTS ( year, month, day, hour, minute, seconds, milliseconds )  
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
  
milliseconds  
指定毫秒数的整数表达式。
  
## <a name="return-types"></a>返回类型
**datetime**
  
## <a name="remarks"></a>备注  
`DATETIMEFROMPARTS` 返回完全初始化的 datetime 值。 如果至少有一个必需参数具有无效值，`DATETIMEFROMPARTS` 将引发错误。 如果至少有一个必需参数具有 NULL 值，则 `DATETIMEFROMPARTS` 返回 NULL。
  
此函数支持在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 及更高版本的服务器上远程执行。 但不支持在版本低于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的服务器上远程执行。
  
## <a name="examples"></a>示例  
  
```sql
SELECT DATETIMEFROMPARTS ( 2010, 12, 31, 23, 59, 59, 0 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
---------------------------  
2010-12-31 23:59:59.000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅
[datetime (Transact-SQL)](../../t-sql/data-types/datetime-transact-sql.md)
  
  

