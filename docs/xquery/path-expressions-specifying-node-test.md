---
title: 指定路径表达式步骤中的节点测试 |Microsoft Docs
description: 了解如何在 XQuery 路径表达式的轴步骤中指定节点测试。
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
- axis step [XQuery]
- node test [XQuery]
ms.assetid: ffe27a4c-fdf3-4c66-94f1-7e955a36cadd
author: rothja
ms.author: jroth
ms.openlocfilehash: 578d10bc078edf886eb7980c3b7a099c99ef59a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338538"
---
# <a name="path-expressions---specifying-node-test"></a>路径表达式 - 指定节点测试
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  路径表达式中的轴步骤包括以下部分：  
  
-   [轴](../xquery/path-expressions-specifying-axis.md)  
  
-   节点测试  
  
-   [零或更多步骤限定符（可选）](../xquery/path-expressions-specifying-predicates.md)  
  
 有关详细信息，请参阅 [路径表达式 &#40;XQuery&#41;](../xquery/path-expressions-xquery.md)。  
  
 节点测试是一个条件，并且是路径表达式中的轴步骤的第二个组件。 在步骤中选定的所有节点都必须满足此条件。 对于路径表达式 `/child::ProductDescription`，节点测试为 `ProductDescription`。 此步骤将仅检索那些名为 ProductDescription 的元素节点子级。  
  
 有下列两种节点测试条件：  
  
-   节点名。 仅返回属于主要节点类型的、具有指定名称的节点。  
  
-   节点类型。 仅返回指定类型的节点。  
  
> [!NOTE]  
>  在 XQuery 路径表达式中指定的节点名不与 Transact-SQL 查询遵守相同的区分排序规则的规则，并且始终是区分大小写的。  
  
## <a name="node-name-as-node-test"></a>节点名作为节点测试  
 当指定节点名作为路径表达式步骤中的节点测试时，必须了解主要节点类型的概念。 每个轴（child、parent 或 attribute）都有一个主要节点类型。 例如：  
  
-   attribute 轴只能包含属性。 因此，属性节点就是 attribute 轴的主要节点类型。  
  
-   对于其他轴，如果轴所选择的节点可以包含元素节点，则元素就是该轴的主要节点类型。  
  
 指定节点名作为节点测试时，步骤将返回下列类型的节点：  
  
-   属于该轴的主要节点类型的节点。  
  
-   具有节点测试中所指定的名称的节点。  
  
 以下面的路径表达式为例：  
  
```  
child::ProductDescription   
```  
  
 此单步表达式指定 `child` 轴和节点名 `ProductDescription` 作为节点测试。 因此，此表达式将仅返回那些属于 child 轴的主要节点类型的、名为 ProductDescription 的元素节点。  
  
 路径表达式 `/child::PD:ProductDescription/child::PD:Features/descendant::*,` 包含三个步骤。 这些步骤将指定 child 轴和 descendant 轴。 在每个步骤中，都指定节点名来作为节点测试。 第三个步骤中的通配符 (`*`) 指明了属于 descendant 轴的主要节点类型的所有节点。 该轴的主要节点类型确定了选定节点的类型以及这些节点所选择的节点名筛选器。  
  
 因此，在对 **ProductModel** 表中的产品目录 XML 文档执行此表达式时，它将检索元素的元素节点子级的所有元素节点子级 \<Features> \<ProductDescription> 。  
  
 路径表达式由 `/child::PD:ProductDescription/attribute::ProductModelID` 两个步骤组成。 这两个步骤都指定节点名作为节点测试。 此外，第二个步骤将使用 attribute 轴。 因此，每个步骤都将选择属于其轴的主要节点类型的、具有指定名称的节点来作为节点测试。 因此，该表达式将返回元素节点的 **ProductModelID** 属性节点 \<ProductDescription> 。  
  
 指定节点名作为节点测试时，还可以使用通配符 (*) 来指定节点的本地名称或作为其命名空间前缀，如以下示例所示：  
  
```  
declare @x xml  
set @x = '  
<greeting xmlns="ns1">  
   <salutation>hello</salutation>  
</greeting>  
<greeting xmlns="ns2">  
   <salutation>welcome</salutation>  
</greeting>  
<farewell xmlns="ns1" />'  
select @x.query('//*:greeting')  
select @x.query('declare namespace ns="ns1"; /ns:*')  
```  
  
## <a name="node-type-as-node-test"></a>节点类型作为节点测试  
 若要查询除元素节点之外的节点类型，请使用节点类型测试。 如下表所示，共有四种节点类型测试。  
  
|节点类型|返回|示例|  
|---------------|-------------|-------------|  
|`comment()`|如果是注释节点，则返回 True。|`following::comment()` 将选择显示在上下文节点之后的所有注释节点。|  
|`node()`|无论是任何类型的节点，都返回 True。|`preceding::node()` 将选择显示在上下文节点之前的所有节点。|  
|`processing-instruction()`|如果是处理指令节点，则返回 True。|`self::processing instruction()` 将选择上下文节点内的所有处理指令节点。|  
|`text()`|如果是文本节点，则返回 True。|`child::text()` 将选择作为上下文节点的子级的文本节点。|  
  
 如果指定节点类型（例如 text() 或 comment() 等）作为节点测试，则该步骤将仅返回指定类型的节点，而不考虑轴的主要节点类型。 例如，以下路径表达式将仅返回上下文节点的注释节点子级：  
  
```  
child::comment()  
```  
  
 同样， `/child::ProductDescription/child::Features/child::comment()` 检索 \<Features> 元素节点的元素节点子级的注释节点子级 \<ProductDescription> 。  
  
## <a name="examples"></a>示例  
 下列示例将比较节点名和节点类型。  
  
### <a name="a-results-of-specifying-the-node-name-and-the-node-type-as-node-tests-in-a-path-expression"></a>A. 指定节点名和节点类型作为路径表达式中的节点测试的结果  
 在下面的示例中，将向 **xml** 类型变量分配一个简单的 xml 文档。 可使用不同的路径表达式来查询该文档。 然后比较结果。  
  
```  
declare @x xml  
set @x='  
<a>  
 <b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
 </b>  
</a>'  
select @x.query('  
/child::a/child::b/descendant::*  
')  
```  
  
 此表达式请求了 `<b>` 元素节点的后代元素节点。  
  
 节点测试中的星号 (`*`) 指明使用一个通配符来表示节点名。 descendant 轴将把该元素节点作为其主要节点类型。 因此，此表达式将返回元素节点 `<b>` 的所有后代元素节点。 也就是说，将返回元素节点 `<c>` 和 `<d>`，如以下结果所示：  
  
```  
<c>text2  
     <d>text3</d>  
</c>  
<d>text3</d>  
```  
  
 如果指定了 descendent-or-self 轴而不是 descendant 轴，则将返回上下文节点及其后代：  
  
```  
/child::a/child::b/descendant-or-self::*  
```  
  
 此表达式将返回元素节点 `<b>` 及其后代元素节点。 返回后代节点时，descendant-or-self 轴的主要节点类型（元素节点类型）确定了要返回的节点类型。  
  
 结果如下：  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
  
<c>text2  
     <d>text3</d>  
</c>  
  
<d>text3</d>   
```  
  
 上一个表达式使用了一个通配符来作为节点名。 可以改为使用 `node()` 函数，如以下表达式中所示：  
  
```  
/child::a/child::b/descendant::node()  
```  
  
 由于 `node()` 是节点类型，因此您将收到子代轴的所有节点。 结果如下：  
  
```  
text1  
<c>text2  
     <d>text3</d>  
</c>  
text2  
<d>text3</d>  
text3  
```  
  
 如果再次指定 descendant-or-self 轴和 `node()` 来作为节点测试，则您将收到所有后代节点、元素节点和文本节点，以及上下文节点和 `<b>` 元素。  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
text1  
<c>text2  
     <d>text3</d>  
</c>  
text2  
<d>text3</d>  
text3  
```  
  
### <a name="b-specifying-a-node-name-in-the-node-test"></a>B. 指定节点测试中的节点名  
 以下示例将指定节点名作为所有路径表达式中的节点测试。 因此，所有表达式都将返回属于轴的主要节点类型的、具有节点测试中所指定的节点名的节点。  
  
 下面的查询表达式 `Warranty` 从表中存储的产品目录 XML 文档中返回 <> 元素 `Production.ProductModel` ：  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::wm:Warranty  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 请注意上述查询的以下方面：  
  
-   XQuery prolog 中的 `namespace` 关键字定义了查询主体所使用的前缀。 有关 XQuery 序言的详细信息，请参阅 [Xquery prolog](../xquery/modules-and-prologs-xquery-prolog.md) 。  
  
-   路径表达式中的全部三个步骤都指定 child 轴和节点名来作为节点测试。  
  
-   不能在表达式的任何一个步骤中指定轴步骤的可选步骤限定符部分。  
  
 查询将返回 `Warranty` <> 元素的 <> 元素子级的 <> 元素子级 `Features` `ProductDescription` 。  
  
 结果如下：  
  
```  
<wm:Warranty xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
  <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
  <wm:Description>parts and labor</wm:Description>  
</wm:Warranty>     
```  
  
 在下面的查询中，路径表达式在节点测试中指定了一个通配符 (`*`)。  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::*  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 该通配符用来表示节点名。 因此，该查询将返回 `Features` <> 元素节点的 <> 元素节点子级的所有元素节点子级 `ProductDescription` 。  
  
 下面的查询类似于上一个查询，只不过除了该通配符以外，它还指定了一个命名空间。 因此，将返回该命名空间中的所有元素节点子级。 请注意，<`Features`> 元素可以包含来自不同命名空间的元素。  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::wm:*  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 可以使用通配符来作为命名空间前缀，如以下查询中所示：  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::*:Maintenance  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 此查询将 `Maintenance` 从产品目录 XML 文档中返回所有命名空间中的 <> 元素节点子级。  
  
### <a name="c-specifying-node-kind-in-the-node-test"></a>C. 指定节点测试中的节点类型  
 以下示例将指定节点类型作为路径表达式中的节点测试。 因此，所有表达式都将返回节点测试中所指定类型的节点。  
  
 在下面的查询中，路径表达式在其第三个步骤中指定了一个节点类型：  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::text()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 在下一个查询中，指定了下列内容：  
  
-   路径表达式包含三个步骤，它们用斜杠 (`/`) 分隔。  
  
-   每个步骤指定了一个 child 轴。  
  
-   前两个步骤指定节点名作为节点测试，第三个步骤指定节点类型作为节点测试。  
  
-   表达式返回 `Features` <> 元素节点 <> 元素子级的文本节点子级 `ProductDescription` 。  
  
 仅返回了一个文本节点。 结果如下：  
  
```  
These are the product highlights.   
```  
  
 下面的查询返回 <> 元素的注释节点子级 `ProductDescription` ：  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::comment()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 请注意上述查询的以下方面：  
  
-   第二个步骤指定节点类型作为节点测试。  
  
-   因此，表达式将返回 <> 元素节点的注释节点子级 `ProductDescription` 。  
  
 结果如下：  
  
```  
<!-- add one or more of these elements... one for each specific product in this product model -->  
<!-- add any tags in <specifications> -->      
```  
  
 下面的查询将检索顶级处理指令节点：  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::processing-instruction()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 结果如下：  
  
```  
<?xml-stylesheet href="ProductDescription.xsl" type="text/xsl"?>   
```  
  
 可以将一个字符串文字参数传递到 `processing-instruction()` 节点测试。 在这种情况下，查询将返回其名称属性值是参数中所指定的字符串文字的处理指令。  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::processing-instruction("xml-stylesheet")  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
## <a name="implementation-limitations"></a>实现限制  
 存在下列特定限制  
  
-   不支持扩展 SequenceType 节点测试。  
  
-   不支持处理指令（名称）。 而应将名称括在引号中。  
  
  
