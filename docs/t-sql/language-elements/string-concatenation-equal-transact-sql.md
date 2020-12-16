---
title: += 字符串串联
description: 将两个字符串串联起来并将一个字符串设置为运算结果。
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 12/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- concatenate strings
- string concatenation
- += (concatenate operator)
ms.assetid: 4aaeaab7-9b2b-48e0-8487-04ed672ebcb1
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 182896b066bd31773231feafa557d85ffe00d47c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481858"
---
# <a name="-string-concatenation-assignment-transact-sql"></a>+=（字符串串联赋值）(Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  将两个字符串串联起来并将一个字符串设置为运算结果。 例如，如果变量 @x 等于 'Adventure'，则 @x += 'Works' 会接受 @x 的原始值，将 'Works' 添加到该字符串中并将 @x 设置为该新值 'AdventureWorks'。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
expression += expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *expression*  
 为任意字符数据类型的任意有效的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
## <a name="result-types"></a>结果类型  
 返回为变量定义的数据类型。  
  
## <a name="remarks"></a>备注  
 SET @v1 += 'expression' 等同于 SET @v1 = @v1 + ('expression')。 同样，SET @v1 = @v2 + @v3 + @v4 等同于 SET @v1 = (@v2 + @v3) + @v4。  
  
 如果没有变量，则不能使用 += 运算符。 例如，下面的代码将导致错误：  
  
```sql  
SELECT 'Adventure' += 'Works'  
```  
  
## <a name="examples"></a>示例  
### <a name="a-concatenation-using--operator"></a>A. 使用 += 运算符进行串联
 下面的示例使用 `+=` 运算符进行串联。  
  
```sql  
DECLARE @v1 VARCHAR(40);  
SET @v1 = 'This is the original.';  
SET @v1 += ' More text.';  
PRINT @v1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `This is the original. More text.`  
  
### <a name="b-order-of-evaluation-while-concatenating-using--operator"></a>B. 使用 += 运算符进行串联时的计算顺序
以下示例将多个字符串串联成一个长字符串，然后尝试计算最终字符串的长度。 此示例演示使用串联运算符时的计算顺序和截断规则。 

```sql
DECLARE @x VARCHAR(4000) = REPLICATE('x', 4000)
DECLARE @z VARCHAR(8000) = REPLICATE('z',8000)
DECLARE @y VARCHAR(max);
 
SET @y = '';
SET @y += @x + @z;
SELECT LEN(@y) AS Y; -- 8000
 
SET @y = '';
SET @y = @y + @x + @z;
SELECT LEN(@y) AS Y; -- 12000
 
SET @y = '';
SET @y = @y +(@x + @z);
SELECT LEN(@y) AS Y; -- 8000
-- or
SET @y = '';
SET @y = @x + @z + @y;
SELECT LEN(@y) AS Y; -- 8000
GO
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Y       
 ------- 
 8000 
  
 (1 row(s) affected) 
  
    
 Y       
 ------- 
 12000 
  
 (1 row(s) affected) 

 Y       
 ------- 
 8000 
  
 (1 row(s) affected) 
  
 Y       
 ------- 
 8000 
  
 (1 row(s) affected)
  ```   
   
## <a name="see-also"></a>另请参阅  
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [+=（加法赋值）(Transact-SQL)](../../t-sql/language-elements/add-equals-transact-sql.md)   
 [+（字符串串联）(Transact-SQL)](../../t-sql/language-elements/string-concatenation-transact-sql.md)  
  
  
