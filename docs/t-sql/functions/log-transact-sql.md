---
description: LOG (Transact-SQL)
title: LOG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- LOG
- LOG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- float expressions
- logarithm of expression
- LOG function
ms.assetid: f7c39511-cd84-4362-93ba-0d93655217ee
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4c962baabeaccdd2c2cbf517cab5894458046bb2
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88468000"
---
# <a name="log-transact-sql"></a>LOG (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中指定 float 表达式的自然对数。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database  
  
LOG ( float_expression [, base ] )  
```  
  
```syntaxsql
-- Syntax for Azure Synapse SQL 
  
LOG ( float_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *float_expression*  
 float 类型或能隐式转换为 float 类型的[表达式](../../t-sql/language-elements/expressions-transact-sql.md) 。  
  
 *base*  
 可选的整型参数，设置对数的底数。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本
  
## <a name="return-types"></a>返回类型  
 **float**  
  
## <a name="remarks"></a>注解  
 默认情况下，LOG() 返回自然对数。 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，可使用可选的 base 参数将对数的底数更改为其他值 。  
  
 自然对数是以 **e** 为底的对数，其中，**e** 是一个无理常量，约等于 2.718281828。  
  
 数字的指数的自然对数是数字本身：LOG (EXP (n)) = n 。 以及数字的自然对数的指数是数字本身：EXP (LOG (n)) = n 。  
  
## <a name="examples"></a>示例  
  
### <a name="a-calculating-the-logarithm-for-a-number"></a>A. 计算某数的对数。  
 以下示例计算指定 float 表达式的 `LOG`。  
  
```sql  
DECLARE @var FLOAT = 10;  
SELECT 'The LOG of the variable is: ' + CONVERT(VARCHAR, LOG(@var));  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------------------------------  
The LOG of the variable is: 2.30259  
  
(1 row(s) affected)  
```  
  
### <a name="b-calculating-the-logarithm-of-the-exponent-of-a-number"></a>B. 计算某数指数的对数。  
 以下示例计算某个数字的指数的 `LOG`。  
  
```sql  
SELECT LOG (EXP (10));  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------------------------------  
10  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-calculating-the-logarithm-for-a-number"></a>C. 计算某数的对数  
 以下示例计算指定 float 表达式的 `LOG`。  
  
```sql  
SELECT LOG(10);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ----------------`  
  
 2.30
 ```  
  
## <a name="see-also"></a>另请参阅  
 [数学函数 (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)   
 [EXP (Transact-SQL)](../../t-sql/functions/exp-transact-sql.md)   
 [LOG10 (Transact-SQL)](../../t-sql/functions/log10-transact-sql.md)  
  
  

