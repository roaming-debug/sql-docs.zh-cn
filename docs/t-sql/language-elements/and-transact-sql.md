---
description: AND (Transact-SQL)
title: AND (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AND_TSQL
- AND
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], TRUE
- "TRUE"
- AND, about AND operators
- AND
- combining expressions
ms.assetid: b61d7f8d-5a51-49b7-91dd-f6190a5a0fb9
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 04d1945753618e5552750af7959e62519113015f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754037"
---
# <a name="and-transact-sql"></a>AND (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  合并两个布尔表达式；在两个表达式均为 TRUE 时返回 TRUE。 当语句中使用多个逻辑运算符时，将首先计算 AND 运算符。 可以通过使用括号改变求值顺序。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
boolean_expression AND boolean_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *boolean_expression*  
 返回以下布尔值的任何有效[表达式](../../t-sql/language-elements/expressions-transact-sql.md)：TRUE、FALSE 或 UNKNOWN。  
  
## <a name="result-types"></a>结果类型  
 **布尔值**  
  
## <a name="result-value"></a>结果值  
 当两个表达式均为 TRUE 时返回 TRUE。  
  
## <a name="remarks"></a>注解  
 下表显示了使用 AND 运算符比较 TRUE 值和 FALSE 值时的结果。  
  
||true|false|UNKNOWN|  
|------|----------|-----------|-------------|  
|**TRUE**|true|false|UNKNOWN|  
|**FALSE**|FALSE|false|false|  
|**未知**|UNKNOWN|FALSE|UNKNOWN|  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-the-and-operator"></a>A. 使用 AND 运算符  
 下面的示例选择与职位为 `Marketing Assistant` 且可用假期小时数超过 `41` 的员工有关的信息。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT  BusinessEntityID, LoginID, JobTitle, VacationHours   
FROM HumanResources.Employee  
WHERE JobTitle = 'Marketing Assistant'  
AND VacationHours > 41 ;  
```  
  
### <a name="b-using-the-and-operator-in-an-if-statement"></a>B. 在 IF 语句中使用 AND 运算符  
 下面的示例显示如何在 IF 语句中使用 AND。 在第一个语句中，`1 = 1` 和 `2 = 2` 均为 True，因此结果是 True。 在第二个示例中，参数 `2 = 17` 为 False，因此结果为 False。  
  
```sql  
IF 1 = 1 AND 2 = 2  
BEGIN  
   PRINT 'First Example is TRUE'  
END  
ELSE PRINT 'First Example is FALSE' ;  
GO  
  
IF 1 = 1 AND 2 = 17  
BEGIN  
   PRINT 'Second Example is TRUE'  
END  
ELSE PRINT 'Second Example is FALSE' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
  
  
