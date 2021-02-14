---
title: 原子化 (XQuery) |Microsoft Docs
description: 了解 XQuery 中的原子化的过程，在此过程中，将提取项的类型化值。
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, atomization
- atomization [XQuery]
ms.assetid: e3d7cf2f-c6fb-43c2-8538-4470a6375af5
author: rothja
ms.author: jroth
ms.openlocfilehash: e12ac51b38073363edf7774f4ac8046c1985b38c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340535"
---
# <a name="atomization-xquery"></a>原子化 (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  原子化是提取项的类型化值的过程。 在某些环境下，此进程是隐式进行的。 某些 XQuery 运算符（如算术运算符和比较运算符）依赖于此进程。 例如，将算术运算符直接应用于节点时，将首先通过隐式调用 [数据函数](../xquery/data-accessor-functions-data-xquery.md)来检索节点的类型化值。 这将把原子值作为操作数传递给算术运算符。  
  
 例如，下面的查询将返回 LaborHours 属性的总数。 在这种情况下， **数据 ()** 将隐式应用于属性节点。  
  
```  
declare @x xml  
set @x='<ROOT><Location LID="1" SetupTime="1.1" LaborHours="3.3" />  
<Location LID="2" SetupTime="1.0" LaborHours="5" />  
<Location LID="3" SetupTime="2.1" LaborHours="4" />  
</ROOT>'  
-- data() implicitly applied to the attribute node sequence.  
SELECT @x.query('sum(/ROOT/Location/@LaborHours)')  
```  
  
 虽然不是必需的，但你也可以显式指定 **数据 ()** 函数：  
  
```  
SELECT @x.query('sum(data(ROOT/Location/@LaborHours))')  
```  
  
 隐式原子化的另一个示例是使用算术运算符。 **+** 运算符要求原子值，并隐式应用 **数据 ()** 来检索 LaborHours 属性的原子值。 此查询是针对 ProductModel 表中 **xml** 类型的指令列指定的。 下面的查询将返回 LaborHours 属性三次。 在该查询中，请注意下列情况：  
  
-   构造 OrignialLaborHours 属性时，原子化隐式应用于 `$WC/@LaborHours` 返回的单独序列。 LaborHours 属性的类型化值被分配给 OrignialLaborHours。  
  
-   在构造 UpdatedLaborHoursV1 属性时，算术运算符需要原子值。 因此， **数据 ()** 将隐式应用到 () 返回的 LaborHours 属性 `$WC/@LaborHours` 。 然后，原子值 1 添加到该属性。 属性 UpdatedLaborHoursV2 的构造显示 **() 的数据** 的显式应用程序，但不是必需的。  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in /AWMI:root/AWMI:Location[1]  
        return  
            <WC OriginalLaborHours = "{ $WC/@LaborHours }"  
                UpdatedLaborHoursV1 = "{ $WC/@LaborHours + 1 }"   
                UpdatedLaborHoursV2 = "{ data($WC/@LaborHours) + 1 }" >  
            </WC>') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 结果如下：  
  
```  
<WC OriginalLaborHours="2.5"   
    UpdatedLaborHoursV1="3.5"   
    UpdatedLaborHoursV2="3.5" />  
```  
  
 原子化将导致简单类型、空集、静态类型的实例错误。  
  
 原子化也会出现在传递给函数的比较表达式参数、函数返回的值、 **强制转换 ()** 表达式和在 order by 子句中传递的排序表达式。  
  
## <a name="see-also"></a>另请参阅  
 [XQuery 基础知识](../xquery/xquery-basics.md)   
 [XQuery&#41;&#40;比较表达式 ](../xquery/comparison-expressions-xquery.md)   
 [针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
