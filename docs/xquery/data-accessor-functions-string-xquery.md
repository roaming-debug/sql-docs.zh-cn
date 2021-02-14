---
title: )  (XQuery 的字符串函数 |Microsoft Docs
description: 了解 XQuery 函数字符串 () ，该字符串返回以字符串表示的参数值。
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
- string function
- fn:string function
ms.assetid: 7baa2959-9340-429b-ad53-3df03d8e13fc
author: rothja
ms.author: jroth
ms.openlocfilehash: 9c334f2f78ae79b9a1af3a268728d6fcc5454f05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351085"
---
# <a name="data-accessor-functions---string-xquery"></a>数据取值函数 - string (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  返回表示为字符串的 *$arg* 的值。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:string() as xs:string  
fn:string($arg as item()?) as xs:string  
```  
  
## <a name="arguments"></a>参数  
 *$arg*  
 一个节点或原子值。  
  
## <a name="remarks"></a>备注  
  
-   如果 *$arg* 是空序列，则返回长度为零的字符串。  
  
-   如果 *$arg* 是节点，则该函数将返回使用字符串值访问器获取的节点的字符串值。 W3C XQuery 1.0 和 XPath 2.0 数据模型规范中对此进行了定义。  
  
-   如果 *$arg* 是原子值，则该函数返回由表达式 cast 作为 **xs： string**、 *$arg* 返回的同一字符串，但在其他情况下除外。  
  
-   如果 *$arg* 的类型为 **xs： ANYURI**，则 URI 将转换为字符串，而不会转义特殊字符。  
  
-   此实现、无参数的 **fn： string ()** 只能在上下文相关的谓词的上下文中使用。 特别要指出的是，它只能在方括号 ([ ]) 内使用。  
  
## <a name="examples"></a>示例  
 本主题提供了对存储在 AdventureWorks 数据库的各种 **xml** 类型列中的 xml 实例的 XQuery 示例。  
  
### <a name="a-using-the-string-function"></a>A. 使用 string 函数  
 下面的查询检索 `Features` <> 元素的 <> 子元素节点 `ProductDescription` 。  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
 /PD:ProductDescription/PD:Features  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 下面是部分结果：  
  
```  
<PD:Features xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   These are the product highlights.   
   <p1:Warranty xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p1:WarrantyPeriod>3 years</p1:WarrantyPeriod>  
    <p1:Description>parts and labor</p1:Description>  
   </p1:Warranty>  
       ...  
</PD:Features>  
```  
  
 如果指定 **字符串 ()** 函数，则会收到指定节点的字符串值。  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
 string(/PD:ProductDescription[1]/PD:Features[1])  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 下面是部分结果：  
  
```  
These are the product highlights.   
3 yearsparts and labor...    
```  
  
### <a name="b-using-the-string-function-on-various-nodes"></a>B. 对各种节点使用 string 函数  
 在下面的示例中，一个 XML 实例被分配给一个 xml 类型变量。 指定查询以说明将 **字符串 ()** 应用到各个节点的结果。  
  
```  
declare @x xml  
set @x = '<?xml version="1.0" encoding="UTF-8" ?>  
<!--  This is a comment -->  
<root>  
  <a>10</a>  
just text  
  <b attr="x">20</b>  
</root>  
'  
```  
  
 下面的查询检索文档节点的字符串值。 此值是通过串联所有后代文本节点的字符串值形成的。  
  
```  
select @x.query('string(/)')  
```  
  
 结果如下：  
  
```  
This is a comment 10  
just text  
 20  
```  
  
 下面的查询尝试检索处理指令节点的字符串值。 结果是一个空序列，因为它不包含文本节点。  
  
```  
select @x.query('string(/processing-instruction()[1])')  
```  
  
 下面的查询检索注释节点的字符串值并返回文本节点。  
  
```  
select @x.query('string(/comment()[1])')  
```  
  
 结果如下：  
  
```  
This is a comment   
```  
  
## <a name="see-also"></a>另请参阅  
 [针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
