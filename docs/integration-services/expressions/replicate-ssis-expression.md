---
description: REPLICATE（SSIS 表达式）
title: REPLICATE（SSIS 表达式）| Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- REPLICATE function
ms.assetid: e7a37b93-6d1d-42d5-9a65-de1790abf6a5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 66560e6c72a0b6a5d36a2eee744d81eb1c232fda
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88477439"
---
# <a name="replicate-ssis-expression"></a>REPLICATE（SSIS 表达式）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  返回多次复制后的字符表达式。 *times* 参数的计算结果必须为整数。  
  
> [!NOTE]  
>  REPLICATE 函数经常使用长字符串，因此更有可能受到对表达式长度 4000 个字符的限制。 如果某表达式的计算结果具有 Integration Services 数据类型 DT_WSTR 或 DT_STR，则该表达式将被在 4000 个字符处截断。 如果子表达式的结果类型为 DT_STR 或 DT_WSTR，则该子表达式将同样被截断为 4000 个字符，与总体表达式结果类型无关。 可以适当处理截断的结果，否则会导致警告或错误。 有关详细信息，请参阅[语法 (SSIS)](../../integration-services/expressions/syntax-ssis.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
REPLICATE(character_expression,times)  
```  
  
## <a name="arguments"></a>参数  
 *character_expression*  
 要复制的字符表达式。  
  
 *times*  
 指定 character_expression 复制次数的整数表达式。  
  
## <a name="result-types"></a>结果类型  
 DT_WSTR  
  
## <a name="remarks"></a>备注  
 如果 times 为 0，则该函数返回零长度的字符串。  
  
 如果 *times* 为负数，则该函数返回一个错误。  
  
 *times* 参数还可以使用变量和列。  
  
 REPLICATE 只可用于 DT_WSTR 数据类型。 如果 character_expression 参数是字符串文字或数据类型为 DT_STR 的数据列，则它在 REPLICATE 执行操作前隐式转换为 DT_WSTR 数据类型。 其他数据类型必须显式转换为 DT_WSTR 数据类型。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)和[转换（SSIS 表达式）](../../integration-services/expressions/cast-ssis-expression.md)。  
  
 如果有一个参数为空，REPLICATE 将返回空结果。  
  
## <a name="expression-examples"></a>表达式示例  
 以下示例将一个字符串文字复制了三次。 返回结果为“Mountain BikeMountain BikeMountain Bike”。  
  
```  
REPLICATE("Mountain Bike", 3)  
```  
  
 以下示例将复制 **Name** 列中的值，复制次数为 **Times** 变量中的值。 如果 **Times** 为 3， **Name** 为 Touring Front Wheel，则返回结果为 Touring Front WheelTouring Front WheelTouring Front Wheel。  
  
```  
REPLICATE(Name, @Times)  
```  
  
 以下示例将复制 **Name** 变量中的值，复制次数为 **Times** 列中的值。 **Times** 是非整数数据类型，而表达式包含了一个到整数数据类型的显式转换。 如果 **Name** 包含 Helmet， **Times** 为 2，则返回结果为“HelmetHelmet”。  
  
```  
REPLICATE(@Name, (DT_I4(Times))  
```  
  
## <a name="see-also"></a>另请参阅  
 [函数（SSIS 表达式）](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
