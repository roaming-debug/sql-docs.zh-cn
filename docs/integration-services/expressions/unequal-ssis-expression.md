---
description: '!=（不等于）（SSIS 表达式）'
title: '!=（不等于）（SSIS 表达式）| Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- unequal operator (!=)
- '!= (not equal to)'
ms.assetid: fad20e85-c0e6-42bf-af70-2bc80ee09be5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7149d9e82243d1bd377fd4e7f75c2ce16edd2947
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123233"
---
# <a name="-unequal-ssis-expression"></a>!=（不等于）（SSIS 表达式）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  执行比较操作以确定具有兼容数据类型的两个表达式是否不相等。 在执行比较前表达式计算器会自动转换多种数据类型。  
  
 但是，某些数据类型要求表达式包括显式转换，才能成功进行计算。 有关数据类型之间的合法转换的详细信息，请参阅[转换（SSIS 表达式）](../../integration-services/expressions/cast-ssis-expression.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
expression1 != expression2  
  
```  
  
## <a name="arguments"></a>参数  
 *expression1、expression2*  
 为任何有效的表达式。  
  
## <a name="result-types"></a>结果类型  
 DT_BOOL  
  
## <a name="remarks"></a>注解  
 如果比较中的任一表达式为空，则比较结果为空。 如果两个表达式都为空，则结果为空。  
  
 表达式集， *expression1* 和 *expression2*，必须遵守下列规则之一：  
  
-   **Numeric***expression1* 和 *expression2* 必须为数值数据类型。 数据类型的交集必须为数值数据类型，此类型在表达式计算器隐式数值转换的有关规则中指定。 两个数值数据类型的交集不能为空。 有关详细信息，请参阅 [Integration Services Data Types in Expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md)。  
  
-   **Character** ： *expression1* 和 *expression2* 的计算结果必须为 DT_STR 或 DT_WSTR 数据类型。 两个表达式的计算结果可以为不同的字符串数据类型。  
  
    > [!NOTE]  
    >  字符串比较区分大小写、重音、假名和全半角。  
  
-   **Date、Time 或 Date/Time***expression1* 和 *expression2* 的计算结果必须为下列数据类型之一：DT_DBDATE、DT_DATE、DT_DBTIME、DT_DBTIME2、DT_DBTIMESTAMP、DT_DBTIMESTAMP2、DT_DBTIMESTAPMOFFSET 或 DT_FILETIME。  
  
    > [!NOTE]  
    >  系统不支持对计算结果为时间数据类型的表达式和计算结果为日期或日期/时间数据类型的表达式进行比较。 否则系统会生成错误。  
  
     在对表达式进行比较时，系统会按照所列出的顺序应用下面的转换规则：  
  
    -   当两个表达式的计算结果为同一个数据类型时，则会执行该数据类型的比较。  
  
    -   如果一个表达式是 DT_DBTIMESTAMPOFFSET 数据类型，则另一个表达式会隐式转换为 DT_DBTIMESTAMPOFFSET，并执行 DT_DBTIMESTAMPOFFSET 比较。 有关详细信息，请参阅 [Integration Services Data Types in Expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md)。  
  
    -   如果一个表达式是 DT_DBTIMESTAMP2 数据类型，则另一个表达式会隐式转换为 DT_DBTIMESTAMP2，并执行 DT_DBTIMESTAMP2 比较。  
  
    -   如果一个表达式是 DT_DBTIME2 数据类型，则另一个表达式会隐式转换为 DT_DBTIME2，并执行 DT_DBTIME2 比较。  
  
    -   如果一个表达式是 DT_DBTIMESTAMPOFFSET、DT_DBTIMESTAMP2 或 DT_DBTIME2 以外的类型，那么，在对表达式进行比较之前，会将它们转换为 DT_DBTIMESTAMP 数据类型。  
  
     在对表达式进行比较时，系统会进行如下假设：  
  
    -   如果每个表达式的数据类型都包含小数秒，则系统会假设对于小数秒位数最少的数据类型，用零填充其余位。  
  
    -   如果每个表达式都是日期数据类型，但是只有一个表达式具有时区偏移量，系统会假设没有时区偏移量的日期数据类型采用的是协调世界时 (UTC)。  
  
-   **Logical***expression1* 和 *expression2* 的计算结果必须为布尔值。  
  
-   **GUID** ： *expression1* 和 *expression2* 的计算结果必须为 DT_GUID 数据类型。  
  
-   **Binary** ： *expression1* 和 *expression2* 的计算结果必须为 DT_BYTES 数据类型。  
  
-   **BLOB** ： *expression1* 和 *expression2* 的计算结果必须为同一 BLOB（二进制大型对象块）数据类型：DT_TEXT、DT_NTEXT 或 DT_IMAGE。  
  
 有关数据类型的详细信息，请参阅 [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md)。  
  
## <a name="expression-examples"></a>表达式示例  
 仅在当前日期不是 2003 年 7 月 4 日时，此示例的计算结果才为 TRUE。 有关详细信息，请参阅 [GETDATE（SSIS 表达式）](../../integration-services/expressions/getdate-ssis-expression.md)。  
  
```  
"7/4/2003" != GETDATE()  
```  
  
 以下示例中，如果 **ListPrice** 列中的值不是 500，则此表达式的计算结果为 TRUE。  
  
```  
ListPrice != 500  
```  
  
 此示例使用了变量 **LPrice**。 如果 **LPrice** 的值不为 500，则此示例的计算结果为 TRUE。 该变量的数据类型必须为数值以便分析表达式。  
  
```  
@LPrice != 500  
```  
  
## <a name="see-also"></a>另请参阅  
 [==（等于）（SSIS 表达式）](../../integration-services/expressions/equal-ssis-expression.md)   
 [运算符优先级和结合性](../../integration-services/expressions/operator-precedence-and-associativity.md)   
 [运算符（SSIS 表达式）](../../integration-services/expressions/operators-ssis-expression.md)  
  
  
