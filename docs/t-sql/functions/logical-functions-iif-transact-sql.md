---
description: 逻辑函数 - IIF (Transact-SQL)
title: IIF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IIF_TSQL
- IIF
dev_langs:
- TSQL
helpviewer_keywords:
- IIF function
ms.assetid: e3ccf8ed-1cec-43ac-90b7-d8597c24b050
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8dd7e4faa4c8e175987254981f09d78da9445d1a
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151273"
---
# <a name="logical-functions---iif-transact-sql"></a>逻辑函数 - IIF (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，根据布尔表达式计算为 true 还是 false，返回其中一个值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
IIF( boolean_expression, true_value, false_value )
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *boolean_expression*  
 一个有效的布尔表达式。  
  
 如果此参数不是布尔表达式，则引发一个语法错误。  
  
 true_value  
 boolean_expression 计算结果为 true 时要返回的值。  
  
 false_value  
 boolean_expression 计算结果为 false 时要返回的值。  
  
## <a name="return-types"></a>返回类型  
 从 true_value 和 false_value 的类型中返回优先级最高的数据类型。 有关详细信息，请参阅[数据类型优先级 (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md)。  
  
## <a name="remarks"></a>注解  
 IIF 是一种用于编写 CASE 表达式的快速方法。 它将传递的布尔表达式计算为第一个参数，然后根据计算结果返回其他两个参数之一。 也即，如果布尔表达式为 true，则返回 true_value；如果布尔表达式为 false 或未知，则返回 false_value。 true_value 和 false_value 可以是任何类型。 适用于布尔表达式、null 处理和返回类型的 CASE 表达式的相同规则也适用于 IIF。 有关详细信息，请参阅 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)。  
  
 IIF 转换为 CASE 这一事实也影响此函数的行为的其他方面。 因为 CASE 表达式最多可嵌套 10 层，所以 IIF 语句最多也只能嵌套 10 层。 此外，IIF 将作为语义上等同的 CASE 表达式对其他服务器远程执行，且具备远程执行的 CASE 表达式的所有行为。  
  
## <a name="examples"></a>示例  
  
### <a name="a-simple-iif-example"></a>A. 简单 IIF 示例  
  
```sql  
DECLARE @a INT = 45, @b INT = 40;
SELECT [Result] = IIF( @a > @b, 'TRUE', 'FALSE' );
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------  
TRUE  
```  
  
### <a name="b-iif-with-null-constants"></a>B. 带有 NULL 常量的 IIF  
  
```sql 
SELECT [Result] = IIF( 45 > 30, NULL, NULL );
```  
  
 此语句的结果是一个错误。  
  
### <a name="c-iif-with-null-parameters"></a>C. 具有 NULL 参数的 IIF  
  
```sql  
DECLARE @P INT = NULL, @S INT = NULL;  
SELECT [Result] = IIF( 45 > 30, @P, @S );
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------  
NULL  
```  
  
## <a name="see-also"></a>另请参阅  
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [CHOOSE (Transact-SQL)](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  
