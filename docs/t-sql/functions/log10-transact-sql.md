---
description: LOG10 (Transact-SQL)
title: LOG10 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- LOG10
- LOG10_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- float expressions
- LOG10 function
- base-10 logarithms
- logarithm of expression
ms.assetid: 1eb7fb34-1937-4a39-a936-f5c0c7c7e06f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5f78bf83bb364737b2a8f91db97a2140a60fb823
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468178"
---
# <a name="log10-transact-sql"></a>LOG10 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回指定 float 表达式的以 10 为底的对数。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
LOG10 ( float_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *float_expression*  
 float 类型或能隐式转换为 float 类型的[表达式](../../t-sql/language-elements/expressions-transact-sql.md) 。  
  
## <a name="return-types"></a>返回类型  
 **float**  
  
## <a name="remarks"></a>备注  
 LOG10 和 POWER 函数互为反函数。 例如，10 ^ LOG10(n) = n 。  
  
## <a name="examples"></a>示例  
  
### <a name="a-calculating-the-base-10-logarithm-for-a-variable"></a>A. 计算变量的以 10 为底的对数。  
 以下示例计算指定变量的 `LOG10`。  
  
```sql  
DECLARE @var FLOAT;  
SET @var = 145.175643;  
SELECT 'The LOG10 of the variable is: ' + CONVERT(VARCHAR,LOG10(@var));  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
The LOG10 of the variable is: 2.16189      
  
(1 row(s) affected)  
```  
  
### <a name="b-calculating-the-result-of-raising-a-base-10-logarithm-to-a-specified-power"></a>B. 对以 10 为底的对数执行指定幂计算的结果。  
 以下示例返回对以 10 为底的对数执行指定幂计算的结果。  
  
```sql  
SELECT POWER (10, LOG10(5));   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-----------  
5  
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-calculating-the-base-10-logarithm-for-a-value"></a>C：计算值的以 10 为底的对数。  
 以下示例计算指定值的 `LOG10`。  
  
```sql  
SELECT LOG10(145.175642);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
-------------------  
2.16
```  
  
## <a name="see-also"></a>另请参阅  
 [数学函数 (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)   
 [POWER (Transact-SQL)](../../t-sql/functions/power-transact-sql.md)   
 [LOG (Transact-SQL)](../../t-sql/functions/log-transact-sql.md)  
  
  

