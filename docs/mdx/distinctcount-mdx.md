---
description: DistinctCount (MDX)
title: DistinctCount (MDX) |Microsoft Docs
ms.date: 11/12/2020
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 28807d1a24f97a6b197ad56d0434399ab53cd742
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584844"
---
# <a name="distinctcount-mdx"></a>DistinctCount (MDX)


  返回集中非重复的非空元组的数目。  
  
## <a name="syntax"></a>语法  
  
```  
  
DistinctCount(Set_Expression)  
```  
  
## <a name="arguments"></a>自变量  
 *Set_Expression*  
 返回集的有效多维表达式 (MDX)。  
  
## <a name="remarks"></a>注解  
 **DistinctCount** 函数等效于 `Count(Distinct(Set_Expression), EXCLUDEEMPTY)` 。  
  
## <a name="examples"></a>示例  
 以下查询说明如何使用 DistinctCount 函数：  
  
 `WITH SET MySet AS`  
  
 `{[Customer].[Customer Geography].[Country].&[Australia],[Customer].[Customer Geography].[Country].&[Australia],`  
  
 `[Customer].[Customer Geography].[Country].&[Canada],[Customer].[Customer Geography].[Country].&[France],`  
  
 `[Customer].[Customer Geography].[Country].&[United Kingdom],[Customer].[Customer Geography].[Country].&[United Kingdom]}`  
  
 `*`  
  
 `{([Date].[Calendar].[Date].&[20010701],[Measures].[Internet Sales Amount] )}`  
  
 `//Returns the value 3 because Internet Sales Amount is null`  
  
 `//for the UK on the date specified`  
  
 `MEMBER MEASURES.SETDISTINCTCOUNT AS`  
  
 `DISTINCTCOUNT(MySet)`  
  
 `SELECT {MEASURES.SETDISTINCTCOUNT} ON 0`  
  
 `FROM [Adventure Works]`  
 
DistinctCount 函数返回集合中的非重复项数;在此示例中，可选的第二个参数用于排除没有给定元组值的项。 在这种情况下，第一个参数的集合中有四个不同的项，但该函数返回三个，这是因为只有澳大利亚、加拿大和法国的数据适用于 Internet 销售金额7月 1 2001 日。
 
## <a name="see-also"></a>另请参阅  
 [&#41; &#40;MDX&#41;&#40;集计数 ](../mdx/count-set-mdx.md)   
 [MDX 函数引用 (MDX)](../mdx/mdx-function-reference-mdx.md)  
  
  
