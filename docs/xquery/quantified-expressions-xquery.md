---
title: )  (XQuery 的定量表达式 |Microsoft Docs
description: 了解如何使用 XQuery 中的定量表达式对一个或多个序列中的表达式应用存在性或通用定量。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- universal quantifiers
- effective Boolean value [XQuery]
- quantified expressions [XQuery]
- XQuery, quantified expressions
- every expression [XQuery]
- existential quantifiers [XQuery]
- some expression [XQuery]
- EBV
- expressions [XQuery], quantifiers
ms.assetid: a3a75a6c-8f67-4923-8406-1ada546c817f
author: rothja
ms.author: jroth
ms.openlocfilehash: e673895553b6a3207d0ef86337c47e2f6def2bfe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339910"
---
# <a name="quantified-expressions-xquery"></a>限定表达式 (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  存在量词和全称量词为应用于两个序列的布尔运算符指定不同的语义。 如下表所示。  
  
 *存在量词*  
 假设有两个序列，第一个序列中的某一项在第二个序列中存在匹配项，则根据所用的比较运算符，返回的值为 True。  
  
 *全称量词*  
 假设有两个序列，第一个序列中的每一项在第二个序列中都存在匹配项，则返回的值为 True。  
  
 XQuery 支持下列格式的限定表达式：  
  
```  
( some | every ) <variable> in <Expression> (,...) satisfies <Expression>  
```  
  
 可以在查询中使用这些表达式，来对作用于一个或多个序列上的表达式显式应用存在限定或全称限定。 在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中，`satisfies` 子句中表达式的结果可以为：节点序列、空序列或布尔值。 限定中将使用该表达式结果的有效布尔值。 如果限定符绑定的值中至少有一个值在满足表达式中的结果为 True，则使用 **some** 的存在性定量将返回 true。 使用 **每个** 的通用定量对于由限定符绑定的所有值都必须为 True。  
  
 例如，下面的查询检查每个 \<Location> 元素，以查看它是否具有 LocationID 属性。  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        if (every $WC in //AWMI:root/AWMI:Location   
            satisfies $WC/@LocationID)  
        then  
             <Result>All work centers have workcenterLocation ID</Result>  
         else  
             <Result>Not all work centers have workcenterLocation ID</Result>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 由于 LocationID 是元素的必需属性 \<Location> ，因此你会收到预期结果：  
  
```  
<Result>All work centers have Location ID</Result>   
```  
  
 您可以使用[值 () 方法](../t-sql/xml/value-method-xml-data-type.md)将结果返回给关系世界，而不是使用[query () 方法](../t-sql/xml/query-method-xml-data-type.md)，如以下查询中所示。 如果所有生产车间都具有 LocationID 属性，则查询返回 True。 否则，查询返回 False。  
  
```  
SELECT Instructions.value('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        every $WC in  //AWMI:root/AWMI:Location   
            satisfies $WC/@LocationID',   
  'nvarchar(10)') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 下面的查询检查是否其中一个产品图片为小图片。 在产品目录 XML 中，存储了各种大小不同、角度各异的产品图片。 您可能希望确保每个产品目录 XML 至少包括一个小尺寸图片。 下面的查询将实现此目的：  
  
```  
SELECT ProductModelID, CatalogDescription.value('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     some $F in /PD:ProductDescription/PD:Picture  
        satisfies $F/PD:Size="small"', 'nvarchar(20)') as SmallPicturesStored  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 下面是部分结果：  
  
```  
ProductModelID SmallPicturesStored   
-------------- --------------------  
19             true        
```  
  
## <a name="implementation-limitations"></a>实现限制  
 限制如下：  
  
-   绑定限定表达式中的变量时不支持类型断定。  
  
## <a name="see-also"></a>另请参阅  
 [XQuery 表达式](../xquery/xquery-expressions.md)  
  
  
