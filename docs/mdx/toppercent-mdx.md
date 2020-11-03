---
description: TopPercent (MDX)
title: TopPercent (MDX) |Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: f275628747d0b17ede6c76f67961fe5233e788c4
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243536"
---
# <a name="toppercent-mdx"></a>TopPercent (MDX)


  按降序对集进行排序，并返回一个最大值元组集，该元组集的累积合计等于或大于指定的百分比。  
  
## <a name="syntax"></a>语法  
  
```  
  
TopPercent(Set_Expression, Percentage, Numeric_Expression)   
```  
  
## <a name="arguments"></a>参数  
 *Set_Expression*  
 返回集的有效多维表达式 (MDX)。  
  
 *百分比*  
 有效的数值表达式，指定要返回的元组的百分比。  
  
> [!IMPORTANT]  
>  *百分比*  需要为正值;负值会生成错误。  
  
 *Numeric_Expression*  
 返回数字的有效数值表达式，通常为单元坐标的多维表达式 (MDX)。  
  
## <a name="remarks"></a>备注  
 **TopPercent** 函数计算指定数值表达式对指定集求得的总和，并按降序对集进行排序。 然后，该函数返回具有最高值的元素，其总合计值的累积百分比至少是指定的百分比。 该函数返回累积合计至少达到指定百分比的最小子集。 返回的元素按从大到小的顺序排序。  
  
> [!WARNING]  
>  如果 *Numeric_Expression*  返回任何负值，则 **TopPercent** 只返回一个 (1) 行。  
>   
>  请参阅针对此行为的详细演示的第二个示例。  
  
> [!IMPORTANT]  
>  与 [BottomPercent](../mdx/bottompercent-mdx.md) 函数一样， **TopPercent** 函数始终中断层次结构。  
  
## <a name="examples"></a>示例  

### <a name="a-return-toppercent"></a>A. 返回 TopPercent

 下面的示例返回对于“自行车”类别，帮助实现前 10% 的分销商销售额的最佳城市。 结果将按降序排序，并且从具有最高销售额的城市开始。  
  
```  
SELECT [Measures].[Reseller Sales Amount] ON 0,  
TopPercent  
   ({[Geography].[Geography].[City].Members}  
   , 10  
   , [Measures].[Reseller Sales Amount]  
   ) ON 1  
FROM [Adventure Works]  
WHERE([Product].[Product Categories].[Bikes])  
```  
  
 上面的表达式生成以下结果：  
  
|City|Reseller Sales Amount|  
|-|---------------------------|  
|Toronto|$3508904.84|  
|London|$1521530.09|  
|Seattle|$1209418.16|  
|Paris|$1170425.18|  
  
 使用下面的查询可以获取原始数据集，并返回 588 行：  
  
```  
SELECT [Measures].[Reseller Sales Amount] ON 0,  
Order  
   ({[Geography].[Geography].[City].Members}  
   , [Measures].[Reseller Sales Amount]  
   , BDESC  
   ) ON 1  
FROM [Adventure Works]  
WHERE([Product].[Product Categories].[Bikes])  
  
```  
  
### <a name="b-understand-the-effect-of-negative-values"></a>B. 了解负值的效果

 下面的演练将帮助了解 *Numeric_Expression* 中的负值的影响。 首先，将生成一些可展示该行为的上下文。  
  
 下面的查询将返回一个表，由分销商的“Sales Amount”、“Total Product Cost”和“Gross Profit”构成，并且按利润的降序排序。 请注意，对于利润仅存在负值；因此，最小的利润损失将出现在最顶部。  
  
```  
SELECT { [Measures].[Reseller Sales Amount], [Measures].[Reseller Total Product Cost], [Measures].[Reseller Gross Profit] } ON columns  
     ,  ORDER( [Product].[Product Categories].[Bikes].[Touring Bikes].children, [Measures].[Reseller Gross Profit], BDESC )   ON rows  
FROM [Adventure Works]  
  
```  
  
 上述查询会返回以下结果；为便于阅读，中间部分的行已删除。  
  
|观光自行车|Reseller Sales Amount|Reseller Total Product Cost|Reseller Gross Profit|  
|-|---------------------------|---------------------------------|---------------------------|  
|Touring-2000 Blue, 50|$157444.56|$163112.57| ($5668.01) |  
|旅行-2000 蓝，46|$321027.03|$333021.50| ($11994.47) |  
|Touring-3000 Blue, 62|$87773.61|$100133.52| ($12359.91) |  
|...|...|...|...|  
|旅行-1000 黄色，46|$1016312.83|$1234454.27| ($218141.44) |  
|Touring-1000 Yellow, 60|$1184363.30|$1443407.51| ($259044.21) |  
  
 现在，如果要求您按利润展示前 100% 的自行车，则编写如下查询：  
  
```  
SELECT { [Measures].[Reseller Sales Amount], [Measures].[Reseller Total Product Cost], [Measures].[Reseller Gross Profit] } ON columns  
     ,  TOPPERCENT( [Product].[Product Categories].[Bikes].[Touring Bikes].children, 100,[Measures].[Reseller Gross Profit] )   ON rows  
FROM [Adventure Works]  
  
```  
  
 请注意，查询要求百分之百 (100%)；这意味着应返回所有行。 但是，由于 *Numeric_Expression* 中有负值，因此只返回一行。  
  
|观光自行车|Reseller Sales Amount|Reseller Total Product Cost|Reseller Gross Profit|  
|-|---------------------------|---------------------------------|---------------------------|  
|Touring-2000 Blue, 50|$157444.56|$163112.57| ($5668.01) |  
  
## <a name="see-also"></a>另请参阅  
 [MDX 函数引用 (MDX)](../mdx/mdx-function-reference-mdx.md)  
  
  
