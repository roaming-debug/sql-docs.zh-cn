---
title: XQuery 和静态键入 |Microsoft Docs
description: 了解 XQuery 中的静态类型推理和静态类型检查。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, static typing
- static typing
- checking static types
- inference [XQuery]
ms.assetid: d599c791-200d-46f8-b758-97e761a1a5c0
author: rothja
ms.author: jroth
ms.openlocfilehash: 6dcaa7bbbd5674c6db3b6d8ec5185cc31e890707
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352308"
---
# <a name="xquery-and-static-typing"></a>XQuery 与静态类型化
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的 XQuery 是一种静态类型化的语言。 即，在查询编译期间，如果特定函数或运算符不接受表达式返回的值的类型或基数，则会出现类型错误。 此外，静态类型检查还可以检测类型化的 XML 文档的路径表达式是否已正确类型化。 XQuery 编译器首先应用添加隐式运算（例如原子化）的规范化阶段，然后执行静态类型推导和静态类型检查。  
  
## <a name="static-type-inference"></a>静态类型推导  
 静态类型推导用于确定表达式的返回类型。 它通过运用输入参数的静态类型和运算的静态语义并推断结果的静态类型，来确定返回类型。 例如，通过以下方式来确定表达式 1 + 2.3 的静态类型：  
  
-   1的静态类型为 **xs： integer** ，而2.3 的静态类型为 **xs： decimal**。 根据动态语义，操作的静态语义 **+** 将整数转换为十进制，然后返回 decimal。 推断的静态类型将为 **xs： decimal**。  
  
 对于非类型化的 XML 实例，有一些特殊类型表示数据没有被类型化。 此信息用于静态类型检查期间和执行某些隐式转换。  
  
 对于类型化的数据，输入类型通过约束 XML 数据类型实例的 XML 架构集合来推断。 例如，如果架构仅允许类型为 **xs： integer** 的元素，则使用该元素的路径表达式的结果将为类型为 **xs： integer** 的零个或多个元素。 当前通过使用表达式来表示， `element(age,xs:integer)*` 其中星号 (\*) 指示所生成类型的基数。 在此示例中，表达式可能会导致名称为 "age" 的零个或多个元素，并键入 **xs： integer**。 其他基数是完全相同的，只使用 "类型名称" 来表示，使用 "问号" (**？**) ，用一个或多个问号表示，并使用加号 (**+**) 表示。  
  
 有时，静态类型推导可以推断出表达式会始终返回空序列。 例如，如果类型化的 XML 数据类型的路径表达式在 \<name> \<customer> (/customer/name) 的元素内查找元素，但该架构不允许在中使用 \<name> \<customer> ，则静态类型推理将推断结果将为空。 这将用于检测不正确的查询并将其报告为静态错误，除非该表达式 () 或 **数据 ( () )**。  
  
 在 XQuery 规范的正式语义中介绍了详细的推导规则。 Microsoft 对这些规则仅稍加修改，以使用类型化的 XML 数据类型实例。 对该规范所做的最重要的更改是，隐式文档节点了解 XML 数据类型实例的类型。 因此，将根据该信息精确类型化 form /age 的路径表达式。  
  
 通过使用 [SQL Server Profiler 模板和权限](../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)，您可以查看作为查询编译的一部分返回的静态类型。 若要查看这些静态类型，跟踪必须包括 TSQL 事件类别中的 XQuery 静态类型事件。  
  
## <a name="static-type-checking"></a>静态类型检查  
 静态类型检查用于确保运行时执行仅接收适合该运算的类型的值。 由于无需在运行时检查类型，因此在编译早期可以检测到潜在的错误。 这有助于改善性能。 但是，静态类型化要求查询编写器在表述查询时要更加小心。  
  
 以下为可以使用的适当类型：  
  
-   函数或运算明确允许使用的类型。  
  
