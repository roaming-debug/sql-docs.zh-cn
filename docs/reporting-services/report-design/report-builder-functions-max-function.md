---
title: Max 函数（报表生成器）| Microsoft Docs
description: 了解 Max 函数，该函数返回报表生成器中的表达式所指定的全部非 NULL 数值的最大值。
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 61c4d6ff-6435-456a-9cbd-5113d2113e8a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7b218b63de094ef11d04af70b8492b8fbe3522c0
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596086"
---
# <a name="report-builder-functions---max-function"></a>报表生成器函数 - Max 函数
  返回在给定作用域上下文中由表达式指定的所有非 Null 数值的最大值。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="syntax"></a>语法  
  
```  
  
Max(expression, scope, recursive)  
```  
  
#### <a name="parameters"></a>参数  
 *expression*  
 (**Variant**) 要对其执行聚合的表达式。  
  
 *作用域*  
 (**String**) 可选。 包含要对其应用聚合函数的报表项的数据集、组或数据区域的名称。 如果未指定 *scope* ，则使用当前作用域。  
  
 *递归*  
 (**Enumerated Type**) 可选。 **Simple** （默认）或 **RdlRecursive**。 指定是否以递归方式执行聚合。  
  
## <a name="return-type"></a>返回类型  
 视表达式的类型而定。  
  
## <a name="remarks"></a>备注  
 表达式中指定的数据集必须具有相同的数据类型。 若要将具有多个数值数据类型的数据转换为同一数据类型，请使用类似 **CInt**、 **CDbl** 或 **CDec** 的转换函数。 有关详细信息，请参阅 [Type Conversion Functions](/dotnet/visual-basic/language-reference/functions/type-conversion-functions)（类型转换函数）。  
  
 *scope* 的值必须是字符串常量，不能是表达式。 对于外部聚合或未指定其他聚合的聚合， *scope* 必须引用当前作用域或包含作用域。 对于聚合的聚合，嵌套聚合可以指定子作用域。  
  
 *Expression* 可以包含对嵌套聚合函数的调用，但具有以下例外和条件：  
  
-   嵌套聚合的 *Scope* 必须与外部聚合的作用域相同，或者包含在外部聚合的作用域中。 对于表达式中的所有非重复作用域，一个作用域必须相对所有其他作用域处于子关系中。  
  
-   嵌套聚合的 *Scope* 不能为数据集的名称。  
  
-   *Expression* 不得包含 **First**、 **Last**、 **Previous** 或 **RunningValue** 函数。  
  
-   *Expression* 不得包含用于指定 *recursive* 的嵌套聚合。  
  
 有关详细信息，请参阅[聚合函数引用（报表生成器和 SSRS）](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md)和[总计、聚合和内置集合的表达式作用域（报表生成器和 SSRS）](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)。  
  
 有关递归聚合的详细信息，请参阅[创建递归层次结构组（报表生成器和 SSRS）](../../reporting-services/report-design/creating-recursive-hierarchy-groups-report-builder-and-ssrs.md)。  
  
## <a name="example"></a>示例  
 下面的代码示例提供了 `Year` 组或数据区域中的最大合计值。  
  
```  
=Max(Fields!OrderTotal.Value, "Year")  
```  
  
## <a name="see-also"></a>另请参阅  
 [在报表中使用表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [表达式示例（报表生成器和 SSRS）](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [表达式中的数据类型（报表生成器和 SSRS）](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [总计、聚合和内置集合的表达式作用域（报表生成器和 SSRS）](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
