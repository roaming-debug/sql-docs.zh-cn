---
description: FINDSTRING（SSIS 表达式）
title: FINDSTRING（SSIS 表达式）| Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- FINDSTRING function
ms.assetid: c83cb1b1-3c52-4496-b518-4c9253b9336d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3c4da6f54ead49d01d9d691cc081ac9b9955c693
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123252"
---
# <a name="findstring-ssis-expression"></a>FINDSTRING（SSIS 表达式）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  返回一个字符串的指定出现在字符表达式中的位置。 返回结果是该出现的索引（索引从 1 开始）。 字符串参数的取值必须为字符表达式，而 occurrence 参数的取值必须为整数。 如果找不到字符串，则返回值是 0。 如果字符串的出现次数少于所指定的 occurrence 参数，则返回值为 0。  
  
## <a name="syntax"></a>语法  
  
```  
  
FINDSTRING(character_expression, searchstring, occurrence)  
```  
  
## <a name="arguments"></a>参数  
 *character_expression*  
 是要在其中进行搜索的字符串。  
  
 *searchstring*  
 是要搜索的字符串。  
  
 *occurrence*  
 有符号或无符号的整数，用以指定要报告的 *searchstring* 的匹配项。  
  
## <a name="result-types"></a>结果类型  
 DT_I4  
  
## <a name="remarks"></a>备注  
 FINDSTRING 只能处理 DT_WSTR 数据类型。  如果 *character_expression* 和 *searchstring* 参数是字符串文字或数据类型为 DT_STR 的数据列，则它们在 FINDSTRING 执行操作前隐式转换为 DT_WSTR 数据类型。 其他数据类型必须显式转换为 DT_WSTR 数据类型。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)和[转换（SSIS 表达式）](../../integration-services/expressions/cast-ssis-expression.md)。  
  
 如果 *character_expression* 或 *searchstring* 为 null，则 FINDSTRING 返回 null。  
  
 在 *occurrence* 参数中使用值 1 将获得第一次出现的索引，使用 2 将获得第二次出现的索引，依此类推。  
  
 *occurrence* 必须为值大于 0 的整数。  
  
## <a name="expression-examples"></a>表达式示例  
 此示例使用一个字符串文字。 它返回值 11。  
  
```  
FINDSTRING("New York, NY, NY", "NY", 1)   
```  
  
 此示例使用一个字符串文字。 由于字符串“NY”只出现两次，所以返回结果是 0。  
  
```  
FINDSTRING("New York, NY, NY", "NY", 3)   
```  
  
 此示例使用 **Name** 列。 它返回“Name”列中第二个“n”的位置。 返回结果因 **Name** 中值的不同而不同。 如果 **Name** 包含 Anderson，则函数返回 8。  
  
```  
FINDSTRING(Name, "n", 2)   
```  
  
 此示例使用 **Name** 和 **Size** 列。 它返回 **Name** 列中 **Size** 值的最左侧字符的位置。 返回结果因列值的不同而不同。 如果 **Name** 包含 Mountain,500Red,42，且 **Size** 包含 42，则返回结果为 17。  
  
```  
FINDSTRING(Name,Size,1)   
```  
  
## <a name="see-also"></a>另请参阅  
 [REPLACE（SSIS 表达式）](../../integration-services/expressions/replace-ssis-expression.md)   
 [函数（SSIS 表达式）](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
