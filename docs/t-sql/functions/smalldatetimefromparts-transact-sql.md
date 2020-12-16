---
description: SMALLDATETIMEFROMPARTS (Transact-SQL)
title: SMALLDATETIMEFROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SMALLDATETIMEFROMPARTS
- SMALLDATETIMEFROMPARTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SMALLDATETIMEFROMPARTS function
ms.assetid: 7467fdab-e588-419c-9e29-42caec34a9ea
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fdb92fe477a1a8c59da96f454f1944b9102381f8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484149"
---
# <a name="smalldatetimefromparts-transact-sql"></a>SMALLDATETIMEFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为指定的日期和时间返回 smalldatetime 值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
SMALLDATETIMEFROMPARTS ( year, month, day, hour, minute )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *year*  
 用于指定年度的整数表达式。  
  
 *month*  
 用于指定月份的整数表达式。  
  
 *day*  
 用于指定日期的整数表达式。  
  
 hour  
 用于指定小时的整数表达式。  
  
 minute  
 用于指定分钟的整数表达式。  
  
## <a name="return-types"></a>返回类型  
 **smalldatetime**  
  
## <a name="remarks"></a>备注  
 此函数的行为像完全初始化的 smalldatetime 值的构造函数。 如果参数无效，则引发错误。 如果所需的参数为 null，则返回 null。  
  
 此函数可以在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 服务器以及更高版本上远程执行。 但在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 之下的服务器版本中无法远程执行。  
  
## <a name="examples"></a>示例  
  
```sql  
SELECT SMALLDATETIMEFROMPARTS ( 2010, 12, 31, 23, 59 ) AS Result  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------------------  
2010-12-31 23:59:00  
  
(1 row(s) affected)  
```  
  

