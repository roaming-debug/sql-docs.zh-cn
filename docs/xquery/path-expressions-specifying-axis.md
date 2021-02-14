---
title: 在路径表达式步骤中指定轴 |Microsoft Docs
description: 了解如何在 XQuery 路径表达式中指定轴步骤。
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
- attribute axis [SQL Server]
- axis step [XQuery]
- descendant axis
- self axis
- path expressions [XQuery]
- child axis
- descendant-or-self axis
- parent axis
ms.assetid: c44fb843-0626-4496-bde0-52ca0bac0a9e
author: rothja
ms.author: jroth
ms.openlocfilehash: 2207dcce2787fb6f51668e777b01ce345d1f33ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344660"
---
# <a name="path-expressions---specifying-axis"></a>路径表达式 - 指定轴
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  路径表达式中的轴步骤包括以下部分：  
  
-   轴  
  
-   [节点测试](../xquery/path-expressions-specifying-node-test.md)  
  
-   [零或更多步骤限定符（可选）](../xquery/path-expressions-specifying-predicates.md)  
  
 有关详细信息，请参阅 [路径表达式 &#40;XQuery&#41;](../xquery/path-expressions-xquery.md)。  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的 XQuery 实现支持以下轴步骤：  
  
|轴|描述|  
|----------|-----------------|  
|**子代**|返回上下文节点的子级。|  
|**descendant**|返回上下文节点的所有后代。|  
|parent|返回上下文节点的父级。|  
|**attribute**|返回上下文节点的属性。|  
|**解压**|返回上下文节点本身。|  
|**后代或自身**|返回上下文节点及其所有后代。|  
  
 所有这些轴（ **父** 轴除外）都是向前轴。 **父** 轴是反向轴，因为它在文档层次结构中向后搜索。 例如，相对路径表达式 `child::ProductDescription/child::Summary` 包含两个步骤，每个步骤均指定一个 `child` 轴。 第一步将检索 \<ProductDescription> 上下文节点的子元素。 对于每个 \<ProductDescription> 元素节点，第二个步骤检索 \<Summary> 元素节点子级。  
  
 相对路径表达式 `child::root/child::Location/attribute::LocationID` 包含三个步骤。 前两个步骤各指定一个 `child` 轴，第三步是指定 `attribute` 轴。 针对 **ProductModel** 表中的生产说明 XML 文档执行时，该表达式将返回 `LocationID` 元素的 \<Location> 元素节点子元素的属性 \<root> 。  
  
## <a name="examples"></a>示例  
 本主题中的查询示例是针对 **AdventureWorks** 数据库中的 **xml** 类型列指定的。  
  
### <a name="a-specifying-a-child-axis"></a>A. 指定 child 轴  
 对于特定的产品型号，下面的查询 \<Features> \<ProductDescription> 从表中存储的产品目录说明中检索元素节点的元素节点子级 `Production.ProductModel` 。  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  /child::PD:ProductDescription/child::PD:Features')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 请注意上述查询的以下方面：  
  
-   `query()` **Xml** 数据类型的方法指定路径表达式。  
  
-   路径表达式中的两个步骤都指定一个 `child` 轴和节点名称（`ProductDescription` 和 `Features`）作为节点测试。 有关节点测试的信息，请参阅 [在路径表达式步骤中指定节点测试](../xquery/path-expressions-specifying-node-test.md)。  
  
### <a name="b-specifying-descendant-and-descendant-or-self-axes"></a>B. 指定 descendant 轴和 descendant-or-self 轴  
 以下示例使用 descendant 轴和 descendant-or-self 轴。 本示例中的查询是针对 **xml** 类型变量指定的。 该 XML 实例经过简化以方便说明生成的结果中的差异。  
  
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
declare @y xml  
set @y = @x.query('  
  /child::a/child::b  
')  
select @y  
```  
  
 在以下结果中，表达式返回 `<b>` 元素节点的 `<a>` 元素节点子级：  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
```  
  
 在此表达式中，如果为路径表达式指定了 descendant 轴   
  
 `/child::a/child::b/descendant::*`下，你将要求 <`b`> 元素节点的所有后代。  
  
 节点测试中的星号 (*) 以一个节点测试表示节点名称。 因此，descendant 轴的主节点类型（元素节点）确定返回节点的类型。 即表达式返回所有元素节点。 但不返回文本节点。 有关主节点类型及其与节点测试之间的关系的详细信息，请参阅 [在路径表达式步骤中指定节点测试](../xquery/path-expressions-specifying-node-test.md) 主题。  
  
 将返回元素节点 <`c`> 和 <`d`>，如以下结果所示：  
  
```  
<c>text2  
     <d>text3</d>  
</c>  
<d>text3</d>  
```  
  
 如果指定的是子代轴或自身轴而不是子代轴，则 `/child::a/child::b/descendant-or-self::*` 返回上下文节点、元素 <`b`> 及其子代。  
  
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
  
 以下针对 **AdventureWorks** 数据库的示例查询将检索 `Features` <> 元素的 <> 元素子级的所有后代元素节点 `ProductDescription` ：  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  /child::PD:ProductDescription/child::PD:Features/descendant::*  
')  
FROM  Production.ProductModel  
WHERE ProductModelID=19  
```  
  
### <a name="c-specifying-a-parent-axis"></a>C. 指定 parent 轴  
 下面的查询将返回 `Summary` `ProductDescription` 表中存储的产品目录 XML 文档中 <> 元素的 <> 元素子级 `Production.ProductModel` 。  
  
 此示例使用父轴返回到 <`Feature`> 元素的父级，并检索 `Summary` <> 元素的 <> 子元素 `ProductDescription` 。  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  
/child::PD:ProductDescription/child::PD:Features/parent::PD:ProductDescription/child::PD:Summary  
')  
FROM   Production.ProductModel  
WHERE  ProductModelID=19  
  
```  
  
 在此查询示例中，路径表达式使用 `parent` 轴。 您可以重写该表达式，不使用 parent 轴，如下所示：  
  
```  
/child::PD:ProductDescription[child::PD:Features]/child::PD:Summary  
```  
  
 以下是一个关于 parent 轴的更为有用的示例。  
  
 在 **ProductModel** 表的 **CatalogDescription** 列中存储的每个产品型号目录说明都有一个 `<ProductDescription>` 元素，该元素具有 `ProductModelID` 属性和 `<Features>` 子元素，如以下片段中所示：  
  
```  
<ProductDescription ProductModelID="..." >  
  ...  
  <Features>  
    <Feature1>...</Feature1>  
    <Feature2>...</Feature2>  
   ...  
</ProductDescription>  
```  
  
 查询在 FLWOR 语句中设置了一个迭代器变量 `$f`，以返回 `<Features>` 元素的元素子级。 有关详细信息，请参阅 [FLWOR 语句和迭代 &#40;XQuery&#41;](../xquery/flwor-statement-and-iteration-xquery.md)。 对于每个功能，`return` 子句都构造一个以下形式的 XML：  
  
```  
<Feature ProductModelID="...">...</Feature>  
<Feature ProductModelID="...">...</Feature>  
```  
  
 若要为 `ProductModelID` 每个 `<Feature`> 元素添加，则 `parent` 指定轴：  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
  for $f in /child::PD:ProductDescription/child::PD:Features/child::*  
  return  
   <Feature  
     ProductModelID="{ ($f/parent::PD:Features/parent::PD:ProductDescription/attribute::ProductModelID)[1]}" >  
          { $f }  
   </Feature>  
')  
FROM  Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 下面是部分结果：  
  
```  
<Feature ProductModelID="19">  
  <wm:Warranty   
   xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
    <wm:Description>parts and labor</wm:Description>  
  </wm:Warranty>  
</Feature>  
<Feature ProductModelID="19">  
  <wm:Maintenance   
   xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <wm:NoOfYears>10 years</wm:NoOfYears>  
    <wm:Description>maintenance contract available through your dealer   
                  or any AdventureWorks retail store.</wm:Description>  
  </wm:Maintenance>  
</Feature>  
<Feature ProductModelID="19">  
  <p1:wheel   
   xmlns:p1="https://www.adventure-works.com/schemas/OtherFeatures">  
      High performance wheels.  
  </p1:wheel>  
</Feature>  
```  
  
 请注意，在路径表达式中添加了谓词 `[1]`，这是为了确保返回单一值。  
  
  
