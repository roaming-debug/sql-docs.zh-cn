---
title: 位置函数 (XQuery) |Microsoft Docs
description: 了解 XQuery 函数位置 () ，该函数返回一个整数值，指示上下文项在项序列中的位置。
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- position function
- fn:position function
ms.assetid: f1bab9e4-1715-4c06-9cb0-06c7e0c9c97f
author: rothja
ms.author: jroth
ms.openlocfilehash: dd2be5cf246bd625e63c31975effa59fb1b919d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345034"
---
# <a name="context-functions---position-xquery"></a>上下文函数 - position (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  返回一个整数值，指示上下文项在当前处理的项序列中的位置。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:position() as xs:integer  
```  
  
## <a name="remarks"></a>备注  
 在中 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ， **fn： position ()** 只能在依赖于上下文的谓词的上下文中使用。 确切地说，仅可用在方括号 ([ ]) 内。与此函数比较不会在静态类型推导过程中减少基数。  
  
## <a name="examples"></a>示例  
 本主题提供针对数据库中各种 **xml** 类型列中存储的 xml 实例的 XQuery 示例 [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] 。  
  
### <a name="a-using-the-position-xquery-function-to-retrieve-the-first-two-product-features"></a>A. 使用 position() XQuery 函数检索前两个产品功能  
 下面的查询 `Features` 从产品型号目录说明中检索前两个功能，即 <> 元素的前两个子元素。 如果有更多功能，则会将 <`there-is-more/`> 元素添加到结果中。  
  
```  
SELECT CatalogDescription.query('  
     declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <Product>   
          { /pd:ProductDescription/@ProductModelID }  
          { /pd:ProductDescription/@ProductModelName }   
          {  
            for $f in /pd:ProductDescription/pd:Features/*[position()<=2]  
            return  
            $f   
          }  
          {  
            if (count(/pd:ProductDescription/pd:Features/*) > 2)  
            then <there-is-more/>  
            else ()  
          }   
     </Product>          
') as x  
FROM Production.ProductModel  
WHERE CatalogDescription is not null  
```  
  
 请注意上述查询的以下方面：  
  
-   [XQuery Prolog](../xquery/modules-and-prologs-xquery-prolog.md)中的 **namespace** 关键字用于定义在查询主体中使用的命名空间前缀。  
  
-   查询主体构造具有 \<Product> **ProductModelID** 和 **ProductModelName** 属性的元素的 XML，并将产品功能作为子元素返回。  
  
-   谓词中使用 **() 函数位置** 来确定 \<Features> 子元素在上下文中的位置。 如果是第一个或第二个功能，将返回该子元素。  
  
-   如果产品目录中有两个以上的功能，IF 语句将向 \<there-is-more/> 结果中添加一个元素。  
  
-   由于不是所有产品型号都将其目录说明存储在表中，因此使用 WHERE 子句来放弃 CatalogDescriptions 为 NULL 的行。  
  
 下面是部分结果：  
  
```  
<Product ProductModelID="19" ProductModelName="Mountain 100">  
  <p1:Warranty xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p1:WarrantyPeriod>3 year</p1:WarrantyPeriod>  
    <p1:Description>parts and labor</p1:Description>  
  </p1:Warranty>  
  <p2:Maintenance xmlns:p2="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p2:NoOfYears>10</p2:NoOfYears>  
    <p2:Description>maintenance contact available through your dealer or  
                    any AdventureWorks retail store.</p2:Description>  
  </p2:Maintenance>  
  <there-is-more/>  
</Product>   
...  
```  
  
## <a name="see-also"></a>另请参阅  
 [针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
