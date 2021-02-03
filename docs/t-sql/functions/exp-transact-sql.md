---
description: EXP (Transact-SQL)
title: EXP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EXP_TSQL
- EXP
dev_langs:
- TSQL
helpviewer_keywords:
- exponential functions
- EXP function
ms.assetid: 5a9b8c52-6fb6-4e33-8b02-a878785b2f51
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bd96424517e461d55b79cb89ae2b2613bfae6429
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181277"
---
# <a name="exp-transact-sql"></a>EXP (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回指定的 float 表达式的指数值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
EXP ( float_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *float_expression*  
 float 类型或能隐式转换为 float 类型的[表达式](../../t-sql/language-elements/expressions-transact-sql.md) 。  
  
## <a name="return-types"></a>返回类型  
 **float**  
  
## <a name="remarks"></a>注解  
 常量 e (2.718281…) 是自然对数的底数。  
  
 一个数字的指数是常量 **e** 的该数字次幂。 例如，EXP(1.0) = e^1.0 = 2.71828182845905，而 EXP(10) = e^10 = 22026.4657948067。  
  
 数字的自然对数的指数是数字本身：EXP (LOG (n)) = n。 数字的指数的自然对数是数字本身：LOG (EXP (n)) = n。  
  
## <a name="examples"></a>示例  
  
### <a name="a-finding-the-exponent-of-a-number"></a>A. 查找数字的指数  
 以下示例声明一个变量，并返回指定变量 (`10`) 的指数值，并附有文字说明。  
  
```sql  
DECLARE @var FLOAT  
SET @var = 10  
SELECT 'The EXP of the variable is: ' + CONVERT(VARCHAR, EXP(@var))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------------------------------------------------------  
The EXP of the variable is: 22026.5  
(1 row(s) affected)  
```  
  
### <a name="b-finding-exponentials-and-natural-logarithms"></a>B. 查找指数和自然对数  
 以下示例返回 `20` 的自然对数的指数值，以及 `20` 的指数的自然对数。 由于这两个函数彼此互为反函数，所以两种情况下的返回值都是 `20`。  
  
```sql  
SELECT EXP(LOG(20)), LOG(EXP(20))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------------------- ----------------------  
20                     20  
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-finding-the-exponent-of-a-number"></a>C. 查找数字的指数  
 以下示例返回指定值 (`10`) 的指数值。  
  
```sql  
SELECT EXP(10);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------  
22026.4657948067  
```  
  
### <a name="d-finding-exponential-values-and-natural-logarithms"></a>D. 查找指数值和自然对数  
 以下示例返回 `20` 的自然对数的指数值，以及 `20` 的指数的自然对数。 由于这两个函数彼此互为反函数，所以两种情况下的返回值都是 `20`。  
  
```sql  
SELECT EXP( LOG(20)), LOG( EXP(20));  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------- -----------------  
20                  20  
```  
  
## <a name="see-also"></a>另请参阅  
 [数学函数 (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)   
 [LOG (Transact-SQL)](../../t-sql/functions/log-transact-sql.md)   
 [LOG10 (Transact-SQL)](../../t-sql/functions/log10-transact-sql.md)  
  
  

