---
description: + (Unary Plus) (Transact-SQL)
title: + (Unary Plus) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- + (positive)
- +
- positive
dev_langs:
- TSQL
helpviewer_keywords:
- + (positive operator)
- positive operator (+)
- positive values [SQL Server]
ms.assetid: 0f31c5cc-3078-4f6a-9870-7eb1a98053fb
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ec7815d623208c5e2860fd233ba5a290e2e8762b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460900"
---
# <a name="unary-operators---positive"></a>一元运算符 - 正
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

返回数值表达式（一个一元运算符）的值。 一元运算符只对一个表达式执行操作，该表达式可以是 numeric 数据类型类别中的任何一种数据类型。   
  
|运算符|含义|  
|--------------|-------------|  
|[+（正）](../../t-sql/language-elements/unary-operators-positive.md)|数值为正。|  
|[-（负）](../../t-sql/language-elements/unary-operators-negative.md)|数值为负。|  
|[~（位非）](../../t-sql/language-elements/bitwise-not-transact-sql.md)|返回数字的非。|  
  
 +（正）和 -（负）运算符可以用于 numeric 数据类型类别中任一数据类型的任意表达式。 ~ （位非）运算符只能用于整数数据类型类别中任一数据类型的表达式。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
+ numeric_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *numeric_expression*  
 具有数值数据类型类别中任一数据类型（datetime 和 smalldatetime 数据类型除外）的任何有效[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
## <a name="result-types"></a>结果类型  
 返回 *numeric_expression* 的数据类型。  
  
## <a name="remarks"></a>备注  
 尽管一元加号可以出现在任何数值表达式之前，但它对表达式返回的值不执行操作。 很明显，负表达式不会返回正值。 若要返回负表达式的正值，请使用 [ABS](../../t-sql/functions/abs-transact-sql.md) 函数。  
  
## <a name="examples"></a>示例  
  
### <a name="a-setting-a-variable-to-a-positive-value"></a>A. 将一个变量设置为正值  
 以下示例将变量设置为正值。  
  
```sql  
DECLARE @MyNumber DECIMAL(10,2);  
SET @MyNumber = +123.45;  
SELECT @MyNumber;  
GO  
```  
  
 下面是结果集：  
  
```  
-----------   
123.45            
  
(1 row(s) affected)  
```  
  
### <a name="b-using-the-unary-plus-operator-with-a-negative-value"></a>B. 将一元加号运算符用于负值  
 以下示例显示了如何将一元加号用于负表达式和将 ABS() 函数用于同一负表达式上。 一元加号不影响该表达式，但 ABS 函数将返回该表达式的正值。  
  
```sql  
USE tempdb;  
GO  
DECLARE @Num1 INT;  
SET @Num1 = -5;  
SELECT +@Num1, ABS(@Num1);  
GO  
```  
  
 下面是结果集：  
  
```  
----------- -----------  
-5          5  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [ABS (Transact-SQL)](../../t-sql/functions/abs-transact-sql.md)  
  
  