-   明确允许使用的类型的子类型。  
  
 子类型是根据使用派生（通过限制或扩展 XML 架构）的子类型化规则定义的。 例如，如果类型为 S 的所有值还是类型 T 的实例，则类型 S 就是类型 T 的子类型。  
  
 此外，根据 XML 架构类型的层次结构，所有整数值还都是小数值。 但是，并非所有小数值都是整数。 因此，整数是小数的子类型，反之则不然。 例如， **+** 操作仅允许某些类型的值，如数值类型 **xs： integer**、 **xs： decimal**、 **xs： float** 和 **xs： double**。 如果传递其他类型的值（如 **xs： string**），则操作将引发类型错误。 这称为强类型化。 其他类型（例如用于表示非类型化的 XML 的原子类型）的值可以隐式转换为运算所接受的类型的值。 这称为弱类型化。  
  
 如果要求在隐式转换后进行弱类型化，则静态类型检查可保证只有带有正确基数的允许使用类型的值才会被传递到运算。 对于 "string" + 1，它识别 "string" 的静态类型为 **xs： string**。 由于这不是操作允许的类型 **+** ，因此引发了类型错误。  
  
 在将任意表达式 E2 与任意表达式 E1 的结果相加 (E1 + E2) 的情况下，静态类型推导将首先确定 E1 和 E2 的静态类型，然后检查它们的静态类型是否为该运算允许使用的类型。 例如，如果 E1 的静态类型可以是 **xs： string** 或 **xs： integer**，则静态类型检查将引发类型错误，即使运行时的某些值可能是整数。 如果 E1 的静态类型为 **xs： integer&#42;**，就会出现这种情况。 由于 **+** 操作仅接受一个整数值，E1 可以返回零或大于1，因此静态类型检查会引发错误。  
  
 如前面所述，类型推导经常推断出不同于所传递数据的、用户不了解的数据类型。 在这些情况下，用户就必须重写查询。 以下为一些常见的情况：  
  
-   从类型推断出更全面的类型，例如超类型或联合类型。 如果该类型为原子类型，则应该使用转换表达式或构造函数来指明实际的静态类型。 例如，如果表达式 E1 的推断类型是 **xs： string** 或 **xs： integer** 之间的选择，而加法要求使用 **xs： integer**，则应该写入 `xs:integer(E1) + E2` 而不是 `E1+E2` 。 如果遇到无法转换为 **xs： integer** 的字符串值，此表达式可能在运行时失败。 但是，这样编写后表达式就可以通过静态类型检查了。 此表达式映射到空序列。  
  
-   从类型推断出比数据实际包含的基数更高的基数。 这种情况经常发生，因为 **xml** 数据类型可以包含多个顶级元素，xml 架构集合无法对此进行约束。 为了减少静态类型并保证实际上最多传递一个值，应使用位置谓词 `[1]`。 例如，若要在顶级 a 元素下使元素 `c` 的属性 `b` 的值加 1，则必须编写 `write (/a/b/@c)[1]+1`。 此外，DOCUMENT 关键字可与 XML 架构集合一起使用。  
  
-   某些操作在推导期间会丢失类型信息。 例如，如果无法确定节点的类型，则该节点的类型将变为 " **anyType**"。 这不会隐式转换为任何其他类型。 在使用父轴进行导航期间，尤其会发生这些转换。 如果表达式将产生静态类型错误，则应避免使用这些操作并重写查询。  
  
## <a name="type-checking-of-union-types"></a>联合类型的类型检查  
 由于类型检查，联合类型需要小心处理。 下列示例中说明了其中两个问题。  
  
### <a name="example-function-over-union-type"></a>示例：联合类型的函数  
 考虑 `r` 联合类型 <> 的元素定义：  
  
```  
<xs:element name="r">  
<xs:simpleType>  
   <xs:union memberTypes="xs:int xs:float xs:double"/>  
</xs:simpleType>  
</xs:element>  
```  
  
 在 XQuery 上下文内，"average" 函数 `fn:avg (//r)` 将返回静态错误，因为 XQuery 编译器无法为 fn： avg () 的参数中的 <> 元素添加不同类型的值 (**xs： int**、 **xs： float** 或 **xs： double**) `r` 。 **** 若要解决此问题，请将函数调用重写为 `fn:avg(for $r in //r return $r cast as xs:double ?)`。  
  
### <a name="example-operator-over-union-type"></a>示例：联合类型的运算符  
 加法运算（“+”）要求使用精确类型的操作数。 因此，该表达式将 `(//r)[1] + 1` 返回一个静态错误，该错误具有前面描述的元素 <> 的类型定义 `r` 。 一种解决办法是将其重写为 `(//r)[1] cast as xs:int? +1`，其中“?”表示取 0 或 1 值。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 要求带有“?”的“cast as”，这是因为任何转换都可能由于运行时错误而导致产生空序列。  
  
## <a name="see-also"></a>另请参阅  
 [XQuery 语言参考 (SQL Server)](../xquery/xquery-language-reference-sql-server.md)  
  
  
