---
title: number 函数 (XQuery) |Microsoft Docs
description: 了解返回指定参数数值的 XQuery 函数编号 () 。
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
- number function
- fn:number function
ms.assetid: dff6d19b-765c-4df9-afff-9a0e7be9b91b
author: rothja
ms.author: jroth
ms.openlocfilehash: 6aa035be000fbbda12a7205c33925daf656c3da0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345026"
---
# <a name="functions-on-nodes---number"></a>基于节点的函数 - number
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  返回 *$arg* 指示的节点的数值。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:number() as xs:double?   
fn:number($arg as node()?) as xs:double?  
```  
  
## <a name="arguments"></a>参数  
 *$arg*  
 将以数字返回其值的节点。  
  
## <a name="remarks"></a>备注  
 如果未指定 *$arg* ，则返回上下文节点的数值，转换为双精度值。 在 SQL Server 中，不带参数的 **fn： number ()** 仅可用于上下文相关的谓词的上下文中。 特别要指出的是，它只能在方括号 ([ ]) 内使用。 例如，下面的表达式将返回 <`ROOT`> 元素。  
  
```  
declare @x xml  
set @x='<ROOT>111</ROOT>'  
select @x.query('/ROOT[number()=111]')  
```  
  
 如果该节点的值不是数字简单类型的有效词法表示形式（如 **XML 架构第2部分：数据类型、W3C 建议**，则函数返回空序列）。 不支持 NaN。  
  
## <a name="examples"></a>示例  
 本主题提供了针对 AdventureWorks 数据库中各种 **xml** 类型列中存储的 xml 实例的 XQuery 示例。  
  
### <a name="a-using-the-number-xquery-function-to-retrieve-the-numeric-value-of-an-attribute"></a>A. 使用 number() XQuery 函数检索属性的数值  
 下面的查询从产品型号 7 的生产进程的第一个生产车间检索 lot size 属性的数值。  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     for $i in (//AWMI:root//AWMI:Location)[1]  
     return   
       <Location LocationID="{ ($i/@LocationID) }"   
                   LotSizeA="{  $i/@LotSize }"  
                   LotSizeB="{  number($i/@LotSize) }"  
                   LotSizeC="{ number($i/@LotSize) + 1 }" >  
  
       </Location>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 请注意上述查询的以下方面：  
  
-   **()** 函数不是必需的，如 **LotSizeA** 属性的查询所示。 这是一个 XPath 1.0 函数，主要是为了具有向后兼容性才包括在内。  
  
-   **LotSizeB** 的 XQuery 指定 number 函数，并且是冗余的。  
  
-   **LotSizeD** 的查询说明了在算术运算中使用数字值的情况。  
  
 结果如下：  
  
```  
ProductModelID   Result  
----------------------------------------------  
7              <Location LocationID="10"   
                         LotSizeA="100"   
                         LotSizeB="100"   
                         LotSizeC="101" />  
```  
  
### <a name="implementation-limitations"></a>实现限制  
 限制如下：  
  
-    () 函数的 **数量** 仅接受节点。 它不接受原子值。  
  
-   如果值不能以数字形式返回，则 **() 函数数值** 返回空序列而不是 NaN。  
  
## <a name="see-also"></a>另请参阅  
 [针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
