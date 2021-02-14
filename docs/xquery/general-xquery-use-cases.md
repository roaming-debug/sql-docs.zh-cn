---
title: 一般 XQuery 用例 |Microsoft Docs
description: 查看一般 XQuery 使用的几个示例，包括搜索、检索和列出产品目录中的特定数据。
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
- XQuery, general usage cases
ms.assetid: 5187c97b-6866-474d-8bdb-a082634039cc
author: rothja
ms.author: jroth
ms.openlocfilehash: 62fa5c7bba9133c9a8b0098142d97fdd422c47ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341908"
---
# <a name="general-xquery-use-cases"></a>XQuery 常规使用情况
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  本主题提供 XQuery 常规使用情况示例。  
  
## <a name="examples"></a>示例  
  
### <a name="a-query-catalog-descriptions-to-find-products-and-weights"></a>A. 查询目录说明以查找产品和权重  
 下面的查询将从产品目录说明中返回产品型号 ID 和权重（如果它们存在的话）。 该查询将构造如下形式的 XML 内容：  
  
```  
<Product ProductModelID="...">  
  <Weight>...</Weight>  
</Product>  
```  
  
 以下是查询语句：  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  <Product  ProductModelID="{ (/p1:ProductDescription/@ProductModelID)[1] }">  
     {   
       /p1:ProductDescription/p1:Specifications/Weight   
     }   
  </Product>  
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription is not null  
```  
  
 请注意上述查询的以下方面：  
  
-   XQuery prolog 中的 **namespace** 关键字用于定义在查询主体中使用的命名空间前缀。  
  
-   查询主体用于构造所需的 XML。  
  
-   在 WHERE 子句中， **存在 ()** 方法用于仅查找包含产品目录说明的行。 也就是说，包含 <`ProductDescription`> 元素的 XML。  
  
 结果如下：  
  
```  
<Product ProductModelID="19"/>  
<Product ProductModelID="23"/>   
<Product ProductModelID="25"/>   
<Product ProductModelID="28"><Weight>Varies with size.</Weight></Product>  
<Product ProductModelID="34"/>  
<Product ProductModelID="35"/>  
```  
  
 下面的查询检索的信息相同，但仅适用于目录说明中包含权重、<`Weight`> 元素 <> 元素的产品型号 `Specifications` 。 此示例使用 WITH XMLNAMESPACES 来声明 pd 前缀与其命名空间的绑定。 这样，就不会在 **query ()** 方法和 **()** 方法中描述绑定。  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT CatalogDescription.query('  
          <Product  ProductModelID="{ (/pd:ProductDescription/@ProductModelID)[1] }">  
                 {   
                      /pd:ProductDescription/pd:Specifications/Weight   
                 }   
          </Product>  
') as x  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('/pd:ProductDescription/pd:Specifications//Weight ') = 1  
```  
  
 在前面的查询中，WHERE 子句中的 **xml** 数据类型 **() 方法存在**，以查看 `Weight` <> 元素中是否存在 <> 元素 `Specifications` 。  
  
### <a name="b-find-product-model-ids-for-product-models-whose-catalog-descriptions-include-front-angle-and-small-size-pictures"></a>B. 为其目录说明包含前角和小幅图片的产品型号查找产品型号 ID  
 XML 产品目录说明包含产品图片 <`Picture`> 元素。 每个图片都具有若干属性。 其中包括图片角度、<`Angle`> 元素以及大小 <`Size`> 元素。  
  
 对于其目录说明包含前角和小幅图片的产品型号，查询将构造具有以下形式的 XML：  
  
```  
< Product ProductModelID="...">  
  <Picture>  
    <Angle>front</Angle>  
    <Size>small</Size>  
  </Picture>  
</Product>  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT CatalogDescription.query('  
   <pd:Product  ProductModelID="{ (/pd:ProductDescription/@ProductModelID)[1] }">  
      <Picture>  
         {  /pd:ProductDescription/pd:Picture/pd:Angle }   
         {  /pd:ProductDescription/pd:Picture/pd:Size }   
      </Picture>  
   </pd:Product>  
') as Result  
FROM  Production.ProductModel  
WHERE CatalogDescription.exist('/pd:ProductDescription/pd:Picture') = 1  
AND   CatalogDescription.value('(/pd:ProductDescription/pd:Picture/pd:Angle)[1]', 'varchar(20)')  = 'front'  
AND   CatalogDescription.value('(/pd:ProductDescription/pd:Picture/pd:Size)[1]', 'varchar(20)')  = 'small'  
```  
  
 请注意上述查询的以下方面：  
  
