---
description: '? 解码的字符：（条件）（SSIS 表达式）'
title: '? :（条件）（SSIS 表达式）| Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- conditional operator (?:)
- '?: (conditional operator)'
ms.assetid: d38e6890-7338-4ce0-a837-2dbb41823a37
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f63ea95489b639672ddb50b44fa882eecc463025
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123281"
---
# <a name="--conditional-ssis-expression"></a>? 解码的字符：（条件）（SSIS 表达式）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  根据布尔表达式的计算结果，返回两个表达式之一。 如果布尔表达式的计算结果为 TRUE，则计算第一个表达式，结果为该表达式的结果。 如果布尔表达式的计算结果为 FALSE，则计算第二个表达式，结果为该表达式的结果。  
  
## <a name="syntax"></a>语法  
  
```  
  
boolean_expression?expression1:expression2  
  
```  
  
## <a name="arguments"></a>参数  
 *boolean_expression*  
 计算结果为 TRUE、FALSE 或 NULL 的任意有效表达式。  
  
 *expression1*  
 为任何有效的表达式。  
  
 *expression2*  
 为任何有效的表达式。  
  
## <a name="result-types"></a>结果类型  
 *expression1* 或 *expression2* 的数据类型。  
  
## <a name="remarks"></a>备注  
 如果 *boolean_expression* 的计算结果为 NULL，则表达式结果为 NULL。 如果选择的表达式（ *expression1* 或 *expression2* ）为 NULL，则结果为 NULL。 如果选择的表达式不为 NULL，但未选择的表达式为 NULL，则结果为所选表达式的值。  
  
 如果 *expression1* 和 *expression2* 的数据类型相同，则结果便为该数据类型。 对于结果类型适用于下列附加规则：  
  
-   DT_TEXT 数据类型要求 *expression1* 和 *expression2* 具有相同的代码页。  
  
-   DT_BYTES 数据类型的结果长度是较长参数的长度。  
  
 表达式集（ *expression1* 和 *expression2*）的计算结果必须为有效的数据类型而且必须遵循下列规则之一：  
  
-   **Numeric***expression1* 和 *expression2* 必须为数值数据类型。 数据类型的交集必须为数值数据类型，该类型在表达式计算器执行隐式数值转换的规则中指定。 两个数值数据类型的交集不能为空。 有关详细信息，请参阅 [Integration Services Data Types in Expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md)。  
  
-   **字符串***expression1* 和 *expression2* 必须为字符串数据类型：DT_STR 或 DT_WSTR。 两个表达式的计算结果可以为不同的字符串数据类型。 结果为 DT_WSTR 数据类型，其长度为较长参数的长度。  
  
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
  
 有关数据类型的详细信息，请参阅 [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md)。  
  
## <a name="expression-examples"></a>表达式示例  
 以下示例显示根据条件计算结果为 `savannah` 或 `unknown`的表达式。  
  
```  
@AnimalName == "Elephant"? "savannah": "unknown"  
```  
  
 以下示例显示引用 **ListPrice** 列的表达式。 **ListPrice** 具有 DT_CY 数据类型。 表达式根据条件将 **ListPrice** 乘以 0.2 或 0.1。  
  
```  
ListPrice < 350.00 ? ListPrice * .2 : ListPrice * .1  
```  
  
## <a name="see-also"></a>另请参阅  
 [运算符优先级和结合性](../../integration-services/expressions/operator-precedence-and-associativity.md)   
 [运算符（SSIS 表达式）](../../integration-services/expressions/operators-ssis-expression.md)  
  
  
