---
description: '- （负值）(Transact-SQL)'
title: '- （负值）(Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- negative
dev_langs:
- TSQL
helpviewer_keywords:
- '- (negative)'
- negative operator (-)
- negative values
ms.assetid: d6c14d14-d379-403b-82db-c197ad58c896
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 77604125d1f69c09283148b59f2dfbdda7f62546
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462318"
---
# <a name="unary-operators---negative"></a>一元运算符 - 负值
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回数值表达式的值的负值（一元运算符）。 一元运算符只对一个表达式执行操作，该表达式可以是 numeric 数据类型类别中的任何一种数据类型。   
  
|运算符|含义|  
|--------------|-------------|  
|[+（正）](../../t-sql/language-elements/unary-operators-positive.md)|数值为正。|  
|[-（负）](../../t-sql/language-elements/unary-operators-negative.md)|数值为负。|  
|[~（位非）](../../t-sql/language-elements/bitwise-not-transact-sql.md)|返回数字的非。|  
  
 +（正）和 -（负）运算符可以用于 numeric 数据类型类别中任一数据类型的任意表达式。 ~ （位非）运算符只能用于整数数据类型类别中任一数据类型的表达式。 
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
- numeric_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *numeric_expression*  
 数值数据类型类别中任何数据类型（日期和时间类别除外）的任何有效[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
## <a name="result-types"></a>结果类型  
 除了无符号 tinyint 表达式被提升为有符号的 smallint 结果之外，返回 numeric_expression 的数据类型。  
  
## <a name="examples"></a>示例  
  
### <a name="a-setting-a-variable-to-a-negative-value"></a>A. 将一个变量设置为负值  
 以下示例将一个变量设置为负值。  
  
```sql 
USE tempdb;  
GO  
DECLARE @MyNumber DECIMAL(10,2);  
SET @MyNumber = -123.45;  
SELECT @MyNumber AS NegativeValue;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
NegativeValue  
---------------------------------------  
-123.45  
  
(1 row(s) affected)  
  
```  
  
### <a name="b-changing-a-variable-to-a-negative-value"></a>B. 将一个变量更改为负值  
 以下示例将一个变量更改为负值。  
  
```sql  
USE tempdb;  
GO  
DECLARE @Num1 INT;  
SET @Num1 = 5;  
SELECT @Num1 AS VariableValue, -@Num1 AS NegativeValue;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
VariableValue NegativeValue  
------------- -------------  
5             -5  
  
(1 row(s) affected)  
  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-returning-the-negative-of-a-positive-constant"></a>C. 返回正常量的负值  
 下面的示例返回正常量的负值。  
  
```sql  
USE ssawPDW;  
  
SELECT TOP (1) - 17 FROM DimEmployee;  
```  
  
 返回  
  
```  
-17  
```  
  
### <a name="d-returning-the-positive-of-a-negative-constant"></a>D. 返回负常量的正值  
 下面的示例返回负常量的正值。  
  
```sql  
USE ssawPDW;  
  
SELECT TOP (1) - ( - 17) FROM DimEmployee;  
```  
  
 返回  
  
```  
17  
```  
  
### <a name="e-returning-the-negative-of-a-column"></a>E. 返回列的负值  
 下面的示例返回 `dimEmployee` 表中每位员工的 `BaseRate` 值的负值。  
  
```sql  
USE ssawPDW;  
  
SELECT - BaseRate FROM DimEmployee;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)  
  
  