-   在 WHERE 子句中， **存在 ()** 方法用于仅检索具有 <> 元素的产品目录说明的行 `Picture` 。  
  
-   WHERE 子句使用 () 方法的 **值** 两次，比较 <`Size`> 和 <`Angle`> 元素的值。  
  
 下面是部分结果：  
  
```  
<p1:Product   
  xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
  ProductModelID="19">  
  <Picture>  
    <p1:Angle>front</p1:Angle>  
    <p1:Size>small</p1:Size>  
  </Picture>  
</p1:Product>  
...  
```  
  
### <a name="c-create-a-flat-list-of-the-product-model-name-and-feature-pairs-with-each-pair-enclosed-in-the-features-element"></a>C. 创建产品型号名称和功能对的简单列表，每个对都包含在元素中 \<Features>  
 在产品型号目录说明中，XML 包含若干种产品功能。 所有这些功能都包含在 <`Features`> 元素中。 查询使用 [XML 构造 (XQuery) ](../xquery/xml-construction-xquery.md) 来构造所需的 XML。 大括号中的表达式将替换为结果。  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  for $pd in /p1:ProductDescription,  
   $f in $pd/p1:Features/*  
  return  
   <Feature>  
     <ProductModelName> { data($pd/@ProductModelName) } </ProductModelName>  
     { $f }  
  </Feature>          
') as x  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 请注意上述查询的以下方面：  
  
-   $pd/p1： Features/* 仅返回 <> 的元素节点子级 `Features` ，但 $pd/p1：功能/节点 () 返回所有节点。 其中包括元素节点、文本节点、处理指令和注释。  
  
-   两个 FOR 循环将生成笛卡儿积，将从该积返回产品名称和单个功能。  
  
-   **ProductName** 是一个属性。 此查询中的 XML 构造将其作为一个元素返回。  
  
 下面是部分结果：  
  
```  
<Feature>  
 <ProductModelName>Mountain 100</ProductModelName>  
 <ProductModelID>19</ProductModelID>  
 <p1:Warranty   
   xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p1:WarrantyPeriod>3 year</p1:WarrantyPeriod>  
    <p1:Description>parts and labor</p1:Description>  
 </p1:Warranty>  
</Feature>  
<Feature>  
 <ProductModelName>Mountain 100</ProductModelName>  
 <ProductModelID>19</ProductModelID>  
 <p2:Maintenance xmlns:p2="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p2:NoOfYears>10</p2:NoOfYears>  
    <p2:Description>maintenance contact available through your dealer   
           or any AdventureWorks retail store.</p2:Description>  
    </p2:Maintenance>  
</Feature>  
...  
...      
```  
  
### <a name="d-from-the-catalog-description-of-a-product-model-list-the-product-model-name-model-id-and-features-grouped-inside-a-product-element"></a>D. 从产品型号的目录说明中，列出了在元素内分组的产品型号名称、模型 ID 和功能 \<Product>  
 以下查询使用产品型号的目录说明中存储的信息列出了在元素内分组的产品型号名称、模型 ID 和功能 \<Product> 。  
  
```  
SELECT ProductModelID, CatalogDescription.query('  
     declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <Product>  
         <ProductModelName>   
           { data(/pd:ProductDescription/@ProductModelName) }   
         </ProductModelName>  
         <ProductModelID>   
           { data(/pd:ProductDescription/@ProductModelID) }   
         </ProductModelID>  
         { /pd:ProductDescription/pd:Features/* }  
     </Product>          
') as x  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 下面是部分结果：  
  
```  
<Product>  
  <ProductModelName>Mountain 100</ProductModelName>  
  <ProductModelID>19</ProductModelID>  
  <p1:Warranty>... </p1:Warranty>  
  <p2:Maintenance>...  </p2:Maintenance>  
  <p3:wheel xmlns:p3="https://www.adventure-works.com/schemas/OtherFeatures">High performance wheels.</p3:wheel>  
  <p4:saddle xmlns:p4="https://www.adventure-works.com/schemas/OtherFeatures">  
    <p5:i xmlns:p5="http://www.w3.org/1999/xhtml">Anatomic design</p5:i> and made from durable leather for a full-day of riding in comfort.</p4:saddle>  
  <p6:pedal xmlns:p6="https://www.adventure-works.com/schemas/OtherFeatures">  
    <p7:b xmlns:p7="http://www.w3.org/1999/xhtml">Top-of-the-line</p7:b> clipless pedals with adjustable tension.</p6:pedal>  
   ...  
```  
  
### <a name="e-retrieve-product-model-feature-descriptions"></a>E. 检索产品型号功能说明  
 下面的查询构造包含一个 <`Product`> 元素的 XML，该元素具有 **ProducModelID**、 **ProductModelName** 特性和前两个产品功能。 具体而言，前两个产品功能是 <> 元素的前两个子元素 `Features` 。 如果有更多的功能，它将返回空的 <`There-is-more/`> 元素。  
  
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
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription is not NULL  
```  
  
 请注意上述查询的以下方面：  
  
-   用于 .。。返回循环结构检索前两个产品功能。 **() 函数位置** 用于查找元素在序列中的位置。  
  
### <a name="f-find-element-names-from-the-product-catalog-description-that-end-with-ons"></a>F. 从产品目录说明中查找以“ons”结尾的元素名称  
 下面的查询将搜索目录说明并返回 <`ProductDescription`> 元素中名称以 "ons" 结尾的所有元素。  
  
```  
SELECT ProductModelID, CatalogDescription.query('  
     declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      for $pd in /p1:ProductDescription/*[substring(local-name(.),string-length(local-name(.))-2,3)="ons"]  
      return   
          <Root>  
             { $pd }  
          </Root>  
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription is not NULL  
```  
  
 下面是部分结果：  
  
```  
ProductModelID   Result  
-----------------------------------------  
         19        <Root>         
                     <p1:Specifications xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">          
                          ...         
                     </p1:Specifications>         
                   </Root>          
```  
  
### <a name="g-find-summary-descriptions-that-contain-the-word-aerodynamic"></a>G. 查找包含单词“Aerodynamic”的概要说明  
 下面的查询将检索其目录说明在概要说明中包含单词“Aerodynamic”的产品型号：  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
          <Prod >  
             { /pd:ProductDescription/@ProductModelID }  
             { /pd:ProductDescription/pd:Summary }  
          </Prod>  
 ') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.value('  
     contains( string( (/pd:ProductDescription/pd:Summary)[1] ),"Aerodynamic")','bit') = 1  
```  
  
 请注意，SELECT 查询将指定 **query ()** 和 **value ()** **xml** 数据类型的方法。 因此，该查询中使用了前缀 pd，并且只使用 WITH XMLNAMESPACES 定义了一次该前缀 pd，而不是在两个不同的查询 prolog 中两次重复声明命名空间。  
  
 请注意上述查询的以下方面：  
  
-   WHERE 子句用于仅检索目录说明在 <> 元素中包含单词 "Aerodynamic" 的行 `Summary` 。  
  
-   **Contains ()** 函数用于查看文本中是否包含单词。  
  
-   **Xml** 数据类型的 **值 ()** 方法将 **包含 ()** 返回的值与1进行比较。  
  
 结果如下：  
  
```  
ProductModelID Result        
-------------- ------------------------------------------  
28     <Prod ProductModelID="28">  
        <pd:Summary xmlns:pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
       <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
         A TRUE multi-sport bike that offers streamlined riding and a  
         revolutionary design. Aerodynamic design lets you ride with the   
         pros, and the gearing will conquer hilly roads.</p1:p>  
       </pd:Summary>  
      </Prod>    
```  
  
### <a name="h-find-product-models-whose-catalog-descriptions-do-not-include-product-model-pictures"></a>H. 查找其目录说明不包含产品型号图片的产品型号  
 下面的查询将检索其目录说明不包含 <> 元素的产品型号的 Productmodelid `Picture` 。  
  
```  
SELECT  ProductModelID  
FROM    Production.ProductModel  
WHERE   CatalogDescription is not NULL  
AND     CatalogDescription.exist('declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /p1:ProductDescription/p1:Picture  
') = 0  
```  
  
 请注意上述查询的以下方面：  
  
-   如果在 WHERE 子句中 **存在 ()** 方法返回 False (0) ，则返回产品型号 ID。 否则，将不返回。  
  
-   由于所有产品说明都包含一个 <`Picture`> 元素，因此在这种情况下，结果集为空。  
  
## <a name="see-also"></a>另请参阅  
 [涉及层次结构的 Xquery](../xquery/xqueries-involving-hierarchy.md)   
 [涉及顺序的 Xquery](../xquery/xqueries-involving-order.md)   
 [Xquery 处理关系数据](../xquery/xqueries-handling-relational-data.md)   
 [XQuery 中的字符串搜索](../xquery/string-search-in-xquery.md)   
 [在 XQuery 中处理命名空间](../xquery/handling-namespaces-in-xquery.md)   
 [使用 WITH XMLNAMESPACES 将命名空间添加到查询](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [XML 数据 (SQL Server)](../relational-databases/xml/xml-data-sql-server.md)   
 [XQuery 语言参考 (SQL Server)](../xquery/xquery-language-reference-sql-server.md)  
  
  
