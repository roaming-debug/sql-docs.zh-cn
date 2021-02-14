---
title: 命名空间 uri 函数 (XQuery) |Microsoft Docs
description: 了解如何使用 XQuery 中的命名空间 uri 函数返回指定 QName 的命名空间 URI。
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
- fn:namespace-uri function
- namespace-uri function
ms.assetid: 9b48d216-26c8-431d-9ab4-20ab187917f4
author: rothja
ms.author: jroth
ms.openlocfilehash: 4a9734dd4efd2f4dcedd539218a0a7d989211749
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353286"
---
# <a name="functions-on-nodes---namespace-uri"></a>基于节点的函数 - namespace-uri
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  返回 *$arg* 中指定的 QName 的命名空间 URI 作为 xs： string。  
  
## <a name="syntax"></a>语法  
  
```  
fn:namespace-uri() as xs:string  
fn:namespace-uri($arg as node()?) as xs:string  
```  
  
## <a name="arguments"></a>参数  
 *$arg*  
 将检索其命名空间 URI 部分的节点名称。  
  
## <a name="remarks"></a>备注  
  
-   如果省略该参数，则默认值为上下文节点。  
  
-   在 SQL Server 中，无参数的 **fn： namespace ()** 只能在上下文相关的谓词的上下文中使用。 特别要指出的是，它只能在方括号 ([ ]) 内使用。  
  
-   如果 *$arg* 是空序列，则返回长度为零的字符串。  
  
-   如果 *$arg* 是其展开的 QName 不在命名空间中的元素或属性节点，则该函数将返回长度为零的字符串。  
  
## <a name="examples"></a>示例  
 本主题提供了针对 AdventureWorks 数据库中各种 **xml** 类型列中存储的 xml 实例的 XQuery 示例。  
  
### <a name="a-retrieve-namespace-uri-of-a-specific-node"></a>A. 检索特定节点的命名空间 URI  
 下面的查询是针对非类型化的 XML 实例指定的。 查询表达式 (`namespace-uri(/ROOT[1])`) 将检索指定节点的命名空间 URI 部分。  
  
```  
set @x='<ROOT><a>111</a></ROOT>'  
SELECT @x.query('namespace-uri(/ROOT[1])')  
```  
  
 由于指定的 QName 没有命名空间 URI 部分而只有本地名称部分，因此结果是长度为零的字符串。  
  
 下面的查询是针对指令类型化的 **xml** 列指定的。 表达式 `namespace-uri(/AWMI:root[1]/AWMI:Location[1])` 返回 <> 元素的第一个 <`Location`> 元素子级的命名空间 URI `root` 。  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     namespace-uri(/AWMI:root[1]/AWMI:Location[1])') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 结果如下：  
  
```  
https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions  
```  
  
### <a name="b-using-namespace-uri-without-argument-in-a-predicate"></a>B. 在谓词中使用没有参数的 namespace-uri()  
 对类型为 xml 的 CatalogDescription 列指定了以下查询。 表达式将返回其命名空间 URI 为 `https://www.adventure-works.com/schemas/OtherFeatures` 的所有元素节点。 指定的命名空间 **uri ()** 函数没有参数，并使用上下文节点。  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
   /p1:ProductDescription//*[namespace-uri() = "https://www.adventure-works.com/schemas/OtherFeatures"]  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 以下是部分结果：  
  
```  
<p1:wheel xmlns:p1="https://www.adventure-works.com/schemas/OtherFeatures">High performance wheels.</p1:wheel>  
<p2:saddle xmlns:p2="https://www.adventure-works.com/schemas/OtherFeatures">  
  <p3:i xmlns:p3="http://www.w3.org/1999/xhtml">Anatomic design</p3:i> and made from durable leather for a full-day of riding in comfort.</p2:saddle>  
...  
```  
  
 可以将以前查询中的命名空间 URI 更改为 `https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain`。 然后，您将收到 <> 元素的所有元素节点子级，该 `ProductDescription` 元素的命名空间 URI 部分为已展开的 QName `https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain` 。  
  
### <a name="implementation-limitations"></a>实现限制  
 限制如下：  
  
-   **命名空间 uri ()** 函数返回类型为 xs： string 而不是 Xs： anyURI 的实例。  
  
## <a name="see-also"></a>另请参阅  
 [节点上的函数](./xquery-functions-against-the-xml-data-type.md)   
 [&#40;XQuery&#41;本地名称函数 ](../xquery/functions-on-nodes-local-name.md)  
  
