---
title: 处理 XQuery 中的命名空间 |Microsoft Docs
description: 查看在 XQuery 中处理命名空间的示例，其中包括如何声明新命名空间和默认命名空间。
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- declaring namespaces
- namespaces [XQuery]
- XQuery, namespaces
ms.assetid: 542b63da-4d3d-4ad5-acea-f577730688f1
author: rothja
ms.author: jroth
ms.openlocfilehash: acb1595ca2e7032d0363df3a1dd81740b7bcbf2d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341886"
---
# <a name="handling-namespaces-in-xquery"></a>处理 XQuery 中的命名空间
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  本主题提供有关处理查询中的命名空间的示例。  
  
## <a name="examples"></a>示例  
  
### <a name="a-declaring-a-namespace"></a>A. 声明一个命名空间  
 下面的查询将检索特定产品型号的生产步骤。  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        /AWMI:root/AWMI:Location[1]/AWMI:step  
    ') as x  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 下面是部分结果：  
  
```  
<AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">Insert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>. </AWMI:step>  
...  
```  
  
 请注意， **namespace** 关键字用于定义新的命名空间前缀 "AWMI："。 随后必须在查询中对该命名空间范围内的所有元素使用此前缀。  
  
### <a name="b-declaring-a-default-namespace"></a>B. 声明默认的命名空间  
 在上面的查询中，定义了一个新的命名空间前缀。 随后必须在查询中使用该前缀来选择所需的 XML 结构。 或者，也可以将命名空间声明为默认命名空间，如以下修改后的查询所示：  
  
```  
SELECT Instructions.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        /root/Location[1]/step  
    ') as x  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 下面是查询结果。  
  
```  
<step xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">Insert <material>aluminum sheet MS-2341</material> into the <tool>T-85A framing tool</tool>. </step>  
...  
```  
  
 请注意，在此示例中，使定义的命名空间 `"https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"` 覆盖了默认命名空间（空命名空间）。 因此，用于查询的路径表达式中将不再含有命名空间前缀。 并且，显示在结果中的元素名称中也不再含有命名空间前缀。 另外，默认命名空间适用于所有元素，但不适用于它们的属性。  
  
### <a name="c-using-namespaces-in-xml-construction"></a>C. 在 XML 构造中使用命名空间  
 定义新的命名空间时，不仅要将它们引入查询范围，还要将它们引入构造范围。 例如，构造 XML 时，可以使用“`declare namespace ...`”声明定义一个新的命名空间，然后将该命名空间用于所构造的要在查询结果内显示的任何元素和属性。  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     declare namespace myNS="uri:SomeNamespace";<myNS:Result>  
          { /ProductDescription/Summary }  
       </myNS:Result>  
  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 结果如下：  
  
```  
  
      <myNS:Result xmlns:myNS="uri:SomeNamespace">  
  <Summary xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
     Our top-of-the-line competition mountain bike. Performance-enhancing   
     options include the innovative HL Frame, super-smooth front   
     suspension, and traction for all terrain.</p1:p>  
  </Summary>  
</myNS:Result>  
```  
  
 或者，也可以在命名空间用作 XML 构造的一部分的每个位置显式定义命名空间，如以下查询所示：  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       <myNS:Result xmlns:myNS="uri:SomeNamespace">  
          { /ProductDescription/Summary }  
       </myNS:Result>  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
### <a name="d-construction-using-default-namespaces"></a>D. 使用默认命名空间进行构造  
 您还可以定义要在 XML 构造中使用的默认命名空间。 例如，下面的查询演示如何指定默认命名空间 "uri： SomeNamespace" \\ ，以用作构造的本地命名元素（例如元素）的默认命名空间 `<Result>` 。  
  
```  
SELECT CatalogDescription.query('  
      declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      declare default element namespace "uri:SomeNamespace";<Result>  
          { /PD:ProductDescription/PD:Summary }  
       </Result>  
  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 结果如下：  
  
```  
  
      <Result xmlns="uri:SomeNamespace">  
  <PD:Summary xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
         Our top-of-the-line competition mountain bike. Performance-  
         enhancing options include the innovative HL Frame, super-smooth   
         front suspension, and traction for all terrain.</p1:p>  
  </PD:Summary>  
</Result>  
```  
  
 请注意，通过覆盖元素的默认命名空间（空命名空间），XML 构造中的所有本地命名元素随后将被绑定到覆盖的默认命名空间。 因此，如果在构造 XML 时需要灵活利用空命名空间，则不要覆盖元素的默认命名空间。  
  
## <a name="see-also"></a>另请参阅  
 [使用 WITH XMLNAMESPACES 将命名空间添加到查询](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [XML 数据 (SQL Server)](../relational-databases/xml/xml-data-sql-server.md)   
 [XQuery 语言参考 (SQL Server)](../xquery/xquery-language-reference-sql-server.md)  
  
  
