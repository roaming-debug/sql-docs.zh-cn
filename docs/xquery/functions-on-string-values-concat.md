---
title: )  (XQuery 的 concat 函数 |Microsoft Docs
description: 了解 XQuery 函数 concat () ，该函数返回通过将指定为参数的零个或多个字符串串联而创建的字符串。
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:concat function
- concat function [XQuery]
ms.assetid: d50afd20-a297-445e-be9e-13b48017e7ca
author: rothja
ms.author: jroth
ms.openlocfilehash: 3ad92315e20ae314fb9a22ddea1241f379ecc90f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341921"
---
# <a name="functions-on-string-values---concat"></a>基于字符串值的函数 - concat
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  接受零或更多个字符串作为参数，并返回通过连接其中的每个参数值而创建的字符串。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:concat ($string as xs:string?  
           ,$string as xs:string?  
           [, ...]) as xs:string  
```  
  
## <a name="arguments"></a>参数  
 *$string*  
 可选择进行连接的字符串。  
  
## <a name="remarks"></a>备注  
 此函数必须至少包含两个参数。 如果一个参数为空序列，则将被作为零长度字符串处理。  
  
## <a name="supplementary-characters-surrogate-pairs"></a>补充字符（代理项对）  
 XQuery 函数中代理对的行为依赖于数据库兼容级别，并且在某些情况下，还依赖于函数的默认命名空间 URI。 有关详细信息，请参阅 [SQL Server 2016 中数据库引擎功能的重大更改](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md)主题中的 "XQuery 函数是代理感知" 部分。 另请参阅 [ALTER DATABASE 兼容级别 &#40;transact-sql&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) 和 [排序规则和 Unicode 支持](../relational-databases/collations/collation-and-unicode-support.md)。  
  
## <a name="examples"></a>示例  
 本主题提供了针对在 AdventureWorks 示例数据库的各种 **xml** 类型列中存储的 xml 实例的 XQuery 示例。  
  
### <a name="a-using-the-concat-xquery-function-to-concatenate-strings"></a>A. 使用 concat() XQuery 函数连接字符串  
 对于特定产品型号，此查询将返回通过连接保修期和保修说明而创建的字符串。 在目录说明文档中，<`Warranty`> 元素由 <`WarrantyPeriod`> 和 <的 `Description` 子元素组成。  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
SELECT CatalogDescription.query('  
    <Product   
        ProductModelID= "{ (/pd:ProductDescription/@ProductModelID)[1] }"  
        ProductModelName = "{ sql:column("PD.Name") }" >  
        {   
          concat( string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:WarrantyPeriod)[1]), "-",  
                  string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:Description)[1]))   
         }   
     </Product>  
 ') as Result  
FROM Production.ProductModel PD  
WHERE  PD.ProductModelID=28  
  
```  
  
 请注意上述查询的以下方面：  
  
-   在 SELECT 子句中，CatalogDescription 是一个 **xml** 类型列。 因此，将使用 [查询 () 方法 (XML 数据类型) ](../t-sql/xml/query-method-xml-data-type.md)、说明 () 。 XQuery 语句指定为该查询方法的参数。  
  
-   对其执行查询的文档将使用命名空间。 因此， **namespace** 关键字用于定义命名空间的前缀。 有关详细信息，请参阅 [XQuery Prolog](../xquery/modules-and-prologs-xquery-prolog.md)。  
  
 结果如下：  
  
```  
<Product ProductModelID="28" ProductModelName="Road-450">1 year-parts and labor</Product>  
```  
  
 上一个查询检索了有关特定产品的信息。 以下查询将针对为其存储了 XML 目录说明的所有产品检索同样的信息。 如果行中的 XML 文档具有 <> 元素，则 WHERE 子句中的 **xml** 数据类型 **()** 方法就会返回 True `ProductDescription` 。  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
    <Product   
        ProductModelID= "{ (/pd:ProductDescription/@ProductModelID)[1] }"   
        ProductName = "{ sql:column("PD.Name") }" >  
        {   
          concat( string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:WarrantyPeriod)[1]), "-",  
                  string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:Description)[1]))   
         }   
     </Product>  
 ') as Result  
FROM Production.ProductModel PD  
WHERE CatalogDescription.exist('//pd:ProductDescription ') = 1  
  
```  
  
 请注意， **()** **xml** 类型的方法所返回的布尔值与1进行比较。  
  
### <a name="implementation-limitations"></a>实现限制  
 限制如下：  
  
-   SQL Server 中的 **concat ()** 函数只接受 xs： string 类型的值。 其他值必须显式转换为 xs:string 或 xdt:untypedAtomic 类型。  
  
## <a name="see-also"></a>另请参阅  
 [针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
